# 3. DNS Configuration (20 marks)

**Owner:** [Name]
**Status:** ⬜ Not started / 🟨 In progress / ✅ Done

## Objective
Verify the DNS server installed automatically with AD DS, configure a reverse
lookup zone, add manual DNS records, and test resolution from a client.

## Steps

### 3.1 Verify Forward Lookup Zone
- Open DNS Manager on Group6-DC01.
- Confirm the forward lookup zone `group6.local` exists automatically.

![DNS Manager - forward lookup zone](../screenshots/03-dns/01-forward-lookup-zone.png)

### 3.2 Create Reverse Lookup Zone
- DNS Manager → Reverse Lookup Zones → New Zone.
- Configure for your subnet (e.g., 192.168.6.0/24), set to update automatically.

![New reverse lookup zone wizard](../screenshots/03-dns/02-new-reverse-zone.png)
![Reverse zone created](../screenshots/03-dns/03-reverse-zone-created.png)

### 3.3 Add DNS Records
- Add an A record manually (e.g., for the client, or a placeholder server).
- Optionally add a CNAME record (alias).

![Add A record](../screenshots/03-dns/04-add-a-record.png)
![Add CNAME record](../screenshots/03-dns/05-add-cname-record.png)

## Testing
- From the client, run:
  - `nslookup group6-dc01.group6.local`
  - `nslookup <client-hostname>.group6.local`
- Confirm both resolve to the correct IP addresses.

![nslookup - DC resolution](../screenshots/03-dns/06-nslookup-dc.png)
![nslookup - client resolution](../screenshots/03-dns/07-nslookup-client.png)

## Notes / Issues Encountered
- [Add any problems and how you solved them]
