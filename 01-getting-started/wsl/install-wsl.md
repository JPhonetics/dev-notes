---
Created: 2026-10-05
Modified: 2026-11-10
---

# Install Windows Subsystem for Linux (WSL)

Windows Subsystem for Linux (WSL) allows Linux distributions such as Ubuntu to run directly within Windows without requiring a traditional virtual machine or dual-boot configuration. It provides access to Linux command-line tools, package managers, utilities, and development frameworks while continuing to use Windows as the primary operating system.

WSL is especially useful for software development because many development tools and production environments are Linux-based. It allows Linux workflows to run alongside Windows applications such as Visual Studio Code and web browsers, providing the benefits of both environments on the same system.

This guide installs WSL and Ubuntu, completes the initial Linux user configuration, and verifies that the Linux environment is installed and configured correctly.

## Overview

1. [Install WSL](#install-wsl)
2. [Set Up Ubuntu](#set-up-ubuntu)
3. [Verify Installation](#verify-installation)
4. [Related Documentation](#related-documentation)
5. [Official Documentation](#official-documentation)

## Install WSL

1. Open **PowerShell as Administrator** and install Windows Subsystem for Linux (WSL).

    - `wsl` is the Windows command-line tool used to install, configure, and manage Linux distributions running through WSL.
    - `--install` installs the required WSL components and, by default, the Ubuntu Linux distribution.

    ```powershell
    wsl --install
    ```

2. After the installation completes, reboot Windows for the changes to take effect.

[🔼 Back to Overview](#overview)

## Set Up Ubuntu

1. After restarting Windows, launch **Ubuntu** from the Start menu. Alternatively, open **PowerShell** and launch Ubuntu using WSL.

    - `wsl -d Ubuntu` launches the installed Ubuntu distribution.
        - `-d` specifies which Linux distribution to launch.
        - `Ubuntu` identifies the Linux distribution.

    ```powershell
    wsl -d Ubuntu
    ```

> [!IMPORTANT]
> The Linux username and password are separate from your Windows account credentials. When entering the password, no characters will appear on screen. This is normal Linux terminal behavior.

2. During Ubuntu's first launch, you will be prompted to create a Linux username and password.

    After completing the initial setup, the terminal will enter the **Ubuntu environment**.

> [!NOTE]
> **Alternative Linux distributions**
>
> `--list --online` displays the Linux distributions available for installation.
>
> ```powershell
> wsl --list --online
> ```
>
> Install a different distribution by replacing `<DISTRO>` with its listed name:
>
> ```powershell
> wsl --install <DISTRO>
> ```

[🔼 Back to Overview](#overview)

## Verify Installation

1. Return to **PowerShell** to verify the WSL installation. If Ubuntu was launched from PowerShell, use `exit` to close the Linux shell and return to PowerShell. Otherwise, open a new **PowerShell** window.

    - `exit` closes the current Linux shell session.

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

[🔼 Back to Overview](#overview)

## Related Documentation

- [Setup Windows for Development](../setup-windows.md) — Follow the complete Windows development environment setup roadmap.

## Official Documentation

- [Windows Subsystem for Linux Documentation](https://learn.microsoft.com/windows/wsl/) — Official Microsoft documentation for installing, configuring, managing, and troubleshooting WSL, including Linux distributions, commands, settings, and Windows integration.

---

[🔼 Back to Getting Started](../README.md)