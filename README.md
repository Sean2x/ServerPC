# Homelab Server

A self-hosted, always-on Linux server built from a repurposed HP desktop. It serves as a general-purpose platform for learning systems administration, networking, and infrastructure, and for hosting personal services (web, game, media, and self-hosted applications).

This repository is the engineering log for the project. It records the hardware, the decisions made and why, a dated build log, problems encountered and their fixes, and the steps to rebuild the system from scratch.

**Status:** Planning / pre-install

## Objectives

- Run a reliable 24/7 server on low-end hardware.
- Build hands-on experience with Linux, networking, security, containers, and service deployment.
- Host personal services, such as a website, a Minecraft server, self-hosted apps, and a media server.
- Keep every change documented and reproducible.

## Hardware

| Component | Detail |
|---|---|
| System | HP 700-215xt (SKU F9A62AV#ABA, board 2AF7) |
| CPU | Intel Core i7-4790 (4 cores / 8 threads, Haswell) |
| Memory | 16 GB DDR3 |
| Storage | Seagate ST2000DM001, ~1.8 TiB HDD |
| GPU | NVIDIA GeForce GTX 745 |
| Network | Realtek Gigabit ethernet; Broadcom 802.11n Wi-Fi |
| BIOS | AMI 80.20 (31/10/2014), Legacy mode, Secure Boot unsupported |

Trimmed system information export: [docs/system-info.txt](docs/system-info.txt) (identifiers removed).

## Decisions

Each entry records what was chosen, the alternatives considered, and the reason.

| # | Decision | Alternatives | Reasoning |
|---|---|---|---|
| 1 | Ubuntu Server LTS as the OS | Other distributions, Windows Server | LTS provides 5 years of security updates, which suits a long-running server. Large community and documentation. |
| 2 | Wipe Windows rather than dual-boot | Dual-boot | A server that can boot into another OS is not a reliable server. Dual-booting adds complexity without learning value. |
| 3 | Remote access via SSH over Tailscale | Router port forwarding | Works behind networks that block inbound connections and requires no router configuration. |
| 4 | Wired ethernet | Wi-Fi | More stable, and less setup on a headless server. |

## Build log

Dated entries. Record commands as they are run, along with the outcome.

### Template

```
### YYYY-MM-DD: Short title
Goal:
Commands:
Result:
Notes:
```

### Entries

### 2026-10-06: Recorded hardware inventory
Goal: capture the machine's specs while Windows 10 was still installed.
Commands: `msinfo32` -> File -> Export; trimmed the export to remove the computer name, username, MAC address and serial numbers.
Result: model confirmed as HP 700-215xt. BIOS is AMI 80.20 (2014) in Legacy mode, so the Ubuntu installer will use legacy/MBR boot. Secure Boot is unsupported. A Gigabit ethernet port is present. The drive is a single 1.82 TB disk with 1.78 TB free, so there is little data to preserve.
Notes: Windows reports virtualization as disabled in firmware. VT-x must be enabled in the BIOS before installing Docker or VMs.

## Problems & fixes

| Date | Problem | Cause | Fix |
|---|---|---|---|
| | | | |

## Known risks

- **Single HDD, no redundancy.** The ST2000DM001 is an older consumer drive, and this model has a poor reputation for reliability. There is no RAID, so a drive failure means data loss. Plan: monitor with SMART (`smartmontools`), keep backups of anything important on a separate device, and consider replacing it with an SSD or adding a second drive.
- **No UPS.** Power loss can corrupt data. The BIOS is set to power on after an outage to limit downtime.
- **Aging hardware.** DDR3 and a 2014-era CPU, with limited headroom for heavy workloads.

## How to rebuild

Steps to reproduce the system from scratch. Filled in as the build progresses.

1. Back up any data on the machine; the install wipes the drive.
2. BIOS (Legacy mode): enable virtualization (VT-x, currently disabled), set power-on after AC loss if available, and note boot order.
3. Write the latest Ubuntu Server LTS ISO to a USB stick (8 GB or larger) with Rufus, using MBR partition scheme and BIOS target (the machine is Legacy-only).
4. Install Ubuntu Server over wired ethernet and enable the OpenSSH server.
5. Install Tailscale and confirm remote access.
6. Harden and configure: TBD.
7. Deploy services: TBD.

## Roadmap

- [x] Record hardware details (system info export, drives, network)
- [ ] BIOS configuration
- [ ] Install Ubuntu Server LTS
- [ ] SSH and Tailscale access
- [ ] Baseline hardening (updates, firewall, key-based SSH)
- [ ] Docker
- [ ] Monitoring and SMART health checks
- [ ] Backups
- [ ] Services: website, Minecraft, media server, self-hosted apps

## Repository layout

```
README.md   Project overview, decisions, build log, rebuild steps
docs/       Supporting exports and reference material (planned)
```
