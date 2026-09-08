# 4. DHCP Configuration (20 marks)

**Owner:** [Name]
**Status:** ⬜ Not started / 🟨 In progress / ✅ Done

## Objective
Install the DHCP role, authorize it in Active Directory, create a scope, and
verify a client can obtain a lease.

## Steps

### 4.1 Install DHCP Server Role
- Server Manager → Add Roles and Features → DHCP Server.

![Add DHCP role](../screenshots/04-dhcp/01-add-dhcp-role.png)
![DHCP install complete](../screenshots/04-dhcp/02-dhcp-install-complete.png)

### 4.2 Authorize DHCP Server in AD
- DHCP console → right-click server → Authorize.
- Confirm the server icon shows a green checkmark (authorized).

![Authorize DHCP server](../screenshots/04-dhcp/03-authorize-dhcp.png)

### 4.3 Create a Scope
- New Scope Wizard: name, IP range (e.g., 192.168.6.100–200), subnet mask.
- Configure default gateway and DNS server (point to Group6-DC01).
- Set lease duration.

![New scope wizard - range](../screenshots/04-dhcp/04-scope-range.png)
![Scope options - gateway/DNS](../screenshots/04-dhcp/05-scope-options.png)
![Scope activated](../screenshots/04-dhcp/06-scope-activated.png)

### 4.4 Configure Exclusion / Reservation (extra credit)
- Add an exclusion range and/or a reservation for a specific MAC address.

![Exclusion range](../screenshots/04-dhcp/07-exclusion-range.png)
![Reservation](../screenshots/04-dhcp/08-reservation.png)

## Testing
- On the client, set the NIC to "Obtain an IP address automatically."
- Run `ipconfig /release` then `ipconfig /renew`.
- Confirm the leased IP falls within the configured scope.
- Cross-check the lease appears in DHCP console → Address Leases.

![ipconfig /renew result](../screenshots/04-dhcp/09-ipconfig-renew.png)
![DHCP console - address leases](../screenshots/04-dhcp/10-address-leases.png)

## Notes / Issues Encountered
- [Add any problems and how you solved them]
