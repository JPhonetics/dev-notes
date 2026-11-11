---
Created: 2026-10-07
Modified: 2026-11-10
---

# Install Claude Code

Claude Code is an AI-assisted development tool developed by Anthropic that runs from the terminal and works directly with a project's files, source code, and development environment. It can help explain code, modify files, debug issues, navigate codebases, execute commands, and assist with software development workflows.

Claude Code is separate from the standard Claude Chat interface, although both use Anthropic's Claude models. Coding tasks can consume substantial amounts of tokens because they may involve reading multiple files, maintaining context, executing tools, and performing multi-step operations. Actual usage depends on the complexity of the task, the model selected, and the amount of context processed.

Claude Code supports different authentication and billing methods:

- **Claude subscription:** Eligible subscription plans include Claude Code access. Claude Chat and Claude Code share the subscription's usage limits, so heavy use of either can affect availability in the other. Depending on the plan and account configuration, additional paid usage may be available after included limits are reached.
- **Anthropic API:** Claude Code can use an Anthropic API key associated with a Claude Console account. API usage is billed separately based on consumption and does not use the Claude subscription allowance.

This guide installs Claude Code inside the Ubuntu WSL environment, verifies that its executable is accessible through `PATH`, authenticates using a Claude subscription, and confirms that the installation is working correctly.

> [!TIP]
> Use Claude Chat for preliminary planning, discussing approaches, and refining prompts before starting a Claude Code session. Keeping initial planning separate from repository operations may help reduce unnecessary context processing and preserve subscription usage.

## Overview

