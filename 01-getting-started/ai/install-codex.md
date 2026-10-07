---
Created: 2026-10-07
Modified:
---

# Install Codex CLI

Codex CLI is an AI-assisted development tool by OpenAI that runs from the terminal and can work directly with a project's files, source code, and development environment. It can help explain code, make changes, debug issues, navigate a codebase, execute development tasks, and assist with software development workflows from within a project directory.

Codex CLI is separate from the standard ChatGPT chat interface, although both use OpenAI models and consume tokens based on the amount of text and context being processed. Codex is generally more token-intensive than normal chat because coding tasks can require reading project files, maintaining larger amounts of context, using tools, and performing multi-step operations.

Codex CLI can be connected using a ChatGPT account or through OpenAI API billing. When Codex CLI is used through a ChatGPT plan, its usage is tracked separately from normal ChatGPT chat usage, so reaching a Codex usage limit does not prevent continued use of regular ChatGPT chat. When Codex CLI is connected through an OpenAI API key, usage is billed separately on a pay-as-you-go basis according to API pricing.

This guide installs Codex CLI inside the Ubuntu WSL environment, connects it to a ChatGPT account, and verifies that the installation is working correctly.

> [!TIP]
> Use ChatGPT to plan your approach, develop a strategy, and refine prompts before starting a Codex session. Normal ChatGPT usage is separate from Codex usage, so this can help preserve Codex allowance for coding tasks.

## Overview

1. [Install Codex CLI](#install-codex-cli)
2. [Log into ChatGPT](#log-into-chatgpt)
3. [Verify Installation](#verify-installation)

## Install Codex CLI

> [!IMPORTANT]
> All commands in this guide are executed inside the **Ubuntu WSL environment**, not PowerShell or Command Prompt.

1. Open **Windows Terminal**.
2. Confirm the terminal is running **Ubuntu (WSL)**. If not, click the `⌵` menu and select **Ubuntu**.
3. Install **Codex CLI**.
   - `curl -fsSL` downloads Codex CLI's installation script over HTTPS. `-f` causes the command to fail when the server returns an HTTP error. `-s` runs `curl` without the normal progress display. `-S` still displays an error message if the command fails. `-L` follows redirects if the download URL redirects elsewhere.
   - `|` passes the output from the command on its left directly to the command on its right.
   - `sh` executes the downloaded script using the system's POSIX shell.

```bash
curl -fsSL https://chatgpt.com/codex/install.sh | sh
```

4. Close and relaunch **Windows Terminal**.

## Log into ChatGPT

1. Launch **Codex CLI**. The first launch begins the setup and authentication process. This is also the command used to start **Codex CLI** later from within a project directory.

```bash
codex
```

2. Select a text style when prompted.
3. **Select login method:**
   - Sign in with ChatGPT
4. Authenticate in the browser.
   1. Log into your **ChatGPT** account in the browser.
   2. **Select a workspace**.
   3. Click **Continue**.
5. Return to **Windows Terminal** after authentication completes.
   - Press **Enter** to continue.
   
> [!NOTE]
> To log out of **Codex CLI**:
> 
> ```bash
> codex logout
> ```

## Verify Installation

1. Check the installed version of Codex CLI.

```bash
codex --version
```

## Related Documentation

- [Windows Development Setup](../setup-windows.md)

## Official Documentation

- [Codex Documentation](https://developers.openai.com/codex/)