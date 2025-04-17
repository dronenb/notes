# Container Tools

## [`crane`](https://github.com/google/go-containerregistry/blob/main/cmd/crane/doc/crane.md)

## [`img`](https://github.com/genuinetools/img/tree/master)

## [`incert`](https://github.com/chainguard-dev/incert)

### Add a CA cert to an image built by `ko`

Example for pulling ca certificates out of `ubi9-minimal`

```bash
incert \
  -ca-certs-image-url registry.access.redhat.com/ubi9-minimal:9.5-1742914212@sha256:ac61c96b93894b9169221e87718733354dd3765dd4a62b275893c7ff0d876869 \
  -image-url "${existing_image_location}" \
  -owner-group-id 0 \
  -owner-user-id 1001 \
  -platform linux/amd64 \
  -image-cert-path /etc/pki/ca-trust/extracted/pem/tls-ca-bundle.pem \
  -dest-image-url "${destination_image_location}" \
  -replace-certs
```

## [`ko`](https://ko.build/)

Example `.ko.yaml`:

```yaml
defaultBaseImage: registry.access.redhat.com/ubi9/ubi-micro:9.5-1744118077@sha256:dca8bc186bb579f36414c6ad28f1dbeda33e5cf0bd5fc1c51430cc578e25f819
defaultPlatforms:
  - linux/amd64
defaultLdflags:
  - -s -w
  - -extldflags "-static"
defaultEnv:
  - CGO_ENABLED=0
  - GOOS=linux
```

Example command:

```bash
KO_DOCKER_REPO=<registry> ko build --tags ubi9-micro --image-user=1001:0 .
```

## [`oras`](https://oras.land/docs/category/oras-commands/)

## [`skopeo`](https://github.com/containers/skopeo)

## [`umoci`](https://umo.ci/)

### Create new image

```bash
umoci init --layout oci-image
```
