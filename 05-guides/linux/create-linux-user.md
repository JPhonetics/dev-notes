---
Created: 2026-11-11
Modified:
---

# Create a Linux User

Linux uses user accounts to control access to the operating system, files, applications, and administrative functions. Each account has its own identity, permissions, and group memberships.

The `root` account has unrestricted administrative privileges. Using a separate user account for routine administration reduces the need to operate continuously as root, while `sudo` allows authorized users to execute individual commands with elevated privileges.

This guide covers creating and managing local Linux users on Debian and Ubuntu-based systems, including checking existing accounts, creating users, managing passwords, configuring optional sudo privileges, and verifying account access.

> [!IMPORTANT]
> Commands that create or modify user accounts require administrative privileges. This guide assumes you are using an authorized root shell.
>
> If you already have sudo access, prefix administrative commands with `sudo`.

## Overview

1. [Check Existing Linux Users](#check-existing-linux-users)
2. [Create a Linux User](#create-a-linux-user)
3. [Change or Reset a User Password](#change-or-reset-a-user-password)
4. [Configure Sudo Access](#configure-sudo-access)
5. [Verify the User Account](#verify-the-user-account)
6. [Related Documentation](#related-documentation)
7. [Official Documentation](#official-documentation)

## Check Existing Linux Users

Linux maintains account information in a user database. Each account has a unique username and numeric user ID (UID), along with information such as its home directory and login shell.

Before creating a new account, check whether the desired username is already in use.

> [!NOTE]
> Linux includes system accounts used by services and background processes. Not every account displayed is intended for interactive login.

1. List the usernames known to the Linux system.

    - `getent passwd | cut -d: -f1` retrieves user account records and displays only their usernames.
        - `getent` retrieves entries from system databases, including local accounts and configured external identity services.
        - `passwd` specifies the user account database.
        - `|` (pipe) passes the output of the first command to the next command.
        - `cut` extracts specific portions of each line.
        - `-d:` specifies the colon (`:`) as the field separator.
        - `-f1` selects the first field, which contains the username.

    ```bash
    getent passwd | cut -d: -f1
    ```

    The output may include accounts such as:

    ```text
    root
    daemon
    bin
    sys
    nobody
    ```

2. Check whether the username you want to create already exists. If no account is found, the command normally returns no output.

    - `getent passwd <USERNAME>` searches the user account database for the specified username.
        - `<USERNAME>` is the username you want to create or verify.

    ```bash
    getent passwd <USERNAME>
    ```

    If the account exists, its information will be displayed. For example:

    ```text
    admin:x:1000:1000:,,,:/home/admin:/bin/bash
    ```

[🔼 Back to Overview](#overview)

## Create a Linux User

The `adduser` utility creates a new Linux user account and configures its basic information. On Debian and Ubuntu, it normally creates a home directory, assigns a user ID, creates a primary group, and prompts for a password.

1. Create the new Linux user.

    - `adduser <USERNAME>` creates a user account using the specified username.
        - `adduser` is an interactive utility for creating Linux users and groups.

    ```bash
    adduser <USERNAME>
    ```

2. Enter and confirm a password when prompted. The password is used to authenticate the new account when password-based authentication is permitted.

    > [!NOTE]
    > Password characters are not displayed while typing in the terminal. This is normal behavior.

3. Complete the optional account information prompts. 

    - The installer may ask for information such as the user's full name, room number, and telephone number. These fields can normally be left blank by pressing `Enter`.

4. Review the information and confirm the account creation when prompted. After the process completes, the user account should have its own home directory, typically located at `/home/<USERNAME>`.

[🔼 Back to Overview](#overview)

## Change or Reset a User Password

Linux uses the `passwd` utility to manage account passwords. Users can change their own passwords, while administrators can reset passwords for other local accounts.

### Change Your Own Password

Users can change their own passwords without administrative privileges. Run the following command while logged in to the account whose password you want to change.

1. Change the password for the currently logged-in account. Enter the current password when prompted, then enter and confirm the new password.

    - `passwd` starts the interactive password-change process for the current user.

    ```bash
    passwd
    ```

### Reset Another User's Password

1. Reset the password for an existing user from an authorized root shell. Enter and confirm the new password when prompted.

    - `passwd <USERNAME>` sets a new password for the specified account without requiring its current password.
        - `<USERNAME>` identifies the Linux account whose password will be reset.

    ```bash
    passwd <USERNAME>
    ```

    > [!NOTE]
    > If using an account with sudo privileges instead of root, execute the command as `sudo passwd <USERNAME>`.

[🔼 Back to Overview](#overview)

## Configure Sudo Access

By default, a newly created user does not automatically receive administrative privileges.

`sudo` (superuser do) allows an authorized user to execute commands with elevated privileges. On Debian and Ubuntu, users belonging to the `sudo` group are typically permitted to run administrative commands, subject to the system's sudo configuration.

> [!CAUTION]
> This section is optional. Only grant sudo access to accounts that require administrative privileges. Regular application and service accounts generally should not receive unrestricted sudo access.

1. Ensure that the `sudo` package is installed. If the package is already installed, APT will report that no additional installation is necessary.

    - `apt install sudo` installs the sudo utility if it is not already installed.
        - `apt` (Advanced Package Tool) is Debian's package manager.
        - `install` installs the specified package and any required dependencies.
        - `sudo` identifies the package to install.

    ```bash
    apt install sudo
    ```

2. Add the user to the `sudo` group.

    - `usermod -aG sudo <USERNAME>` adds the specified user to the sudo group without removing existing supplementary group memberships.
        - `usermod` modifies an existing Linux user account.
        - `-a` appends the user to the specified group without removing existing supplementary groups.
        - `-G` specifies the supplementary group or groups to assign.
        - `sudo` is the administrative group being added.

    ```bash
    usermod -aG sudo <USERNAME>
    ```

    > [!IMPORTANT]
    > Always use `-a` with `-G` when adding supplementary groups. Omitting `-a` can replace the user's existing supplementary group memberships.

    > [!NOTE]
    > The updated group membership takes effect when the user starts a new login session. If the user is already logged in, log out and reconnect before verifying sudo access.

[🔼 Back to Overview](#overview)

## Verify the User Account

After creating the account, verify that it exists, has the expected group memberships, and can start a login shell successfully.

1. Display the user's account identity and group memberships.

    - `id <USERNAME>` displays the user's numeric UID, primary group ID (GID), and supplementary groups.
        - `id` displays user and group identity information.

    ```bash
    id <USERNAME>
    ```

    For a user granted sudo access, the output should include the `sudo` group. The actual UID and GID values may differ.

    ```text
    uid=1000(admin) gid=1000(admin) groups=1000(admin),27(sudo)
    ```

2. Switch to the new user account.

    - `su - <USERNAME>` starts a login shell as the specified user.
        - `su` (substitute user) switches to another Linux user account.
        - `-` starts a login shell, loading the target user's login environment and home directory.

    ```bash
    su - <USERNAME>
    ```

3. Verify the currently active user account. The output should display the new username.

    - `whoami` displays the username associated with the current effective user identity.

    ```bash
    whoami
    ```

4. If sudo access was configured, verify that the account can execute administrative commands.

    - `sudo whoami` executes `whoami` with elevated privileges to verify sudo access.

    ```bash
    sudo whoami
    ```

    When prompted, enter the new user's password. Successful execution should return:

    ```text
    root
    ```

5. Exit the new user's shell to return to the previous administrative session.

    - `exit` closes the current shell session.

    ```bash
    exit
    ```

> [!NOTE]
> After creating and verifying the user account, you can connect remotely if an SSH server is installed and configured to permit access.
>
> See [Connecting to Remote Servers with SSH](../ssh/connecting-to-remote-servers.md) for connection instructions, authentication, and troubleshooting.

[🔼 Back to Overview](#overview)

## Related Documentation

- [Connecting to Remote Servers with SSH](../../01-getting-started/remote-access/ssh.md) — Connect to remote servers using SSH from Windows, Linux, and macOS.

## Official Documentation

- [Ubuntu User Management](https://ubuntu.com/server/docs/security-users/) — Official Ubuntu documentation for creating, managing, and removing Linux user accounts and groups.
- [Ubuntu User Security](https://ubuntu.com/server/docs/how-to/security/user-management/) — Official documentation covering user account permissions, passwords, and access security.
- [Debian Reference](https://www.debian.org/doc/manuals/debian-reference/) — Official Debian reference covering user accounts, authentication, system administration, and package management.
- [Debian Sudo Documentation](https://wiki.debian.org/sudo) — Official Debian guidance for installing sudo, configuring administrative access, and managing the sudo group.

---

[🔼 Back to Guides](../README.md)