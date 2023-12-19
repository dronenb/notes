# `kubectl`

Tips and tricks for `kubectl` and other utils so I don't have to remember everything all the time.

## Get shell into pod

```bash
kubectl exec -it pods/<pod_name> -- /bin/bash
```

## Create an ad-hoc pod using a push/pull secret:

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

## Create ad-hoc container image using a service account:

```bash
kubectl run -it toolbox --image <image> --overrides='{ "spec": { "serviceAccount": "<sa_name>" }  }'
```
