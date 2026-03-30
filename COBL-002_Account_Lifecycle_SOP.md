# Standard Operating Procedure — COBL-002
# AWS Account Lifecycle Management
# Mist Avinya Technologies LLP

| Field | Detail |
|---|---|
| Document ID | MIST-GOV-SOP-003 |
| Version | 1.0 |
| Effective Date | August 1, 2025 |
| Last Reviewed | November 25, 2025 |
| Owner | Cloud Governance & FinOps Practice Team |
| Classification | Internal — Confidential |
| Applicable To | All Mist Avinya customer engagements requiring AWS multi-account management |
| Methodology Reference | Multi-Account Architecture and Governance Playbook |

---

## 1. Purpose

This SOP defines the end-to-end AWS account lifecycle management process used by Mist Avinya Technologies LLP across all customer engagements. It establishes mandatory procedures for requesting, provisioning, governing, and decommissioning AWS accounts within an AWS Organizations structure managed through AWS Control Tower.

This document directly addresses the AWS Cloud Operations Competency baseline requirement COBL-002, which requires the partner to demonstrate a documented account lifecycle process covering:

- Request and Approval
- Automated Provisioning
- Operational Governance (including SCPs, logging, and security baselines)
- Decommissioning and Retention
- Responsible Roles at each phase

This SOP is anchored in the Multi-Account Architecture and Governance Playbook and is aligned with the AWS Well-Architected Framework (Security Pillar, Operational Excellence Pillar) and AWS Control Tower best practices.

---

## 2. Scope

This SOP applies to:

- All AWS accounts provisioned, operated, or decommissioned as part of a Mist Avinya customer engagement
- All Organizational Units (OUs) within the customer's AWS Organizations hierarchy
- All personnel involved in account lifecycle activities — Mist Avinya and customer stakeholders
- All AWS regions in which the customer operates

This SOP does NOT cover:

- Application-level deployments within accounts (covered by COBL-003 GitOps and IaC SOP)
- Identity and access management for partner personnel accessing customer accounts (covered by ACCT-002 Identity Security SOP, Document ID MIST-SEC-SOP-002)
- Tagging strategy and enforcement (covered by COBL-004 Tagging Strategy SOP)

---

## 3. Responsible Roles — RACI Matrix

The following roles are defined for all account lifecycle activities. Each phase in Section 4 references these roles.

| Role | Organization | Responsibilities |
|---|---|---|
| Cloud Governance Lead | Mist Avinya | Owns the account lifecycle process. Reviews and approves account requests. Designs OU placement and SCP assignments. Oversees decommissioning. |
| Project Manager | Mist Avinya | Coordinates account lifecycle activities with the customer. Ensures timeline adherence. Escalation point for approval delays. |
| DevOps / Platform Engineer | Mist Avinya | Executes automated provisioning via Control Tower Account Factory. Maintains Account Blueprint CloudFormation templates. Manages CI/CD pipeline for IaC deployments. |
| Security Engineer | Mist Avinya | Validates security baselines on new accounts. Configures CloudTrail, GuardDuty, Security Hub, and Config. Reviews SCPs before deployment. |
| FinOps Analyst | Mist Avinya | Validates cost allocation tags, budget configurations, and CUR integration for new accounts. Confirms cost visibility in dashboards. |
| Business Unit Owner / Requestor | Customer | Submits account requests with business justification. Designates the account owner and cost center. |
| Head of Engineering / Technical Lead | Customer | Approves account requests. Authorizes OU placement. Signs off on decommissioning decisions. |
| Cloud / DevOps Team Lead | Customer | Operational owner of the account post-provisioning. Responsible for day-to-day workload management within the account. |
| Lead Architect | Customer | Validates account architecture alignment. Verifies credential handover during provisioning and decommissioning. |

### RACI per Lifecycle Phase

| Activity | Cloud Governance Lead | Project Manager | DevOps Engineer | Security Engineer | FinOps Analyst | Customer Requestor | Customer Technical Lead | Customer Cloud Team |
|---|---|---|---|---|---|---|---|---|
| Stage 1: Request & Approval | A | C | I | C | C | R | A | I |
| Stage 2: Automated Provisioning | A | I | R | C | C | I | I | I |
| Stage 3: Operational Governance | A | I | R | R | R | I | C | R |
| Stage 4: Decommissioning | A | C | R | R | C | I | A | R |

R = Responsible (does the work) | A = Accountable (owns the outcome) | C = Consulted (provides input) | I = Informed (kept updated)

---

## 4. Account Lifecycle — Four Stages

### Overview

```
┌─────────────────┐     ┌──────────────────────┐     ┌─────────────────────┐     ┌──────────────────────┐
│  STAGE 1        │     │  STAGE 2             │     │  STAGE 3            │     │  STAGE 4             │
│  Request &      │────>│  Automated           │────>│  Operational        │────>│  Decommissioning     │
│  Approval       │     │  Provisioning &      │     │  Governance         │     │  & Retention         │
│                 │     │  Customization       │     │                     │     │                      │
│  Requestor      │     │  DevOps Engineer     │     │  Security + FinOps  │     │  Cloud Governance    │
│  submits →      │     │  executes →          │     │  + Customer Cloud   │     │  Lead owns →         │
│  Governance     │     │  Control Tower       │     │  Team operate →     │     │  Suspended OU →      │
│  Lead approves  │     │  Account Factory     │     │  SCPs + Logging +   │     │  Data Archive →      │
│                 │     │  + Account Blueprint  │     │  Config + GuardDuty │     │  90-day hold →       │
│                 │     │                      │     │                     │     │  Formal closure      │
└─────────────────┘     └──────────────────────┘     └─────────────────────┘     └──────────────────────┘
```

