---
Created: 2026-10-07
Modified:
---

# Install Claude Code

Claude Code is an AI-assisted development tool by Anthropic that runs from the terminal and can work directly with a project's files, source code, and development environment. It can help explain code, make changes, debug issues, navigate a codebase, execute development tasks, and assist with software development workflows from within a project directory.

Claude Code is separate from the standard Claude chat interface, although both use Anthropic's Claude models and consume tokens based on the amount of text and context processed. Claude Code is generally more token-intensive than normal chat because coding tasks can require reading project files, maintaining larger amounts of context, using tools, and performing multi-step operations.

Claude Code can be connected in different ways depending on how usage should be billed. When Claude Code is used through an eligible Claude subscription, its usage shares the same subscription allowance as Claude Chat. Because that allowance is shared, reaching the usage limit in Claude Code can also prevent continued use of Claude Chat until the limit resets. When Claude Code is connected through an Anthropic API key, its usage is billed separately on a pay-as-you-go basis and does not consume the Claude subscription allowance.

This guide installs Claude Code inside the Ubuntu WSL environment, verifies that its installation directory is available through `PATH`, connects Claude Code using a Claude subscription, and verifies that the installation is working correctly.

> [!TIP]
> Use Claude Chat to plan your approach, develop a strategy, and refine prompts before starting a Claude Code session. Chat is generally less token-intensive than Claude Code for simple planning tasks, which can help reduce unnecessary consumption of the shared subscription allowance.

## Overview

1. [Install Claude Code](#install-claude-code)
2. [Verify PATH](#verify-path)
3. [Log into Claude Code](#log-into-claude-code)
4. [Verify Installation](#verify-installation)

## Install Claude Code

> [!IMPORTANT]
> All commands in this guide are executed inside the **Ubuntu WSL environment**, not PowerShell or Command Prompt.

1. Open **Windows Terminal**.
2. Confirm the terminal is running **Ubuntu (WSL)**. If not, click the `⌵` menu and select **Ubuntu**.
3. Install **Claude Code**.
   - `curl -fsSL` downloads Claude Code's installation script over HTTPS. `-f` causes the command to fail when the server returns an HTTP error. `-s` runs `curl` without the normal progress display. `-S` still displays an error message if the command fails. `-L` follows redirects if the download URL redirects elsewhere.
   - `|` passes the output from the command on its left directly to the command on its right.
   - `bash` executes the downloaded script using Bash.

```bash
curl -fsSL https://claude.ai/install.sh | bash
```

## Verify PATH

Bash is the command-line shell used by Ubuntu to interpret and execute commands. `PATH` is an environment variable containing a list of directories that Bash searches when a command such as `claude` is entered.

Claude Code installs its executable in `~/.local/bin`. In Ubuntu, `~` represents the current user's home directory, such as `/home/<YOU>`, so this path resolves to `/home/<YOU>/.local/bin`. If that directory is not included as a searchable directory in `PATH`, Bash cannot locate the `claude` command and will return `command not found`.

The `$` symbol is used to expand the value stored in a variable. For example, `$PATH` expands to the list of searchable directories separated by `:`, while `$HOME` expands to the current user's home directory.

1. Check whether `~/.local/bin` is already included in the current `PATH`. If the command returns `/home/<YOU>/.local/bin`, skip to the next section. Otherwise, continue to step 2.
   - `echo $PATH` displays the current `PATH`.
   - `tr ':' '\n'` replaces each colon (`:`) separator with a new line (`\n`) so each directory is displayed separately.
   - `|` passes the output from the command on its left directly to the command on its right.
   - `grep -Fx` searches the resulting list for `"$HOME/.local/bin"`. `-F` treats the search text as a literal string, while `-x` requires the entire line to match.

```bash
echo $PATH | tr ':' '\n' | grep -Fx "$HOME/.local/bin"
```

2. If nothing is returned, we need to make Bash aware of the directory where Claude was installed so `claude` can be launched by name from any working directory. We do this by redefining the current `PATH` to add Claude's install location (`$HOME/.local/bin`) to the beginning of the existing list of directories Bash searches for commands.
   - `echo` prints the specified text.
   - `export` makes the updated `PATH` available to programs launched from the shell.
   - `PATH="..."` defines the new value of the `PATH` environment variable.
   - `$HOME/.local/bin` is where `claude` is installed and is placed at the beginning of the new `PATH` value.
   - `:` separates individual directories inside `PATH`.
   - `$PATH` takes the existing `PATH` value and places it after `$HOME/.local/bin`, preserving the directories that were already configured.
   - `>>` appends the output from `echo` as a new line in `.bashrc` instead of replacing the file.
   - `source ~/.bashrc` reloads `.bashrc` in the current shell so the new `PATH` takes effect immediately.

```bash
echo 'export PATH="$HOME/.local/bin:$PATH"' >> ~/.bashrc
source ~/.bashrc
```

3. Check `PATH` again by re-running the command from step 1. It should now return `/home/<YOU>/.local/bin`.
4. Verify that the configuration was added to `.bashrc`.
   - `grep -nF` searches the file for the specified text. `-n` displays the line number where the text was found. `-F` treats the search text as a literal string instead of a regular expression.
   - `~/.bashrc` is the path to the Bash configuration file being searched.

```bash
grep -nF 'export PATH="$HOME/.local/bin:$PATH"' ~/.bashrc
```

## Log into Claude Code

1. Launch **Claude Code**. The first launch begins the setup and authentication process. This is also the command used to start **Claude Code** later from within a project directory.

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
> To log out of **Claude Code:**
> 
> ```bash
> claude /logout
> ```

## Verify Installation

1. Check the installed version of Claude Code.

```bash
claude --version
```

## Related Documentation

- [Windows Development Setup](../setup-windows.md)

## Official Documentation

- [Claude Code Documentation](https://docs.anthropic.com/en/docs/claude-code/overview)