# 2. Active Directory Domain Services (20 marks)

**Owner:** [Name]
**Status:** ⬜ Not started / 🟨 In progress / ✅ Done

## Objective
Install AD DS, promote the server to a Domain Controller, create the domain
`group6.local`, build an OU structure, and create test user accounts.

## Steps

### 2.1 Install the AD DS Role
- Server Manager → Add Roles and Features → Active Directory Domain Services.

![Add roles wizard](../screenshots/02-active-directory/01-add-roles.png)
![AD DS role selected](../screenshots/02-active-directory/02-select-adds.png)
![Installation progress](../screenshots/02-active-directory/03-install-progress.png)

### 2.2 Promote to Domain Controller
- Click "Promote this server to a domain controller."
- Select "Add a new forest," domain name: `group6.local`.
- Set forest/domain functional level, DSRM password.
- Set NetBIOS name: `GROUP6`.
- Complete the wizard and allow the server to reboot.

![Add new forest](../screenshots/02-active-directory/04-new-forest.png)
![Domain controller options / DSRM password](../screenshots/02-active-directory/05-dsrm-password.png)
![NetBIOS name](../screenshots/02-active-directory/06-netbios-name.png)
![Prerequisites check](../screenshots/02-active-directory/07-prereq-check.png)
![Promotion complete / reboot](../screenshots/02-active-directory/08-promotion-complete.png)

### 2.3 Create OU Structure
- Open Active Directory Users and Computers (ADUC).
- Create OUs: `Group6_Staff`, `Group6_Students` (or as appropriate).

![ADUC - OU creation](../screenshots/02-active-directory/09-create-ou.png)

### 2.4 Create Test User Accounts
- Create at least 2–3 test users inside the OUs, set passwords compliant with policy.

![Create user account](../screenshots/02-active-directory/10-create-user.png)

## Testing
- Join `Group6-CLIENT01` to the domain: System Properties → Change → Domain: `group6.local`.
- Confirm the "Welcome to the group6.local domain" message appears.
- Log in on the client using one of the AD test accounts.

![Join domain](../screenshots/02-active-directory/11-join-domain.png)
![Welcome to domain message](../screenshots/02-active-directory/12-welcome-message.png)
![Login as domain user](../screenshots/02-active-directory/13-login-domain-user.png)

## Notes / Issues Encountered
- [Add any problems and how you solved them]