---

### 4.1 Stage 1 — Request and Approval

#### 4.1.1 Purpose

Ensure every new AWS account has a documented business justification, a designated owner, a cost center for financial attribution, and an approved OU placement before any provisioning activity begins.

#### 4.1.2 Trigger

A business unit within the customer organization identifies the need for a new AWS account — for example, a new blockchain protocol (e.g., adding Avalanche alongside existing Ethereum, Solana, Polygon), a new environment (e.g., performance testing), or a new shared service.

#### 4.1.3 Procedure

| Step | Action | Responsible | Tool / Channel |
|---|---|---|---|
| 1.1 | Business Unit Owner submits a New Account Request through the customer's internal portal or ITSM tool (e.g., ServiceNow, Jira Service Management). | Customer Requestor | ITSM Portal |
| 1.2 | The request MUST include the following mandatory fields: | Customer Requestor | ITSM Portal |
| | — Business Justification: Why is this account needed? What workload will it host? | | |
| | — Designated Account Owner: Name, email, and team of the person responsible for the account | | |
| | — Cost Center: The financial cost center to which all charges in this account will be attributed | | |
| | — Desired OU Placement: Which Organizational Unit should this account reside in (e.g., Production OU, Non-Production OU, Sandbox OU) | | |
| | — Requested AWS Regions: Which regions will be enabled for this account | | |
| | — Estimated Monthly Spend: Projected monthly AWS cost for budget configuration | | |
| | — Mandatory Tags: Pre-defined values for the five mandatory cost allocation tags (Chain, Tenant, Node-Type, Environment, Owner) | | |
| 1.3 | The Cloud Governance Lead (Mist Avinya) reviews the request for completeness and alignment with the customer's OU architecture and governance policies. | Cloud Governance Lead | ITSM Portal |
| 1.4 | The Security Engineer (Mist Avinya) reviews the request for security implications — validates that the requested OU placement is appropriate for the workload's security classification and that no existing account can serve the purpose. | Security Engineer | ITSM Portal |
| 1.5 | The FinOps Analyst (Mist Avinya) validates that the cost center is valid, the mandatory tags are correctly defined, and the estimated spend is reasonable for budget configuration. | FinOps Analyst | ITSM Portal |
| 1.6 | The Customer Technical Lead (Head of Engineering) provides written approval for the account request, confirming OU placement and budget allocation. | Customer Technical Lead | ITSM Portal / Email |
| 1.7 | The Cloud Governance Lead provides final approval and transitions the request to Stage 2. | Cloud Governance Lead | ITSM Portal |

#### 4.1.4 Approval Criteria

The request is approved ONLY when ALL of the following are satisfied:

- Business justification is clear and the workload cannot be hosted in an existing account
- Designated owner is identified and has acknowledged responsibility
- Cost center is valid and mapped in the customer's financial system
- OU placement aligns with the documented OU architecture (see Section 5)
- Mandatory tag values are defined and conform to the Tag Dictionary (per COBL-004)
- Customer Technical Lead has provided written authorization
- Security review confirms no policy conflicts

#### 4.1.5 Rejection Handling

If the request is rejected:

- The Cloud Governance Lead documents the rejection reason in the ITSM ticket
- The requestor is notified with specific guidance on what needs to change
- The requestor may resubmit after addressing the feedback
- All rejection decisions are logged for audit trail purposes

#### 4.1.6 SLA

| Activity | Target SLA |
|---|---|
| Initial review by Cloud Governance Lead | 1 business day from submission |
| Security and FinOps review | 1 business day from Governance review |
| Customer Technical Lead approval | 2 business days from review completion |
| Total: Request to Approval | 3-5 business days |

---

### 4.2 Stage 2 — Automated Provisioning and Customization

#### 4.2.1 Purpose

Provision the approved AWS account using fully automated tooling with zero manual intervention, ensuring the account inherits all governance controls, security baselines, and networking infrastructure from the moment of creation.

#### 4.2.2 Trigger

Stage 1 approval is complete. The Cloud Governance Lead transitions the ITSM ticket to "Approved — Ready for Provisioning."

#### 4.2.3 Procedure

