---
title: Secure SSH Configuration - SSH Key Authentication Only
date: 2026-08-24 22:53:01
tags: [ssh, authentication, server, security]
category: ["ssh"]
---

This guide shows how to generate an SSH key and configure a Linux server so that SSH authentication is performed only with an SSH key, without allowing login using the user's password.

<!--more-->

> **Important:** Do not disable password authentication until you have verified that SSH key login works correctly.

## 1. Generate an SSH key

The recommended key type for modern SSH setups is `ed25519`.

### Windows PowerShell

Generate a new SSH key:

```powershell
ssh-keygen -t ed25519 -C "your-name-or-server-name"
```

For example:

```powershell
ssh-keygen -t ed25519 -C "testapp-servename"
```

When asked where to save the key, you can press `Enter` to use the default location:

```text
C:\Users\<username>\.ssh\id_ed25519
```

This creates two files:

```text
id_ed25519      -> private key
id_ed25519.pub  -> public key
```

You can display the public key with:

```powershell
Get-Content $env:USERPROFILE\.ssh\id_ed25519.pub
```

### Generate a separate key for a specific server

If you already have an SSH key and want a separate one for this VPS:

```powershell
ssh-keygen -t ed25519 -f $env:USERPROFILE\.ssh\id_ed25519_testapp -C "testapp-servename"
```

Display its public key:

```powershell
Get-Content $env:USERPROFILE\.ssh\id_ed25519_testapp.pub
```

Connect using that specific private key:

```powershell
ssh -i $env:USERPROFILE\.ssh\id_ed25519_testapp user@SERVER_IP
```

### Linux or macOS

Generate the key:

```bash
ssh-keygen -t ed25519 -C "testapp-servername"
```

Display the public key:

```bash
cat ~/.ssh/id_ed25519.pub
```

### Passphrase recommendation

When `ssh-keygen` asks for a passphrase, it is recommended to set one, especially for keys that provide access to production servers.

A passphrase protects the private key if the file is ever copied or stolen.

## 2. Add the public key to the server

Connect to the server and create the `.ssh` directory if it does not already exist:

```bash
mkdir -p ~/.ssh
chmod 700 ~/.ssh
```

Open the `authorized_keys` file:

```bash
nano ~/.ssh/authorized_keys
```

Add your SSH public key to the file, for example:

```text
ssh-ed25519 AAAAC3... your-public-key
```

Save the file and set the correct permissions:

```bash
chmod 600 ~/.ssh/authorized_keys
```

## 3. Test SSH key authentication

Do not close your current SSH session.

Open a new terminal on your computer and try to connect:

```powershell
ssh user@SERVER_IP
```

If you use an SSH key with a custom filename:

```powershell
ssh -i $env:USERPROFILE\.ssh\id_ed25519_testapp user@SERVER_IP
```

If you can connect without entering the Linux user's password, SSH key authentication is working correctly.

If your SSH key has a passphrase, SSH may ask for the **key passphrase**. This is different from the Linux user's password.

## 4. Disable SSH password authentication

Edit the SSH server configuration:

```bash
sudo nano /etc/ssh/sshd_config
```

Make sure the following settings are present:

```text
PubkeyAuthentication yes
PasswordAuthentication no
PermitRootLogin no
KbdInteractiveAuthentication no
```

### What these options do

- `PubkeyAuthentication yes` — enables SSH key authentication.
- `PasswordAuthentication no` — disables SSH login using the user's password.
- `PermitRootLogin no` — disables direct SSH login as `root`.
- `KbdInteractiveAuthentication no` — disables interactive password-based authentication methods.

## 5. Validate the SSH configuration

Before restarting the SSH service, validate the configuration:

```bash
sudo sshd -t
```

If this command does not display any errors, the configuration is valid.

## 6. Restart the SSH service

On Ubuntu:

```bash
sudo systemctl restart ssh
```

You can also check the service status:

```bash
sudo systemctl status ssh
```

## 7. Test the connection again

Open another new terminal and connect again:

```powershell
ssh user@SERVER_IP
```

The connection should work using the SSH key.

If someone tries to connect using only the user's password, authentication should be rejected.

## 8. The user's password can remain enabled for sudo

You do not need to remove the Linux user's password.

A good configuration is:

```text
SSH login -> SSH key only
sudo      -> user password may still be required
```

This means the user's password cannot be used for SSH login, but it can still be used for administrative operations with `sudo`.

## Optional: SSH config for easier connections

If you use a custom SSH key, you can add an entry to your SSH config file.

On Windows:

```text
C:\Users\<username>\.ssh\config
```

On Linux/macOS:

```text
~/.ssh/config
```

Example:

```text
Host testapp
    HostName SERVER_IP
    User user
    IdentityFile ~/.ssh/id_ed25519_testapp
```

Then you can connect simply with:

```bash
ssh testapp
```

## Additional recommendations

For a public VPS, it is also worth configuring:

- a firewall such as `ufw`;
- SSH access only for specific users;
- regular security updates;
- Fail2ban, if you want additional protection;
- backups for important data and configuration.

## Important security note

Keep your SSH private key only on your own device.

Never copy or upload the private key to the server or to external services.

The public key can safely be stored in:

```text
~/.ssh/authorized_keys
```

The private key should remain only on the computer you use to connect to the server.
