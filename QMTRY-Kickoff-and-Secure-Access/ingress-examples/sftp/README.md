# SFTP Ingress (Demo)

```bash
sftp -i /path/to/key demo_ingress@ingress.qmtry.ai <<'EOF'
put ./synthea/*.csv /incoming/synthea/
bye
EOF
```

- Keys are time-boxed and least-privilege.
- Use **Synthea** only for demos (no PHI).
