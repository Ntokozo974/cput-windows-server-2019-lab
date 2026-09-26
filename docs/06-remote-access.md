# 6. Remote Access Configuration (10 marks)

**Owner:** Mahlogonolo Mkhawane
**Status:** ✅ Done

## Objective
Install and configure the Remote Access role (RRAS) as a VPN server, and
verify a client can establish a VPN connection.

## Steps

### 6.1 Install Remote Access Role
- Server Manager → Add Roles and Features → Remote Access.
- Selected role service: DirectAccess and VPN (RAS).

![Add Remote Access role](../screenshots/06-remote-access/01-add-remote-access-role.png)
![Select VPN (RAS) role service](../screenshots/06-remote-access/02-select-vpn-ras.png)
![Confirmation before install](../screenshots/06-remote-access/03-remote-access-confirmation.png)
![Installation complete](../screenshots/06-remote-access/04-remote-access-install-complete.png)

### 6.2 Configure RRAS
- Opened Routing and Remote Access console (Tools → Routing and Remote Access).
- Selected "Deploy VPN only," then configured and enabled Routing and Remote
  Access using Custom configuration → VPN access.
- Started the RRAS service.

![RRAS custom configuration selected](../screenshots/06-remote-access/05-rras-custom-configuration.png)
![VPN access selected](../screenshots/06-remote-access/06-vpn-access-selected.png)
![RRAS service running](../screenshots/06-remote-access/07-rras-service-started.png)

## Testing

- On the client, created a new VPN connection (Windows built-in VPN type)
  pointing to the server's static IP (192.168.6.10).

![Client VPN connection setup](../screenshots/06-remote-access/08-client-vpn-setup.png)

- **First attempt** failed: "The remote connection was not made because the
  name of the remote access server did not resolve." Corrected the server
  address field and confirmed VPN type was set explicitly to PPTP.

- **Second attempt** failed: "The remote connection was denied because the
  user name and password combination you provided is not recognized or the
  selected authentication protocol is not permitted on the remote access
  server." This was resolved by opening Active Directory Users and Computers,
  locating the Administrator account (in the default Users container), and
  under the Dial-in tab, setting Network Access Permission to "Allow access"
  (it defaults to "Control access through NPS Network Policy," which blocks
  dial-in without an explicit policy).

![Dial-in permission set to Allow access](../screenshots/06-remote-access/09-dialin-permission-allow.png)

- **Third attempt** (PPTP) failed with "A connection to the remote computer
  could not be established." An attempt with SSTP instead failed with "An
  existing connection was forcibly closed by the remote host" — expected,
  since SSTP requires an SSL/TLS certificate on the RRAS server, which was
  not yet configured at this point (see Section 7).
- Investigated the RRAS server's IPv4 address assignment setting, which was
  set to "DHCP." Switched this to a **Static address pool**
  (192.168.6.220–192.168.6.230), chosen to avoid overlapping with the
  existing DHCP scope (192.168.6.100–200).

![RRAS static address pool configured](../screenshots/06-remote-access/10-rras-static-pool.png)

- Retried the VPN connection using PPTP with `GROUP6\Administrator`
  credentials — **connection succeeded.**

![VPN connection successful](../screenshots/06-remote-access/11-vpn-connected-success.png)

## Notes / Issues Encountered

The VPN connection initially failed for several distinct reasons, resolved
in sequence:
1. Server address typed incorrectly / VPN type not explicitly set — fixed by
   re-entering the IP and setting VPN type to PPTP.
2. Domain Administrator account lacked explicit dial-in permission — fixed
   via the Dial-in tab in Active Directory Users and Computers.
3. RRAS was assigning client addresses via DHCP rather than a dedicated
   static pool, which prevented the VPN tunnel from establishing correctly —
   fixed by configuring a static address pool separate from the DHCP scope.

SSTP was also attempted as an alternative protocol but failed as expected,
since it requires a server-side SSL certificate not yet configured at this
stage of the project (addressed in Section 7: Certificate Services).
