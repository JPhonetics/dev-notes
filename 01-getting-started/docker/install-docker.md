---
Created: 2026-10-07
Modified: 
---

# Install Docker

Docker is a container platform used to package applications and their dependencies into isolated, portable environments called containers. Containers allow applications and services to run consistently across development, testing, and production environments without requiring their dependencies to be installed directly on the host system.

Docker is useful for software development because it can provide reproducible application environments and run supporting services such as databases, web servers, and development tools without permanently installing or configuring them in Ubuntu. Docker Compose can also define and manage multiple related containers as a single application environment.

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

```bash
# Add Docker's official GPG key:
sudo apt update
sudo apt install ca-certificates curl -y
sudo install -m 0755 -d /etc/apt/keyrings
sudo curl -fsSL https://download.docker.com/linux/ubuntu/gpg -o /etc/apt/keyrings/docker.asc
sudo chmod a+r /etc/apt/keyrings/docker.asc
```

4. Add Docker’s official APT repository to Ubuntu’s package sources. The repository configuration automatically uses the current Ubuntu release and system architecture, and references Docker’s signing key for package verification.

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

```bash
sudo apt install docker-ce docker-ce-cli containerd.io docker-buildx-plugin docker-compose-plugin -y
```

## Verify Installation

1. Verify Docker.

```bash
docker --version
```

2. Verify Docker Compose.

```bash
docker compose version
```

3. Verify the Docker service is running. It should start automatically after installation completes.

```bash
sudo systemctl status docker
```

> [!NOTE]
> If Docker is not running, start it manually:
> 
> ```bash
> sudo systemctl start docker
> ```

## Run `hello-world` Image

1. Download the `hello-world` image and run it in a container. This test image prints a confirmation message to verify that Docker can download an image, create a container, and execute it successfully. This step can be skipped.

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