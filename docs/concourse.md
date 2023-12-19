# Concourse

Tips and tricks for working with [Concourse CI](https://concourse-ci.org/) from VMWare.

## Hijack Concourse container

```bash
fly --target=<team> hijack --job=<pipeline>/<job>
```

# Trigger Schedule Pipeline

This must be done if you want to run a pipeline job prior to first scheduled run.

```bash
for target in $(fly targets | tail -n +1 | grep -v main | awk '{print $1}'); do
    fly -t "$target" check-resource \
        --resource <pipeline>/<job> \
        --from "time:2023-11-18T14:00:00Z"
done
```
