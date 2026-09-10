# 1\. Server Installation \& Initial Configuration (10 marks)

**Owner:** \[Name]
**Status:** ⬜ Not started / 🟨 In progress / ✅ Done

## Objective

Install Windows Server 2019, rename the server with the group naming convention,
assign a static IP address, and confirm basic network connectivity.

## Steps

### 1.1 Install Windows Server 2019

* Boot the VM from the Windows Server 2019 ISO.
* Select edition: Windows Server 2019 Standard (Desktop Experience).
* Choose "Custom: Install Windows only" and select the target disk.
* Wait for installation to complete and set the local Administrator password.

!\[Install - language selection](../screenshots/01-server-installation/01-language-selection.png)
!\[Install - disk partitioning](../screenshots/01-server-installation/02-disk-partitioning.png)
!\[Install - progress](../screenshots/01-server-installation/03-install-progress.png)
!\[Install - first logon](../screenshots/01-server-installation/04-first-logon.png)

### 1.2 Rename the Server

* Open Server Manager → Local Server → click the current computer name.
* Rename to `Group6-DC01`, restart when prompted.

!\[Rename server](../screenshots/01-server-installation/05-rename-server.png)

### 1.3 Assign a Static IP Address

* Network and Sharing Center → Change adapter settings → Properties → IPv4.
* Set static IP (e.g., 192.168.6.10), subnet mask, gateway, and preferred DNS (self).

!\[Static IP configuration](../screenshots/01-server-installation/06-static-ip.png)

## Testing

* Run `ipconfig /all` to confirm hostname and IP settings.
* Ping the host machine / another VM to confirm connectivity.

!\[ipconfig /all output](../screenshots/01-server-installation/07-ipconfig-test.png)
!\[ping test](../screenshots/01-server-installation/08-ping-test.png)

## Notes / Issues Encountered

After joining the client VM to the same VirtualBox Internal Network as the
server, ping tests between the two machines failed intermittently with
"Destination host unreachable" and packet loss, despite correct IP/subnet
configuration on both sides.

![Troubleshooting - ping and arp failure](../screenshots/01-server-installation/11-troubleshooting-ping-arp.png)

**Resolution:** Switched both VMs' network adapters from "Internal Network"
to "NAT Network" mode in VirtualBox, which resolved the connectivity issue.

![Ping success after switching to NAT Network](../screenshots/01-server-installation/12-ping-success-nat-network.png)

