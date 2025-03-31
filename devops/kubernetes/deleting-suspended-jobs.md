# How to delete multiple suspend cronjobs at once

First step is to list the jobs within the namespace.

Then once you have then you can use a bash script to loop through them and delete them one by one.

```bash
# DRY RUN
kubectl get cronjob -n <namespace> -o jsonpath='{range .items[*]}{.metadata.name}{"\t"}{.spec.suspend}{"\n"}{end}' | awk '$2=="true" {print $1}'

# FULL RUN
for cronjob in $(kubectl get cronjob -n <namespace> -o jsonpath='{range .items[*]}{.metadata.name}{"\t"}{.spec.suspend}{"\n"}{end}' | awk '$2=="true" {print $1}'); do
  kubectl delete cronjob "$cronjob" -n <namespace>
done
```
