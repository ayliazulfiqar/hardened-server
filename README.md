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
| 2. Baseline | Run a **Lynis** audit and an **Nmap** scan from Kali, and record the score and open ports | Done – Lynis completed; Nmap evidence pending |
| 3. SSH hardening | Key-based login, disable password login, disable root login | Planned |
| 4. Firewall | Enable **UFW** with a default-deny policy and only the ports I need | Planned |
| 5. Brute-force protection | Install and configure **fail2ban** | Planned |
| 6. Updates | Enable automatic security updates | Planned |
| 7. Least privilege | Review users, sudo rights, and file permissions; remove unused services | Planned |
| 8. Logging | Review `auth.log` and `journalctl` to see failed logins and blocked attempts | Planned |
| 9. Re-test | Re-run Lynis and Nmap, and compare with the baseline | Planned |

I take a snapshot after each stage so I can roll back if something breaks.

## Evidence collected

- Initial Lynis security audit saved as `lynis-baseline.txt`.
- Initial Lynis hardening index and findings recorded.
- SSH connection successfully tested from Kali Linux during initial setup.
- Ubuntu network configuration recorded.
- Nmap baseline scan and screenshot still need to be completed.
- Screenshots will be stored in the `screenshots/` folder.

## Results

The following table records the initial security baseline before hardening. The After column will be completed after the hardening stages.

| Measure | Before | After |
|---|---|---|
| Lynis hardening index | 63 | Pending |
| Lynis warnings | 0 | Pending |
| Lynis suggestions | 36 | Pending |
| Open ports (Nmap) | Pending scan | Pending |
| SSH password login | Pending verification | Pending |
| Firewall | Pending verification | Pending |

### Initial Lynis findings

- **Hardening index:** 63
- **Warnings:** 0
- **Suggestions:** 36

These results represent the initial audit, not the final security state of the server.

## Safety and rules

- I work only on my own VMs, inside my isolated lab.
- I keep a second terminal or snapshot ready before changing SSH or firewall settings, so I can't lock myself out.
- I make one change at a time and test after each change.
- No real passwords, private keys, or sensitive private information are committed to this repository.
- I verify connectivity before running scans or changing network settings.

## Problems I encountered

- The NAT network initially did not provide the expected IP configuration. I enabled DHCP for the VirtualBox NAT network.
- DNS resolution did not work correctly during the initial setup.
- SSH access from Kali was initially successful, but later attempts returned a `No route to host` error.
- VirtualBox displayed a graphics compatibility warning during setup.

These issues helped me understand the importance of checking network interfaces, IP addresses, DNS, and service availability before conducting security tests.

## What I learned

- How to build an Ubuntu Server virtual machine in VirtualBox.
- How NAT and host-only networking work together in an isolated lab.
- How to enable and test SSH access from Kali Linux.
- How to use Lynis to audit Linux system security.
- How to interpret a hardening index, warnings, and suggestions.
- Why an initial security baseline is important before applying hardening changes.

## Limitations

- Ubuntu 26.04.1 LTS was used instead of the originally planned Ubuntu 24.04 LTS.
- The initial Nmap scan has not yet been successfully recorded.
- The baseline does not yet include verified open-port information or a completed Nmap screenshot.
- The Lynis results are an initial assessment and do not guarantee that the server is free from vulnerabilities.

## Next steps

1. Restore connectivity between Kali Linux and Ubuntu Server.
2. Complete the Nmap service and version detection scan.
3. Save the Nmap output as `baseline-scan.txt`.
4. Save the Nmap screenshot as `nmap-baseline.png` inside `screenshots/`.
5. Take a VirtualBox snapshot named `baseline-complete`.
6. Continue with SSH hardening, firewall configuration, and the remaining stages.
7. Re-run Lynis and Nmap after hardening and compare the results.

Part of my [Cybersecurity Journal](../cybersec-journal/README.md).
