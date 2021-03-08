# Adding multiple schemas

At first it's not very easy to understand how to use multiple schemas in DataGrip, you first have to set up your connection to one schema and then following the below.

### URL format
```
jdbc:presto://<host>:<port>/<catalog>/<schema>
```

Which in turn becomes

```
jdbc:presto://xxx-presto-xx.domain.com:1234/catalog/schema?SSL=True
```

Once you have connected succesfully and you can see the one schema.

Right click on your database connection, and then click properties, you can choose schemas in schemas tab. Select your schemas, and then you must right click on your database connection again, and click synchronize.