# HyperCloud Console

[hypercloud-console](https://github.com/tmax-cloud/console) 5.0 설치 가이드  

## TL;DR;

```console
$ git clone https://github.com/tmax-cloud/charts.git
$ cd console
$ helm install console .
```

## Introduction

This chart bootstraps an console deployment on a [Kubernetes](http://kubernetes.io) cluster using the [Helm](https://helm.sh) package manager.

## Installing the Chart

To install the chart with the release name `console` on a api-gateway-system namespace:

```console
$ helm install console . --namespace api-gateway-system --create-namespace --wait
```

The [configuration](#configuration) section lists the parameters that can be configured during installation.

## Uninstalling the Chart

To uninstall/delete the `my-release` deployment:

```console
$ helm uninstall -n api-gateway-system console
```

The command removes all the Kubernetes components associated with the chart and deletes the release.

## Configuration

The following table lists the configurable parameters of the oauth2-proxy chart and their default values.

| Parameter                          | Description                                                                              | Default                                      |
|------------------------------------|------------------------------------------------------------------------------------------|----------------------------------------------|
| `global.serviceAccount`            | serviceAccount token                                                                     | `console`                                    |
| `global.domain`                    | domain name                                                                              | `""`                                         |
| `global.consoleDomain`             | sub domain name                                                                          | `""`                                         |
| `global.hyperAuth.address`         | hyperAuth address                                                                        | `""`                                         |
| `global.hyperAuth.realm`           | hyperAuth realm                                                                          | `""`                                         |
| `console.clientID`                 | hyperAuth client ID                                                                      | `""`                                         |
| `console.mcMode`                   | multiCluster mode (true), singleCluster (false)                                          | `true`                                       |
| `console.chatbotEmbed`             | Flag to enable chatbot                                                                   | `true`                                       |
| `console.logInfo.logLevel`         | log Level ()                                                                             | `"debug"`                                    |
| `console.logInfo.logType`          | log Type ()                                                                              | `"pretty"`                                   |
| `image.repository`                 | console image repository                                                                 | `"docker.io/tmaxcloudck/hypercloud-console"` |
| `image.tag`                        | console image version(tag)                                                               | `"5.0.78.0"`                                 |
| `oauth2-proxy.enabled`             | Flag to install oauth2-proxy                                                             | `ture`                                       |
| `oauth2-proxy.config.clientID`     | hyperAuth client ID                                                                      | `""`                                         |
| `oauth2-proxy.config.clientSecret` | hyperAuth client Secret                                                                  | `""`                                         |
| `oauth2-proxy.config.cookieSecret` | [cookie Secret](https://oauth2-proxy.github.io/oauth2-proxy/docs/configuration/overview) | `""`                                         | 
| `oauth2-proxy.image.repository`    | oauth2-proxy image repository                                                            | `"docker.io/tmaxcloudck/oauth2-proxy"`       |
| `oauth2-proxy.image.tag`           | oauth2-proxy version(tag)                                                                | `"v7.3.0"`                                   |
| `jwt-decode-auth.enabled`          | Falg to install jwt-decode-auth                                                          | `true`                                       |
| `jwt-decode-auth.logInfo.logLevel` | log Level ()                                                                             | `"debug"`                                    |
| `jwt-decode-auth.logInfo.logType`  | log Type ()                                                                              | `"pretty"`                                   |
| `jwt-decode-auth.image.repository` | jwt-decode-auth image repository                                                         | `"docker.io/tmaxcloudck/jwt-decode"`         |
| `jwt-decode-auth.image.tag`        | jwt-decode-auth image tag                                                                | `"5.0.0.3"`                                  |

Specify each parameter using the `--set key=value[,key=value]` argument to `helm install`. For example,

```console
$ helm install console . \
  --set=image.tag=v0.0.2,global.domain=tmaxcloud.org
```

Alternatively, a YAML file that specifies the values for the above parameters can be provided while installing the chart. For example,

```console
$ helm install console . -f values.yaml
```

> **Tip**: You can use the default [values.yaml](values.yaml)


## Extra environment variable templating
The extraEnv value supports the tpl function which evaluate strings as templates inside the deployment template.
This is useful to pass a template string as a value to the chart's extra environment variables and to render external configuration environment values


```yaml
...
tplValue: "This is a test value for the tpl function"
extraEnv:
  - name: TEST_ENV_VAR_1
    value: test_value_1
  - name: TEST_ENV_VAR_2
    value: '{{ .Values.tplValue }}'
```