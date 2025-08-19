# Architecture

The landing zone is composed of multiple AWS accounts, shared services, security controls, and networking constructs.

![Landing Zone Architecture](_assets/diagrams/arch-organization.svg){ width="100%" }

## Key components

- Management, Log Archive, and Security accounts
- Shared services (directory, tooling) and workload accounts
- Detective controls (CloudTrail, Config), preventive controls (SCPs), and guardrails
- Centralized networking (optional) with egress, ingress, and inspection patterns
