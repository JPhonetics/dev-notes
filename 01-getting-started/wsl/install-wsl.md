---
Created: 2026-10-05
Modified: 
---

# Install Windows Subsystem for Linux (WSL)

Windows Subsystem for Linux (WSL) allows Linux distributions such as Ubuntu to run directly within Windows without requiring a traditional virtual machine or dual-boot configuration. It provides access to Linux command-line tools, package managers, utilities, and development frameworks while continuing to use Windows as the primary operating system.

WSL is especially useful for software development because many development tools and production environments are Linux-based. It allows Linux workflows to run alongside Windows applications such as Visual Studio Code and web browsers, providing the benefits of both environments on the same system.

This guide installs WSL and Ubuntu, completes the initial Linux user configuration, and verifies that the Linux environment is installed and configured correctly.

## Overview

1. [Install WSL](#install-wsl)
2. [Install Ubuntu](#install-ubuntu)
3. [Verify Installation](#verify-installation)

## Install WSL

1. Open **PowerShell as Administrator** and install Windows Subsystem for Linux (WSL).
   - `wsl` is the Windows command-line tool used to install, configure, and manage Linux distributions running through WSL.
   - `--install` installs the required WSL components and, by default, the Ubuntu Linux distribution.

```powershell
wsl --install
```

2. After the installation completes, reboot Windows for the changes to take effect.

## Install Ubuntu

1. After restarting Windows, open **PowerShell as Administrator** and install Ubuntu.
   - `Ubuntu` specifies the Linux distribution to install. WSL uses this name to identify Ubuntu from the available distributions.

```powershell
wsl --install Ubuntu
```

2. After the installation completes, Ubuntu should start in the same **PowerShell** window. During its first launch, you will be prompted to create a Linux username and password.

> [!IMPORTANT]
> The Linux username and password are separate from your Windows account credentials. When entering the password, no characters will appear on screen. This is normal Linux terminal behavior.

After completing the initial setup, the terminal will enter the **Ubuntu environment**.

<br>

> [!NOTE]
> ### Alternative Linux distributions
>
> `--list --online` displays the Linux distributions available for installation.
>
> ```powershell
> wsl --list --online
> ```
>
> Install a different distribution by replacing `<Distro>` with its listed name:
>
> ```powershell
> wsl --install <Distro>
> ```

## Verify Installation

1. Exit the **Ubuntu environment** to return to **PowerShell**. Alternatively, open a new **PowerShell** window and skip this step.
   - `exit` closes the current Linux shell session and returns control to the previous terminal environment.

```bash
exit
```

2. Check the installed Linux distributions and their WSL versions in **PowerShell**.
   - `--list` displays the Linux distributions installed through WSL.
   - `--verbose` displays additional information, including each distribution's current state and WSL version.

```powershell
wsl --list --verbose
```

3. Confirm the installed Linux distribution and that the `VERSION` column shows `2`. The `*` indicates the default Linux distribution. The `STATE` value may vary from the example below.

```text
  NAME      STATE           VERSION
* Ubuntu    Running         2
```

## Related Documentation

- [Windows Development Setup](../setup-windows.md)

## Official Documentation

- [Windows Subsystem for Linux Documentation](https://learn.microsoft.com/windows/wsl/)