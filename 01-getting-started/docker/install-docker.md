---
Created: 2026-10-07
Modified: 
---

# Install Docker

Docker is a container platform used to package applications and their dependencies into isolated, portable environments called containers. Containers allow applications and services to run consistently across development, testing, and production environments without requiring their dependencies to be installed directly on the host system.

Docker is useful for software development because it can provide reproducible application environments and run supporting services such as databases, web servers, and development tools without permanently installing or configuring them in Ubuntu. Docker Compose can also define and manage multiple related containers as a single application environment. Most day-to-day Docker tasks are performed through the `docker` CLI, while Docker uses `containerd` behind the scenes as its container runtime.

This guide installs Docker Engine and Docker Compose inside the Ubuntu WSL environment using Docker's official APT repository, verifies that the Docker service is running, and runs a test container to confirm that Docker is working correctly.

## Overview

1. [Install Docker](#install-docker)
2. [Verify Installation](#verify-installation)
3. [Run `hello-world` Image](#run-hello-world-image)

## Install Docker

> [!IMPORTANT]
> All commands in this guide are executed inside the **Ubuntu WSL environment**, not PowerShell or Command Prompt.

1. Open **Windows Terminal**.
2. Confirm the terminal is running **Ubuntu (WSL)**. If not, click the `⌵` menu and select **Ubuntu**.
3. Update Ubuntu’s package lists and install the tools required to securely download packages over HTTPS. Then create the APT keyring directory and add Docker’s official GPG signing key so Ubuntu can verify that Docker packages are authentic.
   - `sudo` (superuser do) executes a command with administrative privileges, which are required to install, update, or remove system packages.
   - `apt update` refreshes Ubuntu's list of available packages and versions. It does not install any updates.
   - `apt install` installs the specified software packages and any required dependencies from Ubuntu's configured repositories. `-y` is an APT command-line option that automatically answers yes to confirmation prompts.
   - `ca-certificates` and `curl` are packages available through Ubuntu's package manager. `ca-certificates` installs trusted Certificate Authority (CA) certificates used to verify the identity of servers when establishing secure HTTPS connections, while `curl` is a command-line tool used to transfer data to and from URLs. Together, they support securely downloading Docker's official GPG signing key.
   - `install -m 0755 -d /etc/apt/keyrings` creates the `/etc/apt/keyrings` directory with permissions that allow it to be read and accessed by the system.
   - `curl -fsSL` downloads Docker's GPG signing key over HTTPS. `-f` causes the command to fail when the server returns an HTTP error. `-s` runs `curl` without the normal progress display. `-S` still displays an error message if the command fails. `-L` follows redirects if the download URL redirects elsewhere.
   - `-o /etc/apt/keyrings/docker.asc` saves the downloaded key to `/etc/apt/keyrings/docker.asc`.
   - `chmod a+r` makes the signing key readable by all users and services that need to verify Docker packages.

```bash
# Add Docker's official GPG key:
sudo apt update
sudo apt install ca-certificates curl -y
sudo install -m 0755 -d /etc/apt/keyrings
sudo curl -fsSL https://download.docker.com/linux/ubuntu/gpg -o /etc/apt/keyrings/docker.asc
sudo chmod a+r /etc/apt/keyrings/docker.asc
```

4. Add Docker’s official APT repository to Ubuntu’s package sources. The repository configuration automatically uses the current Ubuntu release and system architecture, and references Docker’s signing key for package verification.
   - `tee` writes the repository configuration into `/etc/apt/sources.list.d/docker.sources`.
   - `<<EOF` begins a multi-line block of text that is passed to `tee`; the final `EOF` marks the end of that block.
   - `Types: deb` identifies this as a repository containing installable Debian packages.
   - `URIs` specifies the location of Docker's package repository.
   - `Suites` automatically determines the current Ubuntu release name.
   - `Components: stable` selects Docker's stable package channel.
   - `Architectures` automatically detects the system architecture, such as `amd64`.
   - `Signed-By` tells APT which GPG key should be used to verify packages from this repository.

```bash
# Add the repository to APT sources:
sudo tee /etc/apt/sources.list.d/docker.sources <<EOF
Types: deb
URIs: https://download.docker.com/linux/ubuntu
Suites: $(. /etc/os-release && echo "${UBUNTU_CODENAME:-$VERSION_CODENAME}")
Components: stable
Architectures: $(dpkg --print-architecture)
Signed-By: /etc/apt/keyrings/docker.asc
EOF
```

5. Update Ubuntu's package lists to include the packages available from Docker's newly added repository.

```bash
sudo apt update
```

6. Install **Docker Engine**, the Docker CLI, containerd, Buildx, and Docker Compose.
   - `docker-ce` installs Docker Engine, which runs and manages containers.
   - `docker-ce-cli` installs the `docker` command-line interface used to interact with Docker Engine.
   - `containerd.io` installs the container runtime Docker uses behind the scenes to manage container processes and images.
   - `docker-buildx-plugin` adds Docker Buildx for advanced image-building features.
   - `docker-compose-plugin` adds Docker Compose for defining and managing applications made from multiple containers.

```bash
sudo apt install docker-ce docker-ce-cli containerd.io docker-buildx-plugin docker-compose-plugin -y
```

## Verify Installation

1. Check the installed version of Docker.

```bash
docker --version
```

2. Check the installed version of Docker Compose.

```bash
docker compose version
```

3. Verify the Docker service is running. It should start automatically after installation completes.
   - `systemctl` is a command-line tool used to manage and inspect system services controlled by `systemd`, Ubuntu's system and service manager.
   - `status` displays the current state of a service, including whether it is active, inactive, or failed.
   - `docker` specifies the Docker service to inspect.

```bash
sudo systemctl status docker
```

> [!NOTE]
> If Docker is not running, start it manually. The `start` command tells `systemctl` to start the specified service.
>
> ```bash
> sudo systemctl start docker
> ```

## Run `hello-world` Image

1. Download the [hello-world](https://hub.docker.com/_/hello-world) image from Docker Hub and run it in a container. This test image prints a confirmation message to verify that Docker can download an image, create a container, and execute it successfully. This step can be skipped.
   - `docker run` creates and starts a container from the specified image.
   - `hello-world` is the name of the image Docker should use.
   - Docker first checks whether the image is stored locally. If it is not, Docker automatically pulls it from the configured container registry before creating the container.
   
```bash
sudo docker run hello-world
```

## Related Documentation

- [Windows Development Setup](../setup-windows.md)

## Official Documentation

- [Docker Documentation](https://docs.docker.com/)
- [Docker Engine Documentation](https://docs.docker.com/engine/)
- [Install Docker Engine on Ubuntu](https://docs.docker.com/engine/install/ubuntu/)
- [Docker Compose Documentation](https://docs.docker.com/compose/)