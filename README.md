# Project : Hardened Linux Server

A fresh Ubuntu Server VM that I audit, harden step by step, and re-test, with measurable before-and-after evidence.

## Goal

Learn what it takes to secure a Linux server, not just attack one. I want to measure the server's security before hardening, apply common defences one at a time, and show the results with an audit score and before-and-after scans from my Kali testing machine.

## Why this project

This project looks at cybersecurity from the defender's side, helping me understand why weaknesses such as weak SSH logins, unnecessary open ports, and misconfigured permissions matter, and how to fix them.

It also supports my Linux administration and cloud security goals.

## Environment

| Machine | Role | Network adapters |
|---|---|---|
| Ubuntu Server VM | Server to harden | Adapter 1: NAT Network (`Kali-NATNetwork`), Adapter 2: Host-only (`vboxnet0`) |
| Kali Linux VM | Attacker and tester | Adapter 1: NAT, Adapter 2: Host-only (`vboxnet0`) |
| Host | Linux Mint, runs VirtualBox | n/a |

**Ubuntu version:** Ubuntu 26.04.1 LTS

**Ubuntu username:** `vboxuser`

**Ubuntu network addresses recorded during setup:**
- NAT: `10.0.0.3`
- Host-only: `192.168.56.104`

All testing happens only inside my own isolated lab. Network addresses may change depending on the current VM configuration.

## Plan

| Stage | What I'll do | Status |
|---|---|---|
| 1. Build | Install Ubuntu Server, create a normal admin user, configure networking, and take the `fresh-install` snapshot | Done |
| 2. Baseline | Run a Lynis audit and Nmap scan from Kali, and record the score and open ports | Done – baseline and re-test scans performed |
| 3. SSH hardening | Configure key-based login, disable password login, disable root login, and limit authentication attempts | Done |
| 4. Firewall | Configure UFW with a default-deny incoming policy and allow SSH | Done |
| 5. Brute-force protection | Install and configure Fail2ban | Done – SSH ban verification still requires troubleshooting |
| 6. Updates | Enable automatic security updates | Done |
| 7. Least privilege | Review users, sudo rights, and file permissions | Done |
| 8. Logging | Review SSH authentication logs and failed login attempts | Done |
| 9. Re-test | Re-run Lynis and Nmap, and compare with the baseline | Done |

I take snapshots after each stage so I can roll back if something breaks.

## Evidence collected

### Baseline assessment

- Initial Lynis security audit saved as `lynis-baseline.txt`.
- Initial Lynis hardening index and findings recorded.
- Nmap scans performed before and after hardening.
- SSH connection successfully tested from Kali Linux.

### SSH hardening

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

### Firewall

- UFW configured to allow SSH, deny incoming connections by default, and allow outgoing connections.
- UFW enabled and checked using `sudo ufw status verbose`.

### Additional hardening

- Fail2ban installed and SSH jail configured.
- Automatic security updates configured using unattended upgrades.
- Sudo group membership and listening services reviewed.
- SSH authentication logs reviewed using `journalctl`.
- Final Lynis security audit completed and saved as `lynis-after.txt`.

**Note:** Fail2ban was configured, but a successful ban of the Kali IP was not confirmed during testing.

## Screenshots

Screenshots are stored in the `screenshots/` folder.

## Results

The following table records the initial security baseline and the final hardening assessment.

| Measure | Before | After |
|---|---|---|
| Lynis hardening index | 63 | 61 |
| Lynis warnings | 0 | 1 |
| Lynis suggestions | 36 | 44 |
| Lynis tests performed | Initial audit | 257 |
| SSH password login | Baseline state | Disabled in SSH configuration |
| SSH public-key authentication | Not configured | Enabled |
| Root SSH login | Baseline state | Disabled in SSH configuration |
| SSH maximum authentication attempts | Baseline state | 3 |
| Firewall | Initial state | UFW configured and enabled |
| Fail2ban | Not configured | Installed and SSH jail configured |
| Automatic updates | Not configured | Configured |
| Least privilege | Not reviewed | Sudo users and privileges reviewed |
| Logging | Not reviewed | SSH authentication logs reviewed |
| Nmap open ports | Recorded in baseline scan | Recorded in final scan |

