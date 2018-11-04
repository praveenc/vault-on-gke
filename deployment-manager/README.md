# Provisioning Vault GKE cluster using GCP's Deployment Manager

This tutorial walks you through provisioning a multi-node HashiCorp Vault cluster on
 GKE using [GCP Cloud-Foundation DM Templates](https://github.com/GoogleCloudPlatform/deploymentmanager-samples/tree/master/community/cloud-foundation) .

## Prerequisites

- Install [gcloud](https://cloud.google.com/sdk)

### Create a New Project

In this section you will create a new GCP project and enable the APIs required by this tutorial.

Generate a project ID:

```(shell)
PROJECT_ID="vault-$(($(date +%s%N)/1000000))"
```

Create a new GCP project:

```(shell)
gcloud projects create ${PROJECT_ID} \
  --name "${PROJECT_ID}"
```

[Enable billing](https://cloud.google.com/billing/docs/how-to/modify-project#enable_billing_for_a_new_project)
on the new project before moving on to the next step.

Enable the GCP APIs required by this tutorial:

```(shell)
gcloud services enable \
  cloudapis.googleapis.com \
  cloudkms.googleapis.com \
  container.googleapis.com \
  containerregistry.googleapis.com \
  iam.googleapis.com \
  --project ${PROJECT_ID}
```

### Getting Deployment Manager templates

Clone the [Deployment Manager samples repository](https://github.com/GoogleCloudPlatform/deploymentmanager-samples)

```(shell)
git clone https://github.com/GoogleCloudPlatform/deploymentmanager-samples
```

Copy the `vault-gke.yaml` file in the repo to the `cloud-foundation` directory

```(shell)
cp vault-gke.yaml deploymentmanager-samples/community/cloud-foundation/vault-gke.yaml
```

Go to the community/cloud-foundation directory

```(shell)
cd deploymentmanager-samples/community/cloud-foundation
```

Replace the <FIXME:..> placeholder values in the config file to your setup.

```shell
vim vault-gke.yaml  # <== change values to match your GCP setup
```

## Deployment

Create your deployment as described below, we'll call this deployment with name `vault-gke`

```(shell)
gcloud deployment-manager deployments create vault-gke \
    --config vault-gke.yaml
```

In case you need to **update** your deployment:

```(shell)
gcloud deployment-manager deployments update vault-gke \
    --config vault-gke.yaml
```

In case you need to **delete** your deployment:

```(shell)
gcloud deployment-manager deployments delete vault-gke \
    --delete-policy=abandon --quiet
```
