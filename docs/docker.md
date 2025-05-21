# Docker

## Docker on macOS without Docker Desktop

```bash
brew install docker lima
limactl start --vm-type vz --cpus 4 --memory 16 --name docker template://docker --tty=false
docker context create lima-docker --docker "host=unix:///Users/dronenb/.lima/docker/sock/docker.sock"
docker context use lima-docker
# Add to .zshrc or the like
export DOCKER_HOST=$(limactl list docker --format 'unix://{{.Dir}}/sock/docker.sock')
```

## Install `docker buildx`

for macOS:

```bash
mkdir -p "${HOME}/.docker/cli-plugins"
DOCKER_BUILDX_VERSION=$(curl -sL https://api.github.com/repos/docker/buildx/releases | jq -r ".[0].name")
curl -sL "https://github.com/docker/buildx/releases/download/${DOCKER_BUILDX_VERSION}/buildx-${DOCKER_BUILDX_VERSION}.darwin-arm64" -o "${HOME}/.docker/cli-plugins/docker-buildx"
```

ref: <https://github.com/docker/buildx?tab=readme-ov-file#manual-download>
