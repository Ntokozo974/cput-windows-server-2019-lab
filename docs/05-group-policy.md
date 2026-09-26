# 5. Group Policy Configuration (10 marks)

**Owner:** Thapelo Vundla
**Status:** ✅ Done

## Objective
Create a Group Policy Object (GPO) enforcing five security requirements, link
it to the domain, and test that each restriction is applied on the client.

## GPO Setup

- Group Policy Management → right-click `group6.local` → "Create a GPO in
  this domain, and Link it here..."
- Named it `Group6_Security_Policy`, linked at the domain level so it applies
  to all users and computers in `group6.local`.

![Create GPO](../screenshots/05-group-policy/01-create-gpo.png)
![GPO linked to domain](../screenshots/05-group-policy/02-gpo-linked.png)

## Policy Settings

### 5.1 Disable Command Prompt
**Path:** User Configuration → Policies → Administrative Templates → System →
*Prevent access to the command prompt* → Enabled

![Disable cmd setting](../screenshots/05-group-policy/03-disable-cmd-setting.png)

### 5.2 Disable Removable Media / CD / DVD / Floppy
**Path:** Computer Configuration → Policies → Administrative Templates →
System → *Removable Storage Access* → deny read/write access for Removable
Disks, CD and DVD, and Floppy Drives.

![Removable storage access settings](../screenshots/05-group-policy/04-removable-storage-settings.png)

### 5.3 Prevent Software Installation
**Path:** Computer Configuration → Policies → Administrative Templates →
Windows Components → Windows Installer → *Turn Off Windows Installer* →
Enabled, set to "Always."

![Disable Windows Installer setting](../screenshots/05-group-policy/05-disable-installer-setting.png)

### 5.4 Minimum Password Length (10+ characters)
**Path:** Computer Configuration → Policies → Windows Settings → Security
Settings → Account Policies → Password Policy → *Minimum password length* = 10

![Minimum password length setting](../screenshots/05-group-policy/06-min-password-length.png)

### 5.5 Maximum Password Age (lower than default 42 days)
**Path:** Same as above → *Maximum password age* = 30 days
(Windows automatically adjusted Minimum password age to 29 days to keep it
below the new maximum — expected behaviour, not an error.)

![Maximum password age setting](../screenshots/05-group-policy/07-max-password-age.png)

## Testing

- Ran `gpupdate /force` on the client (via PowerShell, since Command Prompt
  was already blocked by this point) to confirm policy application.

![gpupdate /force](../screenshots/05-group-policy/08-gpupdate-force.png)

- Attempted to open Command Prompt on the client — blocked with a policy
  restriction message.

![cmd blocked message](../screenshots/05-group-policy/09-cmd-blocked.png)

- Attempted to set a password shorter than 10 characters — rejected. Then set
  a valid 10+ character password — accepted.

![Invalid password attempt](../screenshots/05-group-policy/10a-invalid-password-attempt.png)
![Password rejected](../screenshots/05-group-policy/10b-password-rejected.png)
![Valid password attempt](../screenshots/05-group-policy/10c-valid-password-attempt.png)
![Password accepted](../screenshots/05-group-policy/10d-password-accepted.png)

- Mounted an ISO to the client's virtual CD/DVD drive and attempted to browse
  it in File Explorer — access was denied, confirming the removable storage
  restriction was working.

![Removable media blocked](../screenshots/05-group-policy/11-removable-media-blocked.png)

## Notes / Issues Encountered

**Software installer test:** Attempting to enable a built-in Windows feature
("Containers") via *Turn Windows features on or off* succeeded despite the
"Turn Off Windows Installer" policy being active. This is expected — that
particular Windows feature uses the DISM/servicing stack rather than the
classic Windows Installer (MSI) service targeted by this policy, so it was
not a suitable test case. The setting itself was correctly configured and
verified via Group Policy Management; a true MSI-based install test was not
performed due to the isolated lab network having no external installer
packages available.
