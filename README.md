# helm-dnspod-webhook-cert-manager-example

dnspod-webhook-cert-manager is a cert-manager plugin to manage SSL certificates on Kubernetes. 

Along with Nginx Ingress, you can configure HTTPS access to the website. With Let's Encrypt provide free SSL certificates, all the free packs of cert-manager + Nginx Ingress + Let's Encrypt are automatic generated. But cert-manager does not support dnspod by default. 

You can modify it and use dnspod-webhook-cert-manager. There is example about installing it with Helm in the article.

## Prerequisites

- [Kubernetes(K8S)](https://kubernetes.io/)
  
    Kubernetes (K8s) is an open-source system for automating deployment, scaling, and management of containerized applications.

- [Helm](https://helm.sh/)
  
    Helm is the best way to find, share, and use software built for Kubernetes.

- [cert-manager](https://cert-manager.io/)
  
    x509 certificate management for Kubernetes.

- [DNSPod](https://support.dnspod.cn/account/5f2d466de8320f1a740d9ff3/) API ID and Token.
    
    DNSPod is the largest free DNS service provider in China.

## How to Install

```shell
# git clone example and dnspod-webhook-cert-manager.
$ git clone --recursive https://github.com/CloudoLife/helm-dnspod-webhook-cert-manager-example

$ cd helm-dnspod-webhook-cert-manager-example
```

### Custom Values.yaml

Edit [values.yaml](./values.yaml) in helm-dnspod-webhook-cert-manager-example directory, and replace content within < and >.
```shell
# cert-manager-webhook-dnspod/values.yaml at master · qqshfox/cert-manager-webhook-dnspod
# https://github.com/qqshfox/cert-manager-webhook-dnspod/blob/master/deploy/cert-manager-webhook-dnspod/values.yaml

# The GroupName here is used to identify your company or business unit that
# created this webhook.
# For example, this may be "acme.mycompany.com".
# This name will need to be referenced in each Issuer's `webhook` stanza to
# inform cert-manager of where to send ChallengePayload resources in order to
# solve the DNS01 challenge.
# This group name should be **unique**, hence using your own company's domain
# here is recommended.
groupName: <Your Company>

secrets:
  # 密钥管理 - DNSPod 官方帮助中心-技术支持-DNSPod-免费智能DNS解析服务商-电信_网通_教育网,智能DNS
  # https://support.dnspod.cn/account/5f2d466de8320f1a740d9ff3/
  apiID: <Your DNSPod API ID>
  apiToken:  <Your DNSPod API Token>

clusterIssuer:
  enabled: true
  # staging: false
  email: <Your email>

  # https://cert-manager.io/docs/configuration/acme/#adding-multiple-solver-types
  selector:
    dnsZones:
      - <Your domain>

image:
#   repository: qqshfox/cert-manager-webhook-dnspod
# Or
#   repository: cloudolife/cert-manager-webhook-dnspod
# Or
  repository: <Your Repository>/cert-manager-webhook-dnspod
  tag: latest
  pullPolicy: IfNotPresent

# nameOverride: ""
# fullnameOverride: ""

# service:
#   type: ClusterIP
#   port: 443

# resources: {}
#   # We usually recommend not to specify default resources and to leave this as a conscious
#   # choice for the user. This also increases chances charts run on environments with little
#   # resources, such as Minikube. If you do want to specify resources, uncomment the following
#   # lines, adjust them as necessary, and remove the curly braces after 'resources:'.
#   # limits:
#   #  cpu: 100m
#   #  memory: 128Mi
#   # requests:
#   #  cpu: 100m
#   #  memory: 128Mi

# nodeSelector: {}

# tolerations: []

# affinity: {}
```

### Install by Helm

Helm install cert-manager-webhook-dnspod within cert-manager namespace. Please create cert-manager namespace first if not exist.

```shell
$ helm install --name cert-manager-webhook-dnspod --namespace=cert-manager ./cert-manager-webhook-dnspod/deploy/webhook-dnspod -f values.yaml
```

See Helm release about cert-manager-webhook-dnspod.

```shell
$ helm list -n cert-manager
NAME                       	NAMESPACE   	REVISION	UPDATED                               	STATUS  	CHART                            	APP VERSION
cert-manager-webhook-dnspod	cert-manager	1       	2020-09-25 12:57:30.168666 +0800 +0800	deployed	webhook-dnspod-0.1.0             	1.0
```

See pods about cert-manager-webhook-dnspod.

```shell
$ kubectl get pods -n cert-manager
...
```

## References
[1] [CloudoLife/helm-dnspod-webhook-cert-manager-example: Examples about Helm install dnspod Webhook cert-manager. https://github.com/CloudoLife/helm-dnspod-webhook-cert-manager-example - https://github.com/CloudoLife/helm-dnspod-webhook-cert-manager-example](https://github.com/CloudoLife/helm-dnspod-webhook-cert-manager-example)

[2] [qqshfox/cert-manager-webhook-dnspod: DNSPod Webhook for Cert Manager - https://github.com/qqshfox/cert-manager-webhook-dnspod](https://github.com/qqshfox/cert-manager-webhook-dnspod)

[3] [cert-manager - https://cert-manager.io/](https://cert-manager.io/)

[4] [jetstack/cert-manager-webhook-example: A cert-manager sample repository for creating an ACME DNS01 solver webhook - https://github.com/jetstack/cert-manager-webhook-example](https://github.com/jetstack/cert-manager-webhook-example)

[5] [Helm - https://helm.sh/](https://helm.sh/)

[6] [Kubernetes - https://kubernetes.io/](https://kubernetes.io/)

[7] [密钥管理 - DNSPod 官方帮助中心-技术支持-DNSPod-免费智能DNS解析服务商-电信_网通_教育网,智能DNS - https://support.dnspod.cn/account/5f2d466de8320f1a740d9ff3/](https://support.dnspod.cn/account/5f2d466de8320f1a740d9ff3/)
