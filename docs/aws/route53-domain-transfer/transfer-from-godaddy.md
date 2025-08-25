# Transfer from GoDaddy to Route 53

Choose one of the following approaches:

- Option 1 (Recommended): Transfer the GoDaddy registrar to AWS Route 53.
- Option 2: Copy Route 53 NS records into GoDaddy to make Route 53 the DNS record manager, while GoDaddy remains the registrar.

---

## Option 1 (Recommended): Transfer registrar to Route 53

Follow these steps to transfer your domain from GoDaddy to Amazon Route 53. GoDaddy-specific steps include reference links.

### Steps

1) Sign in to GoDaddy

- GoDaddy Dashboard → Domains → Select your domain

2) Turn off domain lock

- Disable the registrar lock so the domain can transfer

3) Export the zone file (backup of DNS records)

- Reference: [Export my domain's zone file records (GoDaddy)](https://www.godaddy.com/help/export-my-domains-zone-file-records-4166)

4) Get the auth (EPP) code

- Option A: Per-domain auth code — Reference: [Get the auth code for my domain (GoDaddy)](https://www.godaddy.com/help/get-the-auth-code-for-my-domain-1685)

- Option B: Export CSV (includes auth codes) — Reference: [Export a list of my domains (GoDaddy)](https://www.godaddy.com/help/export-a-list-of-my-domains-3681)

5) Create hosted zone(s) in Route 53 (terraform/cfn recommended)

- Route 53 → Hosted zones → Create hosted zone(s) matching the domain(s) you are transferring

- Recreate/import DNS records using the exported zone file as a reference (adjust TTLs if needed)

Optional: Manage DNS with Terraform (import existing records)

```bash
# Look up the hosted zone ID
aws route53 list-hosted-zones-by-name \
  --dns-name example.com \
  --query 'HostedZones[0].Id' \
  --output text

# Import the hosted zone into Terraform state
terraform import aws_route53_zone.main Z1234567890ABC

# Import individual records (examples)
# Root A record
terraform import 'aws_route53_record.root_a' Z1234567890ABC_example.com_A

# www CNAME
terraform import 'aws_route53_record.www_cname' Z1234567890ABC_www.example.com_CNAME
```

- Create matching Terraform resources (aws_route53_zone, aws_route53_record) before importing.

- After import, run `terraform plan` to reconcile attributes like TTL and record values.

- Reference: [Terraform aws_route53_zone import](https://registry.terraform.io/providers/hashicorp/aws/latest/docs/resources/route53_zone#import), [Terraform aws_route53_record import](https://registry.terraform.io/providers/hashicorp/aws/latest/docs/resources/route53_record#import)

If you're importing DNS records into a terraform module, run `terraform plan` to determine the module key

```bash
Terraform will perform the following actions:

  # module.records.aws_route53_record.this[" MX"] will be created
  + resource "aws_route53_record" "this" {
      + allow_overwrite = false
      + fqdn            = (known after apply)
      + id              = (known after apply)
      + name            = "example.com"
      + records         = [
          + "0 smtp.google.com.",
        ]
      + ttl             = 3600
      + type            = "MX"
      + zone_id         = "Z06353253219H5P6ZZ7L"
    }
```
Then you can easily import based on what the comment at the top says:

!!!warning "module.records.aws_route53_record.this[" MX"] will be created"
```bash
terraform import 'module.records.aws_route53_record.this[" MX"]' Z06353253219H5P6ZZ7L_example.com_MX
```
!!!note "The import value should follow this pattern: `<ZONE_ID><FQDN><TYPE>`"

6) Start the transfer in Route 53

- Route 53 → Registered domains → Transfer domain → Enter the domain → Check transferability

7) Enter contact information and the auth code

- Provide the Authorization (EPP) code obtained in step 4

- Confirm WHOIS contact details

- Pay the transfer fee (typically includes 1-year renewal)

8) Approve and complete transfer

- Approve the email(s) from GoDaddy/ICANN to expedite completion

- Transfer typically completes within 1-10 days otherwise

### After transfer: Configure Nameservers

1) Set the domain’s nameservers to the Route 53 hosted zone values.

- Route 53 → Hosted Zones → `example.com` → Note down the NS record values (they look similar to the following): 

```bash
ns-1234.awsdns-20.co.uk.
ns-123.awsdns-16.com.
ns-1234.awsdns-23.org.
ns-123.awsdns-50.net.
```

- Route 53 → Registered Domains → `example.com` → Actions → Edit name servers → Add all of the values you noted in the previous step.

2) Validate your Route 53 records match the previous zone and resolve correctly

### Validation

- Domain shows under Route 53 → Registered domains

- Nameservers reflect Route 53 hosted zone

- `dig`/`nslookup` return expected answers

- Verify nameserver delegation and registrar data:

```bash
# Authoritative nameservers for the domain
host -t NS example.com

# Registrar and nameserver data from WHOIS
whois example.com
```

### Troubleshooting

- If EPP code fails, regenerate a new code in GoDaddy and retry

- If transfer is stuck, check email approvals and lock/Privacy status

- Some TLDs have special transfer rules or timing windows; verify before starting

---

## Option 2: Route 53 for DNS only (keep GoDaddy as registrar)

This makes Route 53 your DNS record manager while GoDaddy continues to manage the domain registration.

1) Create a Route 53 hosted zone for your domain

- Route 53 → Hosted zones → Create hosted zone → Enter your domain

- Recreate/import DNS records in Route 53 (use your GoDaddy zone export for reference)

- Optional: reduce TTLs on key records to speed up propagation

2) Update nameservers at GoDaddy to Route 53

- Copy the NS records from the Route 53 hosted zone (ns-XXXX.awsdns-XX, etc.)

- In GoDaddy, open your domain → Manage DNS → Nameservers → Change → Enter Route 53 nameservers

- Propagation can take up to 48 hours; validate with `dig`/`nslookup`

3) Validate DNS

- Use `dig`/`nslookup` to confirm records resolve from Route 53

- Check that authoritative nameservers are the Route 53 NS values

- Verify nameserver delegation and registrar data:

```bash
host -t NS example.com
whois example.com
``` 