### Initial Lynis findings

- **Hardening index:** 63
- **Warnings:** 0
- **Suggestions:** 36

### Final Lynis findings

- **Hardening index:** 61
- **Warnings:** 1
- **Suggestions:** 44
- **Tests performed:** 257
- **Lynis version:** 3.1.6

The final hardening index decreased by 2 points compared with the initial assessment. The final audit also reported one warning and 44 suggestions. These findings show that additional security improvements may still be needed.

The Lynis score alone does not determine whether the server is secure. The remaining findings need to be reviewed individually.

## Problems I encountered

- The NAT network initially did not provide the expected IP configuration. I enabled DHCP for the VirtualBox NAT network.
- DNS resolution did not work correctly during the initial setup.
- SSH access from Kali was initially successful, but later attempts returned a `No route to host` error.
- VirtualBox displayed a graphics compatibility warning during setup.
- When installing my SSH public key, `ssh-copy-id` initially reported that all keys were skipped because they already existed on the remote system. I used `ssh-copy-id -f` to force the installation, and it confirmed that one key was added.
- The SSH configuration settings initially had `#` before them. I learned that these lines are commented out and must be activated to apply the settings.
- I initially used the wrong SSH validation command and received an SSH usage message. I corrected it to `sudo /usr/sbin/sshd -t`.
- After installing UFW, its status still showed inactive. I learned that installing the firewall does not automatically enable it. I configured the SSH allow rule and default policies before enabling UFW.
- During Fail2ban testing, repeated failed login attempts from Kali did not result in the expected IP ban. The configuration was created, but the ban test still needs troubleshooting.
- The final Lynis score was 61 instead of the initial 63. I recorded the result rather than assuming that the score must increase.

These issues helped me understand the importance of checking network interfaces, IP addresses, DNS, SSH configuration, service availability, and security logs before conducting security tests.

## What I learned

- How to build an Ubuntu Server virtual machine in VirtualBox.
- How NAT and host-only networking work together in an isolated lab.
- How to enable and test SSH access from Kali Linux.
- How to generate and install Ed25519 SSH keys.
- How to harden SSH by disabling password authentication and root login.
- How to validate SSH daemon configuration before applying changes.
- How to configure and enable UFW with a default-deny incoming policy.
- How to install and configure Fail2ban.
- How to enable automatic security updates.
- How to review Linux sudo users and privileges.
- How to inspect SSH authentication logs.
- How to use Lynis to audit Linux system security.
- How to interpret a hardening index, warnings, and suggestions.
- Why an initial security baseline is important before applying hardening changes.
- How to compare network scans before and after hardening.

## Safety and rules

- I work only on my own VMs, inside my isolated lab.
- I keep a second terminal or snapshot ready before changing SSH or firewall settings, so I cannot lock myself out.
- I make one change at a time and test after each change.
- No real passwords, private keys, or sensitive private information are committed to this repository.
- I verify connectivity before running scans or changing network settings.

## Limitations and remaining work

- Ubuntu 26.04.1 LTS was used instead of the originally planned Ubuntu 24.04 LTS.
- Fail2ban's successful IP-ban behaviour still requires verification.
- The final Lynis audit reported one warning and 44 suggestions that may need further investigation.
- The final Lynis hardening index was 61, compared with the baseline score of 63.
- The Nmap before-and-after scans were performed; the actual port differences should be documented using the saved scan results.
- Screenshots missed during the original work are being recreated from the current server state. Historical evidence is used only where it was saved.
- The Lynis results do not guarantee that the server is free from vulnerabilities.

Part of my [Cybersecurity Journal](cybersec-journal/README.md).

See [Day 33 Documentation](cybersec-journal/day-33.md) for the record and baseline details.

See [Day 34 Documentation](cybersec-journal/day-34.md) for SSH hardening and firewall evidence.

See [Day 35 Documentation](cybersec-journal/day-35.md) for Fail2ban, automatic updates, least-privilege review, logging, and the final Lynis audit.
