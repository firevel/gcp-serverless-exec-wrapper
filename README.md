# GCP Serverless Exec Wrapper

This is a wrapper Docker image that sets up an environment similar to Google App Engine and Google Cloud Run, suitable for running scripts and maintenance tasks. In particular, it ensures a suitable Cloud SQL Proxy is running in the environment.

Its driving use case is running production database migrations for Laravel applications, but it is also useful for similar uses for other languages and frameworks.

This package is based on https://github.com/GoogleCloudPlatform/ruby-docker/tree/master/app-engine-exec-wrapper

## Cloudbuild usage

```
steps:
- name: "firevel/gcp-serverless-exec-wrapper"
  args: ["-i", "gcr.io/${PROJECT_ID}/your-image", "-r", "launcher", "-s", "${_CLOUDSQL_INSTANCE}", '--', 'php', 'artisan', 'migrate']
```

For more examples check: https://github.com/GoogleCloudPlatform/ruby-docker/tree/master/app-engine-exec-wrapper

## Snippet to access image workspace

If you are using Cloud Build together with buildpacks in some cases you might need to access workspace files. To do this you can use this snippet:
```
- name: 'gcr.io/cloud-builders/docker'
  entrypoint: 'bash'
  args:
  - '-c'
  - |
    docker create --name temp_container gcr.io/${PROJECT_ID}/your-image
    docker cp temp_container:/workspace /
    docker rm temp_container
```

It will copy files from `/workspace` of `gcr.io/${PROJECT_ID}/your-image` and copy them to current workspace.
