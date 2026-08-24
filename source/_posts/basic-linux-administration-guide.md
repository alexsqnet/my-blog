---
title: Basic Linux Administration Guide
date: 2026-07-14 23:23:05
tags: [linux, administration]
category: ["linux"]
---


This guide contains common Linux administration commands useful when managing a VPS, especially Ubuntu/Debian-based servers.

<!--more-->

> Most administrative commands require `sudo`.

---

## 1. Check the current user

```bash
whoami
```

Show information about the current user and groups:

```bash
id
```

Show currently logged-in users:

```bash
who
```

---

## 2. Create a new user

Create a new user:

```bash
sudo adduser username
```

Example:

```bash
sudo adduser deployuser
```

The command will ask you to set a password and optionally enter additional user information.

Check that the user exists:

```bash
id deployuser
```

---

## 3. Add a user to the sudo group

On Ubuntu/Debian, users in the `sudo` group can execute administrative commands.

```bash
sudo usermod -aG sudo username
```

Example:

```bash
sudo usermod -aG sudo deployuser
```

Verify the user's groups:

```bash
groups deployuser
```

or:

```bash
id deployuser
```

You should see:

```text
sudo
```

The user may need to log out and log back in before the new group membership is applied.

---

## 4. Test sudo access

Switch to the user:

```bash
su - deployuser
```

Then test:

```bash
sudo whoami
```

Expected result:

```text
root
```

---

## 5. Switch users

Switch to another user:

```bash
su - username
```

Example:

```bash
su - deployuser
```

Open a root shell:

```bash
sudo -i
```

Exit the current shell:

```bash
exit
```

---

## 6. Change a user's password

Change your own password:

```bash
passwd
```

Change another user's password:

```bash
sudo passwd username
```

Example:

```bash
sudo passwd deployuser
```

---

## 7. Lock or unlock a user account

Lock a user:

```bash
sudo passwd -l username
```

Unlock a user:

```bash
sudo passwd -u username
```

---

## 8. Delete a user

Delete a user but keep their home directory:

```bash
sudo deluser username
```

Delete the user and their home directory:

```bash
sudo deluser --remove-home username
```

Example:

```bash
sudo deluser --remove-home deployuser
```

---

## 9. Create and manage groups

Create a group:

```bash
sudo groupadd groupname
```

Add a user to a group:

```bash
sudo usermod -aG groupname username
```

Example:

```bash
sudo usermod -aG docker deployuser
```

Remove a user from a group:

```bash
sudo deluser username groupname
```

List all groups:

```bash
cat /etc/group
```

---

## 10. File and directory navigation

Show the current directory:

```bash
pwd
```

List files:

```bash
ls
```

Detailed listing:

```bash
ls -la
```

Change directory:

```bash
cd /path/to/directory
```

Go to the home directory:

```bash
cd ~
```

Go one directory up:

```bash
cd ..
```

---

## 11. Create files and directories

Create a directory:

```bash
mkdir directory-name
```

Create nested directories:

```bash
mkdir -p parent/child
```

Create an empty file:

```bash
touch file.txt
```

---

## 12. Copy, move, and delete files

Copy a file:

```bash
cp source.txt destination.txt
```

Copy a directory recursively:

```bash
cp -r source-directory destination-directory
```

Move or rename a file:

```bash
mv old-name new-name
```

Delete a file:

```bash
rm file.txt
```

Delete a directory recursively:

```bash
rm -r directory
```

Force recursive deletion:

```bash
rm -rf directory
```

> Be very careful with `rm -rf`. Deleted files are usually not recoverable.

---

## 13. View file contents

Display a file:

```bash
cat file.txt
```

Read a file page by page:

```bash
less file.txt
```

Show the first lines:

```bash
head file.txt
```

Show the last lines:

```bash
tail file.txt
```

Follow a log file in real time:

```bash
tail -f /var/log/syslog
```

---

## 14. Edit files

Using Nano:

```bash
nano file.txt
```

Useful Nano shortcuts:

```text
Ctrl + O  -> save
Ctrl + X  -> exit
Ctrl + W  -> search
```

---

## 15. File ownership

Show ownership:

```bash
ls -l
```

Change the owner of a file:

```bash
sudo chown username file.txt
```

Change owner and group:

```bash
sudo chown username:groupname file.txt
```

Change ownership recursively:

```bash
sudo chown -R username:groupname directory
```

Example:

```bash
sudo chown -R deployuser:deployuser /opt/myapp
```

---

## 16. File permissions

Example:

```bash
chmod 644 file.txt
```

Common permission values:

```text
600 -> owner read/write only
644 -> owner read/write, everyone else read
700 -> owner full access only
755 -> owner full access, others read/execute
```

Example for an SSH directory:

```bash
chmod 700 ~/.ssh
chmod 600 ~/.ssh/authorized_keys
```

Make a script executable:

```bash
chmod +x script.sh
```

