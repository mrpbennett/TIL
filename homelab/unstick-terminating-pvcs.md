# Unsticking PVCs Stuck in Terminating State

PVCs (and many Kubernetes resources) use **finalizers** — strings in
`metadata.finalizers` that tell Kubernetes "don't delete this until something
cleans it up first." When you delete a PVC, the storage provisioner is
supposed to remove its finalizer after detaching/cleaning up the volume. If
the provisioner is gone, replaced, or unresponsive (e.g. you switched from
`local-path` to Longhorn), its finalizer never gets removed and the PVC sits
in `Terminating` forever.

The fix is to patch the finalizers list to `null`, which tells Kubernetes
nothing is blocking deletion and it can complete immediately:

```bash
kubectl patch pvc <name> -n <namespace> -p '{"metadata":{"finalizers":null}}'
```

For multiple stuck PVCs:

```bash
kubectl get pvc -A | grep Terminating
# then patch each one
kubectl patch pvc registry-pvc -n docker-registry -p '{"metadata":{"finalizers":null}}'
kubectl patch pvc my-other-pvc -n my-ns -p '{"metadata":{"finalizers":null}}'
```

Note: the underlying host directory (for `local-path`) may still exist on the
node after this — the PVC object is gone but the data on disk isn't
automatically cleaned up.
