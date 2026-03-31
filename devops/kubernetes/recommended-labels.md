# Kubernetes Recommended Labels

Kubernetes defines a set of common labels under the `app.kubernetes.io/`
prefix. Applying them consistently across your manifests makes your workloads
easier to query, visualise in dashboards, and understand at a glance — without
changing how selectors or services route traffic.

## The three you'll use most

```yaml
labels:
  app.kubernetes.io/name: api          # the component's name
  app.kubernetes.io/part-of: myapp     # the top-level application it belongs to
  app.kubernetes.io/component: api     # the role this piece plays
```

| Label | What it describes | Example values |
|---|---|---|
| `app.kubernetes.io/name` | The component itself | `api`, `worker`, `redis` |
| `app.kubernetes.io/part-of` | The parent application | `pixeldebugger`, `checkout-service` |
| `app.kubernetes.io/component` | The architectural role | `api`, `worker`, `cache`, `database` |

Other labels in the family: `version`, `instance`, `managed-by`.

## Why these exist alongside `app:`

The plain `app: my-service` label is fine for pod selectors and Service
`selector:` blocks — and you should keep it for that. But it conflates
identity with routing. The `app.kubernetes.io/` labels separate those concerns:

- `app: pixeldebugger-api` — used by the Service selector to find pods
- `app.kubernetes.io/name: api` — describes what the resource *is* for tooling

Tools like Helm, Argo CD, kubectl's `--selector`, and cluster dashboards (Lens,
k9s) all recognise the recommended labels natively. Running
`kubectl get all -l app.kubernetes.io/part-of=pixeldebugger` returns every
resource in your application across Deployments, Services, HPAs, and more —
without needing a custom label scheme.

## Where to put them

Apply to **every** resource — Deployments, StatefulSets, Services, HPAs,
Ingresses, ConfigMaps, and the Namespace itself:

```yaml
# Deployment
apiVersion: apps/v1
kind: Deployment
metadata:
  name: pixeldebugger-api
  namespace: tam
  labels:
    app: pixeldebugger-api                     # selector label — keep this
    app.kubernetes.io/name: api
    app.kubernetes.io/part-of: pixeldebugger
    app.kubernetes.io/component: api
spec:
  selector:
    matchLabels:
      app: pixeldebugger-api                   # selector only needs `app:`
  template:
    metadata:
      labels:
        app: pixeldebugger-api                 # pod label — must match selector
        app.kubernetes.io/name: api
        app.kubernetes.io/part-of: pixeldebugger
        app.kubernetes.io/component: api
```

The recommended labels go on `metadata.labels` of the resource *and* on the
pod template — but they do **not** need to appear in `selector.matchLabels`.
Selectors are intentionally kept minimal (just `app:`) because changing them
later requires deleting and recreating the Deployment.

## Quick reference

```bash
# List every resource belonging to an application
kubectl get all -l app.kubernetes.io/part-of=pixeldebugger -n tam

# Find all caches across all namespaces
kubectl get all -l app.kubernetes.io/component=cache -A
```

Official docs: https://kubernetes.io/docs/concepts/overview/working-with-objects/common-labels/