---

## 17. Check disk usage

Show filesystem usage:

```bash
df -h
```

Show the size of a directory:

```bash
du -sh /path/to/directory
```

Show directory sizes:

```bash
du -h --max-depth=1 /path/to/directory
```

Example:

```bash
du -h --max-depth=1 /var
```

---

## 18. Check memory usage

```bash
free -h
```

---

## 19. Check CPU and running processes

Interactive process viewer:

```bash
top
```

A more convenient alternative:

```bash
htop
```

Install it if necessary:

```bash
sudo apt install htop
```

List processes:

```bash
ps aux
```

Find a process:

```bash
ps aux | grep process-name
```

Example:

```bash
ps aux | grep nginx
```

---

## 20. Stop a process

Find the PID:

```bash
ps aux | grep process-name
```

Gracefully stop it:

```bash
kill PID
```

Force stop:

```bash
kill -9 PID
```

Use `kill -9` only when a normal `kill` does not work.

---

## 21. Manage services with systemd

Check a service:

```bash
sudo systemctl status nginx
```

Start a service:

```bash
sudo systemctl start nginx
```

Stop a service:

```bash
sudo systemctl stop nginx
```

Restart a service:

```bash
sudo systemctl restart nginx
```

Reload configuration:

```bash
sudo systemctl reload nginx
```

Enable a service at boot:

```bash
sudo systemctl enable nginx
```

Disable it at boot:

```bash
sudo systemctl disable nginx
```

---

## 22. View service logs

Show logs for a service:

```bash
sudo journalctl -u nginx
```

Follow logs in real time:

```bash
sudo journalctl -u nginx -f
```

Show logs from the current boot:

```bash
sudo journalctl -b
```

---

## 23. Update the server

Refresh available package information:

```bash
sudo apt update
```

Upgrade installed packages:

```bash
sudo apt upgrade
```

Run both:

```bash
sudo apt update && sudo apt upgrade
```

Remove unused packages:

```bash
sudo apt autoremove
```

---

## 24. Install and remove software

Install a package:

```bash
sudo apt install package-name
```

Example:

```bash
sudo apt install curl
```

Remove a package:

```bash
sudo apt remove package-name
```

Remove package and configuration files:

```bash
sudo apt purge package-name
```

---

## 25. Check Linux version

```bash
cat /etc/os-release
```

Kernel version:

```bash
uname -a
```

Hostname:

```bash
hostname
```

More host information:

```bash
hostnamectl
```

---

## 26. Change the hostname

```bash
sudo hostnamectl set-hostname new-hostname
```

Example:

```bash
sudo hostnamectl set-hostname my-vps
```

Check it:

```bash
hostnamectl
```

---

## 27. Network information

Show IP addresses:

```bash
ip addr
```

Short version:

```bash
ip a
```

Show routes:

```bash
ip route
```

Test network connectivity:

```bash
ping google.com
```

Find the public IP:

```bash
curl ifconfig.me
```

---

## 28. Check open/listening ports

Recommended:

```bash
sudo ss -tulpn
```

Typical output can show services listening on ports such as:

```text
22   SSH
80   HTTP
443  HTTPS
```

---

## 29. Check which process uses a port

Example for port `80`:

```bash
sudo lsof -i :80
```

or:

```bash
sudo ss -ltnp | grep :80
```

---

## 30. Basic firewall with UFW

Install UFW if necessary:

```bash
sudo apt install ufw
```

Allow SSH before enabling the firewall:

```bash
sudo ufw allow OpenSSH
```

Allow HTTP:

```bash
sudo ufw allow 80/tcp
```

Allow HTTPS:

```bash
sudo ufw allow 443/tcp
```

Enable UFW:

```bash
sudo ufw enable
```

Check status:

```bash
sudo ufw status
```

Detailed status:

```bash
sudo ufw status verbose
```

Show firewall rules with rule numbers:

```bash
sudo ufw status numbered
```

> Always allow SSH before enabling UFW on a remote VPS, otherwise you may lock yourself out.

### Block an IP address

Block all incoming traffic from a specific IP:

```bash
sudo ufw deny from 203.0.113.10
```

Block an IP only from accessing a specific port, for example SSH on port `22`:

```bash
sudo ufw deny from 203.0.113.10 to any port 22
```

### Check whether an IP is blocked

Search the current UFW rules for a specific IP:

```bash
sudo ufw status | grep 203.0.113.10
```

For numbered rules:

```bash
sudo ufw status numbered
```

If the IP appears in a rule similar to:

```text
DENY IN    203.0.113.10
```

then UFW is configured to block traffic from that IP.

You can also inspect the active UFW rules in more detail:

```bash
sudo ufw show raw
```

### Unblock an IP address

If the IP was blocked with:

```bash
sudo ufw deny from 203.0.113.10
```

remove that rule with:

```bash
sudo ufw delete deny from 203.0.113.10
```

