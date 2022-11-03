# Installation Guide for Master Cluster
## Prerequisites
- Helm v3 [Installed](https://helm.sh/docs/intro/install/)
Add tmax-cloud's chart repository to Helm:
```shell
helm repo add tmax-cloud https://https://tmax-cloud.github.io/charts/
```
## Repo List
- apps
- cert-manager
- console
- gateway(traefik)
- jwt-decode-auth
- namespace
- oauth2-proxy

[//]: # (```)

[//]: # (chart url : https://tmax-cloud.github.io/charts/)

[//]: # (step 1. install namespace )

[//]: # ()
[//]: # (```shell)

[//]: # ($ cd namespace)

[//]: # ($ cat << EOF > ns-values.yaml)

[//]: # (namespace: api-gateway-system)

[//]: # ()
[//]: # (#specifies the quota to be used for resources)

[//]: # (quota:)

[//]: # (  enabled: true)

[//]: # (  requests:)

[//]: # (    cpu: '1')

[//]: # (    memory: 1Gi)

[//]: # (  limits:)

[//]: # (    cpu: '2')

[//]: # (    memory: 2Gi)

[//]: # (  pods: "10")

[//]: # (  persistentvolumeclaims: "1")

[//]: # (  resourcequotas: "1")

[//]: # (  services: "5")

[//]: # ()
[//]: # (#specifies the limit ranges for the chart)

[//]: # (limits:)

[//]: # (  enabled: true)

[//]: # (  default:)

[//]: # (    cpu: 200m)

[//]: # (    memory: 256Mi)

[//]: # (  defaultRequest:)

[//]: # (    cpu: 100m)

[//]: # (    memory: 128Mi)

[//]: # (  type: Container)

[//]: # (EOF)

[//]: # ($ helm install gateway-namespace . --values ns-valuse.yaml --wait)

[//]: # (# or )

[//]: # ($ helm upgrade -i gateway-namespace . --values ns-valuse.yaml --wait)

[//]: # (```)

[//]: # ()
[//]: # (step 2. install gateway on api-gateway-system namespaces)

[//]: # (```shell)

[//]: # ($ cd gateway)

[//]: # ($ cat << EOF > gateway-values.yaml)

[//]: # (tls:)

[//]: # (  domain: local.test)

[//]: # (  acme:)

[//]: # (    enabled: false)

[//]: # (    email: temp@tmax.co.kr)

[//]: # (    dns:)

[//]: # (      type: route53)

[//]: # (      accessKeyID: accessKeyID)

[//]: # (      accessKeySecret: accessKeySecret)

[//]: # (      hostedZoneID: hostZoneID)

[//]: # (    # staging or production)

[//]: # (    environment: production)

[//]: # (  selfsigned:)

[//]: # (    enabled: true)

[//]: # ()
[//]: # (certmanager:)

[//]: # (  enabled: true)

[//]: # (  fullnameOverride: cert-manager)

[//]: # (  extraArgs:)

[//]: # (    - "--dns01-recursive-nameservers=8.8.8.8:53")

[//]: # (    - "--dns01-recursive-nameservers-only")

[//]: # (    - "--enable-certificate-owner-ref")

[//]: # ()
[//]: # (traefik:)

[//]: # (  enabled: true)

[//]: # (  fullnameOverride: traefik)

[//]: # (  additionalArguments:)

[//]: # (    - "--serverstransport.rootcas=/var/run/secrets/tmaxcloud/ca.crt, /var/run/secrets/kubernetes.io/serviceaccount/ca.crt")

[//]: # (    - "--entrypoints.websecure.http.middlewares=cors-header@file")

[//]: # (    - "--providers.file")

[//]: # (    - "--providers.file.directory=/gateway-config")

[//]: # (    - "--providers.file.watch=true")

[//]: # (  image:)

[//]: # (    name: traefik)

[//]: # (    tag: "v2.8.0")

[//]: # (  deployment:)

[//]: # (    podLabels:)

[//]: # (      app: traefik)

[//]: # (    imagePullSecrets: [])

[//]: # (  volumes:)

[//]: # (    - name: gateway-config)

[//]: # (      mountPath: "/gateway-config")

[//]: # (      type: configMap)

[//]: # (    - name: selfsigned-ca)

[//]: # (      mountPath: "/var/run/secrets/tmaxcloud")

[//]: # (      type: secret)

[//]: # (  service:)

[//]: # (    # LoadBalancer , NodePort, ClusterIP)

[//]: # (    type: NodePort)

[//]: # (    annotations:)

[//]: # (      service.beta.kubernetes.io/aws-load-balancer-type: nlb)

[//]: # (      service.beta.kubernetes.io/aws-load-balancer-internal: "true")

[//]: # (  tolerations:)

[//]: # (    - key: "node-role.kubernetes.io/master")

[//]: # (      operator: Exists)

[//]: # (EOF)

[//]: # ($ helm install gateway . --values gateway-values.yaml --namespace api-gateway-system --wait --timeout 7m)

[//]: # (# or)

[//]: # ($ helm upgrade -i gateway . --values gateway-values.yaml --namespace api-gateway-system --wait --timeout 7m)

[//]: # (```)

[//]: # ()
[//]: # (step 3. install console )

[//]: # (```shell)

[//]: # (cd console)

[//]: # (cat << EOF > console-values.yaml)

[//]: # (# Default values for console.)

[//]: # (# This is a YAML-formatted file.)

[//]: # (# Declare variables to be passed into your templates.)

[//]: # (global:)

[//]: # (  serviceAccount: console)

[//]: # (  domain: local.test)

[//]: # (  consoleDomain: console)

[//]: # (  hyperAuth:)

[//]: # (    address: hyperauth.tmaxcloud.org)

[//]: # (    realm: tmax)

[//]: # ()
[//]: # (fullnameOverride: "console")

[//]: # ()
[//]: # (console:)

[//]: # (  clientID: hypercloud5)

[//]: # (  mcMode: true)

[//]: # (  chatbotEmbed: true)

[//]: # (  logInfo:)

[//]: # (    logLevel: debug)

[//]: # (    logType: pretty)

[//]: # (image:)

[//]: # (  repository: docker.io/tmaxcloudck/hypercloud-console)

[//]: # (  pullPolicy: IfNotPresent)

[//]: # (  # Overrides the image tag whose default is the chart appVersion.)

[//]: # (  #  tag: "oauth")

[//]: # (  tag: "5.0.78.0")

[//]: # (imagePullSecrets: [])

[//]: # (#imagePullSecrets:)

[//]: # (#  - name: regcred)

[//]: # ()
[//]: # (oauth2-proxy:)

[//]: # (  enabled: true)

[//]: # (  fullnameOverride: "oauth2-proxy")

[//]: # (  config:)

[//]: # (    clientID: "hypercloud5")

[//]: # (    clientSecret: "7119dfca-8bf6-4f7a-a147-52423769547d")

[//]: # (    cookieSecret: "iM9b7w6ZkDDl4Y72oGmwZA2b5-PkFYGcIxfFTq93gSE=")

[//]: # (  image:)

[//]: # (    repository: "docker.io/tmaxcloudck/oauth2-proxy")

[//]: # (    tag: "v7.3.0")

[//]: # (    imagePullSecrets: [])

[//]: # (  ingressroute:)

[//]: # (    enabled: true)

[//]: # ()
[//]: # (jwt-decode-auth:)

[//]: # (  enabled: true)

[//]: # (  fullnameOverride: "jwt-decode-auth")

[//]: # (  logInfo:)

[//]: # (    logLevel: "debug")

[//]: # (    logType: "pretty")

[//]: # (  image:)

[//]: # (    repository: "docker.io/tmaxcloudck/jwt-decode")

[//]: # (    tag: "5.0.0.3")

[//]: # (  imagePullSecrets: [])

[//]: # ()
[//]: # ()
[//]: # (ingressroute:)

[//]: # (  enabled: true)

[//]: # ()
[//]: # (service:)

[//]: # (  type: ClusterIP)

[//]: # (  port: 31303)

[//]: # ()
[//]: # (#nodeSelector:)

[//]: # (#  node-role.kubernetes.io/master: "")

[//]: # ()
[//]: # (#tolerations: [])

[//]: # (tolerations:)

[//]: # (  - key: "node-role.kubernetes.io/master")

[//]: # (    operator: Exists)

[//]: # ()
[//]: # (affinity:)

[//]: # (  podAffinity:)

[//]: # (    preferredDuringSchedulingIgnoredDuringExecution:)

[//]: # (      - podAffinityTerm:)

[//]: # (          topologyKey: kubernetes.io/hostname)

[//]: # (          labelSelector:)

[//]: # (            matchExpressions:)

[//]: # (              - key: app)

[//]: # (                operator: In)

[//]: # (                values:)

[//]: # (                  - traefik)

[//]: # (        weight: 100)

[//]: # ()
[//]: # (replicaCount: 1)

[//]: # ()
[//]: # (resources: {})

[//]: # (  # We usually recommend not to specify default resources and to leave this as a conscious)

[//]: # (  # choice for the user. This also increases chances charts run on environments with little)

[//]: # (  # resources, such as Minikube. If you do want to specify resources, uncomment the following)

[//]: # (  # lines, adjust them as necessary, and remove the curly braces after 'resources:'.)

[//]: # (  # limits:)

[//]: # (  #   cpu: 100m)

[//]: # (  #   memory: 128Mi)

[//]: # (  # requests:)

[//]: # (  #   cpu: 100m)

[//]: # (#   memory: 128Mi)

[//]: # ()
[//]: # (podAnnotations: {})

[//]: # (podSecurityContext:)

[//]: # (  {})

[//]: # (  # fsGroup: 2000)

[//]: # (securityContext:)

[//]: # (  {})

[//]: # (  # capabilities:)

[//]: # (  #   drop:)

[//]: # (  #   - ALL)

[//]: # (  # readOnlyRootFilesystem: true)

[//]: # (  # runAsNonRoot: true)

[//]: # (  # runAsUser: 1000)

[//]: # (EOF)

[//]: # (helm install console . --values console-values.yaml --wait --timeout 3m)

[//]: # (# or )

[//]: # (helm upgrade -i console . --values console-values.yaml --wait --timeout 3m )

[//]: # (```)
