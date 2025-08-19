# Deployment

## 1. Prepare the organization

- Validate AWS Organizations structure and OU design
- Define baseline guardrails and SCP strategy

## 2. Bootstrap

- Clone the LZA configuration repo (or create a new one)
- Configure global and account-specific parameters

## 3. Provision

- Deploy baseline stacks to management and security accounts
- Roll out to OUs and workload accounts

## 4. Post-deploy

- Verify logging, controls, and networking
- Document access patterns and handoff to operations

---

## Config Files Repo

“The configuration of LZA is managed through configuration files. Configuration files are written in YAML and define the AWS account and service configurations that meet specific compliance objectives. Using the configuration files, the solution helps users manage the lifecycle of their landing zone by setting up a baseline security architecture and automating common administrative and operational activities. This reduces the undifferentiated heavy lifting associated with building regulated environments on AWS, allowing organizations to focus on other high value concerns such as operating models, developer agility, and reducing costs.

After deploying LZA and implementing these configuration files, you can:

- Configure additional functionality, guardrails, and security services such as AWS Config Managed Rules and AWS Security Hub
- Manage your foundational networking topology such as Amazon Virtual Private Cloud (Amazon VPC), AWS Transit Gateway, and AWS Network Firewall
- Generate additional workload accounts using the AWS Control Tower Account Factory or AWS Organizations

This guide describes architectural considerations, design, and configuration steps for deploying the LZA sample configuration files.”

Reference: [LZA sample configurations overview](https://awslabs.github.io/landing-zone-accelerator-on-aws/latest/sample-configurations/standard/overview/)

---

## Configuration file descriptions

Reference: [Using configuration files](https://docs.aws.amazon.com/solutions/latest/landing-zone-accelerator-on-aws/using-configuration-files.html)

Landing Zone Accelerator on AWS includes seven configuration files that you can use to customize the solution. Six of the files are mandatory. The `customizations-config.yaml` file is for optional extensions of the core solution. The solution orchestrates the creation of resources and configurations based on the input from the configuration files. Resources are generated using AWS CDK constructs defined in the solution’s source code.

- accounts-config.yaml - Used to manage all of the AWS accounts within the AWS Organization. Adding a new account to this configuration file invokes the account creation process from Landing Zone Accelerator on AWS.
- customizations-config.yaml (optional) - Used to manage configuration of custom applications, third-party firewall appliances, and CloudFormation stacks.
- global-config.yaml - Used to manage all of the global properties that can be inherited across the AWS Organization.
- iam-config.yaml - Used to manage all of the IAM resources across the AWS Organization.
- network-config.yaml - Used to manage and implement network resources to establish a WAN/LAN architecture to support cloud operations and application workloads in AWS. (VPC, TGW, VPC Endpoints)
- organization-config.yaml - Used to manage all of the organization units in the AWS Organization.
- replacements-config.yaml (optional) - Used to manage all of the replacement values across the configuration files, see Parameter Store reference variable for more details.
- security-config.yaml - Used to manage configuration of AWS security services. (GuardDuty, Macie, CloudTrail)

---

## Setup Instructions (Custom Config Repo)

1. Create a Github Repository to store the configuration files. Recommend this repo to be called: `aws-accelerator-config`.
2. Clone [landing-zone-accelerator-on-aws](https://github.com/awslabs/landing-zone-accelerator-on-aws/tree/release/v1.12.5) repo
   1. Copy the configs and all the contents from the `lza-sample-config` folder under `reference/sample-configurations` to your local `aws-accelerator-config` repo.
3. Create a Codestar connection in the AWS account you have landing zone deployed into
   1. Sign in to the [Amazon Developer Tools console](https://us-east-1.console.aws.amazon.com/codesuite/home?region=us-east-1).
   2. From the left-hand sidebar, select the Settings drop down and select Connections.
   3. On the Connections page, select the Create Connection button.
   4. To create a connection, follow the [Create a connection](https://docs.aws.amazon.com/dtconsole/latest/userguide/connections-create.html) user guide in the Developer Tools console.
      - Note: When creating a connection, select Install a new app, otherwise it is possible the source stage in your pipeline may fail while attempting to connect to your configuration repository
   5. After creating the Code Connection successfully, make sure to note the Code Connection ARN.
   6. Once you have the Code Connection ARN, you can fill out the following Parameters in the LZA Installer Stack:
      - UseExistingConfigRepo: Yes
      - ExistingConfigRepositoryName: aws-accelerator-config
      - ExistingConfigRepositoryOwner: awslabs
        - Note: This needs to be your 3rd party "owner" or namespace
      - ExistingConfigRepositoryBranchName: main
        - Note: This needs to match your branch name in the 3rd party repo
      - ConfigurationRepositoryLocation: codeconnection
