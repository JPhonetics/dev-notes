---
Created: 2026-10-07
Modified: 2026-11-10
---

# Install Docker

Docker is a container platform used to package applications and their dependencies into isolated, portable environments called containers. Containers allow applications and services to run consistently across development, testing, and production environments without requiring their dependencies to be installed directly on the host system.

Docker is useful for software development because it can provide reproducible application environments and run supporting services such as databases, web servers, and development tools without permanently installing or configuring them in Ubuntu. Docker Compose can also define and manage multiple related containers as a single application environment. Most day-to-day Docker tasks are performed through the `docker` CLI, while Docker uses `containerd` behind the scenes as its container runtime.

This guide installs Docker Engine and Docker Compose inside the Ubuntu WSL2 environment using Docker's official APT repository, verifies that the Docker service is running, and runs a test container to confirm that Docker is working correctly.

> [!IMPORTANT]
> If Docker or containerd was previously installed in Ubuntu, review Docker's official installation prerequisites for conflicting packages before continuing. This guide assumes a fresh Docker Engine installation.

> [!WARNING]
> Avoid running Docker Desktop's WSL integration and a separately installed Docker Engine in the same Ubuntu WSL distribution, as they can conflict. Use one Docker environment or the other.

## Overview

1. [Install Docker](#install-docker)
2. [Verify Installation](#verify-installation)
3. [Run `hello-world` Image](#run-hello-world-image)
4. [Related Documentation](#related-documentation)
5. [Official Documentation](#official-documentation)

## Install Docker

> [!IMPORTANT]
> All commands in this guide are executed inside the **Ubuntu WSL environment**, not PowerShell or Command Prompt.

1. Open **Windows Terminal**.

2. Confirm the terminal is running **Ubuntu (WSL)**. If not, click the `⌵` menu and select **Ubuntu**.

3. Verify that `systemd` is running. Docker uses it to manage the Docker service.

    - `ps -p 1 -o comm=` displays the name of the process running as PID 1.
        - `ps` displays information about running processes.
        - `-p 1` selects the process with PID 1, which is the system's initialization process.
        - `-o comm=` displays only the process name without a column heading.

    ```bash
    ps -p 1 -o comm=
    ```

    The expected output is:

    ```text
    systemd
    ```

    If `systemd` is not running, enable it using the [official WSL instructions](https://learn.microsoft.com/windows/wsl/systemd) before continuing.

4. Update Ubuntu's package lists and install the tools required to securely download packages over HTTPS. Then create the APT keyring directory and add Docker's official GPG signing key so APT can verify packages from Docker's repository.

    - `sudo` (superuser do) executes a command with administrative privileges, which are required to install, update, or remove system packages.
    - `apt update` refreshes Ubuntu's list of available packages and versions. It does not install any updates.
    - `apt install ca-certificates curl -y` installs the required packages.
        - `apt install` installs the specified packages and required dependencies.
        - `ca-certificates` provides trusted Certificate Authority (CA) certificates used to verify HTTPS connections.
        - `curl` is a command-line tool used to transfer data to and from URLs.
        - `-y` automatically answers yes to confirmation prompts.
    - `install -m 0755 -d /etc/apt/keyrings` creates the APT keyring directory with the specified permissions.
        - `install` can create directories and copy files while configuring permissions.
        - `-m 0755` sets permissions allowing the owner to read, write, and enter the directory, while other users can read and enter it.
        - `-d` creates the specified directory.
    - `curl -fsSL https://download.docker.com/linux/ubuntu/gpg -o /etc/apt/keyrings/docker.asc` downloads Docker's official GPG signing key over HTTPS and saves it locally.
        - `-f` causes the command to fail when the server returns an HTTP error.
        - `-s` runs `curl` without the normal progress display.
        - `-S` still displays an error message if the command fails.
        - `-L` follows redirects if the download URL redirects elsewhere.
        - `-o` saves the downloaded content to the specified file.
    - `chmod a+r /etc/apt/keyrings/docker.asc` makes the signing key readable by all users, including services that need to verify Docker packages.
        - `chmod` changes file permissions.
        - `a+r` adds read permission for all users.

    ```bash
    sudo apt update
    sudo apt install ca-certificates curl -y
    sudo install -m 0755 -d /etc/apt/keyrings
    sudo curl -fsSL https://download.docker.com/linux/ubuntu/gpg -o /etc/apt/keyrings/docker.asc
    sudo chmod a+r /etc/apt/keyrings/docker.asc
    ```

5. Add Docker's official APT repository to Ubuntu's package sources. The configuration uses the current Ubuntu release and system architecture and references Docker's signing key for package verification.

    - `sudo tee /etc/apt/sources.list.d/docker.sources <<EOF` writes the repository configuration to a new APT source file.
        - `tee` reads text from standard input and writes it to the specified file.
        - `<<EOF` begins a multi-line text block called a here-document. The final `EOF` marks its end.
    - `Types: deb` identifies the repository as a source of Debian binary packages.
    - `URIs` specifies Docker's official package repository.
    - `Suites` determines the current Ubuntu release codename.
        - `$(...)` executes a command and substitutes its output.
        - `. /etc/os-release` loads operating system information into the shell.
        - `&&` executes the next command only if the preceding command succeeds.
        - `echo` prints the specified text.
        - `${UBUNTU_CODENAME:-$VERSION_CODENAME}` uses the Ubuntu codename when available, otherwise falling back to the general version codename.
    - `Components: stable` selects Docker's stable package channel.
    - `Architectures` selects the system architecture using `dpkg --print-architecture`.
    - `Signed-By` identifies the GPG key APT should use to verify packages from the repository.

    ```bash
    sudo tee /etc/apt/sources.list.d/docker.sources <<EOF
    Types: deb
    URIs: https://download.docker.com/linux/ubuntu
    Suites: $(. /etc/os-release && echo "${UBUNTU_CODENAME:-$VERSION_CODENAME}")
    Components: stable
    Architectures: $(dpkg --print-architecture)
    Signed-By: /etc/apt/keyrings/docker.asc
    EOF
    ```

6. Update Ubuntu's package lists to include packages available from Docker's newly added repository.

    ```bash
    sudo apt update
    ```

7. Install **Docker Engine**, the Docker CLI, containerd, Buildx, and Docker Compose.

    - `docker-ce` installs Docker Engine, which runs and manages containers.
    - `docker-ce-cli` installs the Docker command-line interface used to interact with Docker Engine.
    - `containerd.io` installs the container runtime used by Docker to manage container lifecycles.
    - `docker-buildx-plugin` adds Docker Buildx for advanced image-building features.
    - `docker-compose-plugin` adds Docker Compose for defining and managing applications made from multiple containers.

    ```bash
    sudo apt install docker-ce docker-ce-cli containerd.io docker-buildx-plugin docker-compose-plugin -y
    ```

[🔼 Back to Overview](#overview)

## Verify Installation

1. Check the installed version of Docker.

    ```bash
    docker --version
    ```

2. Check the installed version of Docker Compose.

    ```bash
    docker compose version
    ```

3. Verify that the Docker service is running. It should start automatically after installation completes.

    - `systemctl status docker` displays the current state of the Docker service.
        - `systemctl` is a command-line tool used to manage and inspect services controlled by `systemd`, Ubuntu's system and service manager.
        - `status` displays information about a service, including whether it is active, inactive, or failed.
        - `docker` identifies the service to inspect.

    ```bash
    sudo systemctl status docker
    ```

    The service should report `active (running)`. Press `q` to exit the status display if it opens in a pager.

> [!NOTE]
> If Docker is not running, start it manually using `systemctl start`. The `start` command tells `systemctl` to start the specified    service.
>
> ```bash
> sudo systemctl start docker
> ```

[🔼 Back to Overview](#overview)

## Run `hello-world` Image

1. Download the [hello-world](https://hub.docker.com/_/hello-world) image from Docker Hub and run it in a container. This test verifies that Docker can download an image, create a container, and execute it successfully. This step is optional.

    - `sudo docker run hello-world` creates and starts a container using the `hello-world` image.
        - `sudo` provides the administrative privileges needed to access the Docker daemon.
        - `docker run` creates and starts a container from the specified image.
        - `hello-world` identifies the image Docker should use.
    - Docker checks whether the image is available locally. If it is not, Docker automatically downloads it from the configured container registry before creating the container.

    ```bash
    sudo docker run hello-world
    ```

    The container should display a confirmation message beginning with `Hello from Docker!` and then exit.

[🔼 Back to Overview](#overview)

## Related Documentation

- [Setup Windows for Development](../setup-windows.md) — Follow the complete Windows development environment setup roadmap.

## Official Documentation

- [Docker Documentation](https://docs.docker.com/) — Official documentation for Docker, including containers, images, networking, storage, and development workflows.
- [Install Docker Engine on Ubuntu](https://docs.docker.com/engine/install/ubuntu/) — Official installation instructions for Docker Engine using Ubuntu's APT package manager.
- [Docker Engine Documentation](https://docs.docker.com/engine/) — Documentation for configuring and managing Docker Engine.
- [Docker Compose Documentation](https://docs.docker.com/compose/) — Official documentation for defining and managing multi-container applications.
- [Docker CLI Reference](https://docs.docker.com/reference/cli/docker/) — Reference documentation for Docker commands and their available options.
- [Docker Linux Post-installation](https://docs.docker.com/engine/install/linux-postinstall/) — Optional configuration, permissions, and service management after installation.

---

[🔼 Back to Getting Started](../README.md)