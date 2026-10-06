---
Created: 2026-10-05
Modified: 
---

# Setup Windows for Development

This guide builds a complete Windows development environment that combines native Windows applications with a Linux development environment through Windows Subsystem for Linux (WSL). Windows remains the primary operating system, while Ubuntu provides access to Linux command-line tools, package managers, development frameworks, databases, containers, and other tools commonly used in modern software development.

This setup provides the flexibility of working across both Windows and Linux without maintaining a separate Linux machine or dual-boot configuration. Windows applications such as Visual Studio Code, Docker Desktop, browsers, and other desktop tools can be used alongside Linux-based development workflows running through WSL.

This guide provides the recommended installation and configuration order for the development environment. Each major component is documented in its own setup guide, while this page serves as the main roadmap for assembling the complete environment.

## Overview

<details>
<summary><strong>1. Install WSL</strong></summary>

<br>

Windows Subsystem for Linux (WSL) allows Linux distributions such as Ubuntu to run directly within Windows without requiring a traditional virtual machine or dual-boot configuration. It provides access to Linux command-line tools, package managers, utilities, and development frameworks while continuing to use Windows as the primary operating system.

WSL is especially useful for software development because many development tools and production environments are Linux-based. It allows Linux workflows to run alongside Windows applications such as Visual Studio Code, Docker Desktop, and web browsers, providing the benefits of both environments on the same system.

This setup guide installs WSL and Ubuntu, completes the initial Linux user configuration, and verifies that the Ubuntu environment is installed and running with WSL 2.

📌 [Go to Guide](./wsl/install-wsl.md)

</details>

<details>
<summary><strong>2. Install Windows Terminal</strong></summary>

<br>

Windows Terminal is a modern terminal application for command-line environments such as PowerShell, Command Prompt, and WSL. It supports multiple profiles, tabs, panes, and customizable appearance settings within a single interface.

It provides a cleaner and more efficient way to work across Windows and Linux command-line environments without switching between separate terminal applications. For development, it makes it easy to move between PowerShell and the Ubuntu environment running through WSL while keeping everything in one place.

This setup guide installs Windows Terminal, configures Ubuntu as the default profile, and applies a few appearance settings to make the terminal more comfortable for everyday development.

📌 [Go to Guide](./windows/install-windows-terminal.md)

</details>

<details>
<summary><strong>3. Update Ubuntu</strong></summary>

<br>

Ubuntu uses the Advanced Package Tool (APT) to manage software packages, dependencies, and system updates. Keeping the package lists and installed software current helps ensure the development environment has the latest available fixes, security updates, and current package versions.

Updating Ubuntu is useful before installing development tools because many later setup steps depend on system packages and libraries provided by the Linux environment. Starting from an up-to-date system helps reduce installation issues caused by outdated package information or dependencies.

This setup guide updates Ubuntu's package lists, installs available package upgrades, removes packages that are no longer required, and installs a small set of commonly used command-line utilities for development.

📌 [Go to Guide](./linux/update-ubuntu.md)

</details>