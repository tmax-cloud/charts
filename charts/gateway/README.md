# HyperCloud API-GATEWAY

HyperCloud API-GATEWAY 설치 가이드  

## TL;DR;

```console
$ git clone https://github.com/tmax-cloud/charts.git
$ cd gateway
```

## Introduction

This chart bootstraps a gateway deployment on a [Kubernetes](http://kubernetes.io) cluster using the [Helm](https://helm.sh) package manager.

## Installing the Chart

To install the chart with the release name `gateway` on an api-gateway-system namespace:

```console
$ helm install gateway . --namespace api-gateway-system --create-namespace --wait
```

The [configuration](#configuration) section lists the parameters that can be configured during installation.

## Uninstalling the Chart

To uninstall/delete the `gateway` deployment:

```console
$ helm uninstall -n api-gateway-system gateway
```

The command removes all the Kubernetes components associated with the chart and deletes the release.

## Configuration

The following table lists the configurable parameters of the oauth2-proxy chart and their default values.

| Parameter                      | Description                                                                                | Default             |
|--------------------------------|--------------------------------------------------------------------------------------------|---------------------|
| `tls.domain`                   | domain name                                                                                | `""`                |
| `tls.acme.enabled`             | Flag to use [ACME](https://cert-manager.io/docs/configuration/acme/) for TLS certification | `false`             |
| `tls.acme.email`               | email                                                                                      | `"temp@tmax.co.kr"` |
| `tls.acme.dns.type`            | dns type [route53](https://cert-manager.io/docs/configuration/acme/dns01/route53/)         | `"route53"`         |
| `tls.acme.dns.accessKeyID`     | route53 accessKeyID                                                                        | `"accessKeyID"`     |
| `tls.acme.dns.accessKeySecret` | route53 accessKeySecret                                                                    | `"accessKeySecret"` |
| `tls.acme.dns.hostedZoneID`    | route53 hostedZoneID                                                                       | `"hostZoneID"`      |
| `tls.acme.environment`         | "staging" for dev testing, "production" for production                                     | `"staging"`         | 
| `tls.selfsigned`               | selgsigned TLS certification                                                               | `true`              |
| `dashboard.enabled`            | enabled traefik dashboard                                                                  | `true`              |
| `dashboard.id`                 | login id of traefik dashboard (if using, should be set both id and password)               | `""`                |
| `dashboard.password`           | login password of traefik dashboard                                                        | `""`                |
| `traefik.enabled`              | Flag to install traefik                                                                    | `true`              |
| `traefik.image.name`           | image name                                                                                 | `"traefik"`         |
| `traefik.image.tag`            | image tag                                                                                  | `"v2.8.7"`          |
| `traefik.service.type`         | service type                                                                               | `"LoadBalacer"`     |                                |

Specify each parameter using the `--set key=value[,key=value]` argument to `helm install`. For example,

```console
$ helm install gateway . \
  --set=tls.domain=tmaxcloud.org,traefik.image.name=traefik
```

Alternatively, a YAML file that specifies the values for the above parameters can be provided while installing the chart. For example,

```console
$ helm install gateway . -f values.yaml
```

> **Tip**: You can use the default [values.yaml](values.yaml)
