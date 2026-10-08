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
   - `apt upgrade` uses the refreshed package information to install available upgrades without removing installed packages. `-y` is an APT command-line option that automatically answers yes to confirmation prompts.

```bash
sudo apt upgrade -y
```

5. Remove packages that are no longer required.
   - `apt autoremove` removes dependency packages that were installed automatically but are no longer required by any installed software. `-y` is an APT command-line option that automatically answers yes to confirmation prompts.

```bash
sudo apt autoremove -y
```

## Install Additional Packages

Install commonly used command-line utilities that support development tools, installation scripts, downloads, and other Linux workflows. Some of these packages may already be installed.
- `apt install` installs the specified software packages and any required dependencies from Ubuntu's configured repositories. `-y` is an APT command-line option that automatically answers yes to confirmation prompts.
- `curl` is a command-line tool used to transfer data to and from URLs. It is commonly used to download installation scripts, interact with APIs, test web endpoints, and retrieve files from the command line.
- `nano` is a simple terminal-based text editor used to create and modify files directly from the command line. It is especially useful for quickly editing configuration files, scripts, and other text files without opening a graphical editor.
- `unzip` is a command-line utility used to extract files from ZIP archives. It is commonly used when development tools, source code, installers, or other resources are distributed as compressed ZIP files.

```bash
sudo apt install curl nano unzip -y
```

## Related Documentation

- [Windows Development Setup](../setup-windows.md)

## Official Documentation

- [Ubuntu Documentation](https://documentation.ubuntu.com/)
- [APT Documentation](https://manpages.ubuntu.com/manpages/noble/en/man8/apt.8.html)