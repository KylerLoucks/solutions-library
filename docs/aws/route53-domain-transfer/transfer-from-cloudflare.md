# Transfer from Cloudflare Registrar to Route 53

## 1) Prepare the domain in Cloudflare

- Cloudflare Dashboard → Registrar → Your domain
- Disable transfer lock (if enabled)
- Confirm Registrant/Admin email is valid
- Retrieve the Authorization (EPP) code

DNS considerations:
- If Cloudflare currently hosts DNS, export records or recreate them in a Route 53 hosted zone
- Plan the cutover to Route 53 nameservers after transfer completes

## 2) Start the transfer in Route 53

- Route 53 → Domains → Transfer domain
- Enter the Authorization (EPP) code
- Provide contact details and confirm
- Pay the transfer fee

## 3) Approve the transfer

- Approve any email from Cloudflare/ICANN to expedite
- Transfer typically completes within days

## 4) Nameservers and DNS

- Set nameservers to the Route 53 hosted zone
- Recreate Cloudflare DNS records in Route 53 if you have not already

## 5) Validation

- Domain appears under Route 53 → Registered domains
- Nameservers updated to Route 53
- `dig`/`nslookup` return expected answers

Troubleshooting:
- If Cloudflare won’t provide an EPP code, check lock/eligibility and TLD constraints
- Some TLDs have transfer restrictions or timing windows; verify before starting
