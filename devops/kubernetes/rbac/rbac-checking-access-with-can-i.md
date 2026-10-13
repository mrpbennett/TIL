# RBAC — Checking Access with `kubectl auth can-i`

The fastest way to test a Role/ClusterRole is `kubectl auth can-i`. It asks
the API server's authorizer directly, so it reflects what's really in effect —
not what you think you wrote.

```bash
# me
kubectl auth can-i get pods -n default
kubectl auth can-i delete nodes

# everything I can do in a namespace
kubectl auth can-i --list -n argocd
```

## Impersonation with `--as`

Test a service account (or user) without logging in as them. You need the
`impersonate` verb to do this.

```bash
# Alloy's ServiceAccount, bound to a ClusterRole that can read nodes
kubectl auth can-i get nodes --as=system:serviceaccount:logging:alloy
# yes

# ...but it's read-only
kubectl auth can-i delete pods --as=system:serviceaccount:logging:alloy -n logging
# no

# ...and because the ClusterRole includes secrets, it can read them everywhere
kubectl auth can-i get secrets --as=system:serviceaccount:logging:alloy -n default
# yes
```

That last result is the kind of thing you want to catch in an audit.

## Reading `--list` output

```bash
kubectl auth can-i --list -n argocd \
  --as=system:serviceaccount:argocd:argocd-redis-secret-init
```

```text
Resources                                  Non-Resource URLs   Resource Names   Verbs
secrets                                    []                  []               [create]
selfsubjectreviews.authentication.k8s.io   []                  []               [create]
cdiconfigs.cdi.kubevirt.io                 []                  []               [get list watch]
```

The first row is the role's own grant. The rest are baseline rules every
authenticated subject gets from the default discovery roles
(`system:basic-user`, `system:discovery`, `system:public-info-viewer`) and any
aggregated roles.

An admin shows `*.*` with `[*]`. Namespaced vs cluster-scoped mistakes show up
as a harmless warning, e.g.
`Warning: resource 'nodes' is not namespace scoped`.

## Debug loop

1. `kubectl create role ... --dry-run=client -o yaml` → review
2. `kubectl apply` the role and binding
3. `kubectl auth can-i <verb> <resource> --as=<subject>` for what _should_ work
4. ...and again for what should **not**
