# How to download a cert from a secret

This will download the base64 from `.data.ca.crt` and decode it. Allowing you to add it to your local machine for https

```bash
kubectl get secret root-secret -n cert-manager \
  -o jsonpath='{.data.ca\.crt}' | base64 -d > ca.crt
```
