# Istio base Helm Chart

This chart installs resources shared by all Istio revisions. This includes Istio CRDs.

## Setup Repo Info

```console
helm repo add tmax-cloud https://https://tmax-cloud.github.io/charts/
helm repo update
```

_See [helm repo](https://helm.sh/docs/helm/helm_repo/) for command documentation._

## Installing the Chart

To install the chart with the release name `istio-base`:

```console
kubectl create namespace istio-system
helm install istio-base tmax-cloud/istio/base -n istio-system
```
