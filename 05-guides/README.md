---
Created: 2026-11-11
Modified:
---

# Guides

This directory contains practical, step-by-step technical guides for installing, configuring, managing, and troubleshooting operating systems, software, and infrastructure.

Each guide focuses on a specific task, explaining the necessary concepts, commands, configuration options, and verification steps. Guides are organized by technology to make procedures easier to locate and revisit.

## Linux

Guides for managing Linux operating systems, user accounts, permissions, and system configurations.

- [Create a Linux User](./linux/create-linux-user.md) — Check existing users, create accounts, manage passwords, configure sudo privileges, and verify user access.

## Proxmox

Guides for managing Proxmox Virtual Environment (Proxmox VE), including virtual machines, Linux containers, hardware passthrough, and host configuration.

- [Accessing Proxmox VM and LXC Consoles](./proxmox/accessing-proxmox-vm-lxc-consoles.md) — Access virtual machines and Linux containers through the Proxmox web interface or directly from the host, and understand SSH access restrictions.

- [PCI Passthrough to LXC](./proxmox/pci-passthrough-lxc.md) — Identify PCI devices, prepare kernel drivers, and expose compatible device nodes to Linux containers.

    - [Hailo-8 PCIe Driver](./proxmox/pci-passthrough-lxc-hailo-8.md) — Install the Hailo-8 PCIe driver on a Proxmox host for use with Linux containers.

---

[🔼 Back to Dev Notes](../README.md)