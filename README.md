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

**Hypervisor used:** VirtualBox

---

## 🖧 Architecture

```
                     ┌───────────────────────────┐
                     │   Group X-DC01             │
                     │   Windows Server 2019      │
                     │   Roles: AD DS, DNS, DHCP, │
                     │   AD CS, Remote Access     │
                     │   Static IP: 192.168.6.10  │
                     └─────────────┬─────────────┘
                                   │
                     Internal / Host-only Network
                                   │
                     ┌─────────────┴─────────────┐
                     │   Group X-CLIENT01         │
                     │   Windows 10/11            │
                     │   Domain-joined             │
                     │   DHCP-assigned IP          │
                     └───────────────────────────┘
```

See [`docs/00-network-diagram.md`](docs/00-network-diagram.md) for a more detailed
diagram and IP addressing table.

---

## 📂 Documentation Index

| # | Section | Marks | Status |
|---|---------|-------|--------|
| 1 | [Server Installation](docs/01-server-installation.md) | 10 | ⬜ |
| 2 | [Active Directory Domain Services](docs/02-active-directory.md) | 20 | ⬜ |
| 3 | [DNS Configuration](docs/03-dns.md) | 20 | ⬜ |
| 4 | [DHCP Configuration](docs/04-dhcp.md) | 20 | ⬜ |
| 5 | [Group Policy Hardening](docs/05-group-policy.md) | 10 | ⬜ |
| 6 | [Remote Access (VPN/RRAS)](docs/06-remote-access.md) | 10 | ⬜ |
| 7 | [Certificate Services (AD CS)](docs/07-certificate-services.md) | 10 | ⬜ |

Mark each ⬜ as ✅ once that section's screenshots + write-up are complete.

---

## 🛠 Tools Used

- Windows Server 2019 (Desktop Experience)
- [Hypervisor name]
- Windows 10/11 client VM for testing
- Command-line tools: `ipconfig`, `nslookup`, `gpresult`, `gpupdate`

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
