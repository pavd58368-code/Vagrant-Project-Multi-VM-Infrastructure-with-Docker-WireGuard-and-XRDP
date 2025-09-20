Vagrant Project: Multi-VM Infrastructure with Docker, WireGuard, and XRDP
Objective

Deploy three virtual machines using Vagrant with automated configuration via Bash scripts. Each VM is configured according to specific requirements for software, networking, and remote access.

Environment Setup

Base box: ubuntu/jammy64

Private network with static IPs for each VM

/vagrant folder synchronization disabled

SSH configuration on all VMs:

Protocol 2

LoginGraceTime: 10s

MaxAuthTries: 2

MaxSessions: 2

PasswordAuthentication: no

PubkeyAuthentication: yes

PermitRootLogin: no

All packages updated

Hostname set for each VM

User ubuntu password: 123456ubuntu

VM Configurations
VM1

Installed packages: ubuntu-desktop-minimal, snap-store, chromium

XRDP installed and configured for remote desktop access

Port forwarding configured in Vagrantfile for XRDP

SSH access from VM1 to VM2 and VM3 using hostnames under user ubuntu

VM2

Docker installed from the official repository

wireguard-tools installed

Wireguard-UI installed and configured with auto-restart on configuration changes

SSH access from VM1 to VM2 under user ubuntu

VM3

User adam created with:

Home directory /home/adam

Shell /bin/bash

Primary group adam

Sudo privileges without password

SSH access from VM1 to VM3 under user ubuntu

Demonstration & Verification

All VMs boot successfully with static IPs

XRDP connection to VM1 verified

Docker and WireGuard UI on VM2 operational

User adam created on VM3 and sudo access verified

SSH connectivity from VM1 to VM2 and VM3 verified using hostnames
