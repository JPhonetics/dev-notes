---
Created: 2026-10-06
Modified:
---

# Install GitHub

GitHub is a cloud-based platform used to host Git repositories and provide collaboration features such as pull requests, issues, releases, code reviews, and automated workflows. Git continues to manage version control locally, while GitHub provides a remote location for storing and collaborating on repositories.

GitHub CLI (`gh`) provides command-line access to GitHub from the terminal. It can be used to authenticate with GitHub, create and clone repositories, manage pull requests and issues, and perform other GitHub actions without relying entirely on the web interface.

This guide installs GitHub CLI, connects it to a GitHub account, configures Git to authenticate with GitHub over HTTPS, and verifies that the CLI is installed and authenticated correctly.

## Overview

1. [Install GitHub CLI](#install-github-cli)
2. [Verify Installation](#verify-installation)

## Install GitHub CLI

1. Open **Windows Terminal**.
2. Install GitHub CLI.

```bash
sudo apt install gh -y
```

3. Authenticate GitHub CLI with your GitHub account.

```bash
gh auth login
```

4. Follow the prompts:
   1. **What account do you want to log into?** 
      - GitHub.com
   2. **What is your preferred protocol for Git operations on this host?** 
      - HTTPS
   3. **Authenticate Git with your GitHub credentials?**
      - Yes
   4. **How would you like to authenticate GitHub CLI?**
      - Login with a web browser
   5. Copy the one-time code.
   6. Press **Enter** to open [GitHub](https://github.com/login/device) in your browser.
   7. Log into **GitHub** if prompted, or create an account.
   8. Click **Continue**.
   9. Enter the one-time code.
   10. Click **Continue**.
   11. Click **Authorize GitHub**.
5. Confirm authentication completed successfully in **Windows Terminal**.

## Verify Installation

1. Verify GitHub CLI.

```bash
gh --version
```

2. Verify GitHub authentication.

```bash
gh auth status
```

## Official Documentation

- [GitHub CLI Documentation](https://cli.github.com/manual/)
- [GitHub Documentation](https://docs.github.com/)

## Related Documentation

- [Windows Development Setup](../setup-windows.md)