---
Created: 2026-10-05
Modified: 
---

# Setup Windows for Development

Configure a Windows computer for software development using Windows Subsystem for Linux (WSL) and Ubuntu.

This guide provides the recommended setup path for creating a Windows development environment. Windows remains the primary operating system while WSL provides a Linux environment for development tools, command-line utilities, frameworks, databases, containers, and other Linux-based workflows.

The environment combines the convenience of Windows applications with the Linux tooling commonly used for modern software development.

> [!NOTE]
> Detailed installation and configuration instructions are maintained in separate guides. This page provides the recommended setup order and explains how each component fits into the development environment.

## Overview

<details>
<summary><strong>1. Install WSL</strong></summary>

<br>

Windows Subsystem for Linux (WSL) allows Linux distributions such as Ubuntu to run directly within Windows without requiring a traditional virtual machine or dual-boot configuration. It provides access to Linux command-line tools, package managers, utilities, and development frameworks while continuing to use Windows as the primary operating system.

WSL is especially useful for software development because many development tools and production environments are Linux-based. It allows Linux workflows to run alongside Windows applications such as Visual Studio Code, Docker Desktop, and web browsers, providing the benefits of both environments on the same system.

This guide installs WSL and Ubuntu, completes the initial Linux user configuration, and verifies that the Ubuntu environment is installed and running with WSL 2.

➡️ [Go to Guide](./wsl/install-wsl.md)

</details>

<details>
<summary><strong>2. Install Windows Terminal</strong></summary>

<br>

Windows Terminal is a modern terminal application for command-line environments such as PowerShell, Command Prompt, and WSL. It supports multiple profiles, tabs, panes, and customizable appearance settings within a single interface.

It provides a cleaner and more efficient way to work across Windows and Linux command-line environments without switching between separate terminal applications. For development, it makes it easy to move between PowerShell and the Ubuntu environment running through WSL while keeping everything in one place.

This guide installs Windows Terminal, configures Ubuntu as the default profile, and applies a few appearance settings to make the terminal more comfortable for everyday development.

➡️ [Go to Guide](./windows/install-windows-terminal.md)

</details>