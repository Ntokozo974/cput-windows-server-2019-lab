# Group 6 — Windows Server 2019 Infrastructure Lab

**Institution:** Cape Peninsula University of Technology (CPUT)
**Module:** OPS260s
**Team:** Group 6
**Members:**
- Ntokozo Tyanase — 250070480
- Kungawo Mpengesi — 250078732
- Lethabo Ramatlhape — 251511588
- Thapelo Vundla — 231248822
- Mahlogonolo Mkhawane — 251565998
- Amogelang Tshutse — 250336405

> This repository documents the design and implementation of a Windows Server 2019
> environment as part of a university systems administration assignment. It covers
> Active Directory Domain Services, DNS, DHCP, Group Policy hardening, Remote Access
> (VPN/RRAS), and Active Directory Certificate Services.

---

## 📌 Project Overview

CPUT is expanding its operations and requires a secure, centrally-managed IT
infrastructure. This lab simulates that environment using a virtualised Windows
Server 2019 deployment, configured and tested end-to-end.

**Hypervisor used:** Oracle VM VirtualBox

---

## 🖧 Architecture

                 ┌───────────────────────────┐
                 │   Group6-DC01              │
                 │   Windows Server 2019      │
                 │   Roles: AD DS, DNS, DHCP, │
                 │   AD CS, Remote Access     │
                 │   Static IP: 192.168.6.10  │
                 └─────────────┬─────────────┘
                               │
                 VirtualBox NAT Network
                               │
                 ┌─────────────┴─────────────┐
                 │   Group6-CLIENT01          │
                 │   Windows 10 Pro           │
                 │   Domain-joined             │
                 │   DHCP-assigned IP          │
                 │   (192.168.6.100–200 range) │
                 └───────────────────────────┘

See [`docs/00-network-diagram.md`](docs/00-network-diagram.md) for a more detailed
diagram and IP addressing table.

> **Note:** Network mode was switched from VirtualBox "Internal Network" to
> "NAT Network" during setup, after connectivity issues caused by Hyper-V/VBS
> interference on the host machine. See the Host Machine Recommendations
> section below, and [`docs/01-server-installation.md`](docs/01-server-installation.md)
> for the full troubleshooting write-up.

---

## 📂 Documentation Index

| # | Section | Marks | Status |
|---|---------|-------|--------|
| 1 | [Server Installation](docs/01-server-installation.md) | 10 | ✅ |
| 2 | [Active Directory Domain Services](docs/02-active-directory.md) | 20 | ✅ |
| 3 | [DNS Configuration](docs/03-dns.md) | 20 | ✅ |
| 4 | [DHCP Configuration](docs/04-dhcp.md) | 20 | ✅ |
| 5 | [Group Policy Hardening](docs/05-group-policy.md) | 10 | ✅ |
| 6 | [Remote Access (VPN/RRAS)](docs/06-remote-access.md) | 10 | ✅ |
| 7 | [Certificate Services (AD CS)](docs/07-certificate-services.md) | 10 | ✅ |

**All sections complete — 100/100 marks worth of configuration and testing documented.**

---

## 🛠 Tools Used

- Windows Server 2019 Standard Evaluation (Desktop Experience)
- Windows 10 Pro (client)
- Oracle VM VirtualBox
- Command-line tools: `ipconfig`, `ping`, `arp`, `nslookup`, `dcdiag`, `nltest`,
  `gpupdate`, `gpresult`, `net user`, `certutil`

---

## 💻 Host Machine Recommendations (Lessons Learned)

During setup, VM performance was severely degraded and networking between the
server and client VMs was unreliable, despite correct IP/subnet configuration
on both sides. This was traced to **Windows 11's Hyper-V / Virtualization-Based
Security (VBS)** running in the background, competing with VirtualBox for the
host CPU's virtualization extensions (VT-x).

If setting this lab up on a similar Windows 11 host, the following (in order)
resolved the issue completely:

1. Disable Hyper-V, Virtual Machine Platform, and Windows Hypervisor Platform
   under **Windows Features** (`optionalfeatures`).
2. Turn off **Memory Integrity** under Windows Security → Device Security →
   Core Isolation.
3. Run `bcdedit /set hypervisorlaunchtype off` in an elevated PowerShell.
4. If `Get-ComputerInfo -Property "HyperVisorPresent"` still returns `True`
   after the above (common on Lenovo devices with Credential Guard), the
   BIOS/UEFI itself may be locking VT-x/VT-d. In BIOS: disable **Kernel DMA
   Protection** first, then VT-x and VT-d become toggleable — enable both.
   Disabling **Secure Boot** may also be required.
5. As an additional, VirtualBox-side fix: switching both VMs' network
   adapters from **Internal Network** to **NAT Network** resolved persistent
   "Destination host unreachable" errors between the two VMs, even before
   the Hyper-V issue was fully resolved.

Together, these changes took VM performance from frequent freezes and
intermittent domain-join/authentication timeouts to fully stable, responsive
operation. Full details are documented in
[`docs/01-server-installation.md`](docs/01-server-installation.md) and
[`docs/02-active-directory.md`](docs/02-active-directory.md).

---

## 👥 Team Contributions

| Member | Student Number | Section(s) Owned |
|---|---|---|
| Ntokozo Tyanase | 250070480 | [TBD] |
| Kungawo Mpengesi | 250078732 | [TBD] |
| Lethabo Ramatlhape | 251511588 | [TBD] |
| Thapelo Vundla | 231248822 | [TBD] |
| Mahlogonolo Mkhawane | 251565998 | [TBD] |
| Amogelang Tshutse | 250336405 | [TBD] |

---

## 📚 References

See [`references.md`](references.md) for the full reference list (books, journal
articles, conference proceedings, online resources).

---

## ⚠️ Academic Integrity Note

This repository documents coursework completed for an academic module at CPUT.
It is shared for portfolio/learning purposes. If you are a current CPUT student
taking this module, please do not copy this work — consult your lecturer and
follow your institution's academic integrity policy.
