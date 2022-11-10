# Installation Guide of API Gateway
## Prerequisites
- Helm v3 [Installed](https://helm.sh/docs/intro/install/)

### Add tmax-cloud's chart repository to Helm:
```shell
helm repo add tmax-cloud https://tmax-cloud.github.io/charts/
```
## Installation Order 
### console version >= 5.2.0.0
1) Install [Cert-manager](https://github.com/tmax-cloud/charts/blob/main/charts/cert-manager/README.md)
2) Install [Gateway](https://github.com/tmax-cloud/charts/blob/main/charts/gateway/README.md)
3) Install [Oauth2-proxy](https://github.com/tmax-cloud/charts/blob/main/charts/oauth2-proxy/README.md)
4) Install [Console](https://github.com/tmax-cloud/charts/blob/main/charts/console/README.md)

## Repo List
### Available
- namespace
- cert-manager
- gateway (traefik)
- oauth2-proxy
- jwt-decode-auth
- console

### Not yet
- apps
