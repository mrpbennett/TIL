# RBAC — Bindings and Subjects (Who Gets the Role)

A `RoleBinding` / `ClusterRoleBinding` connects a role to one or more
**subjects**: a `User`, a `Group`, or a `ServiceAccount`.

```bash
kubectl create rolebinding read-pods --role=pod-reader --user=jane \
  -n default --dry-run=client -o yaml
```

```yaml
apiVersion: rbac.authorization.k8s.io/v1
kind: RoleBinding
metadata:
  name: read-pods
  namespace: default
roleRef: # what is granted
  apiGroup: rbac.authorization.k8s.io
  kind: Role
  name: pod-reader
subjects: # who gets it
  - apiGroup: rbac.authorization.k8s.io
    kind: User
    name: jane
```

## Subject syntax

```yaml
subjects:
  - kind: User
    name: jane
    apiGroup: rbac.authorization.k8s.io
  - kind: Group
    name: developers
    apiGroup: rbac.authorization.k8s.io
  - kind: ServiceAccount
    name: alloy
    namespace: logging # required for ServiceAccounts
```

Kubernetes has no `User` object — user and group names come from your
authenticator (certs, OIDC, ...). Service accounts are real objects, and their
user name is `system:serviceaccount:<namespace>:<name>`; the group
`system:serviceaccounts:<namespace>` covers every SA in a namespace.

## The ClusterRole + RoleBinding trick

A `RoleBinding` may point at a **ClusterRole**. The permissions are then
limited to the binding's namespace. This lets you define a role once and hand
it out per namespace:

```bash
kubectl create rolebinding bob-admin-binding --clusterrole=admin \
  --user=bob --namespace=acme

kubectl create rolebinding myapp-view-binding --clusterrole=view \
  --serviceaccount=acme:myapp --namespace=acme
```

And the cluster-wide version, for something like a controller (this is how the
Alloy ClusterRole above is wired up):

```bash
kubectl create clusterrolebinding myapp-view-binding --clusterrole=view \
  --serviceaccount=acme:myapp
```

## Gotchas

- `roleRef` is **immutable**. To point a binding at a different role you must
  delete and recreate the binding.
- A binding only references a role by name — creating one doesn't verify the
  role exists, and a missing role simply grants nothing.
- `ClusterRoleBinding` + `ClusterRole` of namespaced resources = access in
  **every** namespace. Prefer a `RoleBinding` unless that's what you want.

Look at the existing ones to see what's bound to what:

```bash
kubectl get clusterrolebindings
kubectl get rolebindings -A
kubectl get clusterrolebinding alloy -o yaml
```