| Step | Action | Responsible | Tool |
|---|---|---|---|
| 2.1 | The DevOps Engineer triggers the AWS Control Tower Account Factory to provision a new AWS account. The Account Factory is configured with the approved parameters: account name, account email (shared alias), and target OU. | DevOps Engineer | AWS Control Tower Account Factory |
| 2.2 | Account Factory automatically provisions the account and places it in the approved OU. The account immediately inherits ALL preventative guardrails (Service Control Policies) associated with that OU. | Automated (AWS Control Tower) | AWS Control Tower |
| 2.3 | A Control Tower Lifecycle Event is emitted upon successful account creation. This event automatically triggers a pre-configured AWS Step Functions state machine. | Automated (AWS EventBridge) | Amazon EventBridge → AWS Step Functions |
| 2.4 | The Step Functions state machine deploys the standardized Account Blueprint — an AWS CloudFormation template that provisions the following baseline resources into the new account: | Automated (AWS Step Functions) | AWS CloudFormation |
| | — IAM Roles: Baseline roles for Mist Avinya access (per ACCT-002 SOP), workload execution roles, and a break-glass emergency role | | |
| | — VPC Architecture: Baseline VPC with public subnets, private subnets, and isolated subnets across all AZs in the approved region(s). NAT Gateways for private subnet egress. VPC Flow Logs enabled and delivered to the centralized Log Archive account. | | |
| | — Security Groups: Default deny-all security group. Baseline security groups for common patterns (web, application, database tiers). | | |
| | — Network ACLs: Restrictive NACLs at the subnet level aligned with the customer's network security policy. | | |
| | — AWS Config: Enabled with managed rules including `required-tags`, `ec2-instance-no-public-ip`, `s3-bucket-server-side-encryption-enabled`, `iam-root-access-key-check`. Config delivery channel configured to the centralized S3 bucket in the Log Archive account. | | |
| | — Amazon GuardDuty: Enabled and configured as a member account under the centralized GuardDuty administrator in the Security OU. | | |
| | — AWS CloudTrail: Organization trail already captures all API activity. Account-level trail configured for additional granularity if required. | | |
| | — AWS Security Hub: Enabled and configured as a member account under the centralized Security Hub administrator. AWS Foundational Security Best Practices standard enabled. | | |
| | — AWS Budgets: Account-level budget configured based on the estimated monthly spend from the request. Alert thresholds at 50%, 80%, and 100% of budget. Notifications via email and webhook (per COCFM-006). | | |
| | — Mandatory Tags: Default tags applied to all baseline resources using the values from the approved request. | | |
| 2.5 | The Step Functions state machine validates the deployment by running a post-provisioning checklist (see Section 4.2.5). If any check fails, the state machine triggers an SNS notification to the Cloud Governance Lead and DevOps Engineer. | Automated (AWS Step Functions) | AWS Step Functions → Amazon SNS |
| 2.6 | The DevOps Engineer confirms successful provisioning and updates the ITSM ticket with the new account ID, account alias, and provisioning timestamp. | DevOps Engineer | ITSM Portal |
| 2.7 | The Cloud Governance Lead notifies the Customer Technical Lead and the designated Account Owner that the account is ready for use. | Cloud Governance Lead | Email / ITSM Portal |

#### 4.2.4 Account Blueprint — CloudFormation Template Contents

The Account Blueprint is a versioned AWS CloudFormation template stored in a dedicated CodeCommit / GitHub repository. It is the single source of truth for the baseline configuration of every new account.

| Blueprint Component | AWS Service | Configuration |
|---|---|---|
| IAM Baseline Roles | AWS IAM | `MistAvinya-FinOps-ReadOnly`, `MistAvinya-Infra-Deployer`, `MistAvinya-Security-Auditor`, `BreakGlass-Emergency` (per ACCT-002 SOP) |
| VPC | Amazon VPC | 3-tier architecture (public, private, isolated) across all AZs. CIDR allocated from the customer's IP address plan managed in the Networking-Hub account. |
| VPC Flow Logs | Amazon VPC | Enabled. Delivered to centralized S3 bucket in Log Archive account. Retention: 90 days in CloudWatch Logs, indefinite in S3. |
| Security Groups | Amazon EC2 | Default deny-all. Baseline groups for web (443 inbound), app (internal only), database (private subnet only). |
| NACLs | Amazon VPC | Restrictive rules. Deny all inbound/outbound by default. Allow rules added per workload requirements. |
| AWS Config | AWS Config | Enabled. Managed rules deployed. Delivery to centralized S3 bucket. |
| GuardDuty | Amazon GuardDuty | Member account enrollment under Security OU administrator. |
| Security Hub | AWS Security Hub | Member account enrollment. AWS FSBP standard enabled. |
| CloudTrail | AWS CloudTrail | Organization trail active. Account-level trail optional. |
| Budgets | AWS Budgets | Account-level budget with 50%, 80%, 100% alert thresholds. Email + webhook notifications. |
| Tags | AWS Resource Groups | Mandatory tags applied to all baseline resources: Chain, Tenant, Node-Type, Environment, Owner. |
| Transit Gateway Attachment | AWS Transit Gateway | Account VPC attached to the centralized Transit Gateway in the Networking-Hub account for inter-account and egress connectivity. |

#### 4.2.5 Post-Provisioning Validation Checklist

The Step Functions state machine executes the following automated checks after deployment:

| Check | Expected Result | Action on Failure |
|---|---|---|
| Account exists in correct OU | Account ID present in target OU | Alert Cloud Governance Lead |
| SCPs inherited | All OU-level SCPs attached | Alert + halt provisioning |
| AWS Config enabled | Config recorder active, delivery channel configured | Re-deploy Config stack |
| GuardDuty enrolled | Member account status: Enabled | Re-enroll via administrator |
| Security Hub enrolled | Member account status: Enabled | Re-enroll via administrator |
| CloudTrail logging | Organization trail capturing events from new account | Alert Security Engineer |
| VPC deployed | VPC ID exists with correct CIDR and subnets | Re-deploy VPC stack |
| IAM roles created | All baseline roles exist with correct trust policies | Re-deploy IAM stack |
| Budgets configured | Budget exists with correct thresholds | Re-deploy Budget stack |
| Mandatory tags applied | All 5 mandatory tags present on baseline resources | Alert FinOps Analyst |
| VPC Flow Logs active | Flow Logs delivering to centralized bucket | Re-deploy Flow Logs |
| Transit Gateway attached | TGW attachment active | Alert DevOps Engineer |

