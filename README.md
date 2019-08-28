# Cluster API Development Environment

This repo houses some tools to make running Cluster API v1alpha2 a little bit easier.

## Repo Setup

- Modify config.json for your environment
- Referenced repositories are expected to be already checked out under the `default_repo_root` defined in config.json

## Install Tilt

<https://docs.tilt.dev/install.html>

## Get a kubernetes cluster

Use kind, set up a cluster on AWS, GCP, etc. Whatever works. This will be your management cluster.

### kind (recommended for linux)

[Install kind](https://github.com/kubernetes-sigs/kind#please-see-our-documentation-for-more-in-depth-installation-etc)

Run kind with the provided config `kind create cluster --config ./devenv/kind/config.yaml`

Set KUBECONFIG.

## Run Tilt

run `tilt up`

## Iterate

Now you can quickly iterate on Cluster API
