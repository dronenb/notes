# Tricks for kubectl

Create an ad-hoc pod using a push/pull secret:

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

Create ad-hoc container image using a service account:

```bash
kubectl run -it toolbox --image <image> --overrides='{ "spec": { "serviceAccount": "<sa_name>" }  }'
```

Get shell into pod

```bash
kubectl exec -it pods/<pod_name> -- /bin/bash
```