#### 4.2.6 SLA

| Activity | Target SLA |
|---|---|
| Account Factory provisioning | 30 minutes (automated) |
| Account Blueprint deployment (Step Functions) | 15-30 minutes (automated) |
| Post-provisioning validation | 10 minutes (automated) |
| DevOps Engineer confirmation and ITSM update | 2 hours from provisioning |
| Total: Approval to Account Ready | Same business day |

---

### 4.3 Stage 3 — Operational Governance

#### 4.3.1 Purpose

Ensure every active AWS account is perpetually monitored, governed, and maintained in compliance with the customer's security, cost, and operational policies throughout its operational lifetime.

#### 4.3.2 Trigger

Stage 2 provisioning is complete. The account is in active use by the designated workload team.

#### 4.3.3 Governance Controls — Service Control Policies (SCPs)

SCPs are the primary preventative governance mechanism. They are attached at the OU level and inherited by all accounts within the OU. SCPs define the maximum permissions boundary — no IAM policy within the account can exceed what the SCP allows.

| SCP Category | Policy Name | Description | Applied To |
|---|---|---|---|
| Region Restriction | `scp-deny-unapproved-regions` | Denies all API actions outside of approved regions (e.g., ap-south-1, ap-south-2). Exceptions for global services (IAM, CloudFront, Route 53, Organizations). | All OUs except Management |
| Root Account Protection | `scp-deny-root-actions` | Denies all actions by the root user except essential account-level operations (billing, MFA, support plan). | All OUs except Management |
| Tag Enforcement | `scp-deny-untagged-ec2` | Denies `ec2:RunInstances` if the request does not include mandatory cost-center and owner tags. | Production OU, Non-Production OU |
| S3 Public Access | `scp-deny-s3-public` | Denies `s3:PutBucketPolicy` and `s3:PutBucketAcl` actions that would make a bucket publicly accessible. | All OUs |
| CloudTrail Protection | `scp-deny-cloudtrail-stop` | Denies `cloudtrail:StopLogging`, `cloudtrail:DeleteTrail`, and `cloudtrail:UpdateTrail` actions. | All OUs except Management |
| GuardDuty Protection | `scp-deny-guardduty-disable` | Denies `guardduty:DisableOrganizationAdminAccount` and `guardduty:DeleteDetector`. | All OUs |
| Config Protection | `scp-deny-config-disable` | Denies `config:StopConfigurationRecorder` and `config:DeleteConfigurationRecorder`. | All OUs |
| Sandbox Spending Limit | `scp-deny-expensive-services` | Denies launch of expensive instance types (e.g., p4d, p5, trn1) and restricts maximum instance count. | Sandbox OU only |
| Leave Organization | `scp-deny-leave-org` | Denies `organizations:LeaveOrganization` to prevent accounts from detaching. | All OUs except Management |

#### 4.3.4 Governance Controls — Logging and Monitoring

All active accounts are continuously monitored by centralized security and operational services:

| Service | Configuration | Centralized In | Responsible Role |
|---|---|---|---|
| AWS CloudTrail | Organization trail enabled in ALL regions including future regions. Logs delivered to centralized S3 bucket in Log Archive account. S3 bucket protected with: versioning enabled, S3 Object Lock in Governance mode (immutability), dedicated IAM role for write-only access, explicit deny policy on delete actions. | Security-Logging Account (Security OU) | Security Engineer |
| Amazon GuardDuty | Enabled in all accounts as member accounts under the centralized GuardDuty administrator. Threat findings aggregated in the Security OU. Auto-remediation via EventBridge + Lambda for critical findings. | Security-Logging Account (Security OU) | Security Engineer |
| AWS Security Hub | Enabled in all accounts. AWS Foundational Security Best Practices standard active. Findings aggregated in the Security OU administrator account. | Security-Logging Account (Security OU) | Security Engineer |
| AWS Config | Configuration recorder enabled in all accounts and all active regions. Managed rules deployed via CloudFormation StackSets (see Section 4.3.5). Conformance packs for CIS Benchmarks. Delivery channel to centralized S3 bucket. | Security-Logging Account (Security OU) | Security Engineer |
| VPC Flow Logs | Enabled on all VPCs. Delivered to CloudWatch Logs (90-day retention) and centralized S3 bucket (indefinite retention). | Security-Logging Account (Security OU) | DevOps Engineer |
| AWS Budgets | Account-level and service-level budgets with alert thresholds at 50%, 80%, 100%. Tag-based budgets per blockchain and per tenant. Notifications via email + webhook. | FinOps-Central Account (Shared Services OU) | FinOps Analyst |
| AWS Cost Anomaly Detection | Configured per account and per service. Alerts within 24 hours of anomaly detection. Integrated with Aquila Clouds for AI-driven analysis. | FinOps-Central Account (Shared Services OU) | FinOps Analyst |

#### 4.3.5 Governance Controls — Security Baselines

