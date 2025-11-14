# Gateway API ParentRefs Allow Cross-Namespace Route Attachment

Today I learned that Kubernetes Gateway API HTTPRoutes don't need to live in the same namespace as the Gateway they attach to. This is controlled by the `parentRefs` field.

## The Problem I Had

I was getting 503 errors from Envoy Gateway because my Service and HTTPRoute were in `envoy-gateway-system`, but my application pods were in the `homepage` namespace. The EndpointSlice showed ports as "unset" because there were no matching pods.

## The Solution

HTTPRoutes can reference Gateways in different namespaces using `parentRefs`:

```yaml
apiVersion: gateway.networking.k8s.io/v1
kind: HTTPRoute
metadata:
  name: homepage-route
  namespace: homepage  # Route is in app namespace
spec:
  parentRefs:
    - name: envoy-shared-gateway
      namespace: envoy-gateway-system  # Gateway is in envoy namespace
  hostnames:
    - 'homepage.70ld.dev'
  rules:
    - matches:
        - path:
            type: PathPrefix
            value: /
      backendRefs:
        - name: homepage
          port: 3000
```

## Best Practice Pattern

Keep your infrastructure organized by namespace:

**Infrastructure namespace** (`envoy-gateway-system`):
- Gateway resources
- Shared ingress infrastructure

**Application namespace** (`homepage`, `api`, etc.):
- Deployment
- Service  
- HTTPRoute (references the Gateway via parentRefs)

This provides better isolation and follows the principle of keeping related resources together.

## Key Takeaways

- `parentRefs` enables cross-namespace Gateway attachment
- The Service must be in the same namespace as the HTTPRoute (unless using ReferenceGrant)
- One shared Gateway can serve routes from multiple namespaces
- This is the recommended pattern for multi-tenant or multi-app clusters

## References

- [Gateway API HTTPRoute Spec](https://gateway-api.sigs.k8s.io/api-types/httproute/)
- [Cross-Namespace References](https://gateway-api.sigs.k8s.io/guides/multiple-ns/)
