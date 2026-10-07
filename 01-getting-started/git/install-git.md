---
Created: 2026-10-06
Modified:
---

# Install Git

Git is a distributed version control system used to track changes to files and source code over time. It allows developers to create a history of changes, work safely across branches, compare revisions, and restore earlier versions when needed.

Git is a core part of modern software development because it makes it easier to manage changes, collaborate with others, and maintain a reliable history of a project. It also provides the local version control system used by platforms such as GitHub.

This guide installs Git, configures the default branch name and user identity, sets Visual Studio Code as the default Git editor, and verifies the installation and global configuration.

## Overview

1. [Install Git](#install-git)
2. [Verify Installation](#verify-installation)

## Install Git

> [!IMPORTANT]
> All commands in this guide are executed inside the **Ubuntu WSL environment**, not PowerShell or Command Prompt.

1. Open **Windows Terminal**.
2. Confirm the terminal is running **Ubuntu (WSL)**. If not, click the `⌵` menu and select **Ubuntu**.
3. Install Git.

```bash
sudo apt install git -y
```

4. Set the default branch name to `main`. This ensures repositories created with `git init` use `main` as the initial branch, matching the naming convention commonly used by GitHub and many modern projects.

```bash
git config --global init.defaultBranch main
```

5. Set the default Git author name. This name is recorded with each commit you create.

```bash
git config --global user.name "<YOUR_NAME>"
```

6. Set the default Git author email. This email is recorded with each commit and can be used by GitHub to associate commits with your account.

```bash
git config --global user.email "<YOUR_EMAIL>"
```

7. Set **Visual Studio Code (VS Code)** as Git's default text editor. The `code` command becomes available after VS Code is installed and WSL integration is configured.

```bash
git config --global core.editor code
```

<br>

> [!NOTE]
> Global Git settings are used by default across repositories, but the author name and email can be overridden for an individual repository. Execute these commands from inside the project folder that contains `.git`.
>
> ```bash
> git config user.name "<YOUR_NAME>"
> git config user.email "<YOUR_EMAIL>"
> ```
>
> Check which name and email Git is using and where each value is configured:
>
> ```bash
> git config --show-origin user.name
> git config --show-origin user.email
> ```
>
> View the complete configuration and the source of each setting:
>
> ```bash
> git config --list --show-origin
> ```

## Verify Installation

1. Verify Git.

```bash
git --version
```

2. Verify global Git configuration.

```bash
git config --global -l
```

## Related Documentation

- [Windows Development Setup](../setup-windows.md)

## Official Documentation

- [Git Documentation](https://git-scm.com/docs)
- [Git Book](https://git-scm.com/book/en/v2)