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
   - `sudo` (superuser do) executes a command with administrative privileges, which are required to install, update, or remove system packages.
   - `apt install` installs the specified software packages and any required dependencies from Ubuntu's configured repositories. `-y` is an APT command-line option that automatically answers yes to confirmation prompts.
   - `git` is the name of the package available through Ubuntu's package manager.

```bash
sudo apt install git -y
```

4. Set the default branch name to `main`. This ensures repositories created with `git init` use `main` as the initial branch, matching the naming convention commonly used by GitHub and many modern projects.
   - `git config` reads or modifies Git configuration settings.
   - `--global` applies the setting to the current user's Git configuration, making it the default across repositories.
   - `init.defaultBranch main` sets `main` as the initial branch name for new repositories created with `git init`.

```bash
git config --global init.defaultBranch main
```

5. Configure your default Git author name and email. These details are recorded with each commit you create and can be used by GitHub to associate commits with your account.
   - `user.name` defines the name Git associates with your commits. It does not have to match your GitHub username.
   - `user.email` defines the email Git associates with your commits. To associate commits with your GitHub account, use an email address linked to that account or your GitHub-provided `noreply` email address.

```bash
git config --global user.name "<YOUR_NAME>"
git config --global user.email "<YOUR_EMAIL>"
```

6. Set **Visual Studio Code (VS Code)** as Git's default text editor. The `code` command becomes available after VS Code is installed and WSL integration is configured.
   - `core.editor` specifies the editor Git uses when it needs you to enter or modify text, such as a commit message.
   - `code` launches Visual Studio Code from the terminal.

```bash
git config --global core.editor code
```

<br>

> [!NOTE]
> Global Git settings apply by default to all repositories for the current user. However, individual repositories can have their own settings that override the global configuration. When `--global` is omitted, `git config` saves the specified settings to the current repository's `.git/config` file, provided the command is executed inside a Git repository.
>
> To override the default author name and email for a specific repository, execute these commands from inside that repository's project directory:
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

1. Check the installed version of Git.

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