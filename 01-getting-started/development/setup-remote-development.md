---
Created: 2026-11-11
Modified:
---

# Set Up a Remote Development Environment

A remote development environment allows developers to write, run, test, and manage software projects on a centralized computer or server while accessing them from another device. Instead of maintaining separate development environments on multiple computers, development tools, dependencies, and project files remain on the remote system.

My primary development machine is a Windows desktop. However, I wanted the flexibility to work from my Windows laptop without being tied to my desk. One alternative is to maintain separate local development environments and continuously push and pull changes through GitHub when switching devices. While this works, it can become inconvenient when working with unfinished changes or files that are not yet ready to commit.

A remote development environment solves this problem by keeping everything in one place. Whether I'm working from my desktop or laptop, I can access the same project files, development tools, and configurations without synchronizing separate working copies.

The remote system does not need to be a dedicated server or run continuously. Any compatible computer can serve as a remote development environment, whether it's a physical machine (bare metal) or a virtual machine (VM). It only needs to be powered on and accessible when development work is being performed.

Visual Studio Code (VS Code) supports remote development through its Remote - SSH extension. VS Code runs locally on the developer's computer, while terminals, language runtimes, development tools, and project files operate on the remote Linux environment.

This guide creates an Ubuntu Server VM using Proxmox Virtual Environment (Proxmox VE), configures remote access through SSH (Secure Shell), connects VS Code to the development environment, and verifies that projects can be accessed from multiple computers.

> [!IMPORTANT]
> A centralized development environment still requires backups. GitHub can preserve committed and pushed source code, but does not automatically back up uncommitted changes, databases, Docker volumes, or local configuration files.
>
> Configure a suitable backup strategy for the VM and its project data.

## Overview

