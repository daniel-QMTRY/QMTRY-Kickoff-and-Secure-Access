# Azure Blob Ingress (Demo)

```bash
az storage blob upload-batch       --account-name <yourAccount>       --destination 'qmtry-demo/synthea'       --source ./synthea
```

Use a scoped SAS with short expiry. Demo data must be **Synthea** only.
