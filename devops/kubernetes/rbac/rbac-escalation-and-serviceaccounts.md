# RBAC — Privilege Escalation Prevention and ServiceAccount Permissions

## You can't grant what you don't have

The RBAC API stops users from editing their way to more power:

- **Create/update a Role or ClusterRole** — you must already hold every
  permission in it (at that scope), _or_ have the `escalate` verb on
  `roles` / `clusterroles`.
- **Create/update a RoleBinding or ClusterRoleBinding** — you must already hold
  every permission in the referenced role, _or_ have the `bind` verb on that
  role.

```yaml
# allow a user to bind (but not edit) one specific ClusterRole
rules:
  - apiGroups: [rbac.authorization.k8s.io]
    resources: [clusterroles]
    verbs: [bind]
    resourceNames: [view]
```

This is why `kubectl apply` of a role can fail with "attempting to grant RBAC
permissions not currently held" — check with
`kubectl auth can-i <verb> <resource>` first. To bootstrap, use a credential in
the `system:masters` group, which is bound to `cluster-admin`.

## ServiceAccounts start with nothing

A ServiceAccount (outside `kube-system`) only gets the discovery roles. Give it
what it needs, ordered from best to worst:

```bash
# 1. dedicated SA per app (best) — pod sets serviceAccountName
kubectl create rolebinding my-sa-view \
  --clusterrole=view --serviceaccount=my-namespace:my-sa -n my-namespace

# 2. the namespace's `default` SA — every pod without serviceAccountName
kubectl create rolebinding default-view \
  --clusterrole=view --serviceaccount=my-namespace:default -n my-namespace

# 3. every SA in a namespace
kubectl create rolebinding serviceaccounts-view \
  --clusterrole=view --group=system:serviceaccounts:my-namespace \
  -n my-namespace

# 4. every SA in the cluster (discouraged)
kubectl create clusterrolebinding serviceaccounts-view \
  --clusterrole=view --group=system:serviceaccounts
```

Never bind `cluster-admin` to `system:serviceaccounts` — any pod in any
namespace would then be a cluster administrator and could read all Secrets.

Verify what a ServiceAccount ended up with:

```bash
kubectl get sa -n argocd
kubectl auth can-i --list -n argocd \
  --as=system:serviceaccount:argocd:argocd-server
```

Docs: <https://kubernetes.io/docs/reference/access-authn-authz/rbac/>
