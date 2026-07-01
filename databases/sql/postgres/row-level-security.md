# Row Level Security (RLS) in PostgreSQL

Row Level Security lets you restrict which rows a database user can see or
modify — enforced at the database level, not in application code. It's
especially useful in multi-tenant apps where every user should only ever touch
their own data.

## Enable RLS on a table

```sql
ALTER TABLE orders ENABLE ROW LEVEL SECURITY;
```

Once enabled, the table is locked down for non-superusers by default —
no rows are visible until you add a policy.

## Create a policy

A policy defines a `USING` expression (for SELECT/UPDATE/DELETE) and an
optional `WITH CHECK` expression (for INSERT/UPDATE). Both must evaluate
to `true` for the operation to succeed.

```sql
-- Users can only see their own orders
CREATE POLICY user_orders_policy
  ON orders
  FOR ALL
  TO authenticated
  USING (user_id = current_setting('app.current_user_id')::uuid);
```

- `ON orders` — the table this policy applies to
- `FOR ALL` — covers SELECT, INSERT, UPDATE, DELETE (use a specific verb to narrow)
- `TO authenticated` — the role this policy targets (omit for all roles)
- `USING (...)` — the row filter; only rows where this is `true` are visible

## Separate read and write policies

```sql
-- Read: own rows only
CREATE POLICY select_own_rows ON documents
  FOR SELECT USING (owner_id = auth.uid());

-- Write: own rows only, and status must not be 'locked'
CREATE POLICY update_own_rows ON documents
  FOR UPDATE
  USING (owner_id = auth.uid())
  WITH CHECK (status <> 'locked');
```

`USING` filters which existing rows are matched; `WITH CHECK` validates
the row state after the write. You typically want both on UPDATE.

## Bypass RLS for admin roles

```sql
ALTER TABLE orders FORCE ROW LEVEL SECURITY;   -- applies to table owner too
ALTER ROLE admin BYPASSRLS;                     -- admin skips all policies
```

`FORCE ROW LEVEL SECURITY` makes policies apply even to the table owner.
Without it, the owner bypasses RLS entirely.

## Useful pattern — pass the current user via a setting

Application code sets a session variable before querying:

```sql
-- Set at the start of each request / transaction
SELECT set_config('app.current_user_id', '123e4567-...', true);
```

Then policies reference it:

```sql
USING (user_id = current_setting('app.current_user_id', true)::uuid)
```

The second `true` argument to `current_setting` makes it return `NULL`
instead of throwing if the setting is missing — safer for non-app sessions.

## Check which policies exist

```sql
SELECT policyname, cmd, roles, qual, with_check
FROM pg_policies
WHERE tablename = 'orders';
```
