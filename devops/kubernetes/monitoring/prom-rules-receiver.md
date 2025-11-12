# Routing PrometheusRule Alerts to Specific Receivers

Today I learned that routing alerts from a PrometheusRule to a specific Alertmanager receiver is done through **alert labels**, not the resource's `metadata.name`.

## The Setup

If you have a receiver configured like this:

```yaml
- receiver: k8s-tam-alerts
  matchers:
    - name: receiver
      value: k8s-tam-alerts
```

## The Solution

Add a matching label to your PrometheusRule alerts:

```yaml
apiVersion: monitoring.coreos.com/v1
kind: PrometheusRule
metadata:
  name: my-alert-rule
spec:
  groups:
    - name: my-alerts
      rules:
        - alert: MyAlert
          expr: up == 0
          labels:
            receiver: k8s-tam-alerts # This routes to your receiver
```

## Key Takeaway

Alert routing happens at the **label level**, not the resource metadata level. When your alert fires, Alertmanager checks its labels against your route matchers to determine where to send it. The `metadata.name` field is just for Kubernetes resource identification and has no impact on alert routing.
