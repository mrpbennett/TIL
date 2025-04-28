# Setting up a CronJob in Postgres

This works in Postgres and especially in Supabase with the cronjob extension

```sql
-- Enable pg_cron extension, create the cleanup function, and schedule it all in one go
create extension if not exists pg_cron;

do $$
begin
  -- Create the cleanup function
  create or replace function cleanup_job_logs() -- Function name
  returns void as $$
  begin
    -- START SQL Logic
    delete from job_logs
    where created_at < NOW() - INTERVAL '2 months';
    -- END SQL Logic
  end;
  $$ language plpgsql;

  -- Schedule the function (midnight daily)
  --cron.schedule('<job name>', '0 0 * * *', 'select <function name from above>');
  perform cron.schedule('cleanup-job-logs', '0 0 * * *', 'select cleanup_job_logs()');
$$;
```
