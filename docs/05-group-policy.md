# 5. Group Policy Configuration (10 marks)

**Owner:** [Name]
**Status:** ⬜ Not started / 🟨 In progress / ✅ Done

## Objective
Create a Group Policy Object (GPO) enforcing five security requirements, link it
to the relevant OU, and test that each restriction is actually applied on the client.

## GPO Setup

- Open Group Policy Management → right-click OU → "Create a GPO in this domain and link it here."
- Name it `Group6_Security_Policy`.

![Create GPO](../screenshots/05-group-policy/01-create-gpo.png)
![Link GPO to OU](../screenshots/05-group-policy/02-link-gpo.png)

## Policy Settings

### 5.1 Disable Command Prompt
**Path:** User Configuration → Policies → Administrative Templates → System →
*Prevent access to the command prompt* → Enabled

![Disable cmd setting](../screenshots/05-group-policy/03-disable-cmd-setting.png)

### 5.2 Disable Removable Media / CD / DVD / Floppy
**Path:** Computer Configuration → Policies → Administrative Templates → System →
*Removable Storage Access* → deny read/write/execute for each removable media type.

![Removable storage access settings](../screenshots/05-group-policy/04-removable-storage.png)

### 5.3 Prevent Software Installation
**Path:** Computer Configuration → Policies → Administrative Templates →
Windows Components → Windows Installer → *Disable Windows Installer* → Enabled
(and/or Software Restriction Policies)

![Disable Windows Installer setting](../screenshots/05-group-policy/05-disable-installer.png)

### 5.4 Minimum Password Length (10+ characters)
**Path:** Computer Configuration → Policies → Windows Settings → Security Settings
→ Account Policies → Password Policy → *Minimum password length* = 10

![Minimum password length setting](../screenshots/05-group-policy/06-min-password-length.png)

### 5.5 Maximum Password Age (lower than default 42 days)
**Path:** Same as above → *Maximum password age* = e.g. 30 days

![Maximum password age setting](../screenshots/05-group-policy/07-max-password-age.png)

## Testing

- On the client, run `gpupdate /force`.
- Run `gpresult /r` to confirm the GPO is applied.
- Attempt to open `cmd.exe` → should be blocked with a policy restriction message.
- Attempt to insert/use removable media → should be blocked.
- Attempt to install an application (e.g., an .msi) → should fail.
- Attempt to set a password shorter than 10 characters → should be rejected.

![gpupdate /force](../screenshots/05-group-policy/08-gpupdate-force.png)
![gpresult /r output](../screenshots/05-group-policy/09-gpresult.png)
![cmd blocked message](../screenshots/05-group-policy/10-cmd-blocked.png)
![removable media blocked](../screenshots/05-group-policy/11-removable-media-blocked.png)
![software install blocked](../screenshots/05-group-policy/12-install-blocked.png)
![short password rejected](../screenshots/05-group-policy/13-password-rejected.png)

## Notes / Issues Encountered
- [Add any problems and how you solved them]
