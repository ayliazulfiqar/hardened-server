# Project 3: Hardened Linux Server

A fresh Ubuntu Server VM that I audit, harden step by step, and re-test, with measurable before-and-after evidence.

## Goal

Learn what it takes to secure a Linux server, not just attack one. I want to measure the server's security before hardening, apply common defences one at a time, and show the improvement with an audit score and a before-and-after scan from my Kali attacker machine.

## Why this project

Projects 1 and 2 were about attacking and scanning. This project looks at cybersecurity from the defender's side, helping me understand why weaknesses such as weak SSH logins, unnecessary open ports, and misconfigured permissions matter, and how to fix them.

It also supports my Linux administration and cloud security goals.

## Environment

| Machine | Role | Network adapters |
|---|---|---|
| Ubuntu Server VM | Server to harden | Adapter 1: NAT Network (`Kali-NATNetwork`), Adapter 2: Host-only (`vboxnet0`) |
| Kali Linux VM | Attacker and tester | Adapter 1: NAT, Adapter 2: Host-only (`vboxnet0`) |
| Host | Linux Mint, runs VirtualBox | n/a |

**Ubuntu version:** Ubuntu 26.04.1 LTS

**Ubuntu username:** `vboxuser`

**Ubuntu network addresses:**
- NAT: `10.0.0.3`
- Host-only: `192.168.56.104`

All testing happens only inside my own isolated lab.

## Plan

| Stage | What I'll do | Status |
|---|---|---|
| 1. Build | Install Ubuntu Server, create a normal admin user, configure networking, and take the `fresh-install` snapshot | Done |
| 2. Baseline | Run a **Lynis** audit and an **Nmap** scan from Kali, and record the score and open ports | Done – Lynis baseline completed; Nmap evidence pending |
| 3. SSH hardening | Configure key-based login, disable password login, disable root login, and limit authentication attempts | Done |
| 4. Firewall | Configure **UFW** with a default-deny incoming policy and allow SSH | Done |
| 5. Brute-force protection | Install and configure **fail2ban** | Planned |
| 6. Updates | Enable automatic security updates | Planned |
| 7. Least privilege | Review users, sudo rights, and file permissions; remove unused services | Planned |
| 8. Logging | Review `auth.log` and `journalctl` to see failed logins and blocked attempts | Planned |
| 9. Re-test | Re-run Lynis and Nmap, and compare with the baseline | Planned |

I take a snapshot after each stage so I can roll back if something breaks.

## Evidence collected

- Initial Lynis security audit saved as `lynis-baseline.txt`.
- Initial Lynis hardening index and findings recorded.
- SSH connection successfully tested from Kali Linux.
- Ed25519 SSH key pair generated on Kali Linux.
- Public key installed on the Ubuntu Server account using `ssh-copy-id`.
- SSH configuration hardened with the following settings:

```text
PermitRootLogin no
PasswordAuthentication no
PubkeyAuthentication yes
MaxAuthTries 3
```

- SSH configuration validated using `sudo /usr/sbin/sshd -t`.
- UFW configured to allow SSH, deny incoming connections by default, and allow outgoing connections.
- UFW enabled and checked using `sudo ufw status verbose`.
- Ubuntu network configuration recorded.
- Screenshots are stored in the `screenshots/` folder.

### Screenshots

Add the following screenshots to the `screenshots/` folder:

## Results

The following table records the initial security baseline and the current hardening progress.

| Measure | Before | After |
|---|---|---|
| Lynis hardening index | 63 | Pending re-audit |
| Lynis warnings | 0 | Pending re-audit |
| Lynis suggestions | 36 | Pending re-audit |
| Open ports (Nmap) | Recorded in baseline scan | Recorded in final scan |
| SSH password login | Initial verification pending | Disabled in SSH configuration; test evidence pending |
| SSH public-key authentication | Not configured | Enabled |
| Root SSH login | Initial verification pending | Disabled in SSH configuration |
| Firewall | Initial verification pending | UFW active; verify final status screenshot |

### Initial Lynis findings

- **Hardening index:** 63
- **Warnings:** 0
- **Suggestions:** 36

These results represent the initial audit, not the final security state of the server.

## Problems I encountered

- The NAT network initially did not provide the expected IP configuration. I enabled DHCP for the VirtualBox NAT network.
- DNS resolution did not work correctly during the initial setup.
- SSH access from Kali was initially successful, but later attempts returned a `No route to host` error.
- VirtualBox displayed a graphics compatibility warning during setup.
- When installing my SSH public key, `ssh-copy-id` initially reported that all keys were skipped because they already existed on the remote system. I used `ssh-copy-id -f` to force the installation, and it confirmed that one key was added.
- The SSH configuration settings initially had `#` before them. I learned that these lines are commented out and must be activated to apply the settings.
- I initially used the wrong SSH validation command and received an SSH usage message. I corrected it to `sudo /usr/sbin/sshd -t`.
- After installing UFW, its status still showed inactive. I learned that installing the firewall does not automatically enable it. I configured the SSH allow rule and default policies before enabling UFW.

These issues helped me understand the importance of checking network interfaces, IP addresses, DNS, SSH configuration, and service availability before conducting security tests.

## What I learned

- How to build an Ubuntu Server virtual machine in VirtualBox.
- How NAT and host-only networking work together in an isolated lab.
- How to enable and test SSH access from Kali Linux.
- How to generate and install Ed25519 SSH keys.
- How to harden SSH by disabling password authentication and root login.
- How to validate SSH daemon configuration before applying changes.
- How to configure and enable UFW with a default-deny incoming policy.
- How to use Lynis to audit Linux system security.
- How to interpret a hardening index, warnings, and suggestions.
- Why an initial security baseline is important before applying hardening changes.

## Safety and rules

- I work only on my own VMs, inside my isolated lab.
- I keep a second terminal or snapshot ready before changing SSH or firewall settings, so I can't lock myself out.
- I make one change at a time and test after each change.
- No real passwords, private keys, or sensitive private information are committed to this repository.
- I verify connectivity before running scans or changing network settings.

## Limitations

- Ubuntu 26.04.1 LTS was used instead of the originally planned Ubuntu 24.04 LTS.
- The initial Nmap scan has not yet been successfully recorded.
- The final Nmap scan and before-and-after comparison are still pending.
- The Lynis results are an initial assessment; a post-hardening audit is still required.
- SSH and firewall configuration have been updated, but final verification evidence should be retained.
- The Lynis results do not guarantee that the server is free from vulnerabilities.

Part of my [Cybersecurity Journal](/cybersec-journal/README.md).

See [Day 34 Documentation](/cybersec-journal/day-34.md) for the SSH hardening and firewall activity details.
