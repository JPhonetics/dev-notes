---
Created: 2026-10-05
Modified: 
---

# Update Ubuntu

Ubuntu uses the Advanced Package Tool (APT) to manage software packages, dependencies, and system updates. Keeping the package lists and installed software current helps ensure the development environment has the latest available fixes, security updates, and current package versions.

Updating Ubuntu is useful before installing development tools because many later setup steps depend on system packages and libraries provided by the Linux environment. Starting from an up-to-date system helps reduce installation issues caused by outdated package information or dependencies.

This guide updates Ubuntu's package lists, installs available package upgrades, and removes packages that are no longer required.

## Overview

1. [Update Ubuntu](#update-ubuntu)

## Update Ubuntu

1. Open **Windows Terminal**.

2. Refresh Ubuntu's package index. `apt update` refreshes Ubuntu's package index by checking the configured repositories for currently available package versions. It does not install any updates.

```bash
sudo apt update
```

3. Install available package upgrades. `apt upgrade` uses the refreshed package information to install newer versions of packages already installed on the system, as long as the upgrade does not require removing installed packages.

> [!IMPORTANT]
> The `-y` option automatically answers **yes** to package confirmation prompts. This applies to both `apt upgrade -y` and `apt autoremove -y`. Remove `-y` if you want to review and manually confirm the changes before they are applied.

```bash
sudo apt upgrade -y
```

4. Remove packages that are no longer required. `apt autoremove` removes dependency packages that were installed automatically but are no longer required by any installed software.

```bash
sudo apt autoremove -y
```

## Related Documentation

- [Windows Development Setup](../setup-windows.md)