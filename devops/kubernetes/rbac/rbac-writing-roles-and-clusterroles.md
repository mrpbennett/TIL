# RBAC — Writing Roles and ClusterRoles (with `kubectl create` and Real Examples)

Skip hand-writing YAML for the basics: `kubectl create role` /
`kubectl create clusterrole` with `--dry-run=client -o yaml` generates a
correct manifest, and fills in the `apiGroups` for you.

## Role (namespaced)

```bash
kubectl create role pod-reader --verb=get,list,watch --resource=pods \
  -n default --dry-run=client -o yaml
```

```yaml
apiVersion: rbac.authorization.k8s.io/v1
kind: Role
metadata:
  name: pod-reader
  namespace: default
rules:
  - apiGroups:
      - "" # core API group
    resources:
      - pods
    verbs:
      - get
      - list
      - watch
```

## ClusterRole (cluster-wide)

```bash
kubectl create clusterrole secret-reader --verb=get,list,watch \
  --resource=secrets --dry-run=client -o yaml
```

Same shape, but kind `ClusterRole` and no `namespace`.

## Flag cheat sheet

```bash
# a resource outside the core group: resource.group
kubectl create role foo --verb=get,list,watch --resource=replicasets.apps

# subresources
kubectl create role foo --verb=get,list,watch --resource=pods,pods/status

# restrict to specific named objects
kubectl create role pod-reader --verb=get --resource=pods \
  --resource-name=readablepod --resource-name=anotherpod

# non-resource URLs (ClusterRole only)
kubectl create clusterrole foo --verb=get --non-resource-url=/healthz
```

`--resource-name` produces a `resourceNames` list. Caveats: it can't limit
`create` (the name doesn't exist yet), and `list`/`watch` only honour it when
the request uses a matching `fieldSelector`.

## Real examples from a live cluster

Don't write a role from scratch if a well-built one already exists — read how
other projects scope theirs.

```bash
kubectl get role -n argocd argocd-redis-secret-init -o yaml
kubectl get clusterrole alloy -o yaml
```

The Argo CD role is a good small example of `resourceNames`: it can `get` only
the secret named `argocd-redis`, but `create` is a separate rule with no name
because `create` can't be name-restricted:

```yaml
rules:
  - apiGroups: [""]
    resourceNames: [argocd-redis]
    resources: [secrets]
    verbs: [get]
  - apiGroups: [""]
    resources: [secrets]
    verbs: [create]
```

The Alloy ClusterRole (a log/metrics collector) shows the other pattern:
read-only (`get`/`list`/`watch`) across many groups, a subresource
(`pods/log`), a cluster-scoped resource (`nodes`), and a non-resource URL:

```yaml
rules:
  - apiGroups: [""]
    resources: [pods, pods/log, namespaces]
    verbs: [get, list, watch]
  - apiGroups: [monitoring.coreos.com]
    resources: [podmonitors, servicemonitors, probes, scrapeconfigs]
    verbs: [get, list, watch]
  - nonResourceURLs: [/metrics]
    verbs: [get]
```

Note it also lists `secrets` and `configmaps` cluster-wide — handy for
collectors, but worth noticing when you audit (see
[checking access](rbac-checking-access-with-can-i.md)).

## Applying

```bash
kubectl apply -f role.yaml
# or the RBAC-aware variant that merges permissions/subjects safely
kubectl auth reconcile -f role.yaml --dry-run=client
```

`kubectl auth reconcile` creates missing objects (even namespaces), adds
missing permissions/subjects, and only removes extras if you pass
`--remove-extra-permissions` / `--remove-extra-subjects`.
