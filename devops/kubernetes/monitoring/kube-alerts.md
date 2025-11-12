# Understanding kube_job_failed Metric Queries

## The Metric

`kube_job_failed` is a **status indicator** (not a counter):

- `0` = job succeeded
- `1` = job failed

## Query Differences

### `kube_job_failed{namespace="tam"}`

Returns **all** time series regardless of value (both 0 and 1).

- Use case: Visibility and debugging
- Problem: Will trigger alerts even for successful jobs

### `kube_job_failed{namespace="tam"} > 0`

Returns **only failed jobs** (value greater than 0).

- Use case: **Standard pattern for alerting** ✅
- Filters out successful jobs automatically
- Best practice for alerts

### `kube_job_failed{namespace="tam"} == 1`

Returns **only jobs with value exactly 1**.

- Functionally identical to `> 0` for this metric
- Would only differ if metric could have values > 1 (which it doesn't)

## Key Takeaway

`> 0` and `== 1` don't mean "failed multiple times" or "failed once" - they both mean "has failed status". The metric is binary, not a failure count.

**For alerting on Kubernetes job failures, always use `> 0`.**
