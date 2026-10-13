# RBAC — The Four Objects (Role, ClusterRole, RoleBinding, ClusterRoleBinding)

Kubernetes RBAC lives in the `rbac.authorization.k8s.io` API group and is made
of just four kinds. Two of them define **what** can be done, two of them define
**who** gets to do it.

|                | Defines permissions | Grants permissions to subjects |
| -------------- | ------------------- | ------------------------------ |
| Namespaced     | `Role`              | `RoleBinding`                  |
| Cluster-scoped | `ClusterRole`       | `ClusterRoleBinding`           |

Two rules to remember:

- Permissions are **purely additive**. There is no "deny" rule — if nothing
  grants it, it is denied.
- A `Role` only ever applies inside its own namespace. A `ClusterRole` can be
  used to (1) grant namespaced resources in one namespace via a `RoleBinding`,
  (2) grant namespaced resources in _all_ namespaces via a
  `ClusterRoleBinding`, or (3) grant cluster-scoped resources (nodes, PVs) and
  non-resource URLs (`/healthz`, `/metrics`).

You can see all four kinds, and which ones are namespaced, straight from the
cluster:

```bash
kubectl api-resources --api-group=rbac.authorization.k8s.io -o wide
```

```text
NAME                  APIVERSION                     NAMESPACED   KIND                 VERBS
clusterrolebindings   rbac.authorization.k8s.io/v1   false        ClusterRoleBinding   create,delete,deletecollection,get,list,patch,update,watch
clusterroles          rbac.authorization.k8s.io/v1   false        ClusterRole          create,delete,deletecollection,get,list,patch,update,watch
rolebindings          rbac.authorization.k8s.io/v1   true         RoleBinding          create,delete,deletecollection,get,list,patch,update,watch
roles                 rbac.authorization.k8s.io/v1   true         Role                 create,delete,deletecollection,get,list,patch,update,watch
```

Note that RBAC objects are themselves protected by RBAC — see
[privilege escalation prevention](rbac-escalation-and-serviceaccounts.md).

Docs: <https://kubernetes.io/docs/reference/access-authn-authz/rbac/>
