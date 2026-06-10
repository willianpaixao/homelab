+++
date = '2024-11-07T18:54:11+01:00'
draft = false
title = 'Homelab'
description = 'Documentation for a GitOps-managed homelab running two K3s Kubernetes clusters on Raspberry Pi and Turing Pi hardware.'
+++

# Willian's Homelab

Welcome to the documentation for my personal homelab infrastructure. This site details the configuration and setup of my Kubernetes clusters, managed using a GitOps approach with FluxCD. The goal is to maintain a reproducible and automated environment.

## Architecture Overview

The homelab is built around two distinct Kubernetes clusters:

*   **Raspberry Pi Cluster:** A K3s cluster running on several Raspberry Pi 4 nodes. This cluster handles general workloads and services.
*   **Turing Pi Cluster:** A K3s cluster utilizing a Turing Pi 2 board with RK1 compute modules. This cluster is often used for specific tasks or experimentation.

Both clusters are managed declaratively via GitOps, pulling their configuration from the [GitHub repository](https://github.com/willianpaixao/homelab). The reconciliation is triggered upon push via webhooks.

For an overview of the workloads running on the clusters, see the [Kubernetes](kubernetes) section.

## Hardware

For details on the Turing Pi 2 cluster board and its RK1 compute modules, see the [Turing Pi Cluster](turingpi) page.

## Roadmap

- Kubernetes
  - [ ] Setup Network Policies to all workloads
  - [ ] Setup Service Account to all workloads

- Network
  - [ ] Configure Multi-cluster
  - [ ] Configure VLANs
  - [ ] Configure IPv6
- Linux
  - [ ] Experiment with [Talos Linux](https://www.siderolabs.com/platform/talos-os-for-kubernetes/) in favor of Ubuntu
- Misc
  - [ ] Implement autoscaling and automatic shutdown of nodes at night
