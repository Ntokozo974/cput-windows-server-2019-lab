# 6. Remote Access Configuration (10 marks)

**Owner:** [Name]
**Status:** ⬜ Not started / 🟨 In progress / ✅ Done

## Objective
Install and configure the Remote Access role (RRAS) as a VPN server, and verify
a client can establish a VPN connection.

## Steps

### 6.1 Install Remote Access Role
- Server Manager → Add Roles and Features → Remote Access.
- Select role services: DirectAccess and VPN (RAS).

![Add Remote Access role](../screenshots/06-remote-access/01-add-remote-access-role.png)
![Select RAS role service](../screenshots/06-remote-access/02-select-ras.png)

### 6.2 Configure RRAS
- Open Routing and Remote Access console.
- Configure and Enable Routing and Remote Access → Custom configuration → VPN access.
- Set the IP address pool or configure DHCP relay for VPN clients.

![RRAS configuration wizard](../screenshots/06-remote-access/03-rras-wizard.png)
![VPN access selected](../screenshots/06-remote-access/04-vpn-access.png)
![IP address assignment](../screenshots/06-remote-access/05-ip-assignment.png)
![RRAS running](../screenshots/06-remote-access/06-rras-running.png)

## Testing
- From a client machine, create a new VPN connection pointing to the server's IP.
- Attempt to connect and confirm a successful connection status.

![Create VPN connection on client](../screenshots/06-remote-access/07-client-vpn-setup.png)
![VPN connected status](../screenshots/06-remote-access/08-vpn-connected.png)

> Note: If your lab network is fully isolated, note this limitation and explain
> how the connection was tested within the constraints of your setup.

## Notes / Issues Encountered
- [Add any problems and how you solved them]
