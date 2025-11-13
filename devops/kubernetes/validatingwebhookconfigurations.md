# Removing Orphaned Webhook Configurations After Uninstalling Ingress Controllers

## Problem
After migrating from nginx ingress controller to Cilium, all Ingress resources failed to apply with:
```
Internal error occurred: failed calling webhook "validate.nginx.ingress.kubernetes.io": 
service "ingress-nginx-controller-admission" not found
```

## Root Cause
When you uninstall an ingress controller, Kubernetes doesn't automatically clean up its webhook configurations. These webhooks continue intercepting Ingress resource operations, trying to validate against a service that no longer exists.

## Solution
Manually delete the orphaned webhook configurations:

```bash
# List and identify the webhooks
kubectl get validatingwebhookconfigurations
kubectl get mutatingwebhookconfigurations

# Remove nginx webhooks
kubectl delete validatingwebhookconfigurations ingress-nginx-admission
kubectl delete mutatingwebhookconfigurations ingress-nginx-admission
```

## Lesson
Always check for and clean up webhook configurations when removing controllers or operators from your cluster. They persist independently of the applications that created them.
