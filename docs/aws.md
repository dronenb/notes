# AWS

## Logging in to `aws` CLI

```bash
aws configure set aws_access_key_id <yourAccessKey>
aws configure set aws_secret_access_key <yourSecretKey>
```

validate with:

```bash
aws sts get-caller-identity
```

ref: <https://stackoverflow.com/a/52311817>
