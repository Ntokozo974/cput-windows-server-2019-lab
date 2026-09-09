# Network Diagram & IP Addressing

## Topology

```
Internet / Host Machine
        │
   [Hypervisor Virtual Network - Internal/Host-only: 192.168.6.0/24]
        │
        ├── Group X-DC01 (Windows Server 2019)
        │     Roles: AD DS, DNS, DHCP, AD CS, Remote Access (RRAS)
        │     Static IP: 192.168.6.10
        │     Subnet Mask: 255.255.255.0
        │     Default Gateway: 192.168.6.1
        │     Preferred DNS: 192.168.6.10 (self)
        │
        └── Group X-CLIENT01 (Windows 10/11)
              Domain-joined to group6.local
              IP assigned via DHCP (scope 192.168.6.100–200)
```

## IP Addressing Table

| Device | Hostname | Role | IP Address | Notes |
|---|---|---|---|---|
| Server 1 | Group6-DC01 | DC / DNS / DHCP / CA / RRAS | 192.168.6.10 (static) | Primary infrastructure server |
| Client 1 | Group6-CLIENT01 | Domain member | DHCP-assigned (192.168.6.100–200) | Used for testing all services |

## Domain Details

- **Domain name:** group6.local
- **NetBIOS name:** GROUP6
- **Forest/Domain functional level:** Windows Server 2016 (or as selected)


