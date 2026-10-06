---
Created: 2026-10-05
Modified:
---

# Install Python

Python is a high-level programming language commonly used for backend development, automation, scripting, data processing, APIs, testing, and many other software development tasks. Installing Python provides the interpreter needed to execute Python scripts and applications from the command line.

pip (Pip Installs Packages) is Python's package manager and is used to install and manage third-party libraries and project dependencies. Python also supports virtual environments, which allow individual projects to maintain isolated sets of packages and dependency versions without modifying the system Python environment.

This guide installs Python, pip, virtual environment support, and system-level development packages commonly required for Python development. These packages provide the interpreter, package management, project isolation, development headers, and compiler tools needed to support both general Python use and projects that depend on native extensions.

## Overview

1. [Install Python](#install-python)
2. [Verify Installation](#verify-installation)

## Install Python

Python provides the runtime used to execute Python scripts and applications, while pip (Pip Installs Packages) manages third-party packages and project dependencies. Virtual environment support is installed so isolated environments can be created later when working on individual projects.

1. Open **Windows Terminal**.

2. Install Python and the supporting packages.

```bash
sudo apt install python3 python3-pip python3-venv python3-dev build-essential python-is-python3 -y
```

> [!NOTE]
> - `python3` is the Python 3 interpreter used to execute Python scripts and applications from the command line. It provides the core runtime needed for Python development.
> - `python3-pip` installs pip, Python's package installer. It is commonly used to install third-party libraries, frameworks, development tools, and project dependencies that are not included with Python itself.
> - `python3-venv` adds support for creating virtual environments. Virtual environments are commonly used to isolate a project's Python packages and dependency versions from the system Python environment and from other projects.
> - `python3-dev` installs Python development headers and supporting files used when Python packages need to compile native extensions.
> - `build-essential` installs common compiler and build tools such as GCC, G++, and `make`. These tools are commonly used when Python packages or other development software need to compile native code during installation.
> - `python-is-python3` allows the `python` command to invoke Python 3. This provides the shorter `python` command while still using the installed Python 3 interpreter.

## Verify Installation

1. Verify Python.

```bash
python --version
```

2. Verify Python 3.

```bash
python3 --version
```

3. Verify pip.

```bash
pip --version
```

## Related Documentation

- [Windows Development Setup](../setup-windows.md)