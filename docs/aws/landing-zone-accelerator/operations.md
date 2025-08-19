# Operations

## Day-2 tasks

- Account lifecycle management (create, move, close)
- Guardrail updates and compliance reviews
- Patch and backup policies

## Monitoring

- Central dashboards and alerts for control plane events
- Log retention and anomaly detection

---

## Adding a new account

In your config GitHub repo, edit the `accounts-config.yaml` file.

In the `workloadAccounts` configuration block, append additional account definition(s). For example, to add `SharedServices` and `Network` accounts to the `Infrastructure` OU and the `Testing-Workload` account to the `Testing` OU:

```yaml
workloadAccounts:
  - name: SharedServices
    description: The SharedServices account
    email: <shared-services>@example.com  # UPDATE EMAIL ADDRESS
    organizationalUnit: Infrastructure
  - name: Network
    description: The Network account
    email: <network>@example.com  # UPDATE EMAIL ADDRESS
    organizationalUnit: Infrastructure
  - name: Testing-Workload
    description: The Workload account
    email: <workload>@example.com  # UPDATE EMAIL ADDRESS
    organizationalUnit: Testing
```

## Adding existing accounts to LZA

Reference: [Performing administrator tasks → Adding an existing account](https://docs.aws.amazon.com/solutions/latest/landing-zone-accelerator-on-aws/performing-administrator-tasks.html#adding-an-existing-account)

If your existing account has already been registered with AWS Organizations and AWS Control Tower, you can follow the steps listed in [Adding a new account](https://docs.aws.amazon.com/solutions/latest/landing-zone-accelerator-on-aws/performing-administrator-tasks.html#adding-a-new-account) to add the account to the solution configuration.

If the account has not yet been invited to your organization, follow the steps in [Inviting an account to join your organization](https://docs.aws.amazon.com/organizations/latest/userguide/orgs_manage_accounts_invites.html) in the AWS Organizations User Guide.

For AWS Control Tower-based installations, refer to [Prerequisites for enrollment](https://docs.aws.amazon.com/controltower/latest/userguide/enrollment-prerequisites.html) in the AWS Control Tower User Guide — the Landing Zone Accelerator on AWS solution can enroll the account in AWS Control Tower for you after you have completed these prerequisites.
