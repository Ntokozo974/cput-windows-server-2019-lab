# 7. Active Directory Certificate Services (10 marks)

**Owner:** Ntokozo Tyanase
**Status:** ✅ Done

## Objective
Install AD CS, configure it as an Enterprise Root Certification Authority,
and issue a test certificate to a client.

## Steps

### 7.1 Install AD CS Role
- Server Manager → Add Roles and Features → Active Directory Certificate
  Services.
- Selected role service: Certification Authority.

![Add AD CS role](../screenshots/07-certificate-services/01-add-adcs-role.png)
![Select CA role service](../screenshots/07-certificate-services/02-select-ca-service.png)
![Confirmation before install](../screenshots/07-certificate-services/03-adcs-confirmation.png)
![Installation complete](../screenshots/07-certificate-services/04-adcs-install-complete.png)

### 7.2 Configure as Enterprise Root CA
- Post-deployment configuration wizard (via Server Manager notification).
- Role service: Certification Authority.
- Setup type: **Enterprise CA**.
- CA type: **Root CA**.
- Created a new private key with default cryptography settings (RSA, 2048-bit,
  SHA256).
- CA named `Group6-RootCA`, default 5-year validity period.

![AD CS role services selection](../screenshots/07-certificate-services/05-adcs-role-services.png)
![Setup type - Enterprise CA](../screenshots/07-certificate-services/06-enterprise-ca.png)
![CA type - Root CA](../screenshots/07-certificate-services/07-root-ca.png)
![Private key configuration](../screenshots/07-certificate-services/07b-private-key.png)
![CA name](../screenshots/07-certificate-services/08-ca-name.png)
![Configuration complete](../screenshots/07-certificate-services/09-adcs-config-complete.png)
![CA console confirmed - Group6-RootCA](../screenshots/07-certificate-services/10-ca-console-confirmed.png)

## Testing

- On the client, opened `certmgr.msc` → Personal → All Tasks → Request New
  Certificate.
- Initially, no certificate templates were selectable ("certificate types
  are not available"). Investigated the Certificate Templates console
  (`certtmpl.msc`) on the server and found the **User** template's Security
  permissions only had "Read" ticked for Authenticated Users — **Enroll**
  was not granted.

![Certificate templates published on the CA](../screenshots/07-certificate-services/11-certificate-templates-list.png)

- Corrected this by ensuring Authenticated Users had both **Read** and
  **Enroll** permissions ticked on the User template, then ran
  `certutil -pulse` on the server and restarted the Active Directory
  Certificate Services service to force the change to take effect.

![User template permissions - Enroll granted](../screenshots/07-certificate-services/12-user-template-permissions.png)

- Retried certificate enrollment on the client — the **User** template was
  now selectable.

![Certificate template now available](../screenshots/07-certificate-services/13-request-certificate-template.png)

- Selected **User**, clicked Enroll — certificate issued successfully.

![Certificate enrolled successfully](../screenshots/07-certificate-services/14-certificate-enrolled.png)

- Confirmed on the server, in the Certification Authority console under
  **Issued Certificates**, that the new certificate appeared correctly.

![Issued certificate confirmed on the CA](../screenshots/07-certificate-services/15-issued-certificates-confirmed.png)

## Notes / Issues Encountered

Certificate templates being published to the CA (visible in the Certificate
Templates folder) does not automatically mean a user can enroll for them —
the template's own Security permissions (managed separately via
`certtmpl.msc`) must explicitly grant the **Enroll** permission to the
relevant user or group (in this case, Authenticated Users), in addition to
Read access. After correcting this and forcing a refresh with
`certutil -pulse` plus a service restart, enrollment succeeded.
