# Talos: Fixing TargetDown for kube-controller-manager and kube-scheduler

If you run kube-prometheus-stack on Talos Linux you will probably see these
alerts straight away:

```text
TargetDown kube-controller-manager  (100% of targets down)
TargetDown kube-scheduler           (100% of targets down)
```

The pods are healthy. The problem is that Prometheus can't reach them.

## Why it happens

Talos starts both components with `--bind-address=127.0.0.1`, so their secure
metrics ports (10257 for the controller-manager, 10259 for the scheduler) only
listen on the node's loopback. The kube-prometheus-stack chart creates a Service
whose endpoints point at the **node IPs** (`<node-ip>:10257`, `<node-ip>:10259`).
Prometheus connects to the node IP, nothing is listening there, and `up == 0`.

You can confirm this from the live pods:

```bash
kubectl -n kube-system get pod -l component=kube-scheduler \
  -o jsonpath='{.items[0].spec.containers[0].command}'
# ... "--bind-address=127.0.0.1" ...

kubectl -n kube-system get endpoints kube-prometheus-stack-kube-scheduler
# 192.168.7.1:10259,192.168.7.2:10259,192.168.7.3:10259
```

It isn't a config conflict, just a mismatch between Talos' secure default and
what the chart expects. kube-proxy (`metricsBindAddress`) and etcd
(`listen-metrics-urls`) need the same kind of change.

## The fix (Talos 1.14+)

In Talos 1.14 most settings were split out of the `v1alpha1` machine config into
their own documents. Putting the args under `cluster.controllerManager` or
`cluster.scheduler` is rejected:

```text
kube-controller-manager config is already set in v1alpha1 config (.cluster.controllerManager)
kube-scheduler config is already set in v1alpha1 config (.cluster.scheduler)
```

Use the standalone documents instead, with a top-level `extraArgs`:

```yaml
apiVersion: v1alpha1
kind: KubeControllerManagerConfig
extraArgs:
  bind-address: 0.0.0.0
---
apiVersion: v1alpha1
kind: KubeSchedulerConfig
extraArgs:
  bind-address: 0.0.0.0
```

Not sure what the document kinds are called? Render your config and grep for
them: `grep '^kind:' rendered/kube-cp-1.yaml`. The field reference is in the
Talos docs for
[KubeControllerManagerConfig](https://docs.siderolabs.com/talos/v1.14/reference/configuration/kubernetes/kubecontrollermanagerconfig.md)
and
[KubeSchedulerConfig](https://docs.siderolabs.com/talos/v1.14/reference/configuration/kubernetes/kubeschedulerconfig.md).

## Verify

Check the pod spec now has the new flag, then ask Prometheus directly:

```bash
curl -s 'http://<prometheus>:9090/api/v1/targets?state=active' \
  | jq -r '.data.activeTargets[]
      | select(.labels.job=="kube-scheduler" or .labels.job=="kube-controller-manager")
      | "\(.labels.job) \(.scrapeUrl) \(.health) \(.lastError)"'
```

All targets should be `up` with an empty `lastError`.

`0.0.0.0` exposes the ports on your LAN. They still require authentication
(Prometheus uses its service account token), unlike etcd's metrics port, which
is unauthenticated.
