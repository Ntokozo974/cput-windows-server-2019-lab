# 7. Active Directory Certificate Services (10 marks)

**Owner:** Amogelang Tshutse
**Status:** ✅ Done

## Objective
Install AD CS, configure it as an Enterprise Root Certification Authority,
and issue a test certificate to a client to confirm it actually works.

## Steps

### 7.1 Install AD CS Role
Server Manager → Add Roles and Features → Active Directory Certificate
Services, with the role service set to Certification Authority.

![Add AD CS role](../screenshots/07-certificate-services/01-add-adcs-role.png)
![Select CA role service](../screenshots/07-certificate-services/02-select-ca-service.png)
![Confirmation before install](../screenshots/07-certificate-services/03-adcs-confirmation.png)
![Installation complete](../screenshots/07-certificate-services/04-adcs-install-complete.png)

### 7.2 Configure as Enterprise Root CA
Ran the post-deployment configuration wizard from the Server Manager
notification. Set up Certification Authority as an **Enterprise CA**, with
CA type **Root CA** since this is the only CA in the setup — nothing above it
in a hierarchy. Created a new private key with the default cryptography
settings (RSA, 2048-bit, SHA256), named the CA `Group6-RootCA`, and kept the
default 5-year validity period.

![AD CS role services selection](../screenshots/07-certificate-services/05-adcs-role-services.png)
![Setup type - Enterprise CA](../screenshots/07-certificate-services/06-enterprise-ca.png)
![CA type - Root CA](../screenshots/07-certificate-services/07-root-ca.png)
![Private key configuration](../screenshots/07-certificate-services/07b-private-key.png)
![CA name](../screenshots/07-certificate-services/08-ca-name.png)
![Configuration complete](../screenshots/07-certificate-services/09-adcs-config-complete.png)
![CA console confirmed - Group6-RootCA](../screenshots/07-certificate-services/10-ca-console-confirmed.png)

## Testing

On the client, opened `certmgr.msc` → Personal → All Tasks → Request New
Certificate — but no templates were selectable at all, with the message
"certificate types are not available." Went back to the server and checked
the Certificate Templates console (`certtmpl.msc`), and found the issue: the
**User** template's Security permissions only had "Read" ticked for
Authenticated Users — **Enroll** hadn't been granted.

![Certificate templates published on the CA](../screenshots/07-certificate-services/11-certificate-templates-list.png)

Fixed it by ticking both **Read** and **Enroll** for Authenticated Users on
the User template, then ran `certutil -pulse` on the server and restarted
the Active Directory Certificate Services service to make sure the change
actually took effect.

![User template permissions - Enroll granted](../screenshots/07-certificate-services/12-user-template-permissions.png)

Retried the certificate request on the client, and this time **User** showed
up as a selectable template.

![Certificate template now available](../screenshots/07-certificate-services/13-request-certificate-template.png)

Selected it, clicked Enroll, and the certificate was issued successfully.

![Certificate enrolled successfully](../screenshots/07-certificate-services/14-certificate-enrolled.png)

Double-checked on the server side too — opened the Certification Authority
console and confirmed the new certificate was listed under Issued
Certificates.

![Issued certificate confirmed on the CA](../screenshots/07-certificate-services/15-issued-certificates-confirmed.png)

## Notes / Issues Encountered

Main lesson from this section: a certificate template showing up in the
Certificate Templates folder on the CA doesn't mean users can actually
enroll for it. The template's own Security permissions (set separately, via
`certtmpl.msc`) need to explicitly grant **Enroll** to whichever user or
group should be able to request it — Read access on its own isn't enough.
Once that was corrected and the change was forced through with
`certutil -pulse` and a service restart, enrollment worked as expected.
