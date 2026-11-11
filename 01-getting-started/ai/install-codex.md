---
Created: 2026-10-07
Modified: 2026-11-10
---

# Install Codex CLI

Codex CLI is an AI-assisted development tool developed by OpenAI that runs from the terminal and works directly with a project's files, source code, and development environment. It can help explain code, modify files, debug issues, navigate codebases, execute commands, and assist with software development workflows.

Codex CLI is separate from the standard ChatGPT chat interface, although both use OpenAI's models. Coding tasks can consume substantial amounts of tokens because they may involve reading multiple files, maintaining context, executing tools, and performing multi-step operations. Actual usage depends on the complexity of the task, the model selected, and the amount of context processed.

Codex CLI supports different authentication and billing methods:

- **ChatGPT account:** Eligible ChatGPT plans include Codex access. Usage is subject to the plan's applicable allowances and limits, which may be shared with other ChatGPT features. Depending on the plan and account configuration, additional paid usage may be available after included limits are reached.
- **OpenAI API:** Codex CLI can use an OpenAI API key associated with an OpenAI Platform account. API usage is billed separately based on consumption and does not use the included ChatGPT plan allowance.

This guide installs Codex CLI inside the Ubuntu WSL environment, authenticates using a ChatGPT account, and confirms that the installation is working correctly.

> [!TIP]
> Use ChatGPT for preliminary planning, discussing approaches, and refining prompts before starting a Codex session. Keeping initial planning separate from repository operations can help reduce unnecessary context processing and preserve Codex usage.

## Overview

1. [Install Codex CLI](#install-codex-cli)
2. [Log into ChatGPT](#log-into-chatgpt)
3. [Verify Installation](#verify-installation)
4. [Related Documentation](#related-documentation)
5. [Official Documentation](#official-documentation)

## Install Codex CLI

> [!IMPORTANT]
> All commands in this guide are executed inside the **Ubuntu WSL environment**, not PowerShell or Command Prompt.

1. Open **Windows Terminal**.

2. Confirm the terminal is running **Ubuntu (WSL)**. If not, click the `⌵` menu and select **Ubuntu**.

3. Install **Codex CLI** using OpenAI's official installation script.

    - `curl -fsSL https://chatgpt.com/codex/install.sh` downloads Codex CLI's installation script over HTTPS.
        - `curl` transfers data from a URL.
        - `-f` causes the command to fail when the server returns an HTTP error.
        - `-s` runs `curl` without the normal progress display.
        - `-S` still displays an error message if the command fails.
        - `-L` follows redirects if the download URL redirects elsewhere.
    - `|` passes the output from the command on its left directly to the command on its right.
    - `sh` executes the downloaded script using the system's POSIX-compatible shell.

    ```bash
    curl -fsSL https://chatgpt.com/codex/install.sh | sh
    ```

4. Close and relaunch **Windows Terminal**.

[🔼 Back to Overview](#overview)

## Log into ChatGPT

1. Launch **Codex CLI**. The first launch begins the setup and authentication process. This is also the command used to start future Codex sessions from within a project directory.

    - `codex` starts the interactive Codex CLI session using the current working directory as its project context.

    ```bash
    codex
    ```

2. Follow the initial configuration prompts, if any.

3. **Select login method:**
    - Sign in with ChatGPT

4. Authenticate in the browser.
    1. Log into your **ChatGPT** account in the browser.
    2. Select a workspace if prompted.
    3. Approve the authorization request or click **Continue** when prompted.

5. Return to **Windows Terminal** after authentication completes.
    - Follow any remaining terminal prompts to continue into Codex CLI.

> [!NOTE]
> To log out of **Codex CLI**, run the following command from the Ubuntu terminal:
>
> ```bash
> codex logout
> ```

[🔼 Back to Overview](#overview)

## Verify Installation

1. Check the installed version of Codex CLI.

    ```bash
    codex --version
    ```

2. Verify that Codex CLI is authenticated. The output should confirm that Codex CLI is signed in using your ChatGPT account.

    ```bash
    codex login status
    ```

[🔼 Back to Overview](#overview)

## Related Documentation

- [Setup Windows for Development](../setup-windows.md) — Follow the complete Windows development environment setup roadmap.

## Official Documentation

- [Codex CLI Documentation](https://learn.chatgpt.com/docs/codex/cli) — Official documentation for installing, configuring, and using Codex CLI for AI-assisted development.
- [Codex Authentication](https://learn.chatgpt.com/docs/auth) — Supported authentication methods and account configuration.
- [Codex CLI Reference](https://developers.openai.com/codex/cli/reference) — Available CLI commands, options, and configuration parameters.
- [Codex GitHub Repository](https://github.com/openai/codex) — Official source code, releases, installation instructions, and issue tracking.

---

[🔼 Back to Getting Started](../README.md)