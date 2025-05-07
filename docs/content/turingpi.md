+++
date = '2024-11-08T11:55:00+01:00'
draft = false
title = 'Turing Pi Cluster Board'
tags = ['cluster', 'turingpi']
+++

# Turing Pi Cluster Board

![Turing Pi 2](../images/turingpi-1.jpg)

## Specs

### Hardware

The cluster is composed of 4 [Turing RK1](https://docs.turingpi.com/docs/turing-rk1-specs-and-io-ports) Compute Modules with 16GB RAM and 1TB NVMe storage. Official documentation available [here](https://docs.turingpi.com/docs/turing-pi2-intro).

#### GPU

The **Rockchip RK3588 SoC** is equipped with a powerful **ARM Mali-G610MP4** quad-core GPU. Proper drivers and the [RKNN-Toolkit2](https://github.com/airockchip/rknn-toolkit2/) need to be installed and configured.

### Software

Ubuntu used in the nodes is 22.04 LTS, and the cluster is running Kubernetes. Image was downloaded from the [official website](https://firmware.turingpi.com/turing-rk1/).

The four nodes are running [K3s](https://k3s.io/) installed using [xanmanning.k3s](https://galaxy.ansible.com/ui/standalone/roles/xanmanning/k3s/) Ansible role.

Initially I was using [onedr0p/cluster-template](https://github.com/onedr0p/cluster-template) to bootstrap and update the cluster, but since [#1482](https://github.com/onedr0p/cluster-template/pull/1482) support for K3s was dropped in favor of [Talos](https://www.siderolabs.com/platform/talos-os-for-kubernetes/).

GitOps workflow is used to maintain the workloads. [FluxCD](https://fluxcd.io/) listens to the repository and reconciles the changes upon new commits due to a webhook.

[SOPS](https://getsops.io/) is used to commit secrets to the public repo in a encrypted format, the cluster has a decryption key to be able to read and use the secrets.
