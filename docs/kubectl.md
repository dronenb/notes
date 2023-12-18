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
