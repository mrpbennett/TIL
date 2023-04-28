# SQL SET Keyword

The SET command is used with UPDATE to specify which columns and values that should be updated in a table.

The following SQL updates the first customer (CustomerID = 1) with a new ContactName and a new City:

```sql
UPDATE Customers
SET ContactName = 'Alfred Schmidt', City= 'Frankfurt'
WHERE CustomerID = 1;
```

The following SQL will update the "ContactName" field to "Juan" for all records where Country is "Mexico":

```sql
UPDATE Customers
SET ContactName='Juan'
WHERE Country='Mexico';
```

Real world examples:

```sql
-- activating a pixel
update AdvPixel
set IsActive = 1
where PixelId = 44900;

-- changing someones surname name
update users
set surname = 'Bennett'
where first_name = 'Fiona'

-- updating a publisher name from an id
update PubDealExchange
set PubDealExchangeName = 'Magnite Video and CTV'
where PublisherID = 560711;
```