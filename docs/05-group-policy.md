# 5. Group Policy Configuration (10 marks)

**Owner:** Thapelo Vundla
**Status:** ✅ Done

## Objective
Create a Group Policy Object (GPO) enforcing five security requirements, link
it to the domain, and test that each restriction is actually applied on the
client.

## GPO Setup

Group Policy Management → right-clicked `group6.local` → "Create a GPO in
this domain, and Link it here..." Named it `Group6_Security_Policy` and
linked it at the domain level, so it applies to every user and computer in
`group6.local` rather than just one OU.

![Create GPO](../screenshots/05-group-policy/01-create-gpo.png)
![GPO linked to domain](../screenshots/05-group-policy/02-gpo-linked.png)

## Policy Settings

### 5.1 Disable Command Prompt
**Path:** User Configuration → Policies → Administrative Templates → System →
*Prevent access to the command prompt* → Enabled

![Disable cmd setting](../screenshots/05-group-policy/03-disable-cmd-setting.png)

### 5.2 Disable Removable Media / CD / DVD / Floppy
**Path:** Computer Configuration → Policies → Administrative Templates →
System → *Removable Storage Access* → denied read/write access for Removable
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

### 5.5 Maximum Password Age (lower than the default 42 days)
**Path:** Same as above → *Maximum password age* = 30 days. Windows
automatically adjusted the minimum password age down to 29 days to keep it
below the new maximum — expected behaviour, not an error.

![Maximum password age setting](../screenshots/05-group-policy/07-max-password-age.png)

## Testing

Ran `gpupdate /force` on the client via PowerShell (Command Prompt was
already blocked by this point) to confirm the policy had applied.

![gpupdate /force](../screenshots/05-group-policy/08-gpupdate-force.png)

Tried opening Command Prompt on the client — blocked, with a clear policy
restriction message.

![cmd blocked message](../screenshots/05-group-policy/09-cmd-blocked.png)

Tried setting a password shorter than 10 characters — rejected. Then tried a
valid password of 10+ characters — accepted without issue.

![Invalid password attempt](../screenshots/05-group-policy/10a-invalid-password-attempt.png)
![Password rejected](../screenshots/05-group-policy/10b-password-rejected.png)
![Valid password attempt](../screenshots/05-group-policy/10c-valid-password-attempt.png)
![Password accepted](../screenshots/05-group-policy/10d-password-accepted.png)

Mounted an ISO to the client's virtual CD/DVD drive and tried browsing it in
File Explorer — access was denied, confirming the removable storage
restriction was actually working, not just configured on paper.

![Removable media blocked](../screenshots/05-group-policy/11-removable-media-blocked.png)

## Notes / Issues Encountered

Tried testing the software installer restriction by enabling a built-in
Windows feature ("Containers") through *Turn Windows features on or off* —
it went through successfully despite the "Turn Off Windows Installer" policy
being active. That turned out not to be a fair test: this particular Windows
feature runs through the DISM/servicing stack rather than the classic Windows
Installer (MSI) service the policy actually targets. The setting itself was
confirmed correctly configured in Group Policy Management; a genuine
MSI-based install test wasn't possible since the isolated lab network had no
external installer packages available to test against.