1. [Create a Development Server](#create-a-development-server)
2. [Install Ubuntu Server](#install-ubuntu-server)
3. [Configure Remote Access](#configure-remote-access)
4. [Prepare the Development Environment](#prepare-the-development-environment)
5. [Connect VS Code](#connect-vs-code)
6. [Verify the Development Environment](#verify-the-development-environment)
7. [Clone a Git Repository](#clone-a-git-repository)
8. [Related Documentation](#related-documentation)
9. [Official Documentation](#official-documentation)

## Create a Development Server

For a bare-metal installation, you can install a Linux distribution directly onto a physical computer, such as Ubuntu Server, Debian, or Fedora. The computer does not need to be dedicated exclusively to development or remain powered on continuously. It only needs to be running and accessible when you want to connect remotely.

Alternatively, virtualization allows you to create a development server without dedicating an entire physical computer to it. A hypervisor is software that creates and manages virtual machines, allowing multiple operating systems to run independently on the same physical hardware.

Virtualization also allows you to create multiple independent development servers instead of hosting every project and its dependencies on a single system. Each VM can have its own operating system, development tools, configurations, and allocated resources. This makes it possible to separate projects, experiment with different environments, or rebuild one development server without affecting the others.

Several virtualization platforms are available, including:

- **Proxmox Virtual Environment (Proxmox VE):** An open-source virtualization platform for managing VMs and Linux containers.
- **Microsoft Hyper-V:** A hypervisor available on supported Windows editions and Windows Server.
- **VMware ESXi:** A bare-metal hypervisor designed for running and managing virtual machines.
- **VMware Workstation Pro:** A desktop virtualization application available for Windows and Linux.
- **Oracle VirtualBox:** A desktop virtualization application that supports multiple operating systems.
- **KVM (Kernel-based Virtual Machine):** A virtualization technology built into Linux that can be managed using tools such as QEMU and libvirt.

The underlying concept remains the same regardless of the platform: create a machine, install a supported operating system, configure networking, and enable remote access.

For this guide, I will be creating an Ubuntu Server VM using Proxmox VE because I already have a Proxmox environment available. The steps for creating the VM may differ between virtualization platforms, but the remaining configuration and remote development workflow should be similar.

### Set Up the VM on Proxmox

1. Download the Ubuntu Server LTS installation image from the [official Ubuntu website](https://ubuntu.com/download/server).

2. Log in to the **Proxmox web interface**.

3. Expand the **Proxmox node** that will host the development environment.

4. Select the storage that supports **ISO Images**.

5. Navigate to **ISO Images** and upload the Ubuntu Server ISO file.

6. Select **Create VM** and configure the development server using the following recommended settings. Leave unspecified options at their defaults.

    | Setting | Value |
    |---|---|
    | OS | Ubuntu Server LTS |
    | Machine / BIOS | q35 / OVMF (UEFI) |
    | EFI Disk | Enabled, RAW |
    | SCSI Controller | VirtIO SCSI single |
    | QEMU Agent | Enabled |
    | Disk | SCSI (0), 100 GiB, RAW |
    | Cache | Default (no cache) |
    | Discard / IO Thread | Enabled / Enabled |
    | CPU | 1 socket, 4 cores, host |
    | Memory | 8192 MiB (8 GiB) |
    | Network | VirtIO, appropriate bridge and VLAN ID |

  > [!NOTE]
  > These are suggested starting resources. Increase the CPU, RAM, or storage as development workloads grow. The `host` CPU type is suitable for a VM that remains on the same physical host, but can limit migration compatibility with hosts using different CPUs.

7. Review the configuration and click **Finish**.

[🔼 Back to Overview](#overview)

## Install Ubuntu Server

This guide uses Ubuntu Server as the development server's operating system. If you are using a different Linux distribution or installing directly onto physical hardware, follow the appropriate installation instructions for your system. Once the operating system is installed, the remaining remote access and development configuration steps will follow a similar process.

1. Start the development server and access its installation interface. For a Proxmox VM, open its **Console** through the web interface. For bare-metal installations, use the computer's connected display and keyboard.

2. Follow the Ubuntu Server installation prompts.

3. Configure the operating system using the following settings:

    - **Installation Type:** Ubuntu Server (standard).
    - **Network:** Configure a static IP or use DHCP.
    - **Storage:** Select the installation disk, use the entire disk, and accept the automatically generated partition layout.
    - **Hostname:** `<HOSTNAME>`.
    - **Username:** Create a non-root Linux account.
    - **Password:** Create a strong account password.
    - **OpenSSH Server:** Select the option to install the OpenSSH server.

4. Complete the installation and restart the development server when prompted.

5. Log in to Ubuntu using the Linux account created during installation or connect via SSH.

> [!NOTE]
> For detailed SSH connection instructions, authentication options, and troubleshooting, see [Connecting to Remote Servers with SSH](../remote-access/ssh.md).

6. Update the installed software packages.

    - `sudo apt update` refreshes the available package information without installing updates.
        - `sudo` (superuser do) executes the command with administrative privileges.
        - `apt` is Ubuntu's package management tool.
        - `update` refreshes package information from the configured repositories.
    - `sudo apt upgrade -y` installs available package upgrades.
        - `upgrade` installs available package updates.
        - `-y` automatically answers yes to confirmation prompts.

    ```bash
    sudo apt update
    sudo apt upgrade -y
    ```

> [!IMPORTANT]
> The QEMU Guest Agent is intended for compatible virtualized environments and is not required for bare-metal installations. Skip this step if the development server is running on physical hardware.
>
> Some Ubuntu versions use a static systemd unit for the agent, so manually enabling the service may not be supported.

7. Install the QEMU Guest Agent if the development server is running as a Proxmox VM. The QEMU Guest Agent allows Proxmox to communicate with the guest operating system for certain management operations, including retrieving guest information and coordinating shutdown operations.

    - `sudo apt install qemu-guest-agent -y` installs the QEMU Guest Agent.
        - `install` installs the specified software package.
        - `qemu-guest-agent` identifies the package to install.

    ```bash
    sudo apt install qemu-guest-agent -y
    ```

    - `sudo systemctl status qemu-guest-agent --no-pager` displays the current status of the guest agent service.
        - `systemctl` manages services controlled by systemd.
        - `status` displays the current status of the specified service.
        - `--no-pager` displays the output directly in the terminal.

    ```bash
    sudo systemctl status qemu-guest-agent --no-pager
    ```

[🔼 Back to Overview](#overview)

## Configure Remote Access

SSH provides encrypted remote access to the development server. This allows VS Code and command-line tools to connect without requiring direct access to the server's local console.

> [!IMPORTANT]
> Use an authorized non-root Linux account for remote development. The development VM should be accessible only from trusted networks or through an authenticated VPN. Do not expose its SSH port directly to the public internet.

1. Verify that the SSH server is available on the development server. The service should report `active (running)` when SSH is running.

    - `sudo systemctl status ssh --no-pager` displays the current status of the SSH service.
        - `ssh` identifies the SSH server service.
        - `--no-pager` displays the output directly in the terminal.

    ```bash
    sudo systemctl status ssh --no-pager
    ```

> [!NOTE]
> Some Ubuntu installations use systemd socket activation, allowing SSH to accept connections without keeping the service continuously running. If the service is inactive, check `sudo systemctl status ssh.socket --no-pager` for `active (listening)`.

2. Identify the VM's IP address. This is the IP address you use to connect to it.

    - `hostname -I` displays the IP addresses assigned to the system.
        - `hostname` displays or manages the system hostname.
        - `-I` displays assigned network addresses.

    ```bash
    hostname -I
    ```

3. From your desktop or laptop, open **PowerShell** or another terminal and connect to the development server.

    - `ssh <USERNAME>@<IP_ADDRESS>` establishes an SSH connection using the specified account and server address.
        - `ssh` starts the SSH client.
        - `<USERNAME>` identifies the Linux account authorized to access the remote server.
        - `<IP_ADDRESS>` identifies the remote development server's IP address.

    ```powershell
    ssh <USERNAME>@<IP_ADDRESS>
    ```

4. Verify the SSH host-key fingerprint against a trusted source before accepting the connection, then authenticate using the configured method.

> [!NOTE]
> If you need to create another Linux account, see [Create a Linux User](../../05-guides/linux/create-linux-user.md). Ubuntu Server already allows you to create an account during installation, so an additional account is not required if the existing account meets your needs.
>
> For detailed SSH connection instructions, authentication methods, and troubleshooting, see [Connecting to Remote Servers with SSH](../remote-access/ssh.md).

[🔼 Back to Overview](#overview)

## Prepare the Development Environment

Development tools and project dependencies should be installed inside the remote VM rather than separately on every computer used for development.

### Install Development Tools

1. Install the required development tools inside the Ubuntu VM.

    - [Git](../git/install-git.md)
    - [GitHub CLI](../github/install-github-cli.md)
    - [Python](../python/install-python.md)
    - [JavaScript](../javascript/install-javascript.md)
    - [Docker](../docker/install-docker.md)

2. Optional tools.

    - [Claude Code](../ai/install-claude.md)
    - [Codex CLI](../ai/install-codex.md)

### Create a Projects Directory

Keeping development repositories in a dedicated directory makes them easier to organize, locate, and manage.

1. Create a directory for development projects.

    - `mkdir -p ~/projects` creates the `projects` directory inside the current user's home directory.
        - `mkdir` creates directories.
        - `-p` creates missing parent directories and avoids an error if the directory already exists.
        - `~` represents the current user's home directory.

    ```bash
    mkdir -p ~/projects
    ```

[🔼 Back to Overview](#overview)

## Connect VS Code

The Remote - SSH extension by Microsoft (`ms-vscode-remote.remote-ssh`) allows VS Code to connect to the development VM and work directly with remote projects. VS Code remains installed locally while its remote components execute on Ubuntu.

Install the extension if it is not already installed.

> [!NOTE]
> Some VS Code extensions run locally, while language servers, debuggers, and development tools may run on the remote host. Install extensions in the remote environment when VS Code indicates that they are required there.
>
> You do not need to install the graphical VS Code application inside the Ubuntu Server VM.

1. Open **VS Code** on your desktop or laptop.

2. Open the remote connection options using either method:

    - Open the **Command Palette** using `Ctrl+Shift+P`.
    - Click the **Remote Window** icon (`><`) in the bottom-left corner of VS Code.

3. Select **`+` Add New SSH Host...** to save the connection.

4. Enter the SSH connection command.

    ```text
    ssh <USERNAME>@<IP_ADDRESS>
    ```

5. Select the SSH configuration file to update.

    ```text
    C:\Users\<WINDOWS_USERNAME>\.ssh\config
    ```

6. Open **Remote-SSH: Connect to Host...** again and select the development server from the saved connections.

7. Select **Linux** if prompted to identify the remote operating system.

8. Authenticate using the configured SSH method.

9. Allow VS Code to install and initialize its remote server components. Once connected, the bottom-left corner should display the remote SSH connection (`>< SSH: <IP_ADDRESS>`).

10. Select **File > Open Folder** and open the remote projects directory.

    ```text
    /home/<USERNAME>/projects
    ```

11. Open **Terminal > New Terminal**. The integrated terminal should now run inside the Ubuntu development VM rather than on the local computer.

    ```text
    <USERNAME>@<HOSTNAME>:~/projects$
    ```

[🔼 Back to Overview](#overview)

## Verify the Development Environment


1. Open **Terminal > New Terminal** and check the current username. The output should display the Linux account used to connect.

    - `whoami` displays the username associated with the current session.

    ```bash
    whoami
    ```

2. Confirm the terminal is operating inside the remote filesystem.

    - `pwd` displays the full path of the current working directory.

    ```bash
    pwd
    ```

    If you opened the projects directory in VS Code, the output should resemble:

    ```text
    /home/<USERNAME>/projects
    ```

[🔼 Back to Overview](#overview)

## Clone a Git Repository

Cloning a repository creates a local copy of an existing Git repository on the development server. The repository and its working files remain on the remote system, allowing you to access the same project from multiple computers.

1. Navigate to the projects directory.

    - `cd ~/projects` changes the current working directory to the projects directory.
        - `cd` changes the shell's current directory.

    ```bash
    cd ~/projects
    ```

2. Clone an existing repository into the projects directory. The repository is typically downloaded into a directory named after the repository.

    - `git clone <REPO_URL>` downloads the specified repository and its version history into a local directory.
        - `git` is the distributed version control system.
        - `clone` creates a local copy of an existing repository.
        - `<REPO_URL>` identifies the remote repository's HTTPS or SSH URL.

    ```bash
    git clone <REPO_URL>
    ```

[🔼 Back to Overview](#overview)

## Related Documentation

- [Set Up a Local Development Environment](./setup-local-development.md) — Configure a local development environment on a personal computer.
- [Connecting to Remote Servers with SSH](../remote-access/ssh.md) — Connect to remote Linux systems and troubleshoot SSH authentication.
- [Create a Linux User](../../05-guides/linux/create-linux-user.md) — Create a non-root Linux account and configure optional sudo privileges.
- [Accessing Proxmox VM and LXC Consoles](../../05-guides/proxmox/accessing-proxmox-vm-lxc-consoles.md) — Access guest systems directly through the Proxmox web interface.

## Official Documentation

- [Proxmox VE Documentation](https://pve.proxmox.com/pve-docs/) — Official documentation for creating and managing virtual machines and Linux containers.
- [Ubuntu Server Documentation](https://ubuntu.com/server/docs/) — Official instructions for installing, configuring, and maintaining Ubuntu Server.
- [VS Code Remote - SSH](https://code.visualstudio.com/docs/remote/ssh) — Official documentation for developing on remote systems through SSH.
- [VS Code Remote Development](https://code.visualstudio.com/docs/remote/remote-overview) — Overview of remote development extensions, environments, and workflows.
- [Docker Engine Installation](https://docs.docker.com/engine/install/ubuntu/) — Official Ubuntu installation instructions for Docker Engine.

---

[🔼 Back to Getting Started](../README.md)