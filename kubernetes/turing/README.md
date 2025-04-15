# Kubernetes Turing Pi Cluster

This directory contains the configuration for the [Turing Pi](https://turingpi.com/) Kubernetes cluster, which is part of the homelab infrastructure. The cluster is managed using [Flux](https://fluxcd.io/) for GitOps and runs various media and photo management services.

## Cluster Specifications

### Hardware

The Turing Pi cluster consists of:

**[Turing Pi 2 Cluster Board](https://turingpi.com/product/turing-pi-2/)**: BMC firmware 2.0.5

| Node Name | IP Address     | Hardware                                                    | Role          |
|-----------|----------------|-------------------------------------------------------------|---------------|
| cotijuba  | 192.168.0.171  | [Turing RK1](https://turingpi.com/product/turing-rk1/) 16GB | Control Plane |
| marajo    | 192.168.0.172  | [Turing RK1](https://turingpi.com/product/turing-rk1/) 16GB | Control Plane |
| outeiro   | 192.168.0.173  | [Turing RK1](https://turingpi.com/product/turing-rk1/) 16GB | Control Plane |
| tapajos   | 192.168.0.174  | Worker                                                      | Worker        |

## Directory Structure

| Directory       | Purpose                                                           |
|-----------------|-------------------------------------------------------------------|
| flux/           | [Flux](https://fluxcd.io/) configuration and repositories         |
| flux-system/    | Core Flux system configurations and secrets                       |
| immich/         | [Immich](https://immich.app/) photo management application        |
| kube-system/    | Core Kubernetes system components                                 |
| media/          | Media services ([Jellyfin](https://jellyfin.org/))                |
| network/        | Networking components (ingress, DNS, certificates)                |
| observability/  | Monitoring with [Prometheus](https://prometheus.io/)              |
| storage/        | Storage configurations                                            |
| system-upgrade/ | K3s system upgrade controller                                     |

## Key Components

### Network

| Component                                                    | Description                                                               |
|--------------------------------------------------------------|---------------------------------------------------------------------------|
| [Ingress NGINX](https://github.com/kubernetes/ingress-nginx) | Internal ingress controller (192.168.0.169)                               |
| [K8s Gateway](https://github.com/ori-edge/k8s_gateway)       | DNS for internal service discovery (192.168.0.168)                        |
| [Cert Manager](https://cert-manager.io/)                     | TLS certificate management with [Let's Encrypt](https://letsencrypt.org/) |

### Storage

| Component                                            | Description                                                                                             |
|------------------------------------------------------|---------------------------------------------------------------------------------------------------------|
| [Longhorn](https://longhorn.io/)                     | Persistent storage with backup to [Backblaze B2](https://www.backblaze.com/cloud-storage) Cloud Storage |
| [CSI Driver NFS](https://github.com/kubernetes-csi/csi-driver-nfs) | For NFS volume mounting                                                                   |

### Applications

| Application                          | Description                                                      |
|--------------------------------------|------------------------------------------------------------------|
| [Jellyfin](https://jellyfin.org/)    | Media server with hardware acceleration using Rockchip GPU       |
| [Immich](https://immich.app/)        | Self-hosted photo and video backup solution with ML capabilities |

### System

| Component                                                                         | Description                                                                                                         |
|-----------------------------------------------------------------------------------|---------------------------------------------------------------------------------------------------------------------|
| [Cilium](https://cilium.io/)                                                      | Networking with [ClusterMesh](https://docs.cilium.io/en/stable/network/clustermesh/) for multi-cluster connectivity |
| [Metrics Server](https://github.com/kubernetes-sigs/metrics-server)               | For pod resource metrics                                                                                            |
| [Reloader](https://github.com/stakater/Reloader)                                  | For automatic reloading of configmaps and secrets                                                                   |
| [System Upgrade Controller](https://github.com/rancher/system-upgrade-controller) | For automated K3s version upgrades                                                                                  |

## Hardware Acceleration

The cluster leverages Rockchip GPU hardware acceleration for media transcoding and photo processing:

| Device/Component     | Purpose                                      |
|----------------------|----------------------------------------------|
| `/dev/dri`           | Direct Rendering Infrastructure              |
| `/dev/mali0`         | Mali GPU device                              |
| `/dev/rga`           | Rockchip RGA (Raster Graphic Acceleration)   |
| `/dev/mpp_service`   | Media Process Platform service               |
| Mali firmware        | GPU acceleration libraries                   |
