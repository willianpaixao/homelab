+++
date = '2025-05-08T23:53:24+02:00'
draft = false
title = 'Kubernetes'
tags = ['kubernetes']
+++

# Kubernetes cluster

![Raspberry Pi Cluster](../images/rpi-1.jpg)

#### Namespaces

| Name           | Description |
| -------------- | ----------- |
| default        |             |
| flux-system    |             |
| immich         |             |
| kube-public    |             |
| kube-system    |             |
| media          |             |
| network        |             |
| observability  |             |
| storage        |             |
| system-upgrade |             |

#### Deployments

| Namespace      | Name                              |
| -------------- | --------------------------------- |
| immich         | immich                            |
| kube-system    | csi-nfs-controller                |
| kube-system    | metrics-server                    |
| kube-system    | reloader                          |
| media          | jellyfin                          |
| network        | cert-manager                      |
| network        | external-dns                      |
| network        | ingress-nginx-internal-controller |
| network        | k8s-gateway                       |
| observability  | prometheus-agent                  |
| storage        | longhorn                          |
| system-upgrade | system-upgrade-controller         |

#### Ingress

| Namespace   | Name         | Ingress                                  | Ingress Class |
| ----------- | ------------ | ---------------------------------------- | ------------- |
| default     | homepage     | https://${SECRET_DOMAIN}                 | internal      |
| default     | willian-eth  | https://willian.${SECRET_DOMAIN}         | external      |
| immich      | immich       | https://immich.${SECRET_DOMAIN}          | internal      |
| media       | jellyfin     | https://jellyfin.${SECRET_DOMAIN}        | internal      |
| storage     | longhorn     | https://longhorn.turing.${SECRET_DOMAIN} | internal      |
| flux-system | flux-webhook | https://flux-webhook.${SECRET_DOMAIN}    | external      |
