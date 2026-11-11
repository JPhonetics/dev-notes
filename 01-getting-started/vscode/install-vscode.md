---
Created: 2026-10-06
Modified: 2026-11-10
---

# Install Visual Studio Code

Visual Studio Code (VS Code) is a lightweight source code editor used for writing, navigating, debugging, and managing code across many programming languages and frameworks. It includes features such as syntax highlighting, integrated terminals, source control integration, debugging tools, and support for extensions.

VS Code is especially useful in this development environment because it can work directly with projects stored inside WSL while still running as a Windows application. This provides a familiar graphical editor while allowing development tools, runtimes, and commands to execute inside the Ubuntu environment.

This guide installs VS Code on Windows, configures it for use with WSL, and verifies that projects inside the Linux development environment can be opened and edited correctly.

## Overview

1. [Install VS Code](#install-vs-code)
2. [WSL Integration](#wsl-integration)
3. [Configure Preferences](#configure-preferences)
4. [Verify Installation](#verify-installation)
5. [Install Extensions](#install-extensions)
6. [Related Documentation](#related-documentation)
7. [Official Documentation](#official-documentation)

## Install VS Code

1. Download and install [VS Code](https://code.visualstudio.com/download).

    1. **Select Additional Tasks:** 
        - ✅ Add "Open with Code" action to Windows Explorer file context menu
        - ✅ Add "Open with Code" action to Windows Explorer directory context menu
        - ✅ Register Code as an editor for supported file types
        - ✅ Add to PATH (requires shell restart)

2. Launch **VS Code**.

3. Signing into **VS Code** is optional. It enables features such as:

    - GitHub Copilot
    - Settings Sync across multiple machines
    - Extensions and services that use account authentication

[🔼 Back to Overview](#overview)

## WSL Integration

The WSL extension allows VS Code to connect directly to the Ubuntu environment running through WSL. When connected, VS Code continues to run as a Windows application while terminals, development tools, runtimes, and project files operate within the Linux environment.

VS Code can reopen previously connected WSL workspaces, allowing you to return to your Linux projects without manually configuring the remote connection each time.

1. Click the **Extensions** icon in the left panel.
2. Install **WSL** by **Microsoft**.
3. Click the `><` icon in the bottom-left corner.
4. Select **Connect to WSL**.

> [!TIP]
> To change or close the remote connection:
> 1. Click the `><` icon in the bottom-left corner.
> 2. Select another connection or **Close Remote Connection**.

[🔼 Back to Overview](#overview)

## Configure Preferences

<details>
<summary><strong>Auto Save</strong></summary>

<br>

Auto Save automatically saves file changes after a short delay, reducing the chance of losing unsaved work and ensuring development tools can immediately detect file changes.

1. Select **File > Auto Save**.

</details>

<details>
<summary><strong>Default Terminal</strong></summary>

<br>

When VS Code is connected to WSL, new integrated terminals automatically run inside the Linux environment. When working in a local Windows window, selecting Ubuntu (WSL) as the default terminal profile allows new terminals to open in Ubuntu instead of PowerShell.

1. Select **Terminal > New Terminal**.
2. Click the `⌵` menu in the **Terminal** section.
3. Select **Select Default Profile**.
4. Select **Ubuntu (WSL)**.

</details>

[🔼 Back to Overview](#overview)

## Verify Installation

> [!NOTE]
> If you previously configured VS Code as Git's default editor in the [Install Git](../git/install-git.md) guide, the `code` command is now available for Git to use when opening commit messages and other files requiring a text editor. No additional Git configuration is needed.

1. Open **Windows Terminal** and select **Ubuntu (WSL)**.

2. Open the current Linux directory in VS Code.

    - `code .` opens the current working directory in Visual Studio Code.
        - `code` launches VS Code from the terminal.
        - `.` represents the current directory.

    ```bash
    code .
    ```

3. Confirm that VS Code opens the directory in a WSL window. The bottom-left corner should display a WSL connection indicator.

4. Select **Terminal > New Terminal**.

5. Verify that the integrated terminal is running inside Ubuntu.

    ```bash
    pwd
    ```

    The output should display the current directory as a Linux path, such as `/home/<USERNAME>`, rather than a Windows path.

[🔼 Back to Overview](#overview)

## Install Extensions

VS Code extensions add language support, debugging tools, linters, formatters, framework integrations, and other development features to the editor.

> [!NOTE]
> Recommended VS Code extensions are documented separately. 
>
> 📌 [VS Code Extensions](./vscode-extensions.md)

[🔼 Back to Overview](#overview)

## Related Documentation

- [Setup Windows for Development](../setup-windows.md) — Follow the complete Windows development environment setup roadmap.

## Official Documentation

- [Visual Studio Code Documentation](https://code.visualstudio.com/docs) — Official documentation for installing, configuring, and using VS Code, including editor features, settings, debugging, and development tools.
- [VS Code WSL Documentation](https://code.visualstudio.com/docs/remote/wsl) — Official guide for connecting VS Code to Windows Subsystem for Linux (WSL) and developing within a Linux environment.
- [VS Code Extensions Documentation](https://code.visualstudio.com/docs/editor/extension-marketplace) — Official documentation for discovering, installing, managing, and configuring VS Code extensions.

---

[🔼 Back to Getting Started](../README.md)