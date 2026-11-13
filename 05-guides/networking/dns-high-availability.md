---
Created: 2026-11-12
Modified:
---

# DNS High Availability

## ⚠️ The Problem

In my quest to improve the reliability and redundancy of my home network, I wanted to eliminate single points of failure, particularly within my DNS infrastructure.

Originally, I hosted most of my networking services, including OPNsense, AdGuard Home, and the Omada Controller, on a single Proxmox server. Although this setup worked well, rebooting or shutting down the Proxmox host would effectively take down essential network services.

To reduce this dependency, I acquired a Raspberry Pi 4 (2GB) to serve as my primary AdGuard Home instance, while retaining my existing Proxmox LXC instance as the secondary instance. This allows DNS resolution to remain available even when the Proxmox host is offline.

However, running two AdGuard Home instances introduced another issue. When either server was rebooted or became unavailable, some devices experienced significant DNS resolution delays. This happens because clients do not necessarily treat the primary and secondary DNS addresses as an ordered failover pair. Depending on the operating system or device, clients may query either server, retry an unavailable server, or wait for a timeout before attempting another.

## 💡 The Solution

**Keepalived** uses the **Virtual Router Redundancy Protocol (VRRP)** to manage a shared **Virtual IP (VIP)** between both AdGuard Home instances. Each instance retains its own IP address, but only one owns the VIP at a time, providing DNS high availability.

Network clients are configured to use the shared VIP as their DNS server. Under normal operation, the primary AdGuard Home instance owns the VIP and handles DNS requests.

If the primary instance becomes unavailable or fails its health checks, Keepalived automatically transfers the VIP to the secondary instance. Clients continue using the same DNS address, eliminating the need to update their configurations during failover.

When the primary instance recovers, Keepalived is configured to automatically return ownership of the VIP to the primary through preemption and its higher priority. A preemption delay allows the primary to stabilize before reclaiming the VIP, helping prevent unnecessary failover cycles.

Additionally, **AdGuard Home Sync** keeps both instances configured consistently by synchronizing supported settings, filtering rules, and DNS rewrites. This ensures the secondary instance provides the same DNS behavior when it takes over.

## 📋 Prerequisites

1. **Two AdGuard Home instances**
    - Both installed, configured, and independently capable of resolving DNS queries.
    - Preferably hosted on separate physical systems to reduce shared points of failure.
2. **Three IP addresses on the same subnet**
    - One static IP address for each AdGuard Home instance and one additional, unused IP address for the shared VIP.
    - All three addresses must be accessible through the same Layer 2 network.

## 🏠 My Environment

| Component | IP Address | Role |
|---|---|---|
| OPNsense | `172.22.21.1` | Network gateway |
| Virtual IP | `172.22.21.5` | Shared DNS endpoint |
| Raspberry Pi 4 | `172.22.21.6` | Primary DNS |
| Proxmox LXC | `172.22.21.7` | Secondary DNS |

Both AdGuard Home instances and the shared VIP reside on my Server VLAN. Each instance retains its own static IP address, while Keepalived manages ownership of the VIP.

My network has a few additional considerations that may not apply to every deployment:
- **DNS Configuration:** Both AdGuard Home instances are currently configured as DNS servers in OPNsense. This deployment will replace their individual DNS addresses with a single shared VIP.
- **OPNsense, Unbound, and Dnsmasq:** Since I use these services for DNS routing and resolution, I need to verify my DNS forwarding configuration, prevent resolution loops, and update firewall/NAT rules to use the VIP.
- **Static DNS configurations:** Since not all devices obtain DNS settings through DHCP, I need to update manually configured DNS addresses on devices such as Proxmox, Unraid, and other infrastructure to use the VIP.

## Overview

