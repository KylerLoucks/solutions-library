# Route 53 Domain Transfer

This guide walks through transferring a domain from an external registrar to Amazon Route 53. Use the dedicated guides for GoDaddy and Cloudflare.

## Prerequisites

- You own the domain and can manage it at the current registrar
- Domain is unlocked and privacy protection is disabled (temporarily)
- You can obtain the authorization (EPP) code
- WHOIS contact email is accurate and accessible
- DNS impact understood: expect up to 48 hours for nameserver changes to propagate

## Steps at a glance

1. Prepare domain at current registrar (unlock, auth code, contacts)
2. Start transfer in Route 53 Domains and enter the authorization code
3. Approve transfer (email from registrar/ICANN)
4. Update Route 53 hosted zone and nameservers as needed
5. Validate DNS resolution

## Guides

- Transfer from GoDaddy: [GoDaddy](transfer-from-godaddy.md)
- Transfer from Cloudflare: [Cloudflare](transfer-from-cloudflare.md)
