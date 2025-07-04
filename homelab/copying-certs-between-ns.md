# Copying certificates between namespaces

I had to copy a secret from one namespace to another so I could mount it. This was how I did it.

```bash
kubectl get secret <secret-name> -n <source-ns> -o yaml \
  | sed 's/namespace: <source-ns>/namespace: <destination-ns>/' \
  | kubectl apply -f -
```
