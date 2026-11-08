# Project 3: Hardened Linux Server

A fresh Ubuntu Server VM that I audit, harden step by step, and re-test, with measurable before-and-after evidence.

## Goal
Learn what it takes to secure a Linux server, not just attack one. I want to measure the server's security before hardening, apply common defences one at a time, and show the improvement with an audit score and a before-and-after scan from my Kali attacker machine.

## Why this project
Projects 1 and 2 were about attacking and scanning. This one looks from the defender's side, so I understand why the weaknesses I practised (weak SSH logins, open ports, misconfigured permissions) matter and how they're fixed. It also supports my Linux administration and cloud security goals.

## Environment
| Machine | Role | Network adapters |
|---|---|---|
| Ubuntu Server VM | Server to harden | Adapter 1: NAT (updates), Adapter 2: Host-only (`vboxnet0`) |
| Kali Linux VM | Attacker and tester | Adapter 1: NAT, Adapter 2: Host-only (`vboxnet0`) |
| Host | Linux Mint, runs VirtualBox | n/a |

All testing happens only inside my own isolated lab.

## Plan
| Stage | What I'll do | Status |
|---|---|---|
| 1. Build | Install Ubuntu Server, create a normal admin user, and snapshot `fresh-install` | Planned |
| 2. Baseline | Run a **Lynis** audit and an **Nmap** scan from Kali, and record the score and open ports | Planned |
| 3. SSH hardening | Key-based login, disable password login, disable root login | Planned |
| 4. Firewall | Enable **UFW** with a default-deny policy and only the ports I need | Planned |
| 5. Brute-force protection | Install and configure **fail2ban** | Planned |
| 6. Updates | Enable automatic security updates | Planned |
| 7. Least privilege | Review users, sudo rights, and file permissions; remove unused services | Planned |
| 8. Logging | Review `auth.log` and `journalctl` to see failed logins and blocked attempts | Planned |
| 9. Re-test | Re-run Lynis and Nmap, and compare with the baseline | Planned |

I take a snapshot after each stage so I can roll back if something breaks.

## Evidence I will collect
- Lynis hardening score and warnings, before and after
- Nmap scan from Kali, before and after
- Screenshots of each configuration change
- A log excerpt showing a blocked or failed login attempt (my own test only)

## Results
*To be filled in as I complete each stage.*

| Measure | Before | After |
|---|---|---|
| Lynis hardening index | [score] | [score] |
| Open ports (Nmap) | [list] | [list] |
| SSH password login | [enabled] | [disabled] |
| Firewall | [off] | [on, default-deny] |

## Safety and rules
- I work only on my own VMs, inside my isolated lab.
- I keep a second terminal or snapshot ready before changing SSH or firewall settings, so I can't lock myself out.
- I make one change at a time, and test after each.
- No real passwords, keys, or private addresses are committed to this repo.

## Problems I hit
- *None yet.*

## What I learned
- *To be written after the project.*

## Limitations 
- *To be written after the project.*

Part of my [Cybersecurity Journal]([cybersec-journal/README.md]).
