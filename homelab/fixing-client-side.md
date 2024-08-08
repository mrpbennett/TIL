# Fixing the Too long: must have at most 262144 bytes,error

I have been deploying some helm charts with ArgoCD and sometimes I was running into the following errors:

```txt
CustomResourceDefinition.apiextensions.k8s.io "alertmanagers.monitoring.coreos.com" is invalid: metadata.annotations: Too long: must have at most 262144 bytes,error when patching "/dev/shm/3149845322":
```

Someone from Discord helped me with the following fixes

[https://github.com/sholdee/home-ops/blob/be975d28c613fc8914020900c4dd899e4e1e8c2f/apps/argocd-conf/argocd-apps.yml#L463](https://github.com/sholdee/home-ops/blob/be975d28c613fc8914020900c4dd899e4e1e8c2f/apps/argocd-conf/argocd-apps.yml#L463) server-side apply should resolve

```yaml
    ...
    syncOptions:
      - CreateNamespace=true
      - ServerSideApply=true
```

there is also an annotation for argo to use server side diff strategy [https://github.com/sholdee/home-ops/blob/be975d28c613fc8914020900c4dd899e4e1e8c2f/apps/argocd-conf/argocd-apps.yml#L346](https://github.com/sholdee/home-ops/blob/be975d28c613fc8914020900c4dd899e4e1e8c2f/apps/argocd-conf/argocd-apps.yml#L346)

```yaml
metadata:
  name: kube-prometheus-stack
  annotations:
    argocd.argoproj.io/compare-options: ServerSideDiff=true
  ...
```
