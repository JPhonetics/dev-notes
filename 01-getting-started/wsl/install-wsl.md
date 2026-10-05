---
Created: 2026-10-05
Modified: 
Tags: [install, linux, powershell, windows, wsl]
---

# Install Windows Subsystem for Linux (WSL)

WSL allows Linux distributions such as Ubuntu to run directly within Windows without requiring a traditional virtual machine or dual-boot configuration. It provides a lightweight, fast way to access Linux command-line tools, utilities, and development frameworks while continuing to use standard Windows applications.

This makes WSL especially useful for software development because Linux-based tools and workflows can run alongside Windows applications such as Visual Studio Code, Docker Desktop, and web browsers.

Ubuntu is installed by default, but WSL also supports other Linux distributions if a different environment is preferred.

> [!NOTE]
> For general WSL commands after installation, see [WSL Commands](commands.md).

## Overview

1. [Install WSL](#1-install-wsl)
2. [Install Ubuntu](#2-install-ubuntu)
3. [Verify Installation](#3-verify-installation)

## 1. Install WSL

Open **PowerShell as Administrator**.

```powershell
wsl --install
```

After the installation completes, reboot Windows for the changes to take effect.

## 2. Install Ubuntu

Open **PowerShell as Administrator**.

```powershell
wsl --install Ubuntu
```

After the installation completes, Ubuntu will start in the same PowerShell window and prompt you to create a Linux user and set a password.

> [!IMPORTANT]
> The Linux username and password are separate from your Windows account credentials. When entering the password, no characters will appear on screen. This is normal Linux terminal behavior.

After completing the initial setup, the terminal will enter the **Ubuntu environment**.

<br>

> [!NOTE]
> ### Alternative Linux distributions
>
> View available distributions:
>
> ```powershell
> wsl --list --online
> ```
>
> Install a different distribution by replacing `<Distro>` with its listed name:
>
> ```powershell
> wsl --install <Distro>
> ```

## 3. Verify Installation

Exit the **Ubuntu environment** to return to **PowerShell**. Alternatively, you can open a new **PowerShell** window.

```bash
exit
```

Check the installed Linux distributions and WSL version in **PowerShell**.

```powershell
wsl --list --verbose
```

Confirm the installed Linux distribution and WSL version. The `*` indicates the default Linux distribution. The `STATE` value may vary from the example below.

```text
  NAME      STATE           VERSION
* Ubuntu    Running         2
```

---

## Related Documentation

- [Windows Development Setup](../setup-windows.md)