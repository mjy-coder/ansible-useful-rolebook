# k8s-pre-install Role

This Ansible role performs pre-installation checks for Kubernetes clusters using kubeadm, following the official Kubernetes documentation requirements.

## Table of Contents

- [Overview](#overview)
- [Requirements](#requirements)
- [Role Variables](#role-variables)
- [Checks Performed](#checks-performed)
- [Usage](#usage)
- [Official Documentation](#official-documentation)

## Overview

This role ensures that your system meets all the prerequisites required for installing Kubernetes using kubeadm, as specified in the official Kubernetes documentation. It performs comprehensive checks across various system components and configurations.

## Requirements

- Ansible 2.9 or higher
- Target systems: Linux (Debian-based or Red Hat-based distributions)

## Role Variables

The following variables are defined in `vars/main.yml`:

```yaml
# Required OS packages for Kubernetes installation
required_packages:
  - curl
  - wget
  - vim
  - git
  - net-tools
  - bind-utils
  - iptables
  - bridge-utils
  - bash-completion
  - chrony

# Minimum CPU cores required
min_cpu_cores: 2

# Minimum memory required (in MB)
min_memory_mb: 2048

# Required kernel modules
required_kernel_modules:
  - overlay
  - br_netfilter

# Required sysctl settings
sysctl_settings:
  net.bridge.bridge-nf-call-iptables: 1
  net.bridge.bridge-nf-call-ip6tables: 1
  net.ipv4.ip_forward: 1

# Container runtime requirements
container_runtime_checks:
  - docker
  - containerd
```

## Checks Performed

This role performs the following checks based on the official Kubernetes documentation:

### 1. System Information Checks

- MAC address uniqueness verification
- product_uuid uniqueness verification
- Hostname uniqueness verification

### 2. System Resources Checks

- Operating system version verification
- CPU core count verification (minimum 2 cores required)
- Memory verification (minimum 2048 MB required)
- Root partition disk space verification
- Swap disabled verification
- Time synchronization status verification

### 3. Kernel and System Configuration Checks

- Required kernel modules (overlay, br_netfilter) loaded and persisted
- Kubernetes required sysctl settings configured

### 4. Security and Firewall Checks

- SELinux status verification
- Firewall status verification
- iptables working status verification

### 5. Container Runtime and Dependencies Checks

- Container runtime (Docker, containerd) availability verification
- Required packages installation

### 6. Network and Connectivity Checks

- Kubernetes repository connectivity verification
- IP address configuration verification
- Hostname resolution verification

### 7. Port Checks

- Control plane ports (6443, 2379-2380, 10250-10252)
- Worker node ports (10250, 30000, 10256)

## Usage

To use this role in your playbook:

```yaml
- hosts: kubernetes_nodes
  become: yes
  roles:
    - k8s-pre-install
```

Run the playbook:

```bash
ansible-playbook -i inventory playbook.yml
```

## Official Documentation

This role follows the requirements specified in the official Kubernetes documentation:

[Install kubeadm | Kubernetes](https://v1-34.docs.kubernetes.io/zh-cn/docs/setup/production-environment/tools/kubeadm/install-kubeadm/)

## License

MIT

## Author Information

TraeAI
