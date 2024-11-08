+++
date = '2024-11-08T11:55:00+01:00'
draft = true
title = 'Network topology'
tags = ['network']
+++

# Network topology

{{<mermaid>}}
architecture-beta
    service vigilant(server)[vigilant]

    group proxmox(cloud)[zion]
    service dozer(server)[dozer] in proxmox
    service tank(server)[tank] in proxmox

    group main(cloud)[main]
    service logos(server)[logos] in main
    service mjolnir(server)[mjolnir] in main
    service mnemosyne(server)[mnemosyne] in main
    service vishnu(server)[vishnu] in main

    group turing(cloud)[turing]
    service cotijuba(server)[cotijuba] in turing
    service marajo(server)[marajo] in turing
    service outeiro(server)[outeiro] in turing
    service tapajos(server)[tapajos] in turing
{{</mermaid>}}


### Gateways and switches

| Host                                                 | IP address    | MAC address       |
|------------------------------------------------------|---------------|-------------------|
| WIFIHUBC2                                            | 192.168.0.1   | 00:cb:51:ee:68:59 |
| [U7 Pro](https://techspecs.ui.com/unifi/wifi/u7-pro) | 192.168.0.2   | d8:b3:70:f9:01:5c |
| DGS-1100-05PD                                        | 192.168.0.254 | 28:3b:82:f6:6f:bc |

### Servers

| Host      | IP address    | OS                 | Hardware                   | MAC address       | Serial number |
|-----------|---------------|--------------------|----------------------------|-------------------|---------------|
| logos     | 192.168.0.4   | Ubuntu 24.04.1 LTS | Raspberry Pi 4 Model B 8GB | dc:a6:32:d2:4e:dd | 1f0faa1f      |
| mjolnir   | 192.168.0.5   | Ubuntu 24.04.1 LTS | Raspberry Pi 4 Model B 8GB | dc:a6:32:b0:31:a3 | 4d9183f1      |
| mnemosyne | 192.168.0.6   | Ubuntu 24.04.1 LTS | Raspberry Pi 4 Model B 8GB | e4:5f:01:0c:a2:e7 | 417c071d      |
| vishnu    | 192.168.0.7   | Ubuntu 24.04.1 LTS | Raspberry Pi 4 Model B 8GB | e4:5f:01:a7:8c:68 | 3a1822b2      |
| vigilant  | 192.168.0.8   | Debian 11 bullseye | Raspberry Pi 3 Model B 1GB | b8:27:eb:7f:0c:6b | 187f0c6b      |
| tank      | 192.168.0.15  | Debian 12 bookworm | ThinkCentre M710 Tiny      | 6c:4b:90:54:e8:9c |               |
| dozer     | 192.168.0.16  | Debian 12 bookworm | ThinkCentre M710 Tiny      | 6c:4b:90:54:ea:7c |               |
| turingpi  | 192.168.0.170 | BMC firmware 2.0.5 | Turing Pi Cluster Board    | 02:00:d4:15:ac:46 |               |
| cotijuba  | 192.168.0.171 | Ubuntu 22.04.5 LTS | Turing RK1 16GB            | 52:88:92:a9:61:d3 |               |
| marajo    | 192.168.0.172 | Ubuntu 22.04.5 LTS | Turing RK1 16GB            | 8a:e3:32:23:e8:99 |               |
| outeiro   | 192.168.0.173 | Ubuntu 22.04.5 LTS | Turing RK1 16GB            | 2e:90:59:19:e6:0b |               |
| tapajos   | 192.168.0.174 | Ubuntu 22.04.5 LTS | Turing RK1 16GB            | d2:0b:c5:9d:69:6c |               |

### Proxmox VE

| Host  | IP address   | Roles |
|-------|--------------|-------|
| tank  | 192.168.0.15 | node  |
| dozer | 192.168.0.16 | node  |

#### Virtual machines

| Host      | IP address    | OS                 | MAC address       |
|-----------|---------------|--------------------|-------------------|
| vm01geth  | 192.168.0.150 | Ubuntu 24.04.1 LTS | bc:24:11:e6:35:e8 |
| vm03prysm | 192.168.0.151 | Ubuntu 24.04.1 LTS | bc:24:11:f3:20:c1 |

### Kubernetes clusters

#### Raspberry Pi cluster

| Host      | IP address  | Roles         |
|-----------|-------------|---------------|
| logos     | 192.168.0.4 | worker        |
| mjolnir   | 192.168.0.5 | control-plane |
| mnemosyne | 192.168.0.6 | control-plane |
| vishnu    | 192.168.0.7 | control-plane |

#### Turing Pi cluster

| Host     | IP address    | Roles         |
|----------|---------------|---------------|
| cotijuba | 192.168.0.171 | control-plane |
| marajo   | 192.168.0.172 | control-plane |
| outeiro  | 192.168.0.173 | control-plane |
| tapajos  | 192.168.0.174 | worker        |
