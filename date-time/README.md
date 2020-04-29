# date-time

## timestamptz comparison
```sql
select * from myTable where trunc(extract(epoch from createdDttm) * 1000) = <timeMillis>
```
Convert createdDttm (timestamptz type) to epoch milliseconds in bigint type, then do compare.
