---
Created: 2026-10-09
Modified: 2026-11-11
---

# Proxmox - PCI Passthrough to LXC

This guide documents how to expose a PCI or PCIe device to a Linux Container (LXC) in Proxmox when the device's Linux driver provides a compatible device interface.

Unlike a virtual machine, an LXC shares the Proxmox host's Linux kernel. The hardware's kernel driver must therefore be installed and loaded on the Proxmox host. When the driver exposes a device node under `/dev`, that node can potentially be made accessible to the LXC. The container installs any required user-space runtime, libraries, or applications.

Not every PCI or PCIe device supports this method. Device compatibility and access requirements depend on the hardware, kernel driver, and software using the device.

See [Device-Specific Installation](#device-specific-installation) for device-specific driver installation instructions. These guides contain only the steps unique to that device. Return to this guide afterward to complete the LXC passthrough process.

> [!IMPORTANT]
> This guide uses `sudo` for commands requiring administrative privileges on the Proxmox host. It assumes you are connected using a Linux account with sudo access. Proxmox commonly uses the root account for host administration, and a separate sudo-enabled account may require additional configuration.
>
> If you are already using a root shell, omit `sudo` from the commands.

## Overview

1. [Verify Proxmox Detects the PCI Device](#verify-proxmox-detects-the-pci-device)
2. [Check for the PCI Device Driver](#check-for-the-pci-device-driver)
3. [Install Proxmox Kernel Headers](#install-proxmox-kernel-headers)
4. [Install the PCI Device Driver](#install-the-pci-device-driver)
    - [Device-Specific Installation](#device-specific-installation)
5. [Verify the PCI Device Driver Installation](#verify-the-pci-device-driver-installation)
6. [Expose the Device to the LXC](#expose-the-device-to-the-lxc)
7. [Verify the Device Inside the LXC](#verify-the-device-inside-the-lxc)
8. [Related Documentation](#related-documentation)
9. [Official Documentation](#official-documentation)

## Verify Proxmox Detects the PCI Device

> [!NOTE]
> A PCI or PCIe device identifies itself using several numeric IDs:
> - **Class ID** identifies the general type of device. For example, `0b40` represents a co-processor.
> - **Vendor ID** identifies the hardware manufacturer. For example, `1e60` identifies Hailo Technologies.
> - **Device ID** identifies the specific device made by that vendor. For example, `2864` identifies the Hailo-8 AI Processor.

1. Access the **Proxmox** host shell through the web interface or connect through SSH (Secure Shell) from **PowerShell**.

    ```powershell
    ssh <USERNAME>@<IP_ADDRESS>
    ```

    > [!NOTE]
    > For instructions on connecting to remote servers from Windows, Linux, or macOS, including SSH authentication, custom ports, and file transfer, see [Connecting to Remote Servers with SSH](../../01-getting-started/ssh/connecting-to-remote-servers.md).

2. Update the local PCI ID database before identifying the hardware. This ensures `lspci` uses the latest available PCI vendor and device names instead of potentially outdated information.

    - `sudo update-pciids` downloads and installs the latest PCI vendor and device identification database.
        - `sudo` (superuser do) executes the command with administrative privileges.
        - `update-pciids` retrieves the latest PCI ID database from the pciutils project's PCI ID Database.

    ```bash
    sudo update-pciids
    ```

    For example, an outdated database may display the Hailo-8 as:

    ```text
    06:00.0 Co-processor [0b40]: Device [1e60:2864] (rev 01)
    ```

3. Use `lspci` to list PCI and PCIe devices detected by the Linux kernel.

    - `lspci -nn` lists detected PCI and PCIe devices with readable names and numeric identifiers.
        - `lspci` displays information about PCI and PCIe devices.
        - `-nn` displays both readable device names and their numeric PCI IDs.

    ```bash
    lspci -nn
    ```

    For example, you can filter the results by device type, vendor name, or device name to locate specific hardware.

    - `lspci -nn | grep -i "co-processor"` filters the results to display devices whose information contains `co-processor`.
        - `|` (pipe) passes the output of the first command to the next command.
        - `grep` searches text for a matching pattern.
        - `-i` makes the search case-insensitive.
        - `"co-processor"` is the search text, which can be replaced with a vendor or device name.

    ```bash
    lspci -nn | grep -i "co-processor"
    ```

    The Hailo-8 should now appear with its readable vendor and device name:

    ```text
    06:00.0 Co-processor [0b40]: Hailo Technologies Ltd. Hailo-8 AI Processor [1e60:2864] (rev 01)
    ```

4. Make note of the PCI address, also called the Bus:Device.Function (BDF) address. It identifies the device's location within the PCI/PCIe hierarchy and is commonly used when configuring passthrough. In this example, the address is `06:00.0`. The address may vary depending on the hardware installed and how the system assigns PCI resources.

    > [!NOTE]
    > The PCI address can change after hardware changes or reboots.
    > 
    > - **Bus** identifies a PCI/PCIe pathway that can contain multiple devices. For example, devices `03:00.0`, `03:01.0`, and `03:02.0` are all located on bus `03`.
    > - **Device** identifies a specific device on that bus. For example, `03:01.0` identifies device `01` on bus `03`.
    > - **Function** identifies a logical function exposed by a device. A single physical device can expose multiple functions. For example, a GPU may appear as `03:03.0` for graphics and `03:03.1` for its HDMI/DisplayPort audio controller.

[🔼 Back to Overview](#overview)

## Check for the PCI Device Driver

Because an LXC shares the Proxmox host's Linux kernel, the host must have a compatible kernel driver for the PCI or PCIe device.

Some drivers are included with Linux and load automatically. Others must be installed separately using a vendor package, DKMS package, installer, or source code.

1. Check which kernel driver, if any, is associated with the PCI device.

    - `lspci -k -s <PCI_ADDRESS>` displays the kernel driver information for the specified PCI device.
        - `-k` displays the kernel driver in use and available kernel modules.
        - `-s` limits the output to a specific PCI device.
        - `<PCI_ADDRESS>` is the Bus:Device.Function (BDF) address identified previously, such as `06:00.0`.

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

[🔼 Back to Overview](#overview)

## Install Proxmox Kernel Headers

Some PCI device drivers compile a kernel module during installation. These drivers require kernel headers that match the currently running Proxmox kernel.

Installing the matching kernel headers ahead of time prepares the host for drivers that need to compile against the kernel. Drivers that do not require kernel headers are unaffected by having them installed.

1. Search for the kernel header package that matches the currently running Proxmox kernel.

    - `apt-cache search "$(uname -r)" | grep -i header` searches the available package metadata for kernel-header packages matching the running kernel.
        - `apt-cache search` searches package metadata without installing packages.
        - `$(uname -r)` executes `uname -r` and substitutes the running kernel version into the command.
        - `header` is the search term used to filter the results.

    ```bash
    apt-cache search "$(uname -r)" | grep -i header
    ```

2. Install the matching Proxmox kernel header package.

    - `sudo apt install -y <PROXMOX_HEADER_PACKAGE>` installs the matching Proxmox kernel headers and any required dependencies.
        - `apt` (Advanced Package Tool) is Debian's package manager.
        - `install` installs the specified package and its dependencies.
        - `-y` automatically answers yes to confirmation prompts.
        - `<PROXMOX_HEADER_PACKAGE>` is the kernel-header package identified in step 1.

    ```bash
    sudo apt install -y <PROXMOX_HEADER_PACKAGE>
    ```

3. Verify that the kernel build directory now exists.

    - `ls -l /lib/modules/$(uname -r)/build` displays the kernel build directory and its file information.
        - `ls` lists files and directories.
        - `-l` displays detailed information, including permissions, ownership, and symbolic link targets.
        - `/lib/modules/$(uname -r)/build` identifies the build directory for the currently running kernel, which normally links to its installed headers.

    ```bash
    ls -l /lib/modules/$(uname -r)/build
    ```

[🔼 Back to Overview](#overview)

## Install the PCI Device Driver

> [!CAUTION]
> Before installing the latest driver, review the hardware vendor's documentation and the requirements of any applications that will use the device. Some applications support only specific driver versions.

For device-specific driver installation and configuration instructions, see [Device-Specific Installation](#device-specific-installation).

If no kernel driver is currently bound to the PCI device, determine whether a compatible driver is already installed or needs to be installed separately. A driver may be present but not loaded, or the device may require a vendor-specific driver.

Follow the hardware vendor's supported installation and configuration instructions before continuing.

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

    - `wget -P /tmp "<PCIE_DRIVER_URL>"` downloads the driver package to the Proxmox host's `/tmp` directory.
        - `wget` downloads files from URLs.
        - `-P /tmp` specifies the destination directory.
        - `<PCIE_DRIVER_URL>` is the direct download URL provided by the hardware manufacturer.

    ```bash
    wget -P /tmp "<PCIE_DRIVER_URL>"
    ```

2. If the download requires authentication, download the file locally through a web browser, then copy it to the Proxmox host using `scp` (Secure Copy).

    - `scp "$HOME\Downloads\<FILENAME>" <USERNAME>@<IP_ADDRESS>:/tmp/` securely copies the downloaded driver package from Windows to the Proxmox host over SSH.
        - `scp` transfers files securely between computers over SSH.
        - `"$HOME\Downloads\<FILENAME>"` identifies the downloaded file in the Windows Downloads directory.
            - `$HOME` represents the current user's home directory.
            - `Downloads` is the directory containing the downloaded file.
            - `<FILENAME>` is the actual filename, including its extension.
        - `<USERNAME>@<IP_ADDRESS>` identifies the remote account and Proxmox host.
        - `:/tmp/` specifies the destination directory on the Proxmox host.

    ```powershell
    scp "$HOME\Downloads\<FILENAME>" <USERNAME>@<IP_ADDRESS>:/tmp/
    ```

### Device-Specific Installation

These guides contain only the driver installation and any device-specific instructions. Return to this guide afterward to complete the LXC passthrough process.

- [Hailo-8 PCI Passthrough to LXC](./pci-passthrough-lxc-hailo-8.md)

[🔼 Back to Overview](#overview)

## Verify the PCI Device Driver Installation

1. Check whether the driver is now bound to the PCI device. The output should include `Kernel driver in use`, confirming that a kernel driver is active. The `Kernel modules` field may also appear, listing compatible kernel modules.

    ```bash
    lspci -k -s <PCI_ADDRESS>
    ```

[🔼 Back to Overview](#overview)

## Expose the Device to the LXC

After the device driver is installed and bound on the Proxmox host, determine how the driver exposes the device to user space. Exposing a device node to an LXC does not automatically grant every application permission to use it. Depending on the container's user permissions and the device driver, additional ownership or access configuration may be required.

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

    - `sudo pct config <CTID>` displays the configuration of the specified LXC.
        - `pct` is Proxmox's command-line tool for managing Linux Containers.
        - `config` displays the container configuration.
        - `<CTID>` is the numeric container ID assigned to the LXC.

    ```bash
    sudo pct config <CTID>
    ```

3. Expose the device node to the LXC.

    - `sudo pct set <CTID> -dev<N> /dev/<DEVICE>` adds the specified host device node to the container configuration.
        - `set` modifies the configuration of an existing LXC.
        - `-dev<N>` selects an available device entry such as `dev0`, `dev1`, or `dev2`.
        - `/dev/<DEVICE>` is the full device node path identified in step 1.

    ```bash
    sudo pct set <CTID> -dev<N> /dev/<DEVICE>
    ```

4. Verify that the device was added to the LXC configuration.

    ```bash
    sudo pct config <CTID>
    ```

5. Reboot the LXC to apply the device configuration.

    - `sudo pct reboot <CTID>` reboots the specified container.

    ```bash
    sudo pct reboot <CTID>
    ```

    > [!NOTE]
    > If the LXC is stopped, use `sudo pct start <CTID>` to start it.

[🔼 Back to Overview](#overview)

## Verify the Device Inside the LXC

1. Access the LXC shell from the Proxmox host.

    - `pct enter` opens an interactive shell inside the specified LXC.

    ```bash
    sudo pct enter <CTID>
    ```

2. Verify that the exposed device node is available inside the LXC.

    ```bash
    ls -l /dev/<DEVICE>
    ```

    The output should display the device node and its ownership and permissions.

3. Verify that the device is accessible using the hardware vendor's diagnostic utility or the application that will use it.

    Successfully listing the device node confirms that it is visible inside the container, but does not guarantee that the device is functioning or that the application has sufficient permissions.

[🔼 Back to Overview](#overview)

## Related Documentation

- [Connecting to Remote Servers with SSH](../../01-getting-started/remote-access/ssh.md) — Connect to remote servers using SSH from Windows, Linux, and macOS.
- [Create a Linux User](../linux/create-linux-user.md) — Create a non-root Linux account and configure sudo privileges for administrative access.

## Official Documentation

- [Proxmox VE Administration Guide](https://pve.proxmox.com/pve-docs/pve-admin-guide.pdf) — Official Proxmox documentation covering host administration, virtual machines, containers, and device management.
- [Proxmox `pct` Manual](https://pve.proxmox.com/pve-docs/pct.1.html) — Official reference for managing Linux containers, including configuration, device access, and container lifecycle commands.
- [pciutils Documentation](https://mj.ucw.cz/sw/pciutils/) — Official documentation for PCI device identification utilities, including `lspci` and `update-pciids`.

---

[🔼 Back to Guides](../README.md)