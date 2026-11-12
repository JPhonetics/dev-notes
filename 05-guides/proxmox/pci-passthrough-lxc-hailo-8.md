---
Created: 2026-10-09
Modified: 2026-11-11
---

# Hailo-8 PCI Passthrough to LXC

This guide installs the Hailo-8 PCIe kernel driver on a Proxmox host using the prebuilt `.deb` package available through the Hailo Developer Zone. The example uses HailoRT PCIe driver version `4.24.0`.

> [!WARNING]
> The installed Hailo PCIe driver must be compatible with the runtime used by the application accessing the device. Installing a newer driver can cause version mismatch errors.
>
> For Frigate, follow the [official Hailo installation instructions](https://docs.frigate.video/frigate/installation/#hailo-8) to install the supported driver and firmware versions. Do not assume that HailoRT `4.24.0` is compatible with your Frigate installation.

## Overview

1. [Install the Hailo-8 PCIe Driver](#install-the-hailo-8-pcie-driver)
2. [Related Documentation](#related-documentation)
3. [Official Documentation](#official-documentation)

## Install the Hailo-8 PCIe Driver

1. Refresh the Proxmox package index.

    - `sudo apt update` refreshes the available package information from Proxmox's configured repositories without installing updates.
        - `sudo` (superuser do) executes the command with administrative privileges.
        - `apt` is Debian's package management tool.

    ```bash
    sudo apt update
    ```

2. Download the Hailo PCIe driver package from the [Hailo Developer Zone](https://hailo.ai/developer-zone/) using a web browser, then transfer it to the Proxmox host through PowerShell. A Hailo Developer Zone account is required to access the download.

    - `scp "$HOME\Downloads\hailort-pcie-driver_4.24.0_all.deb" <USERNAME>@<IP_ADDRESS>:/tmp/` securely copies the downloaded driver package to the Proxmox host.
        - `scp` (Secure Copy) transfers files securely between computers over SSH.
        - `"$HOME\Downloads\hailort-pcie-driver_4.24.0_all.deb"` identifies the downloaded driver package in the Windows Downloads directory.
            - `$HOME` represents the current user's home directory.
            - `Downloads` is the directory containing the downloaded file.
            - `hailort-pcie-driver_4.24.0_all.deb` is the expected driver package filename.
        - `<USERNAME>@<IP_ADDRESS>` identifies the remote login account and Proxmox host.
            - `<USERNAME>` is the account used to connect to Proxmox.
            - `<IP_ADDRESS>` is the Proxmox host's IP address.
        - `:/tmp/` specifies the destination directory on the Proxmox host.

    ```powershell
    scp "$HOME\Downloads\hailort-pcie-driver_4.24.0_all.deb" <USERNAME>@<IP_ADDRESS>:/tmp/
    ```

3. Install the Hailo PCIe driver package on the Proxmox host.

    - `sudo dpkg --install /tmp/hailort-pcie-driver_4.24.0_all.deb` installs the downloaded driver package.
        - `dpkg` is Debian's low-level package management utility for working with `.deb` packages.
        - `--install` installs the specified package.
        - `/tmp/hailort-pcie-driver_4.24.0_all.deb` identifies the downloaded driver package.

    ```bash
    sudo dpkg --install /tmp/hailort-pcie-driver_4.24.0_all.deb
    ```

> [!WARNING]
> Rebooting the Proxmox host interrupts running virtual machines, LXCs, and services. Plan the reboot accordingly.

4. Reboot the Proxmox host if required by the driver installation instructions. A newly installed kernel driver or firmware may require a reboot before the hardware becomes available.

    - `sudo reboot` restarts the Proxmox host.

    ```bash
    sudo reboot
    ```

[🔼 Back to Overview](#overview)

## Related Documentation

- [Proxmox - PCI Passthrough to LXC](./pci-passthrough-lxc.md) — Identify PCI devices, verify drivers, and expose compatible device nodes to an LXC.

## Official Documentation

- [Hailo AI](https://hailo.ai/) — Official website for Hailo AI accelerators, hardware, and software.
- [Hailo Developer Zone](https://hailo.ai/developer-zone/) — Access Hailo driver packages, software downloads, and developer resources.
- [HailoRT 4.24.0 Documentation](https://hailo.ai/developer-zone/documentation/hailort-v4-24-0/) — Official documentation for the HailoRT 4.24.0 release.
- [Frigate Installation Documentation](https://docs.frigate.video/frigate/installation/#hailo-8) — Official instructions for installing compatible Hailo drivers and firmware for Frigate.

---

[🔼 Back to Proxmox - PCI Passthrough to LXC](./pci-passthrough-lxc.md)