# chtoto
# new_commit

# rem_comm

# proba

# suisuisugsgsgdsug


steps:
  # build the container image
  - name: "gcr.io/cloud-builders/docker"
    args:
      [
        "build",
        "--build-arg",
        "CONN=${_CONN_UAT}",
        "-t",
        "eu.gcr.io/$PROJECT_ID/b2b-integration-api:uat-$SHORT_SHA",
        ".",
      ]
  # build the container image
  - name: "gcr.io/cloud-builders/docker"
    args:
      [
        "build",
        "--build-arg",
        "CONN=${_CONN_RU}",
        "-t",
        "eu.gcr.io/$PROJECT_ID/b2b-integration-api:ru-$SHORT_SHA",
        ".",
      ]
  # build the container image
  - name: "gcr.io/cloud-builders/docker"
    args:
      [
        "build",
        "--build-arg",
        "CONN=${_CONN_UA}",
        "-t",
        "eu.gcr.io/$PROJECT_ID/b2b-integration-api:ua-$SHORT_SHA",
        ".",
      ]
  # push the container image to Container Registry
  - name: "gcr.io/cloud-builders/docker"
    args: ["push", "eu.gcr.io/$PROJECT_ID/b2b-integration-api"]
  # Deploy container image to Cloud Run
  - name: "gcr.io/cloud-builders/gcloud"
    args:
      [
        "run",
        "deploy",
        "b2b-integration-api-uat",
        "--image",
        "eu.gcr.io/$PROJECT_ID/b2b-integration-api:uat-$SHORT_SHA",
        "--platform",
        "managed",
        "--allow-unauthenticated",
        "--region",
        "europe-north1",
        "--vpc-connector",
        "eunorth1-vpc",
      ]
  - name: "gcr.io/cloud-builders/gcloud"
    args:
      [
        "run",
        "deploy",
        "b2b-integration-api-ru",
        "--image",
        "eu.gcr.io/$PROJECT_ID/b2b-integration-api:ru-$SHORT_SHA",
        "--platform",
        "managed",
        "--allow-unauthenticated",
        "--region",
        "europe-north1",
        "--vpc-connector",
        "eunorth1-vpc",
      ]
  - name: "gcr.io/cloud-builders/gcloud"
    args:
      [
        "run",
        "deploy",
        "b2b-integration-api-ua",
        "--image",
        "eu.gcr.io/$PROJECT_ID/b2b-integration-api:ua-$SHORT_SHA",
        "--platform",
        "managed",
        "--allow-unauthenticated",
        "--region",
        "europe-north1",
        "--vpc-connector",
        "eunorth1-vpc",
      ]
images:
  - eu.gcr.io/$PROJECT_ID/b2b-integration-api
