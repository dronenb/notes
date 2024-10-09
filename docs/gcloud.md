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

## Secrets Manager

```bash
# Read a secret
gcloud secrets versions access latest --secret="<secret_name>" --project="<project_id>"

# Add a secret version
cat /tmp/<secret_file> | tr -d '\n' | gcloud secrets versions add --project="<project_id>" --data-file=- <secret_name>
```

## Disable Impersonation

```bash
gcloud config unset auth/impersonate_service_account
```

## Disable Junk

```bash
  gcloud config set core/disable_usage_reporting true
  gcloud config set component_manager/disable_update_check true
  gcloud config set metrics/environment github_docker_image
  gcloud config set survey/disable_prompts true
```

