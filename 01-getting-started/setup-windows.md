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

This setup guide updates Ubuntu's package lists and installed packages, removes packages that are no longer required, and installs commonly used command-line utilities for development.

📌 [Go to Guide](./linux/update-ubuntu.md)

</details>

<details>
<summary><strong>4. Install Python</strong></summary>

<br>

Python is a programming language commonly used for backend development, automation, scripting, APIs, testing, and other development tasks.

It provides a general-purpose runtime for executing Python scripts and applications, while supporting isolated project dependencies through virtual environments.

This setup guide installs Python, pip, virtual environment support, and the system-level development packages commonly required for Python development.

📌 [Go to Guide](./python/install-python.md)

</details>

<details>
<summary><strong>5. Install JavaScript</strong></summary>

<br>

JavaScript is a programming language used for frontend applications, development tools, and server-side applications. Node.js provides the runtime needed to execute JavaScript outside of a web browser.

Node Version Manager (NVM) allows multiple Node.js versions to be installed and switched as needed, making it easier to work with projects that require different Node.js versions.

This setup guide installs NVM, uses it to install the current LTS version of Node.js, and verifies that Node.js and npm are ready for JavaScript development.

📌 [Go to Guide](./javascript/install-javascript.md)

</details>

<details>
<summary><strong>6. Install Git</strong></summary>

<br>

Git is a distributed version control system used to track changes to source code and maintain a history of a project over time.

It is a core development tool for managing changes, working with branches, restoring earlier versions, and collaborating through platforms such as GitHub.

This setup guide installs Git, configures the default branch and author identity, sets Visual Studio Code as the default Git text editor, and verifies the global configuration.

📌 [Go to Guide](./git/install-git.md)

</details>

<details>
<summary><strong>7. Install GitHub CLI</strong></summary>

<br>

GitHub is a cloud-based platform used to host Git repositories and provide collaboration features such as pull requests, issues, releases, code reviews, and automated workflows.

GitHub CLI (`gh`) provides command-line access to GitHub, making it possible to authenticate, create and clone repositories, and manage GitHub resources directly from the terminal.

This setup guide installs GitHub CLI, connects it to a GitHub account, configures Git authentication over HTTPS, and verifies that the connection is working correctly.

📌 [Go to Guide](./github/install-github-cli.md)

</details>

<details>
<summary><strong>8. Install Docker</strong></summary>

<br>

Docker is a container platform used to package applications and their dependencies into isolated, portable environments called containers.

It is useful for development because containers can provide consistent application environments, isolate project dependencies, and run supporting services such as databases or web servers without permanently installing them on the host system.

This setup guide installs Docker Engine and Docker Compose inside the Ubuntu WSL environment, configures Docker's official APT repository, and verifies that containers can run successfully.

📌 [Go to Guide](./docker/install-docker.md)

</details>

<details>
<summary><strong>9. Install Visual Studio Code (Text Editor)</strong></summary>

<br>

Visual Studio Code (VS Code) is a lightweight source code editor used for writing, navigating, debugging, and managing code across many programming languages and frameworks.

It integrates closely with WSL, allowing projects stored in Ubuntu to be edited through the Windows application while development tools, runtimes, and commands continue to run inside the Linux environment. Extensions can also add language support, formatting, debugging, container tooling, and other development features as needed.

This setup guide installs VS Code, configures it for use with WSL, and provides the recommended extensions used throughout the development environment.

📌 [Go to Guide](./vscode/install-vscode.md)

🧩 [Recommended VS Code Extensions](./vscode/vscode-extensions.md)

</details>

<details>
<summary><strong>10. Install DBeaver (SQL Client)</strong></summary>

<br>

DBeaver is a graphical database management application that provides a single interface for working with database systems such as PostgreSQL, MySQL, MariaDB, SQLite, and SQL Server.

It is used to explore schemas, view and modify data, execute SQL queries, and connect to databases running locally, remotely, or inside Docker containers.

This setup guide installs DBeaver on Windows and prepares it for use as the primary graphical database client for development.

📌 [Go to Guide](./sql/install-dbeaver.md)

</details>

<details>
<summary><strong>12. Install Postman (API Client)</strong></summary>

<br>

Postman is an API development and testing application used to send HTTP requests, inspect responses, manage authentication, organize request collections, and work with APIs from a graphical interface.

It is useful for developing and debugging APIs because requests, headers, parameters, request bodies, authentication settings, and environments can be saved and reused instead of being recreated manually.

This setup guide installs Postman on Windows and explains the optional account sign-in and the additional features it enables.

📌 [Go to Guide](./api/install-postman.md)

</details>

## Optional Additions

This section includes development tools that can be useful in specific workflows but are not required for the base development environment. Install them as needed based on the requirements of individual projects or development tasks.

<details>
<summary><strong>Install Claude Code</strong></summary>

<br>

Claude Code is an AI-assisted development tool that can work directly with a codebase from the terminal to help explain, modify, debug, and navigate projects.

This optional guide links to the official Claude Code installation documentation and provides a place to document any local setup or configuration used in this development environment.

📌 [Go to Guide](./ai/install-claude.md)

</details>

<details>
<summary><strong>Install Codex</strong></summary>

<br>

Codex is an AI-assisted development tool that can work with source code, terminal commands, project files, and development workflows to help build, modify, debug, and understand software projects.

This optional guide links to the official Codex installation documentation and provides a place to document any local setup or configuration used in this development environment.

📌 [Go to Guide](./ai/install-codex.md)

</details>