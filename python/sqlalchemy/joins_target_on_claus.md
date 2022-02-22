# Joins to a Target with an ON Clause

Creating JOINS with SQL Alchmey. If I wanted to replicate the below SQL query, I can do the following in SQL Alchmey
```sql
SELECT
    constraintid,
    sja.segment,
    sjs.source,
    sjct.constrainttype,
    constraintvalue,
    targeting,
    frequency,
    period
FROM signaljourneyaudienceconstraints
JOIN signaljourneyaudiences sja ON sja.audienceid = signaljourneyaudienceconstraints.audienceid;
JOIN signaljourneysources sjs ON sjs.sourceid = signaljourneyaudienceconstraints.sourceid
JOIN signaljourneyconstrainttype sjct ON sjct.constrainttypeid = signaljourneyaudienceconstraints.constrainttypeid
```

Think of `db.query()` as the select statement. This is where you want to place the columns you want to return. We then use the `.join()` on the query method and specify our joins like so:

```python
.join(
    models.SignalJourneyAudiences, # table we're joining from
    models.SignalJourneyAudiences.audienceId 
    == models.SignalJourneyAudienceConstraints.audienceId, 
)
```     
We only have to specify the table we're using as a join then what we're joining on. You join tables (models), **not** columns (properties)

```python
@app.get("/get-main-query-data", status_code=status.HTTP_200_OK)
def get_main_query_data():
    """Returns data for the main query."""
    return (
        db.query(
            models.SignalJourneyAudienceConstraints.constraintId,
            models.SignalJourneyAudiences.segment,
            models.SingalJourneySources.source,
            models.SignalJourneyConstraintType.constraintType,
            models.SignalJourneyAudienceConstraints.constraintValue,
            models.SignalJourneyAudienceConstraints.targeting,
            models.SignalJourneyAudienceConstraints.frequency,
            models.SignalJourneyAudienceConstraints.period,
        )
        .join(
            models.SignalJourneyAudiences,
            models.SignalJourneyAudiences.audienceId
            == models.SignalJourneyAudienceConstraints.audienceId,
        )
        .join(
            models.SingalJourneySources,
            models.SingalJourneySources.sourceId
            == models.SignalJourneyAudienceConstraints.sourceId,
        )
        .join(
            models.SignalJourneyConstraintType,
            models.SignalJourneyConstraintType.constraintTypeId
            == models.SignalJourneyAudienceConstraints.constraintTypeId,
        )
        .all()
    )
```
