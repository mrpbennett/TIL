# Projected ServiceAccount Tokens for In-Cluster Apps

An application running inside Kubernetes does not need a manually created
`kubernetes.io/service-account-token` Secret to call the Kubernetes API. The
cluster projects a short-lived token into each pod for its configured
ServiceAccount. Applications normally read it from:

```text
/var/run/secrets/kubernetes.io/serviceaccount/token
```

The ServiceAccount must be in the same namespace as the pod. Its permissions
come from RBAC bindings, not from the token Secret. Do not use the `default`
ServiceAccount for an application that needs cluster access: give the workload
its own identity and bind only the permissions it requires.

```yaml
apiVersion: v1
kind: ServiceAccount
metadata:
  name: dashboard
  namespace: dashboard
---
apiVersion: rbac.authorization.k8s.io/v1
kind: ClusterRoleBinding
metadata:
  name: dashboard
roleRef:
  apiGroup: rbac.authorization.k8s.io
  kind: ClusterRole
  name: view
subjects:
  - kind: ServiceAccount
    name: dashboard
    namespace: dashboard
---
apiVersion: apps/v1
kind: Deployment
metadata:
  name: dashboard
  namespace: dashboard
spec:
  template:
    spec:
      serviceAccountName: dashboard
```

A manually created token Secret is only needed when a token must be consumed
outside a pod. It must reference an existing ServiceAccount, and the workload
must explicitly mount or otherwise consume it. Creating one alone neither
grants permissions nor changes the identity of a pod using `default`.

See the Kubernetes documentation on
[ServiceAccounts](https://kubernetes.io/docs/concepts/security/service-accounts/)
and [RBAC](https://kubernetes.io/docs/reference/access-authn-authz/rbac/).