Security baselines are enforced through AWS Config managed rules deployed via CloudFormation StackSets from the Management Account. StackSets ensure consistent rule deployment across all accounts and regions.

| AWS Config Rule | Purpose | Remediation |
|---|---|---|
| `required-tags` | Checks that EC2 instances and S3 buckets have the 5 mandatory tags | EventBridge → Lambda applies default tag or notifies owner |
| `ec2-instance-no-public-ip` | Ensures EC2 instances in private subnets do not have public IPs | Alert to Security Engineer for manual review |
| `s3-bucket-server-side-encryption-enabled` | Ensures all S3 buckets have encryption enabled | Auto-remediation: Lambda enables AES-256 encryption |
| `s3-bucket-public-read-prohibited` | Ensures no S3 bucket allows public read access | Auto-remediation: Lambda removes public access |
| `iam-root-access-key-check` | Ensures no access keys exist for the root user | Alert to Cloud Governance Lead |
| `iam-user-mfa-enabled` | Ensures all IAM users have MFA enabled | Alert to Security Engineer |
| `iam-policy-no-statements-with-admin-access` | Ensures no IAM policy grants full `*:*` admin access | Alert to Security Engineer |
| `cloudtrail-enabled` | Ensures CloudTrail is enabled in all regions | Auto-remediation: Lambda re-enables CloudTrail |
| `guardduty-enabled-centralized` | Ensures GuardDuty is enabled and reporting to administrator | Auto-remediation: Lambda re-enrolls member account |
| `encrypted-volumes` | Ensures all EBS volumes are encrypted | Alert to DevOps Engineer |
| `rds-storage-encrypted` | Ensures all RDS instances have storage encryption enabled | Alert to DevOps Engineer |

#### 4.3.6 Governance Controls — Baseline Updates and Drift Management

| Activity | Method | Frequency | Responsible |
|---|---|---|---|
| Account Blueprint updates | Update the master Account Blueprint CloudFormation template in the source repository. Deploy updates to all accounts using CloudFormation StackSets. | As needed (triggered by policy changes, new security requirements, or audit findings) | DevOps Engineer |
| SCP updates | Update SCP JSON in the IaC repository. Deploy via Terraform through the CI/CD pipeline. Peer-reviewed pull request required. | As needed | Cloud Governance Lead + DevOps Engineer |
| Config Rule updates | Update Config rule definitions in the StackSet template. Deploy via CloudFormation StackSets. | As needed | Security Engineer |
| AWS Control Tower drift detection | Control Tower automatically detects drift (e.g., SCP modifications, OU changes made outside Control Tower). Drift is reported in the Control Tower dashboard. | Continuous (automated) | Cloud Governance Lead |
| Drift remediation | If drift is detected, the Cloud Governance Lead investigates the root cause, reverts unauthorized changes, and documents the incident. | Within 24 hours of detection | Cloud Governance Lead |
| Quarterly access review | Review IAM roles, permissions, and access patterns using IAM Access Analyzer. Tighten permissions based on actual usage from CloudTrail. | Quarterly | Security Engineer |
| Quarterly cost review | Review account-level spend, rightsizing recommendations (AWS Compute Optimizer), and commitment utilization. | Quarterly | FinOps Analyst |

#### 4.3.7 SLA — Operational Governance

| Activity | Target SLA |
|---|---|
| Security finding response (Critical) | 4 hours |
| Security finding response (High) | 24 hours |
| Security finding response (Medium/Low) | 5 business days |
| Budget alert response | 24 hours |
| Cost anomaly investigation | 48 hours |
| Control Tower drift remediation | 24 hours |
| Quarterly access review completion | Within 5 business days of quarter end |

---

### 4.4 Stage 4 — Decommissioning and Retention

#### 4.4.1 Purpose

Safely decommission an AWS account that is no longer needed, ensuring all critical data is preserved according to the customer's retention policies, all resources are terminated to stop billing, and the account is formally closed after a mandatory cooling-off period.

#### 4.4.2 Trigger

The Customer Technical Lead or Business Unit Owner submits a Decommissioning Request through the ITSM portal, or the Cloud Governance Lead identifies an account that has been inactive for an extended period (90+ days with zero or near-zero spend).

#### 4.4.3 Procedure

