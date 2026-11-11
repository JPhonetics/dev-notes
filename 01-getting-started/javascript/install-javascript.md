---
Created: 2026-10-05
Modified: 2026-11-10
---

# Install JavaScript

JavaScript is a programming language commonly used to build interactive websites, frontend applications, development tools, and server-side applications. While web browsers can execute JavaScript directly, local development tools and server-side JavaScript require a runtime outside of the browser.

Node.js provides that runtime and allows JavaScript to execute from the command line. Node Package Manager (npm) is installed with Node.js and is used to install and manage JavaScript packages, development tools, frameworks, and project dependencies.

This guide installs Node Version Manager (NVM), uses NVM to install the current Long-Term Support (LTS) version of Node.js, and verifies that Node.js and npm are ready for JavaScript development.

## Overview

1. [Install Node Version Manager](#install-node-version-manager)
2. [Install Node.js](#install-nodejs)
3. [Verify Installation](#verify-installation)
4. [Related Documentation](#related-documentation)
5. [Official Documentation](#official-documentation)

## Install Node Version Manager

NVM is a version manager for Node.js. It allows multiple versions of Node.js to be installed for the current Linux user and makes it possible to switch between them as needed. This is useful when working on different projects that require different versions of Node.js.

> [!NOTE]
> Before installing NVM, check the [official NVM GitHub repository](https://github.com/nvm-sh/nvm#install--update-script) for the latest installation command. The version referenced in this guide may become outdated as new releases are published.

> [!IMPORTANT]
> All commands in this guide are executed inside the **Ubuntu WSL environment**, not PowerShell or Command Prompt.

1. Open **Windows Terminal**.

2. Confirm the terminal is running **Ubuntu (WSL)**. If not, click the `⌵` menu and select **Ubuntu**.

3. Download and install **NVM** using its official installation script. The installer installs NVM for the current Linux user and normally updates the shell configuration so the `nvm` command is available in future terminal sessions.

    - `curl -o- https://raw.githubusercontent.com/nvm-sh/nvm/v0.41.0/install.sh` downloads NVM's official installation script and sends its contents to standard output.
        - `curl` transfers data from a URL.
        - `-o-` directs the downloaded content to standard output instead of saving it to a file.
        - `https://raw.githubusercontent.com/nvm-sh/nvm/v0.41.0/install.sh` identifies the official installation script for NVM version 0.41.0.
    - `|` passes the output from the command on its left directly to the command on its right.
    - `bash` executes the downloaded installation script using Bash.

    ```bash
    curl -o- https://raw.githubusercontent.com/nvm-sh/nvm/v0.41.0/install.sh | bash
    ```

4. Close and reopen **Windows Terminal** to load NVM into the new shell session.

[🔼 Back to Overview](#overview)

## Install Node.js

Node.js provides the JavaScript runtime, while npm manages JavaScript packages and project dependencies. Development tools such as Vite use whichever Node.js version is currently active in the terminal.

1. Install the current Long-Term Support (LTS) version of Node.js. LTS releases are maintained for a longer period and are generally the preferred choice for development environments and production applications. npm is installed automatically with Node.js, so a separate `apt install npm` command is not required when Node.js is installed through NVM.

    - `nvm install --lts` downloads and installs the latest available LTS version of Node.js.
        - `nvm install` downloads and installs a Node.js version using NVM.
        - `--lts` selects the latest available Long-Term Support release instead of requiring a specific version number.

    ```bash
    nvm install --lts
    ```

2. Confirm the active Node.js version managed by NVM.

    - `nvm current` displays the Node.js version currently selected in the terminal session.

    ```bash
    nvm current
    ```

> [!NOTE]
> `nvm ls` lists locally installed Node.js versions and identifies the currently active version.
>
> ```bash
> nvm ls
> ```
>
> `nvm ls-remote` lists Node.js versions available for installation from the remote version repository.
>
> ```bash
> nvm ls-remote
> ```

> [!NOTE]
> - NVM does not install Node.js inside individual project directories. Node.js versions are stored in the user's NVM environment, and NVM controls which version is active in the shell.
> - `nvm use <version>` switches the active Node.js version for the current terminal session. Commands such as `node`, `npm`, and development tools such as Vite will use that active version.
> - Individual projects can specify a Node.js version using a `.nvmrc` file. When working inside a project that contains `.nvmrc`, executing `nvm use` without a version selects the version specified by that project.
>
> For example, switch to an already installed Node.js version 22:
>
> ```bash
> nvm use 22
> ```

[🔼 Back to Overview](#overview)

## Verify Installation

1. Check the installed version of Node Version Manager.

    ```bash
    nvm --version
    ```

2. Check the installed version of Node.js.

    ```bash
    node --version
    ```

3. Check the installed version of Node Package Manager.

    ```bash
    npm --version
    ```

[🔼 Back to Overview](#overview)

## Related Documentation

- [Setup Windows for Development](../setup-windows.md) — Follow the complete Windows development environment setup roadmap.

## Official Documentation

- [Node.js Documentation](https://nodejs.org/docs/latest/api/) — Official API reference for Node.js, including built-in modules, functions, and runtime features.
- [Node.js Release Schedule](https://github.com/nodejs/Release) — Official Node.js release schedule, including LTS versions, maintenance periods, and end-of-life dates.
- [npm Documentation](https://docs.npmjs.com/) — Official documentation for managing JavaScript packages, dependencies, configuration, and npm commands.
- [NVM Documentation](https://github.com/nvm-sh/nvm) — Official documentation for installing, managing, and switching between multiple Node.js versions using Node Version Manager (NVM).

---

[🔼 Back to Getting Started](../README.md)