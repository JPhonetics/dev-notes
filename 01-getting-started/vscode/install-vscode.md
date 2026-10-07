---
Created: 2026-10-06
Modified:
---

# Install Visual Studio Code

Visual Studio Code (VS Code) is a lightweight source code editor used for writing, navigating, debugging, and managing code across many programming languages and frameworks. It includes features such as syntax highlighting, integrated terminals, source control integration, debugging tools, and support for extensions.

VS Code is especially useful in this development environment because it can work directly with projects stored inside WSL while still running as a Windows application. This provides a familiar graphical editor while allowing development tools, runtimes, and commands to execute inside the Ubuntu environment.

This guide installs VS Code on Windows, configures it for use with WSL, and verifies that projects inside the Linux development environment can be opened and edited correctly.

## Overview

1. [Install VS Code](#install-vs-code)
2. [WSL Integration](#wsl-integration)
3. [Configure Preferences](#configure-preferences)
4. [Install Extensions](#install-extensions)

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

## WSL Integration

The WSL extension allows VS Code to connect directly to the Ubuntu environment running through WSL. When connected, VS Code continues to run as a Windows application while terminals, development tools, runtimes, and project files operate within the Linux environment.

VS Code automatically remembers the most recent remote environment, so launching it later will return to the last remote connection.

1. Click the **Extensions** icon in the left panel.
2. Install **WSL** by **Microsoft**.
3. Click the `><` icon in the bottom-left corner.
4. Select **Connect to WSL**.

> [!TIP]
> To change or close the remote connection:
> 1. Click the `><` icon in the bottom-left corner.
> 2. Select another connection or **Close Remote Connection**.

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

Setting Ubuntu (WSL) as the default terminal ensures new integrated terminal sessions open inside the Linux development environment instead of PowerShell.

1. Select **Terminal > New Terminal**.
2. Click the `⌵` menu in the **Terminal** section.
3. Select **Select Default Profile**.
4. Select **Ubuntu (WSL)**.

</details>

## Install Extensions

VS Code extensions add language support, debugging tools, linters, formatters, framework integrations, and other development features to the editor.

> [!NOTE]
> Recommended VS Code extensions are documented separately. 
>
> 📌 [VS Code Extensions](./vscode-extensions.md)

## Official Documentation

- [Visual Studio Code Documentation](https://code.visualstudio.com/docs)
- [VS Code WSL Documentation](https://code.visualstudio.com/docs/remote/wsl)
- [VS Code Extensions Documentation](https://code.visualstudio.com/docs/editor/extension-marketplace)

## Related Documentation

- [Windows Development Setup](../setup-windows.md)