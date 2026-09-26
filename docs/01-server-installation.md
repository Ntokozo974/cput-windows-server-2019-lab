# 1. Server Installation & Initial Configuration (10 marks)

**Owner:** Ntokozo Tyanase
**Status:** ✅ Done

## Objective
Install Windows Server 2019, rename the server with the group naming convention,
assign a static IP address, and confirm basic network connectivity.

## Steps

### 1.1 Install Windows Server 2019
- Boot the VM from the Windows Server 2019 ISO.
- Select edition: Windows Server 2019 Standard Evaluation (Desktop Experience).
- Choose "Custom: Install Windows only" and select the target disk.
- Set the local Administrator password.
- Wait for installation to complete.

![OS edition selection](../screenshots/01-server-installation/01-os-edition-selection.png)
![Installation type - Custom](../screenshots/01-server-installation/02-installation-type.png)
![Set Administrator password](../screenshots/01-server-installation/03-set-administrator-password.png)
![First logon - Server Manager](../screenshots/01-server-installation/04-first-logon.png)

### 1.2 Rename the Server
- Open Server Manager → Local Server → click the current computer name.
- Rename to `Group6-DC01`, restart when prompted.

![Rename server dialog](../screenshots/01-server-installation/05-rename-server.png)
![Restart required prompt](../screenshots/01-server-installation/06-restart-required.png)
![Rename confirmed after restart](../screenshots/01-server-installation/07-rename-confirmed.png)

### 1.3 Assign a Static IP Address
- Network and Sharing Center → Change adapter settings → Properties → IPv4.
- Set static IP (192.168.6.10), subnet mask (255.255.255.0), and preferred DNS (self).

![Static IP configuration](../screenshots/01-server-installation/08-static-ip-config.png)

## Testing
- Ran `ipconfig /all` to confirm hostname and IP settings.
- Pinged the server's own address to confirm the network stack was working.

![ipconfig /all output](../screenshots/01-server-installation/09-ipconfig-test.png)
![Ping test](../screenshots/01-server-installation/10-ping-test.png)

## Notes / Issues Encountered

After joining the client VM to the same VirtualBox Internal Network as the
server, ping tests between the two machines failed intermittently with
"Destination host unreachable" and packet loss, despite correct IP/subnet
configuration on both sides.

![Troubleshooting - ping and arp failure](../screenshots/01-server-installation/11-troubleshooting-ping-arp.png)

Investigation traced this to Windows 11's Hyper-V/Virtualization-Based
Security (VBS) running on the host machine, competing with VirtualBox for the
CPU's virtualization extensions (VT-x). Software-level fixes (disabling
Hyper-V, Virtual Machine Platform, and Memory Integrity/Core Isolation) did
not fully resolve it — `Get-ComputerInfo -Property "HyperVisorPresent"` still
returned `True`. The root cause turned out to be firmware-level: the host's
BIOS had **Kernel DMA Protection** enabled, which locked the VT-x/VT-d
toggles. Disabling Kernel DMA Protection in BIOS allowed VT-x and VT-d to be
enabled directly at the firmware level.

**Resolution:** Switched both VMs' network adapters from "Internal Network"
to "NAT Network" mode in VirtualBox, and resolved the underlying host
performance issue via the BIOS-level fix above. Together, these changes
resolved the connectivity issue and significantly improved VM responsiveness.

![Ping success after switching to NAT Network](../screenshots/01-server-installation/12-ping-success-nat-network.png)