1. [Back Up & Restore AdGuard Home](#back-up--restore-adguard-home)
2. [Update the Operating Systems](#update-the-operating-systems)
3. [AdGuard Home Synchronization](#adguard-home-synchronization)
4. [Keepalived & Virtual IP](#keepalived--virtual-ip)
5. [Health Checks & Automatic Failback](#health-checks--automatic-failback)
6. [OPNsense Configuration](#opnsense-configuration)
7. [Testing & Verification](#testing--verification)
8. [DNS Configuration Migration](#dns-configuration-migration)
9. [Related Documentation](#related-documentation)
10. [Official Documentation](#official-documentation)

> [!TIP]
> For instructions on connecting to remote servers using SSH from Windows, Linux, and macOS, see [Connecting to Remote Servers with SSH](../../01-getting-started/remote-access/ssh.md).
>
> For direct LXC console access, see [Accessing Proxmox VM and LXC Consoles](../proxmox/accessing-proxmox-vm-lxc-consoles.md).
>
> If already logged in as `root`, omit `sudo` where appropriate.

## Back Up & Restore AdGuard Home

Before making changes, back up both AdGuard Home instances to ensure their configurations can be restored if something goes wrong. AdGuard Home stores its configuration in `AdGuardHome.yaml` and persistent data in the `data` directory. Both must be included in the backup.

The backup method depends on how AdGuard Home is deployed. This guide demonstrates a filesystem backup for a Linux installation and a container-level backup using Proxmox LXC. Use the method appropriate for your environment.

> [!IMPORTANT]
> AdGuard Home backups may contain authentication settings, DNS configurations, and other sensitive information. Store them securely and do not upload them to public repositories.

This section covers:

1. [Raspberry Pi — Back Up](#raspberry-pi--back-up)
2. [Raspberry Pi — Restore](#raspberry-pi--restore)
3. [Proxmox LXC — Back Up](#proxmox-lxc--back-up)
4. [Proxmox LXC — Restore](#proxmox-lxc--restore)

### Raspberry Pi — Back Up

1. Connect to the **primary AdGuard Home instance** and identify its installation directory. The default location is `/opt/AdGuardHome`, but this may vary depending on the installation method.

    - `systemctl cat AdGuardHome` displays the AdGuard Home systemd service unit file and any overrides.
        - `systemctl` is a command-line utility used to manage and inspect systemd services.
        - `cat` displays the service configuration files.
        - `AdGuardHome` identifies the service to inspect.
    - `|` passes the output from the command on its left directly to the command on its right.
    - `grep ExecStart` filters the output to the command used to start AdGuard Home.
        - `ExecStart` identifies the executable and any startup arguments.

    ```shell
    systemctl cat AdGuardHome | grep ExecStart
    ```

    Example output:

    ```text
    ExecStart=/opt/AdGuardHome/AdGuardHome "-s" "run"
    ```

2. Verify that the installation directory contains `AdGuardHome.yaml` and the `data` directory before continuing.

    - `ls /opt/AdGuardHome/` lists the contents of the AdGuard Home installation directory.
        - `ls` lists files and directories.
        - `/opt/AdGuardHome/` specifies the directory to inspect.

    ```shell
    ls /opt/AdGuardHome/
    ```

    Example output:

    ```text
    AdGuardHome  AdGuardHome.sig  AdGuardHome.yaml  CHANGELOG.md  data  LICENSE.txt  README.md
    ```

3. Stop AdGuard Home to prevent changes during the backup.
   
    - `sudo systemctl stop AdGuardHome` stops the AdGuard Home service.
        - `sudo` (superuser do) executes the command with administrative privileges.

    ```shell
    sudo systemctl stop AdGuardHome
    ```

4. Create a compressed backup of the AdGuard Home installation directory.

    - `sudo tar -czvf "$HOME/adguardhome-backup.tar.gz" -C /opt AdGuardHome` creates a compressed archive of the installation directory.
        - `tar` (Tape Archive) creates, extracts, and manages archive files.
        - `-c` creates an archive.
        - `-z` compresses the archive using gzip.
        - `-v` displays the archived files.
        - `-f` specifies the archive filename.
        - `"$HOME"` references the current user's home directory to specify the location to save the archive file.
        - `-C /opt` tells `tar` to change directory into `/opt` before archiving.
        - `AdGuardHome` specifies the folder inside `/opt` to include in the archive.

    ```shell
    sudo tar -czvf "$HOME/adguardhome-backup.tar.gz" -C /opt AdGuardHome
    ```

5. Restart AdGuard Home.

    - `sudo systemctl start AdGuardHome` starts the AdGuard Home service.

    ```shell
    sudo systemctl start AdGuardHome
    ```

6. Verify that the backup archive contains the configuration and data.

    - `-t` lists archive contents without extracting them.

    ```shell
    sudo tar -tzf "$HOME/adguardhome-backup.tar.gz"
    ```

    Confirm that the archive includes:

    ```text
    AdGuardHome/
    AdGuardHome/AdGuardHome.yaml
    AdGuardHome/data/
    ```

7. Verify the integrity of the compressed archive. A successful test produces no output and exits without an error.

    - `sudo gzip -t "$HOME/adguardhome-backup.tar.gz"` tests the integrity of the gzip-compressed archive without extracting it.
        - `gzip` compresses and decompresses files.
        - `-t` tests the compressed file for errors.

    ```shell
    sudo gzip -t "$HOME/adguardhome-backup.tar.gz"
    ```

8. Assign ownership of the backup archive to the current user and restrict access before transferring it.

    - `sudo chown "$(id -un):$(id -gn)" "$HOME/adguardhome-backup.tar.gz"` assigns the archive to the current user and their primary group.
        - `chown` changes file ownership.
        - `id -un` returns the current username.
        - `id -gn` returns the current user's primary group.
    - `chmod 600 "$HOME/adguardhome-backup.tar.gz"` restricts access to the archive.
        - `chmod` changes file permissions.
        - `600` grants read and write access only to the owner.

    ```shell
    sudo chown "$(id -un):$(id -gn)" "$HOME/adguardhome-backup.tar.gz"
    chmod 600 "$HOME/adguardhome-backup.tar.gz"
    ```

9. **Recommended:** Copy the backup archive to another computer using SCP (Secure Copy). This ensures the backup remains available if the Raspberry Pi fails. Run the following command from **PowerShell** on your local Windows computer.

    - `scp <USERNAME>@<IP_ADDRESS>:~/adguardhome-backup.tar.gz "$HOME\Downloads"` securely copies the backup archive from the remote user's home directory to the local Downloads folder.
        - `scp` securely transfers files over SSH.
        - `<USERNAME>@<IP_ADDRESS>` identifies the remote SSH account and Raspberry Pi.
        - `:~/adguardhome-backup.tar.gz` identifies the archive in that account's home directory.
        - `"$HOME\Downloads"` specifies the local destination directory.

    ```powershell
    scp <USERNAME>@<IP_ADDRESS>:~/adguardhome-backup.tar.gz "$HOME\Downloads"
    ```

### Raspberry Pi — Restore

Restoring AdGuard Home replaces the current installation with files from an existing backup. The service must be stopped before restoring to prevent configuration changes during the process.

> [!NOTE]
> If the backup is no longer available on the Raspberry Pi but was copied to another computer, transfer it back before proceeding.
>
> From **PowerShell** on your Windows computer:
>
> ```powershell
> scp "$HOME\Downloads\adguardhome-backup.tar.gz" <USERNAME>@<IP_ADDRESS>:~/
> ```

1. Verify that the backup archive is available and contains the expected files.

    ```shell
    sudo tar -tzf "$HOME/adguardhome-backup.tar.gz"
    ```

2. Stop AdGuard Home before restoring the backup.

    ```shell
    sudo systemctl stop AdGuardHome
    ```

> [!CAUTION]
> Verify that the backup archive is valid and `/opt/AdGuardHome` is the correct installation directory. If `/opt/AdGuardHome.pre-restore` already exists, preserve it before continuing.

3. Rename the existing AdGuard Home installation directory to preserve a temporary rollback copy.

    - `sudo mv /opt/AdGuardHome /opt/AdGuardHome.pre-restore` renames the existing installation.
        - `mv` moves or renames files and directories.
        - `/opt/AdGuardHome.pre-restore` is the temporary rollback directory.

    ```shell
    sudo mv /opt/AdGuardHome /opt/AdGuardHome.pre-restore
    ```

4. Extract the backup archive to restore the installation. This restores the original `/opt/AdGuardHome` directory, including its configuration and persistent data.

    - `sudo tar -xzvf "$HOME/adguardhome-backup.tar.gz" -C /opt` extracts the backup into the `/opt` directory.
        - `-x` extracts archived files.
        - `-z` decompresses the gzip archive.
        - `-v` displays extracted files.
        - `-f` specifies the archive filename.
        - `-C /opt` changes to the `/opt` directory before extracting.

    ```shell
    sudo tar -xzvf "$HOME/adguardhome-backup.tar.gz" -C /opt
    ```

5. Restart AdGuard Home.

    ```shell
    sudo systemctl start AdGuardHome
    ```

6. Verify that the service is running and reports `Active: active (running)`.

    - `systemctl status AdGuardHome --no-pager` displays the current service status.
        - `status` displays the service's current state.
        - `--no-pager` displays the output directly without opening a pager.

    ```shell
    systemctl status AdGuardHome --no-pager
    ```

7. Verify DNS resolution and confirm the restored settings in the AdGuard Home web interface.

    - `nslookup google.com <INSTANCE_IP_ADDRESS>` queries the specified AdGuard Home instance to verify DNS resolution.
        - `nslookup` performs DNS lookups.
        - `google.com` is the domain being resolved.
        - `<INSTANCE_IP_ADDRESS>` identifies the AdGuard Home instance being queried.

    ```bash
    nslookup google.com <INSTANCE_IP_ADDRESS>
    ```

### Proxmox LXC — Back Up

Since the secondary AdGuard Home instance runs inside a Proxmox LXC container, we can use the Proxmox web interface to back up the entire container.

The backup includes the container's configuration and supported filesystems. Bind mounts and certain additional mount points may not be included, so verify that AdGuard Home's configuration and data are captured.

1. Open the **Proxmox web interface**.
2. Select the **AdGuard Home LXC container**.
3. Navigate to **Backup**.
4. Click **Backup now**.
5. Configure the backup settings.

    - **Storage:** Select the location to store the backup.
    - **Mode:** Select **Snapshot** if supported, or **Stop** if necessary.
    - **Compression:** Select **ZSTD** if available.

6. Click **Backup** to begin the process.
7. Verify that the backup completes successfully and appears in the backup list.

### Proxmox LXC — Restore

Proxmox allows an LXC container to be restored from a previously created backup using its web interface. This restores the container's backed-up operating system, configuration, and included filesystems.

> [!CAUTION]
> Restoring an existing container can overwrite its current configuration and data. Verify the selected backup and container ID before proceeding.

1. Open the **Proxmox web interface**. 
2. Select the storage location containing the backup.
3. Navigate to **Backups**.
4. Select the AdGuard Home LXC backup.
5. Click **Restore**.
6. Select the appropriate container ID and destination storage. If replacing an existing container, stop it first and enable the appropriate overwrite option.
7. Review the restoration settings and click **Restore**.
8. Verify that the restoration completes successfully, then start the container if necessary.
9. Verify that AdGuard Home is responding to DNS queries and confirm the restored settings in its web interface.

    ```bash
    nslookup google.com <INSTANCE_IP_ADDRESS>
    ```

[🔼 Back to Overview](#overview)

## Update the Operating Systems

Update the operating systems for both AdGuard Home instances. They should already have static IP addresses assigned. In my environment, both are currently configured as separate DNS servers in OPNsense.

Repeat these steps for both instances, updating and verifying one server at a time to minimize service disruptions.

1. Connect to the **primary AdGuard Home instance** and update the operating system's package information and installed packages.

    - `sudo apt update` refreshes package information from the configured repositories without installing updates.
        - `apt` is Debian's package management tool.
        - `update` refreshes available package information.
    - `sudo apt upgrade` installs available package upgrades.
        - `upgrade` upgrades installed packages without removing existing packages.

    ```bash
    sudo apt update
    sudo apt upgrade
    ```

2. Check whether the operating system requires a reboot after upgrading.

    - `if` starts the conditional statement.
    - `[ -f /var/run/reboot-required ]` checks whether a reboot is required.
        - `-f` tests whether the specified path exists and is a regular file.
        - `/var/run/reboot-required` is a file created by supported Debian/Ubuntu package mechanisms when a reboot is required.
    - `echo` displays the corresponding message based on the result.
    - `fi` marks the end of an `if` conditional statement in Bash.

    ```shell
    if [ -f /var/run/reboot-required ]; then
       echo "Reboot required"
    else
       echo "No reboot required"
    fi
    ```

    If a reboot is required, restart the server:

    ```shell
    sudo reboot
    ```

3. Verify that AdGuard Home responds to DNS queries after the update.

    ```shell
    nslookup google.com <INSTANCE_IP_ADDRESS>
    ```

    Example output from my primary AdGuard Home instance:

    ```text
    Server:  AdGuard-Pi
    Address: 172.22.21.6

    Non-authoritative answer:
    Name:    google.com
    Addresses: 2607:f8b0:4023:200f::65
               142.251.210.142
    ```

4. Connect to the **secondary AdGuard Home instance** and repeat Steps 1–3, using its own IP address for DNS verification.

[🔼 Back to Overview](#overview)

## AdGuard Home Synchronization

AdGuard Home Sync runs on the primary instance and copies configuration changes to the secondary instance. The secondary does not require a separate AdGuard Home Sync installation.

The primary instance acts as the **origin**, while the secondary acts as the **replica**. Synchronization is one-way, copying selected configuration changes from the origin to the replica.

Supported settings include DNS upstreams, filtering rules, rewrites, persistent clients, and general settings. Historical query logs and statistics remain independent on each instance.

> [!IMPORTANT]
> Make configuration changes on the primary instance, as synchronized settings on the secondary may be overwritten. Both instances must be installed, configured, and accessible through their web interfaces before proceeding.

1. Connect to the **primary AdGuard Home instance** and identify its processor architecture to download the correct application.

    - `uname -m` displays the machine's processor architecture.
        - `uname` displays operating system and hardware information.
        - `-m` specifies the machine hardware architecture.

    ```shell
    uname -m
    ```

    Example output for a 64-bit Raspberry Pi:

    ```text
    aarch64
    ```

> [!NOTE]
> The installation commands below use the 64-bit ARM release (`arm64`). If your system reports a different architecture, select a compatible release from the [AdGuard Home Sync releases](https://github.com/bakito/adguardhome-sync/releases) instead. Do not install an ARM64 binary on a 32-bit operating system.

2. Create a directory for AdGuard Home Sync.

    - `sudo mkdir -p /opt/adguardhome-sync` creates the installation directory.
        - `mkdir` creates directories.
        - `-p` creates missing parent directories and avoids errors if the directory already exists.
        - `/opt/adguardhome-sync` specifies the installation location.

    ```shell
    sudo mkdir -p /opt/adguardhome-sync
    ```

3. Download the AdGuard Home Sync release compatible with the instance's operating system and processor architecture. Visit the [AdGuard Home Sync releases](https://github.com/bakito/adguardhome-sync/releases) page and copy the download URL for the appropriate release archive.

    - `curl -fL -o /tmp/adguardhome-sync.tar.gz "<DOWNLOAD_URL>"` downloads the selected archive.
        - `curl` transfers data from a URL.
        - `-f` reports HTTP errors.
        - `-L` follows redirects.
        - `-o` specifies the output filename.
        - `<DOWNLOAD_URL>` is the release archive URL copied from the AdGuard Home Sync releases page.

    ```shell
    curl -fL -o /tmp/adguardhome-sync.tar.gz "<DOWNLOAD_URL>"
    ```

> [!NOTE]
> My Raspberry Pi reports `aarch64`, which corresponds to the Linux ARM64 release. Systems reporting `x86_64` typically use the Linux AMD64 release.
>
> This guide was tested with AdGuard Home Sync `v0.9.6`. Review the release notes and archive structure when using a newer version.

4. Extract the downloaded AdGuard Home Sync archive into its installation directory.

    - `-x` extracts files from the archive.

    ```shell
    sudo tar -xzvf /tmp/adguardhome-sync.tar.gz -C /opt/adguardhome-sync
    ```

5. Confirm that the `adguardhome-sync` executable exists before proceeding.

    ```shell
    ls /opt/adguardhome-sync/
    ```

    Example output:

    ```text
    adguardhome-sync  LICENSE  README.md
    ```

6. Create the synchronization configuration file. Scheduled synchronization will remain disabled until the initial manual synchronization succeeds.

    - `sudo nano /opt/adguardhome-sync/adguardhome-sync.yaml` opens a configuration file in the Nano text editor.
        - `nano` is a terminal-based text editor.
        - `/opt/adguardhome-sync/adguardhome-sync.yaml` specifies the configuration file.

    ```shell
    sudo nano /opt/adguardhome-sync/adguardhome-sync.yaml
    ```

    Add the following configuration. Save the configuration in Nano using `Ctrl + O`, press `Enter`, and exit using `Ctrl + X`.

    ```yaml
    cron: ""
    runOnStart: true
    
    origin: {}
    
    replica: {}
    
    api:
      port: 0
       
    features:
      dns:
        accessLists: true
        serverConfig: true
        rewrites: true
    
      dhcp:
        serverConfig: false
        staticLeases: false
    
      generalSettings: true
      protectionStatus: true
      queryLogConfig: true
      statsConfig: true
      clientSettings: true
      services: true
    
      filters:
        blacklist: true
        whitelist: true
        userRules: true
     
      theme: true
      tlsConfig: false
    ```

7. Create an environment file to store connection information and authentication credentials separately from the YAML configuration.

    ```shell
    sudo nano /opt/adguardhome-sync/.env
    ```

    Add the following variables, replacing the placeholders with your actual AdGuard Home web interface credentials and ports. Save the file using `Ctrl + O`, press `Enter`, and exit using `Ctrl + X`.

    ```ini
    ORIGIN_URL="http://<PRIMARY_IP_ADDRESS>:80"
    ORIGIN_USERNAME="<PRIMARY_USERNAME>"
    ORIGIN_PASSWORD="<PRIMARY_PASSWORD>"

    REPLICA1_URL="http://<SECONDARY_IP_ADDRESS>:80"
    REPLICA1_USERNAME="<SECONDARY_USERNAME>"
    REPLICA1_PASSWORD="<SECONDARY_PASSWORD>"
    ```

8. Restrict access to the environment file. The `.env` file contains authentication credentials for both AdGuard Home instances and should only be accessible to authorized users.

    - `sudo chown root:root /opt/adguardhome-sync/.env` assigns ownership of the file to `root`.
        - `chown` changes file ownership.
        - `root:root` specifies the owner and group.
    - `sudo chmod 600 /opt/adguardhome-sync/.env` restricts access to the file.
        - `chmod` changes file permissions.
        - `600` allows only the owner to read and write the file.

    ```shell
    sudo chown root:root /opt/adguardhome-sync/.env
    sudo chmod 600 /opt/adguardhome-sync/.env
    ```

> [!CAUTION]
> Printing the effective AdGuard Home Sync configuration may expose usernames, passwords, and other sensitive settings. Run this command only in a trusted terminal session. Do not share or save the output without removing credentials.

9. Confirm that the effective configuration contains the correct origin and replica addresses and authentication settings. Do not copy actual passwords into documentation or troubleshooting output.

    - `sudo systemd-run --wait --collect --pipe` runs AdGuard Home Sync through a temporary systemd service.
        - `systemd-run` executes a command through systemd.
        - `--wait` waits for the command to finish.
        - `--collect` cleans up the temporary service afterward.
        - `--pipe` displays the output in the terminal.
        - `-p EnvironmentFile=/opt/adguardhome-sync/.env` loads the environment variables.
        - `--config` specifies the YAML configuration file.
        - `run --printConfigOnly` displays the effective configuration without performing synchronization.

    ```shell
    sudo systemd-run --wait --collect --pipe \
        -p EnvironmentFile=/opt/adguardhome-sync/.env \
        /opt/adguardhome-sync/adguardhome-sync \
        --config /opt/adguardhome-sync/adguardhome-sync.yaml \
        run --printConfigOnly
    ```

    Confirm that the **AdGuard Home Sync aggregated config** displays the correct `origin` and `replicas` connection settings.

    ```yaml
    ### AdGuardHome sync aggregated config

    origin:
        url: http://<PRIMARY_IP_ADDRESS>:80
        username: <PRIMARY_USERNAME>
        password: <PRIMARY_PASSWORD>

    replicas:
        - url: http://<SECONDARY_IP_ADDRESS>:80
          username: <SECONDARY_USERNAME>
          password: <SECONDARY_PASSWORD>
    ```

> [!CAUTION]
> Synchronization copies the primary's selected settings to the secondary. Existing settings on the secondary may be modified or removed if they do not exist on the primary. Verify that the primary contains the intended configuration before proceeding.

10. Run the initial synchronization manually. Confirm that `Sync done` appears without authentication or synchronization errors.

    - `run` executes the synchronization using the validated configuration.

    ```shell
    sudo systemd-run --wait --collect --pipe \
        -p EnvironmentFile=/opt/adguardhome-sync/.env \
        /opt/adguardhome-sync/adguardhome-sync \
        --config /opt/adguardhome-sync/adguardhome-sync.yaml \
        run
    ```

    Example output:

    ```text
    Connected to origin
    Start sync
    Connected to replica
    Sync done
    ```

11. Verify that the settings were synchronized successfully. Open both AdGuard Home web interfaces and compare the following settings:

    - DNS rewrites.
    - DNS blocklists and allowlists.
    - Custom filtering rules.
    - Client configurations.
    - Blocked services.

    The secondary instance should now contain the selected settings from the primary.

12. Enable automatic synchronization by updating the configuration file. Save the file using `Ctrl + O`, press `Enter`, and exit using `Ctrl + X`.

    ```shell
    sudo nano /opt/adguardhome-sync/adguardhome-sync.yaml
    ```

    Change:

    ```yaml
    cron: ""
    ```

    To:

    ```yaml
    cron: "*/5 * * * *"
    ```

    This schedules synchronization every five minutes.

    - `*/5` specifies every five minutes.
    - The remaining four `*` fields represent every hour, day of the month, month, and day of the week.

13. Create a systemd service to run AdGuard Home Sync automatically.

    ```shell
    sudo nano /etc/systemd/system/adguardhome-sync.service
    ```

    Add the following service configuration. Save the file using `Ctrl + O`, press `Enter`, and exit using `Ctrl + X`.

    ```ini
    [Unit]
    Description=AdGuard Home Synchronization
    Wants=network-online.target
    After=network-online.target
    
    [Service]
    Type=simple
    EnvironmentFile=/opt/adguardhome-sync/.env
    ExecStart=/opt/adguardhome-sync/adguardhome-sync --config /opt/adguardhome-sync/adguardhome-sync.yaml run
    Restart=on-failure
    RestartSec=10
    
    [Install]
    WantedBy=multi-user.target
    ```

14. Reload systemd and enable the synchronization service.

    - `sudo systemctl daemon-reload` reloads systemd service configuration files.
    - `sudo systemctl enable --now adguardhome-sync` enables the service at startup and starts it immediately.
        - `enable` configures automatic startup.
        - `--now` also starts the service immediately.

    ```shell
    sudo systemctl daemon-reload
    sudo systemctl enable --now adguardhome-sync
    ```

15. Verify the synchronization service reports `Active: active (running)`.

    - `systemctl status adguardhome-sync --no-pager` displays the service status.

    ```shell
    systemctl status adguardhome-sync --no-pager
    ```

16. Review synchronization logs. Confirm that synchronization completes successfully without authentication or connection errors.

    - `sudo journalctl -u adguardhome-sync -n 50 --no-pager` displays recent synchronization logs.
        - `journalctl` reads logs collected by systemd.
        - `-u adguardhome-sync` filters logs to the specified service.
        - `-n 50` displays the last 50 log entries.
        - `--no-pager` prints output directly to the terminal.

    ```shell
    sudo journalctl -u adguardhome-sync -n 50 --no-pager
    ```

17. Verify that both AdGuard Home instances independently resolve DNS queries.

    ```shell
    nslookup google.com <PRIMARY_IP_ADDRESS>
    nslookup google.com <SECONDARY_IP_ADDRESS>
    ```

[🔼 Back to Overview](#overview)

## Keepalived & Virtual IP

Configure **Keepalived** on both AdGuard Home instances to provide a shared virtual IP address using **VRRP (Virtual Router Redundancy Protocol)**.

Keepalived assigns the VIP to one server at a time. If the active server becomes unavailable, the secondary can assume ownership without requiring clients to change their DNS server addresses.

| Device | IP Address | Priority | Role |
|---|---|---|---|
| Shared VIP | `<VIP_IP_ADDRESS>` | — | DNS service address |
| Raspberry Pi | `<PRIMARY_IP_ADDRESS>` | `150` | Preferred primary |
| Proxmox LXC | `<SECONDARY_IP_ADDRESS>` | `100` | Secondary |

> [!NOTE]
> Keepalived instances must be able to exchange VRRP advertisements using IP protocol `112`. Ensure that any applicable firewall rules permit this traffic. No additional rules are required if VRRP communication is already working.

1. Connect to the **primary AdGuard Home instance** and identify its network interface.

    - `ip -br addr` displays a brief summary of network interfaces and their assigned IP addresses.
        - `ip` manages and displays Linux networking information.
        - `-br` displays the output in a brief format.
        - `addr` displays assigned IP addresses.

    ```shell
    ip -br addr
    ```

    Identify the interface containing the instance's static IP address. In this example, the interface is `eth0`.

    ```text
    lo               UNKNOWN        127.0.0.1/8 ::1/128
    eth0             UP             172.22.21.6/24
    ```

2. Install Keepalived on the primary instance.

    - `sudo apt install keepalived` installs Keepalived and its required dependencies.
        - `install` installs the specified package.
        - `keepalived` identifies the package to install.

    ```bash
    sudo apt install keepalived
    ```

3. Create the primary Keepalived configuration.

    ```shell
    sudo nano /etc/keepalived/keepalived.conf
    ```

    Replace the configuration and update placeholders with the appropriate IP addresses and network interface before saving the configuration. Save the file using `Ctrl + O`, press `Enter`, and exit using `Ctrl + X`.

    ```text
    vrrp_instance VI_DNS {
        state BACKUP
        interface <NETWORK_INTERFACE>
        virtual_router_id 51
        priority 150
        advert_int 1

        unicast_src_ip <PRIMARY_IP_ADDRESS>
        unicast_peer {
            <SECONDARY_IP_ADDRESS>
        }
        check_unicast_src

        virtual_ipaddress {
            <VIP_IP_ADDRESS>/32 dev <NETWORK_INTERFACE>
        }
    }
    ```

    **Configuration explanations:**

    - `state BACKUP` allows the servers to elect the active node based on priority.
    - `interface <NETWORK_INTERFACE>` specifies the network interface used by Keepalived. Replace the placeholder with the interface identified on the corresponding instance.
    - `virtual_router_id 51` identifies the VRRP group. Both servers must use the same ID, which must be unique among VRRP groups on the same network.
    - `priority 150` determines which server is preferred. Higher values take precedence.
    - `unicast_src_ip` specifies the local server's IP address.
    - `unicast_peer` specifies the other Keepalived server's IP address.
    - `virtual_ipaddress` defines the shared virtual IP (`172.22.21.5/32`). Both servers must use the same VIP.

4. Validate the primary Keepalived configuration. Valid configuration returns no output. Resolve any reported errors before proceeding.

    - `sudo keepalived -t` checks the configuration for errors without starting the service.
        - `keepalived` runs the Keepalived application.
        - `-t` tests the configuration syntax and performs configuration validation.

    ```shell
    sudo keepalived -t
    ```

5. Connect to the **secondary AdGuard Home instance** and identify its network interface.

    ```shell
    ip -br addr
    ```

    Example output:

    ```text
    lo               UNKNOWN        127.0.0.1/8 ::1/128
    eth0             UP             172.22.21.7/24
    ```

6. Install Keepalived on the secondary instance.

    ```shell
    sudo apt update
    sudo apt install keepalived
    ```

7. Create the Keepalived configuration for the secondary server.

    ```shell
    sudo nano /etc/keepalived/keepalived.conf
    ```

    Replace the configuration and update placeholders with the appropriate IP addresses and network interface before saving the configuration. Save the file using `Ctrl + O`, press `Enter`, and exit using `Ctrl + X`.

    ```text
    vrrp_instance VI_DNS {
        state BACKUP
        interface <NETWORK_INTERFACE>
        virtual_router_id 51
        priority 100
        advert_int 1

        unicast_src_ip <SECONDARY_IP_ADDRESS>
        unicast_peer {
            <PRIMARY_IP_ADDRESS>
        }
        check_unicast_src

        virtual_ipaddress {
            <VIP_IP_ADDRESS>/32 dev <NETWORK_INTERFACE>
        }
    }
    ```

    The secondary instance uses the same VRRP configuration, with three differences:

    - `priority 100` assigns a lower priority than the primary instance.
    - `unicast_src_ip` specifies the secondary instance's IP address.
    - `unicast_peer` specifies the primary instance's IP address.

8. Validate the secondary Keepalived configuration. Valid configuration returns no output. Resolve any reported errors before proceeding.

    ```shell
    sudo keepalived -t
    ```

9. Start and enable Keepalived on both servers, beginning with the primary Raspberry Pi.

    ```shell
    sudo systemctl enable --now keepalived
    ```

    Repeat the command on the secondary Proxmox LXC.

10. Verify that Keepalived is running on both servers. The service should report `Active: active (running)`.

    ```shell
    systemctl status keepalived --no-pager
    ```

11. Verify that the virtual IP is assigned to the primary server. The VIP should appear on only one server at a time.

    ```shell
    ip -br addr
    ```

    Expected output on the Raspberry Pi:

    ```text
    eth0             UP             172.22.21.6/24 172.22.21.5/32
    ```

    Expected output on the Proxmox LXC:

    ```text
    eth0             UP             172.22.21.7/24
    ```

12. Review Keepalived logs on both servers. Look for state transitions indicating which server is **MASTER** and which is **BACKUP**, as well as any permission, interface, or advertisement errors.

    ```shell
    sudo journalctl -u keepalived -n 50 --no-pager
    ```

13. Verify that AdGuard Home can answer DNS queries through the virtual IP. Execute this command on your local machine.

    ```shell
    nslookup google.com <VIP_IP_ADDRESS>
    ```

    A successful response confirms that the virtual IP is reachable and that the active AdGuard Home instance is responding to DNS queries. `Server: UnKnown` is normal when the DNS server does not have a reverse DNS (PTR) record for its IP address. It does not indicate a DNS failure.

    ```text
    Server:  UnKnown
    Address: 172.22.21.5
    
    Non-authoritative answer:
    Name:    google.com
    Addresses: 2607:f8b0:4005:809::200e
               142.251.218.142
    ```

[🔼 Back to Overview](#overview)

## Health Checks & Automatic Failback

Configure Keepalived to monitor DNS availability and automatically transfer the virtual IP when the active AdGuard Home instance becomes unhealthy.

The primary Raspberry Pi remains the preferred instance. After recovering from a failure, it automatically reclaims the VIP following a 60-second preemption delay.

1. Connect to the **primary AdGuard Home instance** and install the DNS utilities required by the health-check script.

    - `sudo apt install dnsutils` installs DNS troubleshooting utilities, including `nslookup`.

    ```shell
    sudo apt install dnsutils
    ```

2. Create a DNS health-check script that verifies resolution through the local AdGuard Home instance and its configured upstream resolver.

    - `sudo nano /usr/local/sbin/check-adguard-dns.sh` creates a shell script for monitoring DNS availability.

    ```shell
    sudo nano /usr/local/sbin/check-adguard-dns.sh
    ```

    Add the following script. Save the file using `Ctrl + O`, press `Enter`, and exit using `Ctrl + X`.

    ```bash
    #!/bin/bash
   
    SERVER_IP="$1"
   
    if nslookup -timeout=2 -retry=1 google.com "$SERVER_IP" > /dev/null 2>&1; then
        exit 0
    else
        exit 1
    fi
    ```

    Script explanations:

    - `#!/bin/bash` identifies Bash as the script interpreter.
    - `SERVER_IP="$1"` accepts the server IP address supplied by Keepalived.
    - `nslookup` queries the local AdGuard Home instance to verify DNS resolution.
        - `-timeout=2` sets a two-second timeout for DNS responses.
        - `-retry=1` limits retries.
        - `google.com` is the domain being resolved.
        - `"$SERVER_IP"` specifies the DNS server being tested.
    - `> /dev/null 2>&1` suppresses normal output and error messages.
    - `exit 0` indicates a successful health check.
    - `exit 1` indicates a failed health check.

3. Assign ownership and executable permissions to the script.

    - `sudo chown root:root /usr/local/sbin/check-adguard-dns.sh` assigns the script to `root`.
    - `sudo chmod 755 /usr/local/sbin/check-adguard-dns.sh` allows the script to be executed.
        - `755` grants the owner read, write, and execute permissions and other users read and execute permissions.

    ```shell
    sudo chown root:root /usr/local/sbin/check-adguard-dns.sh
    sudo chmod 755 /usr/local/sbin/check-adguard-dns.sh
    ```

4. Test the health-check script against the primary AdGuard Home instance. An exit code of `0` indicates success, while a nonzero value indicates failure.

    - `sudo /usr/local/sbin/check-adguard-dns.sh <PRIMARY_IP_ADDRESS>` runs the health check against the primary instance.
    - `echo $?` displays the exit status of the previous command.
        - `$?` contains the exit status of the most recently executed command.
        - `0` indicates success, while a nonzero value indicates failure.

    ```shell
    sudo /usr/local/sbin/check-adguard-dns.sh <PRIMARY_IP_ADDRESS>
    echo $?
    ```

5. Update Keepalived on the primary AdGuard Home instance.

    ```shell
    sudo nano /etc/keepalived/keepalived.conf
    ```

    Replace the current config with this. Save the file using `Ctrl + O`, press `Enter`, and exit using `Ctrl + X`.

    ```text
    global_defs {
        script_user root
        enable_script_security
    }

    vrrp_script chk_dns {
        script "/usr/local/sbin/check-adguard-dns.sh <PRIMARY_IP_ADDRESS>"
        interval 5
        timeout 4
        weight 0
        fall 2
        rise 3
        user root
    }
   
    vrrp_instance VI_DNS {
        state BACKUP
        interface <NETWORK_INTERFACE>
        virtual_router_id 51
        priority 150
        advert_int 1
        preempt_delay 60
   
        unicast_src_ip <PRIMARY_IP_ADDRESS>
        unicast_peer {
            <SECONDARY_IP_ADDRESS>
        }
        check_unicast_src
   
        virtual_ipaddress {
            <VIP_IP_ADDRESS>/32 dev <NETWORK_INTERFACE>
        }
    
        track_script {
            chk_dns
        }
    }
    ```

    Configuration explanations:

    - `global_defs` defines global Keepalived settings.
    - `script_user root` specifies the default user for executing health-check scripts.
    - `enable_script_security` prevents scripts executed with elevated privileges from being writable by unauthorized users.
    - `vrrp_script chk_dns` defines a health check named `chk_dns`.
    - `script` specifies the script and local IP address to test.
    - `interval 5` executes the health check every five seconds.
    - `timeout 4` limits each health-check execution to four seconds.
    - `weight 0` causes the VRRP instance to enter `FAULT` after the configured number of consecutive failures.
    - `fall 2` requires two consecutive failures before marking the server unhealthy.
    - `rise 3` requires three consecutive successful checks before marking it healthy again.
    - `user root` executes the health-check script as root.
    - `track_script` associates the health check with the VRRP instance.
    - `preempt_delay 60` delays the primary's takeover when a healthy, lower-priority server is already active.

> [!NOTE]
> The 60-second preemption delay is additional to VRRP's normal election timing. It is not an exact 60-second recovery timer, and it does not delay the secondary's takeover when the primary fails.

6. Validate the primary Keepalived configuration. An exit code of `0` indicates successful validation.

    ```shell
    sudo keepalived -t
    echo $?
    ```

7. Connect to the **secondary AdGuard Home instance** and install the required DNS utilities.

    ```bash
    sudo apt install dnsutils
    ```

8. Create the same health-check script on the secondary instance and update the permissions.

    ```shell
    sudo nano /usr/local/sbin/check-adguard-dns.sh
    ```

    Copy the same script from Step 2, then apply the same permissions:
 
    ```shell
    sudo chown root:root /usr/local/sbin/check-adguard-dns.sh
    sudo chmod 755 /usr/local/sbin/check-adguard-dns.sh
    ```

9. Test the health-check script against the secondary AdGuard Home instance. An exit code of `0` indicates success.

    ```shell
    sudo /usr/local/sbin/check-adguard-dns.sh <SECONDARY_IP_ADDRESS>
    echo $?
    ```

10. Update Keepalived on the secondary AdGuard instance.

    ```shell
    sudo nano /etc/keepalived/keepalived.conf
    ```
   
    Replace the current config with this. Save the file using `Ctrl + O`, press `Enter`, and exit using `Ctrl + X`.

    ```text
    global_defs {
        script_user root
        enable_script_security
    }
 
    vrrp_script chk_dns {
        script "/usr/local/sbin/check-adguard-dns.sh <SECONDARY_IP_ADDRESS>"
        interval 5
        timeout 4
        weight 0
        fall 2
        rise 3
        user root
    }
    
    vrrp_instance VI_DNS {
        state BACKUP
        interface <NETWORK_INTERFACE>
        virtual_router_id 51
        priority 100
        advert_int 1
   
        unicast_src_ip <SECONDARY_IP_ADDRESS>
        unicast_peer {
            <PRIMARY_IP_ADDRESS>
        }
        check_unicast_src
   
        virtual_ipaddress {
            <VIP_IP_ADDRESS>/32 dev <NETWORK_INTERFACE>
        }
   
        track_script {
            chk_dns
        }
    }
    ```

    The secondary uses the same health-check settings but tests its own DNS address. Its lower priority allows the primary to reclaim ownership after recovery. The secondary does not require the 60-second preemption delay.

11. Validate the secondary Keepalived configuration. An exit code of `0` indicates successful validation.

    ```shell
    sudo keepalived -t
    echo $?
    ```

12. Restart Keepalived on both instances to apply the updated configurations, beginning with the **secondary AdGuard Home instance** and then the **primary AdGuard Home instance**.

    - `sudo systemctl restart keepalived` restarts the Keepalived service and loads the updated configuration.

    ```shell
    sudo systemctl restart keepalived
    ```

13. Verify that Keepalived is running on both servers. The service should report `Active: active (running)`.

    ```shell
    systemctl status keepalived --no-pager
    ```

14. On both AdGuard Home instances, verify that the primary owns the virtual IP. The VIP should appear on only one server at a time.

    ```shell
    ip -br addr
    ```

    Expected output on the **primary AdGuard Home server**:

    ```text
    eth0    UP    172.22.21.6/24 172.22.21.5/32
    ```

    Expected output on the **secondary AdGuard Home server**:

    ```text
    eth0    UP    172.22.21.7/24
    ```

[🔼 Back to Overview](#overview)

## OPNsense Configuration

Integrate the shared DNS Virtual IP into OPNsense while preserving the existing Dnsmasq, Unbound, and firewall configurations.

OPNsense previously redirected DNS traffic to the individual AdGuard Home addresses. With Keepalived configured, DNS traffic should instead target the shared VIP, allowing Keepalived to determine which server handles requests.

> [!IMPORTANT]
> Updating the DHCP DNS-server option changes the DNS address advertised to clients when they obtain or renew their DHCP leases.
>
> Before applying this change, verify that the shared VIP is assigned to the active AdGuard Home instance and successfully resolves DNS queries.
>
> Existing clients may continue using their previous DNS settings until their DHCP leases are renewed.

> [!WARNING]
> Your infrastructure may differ from this example. Adjust VLANs, interfaces, IP addresses, ports, and firewall rules to match your environment.

This section covers:

1. [Back Up OPNsense](#back-up-opnsense)
2. [Create Firewall Aliases](#create-firewall-aliases)
3. [Destination NAT](#destination-nat)
4. [Firewall Rules](#firewall-rules)
5. [Dnsmasq](#dnsmasq)

### Back Up OPNsense

> [!IMPORTANT]
> Back up your OPNsense configuration before modifying NAT or firewall rules.

1. Open the **OPNsense web interface**.
2. Navigate to **System → Configuration → Backups**.
3. Click **Download configuration**.
4. Save the backup somewhere secure.

### Create Firewall Aliases

Firewall aliases are reusable collections of IP addresses, networks, and ports. They allow us to update multiple firewall or NAT rules by modifying a single alias instead of editing each rule individually.

1. Navigate to **Firewall → Aliases**.
2. Click `+` to add a new alias.
3. Create the aliases below.
4. Click **Save** after creating each alias.
5. Click **Apply**.

| Enabled | Name | Type | Content | Description |
|---|---|---|---|---|
| ✅ | `DNS_ALL` | Host(s) | `<ALL_DNS_IP_ADDRESSES>` | All DNS IPs |
| ✅ | `DNS_Servers` | Host(s) | `<DNS_SERVER_IP_ADDRESSES>` | DNS Servers |
| ✅ | `DNS_VIP` | Host(s) | `<VIP_IP_ADDRESS>` | DNS VIP |
| ✅ | `Ports_DNS_Dnsmasq` | Port(s) | `<DNSMASQ_DNS_PORT>` | Dnsmasq |
| ✅ | `Ports_DNS_Unbound` | Port(s) | `<UNBOUND_DNS_PORT>` | Unbound |

> [!IMPORTANT]
> - `DNS_ALL` should include all 3 IP addresses.
> - `DNS_Servers` should only be the 2 server IP addresses.
> - `DNS_VIP` should only be the DNS VIP IP Address.
> - Don't add `Ports_DNS_Dnsmasq` or `Ports_DNS_Unbound` if you don't use these services.

### Destination NAT

Destination NAT redirects DNS requests from internal clients to the AdGuard Home VIP, even when clients attempt to use another DNS resolver.

The redirect rule excludes requests already destined for the VIP. This prevents OPNsense from unnecessarily rewriting those requests.

An optional No RDR rule allows selected administrative devices to bypass DNS redirection when explicitly querying the individual AdGuard servers.

1. Navigate to **Firewall → NAT → Destination NAT**.
2. Click `+` to add a new NAT rule.
3. Create the DNS redirect rule and, if required, the optional bypass rule using the settings below.
4. Click **Save** after creating each rule.
5. If using the optional **No RDR** bypass rule, position it above the general DNS redirect rule so matching traffic is excluded from redirection.
6. Leave the changes pending until the firewall rules have been configured. Do not click **Apply** yet.

| Setting | Rule #1 | Rule #2 (Optional) |
|---|---|---|
| Enabled | ✅ | ✅ |
| Description | Redirect DNS to VIP | Bypass DNS Redirect |
| Interface | `Internal VLANs` | `Internal VLANs` |
| TCP/IP Version | IPv4 | IPv4 |
| Protocol | TCP/UDP | TCP/UDP |
| Source | Any | `<YOUR_DEVICES_ALIAS>` |
| Source Port | Any | Any |
| Destination / Invert | ✅ | ❌ |
| Destination | `DNS_VIP` | `DNS_Servers` |
| Destination Port | `53` | `53` |
| Redirect Target IP | `DNS_VIP` | Any |
| Redirect Target Port | `53` | `53` |
| No RDR (NOT) | ❌ | ✅ |
| Firewall rule | Manual | Manual |

> [!IMPORTANT]
> `Destination / Invert = ✅` with `Destination = DNS_VIP` means the redirect matches destinations **other than the VIP**.
>
> The redirect target must resolve to only `172.22.21.5`. Using a multi-address alias as the redirect target can cause traffic to be distributed between individual servers, bypassing Keepalived.
>
> Ensure the redirect does not interfere with traffic between AdGuard and Unbound or other DNS infrastructure.

> [!NOTE]
> Rule #2 is optional. It prevents destination NAT for matching requests but does not automatically authorize them through the firewall. Existing firewall policy must also permit the selected devices to query `DNS_Servers` on port `53`.

### Firewall Rules

Firewall rules allow authorized clients to reach the DNS VIP and permit communication with the DNS services used by the deployment.

Rule #1 allows DNS access to the VIP. Rule #2 permits access to Dnsmasq when required, while Rule #3 permits AdGuard Home to forward queries to Unbound.

Review existing firewall rules before creating new ones to avoid unnecessary duplication.

1. Navigate to **Firewall → Rules**.
2. Click `+` to add a new firewall rule.
3. Create the firewall rules below.
4. Click **Save** after creating each rule.
5. Position the rules above any existing rules that would block the required DNS traffic.
6. Leave the changes pending until the Dnsmasq configuration is complete. Do not click **Apply** yet.

| Setting | Rule #1 | Rule #2 (Optional) | Rule #3 (Optional) |
|---|---|---|---|
| Enabled | ✅ | ✅ | ✅ |
| Description| Allow DNS to DNS VIP | Allow Servers to Dnsmasq | Allow Servers to Unbound |
| Interface | `Internal VLANs` | `Server VLAN` | `Server VLAN` |
| Quick | ✅ | ✅ | ✅ |
| Action | Pass | Pass | Pass |
| Direction | In | In | In |
| TCP/IP Version | IPv4 | IPv4 | IPv4 |
| Protocol | TCP/UDP | TCP/UDP | TCP/UDP |
| Source | Any | `DNS_Servers` | `DNS_Servers` |
| Source Port | Any | Any | Any |
| Destination | `DNS_VIP` | `This Firewall` | `This Firewall` |
| Destination Port | `53` | `Ports_DNS_Dnsmasq` | `Ports_DNS_Unbound` |
| Gateway | Default | Default | Default |

### Dnsmasq

Configure Dnsmasq to provide a local hostname for the DNS VIP and advertise the VIP to DHCP clients.

1. Navigate to **Services → Dnsmasq DNS & DHCP → Hosts**.
2. Click `+` to add a new host.
3. Create the host using the following settings.

    | Setting | Value |
    |---|---|
    | Host | `dns-vip` |
    | IP addresses | `<VIP_IP_ADDRESS>` |

4. Click **Save**.
5. Click **Apply**.
6. Navigate to **Services → Dnsmasq DNS & DHCP → DHCP Options**.
7. Click `+` to add a new DHCP option.
8. Create the DHCP option using the following settings.

    | Setting | Value |
    |---|---|
    | Interface | Any |
    | Type | Set |
    | Option | dns-server[6] |
    | Value | `<VIP_IP_ADDRESS>` |
    | Description | DNS Server for DHCP |

9. Click **Save**.
10. Click **Apply**.
11. Navigate back to **Destination NAT** and **Firewall Rules** and apply any pending changes.

[🔼 Back to Overview](#overview)

## Testing & Verification

Verify that the shared DNS VIP operates correctly across the network and that Keepalived automatically transfers ownership during failures and recovery. Brief DNS interruptions may occur during failover and failback. The goal is automatic recovery without requiring clients to change DNS server addresses.

Use the following checklist to track verification progress:

- [ ] Confirm that only the primary instance owns the VIP during normal operation and that DNS resolution works.
- [ ] Monitor DNS availability to identify interruptions during failover and recovery.
- [ ] Verify that the secondary assumes VIP ownership when the primary AdGuard Home service fails.
- [ ] Confirm that the primary automatically reclaims the VIP after recovery.
- [ ] Verify that the secondary assumes VIP ownership when Keepalived stops on the primary.
- [ ] Test failover during a complete primary host failure (optional).
- [ ] Complete final verification of DNS resolution, VLAN access, DHCP settings, filtering, synchronization, and Keepalived logs.

This section covers:

1. [Initial DNS Verification](#initial-dns-verification)
2. [DNS Availability Monitoring](#dns-availability-monitoring)
3. [AdGuard Home Failover](#adguard-home-failover)
4. [Automatic Failback](#automatic-failback)
5. [Keepalived Failover](#keepalived-failover)
6. [Full Host Failure (Optional)](#full-host-failure-optional)
7. [Final Verification](#final-verification)

### Initial DNS Verification

Verify that the primary AdGuard Home instance owns the VIP and that DNS queries resolve successfully before testing failover.

1. On **both AdGuard Home instances**, verify that only the primary owns the VIP before beginning the test.

    ```bash
    ip -br addr
    ```

    Expected output on the primary:

    ```text
    eth0    UP    172.22.21.6/24 172.22.21.5/32
    ```

    Expected output on the secondary:

    ```text
    eth0    UP    172.22.21.7/24
    ```

2. On your local computer, verify that DNS queries resolve through the VIP. A successful response confirms that the VIP is reachable and the active AdGuard Home instance can resolve DNS queries.

    ```shell
    nslookup google.com <VIP_IP_ADDRESS>
    ```

    Example output:

    ```text
    Server:  UnKnown
    Address: 172.22.21.5

    Non-authoritative answer:
    Name:    google.com
    Addresses: 2607:f8b0:4005:809::200e
               142.251.218.142
    ```

> [!NOTE]
> `Server: UnKnown` may appear when the DNS VIP does not have a reverse DNS (PTR) record. This does not indicate a DNS resolution failure.
>
> Configuring a forward hostname in Dnsmasq or advertising the VIP through DHCP does not automatically guarantee that the hostname will appear in `nslookup`.

### DNS Availability Monitoring

Monitor DNS resolution continuously to observe interruptions during failover and recovery.

1. On your local Windows computer, open a separate PowerShell window and start a continuous DNS query.

    - `while ($true)` continuously repeats the enclosed commands.
        - `$true` keeps the loop running until manually stopped.
    - `Write-Host` displays information in the terminal.
        - `$(Get-Date -Format 'HH:mm:ss')` displays the current time in hours, minutes, and seconds.
    - `Start-Sleep -Seconds 2` pauses execution for two seconds between queries.

    ```powershell
    while ($true) {
        Write-Host "$(Get-Date -Format 'HH:mm:ss')"
        nslookup google.com <VIP_IP_ADDRESS>
        Start-Sleep -Seconds 2
    }
    ```

2. Keep the PowerShell window running throughout the remaining failover tests. Watch for DNS timeouts, failed queries, and successful responses after recovery. Press `Ctrl + C` to stop monitoring when testing is complete.

> [!NOTE]
> This monitoring command uses PowerShell. On Linux or macOS, an equivalent shell loop can be used to repeatedly query the VIP.

### AdGuard Home Failover

Verify that Keepalived transfers the VIP when the primary AdGuard Home service becomes unavailable.

1. Connect to the **primary AdGuard Home instance** and stop the AdGuard Home service without stopping Keepalived. Keepalived should detect the failed DNS health checks and release the VIP after the configured failure threshold.

    ```shell
    sudo systemctl stop AdGuardHome
    ```

2. On the **secondary AdGuard Home instance**, verify that the VIP has transferred.

    ```shell
    ip -br addr
    ```

    Example output from my secondary instance:

    ```text
    eth0    UP    172.22.21.7/24 172.22.21.5/32
    ```

3. Verify that DNS resolution resumes through the VIP. Confirm that the continuous DNS monitoring in PowerShell resumes successfully without requiring any changes to the DNS server address.

    ```shell
    nslookup google.com <VIP_IP_ADDRESS>
    ```
### Automatic Failback

Verify that the primary AdGuard Home instance automatically reclaims the VIP after its DNS service recovers.

1. On the **primary AdGuard Home instance**, restart AdGuard Home. Keepalived should detect successful health checks and allow the primary to reclaim the VIP after the configured preemption delay.

    ```shell
    sudo systemctl start AdGuardHome
    ```

2. On both AdGuard Home instances, verify VIP ownership.

    ```shell
    ip -br addr
    ```

    Expected output on the **primary AdGuard**:

    ```text
    eth0    UP    172.22.21.6/24 172.22.21.5/32
    ```

    Expected output on the **secondary AdGuard**:

    ```text
    eth0    UP    172.22.21.7/24
    ```

3. Verify that DNS queries continue resolving through the VIP after failback.

    ```shell
    nslookup google.com <VIP_IP_ADDRESS>
    ```

> [!NOTE]
> The configured `preempt_delay 60` delays the primary's takeover. The total recovery time may exceed 60 seconds because of health-check intervals, recovery thresholds, and VRRP election timing.

### Keepalived Failover

Verify that the secondary AdGuard Home instance assumes ownership when Keepalived stops on the primary.

1. Confirm that the primary AdGuard Home instance owns the VIP before beginning the test.

    ```shell
    ip -br addr
    ```

2. On the **primary AdGuard Home instance**, stop Keepalived. Keepalived should release its managed VIP, allowing the secondary instance to assume ownership.

    ```shell
    sudo systemctl stop keepalived
    ```

3. On the **secondary AdGuard Home instance**, verify that the VIP has transferred.

    ```shell
    ip -br addr
    ```

    Expected output:
 
    ```text
    eth0    UP    172.22.21.7/24 172.22.21.5/32
    ```

4. Verify that DNS queries continue resolving through the VIP.

    ```shell
    nslookup google.com <VIP_IP_ADDRESS>
    ```

5. On the **primary AdGuard Home instance**, restart Keepalived.

    ```shell
    sudo systemctl start keepalived
    ```

6. Confirm that the primary instance eventually reclaims the VIP and that the secondary releases it. Verify that continuous DNS monitoring resumes normally after failback.

    ```shell
    ip -br addr
    ```

### Full Host Failure (Optional)

Test the complete failure of the host running the primary AdGuard Home instance rather than stopping an individual service.

> [!CAUTION]
> This test intentionally shuts down the primary host. Ensure that you have a way to restore power and regain access before proceeding. Do not perform this test if the host runs other critical services that cannot tolerate an interruption.

1. Confirm that the primary AdGuard Home instance owns the VIP and DNS resolution is working.

    ```shell
    ip -br addr
    ```

2. Connect to the host running the **primary AdGuard Home instance** and shut it down safely.

    - `sudo shutdown -h now` initiates an immediate operating system shutdown.
        - `shutdown` schedules a system shutdown.
        - `-h` halts or powers off the system.
        - `now` begins the shutdown immediately.

    ```shell
    sudo shutdown -h now
    ```

3. On the **secondary AdGuard Home instance**, verify that it assumes ownership of the VIP.

    ```shell
    ip -br addr
    ```

    Example output from my secondary instance:

    ```text
    eth0    UP    172.22.21.7/24 172.22.21.5/32
    ```

4. Verify that DNS queries resolve through the VIP while the primary host remains offline. Confirm that the continuous DNS monitoring resumes successfully through the secondary instance.

    ```shell
    nslookup google.com <VIP_IP_ADDRESS>
    ```

5. Power on the primary host and allow its operating system, AdGuard Home, and Keepalived services to start.

6. On both AdGuard Home instances, verify that the VIP automatically returns to the primary after recovery. Confirm that the primary owns the VIP and the secondary no longer advertises it.

    ```shell
    ip -br addr
    ```

7. Verify that DNS resolution remains available after failback.

    ```shell
    nslookup google.com <VIP_IP_ADDRESS>
    ```

### Final Verification

After completing the failover tests, verify that the high-availability configuration works correctly for normal network clients.

1. Verify DNS resolution from each authorized client VLAN. Confirm that clients can resolve DNS through the VIP without timeouts or connection refusals.

    ```shell
    nslookup google.com <VIP_IP_ADDRESS>
    ```

2. If the optional **No RDR** bypass rule is configured, verify that an authorized administrative device can query both AdGuard Home instances directly without being redirected to the VIP.

    ```shell
    nslookup google.com <PRIMARY_IP_ADDRESS>
    nslookup google.com <SECONDARY_IP_ADDRESS>
    ```

    Confirm that each query appears in the corresponding AdGuard Home instance's query log.

> [!NOTE]
> Normal `nslookup` queries typically use UDP. Testing TCP separately helps identify firewall or NAT rules that permit UDP DNS but unintentionally block TCP DNS.

3. Verify that DNS also works over TCP. On a Linux system with `dig` installed, run the following command and confirm that the response includes `status: NOERROR`.

    - `dig` performs DNS queries and displays detailed responses.
        - `@<VIP_IP_ADDRESS>` specifies the DNS server.
        - `+tcp` forces the query to use TCP instead of UDP.

    ```shell
    dig @<VIP_IP_ADDRESS> google.com +tcp
    ```

4. On a Windows client, verify that clients receive the shared DNS VIP through DHCP. Confirm that the DNS Servers field contains the shared VIP.

    - `ipconfig /all` displays detailed Windows network adapter information, including assigned DNS servers.
        - `ipconfig` displays Windows network configuration.
        - `/all` includes detailed information for all network adapters.

    ```powershell
    ipconfig /all
    ```

> [!NOTE]
> Existing DHCP clients may retain their previous DNS settings until their leases are renewed. Statically configured devices will be updated in the **DNS Configuration Migration** section.

5. Verify that configured local DNS records resolve successfully. Confirm that the expected internal IP address is returned.

    ```shell
    nslookup <LOCAL_HOSTNAME> <VIP_IP_ADDRESS>
    ```

6. Verify that AdGuard Home filtering works through the VIP. Open the active AdGuard Home instance's web interface and review the **Query Log** while generating DNS requests from a client. Confirm that queries appear and that configured filtering rules produce the expected results.

7. On the **primary AdGuard Home instance**, verify that AdGuard Home Sync continues synchronizing configuration changes. Confirm that scheduled synchronization completes without authentication or connection errors.

    ```shell
    sudo journalctl -u adguardhome-sync -n 50 --no-pager
    ```

8. Review Keepalived logs on both AdGuard Home instances. Look for the expected transitions between `MASTER`, `BACKUP`, and `FAULT`, along with health-check failures and recovery messages.

    - `-n 100` displays the last 100 log entries.

    ```shell
    sudo journalctl -u keepalived -n 100 --no-pager
    ```

[🔼 Back to Overview](#overview)

## DNS Configuration Migration

After completing OPNsense integration and verifying DNS high availability, update any remaining devices with manually configured DNS server addresses to use the shared VIP.

Devices using DHCP should already receive the VIP through the Dnsmasq configuration. However, devices with static DNS settings may continue referencing the individual AdGuard Home instances, bypassing the high-availability configuration.

1. Identify devices with manually configured DNS addresses, such as Proxmox hosts, virtual machines, Linux containers, NAS devices, and other network infrastructure.

2. Replace the individual AdGuard Home IP addresses with the shared VIP (`<VIP_IP_ADDRESS>`) in each device's DNS configuration.

3. Save or apply the updated DNS configuration. Restart the relevant networking service or renew its connection if necessary for the changes to take effect.

4. Verify that the device can resolve DNS queries through the VIP.

    ```shell
    nslookup google.com <VIP_IP_ADDRESS>
    ```

    Confirm that the DNS query succeeds without timeouts or connection errors.

5. Verify that the device is configured to use the VIP by default.

    On Windows:

    ```powershell
    ipconfig /all
    ```

    On Linux systems using systemd-resolved:

    - `resolvectl status` displays DNS resolver information for the system and its network interfaces.
        - `resolvectl` manages and inspects the systemd-resolved DNS resolver.
        - `status` displays the current resolver configuration.

    ```shell
    resolvectl status
    ```

> [!NOTE]
> DNS configuration methods vary between operating systems and network managers. Systems that do not use systemd-resolved may require checking their network configuration or `/etc/resolv.conf` instead.

6. Review any remaining references to the individual AdGuard Home IP addresses. These should only remain where direct instance access is intentional, such as AdGuard Home Sync, Keepalived health checks, administration, or troubleshooting.

[🔼 Back to Overview](#overview)

## Related Documentation

- [Connecting to Remote Servers with SSH](../../01-getting-started/remote-access/ssh.md) — Connect to remote servers using SSH from Windows, Linux, and macOS.
- [Accessing Proxmox VM and LXC Consoles](../proxmox/accessing-proxmox-vm-lxc-consoles.md) — Access virtual machines and Linux containers directly through the Proxmox web interface when SSH is unavailable.

## Official Documentation

- [AdGuard Home Documentation](https://github.com/AdguardTeam/AdGuardHome/wiki) — Official documentation for AdGuard Home installation, configuration, DNS filtering, and troubleshooting.
- [AdGuard Home Sync](https://github.com/bakito/adguardhome-sync) — Project documentation covering synchronization, configuration options, environment variables, and installation methods.
- [Keepalived Documentation](https://www.keepalived.org/documentation.html) — Official documentation for Keepalived, VRRP, configuration, and high-availability behavior.
- [Keepalived Configuration Reference](https://www.keepalived.org/documentation/keepalived-conf/) — Reference for VRRP instances, health-check scripts, priorities, and failover settings.
- [OPNsense Configuration Backups](https://docs.opnsense.org/manual/backups.html) — Official instructions for backing up and restoring OPNsense configurations.
- [OPNsense Network Address Translation](https://docs.opnsense.org/manual/nat.html) — Documentation for destination NAT, redirect rules, and rule behavior.
- [OPNsense Dnsmasq DNS & DHCP](https://docs.opnsense.org/manual/dnsmasq.html) — Official documentation for Dnsmasq DNS services, host entries, DHCP ranges, and DHCP options.

---

[🔼 Back to Guides](../README.md)