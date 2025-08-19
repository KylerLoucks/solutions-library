# Setup Walkthrough: Landing Zone Accelerator

A high-level, end-to-end guide that walks you through setting up LZA, linking to detailed pages for each step.

Estimated time: 60–90 minutes

## Step 1: Set up the configuration repository

- Create your `aws-accelerator-config` repository
- (Optional) Seed it with the LZA sample configuration files
- Create a CodeStar Connection to GitHub and note the ARN

See: [Setup Config Repository](setup-config-repo.md)

## Step 2: Add configuration files

- Add/edit the YAML configuration files that drive the deployment
- Start with minimal examples, then iterate

See: [Configuration](configuration.md)

Example (minimal `accounts-config.yaml`):
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

## Step 3: Deploy the LZA installer (reference your config repo)

- Launch the installer stack with parameters pointing to your repo and CodeStar Connection
- Pipelines will provision baseline resources across the organization

See: [Deployment](deployment.md)

Required parameters (example):
- `UseExistingConfigRepo`: `Yes`
- `ExistingConfigRepositoryName`: `aws-accelerator-config`
- `ExistingConfigRepositoryOwner`: your GitHub org/username
- `ExistingConfigRepositoryBranchName`: `main`
- `ConfigurationRepositoryLocation`: `codeconnection`

## Step 4: Validate the environment

- Confirm org structure, controls, and logging
- Validate networking and security guardrails

See: [Validation](validation.md)

---

## Common issues and pointers

- CodeStar connection: ensure you selected "Install a new app" and noted the ARN
- Missing permissions: validate your role can create/update required org resources
- Pipeline errors: check CodePipeline/CodeBuild logs and fix YAML/params incrementally
