# Podman

## Open a shell in a new image:

```bash
podman run -it <image> /bin/bash
```

## Open a shell in a running container:

```bash
podman exec -it <image_id> /bin/bash
```

## Run/Build for x86 on Apple Silicon

```bash
podman build --platform linux/amd64 ...
podman run --platform linux/amd64 ...
```

## Delete untagged images:

```bash
for image in $(podman images | grep none | awk '{print $3}'); do podman image rm -f "${image}"; done
```

## Podman Machine

Initialize

```bash
PODMAN_MACHINE_VCPUS=4
PODMAN_MACHINE_MEMORY_MB=4096
PODMAN_MACHINE_DISK_GB=20
PODMAN_MACHINE_NAME="podman-machine-default"

podman machine init \
    --cpus "${PODMAN_MACHINE_VCPUS}" \
    --disk-size "${PODMAN_MACHINE_DISK_GB}" \
    --memory "${PODMAN_MACHINE_MEMORY_MB}" &&
podman machine start
```

Stop

```bash
podman machine stop
```

Delete

```bash
podman machine rm
```

All at once (assumes running)

```
PODMAN_MACHINE_VCPUS=4
PODMAN_MACHINE_MEMORY_MB=4096
PODMAN_MACHINE_DISK_GB=20
PODMAN_MACHINE_NAME="podman-machine-default"
podman machine stop && \
podman machine rm -f && \
podman machine init \
    --cpus "${PODMAN_MACHINE_VCPUS}" \
    --disk-size "${PODMAN_MACHINE_DISK_GB}" \
    --memory "${PODMAN_MACHINE_MEMORY_MB}" &&
podman machine start
```

## Podman Configuration

On macOS, config is stored in `~/.config/containers/containers.conf` in TOML format. Example:

- <https://github.com/containers/common/blob/main/docs/containers.conf.5.md>

## Podman Apple Silicon "Waiting for VM" fix (Late 2023/Early 2024)

```bash
curl -sL https://gitlab.com/kraxel/qemu/-/raw/704f7cad5105246822686f65765ab92045f71a3b/pc-bios/edk2-aarch64-code.fd.bz2 | bzip2 -d - -c > /opt/homebrew/share/qemu/edk2-aarch64-code.fd  
```

 - <https://github.com/containers/podman/issues/20776>

