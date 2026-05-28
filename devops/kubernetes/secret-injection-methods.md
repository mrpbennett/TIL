# Two Ways to Inject Secrets as Environment Variables in Kubernetes

Kubernetes gives you two distinct approaches for exposing Secret values as
environment variables in a Pod spec. They differ in granularity and
explicitness.

## `envFrom` + `secretRef` — bulk import

```yaml
envFrom:
  - secretRef:
      name: my-app-secret
```

Every key in `my-app-secret` becomes an environment variable with the same
name. If the Secret contains `DB_PASS=foo` and `API_KEY=bar`, both land in
the container automatically. No per-key mapping required.

Use this when you own the Secret and want all of its keys available.

## `valueFrom` + `secretKeyRef` — single key import

```yaml
env:
  - name: SFTP_REMOTE_DIR
    valueFrom:
      secretKeyRef:
        name: sftp-secret
        key: AMGEN_SFTP_REMOTE_DIR
```

Pulls only the named key from the Secret and exposes it under whatever `name:`
you choose. The env var name and the Secret key name do not have to match.

Use this when you want explicit control over what's injected, need to rename a
key, or need to pull specific keys from multiple different Secrets.

## Comparison

| | `envFrom` | `valueFrom` |
|---|---|---|
| Keys imported | All keys at once | One key per `env` entry |
| Env var name | Must match the Secret key | Can differ from the Secret key |
| Visibility in manifest | Implicit | Explicit |
| Multi-secret mixing | One Secret per `envFrom` entry | Freely mix keys from any Secrets |

`envFrom` is convenient; `valueFrom` is safer and more auditable in production
workloads where you want to know exactly what's in the container's environment.
