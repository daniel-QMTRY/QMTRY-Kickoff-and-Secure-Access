# Synthea Demo Workbook (Markdown)

This workbook shows example queries against **synthetic** Synthea tables.

## Example Queries (DuckDB syntax)

```sql
-- Top conditions by prevalence
SELECT condition, COUNT(*) AS n
FROM conditions
GROUP BY 1
ORDER BY n DESC
LIMIT 10;
```

```sql
-- Encounters per patient
SELECT patient_id, COUNT(*) AS encounters
FROM encounters
GROUP BY 1
ORDER BY encounters DESC
LIMIT 10;
```

## Notes
- Replace table names with your demo schema if different.
- Attach results to the Evidence Bundle as screenshots or CSVs.
