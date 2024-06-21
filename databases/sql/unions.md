# UNION

The `UNION` operator does what you might expect: combines the results of two SQL queries into a single table. The caveat is that both results from the two queries must have the same number of columns and compatible data types.

`UNION` removes duplicate rows, while `UNION ALL` does not. Use `UNION` ALL by default, unless you care about duplicate results.

### Example

```sql
select surname from cd.members
union
select name from cd.facilities;

-- returns --

+-----------------+
|surname          |
+-----------------+
|Hunt             |
|Farrell          |
|Tennis Court 2   |
|Table Tennis     |
|Dare             |
|Rownam           |
|GUEST            |
|Badminton Court  |
|Smith            |
|Tupperware       |
|Owen             |
|Worthington-Smyth|
|Butters          |
|Rumney           |
|Tracy            |
|Crumpet          |
|Purview          |
|Massage Room 2   |
|Sarwin           |
|Baker            |
|Pool Table       |
|Snooker Table    |
|Jones            |
|Coplin           |
|Mackenzie        |
|Boothe           |
|Joplette         |
|Stibbons         |
|Squash Court     |
|Tennis Court 1   |
|Pinker           |
|Genting          |
|Bader            |
|Massage Room 1   |
+-----------------+
```
