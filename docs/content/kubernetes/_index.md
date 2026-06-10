+++
date = '2025-05-08T23:53:24+02:00'
draft = false
title = 'Kubernetes'
description = 'Namespaces, workloads, and routes running on the Raspberry Pi K3s cluster.'
tags = ['kubernetes']
weight = 10
+++

# Kubernetes cluster

![Raspberry Pi Cluster](../images/rpi-1.jpg)

#### Namespaces

| Name               | Description                                                       |
| ------------------ | ---------------------------------------------------------------- |
| default            | Homepage dashboard and the `willian.eth` site                    |
| flux-system        | FluxCD GitOps controllers and webhook receiver                   |
| immich             | Immich photo & video management                                  |
| knative            | Knative Operator (serverless)                                    |
| kube-public        | Default Kubernetes public namespace                              |
| kube-system        | Core components: Cilium, CoreDNS, metrics-server, reloader       |
| local-path-storage | local-path-provisioner                                           |
| media              | Jellyfin and the *arr stack (Sonarr, Radarr, Prowlarr, …)        |
| network            | Gateways, DNS, certificates and the Cloudflare tunnel            |
| observability      | kube-prometheus-stack (Prometheus, Grafana, Alertmanager)        |
| storage            | Longhorn and CloudNativePG                                       |
| system-upgrade     | system-upgrade-controller (K3s upgrades)                         |

#### Workloads

Applications reconciled by Flux (`HelmRelease`s); some charts create additional Deployments/StatefulSets.

| Namespace          | Name                        |
| ------------------ | --------------------------- |
| default            | homepage                    |
| default            | willian-eth                 |
| immich             | immich                      |
| knative            | knative-operator            |
| kube-system        | cilium                      |
| kube-system        | metrics-server              |
| kube-system        | reloader                    |
| local-path-storage | local-path-provisioner      |
| media              | bazarr                      |
| media              | booklore                    |
| media              | flaresolverr                |
| media              | jellyfin                    |
| media              | jellyseerr                  |
| media              | prowlarr                    |
| media              | qbittorrent                 |
| media              | radarr                      |
| media              | recyclarr                   |
| media              | sonarr                      |
| network            | cert-manager                |
| network            | cloudflared                 |
| network            | external-dns (cloudflare)   |
| network            | external-dns (pihole)       |
| network            | k8s-gateway                 |
| observability      | kube-prometheus-stack       |
| storage            | cloudnative-pg              |
| storage            | longhorn                    |
| system-upgrade     | system-upgrade-controller   |

#### HTTPRoutes

Services are exposed through the Cilium Gateway API via the `internal` and `external` gateways (the legacy NGINX Ingress has been retired).

| Namespace     | Name                  | Hostname                                                   | Gateway  |
| ------------- | --------------------- | --------------------------------------------------------- | -------- |
| default       | homepage              | ${SECRET_DOMAIN}                                          | internal |
| default       | willian-eth           | willian.${SECRET_DOMAIN}                                  | external |
| immich        | immich                | immich.${SECRET_DOMAIN}                                   | external |
| media         | jellyfin              | jellyfin.${SECRET_DOMAIN}                                 | external |
| media         | booklore              | booklore.${SECRET_DOMAIN}                                 | external |
| observability | kube-prometheus-stack | prometheus / alertmanager / grafana.${SECRET_DOMAIN}      | internal |
| storage       | longhorn              | longhorn.raspberry.${SECRET_DOMAIN}                       | internal |
| kube-system   | hubble                | hubble.raspberry.${SECRET_DOMAIN}                         | internal |
| flux-system   | flux-webhook          | flux-webhook.${SECRET_DOMAIN}                             | external |
