# AWS S3 Ingress (Demo)

```bash
aws s3 mb s3://qmtry-demo-synthea --region us-east-1
aws s3 sync ./synthea s3://qmtry-demo-synthea/synthea/
```

See `policy.sample.json` for a least-privilege approach. Replace placeholders with the exact ARNs exchanged under BAA.