If the IP was blocked only for a specific port:

```bash
sudo ufw delete deny from 203.0.113.10 to any port 22
```

### Remove a rule by number

First list numbered rules:

```bash
sudo ufw status numbered
```

Example:

```text
[ 1] 22/tcp                     ALLOW IN    Anywhere
[ 2] Anywhere                   DENY IN     203.0.113.10
```

Delete rule number `2`:

```bash
sudo ufw delete 2
```

Then verify the result:

```bash
sudo ufw status numbered
```

> Be careful when deleting rules by number. Rule numbers can change after a rule is removed.

### Reload UFW

Most UFW rule changes are applied immediately, but you can reload the firewall if needed:

```bash
sudo ufw reload
```


## 32. Fail2ban basics

Fail2ban can automatically block IP addresses that repeatedly fail authentication attempts, such as SSH login attempts.

Install Fail2ban:

```bash
sudo apt install fail2ban
```

Check the Fail2ban service:

```bash
sudo systemctl status fail2ban
```

Start Fail2ban:

```bash
sudo systemctl start fail2ban
```

Enable it at boot:

```bash
sudo systemctl enable fail2ban
```

### Check active jails

```bash
sudo fail2ban-client status
```

Typical output may include:

```text
Jail list: sshd
```

### Check SSH jail status

```bash
sudo fail2ban-client status sshd
```

This shows information such as:

- currently failed attempts;
- total failed attempts;
- currently banned IPs;
- total banned IPs;
- the list of banned IP addresses.

Example output:

```text
Status for the jail: sshd
|- Filter
|  |- Currently failed: 0
|  `- Total failed: 12
`- Actions
   |- Currently banned: 1
   |- Total banned: 3
   `- Banned IP list: 203.0.113.10
```

### Check whether a specific IP is banned

You can inspect the SSH jail:

```bash
sudo fail2ban-client status sshd
```

Or search for a specific IP:

```bash
sudo fail2ban-client status sshd | grep 203.0.113.10
```

If the IP appears in the `Banned IP list`, it is currently banned by Fail2ban.

### Unban an IP address

Unban an IP from the SSH jail:

```bash
sudo fail2ban-client set sshd unbanip 203.0.113.10
```

Example:

```bash
sudo fail2ban-client set sshd unbanip 89.x.x.x
```

Then verify:

```bash
sudo fail2ban-client status sshd
```

### Manually ban an IP

```bash
sudo fail2ban-client set sshd banip 203.0.113.10
```

### Check Fail2ban logs

On Ubuntu/Debian:

```bash
sudo tail -f /var/log/fail2ban.log
```

Search for bans:

```bash
sudo grep "Ban " /var/log/fail2ban.log
```

Search for unbans:

```bash
sudo grep "Unban " /var/log/fail2ban.log
```

### Restart Fail2ban

```bash
sudo systemctl restart fail2ban
```

> UFW and Fail2ban are different tools. UFW manages firewall rules, while Fail2ban watches logs and can dynamically add or remove blocks based on suspicious behavior.

---

## 32. Reboot or shut down the server

Reboot:

```bash
sudo reboot
```

Shutdown:

```bash
sudo shutdown now
```

Schedule shutdown:

```bash
sudo shutdown +10
```

This shuts down the server in 10 minutes.

Cancel a scheduled shutdown:

```bash
sudo shutdown -c
```

---

## 33. Useful directories

Common Linux directories:

```text
/home          -> user home directories
/root          -> root user's home directory
/etc           -> system configuration
/var           -> logs and changing application data
/var/log       -> logs
/opt           -> optional/custom software
/tmp           -> temporary files
/usr           -> installed programs and libraries
/srv           -> service/application data
```

A useful place for your own deployed application can be:

```text
/opt/myapp
```

or:

```text
/srv/myapp
```

---

## 34. Example: Create a dedicated VPS user

Create the user:

```bash
sudo adduser deployuser
```

Give sudo access:

```bash
sudo usermod -aG sudo deployuser
```

Check groups:

```bash
groups deployuser
```

Switch to the user:

```bash
su - deployuser
```

Test sudo:

```bash
sudo whoami
```

Expected result:

```text
root
```

Then configure SSH key authentication for that user and disable password-based SSH login after verifying that the key works.

---

## 35. Useful daily commands

```bash
whoami
id
pwd
ls -la
df -h
free -h
top
ip a
sudo ss -tulpn
sudo systemctl status SERVICE
sudo journalctl -u SERVICE -f
sudo apt update
```

---

## Security recommendations for a VPS

For a public server:

- use SSH keys instead of SSH passwords;
- disable direct root SSH login;
- keep the server updated;
- use a firewall;
- expose only required ports;
- avoid running applications as `root`;
- create separate users where appropriate;
- use strong permissions on configuration and secret files;
- regularly back up important data;
- never commit passwords, private keys, tokens, or production secrets to Git.
