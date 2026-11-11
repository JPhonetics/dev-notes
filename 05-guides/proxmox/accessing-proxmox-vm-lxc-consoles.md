---
Created: 2026-11-10
Modified: 
---

# Accessing Proxmox VM and LXC Consoles

Proxmox Virtual Environment (Proxmox VE) provides console access to virtual machines (VMs) and Linux containers (LXCs) through its web interface. This allows administrators to interact directly with guest environments without requiring an SSH connection.

Console access is especially useful when SSH is unavailable, networking is misconfigured, or a guest requires troubleshooting before remote access can be established.

This guide covers accessing VM and LXC consoles through the Proxmox web interface, opening an LXC shell directly from the Proxmox host, and understanding SSH access restrictions within guest environments.

> [!NOTE]
> Proxmox VE normally enables SSH on the host by default. However, SSH availability inside VMs and LXCs depends on the guest operating system and its configuration.
>
> The console methods in this guide do not require an SSH server to be installed or running inside the guest.

## Overview

1. [Access Through Proxmox Web Interface](#access-through-proxmox-web-interface)
2. [Access LXC from Proxmox Host](#access-lxc-from-proxmox-host)
3. [SSH Access to VMs and LXCs](#ssh-access-to-vms-and-lxcs)
4. [Related Documentation](#related-documentation)
5. [Official Documentation](#official-documentation)

## Access Through Proxmox Web Interface

Proxmox console access is not an SSH session. It uses Proxmox's virtualization and container management capabilities and does not require an SSH server inside the guest.

Console availability and behavior depend on the guest type and configuration.

1. Log in to the **Proxmox web interface**.

2. Select the desired **VM** or **LXC** from the resource tree.

3. Select **Console** to open the guest's console.

4. Authenticate inside the guest operating system, if prompted.

5. Run commands directly within the guest environment.

[🔼 Back to Overview](#overview)

## Access LXC from Proxmox Host

Proxmox provides the `pct` command-line utility for managing Linux containers. An authorized administrator can use it to open a shell directly inside a running LXC from the Proxmox host without requiring SSH.

If the container is stopped, start it through the Proxmox web interface before continuing.

1. Open the **Proxmox web interface**.

2. Select the Proxmox node hosting the container.

3. Select **Shell** to open a terminal on the Proxmox host.

4. Enter the container using its container ID (CTID).

    - `pct enter <CTID>` opens a root shell inside the specified Linux container without requiring SSH.
        - `pct` is Proxmox's command-line utility for managing Linux containers.
        - `enter` opens a root shell inside the container.
        - `<CTID>` identifies the numeric container ID assigned by Proxmox.

    ```bash
    pct enter <CTID>
    ```

    > [!NOTE]
    > If accessing the Proxmox host through a non-root account with sudo privileges, use `sudo pct enter <CTID>` instead.

5. Run the required commands inside the container.

6. Exit the container shell when finished.

    - `exit` closes the container shell and returns to the Proxmox host.

    ```bash
    exit
    ```

> [!IMPORTANT]
> Commands executed through `pct enter` run inside the container, not on the Proxmox host. Confirm you are accessing the correct container before making changes.

[🔼 Back to Overview](#overview)

## SSH Access to VMs and LXCs

VMs and LXCs manage their own user accounts and SSH authentication settings. SSH availability and permitted login methods depend on the guest operating system, installed software, and configuration.

On Debian and Ubuntu-based systems, OpenSSH normally restricts password authentication for the `root` account through the `PermitRootLogin prohibit-password` setting. This allows root login using authorized SSH keys but prevents password authentication. Depending on the guest's configuration, root SSH access may also be disabled entirely.

This restriction applies to SSH authentication, not local console access. You can still use the Proxmox console to administer the guest with an authorized account.

For routine remote administration, consider creating a separate Linux user and granting sudo privileges when required.

- [Create a Linux User](../linux/create-linux-user.md) — Create a non-root account, configure optional sudo privileges, and verify user access.
- [Connecting to Remote Servers with SSH](../ssh/connecting-to-remote-servers.md) — Connect remotely, configure authentication, and troubleshoot SSH access.

> [!NOTE]
> Creating a Linux user does not automatically enable SSH. The guest must have an SSH server installed, running, and configured to permit the account to authenticate.

[🔼 Back to Overview](#overview)

## Related Documentation

- [Create a Linux User](../linux/create-linux-user.md) — Create a non-root Linux account, configure optional sudo privileges, and verify account access.
- [Connecting to Remote Servers with SSH](../ssh/connecting-to-remote-servers.md) — Connect to remote servers using SSH from Windows, Linux, and macOS.

## Official Documentation

- [Proxmox VE Documentation](https://pve.proxmox.com/pve-docs/) — Official documentation for Proxmox VE, including virtual machines, Linux containers, console access, and management tools.

---

[🔼 Back to Guides](../README.md)