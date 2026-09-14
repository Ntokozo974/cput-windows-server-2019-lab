# 2. Active Directory Domain Services (20 marks)

**Owner:** Ntokozo Tyanase
**Status:** ✅ Done

## Objective
Install AD DS, promote the server to a Domain Controller, create the domain
`group6.local`, build an OU structure, and create test user accounts.

## Steps

### 2.1 Install the AD DS Role
- Server Manager → Add Roles and Features → Active Directory Domain Services.

![Server Manager open](../screenshots/02-active-directory/00-server-manager-open.png)
![Add Roles and Features wizard](../screenshots/02-active-directory/01-add-roles-and-features.png)
![Before You Begin](../screenshots/02-active-directory/02-before-you-begin.png)
![Installation type - role-based](../screenshots/02-active-directory/03-installation-type.png)
![Server selection](../screenshots/02-active-directory/04-server-selection.png)
![AD DS role selected](../screenshots/02-active-directory/05a-server-roles-adds.png)
![AD DS role - add features prompt](../screenshots/02-active-directory/05b-server-roles-adds.png)
![AD DS role - confirmed](../screenshots/02-active-directory/05c-server-roles-adds.png)
![Features screen](../screenshots/02-active-directory/06-features.png)
![Confirmation before install](../screenshots/02-active-directory/07-confirmation.png)
![Installation progress](../screenshots/02-active-directory/08-install-progress.png)
![Results - installation succeeded](../screenshots/02-active-directory/09-results-install-complete.png)

### 2.2 Promote to Domain Controller
- Click "Promote this server to a domain controller."
- Select "Add a new forest," domain name: `group6.local`.
- Set forest/domain functional level, DSRM password.
- Set NetBIOS name: `GROUP6`.
- Complete the wizard and allow the server to reboot.

![Add new forest - domain name](../screenshots/02-active-directory/10-new-forest-domain-name.png)
![Domain controller options / DSRM password](../screenshots/02-active-directory/11-dc-options-dsrm.png)
![DNS delegation warning](../screenshots/02-active-directory/12-dns-delegation-warning.png)
![NetBIOS name](../screenshots/02-active-directory/13-netbios-name.png)
![Paths](../screenshots/02-active-directory/14-paths.png)
![Review options](../screenshots/02-active-directory/15-review-options.png)
![dcdiag verification of successful promotion](../screenshots/02-active-directory/16-dcdiag-verification.png)
![ADUC confirmed - group6.local visible](../screenshots/02-active-directory/17-aduc-confirmed.png)

### 2.3 Create OU Structure
- Open Active Directory Users and Computers (ADUC).
- Created OUs: `Group6_Staff`, `Group6_Students`.

![OU structure confirmed](../screenshots/02-active-directory/18-ou-structure-confirmed.png)

### 2.4 Create Test User Accounts
- Created test users inside the OUs: `ntokozo.staff` and `lethabo.student`.

![Create user - staff](../screenshots/02-active-directory/19-create-user-staff.png)
![Create user - student](../screenshots/02-active-directory/20-create-user-student.png)

## Testing
- Set a temporary static IP on the client so it could locate the domain controller.
- Joined `Group6-CLIENT01` to the domain: System Properties → Change → Domain: `group6.local`.
- Confirmed the "Welcome to the group6.local domain" message appeared.
- Restarted the client and logged in using an AD test account (`ntokozo.staff`).

![Client temporary static IP](../screenshots/02-active-directory/21-client-temp-static-ip.png)
![Domain join dialog](../screenshots/02-active-directory/22-domain-join-dialog.png)
![Welcome to the domain message](../screenshots/02-active-directory/23-welcome-to-domain.png)
![Restart required after domain join](../screenshots/02-active-directory/24-restart-required-domain-join.png)
![Successful login as domain user](../screenshots/02-active-directory/25-domain-user-logged-in.png)

## Notes / Issues Encountered

**Issue 1 — VM networking (see Section 1 for full detail):** Connectivity
between the server and client was unreliable when using VirtualBox's Internal
Network mode, caused by Hyper-V/Virtualization-Based Security (VBS) on the
host interfering with VirtualBox's virtual switch. Resolved by switching both
VMs to NAT Network mode.

**Issue 2 — Slow server performance during domain-join:** Even after fixing
basic connectivity, the initial domain-join attempt failed with "An Active
Directory domain controller for the domain 'group6.local' could not be
contacted," despite DNS and connectivity tests (`nslookup`, `nltest
/dsgetdc`) all succeeding. This was traced to the server being under-resourced
and running sluggishly (likely a combination of lingering host-side Hyper-V
overhead and insufficient allocated RAM), causing the domain-join
authentication handshake to time out.

**Resolution:** Increased the server VM's allocated RAM (from 4096 MB to
6144–8192 MB) in VirtualBox settings. This noticeably improved responsiveness,
and the domain-join and subsequent domain-user login then completed
successfully and consistently.

**Recommendation:** When running an Active Directory Domain Controller in a
virtualised lab environment, allocate generous RAM (6GB+) to the DC VM where
host resources allow, since AD DS/DNS services are noticeably sensitive to
CPU/memory contention during authentication operations.
