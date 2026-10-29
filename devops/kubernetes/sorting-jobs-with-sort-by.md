# Sorting kubectl output with --sort-by

`kubectl get` returns resources in name order, which is rarely what you want
when you're hunting for "the job that just ran". The `--sort-by` flag takes a
JSONPath expression and sorts the list by that field.

```bash
kubectl get jobs --sort-by=.metadata.creationTimestamp
```

Sorting is **ascending**, so the oldest job is at the top and the newest is at
the bottom, right next to your cursor. There is no `--reverse` flag.

## Just the latest jobs

Because the newest are last, `tail` is the easiest way to see the most recent:

```bash
kubectl get jobs --sort-by=.metadata.creationTimestamp | tail -n 5
```

## Newest first

Piping through `tac` flips the order but also moves the header to the bottom.
Keep the header in place with a subshell:

```bash
kubectl get jobs --sort-by=.metadata.creationTimestamp \
  | (read -r header; echo "$header"; tac)
```

## Sorting on other fields

The expression is any JSONPath into the object, so you aren't limited to the
creation time. Jobs record when they started and finished:

```bash
# when the job started running
kubectl get jobs --sort-by=.status.startTime

# when the job finished (jobs still running have no value yet)
kubectl get jobs --sort-by=.status.completionTime
```

It works on every resource type, not just jobs:

```bash
# pods by start time
kubectl get pods --sort-by=.status.startTime

# pods by restart count of the first container
kubectl get pods --sort-by='.status.containerStatuses[0].restartCount'

# events in the order they happened (events are unsorted by default)
kubectl get events --sort-by=.lastTimestamp
```

## Showing the field you sorted by

The default table doesn't show `creationTimestamp`, so the order can look
arbitrary. Add it as a column with `custom-columns`:

```bash
kubectl get jobs --sort-by=.metadata.creationTimestamp \
  -o custom-columns='NAME:.metadata.name,CREATED:.metadata.creationTimestamp,SUCCEEDED:.status.succeeded'
```

## Gotchas

- `creationTimestamp` only has one-second resolution, so jobs created in the
  same second may appear in any order.
- The flag sorts what the API returned, so it combines fine with `-n
  <namespace>`, `-A` and `-l <selector>`.
- Jobs spawned by a CronJob are named `<cronjob>-<scheduled-time>`, so a plain
  name sort already roughly works for them, but `--sort-by` is still the
  reliable way.

See the [kubectl reference](https://kubernetes.io/docs/reference/kubectl/jsonpath/)
for the full JSONPath syntax.
