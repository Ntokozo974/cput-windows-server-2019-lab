# 7. Active Directory Certificate Services (10 marks)

**Owner:** [Name]
**Status:** ⬜ Not started / 🟨 In progress / ✅ Done

## Objective
Install AD CS, configure it as an Enterprise Root Certification Authority, and
issue a test certificate to a client.

## Steps

### 7.1 Install AD CS Role
- Server Manager → Add Roles and Features → Active Directory Certificate Services.
- Select role service: Certification Authority.

![Add AD CS role](../screenshots/07-certificate-services/01-add-adcs-role.png)
![Select CA role service](../screenshots/07-certificate-services/02-select-ca-service.png)

### 7.2 Configure as Enterprise Root CA
- Post-deployment configuration wizard.
- Setup type: Enterprise CA.
- CA type: Root CA.
- Create a new private key, set cryptography options (key length, hash algorithm).
- Set CA name and validity period.

![Setup type - Enterprise CA](../screenshots/07-certificate-services/03-enterprise-ca.png)
![CA type - Root CA](../screenshots/07-certificate-services/04-root-ca.png)
![Private key configuration](../screenshots/07-certificate-services/05-private-key.png)
![CA name and validity period](../screenshots/07-certificate-services/06-ca-name-validity.png)
![Configuration complete](../screenshots/07-certificate-services/07-config-complete.png)

## Testing

- On the client, open `certmgr.msc` → Personal → All Tasks → Request New Certificate.
- Or use the CA web enrollment page (if IIS/web enrollment role installed).
- Complete the enrollment and confirm the certificate is issued.
- On the server, open the Certification Authority console → Issued Certificates
  and confirm the new certificate appears.

![Request new certificate on client](../screenshots/07-certificate-services/08-request-cert-client.png)
![Certificate issued confirmation](../screenshots/07-certificate-services/09-cert-issued.png)
![CA console - issued certificates](../screenshots/07-certificate-services/10-issued-certificates.png)

## Notes / Issues Encountered
- [Add any problems and how you solved them]