| Step | Action | Responsible | Tool |
|---|---|---|---|
| 4.1 | Decommissioning Request submitted through ITSM portal. The request MUST include: account ID, account name, reason for decommissioning, confirmation that all workloads have been migrated or are no longer needed, and data retention requirements. | Customer Requestor or Cloud Governance Lead | ITSM Portal |
| 4.2 | The Cloud Governance Lead reviews the request and confirms with the designated Account Owner that the account is eligible for decommissioning. | Cloud Governance Lead | ITSM Portal |
| 4.3 | The Customer Technical Lead provides written approval for decommissioning. | Customer Technical Lead | ITSM Portal / Email |
| 4.4 | The Security Engineer performs a final security audit of the account: reviews CloudTrail logs for recent activity, checks for any active IAM users or roles with external trust policies, and confirms no sensitive data remains unarchived. | Security Engineer | AWS CloudTrail, IAM Access Analyzer |
| 4.5 | The DevOps Engineer executes the Data Archival Process (see Section 4.4.4). | DevOps Engineer | AWS CLI / Automated Scripts |
| 4.6 | The DevOps Engineer moves the account to the Suspended OU. This OU has highly restrictive SCPs that deny ALL actions except read-only access to S3 (for data verification) and Organizations API calls. This effectively freezes the account — no new resources can be created, no existing resources can be modified. | DevOps Engineer | AWS Organizations Console / CLI |
| 4.7 | The FinOps Analyst confirms that the account's budget alerts are updated to reflect zero expected spend. Any residual charges (e.g., S3 storage for data pending archival) are monitored. | FinOps Analyst | AWS Budgets |
| 4.8 | The 90-day mandatory waiting period begins. During this period: the account remains in the Suspended OU with restrictive SCPs, the Cloud Governance Lead monitors for any requests to reverse the decommissioning, the account is flagged in the ITSM system with the decommissioning date and the earliest closure date. | Cloud Governance Lead | ITSM Portal |
| 4.9 | After the 90-day waiting period, the Cloud Governance Lead confirms with the Customer Technical Lead that the account should proceed to formal closure. | Cloud Governance Lead | ITSM Portal / Email |
| 4.10 | The DevOps Engineer executes the Account Closure Process using automated scripts that leverage the AWS Organizations `CloseAccount` API. The script: terminates all remaining resources in the account, removes the account from the Suspended OU, calls `organizations:CloseAccount` to initiate the AWS account closure process. | DevOps Engineer | AWS SDK / CLI (automated script) |
| 4.11 | The Cloud Governance Lead updates the ITSM ticket with the closure confirmation, final archival location references, and the date of closure. The ticket is closed. | Cloud Governance Lead | ITSM Portal |

#### 4.4.4 Data Archival Process

Before moving the account to the Suspended OU, the following data archival steps are executed:

| Data Type | Source | Archive Destination | Retention Period | Method |
|---|---|---|---|---|
| CloudTrail Logs | Account-level CloudTrail S3 bucket | Centralized S3 bucket in Log Archive account | Per customer retention policy (minimum 1 year, recommended 7 years) | S3 Cross-Account Replication or `aws s3 sync` |
| AWS Config Snapshots | Config delivery channel S3 bucket | Centralized S3 bucket in Log Archive account | Per customer retention policy | S3 Cross-Account Replication or `aws s3 sync` |
| Application Data | Account-specific S3 buckets, EBS snapshots, RDS snapshots | Centralized S3 bucket in Log Archive account (S3 data) or shared account (snapshots) | Per customer data retention policy | `aws s3 sync`, `aws ec2 copy-snapshot`, `aws rds copy-db-snapshot` with cross-account sharing |
| VPC Flow Logs | CloudWatch Logs / S3 | Centralized S3 bucket in Log Archive account | 90 days minimum | Already delivered to centralized bucket during operational phase |
| Cost and Usage Data | AWS CUR S3 bucket | FinOps-Central account | Indefinite | Already centralized during operational phase |

Archive Destination Security:
- The centralized S3 bucket in the Log Archive account is protected with:
  - Versioning enabled
  - S3 Object Lock in Governance mode for immutability
  - Dedicated IAM role for write-only access
  - Explicit deny policy on delete actions
  - Server-side encryption using AWS KMS (customer-managed key)

#### 4.4.5 Suspended OU — SCP Configuration

The Suspended OU applies the following SCP to all member accounts:

```json
{
  "Version": "2012-10-17",
  "Statement": [
    {
      "Sid": "DenyAllExceptReadOnlyAndOrganizations",
      "Effect": "Deny",
      "NotAction": [
        "s3:GetObject",
        "s3:ListBucket",
        "s3:ListAllMyBuckets",
        "organizations:DescribeAccount",
        "organizations:ListAccounts",
        "iam:GetAccountSummary",
        "sts:GetCallerIdentity",
        "support:*"
      ],
      "Resource": "*"
    }
  ]
}
```

This SCP ensures:
- No new resources can be created
- No existing resources can be modified or deleted (except through the automated closure script run from the Management Account)
- Read-only S3 access is retained for data verification during the 90-day period
- AWS Support access is retained for any account-level inquiries

#### 4.4.6 SLA — Decommissioning

| Activity | Target SLA |
|---|---|
| Decommissioning request review | 2 business days |
| Security audit | 3 business days |
| Data archival | 5 business days |
| Move to Suspended OU | Same day as archival completion |
| 90-day waiting period | 90 calendar days (mandatory, non-negotiable) |
| Final closure after waiting period | 5 business days from Customer Technical Lead confirmation |
| Total: Request to Closure | ~100 calendar days |

---

## 5. AWS Organizations — OU Architecture Reference

This section documents the standard OU architecture that the account lifecycle process operates within. The OU structure is established during the COBL-001 Multi-Account Strategy implementation and is the foundation for all account lifecycle decisions.

### 5.1 Standard OU Hierarchy

