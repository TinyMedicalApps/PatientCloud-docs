# Using Cloud Build

Save `google.credentials.json` to Secret Manager as `google-identity-cred`. The secret will be saved as a `google.credentials.json` on build and injected inside container.

Example `cloudbuild.yaml`:

```yaml
steps:
  - name: gcr.io/cloud-builders/gcloud
    entrypoint: "bash"
    args:
      [
        "-c",
        "gcloud secrets versions access latest --secret=google-identity-cred --format='get(payload.data)' | tr '_-' '/+' | base64 -d > google.credentials.json",
      ]
  - name: "gcr.io/cloud-builders/docker"
    args:
      [
        "build",
        "-t",
        "gcr.io/$PROJECT_ID/github.com/tinymedicalapps/patientcloud",
        ".",
      ]
  - name: "gcr.io/cloud-builders/docker"
    args: ["push", "gcr.io/$PROJECT_ID/github.com/tinymedicalapps/patientcloud"]

```