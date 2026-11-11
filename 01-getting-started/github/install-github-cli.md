---
Created: 2026-10-06
Modified: 2026-11-10
---

# Install GitHub CLI

GitHub is a cloud-based platform used to host Git repositories and provide collaboration features such as pull requests, issues, releases, code reviews, and automated workflows. Git continues to manage version control locally, while GitHub provides a remote location for storing and collaborating on repositories.

GitHub CLI (`gh`) provides command-line access to GitHub from the terminal. It can be used to authenticate with GitHub, create and clone repositories, manage pull requests and issues, and perform other GitHub actions without relying entirely on the web interface.

This guide installs GitHub CLI, connects it to a GitHub account, configures Git to authenticate with GitHub over HTTPS, and verifies that the CLI is installed and authenticated correctly.

## Overview

1. [Install GitHub CLI](#install-github-cli)
2. [Log into GitHub](#log-into-github)
3. [Verify Installation](#verify-installation)
4. [Related Documentation](#related-documentation)
5. [Official Documentation](#official-documentation)

## Install GitHub CLI

> [!IMPORTANT]
> All commands in this guide are executed inside the **Ubuntu WSL environment**, not PowerShell or Command Prompt.

1. Open **Windows Terminal**.

2. Confirm the terminal is running **Ubuntu (WSL)**. If not, click the `⌵` menu and select **Ubuntu**.

3. Install **GitHub CLI** using GitHub's official APT repository. The installation checks for required tools, downloads GitHub's GPG signing key, configures the repository, and installs GitHub CLI.

    - `type -p wget >/dev/null || (sudo apt update && sudo apt install wget -y)` checks whether `wget` is installed and installs it if necessary.
        - `type -p wget` checks whether the `wget` executable is available by looking for it in the directories specified by `PATH`.
        - `>/dev/null` redirects standard output to `/dev/null`, discarding the result.
        - `||` executes the command on its right only if the command on its left fails.
        - `()` groups commands together in a subshell.
        - `sudo apt update` refreshes Ubuntu's package lists without installing updates.
            - `sudo` (superuser do) executes commands with administrative privileges.
            - `apt update` refreshes the list of available packages and versions from configured repositories.
        - `&&` executes the next command only if the preceding command succeeds.
        - `sudo apt install wget -y` installs the `wget` download utility.
            - `apt install` installs packages and their required dependencies.
            - `wget` downloads files from URLs.
            - `-y` automatically answers yes to installation confirmation prompts.
    - `sudo mkdir -p -m 755 /etc/apt/keyrings` creates the directory used to store repository signing keys.
        - `mkdir` creates directories.
        - `-p` creates missing parent directories and avoids errors if the directory already exists.
        - `-m 755` sets permissions allowing the owner to read, write, and access the directory, while other users can read and access it.
    - `out=$(mktemp)` creates a uniquely named temporary file and stores its path in the `out` variable.
        - `mktemp` securely creates a temporary file.
        - `$(...)` executes a command and substitutes its output.
        - `out=` assigns the resulting path to a shell variable.
    - `wget -nv -O$out https://cli.github.com/packages/githubcli-archive-keyring.gpg` downloads GitHub CLI's official GPG signing key into the temporary file.
        - `-nv` reduces download output while retaining useful messages.
        - `-O` specifies the destination file.
        - `$out` expands to the temporary file path.
    - `cat $out | sudo tee /etc/apt/keyrings/githubcli-archive-keyring.gpg > /dev/null` copies the downloaded signing key into the APT keyring directory.
        - `cat $out` reads the temporary file and outputs its contents.
        - `|` passes the output of the command on the left to the command on the right.
        - `tee` reads standard input and writes it to the specified file while also displaying it.
        - `> /dev/null` redirects standard output to `/dev/null`, suppressing the displayed content.
    - `sudo chmod go+r /etc/apt/keyrings/githubcli-archive-keyring.gpg` makes the signing key readable by the group and other users.
        - `chmod` modifies file permissions.
        - `go+r` adds read permission for the group (`g`) and others (`o`).
    - `sudo mkdir -p -m 755 /etc/apt/sources.list.d` ensures the directory used to store additional APT repository configurations exists.
    - `echo "deb ..."` creates the repository configuration and passes it to `sudo tee /etc/apt/sources.list.d/github-cli.list`, which saves it to the APT source file.
        - `echo` prints the specified text.
        - `deb` identifies the repository as a source of installable Debian packages.
        - `arch=` specifies which system architecture the repository supports.
        - `$(dpkg --print-architecture)` determines the system's package architecture, such as `amd64`.
        - `signed-by=` identifies the GPG key used to verify packages from this repository.
        - `https://cli.github.com/packages` is GitHub CLI's official APT repository.
        - `stable` identifies the repository's distribution.
        - `main` identifies the repository component containing its packages.
    - `sudo apt update` refreshes the package lists to include GitHub CLI's newly added repository.
    - `sudo apt install gh -y` installs GitHub CLI from the configured repository.
        - `gh` is the GitHub CLI package.
    - `\` at the end of a line continues the command onto the next line, allowing the installation sequence to remain readable.

    ```bash
    (type -p wget >/dev/null || (sudo apt update && sudo apt install wget -y)) \
        && sudo mkdir -p -m 755 /etc/apt/keyrings \
        && out=$(mktemp) && wget -nv -O$out https://cli.github.com/packages/githubcli-archive-keyring.gpg \
        && cat $out | sudo tee /etc/apt/keyrings/githubcli-archive-keyring.gpg > /dev/null \
        && sudo chmod go+r /etc/apt/keyrings/githubcli-archive-keyring.gpg \
        && sudo mkdir -p -m 755 /etc/apt/sources.list.d \
        && echo "deb [arch=$(dpkg --print-architecture) signed-by=/etc/apt/keyrings/githubcli-archive-keyring.gpg] https://cli.github.com/packages stable main" | sudo tee /etc/apt/sources.list.d/github-cli.list > /dev/null \
        && sudo apt update \
        && sudo apt install gh -y
    ```

[🔼 Back to Overview](#overview)

## Log into GitHub

1. Authenticate **GitHub CLI** with your GitHub account.

    - `gh auth login` begins the interactive authentication process.
        - `gh` is the command-line interface used to interact with GitHub.
        - `auth login` authenticates GitHub CLI with your GitHub account.

    ```bash
    gh auth login
    ```

2. Follow the authentication prompts.

    1. **What account do you want to log into?**
        - GitHub.com
    2. **What is your preferred protocol for Git operations on this host?**
        - HTTPS
    3. **Authenticate Git with your GitHub credentials?**
        - Yes
    4. **How would you like to authenticate GitHub CLI?**
        - Login with a web browser

3. Complete authentication in the browser.

    1. Copy the one-time authentication code displayed in the terminal.
    2. Press **Enter** to open [GitHub's device authorization page](https://github.com/login/device) in your browser.
    3. Log into your **GitHub** account if prompted.
    4. Enter the one-time code and follow the authorization prompts.
    5. Approve the GitHub CLI authorization request.

4. Return to **Windows Terminal** and confirm authentication completed successfully.

> [!NOTE]
> To log out of **GitHub CLI**, run the following command from the Ubuntu terminal:
>
> ```bash
> gh auth logout
> ```

[🔼 Back to Overview](#overview)

## Verify Installation

1. Check the installed version of GitHub CLI.

    ```bash
    gh --version
    ```

2. Verify GitHub authentication status. The output should confirm that GitHub CLI is signed in to your GitHub account.

    ```bash
    gh auth status
    ```

[🔼 Back to Overview](#overview)

## Related Documentation

- [Setup Windows for Development](../setup-windows.md) — Follow the complete Windows development environment setup roadmap.

## Official Documentation

- [GitHub CLI Documentation](https://cli.github.com/manual/) — Official reference for GitHub CLI commands, authentication, configuration, and managing GitHub resources from the terminal.
- [GitHub CLI Linux Installation](https://github.com/cli/cli/blob/trunk/docs/install_linux.md) — Official instructions for installing GitHub CLI on Ubuntu and other Linux distributions.
- [GitHub CLI Authentication](https://docs.github.com/en/github-cli/github-cli/authenticating-with-github-cli) — Official documentation for authenticating GitHub CLI and configuring Git credentials.
- [GitHub Documentation](https://docs.github.com/) — Official guides for using GitHub, including repositories, collaboration, pull requests, issues, and automated workflows.

---

[🔼 Back to Getting Started](../README.md)