# Configuration: Landing Zone Accelerator

This section covers how to set up and edit the configuration files that drive LZA. It assumes you have completed the prerequisites and created the `aws-accelerator-config` repository.

- See Prerequisites: [Prerequisites](prerequisites.md)
- See Setup Config Repo: [Setup Config Repository](setup-config-repo.md)

## Files overview

- `accounts-config.yaml` – manage accounts and OUs
- `global-config.yaml` – global properties inherited across the org
- `organization-config.yaml` – OU structure
- `iam-config.yaml` – org-wide IAM resources
- `network-config.yaml` – VPCs, TGW, endpoints, firewalls
- `security-config.yaml` – GuardDuty, Macie, CloudTrail, etc.
- `customizations-config.yaml` (optional) – custom apps, 3rd-party appliances, CloudFormation stacks
- `replacements-config.yaml` (optional) – cross-file replacement values

Reference: [Using configuration files](https://docs.aws.amazon.com/solutions/latest/landing-zone-accelerator-on-aws/using-configuration-files.html)

---

## Minimal examples

### accounts-config.yaml
```yaml
mandatoryAccounts:
  - name: Management
    email: management@example.com
    organizationalUnit: Root
workloadAccounts:
  - name: SharedServices
    email: shared@example.com
    organizationalUnit: Infrastructure
```

### organization-config.yaml
```yaml
organizationalUnits:
  - name: Infrastructure
    description: Infra and shared services
  - name: Testing
    description: Test and QA workloads
```

### global-config.yaml
```yaml
homeRegion: us-east-1
enabledRegions:
  - us-east-1
  - us-west-2
```

### iam-config.yaml
```yaml
roles:
  - name: OrganizationAccountAccessRole
    description: Org admin access
    policies:
      - arn:aws:iam::aws:policy/AdministratorAccess
```

### network-config.yaml
```yaml
vpcs:
  - name: shared-vpc
    cidr: 10.10.0.0/16
    region: us-east-1
    subnets:
      - name: app-a
        az: a
        cidr: 10.10.0.0/20
endpoints:
  - service: ssm
  - service: ec2messages
```

### security-config.yaml
```yaml
cloudtrail:
  enable: true
config:
  enable: true
guardduty:
  enable: true
```

### customizations-config.yaml (optional)
```yaml
stacks:
  - name: tags-baseline
    template: s3://my-bucket/templates/tags-baseline.yaml
    regions:
      - us-east-1
```

---

## Patterns

- Start with minimal examples, deploy, then iterate
- Keep emails/owners in one spot (use `replacements-config.yaml` if preferred)
- Commit small changes and let the pipeline surface issues early

## Validation checklist

- Accounts created or registered as expected (AWS Organizations)
- CloudTrail/Config enabled and central logging receiving data
- SCPs and OUs reflect intended guardrails
- Networking primitives (VPCs, endpoints) created as defined
