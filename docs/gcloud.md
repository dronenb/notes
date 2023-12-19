# `gcloud`

Tips and tricks for `gcloud`

## Set project

```bash
gcloud config set project <project-id>
```

## `GOOGLE_OAUTH_ACCESS_TOKEN`

Used by Terraform when running locally:

```bash
export GOOGLE_OAUTH_ACCESS_TOKEN=$(gcloud auth print-access-token)
```

## Artifact Registry + Podman

Example for US Central1:

```bash
gcloud auth print-access-token | podman login -u oauth2accesstoken --password-stdin us-central1-docker.pkg.dev
```
