# date-time

## timestamptz comparison
```sql
select * from myTable where trunc(extract(epoch from createdDttm) * 1000) = <timeMillis>
```
Convert createdDttm (timestamptz type) to epoch milliseconds in bigint type, then do compare.

## diff
* Use extract to calculate date/time diff

  Postgresql supports ```{fn TIMESTAMPDIFF}``` function but got problem.
  For example,
  ```sql
  select {fn TIMESTAMPDIFF(SQL_TSI_HOUR, src.start, src.end)} as wrong_hour_diff
       , floor(extract(epoch from src.end - src.start)) / 3600 as correct_hour_diff
  from (
    select timestamp '2020-05-01 01:00:00' as start, timestamp '2020-05-02 01:00:00' as end
  ) src
  ```
  |worng_hour_diff|correct_hour_diff|
  |---------------|-----------------|
  |0              |24               |
  The ```{fn TIMESTAMPDIFF}``` is translated
  from
  ```sql
  {fn TIMESTAMPDIFF(SQL_TSI_HOUR, src.start, src.end)}
  ```
  to
  ```sql
  extract( hour from ( Time - CURRENT_TIMESTAMP) )
  ```
  which will only extract hour value, not hour diff value.
  > Using postgresql JDBC v42.2.12


