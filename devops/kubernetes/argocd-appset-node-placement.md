# Pinning an ArgoCD ApplicationSet to a Node

ArgoCD can't schedule anything onto a node. An ApplicationSet only generates
`Application` resources, and the Kubernetes scheduler decides where pods run.
To place an appset's workloads on a particular node, set node placement on the
workloads that the generated apps deploy.

## Helm charts: pass `nodeSelector` through the template

```yaml
spec:
  template:
    spec:
      source:
        helm:
          valuesObject:
            nodeSelector:
              kubernetes.io/hostname: rpi-node-2
```

The exact key (`nodeSelector`, `controller.nodeSelector`, `affinity`) depends
on the chart. With a list or git generator you can template the value per app,
for example `{{ .node }}`.

## Kustomize or plain manifests: patch it in

```yaml
patches:
  - target: {kind: Deployment}
    patch: |-
      - op: add
        path: /spec/template/spec/nodeSelector
        value: {kubernetes.io/hostname: rpi-node-2}
```

## Whole namespace: `PodNodeSelector` admission

Annotate the namespace with:

```yaml
scheduler.alpha.kubernetes.io/node-selector: "kubernetes.io/hostname=rpi-node-2"
```

On k3s the `PodNodeSelector` admission plugin must be enabled through
`kube-apiserver-arg`.

## Tips

- Prefer a custom label such as `workload=media` over the hostname. Add it with
  `kubectl label node <node> workload=media`.
- If the node is tainted as dedicated, the pods also need a matching
  `tolerations` entry.
- If you want to target a different *cluster* rather than a node, use the
  cluster generator with label selectors instead.
