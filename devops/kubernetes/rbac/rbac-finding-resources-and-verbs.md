# RBAC — Finding the Resources and Verbs to Put in a Role

Before writing a `Role` you need three values for each rule: the **apiGroup**,
the **resource** name, and the **verbs** it supports. Don't guess — ask the
cluster.

## Resources, API groups and verbs in one shot

```bash
kubectl api-resources -o wide
```

```text
NAME         SHORTNAMES  APIVERSION  NAMESPACED  KIND        VERBS
configmaps   cm          v1          true        ConfigMap   create,delete,deletecollection,get,list,patch,update,watch
namespaces   ns          v1          false       Namespace   create,delete,get,list,patch,update,watch
deployments  deploy      apps/v1     true        Deployment  create,delete,deletecollection,get,list,patch,update,watch
applications app,apps    argoproj.io/v1alpha1  true  Application  delete,deletecollection,get,list,patch,create,update,watch
```

How to read it:

- `APIVERSION` — the part **before** the `/` is the `apiGroups` entry. `v1`
  (no slash) is the core group and is written as `""` in a rule. `apps/v1`
  becomes `apps`, `argoproj.io/v1alpha1` becomes `argoproj.io`.
- `NAME` — goes in `resources` (use the plural, lowercase name).
- `VERBS` — the verbs you are allowed to list in the rule. A verb that isn't
  listed for a resource is meaningless in a rule for it (e.g. `namespaces` has
  no `deletecollection`; `bindings` only supports `create`).
- `NAMESPACED` — decides whether a `Role` is enough (`true`) or you need a
  `ClusterRole` (`false`).

## Narrowing the list

```bash
# cluster-scoped resources — these need a ClusterRole
kubectl api-resources --namespaced=false -o name

# everything in one API group
kubectl api-resources --api-group=apps -o wide

# resources that support a given verb
kubectl api-resources --verbs=list,watch -o name
```

## What the verbs mean

| Verb               | HTTP                 | Notes                                    |
| ------------------ | -------------------- | ---------------------------------------- |
| `get`              | GET one              | `kubectl get pod foo`                    |
| `list`             | GET collection       | also exposes full object contents!       |
| `watch`            | GET `?watch=true`    | `kubectl get -w`, controllers/informers  |
| `create`           | POST                 |                                          |
| `update`           | PUT                  |                                          |
| `patch`            | PATCH                | `kubectl apply`, `edit`, `label`         |
| `delete`           | DELETE one           |                                          |
| `deletecollection` | DELETE collection    | `kubectl delete pods --all`              |

Beyond the table, some resources have special verbs — `escalate` / `bind` on
roles, `impersonate` on users, `use` on PodSecurityPolicies — covered in
[escalation prevention](rbac-escalation-and-serviceaccounts.md).

## Subresources

Subresources show up as `resource/subresource` in the `resources` list. You
won't see them in `api-resources`, but you've used them: `pods/log`,
`pods/exec`, `deployments/scale`, `nodes/metrics`. Granting `pods` does **not**
grant `pods/exec` — they are separate.
