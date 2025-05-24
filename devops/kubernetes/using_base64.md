# Using base64 in the terminal

Sometimes we need to convert something to base64 so we can add it into a K8s secret

```bash
cat <filename> | base64
```

Would be the best way to do it. Then just copy and paste the output into a secret like so:

```yaml
apiVersion: v1
kind: Secret
metadata:
  name: decrypt-secret
  namespace: dev
type: Opaque
data:
  pgp-passphrase: LS0tLS1xxx # base64 here...
```
