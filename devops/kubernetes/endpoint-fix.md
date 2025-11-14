# Debugging Kubernetes Gateway API HTTPRoute with Missing Endpoints

## The Problem

I had a Kubernetes Gateway API setup with an HTTPRoute that wasn't working. The EndpointSlice remained unset, and traffic couldn't reach my application.

**Setup:**
- Gateway in `envoy-gateway-system` namespace with wildcard `*.70ld.dev` listener
- HTTPRoute in `homepage` namespace pointing to `homepage.70ld.dev`
- Service and Deployment both in `homepage` namespace

## The Investigation

First, I verified the HTTPRoute was correctly attaching to the Gateway by checking its status:

```bash
kubectl get httproute homepage-route -n homepage -o yaml
```

The status showed:
- ✅ `Accepted: True` - Route successfully attached to Gateway
- ❌ `BackendsAvailable: False` - "Failed to find endpoints: no ready endpoints for the related Service homepage/homepage"
- ✅ `ResolvedRefs: True` - All references resolved

This told me the Gateway configuration was fine (cross-namespace reference working with `allowedRoutes.namespaces.from: All`), but the Service had no ready endpoints.

## The Root Cause

**Label mismatch between Service and Deployment:**

```yaml
# Service selector
selector:
  app: homepage

# Deployment pod labels
template:
  metadata:
    labels:
      app.kubernetes.io/name: homepage
```

The Service was looking for pods with `app: homepage`, but the Deployment was creating pods with `app.kubernetes.io/name: homepage`. Since these labels didn't match, the Service couldn't find any endpoints.

## The Fix

Updated the Service selector to match the Deployment's labels:

```yaml
selector:
  app.kubernetes.io/name: homepage
```

After applying this change, the Service immediately found the pods, endpoints populated, and the HTTPRoute started routing traffic correctly.

## Key Takeaways

1. **Check HTTPRoute status** - The `status.parents` section provides detailed error messages about why routes aren't working
2. **Label consistency matters** - Services find pods through label selectors, so they must match exactly
3. **Use standard labels** - `app.kubernetes.io/name` is the recommended Kubernetes label, more consistent than custom `app` labels
4. **Cross-namespace Gateway references** require `allowedRoutes.namespaces.from: All` (or specific namespace selectors) in the Gateway listener configuration
