# Day 3 — Hour 1: Ubuntu VM SSH Hardening + UFW + Log Verification

## Objective
Harden SSH on my Ubuntu Server VM by enforcing key-only authentication, restricting allowed SSH users, enabling UFW, and verifying results via sshd effective config + logs.

## VM Context
- Hostname: joshua-lab
- Guest IP: 10.0.2.15 (VirtualBox NAT)
- Port forward: Host 2223 -> Guest 22
- SSH connect method (Windows): `ssh joshua-lab` via `~/.ssh/config`

## SSH Client Config (Windows)
File: `C:\Users\Joshua\.ssh\config`

```sshconfig
Host joshua-lab
  HostName 127.0.0.1
  Port 2223
  User joshua
  IdentityFile ~/.ssh/joshua_lab_ed25519
  IdentitiesOnly yes
````

## Changes Made

### 1) SSH Server Hardening (sshd)

Method: used a drop-in config file (preferred)
File: `/etc/ssh/sshd_config.d/99-joshua-hardening.conf`

Settings applied:

* PermitRootLogin no
* PasswordAuthentication no
* PubkeyAuthentication yes
* KbdInteractiveAuthentication no
* MaxAuthTries 3
* LoginGraceTime 30
* AllowUsers joshua
* X11Forwarding no

Validation:

* `sudo sshd -t` (no output = OK)

Restart:

* `sudo systemctl restart ssh.socket || true`
* `sudo systemctl restart ssh`

### 2) Restrict Authorized Keys Options

File: `/home/joshua/.ssh/authorized_keys`

Added key options to disable forwarding features:

* `no-agent-forwarding`
* `no-port-forwarding`
* `no-X11-forwarding`

Example format:
`no-agent-forwarding,no-port-forwarding,no-X11-forwarding ssh-ed25519 AAAA...`

Permissions:

* `/home/joshua/.ssh` = 700
* `authorized_keys` = 600
* ownership: `joshua:joshua`

### 3) Firewall (UFW)

Goal: allow SSH only.

Commands:

* `sudo ufw allow OpenSSH`
* `sudo ufw enable`
* `sudo ufw status verbose`

## Proof / Verification

### A) Effective sshd settings (source of truth)

Command:

```bash
sudo sshd -T | egrep 'permitrootlogin|passwordauthentication|kbdinteractiveauthentication|allowusers|maxauthtries|logingracetime|x11forwarding'
```

Result (paste your real output here):

```text
logingracetime 30
maxauthtries 3
permitrootlogin no
passwordauthentication no
kbdinteractiveauthentication no
x11forwarding no
allowusers joshua
allowusers joshua
```

### B) SSH access proof (key-only)

From Windows:

* `ssh joshua-lab` succeeds (no password prompt)

Screenshot:

* `day-03/screenshots/day03-hour01-ssh-success.png`

### C) Logs

Commands:

```bash
sudo journalctl -u ssh --no-pager | tail -n 50
sudo tail -n 50 /var/log/auth.log
```

What I checked:

* Successful public key authentication entries
* No password authentication succeeding

## Lessons Learned

* Hostname ≠ username (AllowUsers must match the Linux user, not the hostname).
* Editing base sshd_config is riskier; drop-in files are safer and easier to revert.
* Always test a new SSH session before closing the old one.