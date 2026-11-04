# Helm chart ingress and httpRoute are mutually exclusive — add Tailscale separately

The Forgejo Helm chart (17.x) can expose the web UI through either a classic
`ingress` or a Gateway API `httpRoute`, but not both. If both are enabled,
`helm template` fails with:

```
execution error at (forgejo/templates/gitea/deprecation.yaml:36:8):
`ingress.enabled and httpRoute.enabled are mutually exclusive.
```

Under Argo CD this is easy to miss. The app shows `Sync: Unknown` and a
`ComparisonError` condition, while the old pod keeps running happily on the
previous config. Nothing is broken in the cluster, it just never updates.

```bash
kubectl get application forgejo -n argocd \
  -o jsonpath='{range .status.conditions[*]}{.type}: {.message}{"\n"}{end}'
```

## Wanting both LAN and tailnet access

The goal was `git.example.dev` on the LAN (through Envoy Gateway) and a
Tailscale hostname for when I'm away from home. The chart can't do both, so
split it up:

- Keep `httpRoute` in the chart values for the LAN route.
- Add the Tailscale pieces as plain manifests next to the chart.

```yaml
# chart values
httpRoute:
  enabled: true
  terminate: true # the gateway terminates TLS, so ROOT_URL becomes https://
  parentRefs:
    - name: envoy-shared-gateway
      namespace: envoy-gateway-system
      sectionName: https
  hostnames:
    - git.example.dev
```

```yaml
# standalone manifests
apiVersion: networking.k8s.io/v1
kind: Ingress
metadata:
  name: forgejo-tailscale
  namespace: forgejo
spec:
  ingressClassName: tailscale
  defaultBackend:
    service:
      name: forgejo-http
      port:
        number: 3000
  tls:
    - hosts:
        - git # becomes git.<tailnet>.ts.net
---
apiVersion: v1
kind: Service
metadata:
  name: forgejo-ssh-tailscale
  namespace: forgejo
  annotations:
    tailscale.com/hostname: git-ssh
spec:
  type: LoadBalancer
  loadBalancerClass: tailscale
  selector:
    app.kubernetes.io/instance: forgejo
    app.kubernetes.io/name: forgejo
  ports:
    - name: ssh
      port: 22
      targetPort: ssh
```

## Things worth knowing

- With the Tailscale operator, the **first label in `tls.hosts`** is the
  tailnet hostname. The `host:` in an Ingress rule is not what names the
  device, which is why `hosts: [forgejo]` quietly gave the wrong name.
- `loadBalancerClass: tailscale` on a Service gives you a tailnet-only TCP
  endpoint, which is how SSH gets exposed.
- Forgejo has a single `ROOT_URL`. Both hostnames reach the same pod, but
  clone URLs and absolute redirects will always use the LAN domain.
- Check the selector and named port on the chart's own Service first
  (`kubectl get svc forgejo-ssh -o yaml`) so the extra Service lands on the
  same pod.
- `kubectl apply --dry-run=server -f` is a cheap way to validate the new
  manifests before pushing.
