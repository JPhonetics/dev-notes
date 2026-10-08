---
Created: 2026-10-05
Modified:
---

# Install Python

Python is a high-level programming language commonly used for backend development, automation, scripting, data processing, APIs, testing, and many other software development tasks. Installing Python provides the interpreter needed to execute Python scripts and applications from the command line.

pip (Pip Installs Packages) is Python's package manager and is used to install and manage third-party libraries and project dependencies. Python also supports virtual environments, which allow individual projects to maintain isolated sets of packages and dependency versions without modifying the system Python environment. This is especially important in Ubuntu, where the system Python installation is managed through APT, because virtual environments help prevent project dependencies from interfering with system-managed packages.

This guide installs Python, pip, virtual environment support, and system-level development packages through Ubuntu's Advanced Package Tool (APT). These packages provide the interpreter, package management, project isolation, development headers, and compiler tools needed to support both general Python use and projects that depend on native extensions.

## Overview

1. [Install Python](#install-python)
2. [Verify Installation](#verify-installation)

## Install Python

> [!IMPORTANT]
> All commands in this guide are executed inside the **Ubuntu WSL environment**, not PowerShell or Command Prompt.

1. Open **Windows Terminal**.
2. Confirm the terminal is running **Ubuntu (WSL)**. If not, click the `⌵` menu and select **Ubuntu**.
3. Install Python and its supporting development packages.
   - `sudo` (superuser do) executes a command with administrative privileges, which are required to install, update, or remove system packages.
   - `apt install` installs the specified software packages and any required dependencies from Ubuntu's configured repositories. `-y` is an APT command-line option that automatically answers yes to confirmation prompts.
   - `python3` installs the Python 3 interpreter used to execute Python scripts and applications.
   - `python3-pip` installs pip, Python's package installer. It is used to install third-party libraries, frameworks, development tools, and project dependencies.
   - `python3-venv` adds support for creating virtual environments that isolate a project's Python packages and dependencies from the system Python environment and other projects.
   - `python3-dev` installs Python development headers and supporting files required by packages that compile native extensions, such as C or C++ components.
   - `build-essential` installs common compiler and build tools such as GCC, G++, and `make`, which are used when software needs to compile native code.
   - `python-is-python3` allows the `python` command to invoke Python 3 instead of requiring `python3`.

```bash
sudo apt install python3 python3-pip python3-venv python3-dev build-essential python-is-python3 -y
```

## Verify Installation

1. Check the installed version of Python.

```bash
python --version
```

2. Check the installed version of Python 3.

```bash
python3 --version
```

3. Check the installed version of pip.

```bash
pip --version
```

## Related Documentation

- [Windows Development Setup](../setup-windows.md)

## Official Documentation

- [Python Documentation](https://docs.python.org/3/)
- [pip Documentation](https://pip.pypa.io/en/stable/)
- [Python Virtual Environments](https://docs.python.org/3/library/venv.html)