---
Created: 2026-10-05
Modified: 2026-11-10
---

# Install Windows Terminal

Windows Terminal is a modern terminal application for command-line environments such as PowerShell, Command Prompt, and WSL. It supports multiple profiles, tabs, panes, and customizable appearance settings within a single interface.

It provides a cleaner and more efficient way to work across Windows and Linux command-line environments from one application. For development, it makes it easy to move between PowerShell and the Ubuntu environment running through WSL while keeping everything in one place.

This guide installs Windows Terminal, configures Ubuntu as the default profile, and applies a few appearance settings to make the terminal more comfortable for everyday development.

## Overview

1. [Install Windows Terminal](#install-windows-terminal)
2. [Set Default Profile](#set-default-profile)
3. [Configure Appearance](#configure-appearance)
4. [Related Documentation](#related-documentation)
5. [Official Documentation](#official-documentation)

## Install Windows Terminal

1. Open **Microsoft Store**.
2. Search **Windows Terminal**.
3. Install **Windows Terminal**.

[🔼 Back to Overview](#overview)

## Set Default Profile

> [!NOTE]
> Windows Terminal opens PowerShell by default. Changing the default profile to Ubuntu will launch directly into the WSL development environment.

1. Open **Windows Terminal**.
2. Click the `⌵` menu in the title bar.
3. Select **Settings**.
4. Select **Ubuntu** under **Startup > Default profile**.
5. Click **Save**.

[🔼 Back to Overview](#overview)

## Configure Appearance

> [!NOTE]
> Color scheme and appearance settings can be configured per profile, allowing Ubuntu and PowerShell to use different visual settings if desired.

1. Open **Windows Terminal**.
2. Click the `⌵` menu in the title bar.
3. Select **Settings**.
4. Select **Ubuntu** under **Profiles**.
5. Configure the following:
    - **Color scheme:** One Half Dark
    - **Font face:** Cascadia Code
    - **Font size:** 14
    - **Padding:** 10
    - **Scrollbar:** Visible
6. Click **Save**.

> [!TIP]
> Use the preview pane to test changes before saving.

[🔼 Back to Overview](#overview)

## Related Documentation

- [Setup Windows for Development](../setup-windows.md) — Follow the complete Windows development environment setup roadmap.

## Official Documentation

- [Windows Terminal Documentation](https://learn.microsoft.com/windows/terminal/) — Official documentation for installing, configuring, and using Windows Terminal, including profiles, tabs, panes, appearance settings, and integration with PowerShell and WSL.

---

[🔼 Back to Getting Started](../README.md)