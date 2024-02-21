A little background here, at work we use [Flux](https://fluxcd.io/) this has been set up for quite a while way before ArgoCD was released. So liking new and shiny things, I decided to go with [ArgoCD](https://argoproj.github.io/cd/) plus I liked the logo much more.

I will run through how I have installed Argo on my test [K3s](https://k3s.io) cluster. First let's check our requirements

## Installation

**Requirements**

- Installed `kubectl` command-line tool.
- Have a `kubeconfig` file (default location is `~/.kube/config`).
- CoreDNS

### 1. Install ArgoCD

```bash
kubectl create namespace argocd
kubectl apply -n argocd -f https://raw.githubusercontent.com/argoproj/argo-cd/stable/manifests/install.yaml
```

This will create a new namespace, argocd, where Argo CD services and application resources will live.

### 2. Download Argo CD CLI

I found the easiest way to do this was with homebrew, which is available for MacOS & Linux.

```bash
brew install argocd
```

### 3. Access The Argo CD API Server

By default, the Argo CD API server is not exposed to an external IP. To access the API server, we will go with port forwarding. I want to set it up with ingress but that will be another task at a later date. Kubectl port-forwarding can also be used to connect to the API server without exposing the service.

```bash
kubectl port-forward svc/argocd-server -n argocd 8080:443
```

However, I found this was not persistent. When I closed my terminal session this session to the UI died with it. So in this case I enabled port forwarding via [Lens](https://k8slens.dev/) which kept it open. This all should be resolved once I have sorted out the Ingress.

### 4. Login Using The CLI

The initial password for the admin account is auto-generated and stored as clear text in the field password in a secret named argocd-initial-admin-secret in your Argo CD installation namespace. You can simply retrieve this password using the argocd CLI:

```bash
argocd admin initial-password -n argocd
```

Make sure you change the password in the user settings once the UI has booted up. Now we have ArgoCD up and running.

## Apps of Apps setup

With ArgoCD I believe the way most DevOps guys set it up, is to have one repo per project and add those separately. But what happens if you have a mono repo of all your manifests similar to this:

```
k8s-manifests/
├── namespaces
│   ├── production
│   │   └── namespace.yaml
│   └── staging
│       └── namespace.yaml
├── apps
│   ├── app1
│   │   ├── deployment.yaml
│   │   ├── service.yaml
│   │   └── configmap.yaml
│   └── app2
│       ├── deployment.yaml
│       ├── service.yaml
│       └── ingress.yaml
└── db
    ├── postgres
    │   ├── deployment.yaml
    │   ├── service.yaml
    │   └── pvc.yaml
    └── redis
        ├── deployment.yaml
        └── service.yaml
```

We have something similar set up at my place of work, where we have one repo full of manifests with each directory being its own namespace. I wanted to try and replicate that within ArgoCD. Here we are going to use the [apps of apps](https://argo-cd.readthedocs.io/en/stable/operator-manual/cluster-bootstrapping/) pattern. This is where we load one app to control multiple apps.

This all clicked after watching the following video on YT. [The App of Apps Pattern for Argo CD in Less than 8 Minutes](https://youtu.be/2pvGL0zqf9o?si=1YaQfj95GU9L39iS) where Nicholas goes on to explain that Argo CD applications, projects and settings can be defined declaratively using Kubernetes manifests.

I have my test `kube-manifest` directory setup like so:

```
.
├── README.md
├── databases
│   ├── postgres
│   │   └── postgres.yml
│   └── redis
│       ├── deployments
│       │   └── redis-deployment.yml
│       └── services
│           └── redis.svc.yml
├── development
├── namespaces.yml
├── production
├── staging
└── web-server
    └── deployments
        └── nginx-deployment.yml
```

Where there is one file that holds all the namespaces. In my non-test cluster, I have a namespace manifest inside each directory, but for test purposes, I find it's easier to have them all in one file. Now let's set it up:

![argocd-titlebar](//images.ctfassets.net/53u3hzg2egeu/2aHHmYCardZYR2mYLILWFl/dc4e43378ed2d8db82b4ffea1890e6ab/Screenshot_2024-02-21_at_12.32.41.png)

Click the `+ NEW APP` button, and I found the easiest way was to next `EDIT AS YAML` and paste this in:

```yaml
apiVersion: argoproj.io/v1alpha1
kind: Application
metadata:
  name: homelab-mono-test
spec:
  destination:
    name: ''
    namespace: argocd
    server: 'https://kubernetes.default.svc'
  source:
    path: kube-manifests-test
    repoURL: 'https://github.com/mrpbennett/homelab.git'
    targetRevision: HEAD
    directory:
      recurse: true
  sources: []
  project: default
  syncPolicy:
    automated:
      prune: true
      selfHeal: true
    syncOptions:
      - CreateNamespace=true
```

With this file we're telling ArgoCD to create the namespaces, we're also using `recurse` so Argo can drill down into the directories within my manifest directory. Because by default, directory applications will only include the files from the root of the configured repository/path. I found that tip in this video [Introduction to Argo CD : Kubernetes DevOps CI/CD](https://youtu.be/2WSJF7d8dUg?si=tbFRG-aeRYfPqqu_&t=215). We're also `prune` which will remove any dead / unused resources and `selfHeal` where our application enables automatic sync when the live cluster's state deviates from the state defined in Git. We're also deploying this application under the `argocd` namespace, which keeps the application resources inside the `argocd` namespace.

Once the YAML is in place, it's time to click `CREATE` which will deploy your application. With everything going well you should have deployed your mono repo inside ArgoCD, this is how mine looked in the end with a few manifests.

![argocd_apps-of_apps](//images.ctfassets.net/53u3hzg2egeu/1Tc1Qcrs7TYsf5B0ljUWFH/9e4fb99c9ccae32b74253e174078a4c4/Screenshot_2024-02-21_at_12.06.51.png)

Now I can deploy changes to the git repo, and they will sync right to my cluster via ArgoCD.

LOVELY!! <3
