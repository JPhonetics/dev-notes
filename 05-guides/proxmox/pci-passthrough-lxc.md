---
Created: 2026-10-09
Modified:
---

# Proxmox - PCI Passthrough to LXC

This guide documents the steps to expose a PCI or PCIe device to a Linux Container (LXC) in Proxmox. Because an LXC shares the Proxmox host's Linux kernel, the hardware's kernel driver must be available on the Proxmox host. The host driver provides the device interface that is then exposed to the LXC. The container installs any required user-space runtime, libraries, or applications.

See [Device-Specific Installation](#device-specific-installation) for device-specific driver installation instructions. These guides contain only the steps unique to that device. Return to this guide afterward to complete the LXC passthrough process.

## Overview

1. [Verify Proxmox Detects the PCI Device](#verify-proxmox-detects-the-pci-device)
2. [Check for the PCI Device Driver](#check-for-the-pci-device-driver)
3. [Install Proxmox Kernel Headers](#install-proxmox-kernel-headers)
4. [Install the PCI Device Driver](#install-the-pci-device-driver)
   - [How to Download the Driver](#how-to-download-the-driver)
   - [Device-Specific Installation](#device-specific-installation)
5. [Verify the PCI Device Driver Installation](#verify-the-pci-device-driver-installation)
6. [Expose the Device to the LXC](#expose-the-device-to-the-lxc)
7. [Verify the Device Inside the LXC](#verify-the-device-inside-the-lxc)

## Verify Proxmox Detects the PCI Device

> [!NOTE]
> A PCI or PCIe device identifies itself using several numeric IDs:
> - **Class ID** identifies the general type of device. For example, `0b40` represents a co-processor.
> - **Vendor ID** identifies the hardware manufacturer. For example, `1e60` identifies Hailo Technologies.
> - **Device ID** identifies the specific device made by that vendor. For example, `2864` identifies the Hailo-8 AI Processor.

1. Access the **Proxmox** host shell through the web interface or connect through SSH (Secure Shell) from **PowerShell**.

```powershell
ssh root@<PROXMOX_IP>
```

2. Update the local PCI ID database if the device is detected but does not display a readable hardware name. The database translates numeric PCI identifiers into readable vendor and device names.
   - `update-pciids` downloads the latest PCI vendor and device ID database from the pciutils project's PCI ID Database and updates the local PCI ID database.

```shell
update-pciids
```

For example, an outdated database may display the Hailo-8 as:

```text
06:00.0 Co-processor [0b40]: Device [1e60:2864] (rev 01)
```

3. Use `lspci` to list PCI and PCIe devices detected by the Linux kernel. It uses the local PCI ID database to display readable device information.
   - `-nn` displays both readable device names and their numeric PCI IDs. The IDs are useful for identifying hardware precisely, configuring passthrough, and matching devices to drivers.
   - Filter the output with `grep` by passing the output of `lspci` through the pipe operator `|`. `-i` makes the `grep` search case-insensitive, so the command does not need to match the exact capitalization of the text.

```shell
lspci -nn
```

```shell
lspci -nn | grep -i "co-processor"
```

The Hailo-8 should now appear with its readable vendor and device name:

```text
06:00.0 Co-processor [0b40]: Hailo Technologies Ltd. Hailo-8 AI Processor [1e60:2864] (rev 01)
```

4. Make note of the PCI address, also called the Bus:Device.Function (BDF) address. It identifies the device's location within the PCI/PCIe hierarchy and is commonly used when configuring passthrough. In this example, the address is `06:00.0`. The address may vary depending on the hardware installed and how the system assigns PCI resources.

> [!NOTE]
> The PCI address can change after hardware changes or reboots.
> - **Bus** identifies a PCI/PCIe pathway that can contain multiple devices. For example, devices `03:00.0`, `03:01.0`, and `03:02.0` are all located on bus `03`.
> - **Device** identifies a specific device on that bus. For example, `03:01.0` identifies device `01` on bus `03`.
> - **Function** identifies a logical function exposed by a device. A single physical device can expose multiple functions. For example, a GPU may appear as `03:03.0` for graphics and `03:03.1` for its HDMI/DisplayPort audio controller.

## Check for the PCI Device Driver

Because an LXC shares the Proxmox host's Linux kernel, the host must have a compatible kernel driver for the PCI or PCIe device.

Some drivers are included with Linux and load automatically. Others must be installed separately using a vendor package, DKMS package, installer, or source code.

1. Check which kernel driver, if any, is associated with the PCI device.
   - `-k` displays the kernel driver currently in use and any kernel modules that can handle the device.
   - `-s` limits the output to the specified PCI address.

```bash
lspci -k -s <PCI_ADDRESS>
```

If the driver is already active, the output may include `Kernel driver in use` and `Kernel modules`. `Kernel driver in use` confirms that the driver is currently active. If only the device information is returned, Proxmox detects the PCI device, but no kernel driver is currently bound to it.

```text
06:00.0 Co-processor: Hailo Technologies Ltd. Hailo-8 AI Processor (rev 01)
        Subsystem: Hailo Technologies Ltd. Hailo-8 AI Processor
        Kernel driver in use: <DRIVER_NAME>
        Kernel modules: <MODULE_NAME>
```

## Install Proxmox Kernel Headers

Some PCI device drivers compile a kernel module during installation. These drivers require kernel headers that match the currently running Proxmox kernel.

Installing the matching kernel headers ahead of time prepares the host for drivers that need to compile against the kernel. Drivers that do not require kernel headers are unaffected by having them installed.

1. Search for the kernel header package that matches the currently running Proxmox kernel.
   - `apt-cache search` searches the available package metadata without installing anything.
   - `$(uname -r)` runs `uname -r` and substitutes the currently running kernel version into the command.
   - `|` passes the output from the command on its left directly to the command on its right.
   - `grep -i header` filters the results for packages containing the word `header`. `-i` makes the search case-insensitive.

```bash
apt-cache search "$(uname -r)" | grep -i header
```

2. Install the matching Proxmox kernel header package.
   - `apt` (Advanced Package Tool) is Debian's package manager, used to install, update, and remove software packages.
   - `install -y` installs the specified software package and any required dependencies from the configured Debian and Proxmox repositories. `-y` is an APT command-line option that automatically answers yes to confirmation prompts.

```bash
apt install -y <PROXMOX_HEADER_PACKAGE>
```

3. Verify that the kernel build directory now exists.
   - `ls -l` lists files and directories. `-l` displays detailed information such as permissions, ownership, size, and the target of symbolic links.
   - `/lib/modules/<KERNEL_VERSION>/build` normally points to the installed kernel headers used to compile kernel modules.
   - `$(uname -r)` substitutes the currently running kernel version into the path.

```bash
ls -l /lib/modules/$(uname -r)/build
```

## Install the PCI Device Driver

If no kernel driver is currently bound to the PCI device, install the driver using the hardware vendor's supported method.

The installation method depends on how the driver is distributed:

- **Built into Linux** — the driver may already be installed but not loaded. The vendor documentation may instruct you to load a kernel module manually.
- **Package manager** — the driver may be available as a Debian/Ubuntu package that can be installed with `apt`.
- **Vendor package or installer** — some manufacturers provide a `.deb` package or installation script.
- **DKMS package** — some drivers use DKMS so the kernel module can be rebuilt automatically after kernel updates.
- **Source code** — some drivers must be compiled manually against the currently running kernel.

Follow the hardware vendor's Linux installation documentation and use the method they recommend for the specific device.

### How to Download the Driver

There are two common ways to download a driver package.

1. If the file is publicly available, use `wget` to download it directly to the Proxmox host.
   - `wget` downloads files from a URL.
   - `-P` specifies the directory where the downloaded file will be saved.

```bash
wget -P /tmp "<PCIE_DRIVER_URL>"
```

2. If the download requires authentication, download the file locally through a web browser, then copy it to the Proxmox host using `scp`.
   - `scp` (Secure Copy Protocol) securely copies files between computers over an SSH connection.
   - `"$HOME\Downloads\<FILENAME>"` is the path to the downloaded file on the local computer.
   - `root@<PROXMOX_IP>` connects to the Proxmox host as the `root` user.
   - `/tmp` specifies the destination directory on the Proxmox host.

```powershell
scp "$HOME\Downloads\<FILENAME>" root@<PROXMOX_IP>:/tmp/
```

### Device-Specific Installation

These guides contain only the driver installation and any device-specific instructions. Return to this guide afterward to complete the LXC passthrough process.

- [Hailo-8 PCI Passthrough to LXC](./hailo-8-pci-passthrough-lxc.md)

## Verify the PCI Device Driver Installation

1. Check whether the driver is now bound to the PCI device. The output should now include `Kernel driver in use` and `Kernel modules`.

```bash
lspci -k -s <PCI_ADDRESS>
```

## Expose the Device to the LXC

After the device driver is installed and bound on the Proxmox host, determine how the driver exposes the device to user space.

Some kernel drivers create a device node inside `/dev`, which contains special device files that applications use to communicate with hardware through kernel drivers.

If the driver creates a device node, continue with the steps below. If no device node is created, follow the hardware vendor's documentation to determine how the device is exposed to user space.

> [!NOTE]
> Full path examples:
>
> - Hailo-8: `/dev/hailo0`

1. Check `/dev` for the device node created by the kernel driver and make note of its full path.

```bash
ls -l /dev
```

2. Check the LXC configuration for existing exposed devices. If no `dev<N>` entries are present, use `dev0`. If one or more are already present, use the next available sequential number. For example, an existing exposed device may appear as `dev0: /dev/hailo0`.
   - `pct` is Proxmox's command-line tool for managing Linux Containers.
   - `config` displays the LXC configuration.

```bash
pct config <CTID>
```

3. Expose the device node to the LXC.
   - `set` modifies the configuration of an existing LXC.
   - `<CTID>` is the container ID.
   - `-dev<N>` assigns the device to an available device entry such as `dev0`, `dev1`, or `dev2`.
   - `/dev/<DEVICE>` is the full device node path identified in step 1.

```bash
pct set <CTID> -dev<N> /dev/<DEVICE>
```

4. Verify that the device was added to the LXC configuration.

```bash
pct config <CTID>
```

5. Reboot the LXC to apply the device configuration.
   - `pct reboot` reboots the container and applies pending configuration changes.

```bash
pct reboot <CTID>
```

## Verify the Device Inside the LXC

1. Access the LXC shell.
   - `pct enter` opens an interactive shell inside the specified LXC.

```bash
pct enter <CTID>
```

2. Verify the exposed device node is available inside the LXC.

```bash
ls -l /dev/<DEVICE>
```

## Related Documentation

## Official Documentation

- [Proxmox VE Administration Guide](https://pve.proxmox.com/pve-docs/pve-admin-guide.pdf)
- [Proxmox `pct` Manual](https://pve.proxmox.com/pve-docs/pct.1.html)