```
Root (Management Account)
│
├── Security OU
│   ├── Security-Logging          ← Centralized CloudTrail, Config, GuardDuty administrator
│   └── Log-Archive               ← Immutable log storage, data archival destination
│
├── Infrastructure OU (Shared Services)
│   ├── Networking-Hub            ← Transit Gateway, centralized egress, DNS
│   ├── FinOps-Central            ← CUR, Budgets, Aquila Clouds integration, QuickSight
│   └── CI-CD-Tooling            ← GitHub Actions runners, artifact storage
│
├── Production OU
│   ├── Ethereum-Prod             ← Ethereum validator nodes and dApp backends
│   ├── Solana-Prod               ← Solana validator nodes and dApp backends
│   └── Polygon-Prod              ← Polygon validator nodes and dApp backends
│
├── Non-Production OU
│   ├── Dev-Environment           ← Development workloads
│   ├── Test-Environment          ← Integration and performance testing
│   └── Staging-Environment       ← Pre-production validation
│
├── Sandbox OU
│   └── R&D-Sandbox              ← Gated R&D environment with strict spending limits
│
└── Suspended OU
    └── (Accounts pending decommissioning — 90-day hold)
```

### 5.2 OU-to-SCP Mapping

| OU | SCPs Applied | Purpose |
|---|---|---|
| Security OU | Region restriction, root protection, leave-org denial | Protect centralized security infrastructure |
| Infrastructure OU | Region restriction, root protection, tag enforcement, leave-org denial | Govern shared services |
| Production OU | Region restriction, root protection, tag enforcement, S3 public access denial, CloudTrail/GuardDuty/Config protection, leave-org denial | Maximum governance for production workloads |
| Non-Production OU | Region restriction, root protection, tag enforcement, leave-org denial | Governance with slightly relaxed controls for development agility |
| Sandbox OU | Region restriction, root protection, expensive service denial, spending limits, leave-org denial | Strict cost controls for experimentation |
| Suspended OU | Deny-all except read-only S3 and Organizations (see Section 4.4.5) | Freeze accounts pending decommissioning |

---

## 6. Customer Example — CloudRev (Rev) Implementation

The following describes how the COBL-002 Account Lifecycle SOP was executed for the CloudRev engagement (August–November 2025).

### 6.1 Accounts Provisioned

| Account Name | OU | Purpose | Provisioned Via |
|---|---|---|---|
| Ethereum-Prod | Production OU | Ethereum validator nodes, dApp backends | Control Tower Account Factory + Account Blueprint |
| Solana-Prod | Production OU | Solana validator nodes, dApp backends | Control Tower Account Factory + Account Blueprint |
| Polygon-Prod | Production OU | Polygon validator nodes, dApp backends | Control Tower Account Factory + Account Blueprint |
| Dev-Environment | Non-Production OU | Development workloads | Control Tower Account Factory + Account Blueprint |
| Test-Environment | Non-Production OU | Integration testing | Control Tower Account Factory + Account Blueprint |
| Staging-Environment | Non-Production OU | Pre-production validation | Control Tower Account Factory + Account Blueprint |
| Networking-Hub | Shared Services OU | Transit Gateway, centralized egress | Control Tower Account Factory + Account Blueprint |
| Security-Logging | Security OU | CloudTrail, Config, GuardDuty administrator | Control Tower Account Factory + Account Blueprint |
| FinOps-Central | Shared Services OU | CUR, Budgets, Aquila Clouds, QuickSight | Control Tower Account Factory + Account Blueprint |

### 6.2 Account Blueprint — CloudRev-Specific Configuration

The Account Blueprint for CloudRev included the following customizations beyond the standard baseline:

| Component | CloudRev-Specific Configuration |
|---|---|
| VPC CIDR | Allocated from CloudRev's IP plan: 10.0.0.0/16 (Production), 10.1.0.0/16 (Non-Production), 10.2.0.0/16 (Shared Services) |
| IAM Roles | `MistAvinya-FinOps-ReadOnly`, `MistAvinya-FinOps-Operator`, `MistAvinya-Infra-Deployer`, `MistAvinya-Security-Auditor`, `WebServerEC2S3Role` (per ACCT-002 SOP) |
| Mandatory Tags | Chain (Ethereum/Solana/Polygon), Tenant, Node-Type (validator/RPC/archive), Environment (prod/dev/staging), Owner |
| Budgets | Account-level: based on estimated spend per blockchain. Service-level: EC2, EKS, S3. Tag-based: per Chain, per Tenant. |
| GuardDuty | Member enrollment under Security-Logging account. Threat findings for blockchain-specific attack patterns (e.g., cryptojacking). |
| Config Rules | Standard rules + `required-tags` with CloudRev's 5 mandatory tags. |
| Transit Gateway | All account VPCs attached to centralized Transit Gateway in Networking-Hub for inter-account communication (validator node coordination across blockchains). |

### 6.3 Operational Governance — CloudRev Evidence

