---
Created: 2026-11-10
Modified: 2026-11-13
---

# Connecting to Remote Servers with SSH

SSH (Secure Shell) provides an encrypted terminal connection to a remote machine. 

This guide covers connecting to remote servers from Windows, Linux, and macOS, including verifying SSH availability, transferring files securely, and troubleshooting common connection issues.

## Prerequisites

- A reachable server or VM with an SSH server installed and configured to accept incoming connections.
- A permitted user account and either a password or authorized SSH key.
- Network/firewall permission for the server's SSH port (usually TCP `22`).

> [!NOTE]
> Some Linux systems restrict SSH password authentication for the `root` account. On Debian and Ubuntu, OpenSSH normally prevents root password login while allowing authentication with authorized SSH keys.
>
> If you need a non-root account, see [Create a Linux User](../../05-guides/linux/create-linux-user.md) for instructions on creating an account and configuring optional sudo privileges.

## Overview

1. [Install OpenSSH Client](#install-openssh-client)
2. [Verify SSH on the Target Server](#verify-ssh-on-the-target-server)
3. [Connect from Windows](#connect-from-windows)
4. [Connect from Linux](#connect-from-linux)
5. [Connect from macOS](#connect-from-macos)
6. [File Transfers](#file-transfers)
7. [Troubleshooting](#troubleshooting)
8. [Related Documentation](#related-documentation)
9. [Official Documentation](#official-documentation)

## Install OpenSSH Client

OpenSSH Client provides the `ssh` command used to establish secure connections to remote servers. It is included with Windows 11 and macOS and is commonly preinstalled on Linux distributions.

In most cases, no additional installation is required. If the `ssh` command is unavailable, follow the instructions for your operating system.

### Windows

Windows 11 normally includes OpenSSH Client. If it is missing, install it through Windows Optional Features.

1. Open **Settings**.
2. Navigate to **System → Optional features**.
3. Select **View features** under **Add an optional feature**.
4. Search for and select **OpenSSH Client**.
5. Click **Next**, then **Add**.

### Linux

Most Linux distributions include an SSH client or provide one through their package manager. If the client is missing, install it using your distribution's package manager.

**Debian/Ubuntu:**

1. Open a terminal.

2. Update the package index and install OpenSSH Client.

    - `sudo apt update` refreshes the available package information without installing updates.
        - `sudo` (superuser do) executes the command with administrative privileges.
        - `apt` is Debian's package management tool.
        - `update` refreshes package information from the configured repositories.
    - `sudo apt install openssh-client` installs the OpenSSH client and its required dependencies.
        - `install` installs the specified package.
        - `openssh-client` identifies the package to install.

    ```bash
    sudo apt update
    sudo apt install openssh-client
    ```

Other Linux distributions may use different package managers.

### macOS

macOS includes the OpenSSH client by default, available through **Terminal**. No additional installation is required.

[🔼 Back to Overview](#overview)

## Verify SSH on the Target Server

Before connecting remotely, the target server must have an SSH server installed and configured to accept incoming connections on the designated network port.

The following commands are intended for Debian/Ubuntu servers and must be executed directly on the **target server**, not the computer initiating the SSH connection. If SSH is unavailable, use the server's local console or an authorized virtualization management console.

> [!NOTE]
> The SSH port must be accessible through the server's firewall and network. A listening port confirms that a service is accepting connections locally, but does not guarantee that remote computers can reach it.

### Check the SSH Service

1. Open a terminal on the target server.

2. Check whether the SSH server service is running.

    - `sudo systemctl status ssh --no-pager` displays the current status of the SSH server service.
        - `systemctl` manages and inspects services controlled by systemd.
        - `status` displays the current state of the specified service.
        - `ssh` identifies the SSH server service.
        - `--no-pager` displays the output directly without opening an interactive pager.

    ```bash
    sudo systemctl status ssh --no-pager
    ```

    The service should report `active (running)`. If the service is missing or inactive, the SSH server may need to be installed, enabled, or started before remote connections can succeed.

### Check the SSH Port

1. Check which TCP ports are listening on the target server.

    - `sudo ss -lntp` displays listening TCP sockets and their associated processes.
        - `ss` displays socket and network connection information.
        - `-l` shows listening sockets.
        - `-n` displays numerical addresses and port numbers.
        - `-t` limits the results to TCP sockets.
        - `-p` displays process information when available.

    ```bash
    sudo ss -lntp
    ```

    Locate the SSH service (`sshd`) in the output and identify its listening port under the `Local Address:Port` column. SSH typically uses TCP port `22`, but the server may be configured to use a different port.

    Depending on the server's configuration, the SSH listening socket may be managed by `systemd` instead of appearing directly under `sshd`.

[🔼 Back to Overview](#overview)

> [!IMPORTANT]
> On your first SSH connection, verify the server's host-key fingerprint against a trusted source before accepting it. This helps confirm that you're connecting to the intended server and protects against potential interception.

## Connect from Windows

Windows 11 includes OpenSSH Client, available from **PowerShell**, **Windows Terminal**, and **Command Prompt**.

1. Open **PowerShell** or **Windows Terminal**.

2. Connect to the remote server.

    - `ssh <USERNAME>@<IP_ADDRESS>` starts an encrypted SSH connection to the specified server.
        - `ssh` is the OpenSSH client used to connect to remote systems.
        - `<USERNAME>` identifies the remote login account.
        - `@` separates the username from the server address.
        - `<IP_ADDRESS>` is the remote host IP address or hostname.

    ```powershell
    ssh <USERNAME>@<IP_ADDRESS>
    ```

3. Verify the host-key fingerprint against a trusted source when prompted, then authenticate using your authorized method.

4. End the session using `exit` or `Ctrl+D`.

    - `exit` closes the current remote shell session and returns to the local terminal.
    
    ```shell
    exit
    ```

> [!NOTE] 
> To connect to a custom SSH port:
> 
> - `ssh -p <SSH_PORT> <USERNAME>@<IP_ADDRESS>` connects using a custom SSH port.
>     - `-p` specifies the port to use instead of the default TCP port `22`.
>     - `<SSH_PORT>` is the port number configured on the remote SSH server.
> 
> ```powershell
> ssh -p <SSH_PORT> <USERNAME>@<IP_ADDRESS>
> ```

[🔼 Back to Overview](#overview)

## Connect from Linux

Most Linux distributions include an OpenSSH client or provide one through their package manager. The connection syntax is the same as on Windows.

1. Open a terminal.

2. Connect to the remote server.

    ```bash
    ssh <USERNAME>@<IP_ADDRESS>
    ```

3. Verify the host-key fingerprint against a trusted source when prompted, then authenticate using your authorized method.

4. End the session using `exit` or `Ctrl+D`.

    ```shell
    exit
    ```

> [!NOTE]
> To connect using a custom SSH port:
>
> ```bash
> ssh -p <SSH_PORT> <USERNAME>@<IP_ADDRESS>
> ```

[🔼 Back to Overview](#overview)

## Connect from macOS

macOS includes the OpenSSH client by default, available through **Terminal**. No additional installation is required.

1. Open **Terminal**.

2. Connect to the remote server.

    ```bash
    ssh <USERNAME>@<IP_ADDRESS>
    ```

3. Verify the host-key fingerprint against a trusted source when prompted, then authenticate using your authorized method.

4. End the session using `exit` or `Ctrl+D`.

    ```bash
    exit
    ```

> [!NOTE]
> To connect using a custom SSH port:
>
> ```bash
> ssh -p <SSH_PORT> <USERNAME>@<IP_ADDRESS>
> ```

[🔼 Back to Overview](#overview)

## File Transfers

OpenSSH includes `scp` (Secure Copy), a command-line utility used to securely transfer files between local and remote systems over SSH. It supports copying files to a remote server and downloading files from a remote server to your local computer.

### Copy a File to the Server

1. Open a terminal on your local computer.

2. Copy a local file to the remote server.

    - `scp <LOCAL_FILE> <USERNAME>@<IP_ADDRESS>:<REMOTE_DIRECTORY>/` transfers a local file to the specified directory on the remote server.
        - `scp` securely copies files between systems using SSH.
        - `<LOCAL_FILE>` identifies the local file to transfer.
        - `<USERNAME>@<IP_ADDRESS>` identifies the remote user account and server.
        - `:` separates the remote server address from its file path.
        - `<REMOTE_DIRECTORY>/` identifies the destination directory on the remote server.

    ```bash
    scp <LOCAL_FILE> <USERNAME>@<IP_ADDRESS>:<REMOTE_DIRECTORY>/
    ```

    For example, copy `backup.sql` from the current local directory to `/home/admin/backups/` on a server at `172.22.21.20`:

    ```bash
    scp backup.sql admin@172.22.21.20:/home/admin/backups/
    ```

### Copy a File from the Server

1. Open a terminal on your local computer.

2. Copy a file from the remote server to your local computer.

    - `scp <USERNAME>@<IP_ADDRESS>:<REMOTE_FILE> <LOCAL_DIRECTORY>/` downloads a remote file to the specified local directory.
        - `<REMOTE_FILE>` identifies the file on the remote server.
        - `<LOCAL_DIRECTORY>/` identifies the destination directory on your local computer.

    ```bash
    scp <USERNAME>@<IP_ADDRESS>:<REMOTE_FILE> <LOCAL_DIRECTORY>/
    ```

    For example, download `backup.sql` from `/home/admin/backups/` on the remote server to your local Downloads folder.

    **Windows (PowerShell):**

    - `$HOME` identifies the current user's home directory in PowerShell.

    ```powershell
    scp admin@172.22.21.20:/home/admin/backups/backup.sql "$HOME\Downloads\"
    ```

    **Linux/macOS:**

    - `~/Downloads/` identifies the Downloads directory in the current user's home folder.

    ```bash
    scp admin@172.22.21.20:/home/admin/backups/backup.sql ~/Downloads/
    ```

> [!NOTE]
> To transfer files using a custom SSH port, use uppercase `-P` with `scp`, unlike the lowercase `-p` used by `ssh`.
>
> ```bash
> scp -P <SSH_PORT> <LOCAL_FILE> <USERNAME>@<IP_ADDRESS>:<REMOTE_DIRECTORY>/
> ```
>
> File paths containing spaces should be quoted. On Windows, local paths may use backslashes, such as `C:\Users\...`. File paths inside WSL use Linux-style paths instead.

[🔼 Back to Overview](#overview)

## Troubleshooting

| Symptom | What to check |
|---|---|
| `Connection timed out` | Destination IP, routing, VPN, firewalls, reachable SSH port |
| `Connection refused` | SSH service listening, correct port, host firewall |
| `Host key verification failed` | Verify identity/fingerprint; a changed host key may indicate a rebuild or a security issue |
| `Permission denied (publickey)` | Authorized user, correct key, key permissions, SSH server policy |
| `Permission denied (publickey,password)` | Confirm the username, password or SSH key, and server authentication settings. Root password login may be restricted even when password authentication is enabled for other accounts. |
| `ssh: command not found` | Install the local OpenSSH client |

<details>
<summary><strong>Remote Host Identification Has Changed</strong></summary>

<br>

```text
WARNING: REMOTE HOST IDENTIFICATION HAS CHANGED!
Host key verification failed.
```

This error occurs when the SSH host key presented by a server does not match the key previously saved on your computer. It can happen when a server is reinstalled, a virtual machine is rebuilt, or an IP address is reassigned to a different device.

SSH stores previously accepted host keys in a `known_hosts` file to detect unexpected changes and protect against potential interception attempts.

> **⚠️ CAUTION:** A changed SSH host key can indicate a legitimate server replacement or a potential security issue. Never remove a saved host key or accept a replacement without first verifying the server's identity.

1. Confirm that the IP address or hostname belongs to the intended server. Verify the server's new SSH host-key fingerprint using a trusted source before proceeding.

2. Remove the previous host-key entry from your local computer.

    - `ssh-keygen -R <IP_ADDRESS>` removes the saved SSH host keys associated with the specified IP address or hostname.
        - `ssh-keygen` manages SSH authentication keys and known host entries.
        - `-R` removes matching host-key entries from the local `known_hosts` file.
        - `<IP_ADDRESS>` identifies the server address whose saved entries should be removed.

    ```bash
    ssh-keygen -R <IP_ADDRESS>
    ```

    For a server using a custom SSH port, include the port number in the host identifier. The square brackets `[]` distinguish the host address from the port number.

    ```bash
    ssh-keygen -R "[<IP_ADDRESS>]:<SSH_PORT>"
    ```

3. Reconnect to the server.

    ```bash
    ssh <USERNAME>@<IP_ADDRESS>
    ```

    If the server uses a custom SSH port:

    ```bash
    ssh -p <SSH_PORT> <USERNAME>@<IP_ADDRESS>
    ```

4. Compare the fingerprint displayed during the connection with the independently verified fingerprint. Accept the new host key only if they match.

</details>

[🔼 Back to Overview](#overview)

## Related Documentation

- [Create a Linux User](../../05-guides/linux/create-linux-user.md) — Create a non-root Linux account, configure optional sudo privileges, and verify account access.

## Official Documentation

- [OpenSSH Manual Pages](https://www.openssh.com/manual.html) — Official reference for SSH commands, authentication, configuration, secure file transfers, and troubleshooting.
- [Microsoft Learn — OpenSSH for Windows](https://learn.microsoft.com/en-us/windows-server/administration/openssh/openssh-overview) — Official Microsoft documentation for installing, configuring, and using OpenSSH on Windows.

---

[🔼 Back to Getting Started](../README.md)