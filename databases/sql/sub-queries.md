# Using sub queries

Let's say we have the following table

```
+---------+-------------+--------------+---------+-------------+------------------+
|productID|productName  |quanityInStock|unitPrice|manufactureID|manufactureName   |
+---------+-------------+--------------+---------+-------------+------------------+
|1        |Guinness     |100           |5        |5            |Diageo            |
|2        |Nikka Whisky |25            |20       |2            |Asahi Breweries   |
|3        |Elvis Juice  |15            |6        |4            |BrewDog           |
|4        |Bloody 'Ell  |200           |6.75     |3            |BeaverTown Brewery|
|5        |Neck Oil     |200           |6.25     |3            |BeaverTown Brewery|
|6        |Clwb Tropica |150           |5.9      |8            |Tiny Rebel Brewery|
|7        |Amstel       |300           |4.9      |6            |Heineken N.V.     |
|8        |Becks        |5             |4.75     |1            |AB InBev          |
|9        |Birra Moretti|50            |5.5      |6            |Heineken N.V.     |
|10       |Sip Smith Gin|150           |25       |7            |Sip Smith         |
|11       |Peroni       |56            |6        |9            |Asahi Breweries   |
+---------+-------------+--------------+---------+-------------+------------------+
```

And we want to find all the products that are above the avg priced items of our stock. I can find this out by using a sub query like so:

```sql
select
    "productName",
    "quanityInStock",
    "unitPrice"
from "BreweryProducts"
where "unitPrice" > (
    select avg("unitPrice") from "BreweryProducts"
    );
```

The above selects what I want to see from the table above, and only shows me items that have a unit price above the avg. This is done with the sub query here:

```sql
where "unitPrice" > (
    select avg("unitPrice") from "BreweryProducts" -- <-- this is the sub query
    );
```

### More info here:

- [5 Subquery Examples in SQL](https://learnsql.com/blog/sql-subquery-examples/)
- [Learn to Write a SQL Correlated Subquery in 5 Minutes](https://learnsql.com/blog/correlated-sql-subquery-5-minutes/)
