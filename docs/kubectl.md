# `kubectl`

Tips and tricks for `kubectl` and other utils so I don't have to remember everything all the time.

## Get shell into pod

```bash
kubectl exec -it pods/<pod_name> -- /bin/bash
```

## Create an ad-hoc pod using a push/pull secret

```bash
kubectl run \
    --stdin \
    --tty \
    --rm \
    --overrides='{
        "spec": {
            "imagePullSecrets": [{"name": "secret-name"}]
        }
    }' \
    image-name \
    --image <image> \
    -- /bin/bash
```

## Create ad-hoc container image using a service account

```bash
kubectl run -it toolbox --image <image> --overrides='{ "spec": { "serviceAccount": "<sa_name>" }  }'
```

## Get Resource Names/API Versions/Shortnames

For all api resources:

```bash
kubectl api-resources
```

For namespace api resources:

```bash
kubectl api-resources --namespaced=true
```

Can also do this for a particular resource:

```bash
kubectl explain <resource>
```

## Job from CronJob

```bash
kubectl create job --from=cronjob/<name of cronjob> <name of job>
```

## Get Resource Name

```bash
kubectl get pods --no-headers -o custom-columns=":metadata.name"
```

## Restart a deployment

```bash
kubectl rollout restart deployment/<deployment>
```

## Wait For

Some examples

```bash
kubectl wait -n <namespace> --for jsonpath='{.status.phase}'=Succeeded pod/<pod>
kubectl wait -n <namespace> --for=condition=<condition> "pod/pod" --timeout=600s
```
