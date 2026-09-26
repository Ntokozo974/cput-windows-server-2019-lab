# Network Diagram & IP Addressing

## Topology

```
Internet / Host Machine
        │
   [VirtualBox NAT Network: 192.168.6.0/24]
        │
        ├── Group6-DC01 (Windows Server 2019)
        │     Roles: AD DS, DNS, DHCP, AD CS, Remote Access (RRAS)
        │     Static IP: 192.168.6.10
        │     Subnet Mask: 255.255.255.0
        │     Default Gateway: (none — isolated lab network)
        │     Preferred DNS: 192.168.6.10 (self)
        │
        └── Group6-CLIENT01 (Windows 10 Pro)
              Domain-joined to group6.local
              IP assigned via DHCP (scope 192.168.6.100–200)
```

## IP Addressing Table

| Device | Hostname | Role | IP Address | Notes |
|---|---|---|---|---|
| Server 1 | Group6-DC01 | DC / DNS / DHCP / CA / RRAS | 192.168.6.10 (static) | Primary infrastructure server |
| Client 1 | Group6-CLIENT01 | Domain member | DHCP-assigned (192.168.6.100–200) | Used for testing all services |
| VPN Clients | (dynamic) | Remote Access dial-in | 192.168.6.220–230 (static pool) | Assigned by RRAS on VPN connect, separate from the DHCP scope |

## Domain Details

- **Domain name:** group6.local
- **NetBIOS name:** GROUP6
- **Forest/Domain functional level:** Windows Server 2016

## Network Notes

- Initial setup used VirtualBox **Internal Network** mode. This was switched
  to **NAT Network** after connectivity issues between the server and client
  caused by Hyper-V/Virtualization-Based Security (VBS) interference on the
  host machine — see [`docs/01-server-installation.md`](01-server-installation.md)
  for full troubleshooting detail.
- DHCP exclusion: 192.168.6.101 (used temporarily as a manual static IP
  during early Active Directory testing, before DHCP was configured).