| Governance Control | CloudRev Implementation | Evidence |
|---|---|---|
| SCPs | Region restriction to ap-south-1 and ap-south-2. Tag enforcement denying ec2:RunInstances without mandatory tags. Root protection. CloudTrail/GuardDuty/Config protection. | Cloud Ops Practice Requirements (COBL-002, COBL-004) |
| CloudTrail | Organization trail enabled in all regions. Logs to centralized S3 bucket with Object Lock. | Check List (ACCT-001) |
| GuardDuty | Enabled in all accounts. Centralized in Security-Logging. | Check List (ACCT-001) |
| Security Hub | Enabled in all accounts. AWS FSBP standard active. | Check List (NETSEC-001) |
| AWS Config | Enabled with `required-tags`, `ec2-instance-no-public-ip`, `s3-bucket-server-side-encryption-enabled`. | Cloud Ops Practice Requirements (COBL-004) |
| Budgets | Multi-dimensional: account-level, service-level (EC2, EKS, S3), tag-based (per blockchain, per tenant). | Cloud Ops Practice Requirements (COCFM-006) |
| Anomaly Detection | Aquila Clouds + AWS Cost Anomaly Detection. Mitigation status tracked. | Cloud Ops Practice Requirements (COCFM-006) |
| Tag Compliance | 45% → 98% across 5 mandatory tags. Preventative (SCPs, Tag Policies, CI/CD) + Detective (Config Rules, EventBridge + Lambda). | Cloud Ops Practice Requirements (COBL-004), Project Closure Report |
| Baseline Updates | Account Blueprint updates deployed via CloudFormation StackSets. SCP updates via Terraform + GitHub Actions CI/CD pipeline. | Cloud Ops Practice Requirements (COBL-003) |
| Quarterly Reviews | Documented recommendation: quarterly rightsizing reviews using AWS Compute Optimizer. Quarterly access reviews using IAM Access Analyzer. | Project Closure Report |

### 6.4 Decommissioning — CloudRev Context

During the CloudRev engagement (August–November 2025), no accounts were decommissioned. However, the decommissioning process was documented in the operational runbooks delivered to CloudRev as part of the Phase 4 Operational Readiness handoff. The runbook includes:

- Step-by-step decommissioning procedure aligned with Section 4.4 of this SOP
- Suspended OU SCP template (Section 4.4.5)
- Data archival checklist with retention periods agreed with CloudRev's compliance team
- Automated closure script documentation

The documented recommendation for CloudRev going forward: as CloudRev expands globally and adds new blockchain protocols, use Account Factory to provision new accounts. When a blockchain protocol is sunset, follow the Stage 4 decommissioning process to safely close the associated production account.

---

## 7. Compliance Matrix — COBL-002

| COBL-002 Requirement | How This SOP Addresses It | Section |
|---|---|---|
| Documented account lifecycle SOP | This document (MIST-GOV-SOP-003) | Entire document |
| Request and Approval phase | Stage 1 with mandatory fields, approval criteria, rejection handling, and SLAs | Section 4.1 |
| Automated Provisioning phase | Stage 2 with Control Tower Account Factory, Step Functions state machine, Account Blueprint, and post-provisioning validation | Section 4.2 |
| Operational Governance — SCPs | Comprehensive SCP catalog with OU-level mapping | Section 4.3.3 |
| Operational Governance — Logging | CloudTrail (all regions, immutable storage), GuardDuty, Security Hub, Config, VPC Flow Logs | Section 4.3.4 |
| Operational Governance — Security Baselines | AWS Config managed rules deployed via StackSets with auto-remediation | Section 4.3.5 |
| Operational Governance — Baseline Updates and Drift | CloudFormation StackSets for updates, Control Tower drift detection, quarterly reviews | Section 4.3.6 |
| Decommissioning phase | Stage 4 with data archival, Suspended OU, 90-day waiting period, automated closure | Section 4.4 |
| Retention policies | Data archival process with defined retention periods and immutable storage | Section 4.4.4 |
| Responsible roles defined | RACI matrix with 8 defined roles across Mist Avinya and customer organizations | Section 3 |
| Customer example with implementation evidence | CloudRev implementation with specific accounts, configurations, and governance evidence | Section 6 |

---

## 8. Related Documents

| Document | Document ID | Relationship |
|---|---|---|
| ACCT-002 Identity Security SOP | MIST-SEC-SOP-002 | Defines IAM roles and access methods referenced in the Account Blueprint |
| COBL-001 Multi-Account Strategy | (Practice Requirements) | Defines the OU architecture that this SOP operates within |
| COBL-003 GitOps and IaC SOP | (Practice Requirements) | Defines the CI/CD pipeline used for SCP and Blueprint deployments |
| COBL-004 Tagging Strategy SOP | (Practice Requirements) | Defines the mandatory tags enforced during provisioning and governance |
| COCFM-006 Financial Operations | (Practice Requirements) | Defines budget and anomaly detection configurations referenced in governance |
| Statement of Work — CloudRev | SOW | Defines the engagement scope and methodology phases |
| Project Closure Report — CloudRev | ProjectClosure_CloudRev | Confirms deliverables including OU Architecture and operational handoff |
| Change Request CR-CloudREV-2025-003 | Rev_Change_Request_CR-2025-003 | Documents scope extension within the account lifecycle framework |

---

## 9. Document Control

| Version | Date | Author | Change Description |
|---|---|---|---|
| 1.0 | August 1, 2025 | Cloud Governance & FinOps Practice Team | Initial release for CloudRev engagement |
| 1.0 | November 25, 2025 | Cloud Governance & FinOps Practice Team | Reviewed and confirmed post-engagement. CloudRev-specific implementation details added. |

---

## 10. Approval

| Role | Name | Date |
|---|---|---|
| Cloud Governance Lead | Mist Avinya Cloud Governance Team | August 1, 2025 |
| Project Manager | Shikha Rai | August 1, 2025 |
| Director | Sohrab Pawar | August 1, 2025 |
| Customer Technical Lead (CloudRev) | Harmeet Kaur | August 4, 2025 |
| Customer Founder (CloudRev) | Lalit Bansal | August 4, 2025 |
