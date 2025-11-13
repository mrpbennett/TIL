# Writing Effective PrometheusRules in Kubernetes

Today I learned how to write custom PrometheusRules for the kube-prometheus-stack without modifying Helm values - just create them as standalone resources!

## The Key: Labels Matter

For Prometheus Operator to pick up your custom rules, they need the right labels:

```yaml
apiVersion: monitoring.coreos.com/v1
kind: PrometheusRule
metadata:
  name: my-custom-alerts
  namespace: monitoring
  labels:
    # These labels are critical!
    app: kube-prometheus-stack
    app.kubernetes.io/instance: kube-prometheus-stack
    release: kube-prometheus-stack
spec:
  groups:
    - name: my-alert-group
      interval: 30s
      rules:
        - alert: MyAlert
          expr: my_metric > threshold
          for: 5m
          labels:
            severity: warning
          annotations:
            summary: 'Brief description'
            description: 'Detailed context with {{ $labels.pod }}'
```

## Writing Good Alert Rules

### 1. **Use Meaningful Thresholds**

```yaml
# Bad: Arbitrary number
expr: container_memory_usage > 1000000000

# Good: Percentage of resource request/limit
expr: |
  (
    container_memory_working_set_bytes{container!="",container!="POD"}
    /
    kube_pod_container_resource_requests{resource="memory"}
  ) > 0.8
```

### 2. **Set Appropriate `for:` Durations**

Prevent alert fatigue from transient spikes:

```yaml
# Warning alerts: longer duration
- alert: HighMemoryUsage
  expr: memory_usage > 0.8
  for: 5m  # Must be high for 5 minutes
  labels:
    severity: warning

# Critical alerts: shorter duration
- alert: CriticalMemoryUsage
  expr: memory_usage > 0.95
  for: 2m  # Needs immediate attention
  labels:
    severity: critical
```

### 3. **Write Clear Annotations**

Use template variables to make alerts actionable:

```yaml
annotations:
  summary: 'Pod {{ $labels.namespace }}/{{ $labels.pod }} exceeding memory'
  description: |
    Pod {{ $labels.namespace }}/{{ $labels.pod }} is using
    {{ $value | humanizePercentage }} of its memory request
    (container: {{ $labels.container }})
```

### 4. **Filter Out Noise**

```yaml
# Exclude system pods and empty containers
expr: |
  container_memory_working_set_bytes{
    container!="",
    container!="POD",
    namespace!~"kube-system|kube-public"
  }
```

### 5. **Use Severity Levels Consistently**

```yaml
# info: informational, no action needed
severity: info

# warning: investigate soon
severity: warning

# critical: immediate action required
severity: critical
```

### 6. **Test with Simple Alerts First**

Before writing complex queries, verify your pipeline works:

```yaml
- alert: TestAlert
  expr: vector(1)  # Always true
  for: 10s
  labels:
    severity: warning
  annotations:
    summary: 'Test alert - should fire immediately'
```

## Common Patterns

### Memory Alerts
```yaml
# Approaching memory requests
(container_memory_working_set_bytes / kube_pod_container_resource_requests{resource="memory"}) > 0.8

# Approaching memory limits (OOMKill risk)
(container_memory_working_set_bytes / kube_pod_container_resource_limits{resource="memory"}) > 0.9
```

### CPU Alerts
```yaml
# CPU usage vs requests
(rate(container_cpu_usage_seconds_total[5m]) / kube_pod_container_resource_requests{resource="cpu"}) > 0.8
```

### Pod Status Alerts
```yaml
# OOMKilled pods
kube_pod_container_status_last_terminated_reason{reason="OOMKilled"} == 1

# Frequent restarts
rate(kube_pod_container_status_restarts_total[1h]) > 0.016
```

### Pods Without Resource Limits
```yaml
# Catch pods with no requests set
kube_pod_container_resource_requests{resource="memory"} == 0
```

## Quick Testing Workflow

1. **Deploy test alert** that fires immediately
2. **Verify** it appears in Alertmanager/Discord
3. **Deploy actual alerts** with real queries
4. **Create test pods** that trigger conditions
5. **Monitor** alert progression: Inactive → Pending → Firing

## Gotchas

- ❌ Test pods that **crash** won't trigger alerts (they need to stay running)
- ❌ Alerts need to sustain conditions for the **`for:` duration** before firing
- ❌ Missing labels means Prometheus won't discover your rules
- ✅ Use `--vm-keep` flag with stress testing to maintain memory allocation
- ✅ Always validate PromQL queries in Prometheus UI first

## Resources

- Test queries: `https://prometheus.yourdomain.com/graph`
- View alerts: `https://prometheus.yourdomain.com/alerts`
- Check alertmanager: `https://alertmanager.yourdomain.com`
