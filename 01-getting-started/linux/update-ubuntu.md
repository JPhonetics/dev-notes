---
Created: 2026-10-05
Modified: 
---

# Update Ubuntu

Ubuntu uses the Advanced Package Tool (APT) to manage software packages, dependencies, and system updates. Keeping the package lists and installed software current helps ensure the development environment has the latest available fixes, security updates, and current package versions.

Updating Ubuntu is useful before installing development tools because many later setup steps depend on system packages and libraries provided by the Linux environment. Starting from an up-to-date system helps reduce installation issues caused by outdated package information or dependencies.

This guide updates Ubuntu's package lists, installs available package updates, removes packages that are no longer required, and installs a small set of commonly used command-line utilities for development.

## Overview

1. [Update Ubuntu](#update-ubuntu)
2. [Install Additional Packages](#install-additional-packages)

## Update Ubuntu

> [!IMPORTANT]
> All commands in this guide are executed inside the **Ubuntu WSL environment**, not PowerShell or Command Prompt.

> [!NOTE]
> The `-y` option automatically answers **yes** to package confirmation prompts. This applies to both `apt upgrade -y` and `apt autoremove -y`. Remove `-y` if you want to review and manually confirm the changes before they are applied.

1. Open **Windows Terminal**.
2. Confirm the terminal is running **Ubuntu (WSL)**. If not, click the `⌵` menu and select **Ubuntu**.
3. Refresh Ubuntu's package index. `apt update` refreshes Ubuntu's package index by checking the configured repositories for currently available package versions. It does not install any updates.

```bash
sudo apt update
```

4. Install available package upgrades. `apt upgrade` uses the refreshed package information to install newer versions of packages already installed on the system, as long as the upgrade does not require removing installed packages.

```bash
sudo apt upgrade -y
```

5. Remove packages that are no longer required. `apt autoremove` removes dependency packages that were installed automatically, but are no longer required by any installed software.

```bash
sudo apt autoremove -y
```

## Install Additional Packages

Commonly used command-line utilities that support development tools, installation scripts, downloads, and other Linux workflows. Some of these packages may already be installed.

```bash
sudo apt install curl nano unzip -y
```

> [!NOTE]
> - `curl` is a command-line tool used to transfer data to and from URLs. It is commonly used to download installation scripts, interact with APIs, test web endpoints, and retrieve files from the command line.
>
> - `nano` is a simple terminal-based text editor used to create and modify files directly from the command line. It is especially useful for quickly editing configuration files, scripts, and other text files without opening a graphical editor.
>
> - `unzip` is a command-line utility used to extract files from ZIP archives. It is commonly used when development tools, source code, installers, or other resources are distributed as compressed ZIP files.

## Related Documentation

- [Windows Development Setup](../setup-windows.md)

## Official Documentation

- [Ubuntu Documentation](https://documentation.ubuntu.com/)
- [APT Documentation](https://manpages.ubuntu.com/manpages/noble/en/man8/apt.8.html)