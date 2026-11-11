---
Created: 2026-10-05
Modified: 2026-11-10
---

# Update Ubuntu

Ubuntu uses the Advanced Package Tool (APT) to manage software packages, dependencies, and system updates. Keeping the package lists and installed software current helps ensure the development environment has the latest available fixes, security updates, and current package versions.

Updating Ubuntu is useful before installing development tools because many later setup steps depend on system packages and libraries provided by the Linux environment. Starting from an up-to-date system helps reduce installation issues caused by outdated package information or dependencies.

This guide updates Ubuntu's package lists, installs available package updates, removes packages that are no longer required, and installs a small set of commonly used command-line utilities for development.

## Overview

1. [Update Ubuntu](#update-ubuntu)
2. [Install Additional Packages](#install-additional-packages)
3. [Related Documentation](#related-documentation)
4. [Official Documentation](#official-documentation)

## Update Ubuntu

> [!IMPORTANT]
> All commands in this guide are executed inside the **Ubuntu WSL environment**, not PowerShell or Command Prompt.

1. Open **Windows Terminal**.

2. Confirm the terminal is running **Ubuntu (WSL)**. If not, click the `⌵` menu and select **Ubuntu**.

3. Refresh Ubuntu's package index.

    - `sudo` (superuser do) executes a command with administrative privileges, which are required to install, update, or remove system packages.
    - `apt` (Advanced Package Tool) is Ubuntu's package manager, used to install, update, and remove software packages.
    - `update` tells APT to refresh its package index by checking configured repositories for available package versions. It does not install any updates.

    ```bash
    sudo apt update
    ```

4. Install available package upgrades.

    - `apt upgrade` installs available updates for installed packages using the refreshed package information. It may install additional dependencies when required but does not remove existing packages.
    - `-y` automatically answers yes to confirmation prompts.

    ```bash
    sudo apt upgrade -y
    ```

5. Remove dependency packages that are no longer required. Review the packages APT proposes to remove before confirming.

    - `apt autoremove` identifies packages that were installed automatically as dependencies but are no longer required by installed software. It displays the proposed removals and requests confirmation before proceeding.

    ```bash
    sudo apt autoremove
    ```

[🔼 Back to Overview](#overview)

## Install Additional Packages

Install commonly used command-line utilities that support development tools, installation scripts, downloads, and other Linux workflows. Some of these packages may already be installed.

1. Install the additional development utilities.

    - `apt install curl nano unzip -y` installs the specified packages and any required dependencies.
        - `apt install` installs packages from Ubuntu's configured repositories.
        - `curl` is a command-line tool used to transfer data to and from URLs. It is commonly used to download installation scripts, interact with APIs, test web endpoints, and retrieve files from the command line.
        - `nano` is a simple terminal-based text editor used to create and modify files directly from the command line. It is especially useful for quickly editing configuration files, scripts, and other text files without opening a graphical editor.
        - `unzip` is a command-line utility used to extract files from ZIP archives. It is commonly used when development tools, source code, installers, or other resources are distributed as compressed ZIP files.

    ```bash
    sudo apt install curl nano unzip -y
    ```

[🔼 Back to Overview](#overview)

## Related Documentation

- [Setup Windows for Development](../setup-windows.md) — Follow the complete Windows development environment setup roadmap.

## Official Documentation

- [Ubuntu Documentation](https://documentation.ubuntu.com/) — Official documentation for installing, configuring, maintaining, and troubleshooting Ubuntu.
- [APT Documentation](https://manpages.ubuntu.com/manpages/noble/en/man8/apt.8.html) — Official reference for APT commands and options used to install, update, remove, and manage software packages in Ubuntu.
- [WSL Development Environment](https://learn.microsoft.com/windows/wsl/setup/environment) — Microsoft's official documentation for configuring and maintaining a Linux development environment within WSL.

---

[🔼 Back to Getting Started](../README.md)