---
Created: 2026-10-05
Modified:
---

# Install JavaScript

JavaScript is a programming language commonly used to build interactive websites, frontend applications, development tools, and server-side applications. While web browsers can execute JavaScript directly, local development also requires a JavaScript runtime outside of the browser.

Node.js provides that runtime and allows JavaScript to execute from the command line. Node Package Manager (npm) is installed with Node.js and is used to install and manage JavaScript packages, development tools, frameworks, and project dependencies.

This setup guide installs Node Version Manager (NVM), uses NVM to install the current Long-Term Support (LTS) version of Node.js, and verifies that Node.js and npm are ready for JavaScript development.

## Overview

1. [Install Node Version Manager](#install-node-version-manager)
2. [Install Node.js](#install-nodejs)
3. [Verify Installation](#verify-installation)

## Install Node Version Manager

NVM is a version manager for Node.js. It allows multiple versions of Node.js to be installed for the current Linux user and makes it possible to switch between them as needed. This is useful when working on different projects that require different versions of Node.js.

1. Open **Windows Terminal**.

2. Download and install NVM. The NVM installation script installs NVM for the current Linux user and updates the shell configuration so that the `nvm` command is available in future terminal sessions.

```bash
curl -o- https://raw.githubusercontent.com/nvm-sh/nvm/v0.40.8/install.sh | bash
```

3. Close and reopen **Windows Terminal** to load NVM into the new shell session.

## Install Node.js

Node.js provides the JavaScript runtime, while npm manages JavaScript packages and project dependencies. Development tools such as Vite use whichever Node.js version is currently active in the terminal.

1. Install the current Long-Term Support (LTS) version of Node.js. LTS releases are maintained for a longer period and are generally the preferred choice for development environments and production applications. npm is installed automatically with Node.js, so a separate `apt install npm` command is not required when Node.js is installed through NVM.

```bash
nvm install --lts
```

2. Confirm the active Node.js version managed by NVM.

```bash
nvm current
```

<br>

> [!IMPORTANT]
> - NVM does not install Node.js inside individual project directories. Node.js versions are stored in the user's NVM environment, and NVM controls which version is active in the shell.
> - Use `nvm use <version>` to switch the active Node.js version for the current terminal session. Commands such as `node`, `npm`, and development tools such as Vite will use that active version.
> - Individual projects can later specify a Node.js version using a `.nvmrc` file. When working inside a project that contains `.nvmrc`, running `nvm use` selects the version specified by that project.
>
> For example:
>
> ```bash
> nvm use 22
> ```

## Verify Installation

1. Verify Node Version Manager.

```bash
nvm --version
```

2. Verify Node.js.

```bash
node --version
```

3. Verify Node Package Manager.

```bash
npm --version
```

## Related Documentation

- [Windows Development Setup](../setup-windows.md)