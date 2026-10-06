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

Windows Subsystem for Linux (WSL) provides a Linux environment directly within Windows, allowing Linux command-line tools, package managers, and development workflows to run alongside Windows applications.

WSL is used as the primary Linux development environment for this setup, providing access to the Linux tooling commonly used for modern software development without requiring a separate virtual machine or dual-boot configuration.

This setup guide installs WSL and Ubuntu, completes the initial Linux user configuration, and verifies that the environment is installed correctly.

📌 [Go to Guide](./wsl/install-wsl.md)

</details>

<details>
<summary><strong>2. Install Windows Terminal</strong></summary>

<br>

Windows Terminal provides a modern interface for command-line environments such as PowerShell, Command Prompt, and WSL, with support for multiple profiles, tabs, panes, and appearance settings.

It provides a single place to work with both Windows and Linux command-line environments and serves as the primary terminal interface for accessing Ubuntu through WSL.

This setup guide installs Windows Terminal, configures Ubuntu as the default profile, and applies basic appearance settings for development.

📌 [Go to Guide](./windows/install-windows-terminal.md)

</details>

<details>
<summary><strong>3. Update Ubuntu</strong></summary>

<br>

Ubuntu provides the Linux environment used for development through WSL and uses APT to manage system packages, dependencies, and updates.

Keeping Ubuntu current provides an up-to-date foundation for the development tools installed later and helps prevent issues caused by outdated packages or dependencies.

This setup guide updates Ubuntu, removes packages that are no longer required, and installs commonly used command-line utilities for development.

📌 [Go to Guide](./linux/update-ubuntu.md)

</details>

<details>
<summary><strong>3. Install Python</strong></summary>

<br>

Python is a programming language commonly used for backend development, automation, scripting, APIs, testing, and other development tasks.

It provides a general-purpose runtime for executing Python scripts and applications, while supporting isolated project dependencies through virtual environments.

This setup guide installs Python, pip, virtual environment support, and the system-level development packages commonly required for Python development.

📌 [Go to Guide](./python/install-python.md)

</details>

<details>
<summary><strong>3. Install JavaScript</strong></summary>

<br>

JavaScript is a programming language used for frontend applications, development tools, and server-side applications. Node.js provides the runtime needed to execute JavaScript outside of a web browser.

Node Version Manager (NVM) allows multiple Node.js versions to be installed and switched as needed, making it easier to work with projects that require different Node.js versions.

This setup guide installs NVM, uses it to install the current LTS version of Node.js and npm, and verifies that the environment is ready for JavaScript development.

📌 [Go to Guide](./javascript/install-javascript.md)

</details>