# BigQuery External Table (Demo)

```sql
CREATE EXTERNAL TABLE `demo.synthea_patients`
WITH CONNECTION `us.gcs.my_conn`
OPTIONS (
  format = 'CSV',
  uris = ['gs://qmtry-demo/synthea/patients/*.csv'],
  skip_leading_rows = 1
);
```
