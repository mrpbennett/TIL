# `DATE_SUB` & `DATE_ADD` functions

The `DATE_ADD()` function adds a time/date interval to a date and then returns
the date.

### Syntax

```sql
DATE_ADD(date, INTERVAL value addunit)
```

| Parameter | Description                                                                                         |
| --------- | --------------------------------------------------------------------------------------------------- |
| `date`    | Required. The date to be modified                                                                   |
| `value`   | Required. The value of the time/date interval to add. Both positive and negative values are allowed |
| `addunit` | Required. The type of interval to add. Can be one of the following values:                          |

- MICROSECOND
- SECOND
- MINUTE
- HOUR
- DAY
- WEEK
- MONTH
- QUARTER
- YEAR
- SECOND_MICROSECOND
- MINUTE_MICROSECOND
- MINUTE_SECOND
- HOUR_MICROSECOND
- HOUR_SECOND
- HOUR_MINUTE
- DAY_MICROSECOND
- DAY_SECOND
- DAY_MINUTE
- DAY_HOUR
- YEAR_MONTH |

### Use case

Add 5 months to a date and return the date:

```sql
SELECT DATE_ADD("2023-06-01", INTERVAL 5 MONTH);
```

- [`DATE_ADD` reference](https://www.w3schools.com/sql/func_mysql_date_add.asp)
- [`DATE_SUB` reference](https://www.w3schools.com/sql/func_mysql_date_sub.asp)
