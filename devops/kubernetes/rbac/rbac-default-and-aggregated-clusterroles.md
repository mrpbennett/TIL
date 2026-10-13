# RBAC — Default ClusterRoles and Aggregation

Every cluster ships with default ClusterRoles, labelled
`kubernetes.io/bootstrapping=rbac-defaults`. You'll mostly use four of them:

| Role            | Gives                                                              |
| --------------- | ------------------------------------------------------------------ |
| `cluster-admin` | Everything, everywhere. In a `RoleBinding`: everything in that ns. |
| `admin`         | Read/write most of a namespace, including roles & rolebindings.    |
| `edit`          | Read/write most namespaced objects; can read Secrets; no RBAC.     |
| `view`          | Read-only; no Secrets, no roles/rolebindings.                      |

The `system:*` ones belong to control-plane components — don't edit them. The
API server re-adds missing permissions to defaults on startup (set the
annotation `rbac.authorization.kubernetes.io/autoupdate: "false"` to opt out,
at your own risk).

```bash
kubectl get clusterroles | grep -v '^system:'
kubectl get clusterrole view -o yaml
```

## Aggregation: how `view` stays up to date with CRDs

`admin`, `edit` and `view` have no hand-written rules. They have an
`aggregationRule`, and a controller fills in `rules` from every ClusterRole
that matches the label selector:

```bash
kubectl get clusterrole view -o jsonpath='{.aggregationRule}'
```

```json
{"clusterRoleSelectors":[{"matchLabels":{"rbac.authorization.k8s.io/aggregate-to-view":"true"}}]}
```

So to make `view` understand your CRD, create a ClusterRole carrying the label.
Find who's already contributing on a cluster:

```bash
kubectl get clusterrole -l rbac.authorization.k8s.io/aggregate-to-view=true
```

```text
cdi.kubevirt.io:view
cert-manager-view
kubevirt.io:view
system:aggregate-to-view
...
```

`cert-manager-view` is a typical contributor — read-only rules plus labels:

```yaml
metadata:
  name: cert-manager-view
  labels:
    rbac.authorization.k8s.io/aggregate-to-view: "true"
    rbac.authorization.k8s.io/aggregate-to-edit: "true"
    rbac.authorization.k8s.io/aggregate-to-admin: "true"
rules:
  - apiGroups: [cert-manager.io]
    resources: [certificates, certificaterequests, issuers]
    verbs: [get, list, watch]
```

Now anyone bound to `view` can read cert-manager `Certificate`s, with no extra
bindings. The `edit` ClusterRole aggregates `aggregate-to-edit`, `admin`
aggregates `aggregate-to-admin`; note `view` itself carries the
`aggregate-to-edit` label, so edit inherits everything in view.

## Making your own aggregate

```bash
kubectl create clusterrole monitoring \
  --aggregation-rule="rbac.example.com/aggregate-to-monitoring=true"
```

Any rules you write into an aggregated ClusterRole by hand will be
**overwritten** by the controller — put them in a labelled child role instead.