1. [Install Claude Code](#install-claude-code)
2. [Verify PATH](#verify-path)
3. [Log into Claude Code](#log-into-claude-code)
4. [Verify Installation](#verify-installation)
5. [Related Documentation](#related-documentation)
6. [Official Documentation](#official-documentation)

## Install Claude Code

> [!IMPORTANT]
> All commands in this guide are executed inside the **Ubuntu WSL environment**, not PowerShell or Command Prompt.

1. Open **Windows Terminal**.
2. Confirm the terminal is running **Ubuntu (WSL)**. If not, click the `⌵` menu and select **Ubuntu**.
3. Install **Claude Code** using Anthropic's official installation script.

    - `curl -fsSL https://claude.ai/install.sh` downloads Claude Code's installation script over HTTPS.
        - `curl` transfers data from a URL.
        - `-f` causes the command to fail when the server returns an HTTP error.
        - `-s` runs `curl` without the normal progress display.
        - `-S` still displays an error message if the command fails.
        - `-L` follows redirects if the download URL redirects elsewhere.
    - `|` passes the output from the command on its left directly to the command on its right.
    - `bash` executes the downloaded script using Bash.

    ```bash
    curl -fsSL https://claude.ai/install.sh | bash
    ```

[🔼 Back to Overview](#overview)

## Verify PATH

Bash is the command-line shell used by Ubuntu to interpret and execute commands. `PATH` is an environment variable containing a list of directories that Bash searches when a command such as `claude` is entered.

Claude Code installs its executable in `~/.local/bin`. In Ubuntu, `~` represents the current user's home directory, such as `/home/<YOU>`, so this path resolves to `/home/<YOU>/.local/bin`. If that directory is not included as a searchable directory in `PATH`, Bash cannot locate the `claude` command and will return `command not found`.

The `$` symbol is used to expand the value stored in a variable. For example, `$PATH` expands to the list of searchable directories separated by `:`, while `$HOME` expands to the current user's home directory.

1. Check whether Bash can locate the `claude` executable. If the command returns a path such as `/home/<YOU>/.local/bin/claude`, skip to the next section. Otherwise, continue to step 2.

    - `command -v claude` checks whether Bash can resolve the `claude` command.
        - `command` is a Bash built-in used to execute commands or inspect how they are resolved.
        - `-v` displays how the specified command would be resolved without executing it.

    ```bash
    command -v claude
    ```

2. Check whether `~/.local/bin` is already included in the current `PATH`. If the command returns `/home/<YOU>/.local/bin`, the installation directory is already configured. Open a new Ubuntu terminal and repeat step 1. If Claude Code still cannot be found, consult the official troubleshooting documentation. If nothing is returned, continue to step 3.

    - `printf '%s\n' "$PATH"` displays the current `PATH` value followed by a newline.
        - `printf` prints formatted text.
        - `%s` prints the supplied value as a string.
        - `\n` adds a newline.
        - `"$PATH"` expands to the existing list of searchable directories.
    - `tr ':' '\n'` replaces each colon (`:`) separator with a newline (`\n`) so each directory is displayed separately.
    - `grep -Fx "$HOME/.local/bin"` searches the resulting list for the installation directory.
        - `-F` treats the search text as a literal string.
        - `-x` requires the entire line to match.

    ```bash
    printf '%s\n' "$PATH" | tr ':' '\n' | grep -Fx "$HOME/.local/bin"
    ```

3. If nothing is returned, ensure Claude Code's installation directory is configured in `.bashrc`. The following command checks whether the configuration already exists and adds it only if necessary. This makes the directory available in future terminal sessions without replacing the existing `PATH`.

    - `grep -qxF 'export PATH="$HOME/.local/bin:$PATH"' ~/.bashrc` checks whether the configuration already exists.
        - `-q` suppresses output and uses the command's exit status to indicate whether a match was found.
    - `||` executes the command on its right only when the command on its left fails.
    - `echo 'export PATH="$HOME/.local/bin:$PATH"'` prints the configuration line.
        - `echo` prints the specified text.
        - `export` makes the updated `PATH` available to programs launched from the shell.
        - `PATH="..."` assigns a new value to the `PATH` environment variable.
        - `$HOME/.local/bin` is placed at the beginning of the existing `PATH`.
        - `:` separates the new directory from the existing list of directories.
        - `$PATH` preserves the directories already configured.
    - `>> ~/.bashrc` appends the configuration line to `.bashrc` instead of replacing the file.

    ```bash
    grep -qxF 'export PATH="$HOME/.local/bin:$PATH"' ~/.bashrc || echo 'export PATH="$HOME/.local/bin:$PATH"' >> ~/.bashrc
    ```

4. Reload `.bashrc` so the updated `PATH` takes effect in the current terminal session.

    - `source ~/.bashrc` executes the Bash configuration file in the current shell without opening a new terminal.

    ```bash
    source ~/.bashrc
    ```

5. Verify that Bash can now locate the `claude` executable.

    ```bash
    command -v claude
    ```

    The expected output is similar to:

    ```text
    /home/<YOU>/.local/bin/claude
    ```

[🔼 Back to Overview](#overview)

## Log into Claude Code

1. Launch **Claude Code**. The first launch begins the setup and authentication process. This is also the command used to start future Claude Code sessions from within a project directory.

    - `claude` starts the interactive Claude Code session using the current working directory as its project context.

    ```bash
    claude
    ```

2. Select a text style when prompted.

3. **Select login method:**
    - Claude account with subscription

4. Authenticate in the browser.
    1. Log into your Claude account in the browser.
    2. Approve the **Claude Code** authorization request.

5. Return to **Windows Terminal** after authentication completes.
    - In WSL, the browser may display a login code instead of automatically returning to **Claude Code**. If prompted, copy the code from the browser and paste it into the terminal to complete authentication.

> [!NOTE]
> To log out of **Claude Code**, enter `/logout` inside an active Claude Code session.
>
> To exit Claude Code without logging out, enter `/exit` or press `Ctrl+D`.

[🔼 Back to Overview](#overview)

## Verify Installation

1. Check the installed version of Claude Code.

    ```bash
    claude --version
    ```

2. Verify that Claude Code is authenticated. The output should confirm that Claude Code is signed in using your Claude account.

    - `--text` displays the results as readable text instead of JSON.

    ```bash
    claude auth status --text
    ```

[🔼 Back to Overview](#overview)

## Related Documentation

- [Setup Windows for Development](../setup-windows.md) — Follow the complete Windows development environment setup roadmap.

## Official Documentation

- [Claude Code Documentation](https://code.claude.com/docs/en/overview) — Official documentation for installing, configuring, and using Claude Code for AI-assisted development.
- [Claude Code Setup](https://code.claude.com/docs/en/setup) — Installation requirements, setup instructions, and platform-specific configuration.
- [Claude Code Troubleshooting](https://code.claude.com/docs/en/troubleshooting) — Diagnose and resolve installation and configuration issues.

---

[🔼 Back to Getting Started](../README.md)