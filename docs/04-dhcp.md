# 4. DHCP Configuration (20 marks)

**Owner:** Lethabo Ramatlhape
**Status:** ✅ Done

## Objective
Install the DHCP role, authorize it in Active Directory, create a scope, and
verify a client can automatically obtain a lease.

## Steps

### 4.1 Install DHCP Server Role
Server Manager → Add Roles and Features → DHCP Server.

![Add DHCP role](../screenshots/04-dhcp/01-add-dhcp-role.png)
![DHCP confirmation screen](../screenshots/04-dhcp/02-dhcp-confirmation.png)
![DHCP install complete](../screenshots/04-dhcp/03-dhcp-install-complete.png)

### 4.2 Authorize DHCP Server in AD
An unauthorized DHCP server won't hand out leases on a domain network, so
this step is easy to forget but essential. DHCP console → right-clicked the
server → Authorize. The server icon then showed a green checkmark, confirming
it was authorized.

![Authorize DHCP server](../screenshots/04-dhcp/04-authorize-dhcp.png)

### 4.3 Create a Scope
Used the New Scope Wizard to set up `Group6_Scope`, covering
192.168.6.100–200 with a subnet mask of 255.255.255.0. Excluded
192.168.6.101, since that address had been used earlier as a manual static
IP during Active Directory and DNS testing (Sections 1–3), before DHCP
existed. DNS options were set to point clients to `group6.local` and DNS
server 192.168.6.10. Scope was activated immediately.

![Scope IP address range](../screenshots/04-dhcp/05-scope-ip-range.png)
![Scope DNS options](../screenshots/04-dhcp/06-scope-dns-options.png)
![Scope activated in console](../screenshots/04-dhcp/07-scope-activated.png)

## Testing

On the client, switched the network adapter back to "Obtain an IP address
automatically" and "Obtain DNS server address automatically" — it had been
set to a manual static IP for the earlier domain-join testing. Ran
`ipconfig /release` followed by `ipconfig /renew`, and the client picked up
192.168.6.100 — the first address in the scope, correctly skipping the
excluded 192.168.6.101. Cross-checked the DHCP console under Address Leases
and the entry matched the client exactly.

![Client set to obtain IP automatically](../screenshots/04-dhcp/08-client-dhcp-enabled.png)
![ipconfig /renew result - 192.168.6.100 assigned](../screenshots/04-dhcp/09-ipconfig-renew.png)
![DHCP console - address lease confirmed](../screenshots/04-dhcp/10-address-leases.png)

## Notes / Issues Encountered

While reconfiguring the client's network adapter settings, a User Account
Control prompt requested elevated credentials — the domain user logged in at
the time (`ntokozo.staff`) didn't have sufficient local permissions on the
client machine to make network changes. Authenticating with the client's
local Administrator account (`.\Administrator`) at the prompt resolved this,
after which the adapter settings could be changed without further issue.
