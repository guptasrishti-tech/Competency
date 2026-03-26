# CloudRev - AWS Cloud Operations Competency Audit Script
# Spoken Presentation: 30-45 Minutes
# Presenter: Mist Avinya Technologies LLP

---

# PART 1: SETTING THE STAGE (0:00 - 5:00)

"Good morning. Thank you for your time today. I am [Name] from Mist Avinya Technologies LLP, and I will be walking you through our CloudRev engagement for the AWS Cloud Operations Competency validation under the Cloud Financial Management category.

CloudRev, or Rev, is a Web3 infrastructure provider in the Blockchain and Digital Assets space. They operate a globally distributed network of blockchain validator nodes and transaction processing infrastructure for decentralized applications and enterprise clients. Their workloads run 24/7 across ap-south-1 Mumbai and ap-south-2 Hyderabad, and demand is volatile because it is driven by market activity.

The engagement ran from August 2025 to November 2025 - 120 days. It was a FinOps Transformation engagement. The core workloads on AWS were Amazon EC2 for validator nodes, Amazon EKS for dApp backend services, Amazon Managed Blockchain for network management, Amazon DynamoDB for off-chain state data, and Amazon S3 for ledger snapshots and archives.

Our Project Manager was Shikha Rai. The CloudRev stakeholder was Lalit Bansal, Founder of CloudRev. The project was formally signed off on November 25, 2025, and the Project Sign-Off and Acceptance Report was co-signed by Lalit Bansal on July 22, 2025 and our Director Sohrab Pawar on July 24, 2025."

---

# PART 2: THE CHALLENGE (5:00 - 10:00)

"Let me explain what CloudRev was dealing with before we came in. There were three layers of problems.

First, cost management. Their monthly AWS spend was at $105,000 - documented in our Project Closure Report. They had skyrocketing compute costs because validator nodes and transaction processing run around the clock. But the bigger issue was that they had poor cost visibility. They could not attribute costs to specific blockchains, specific dApps, or specific tenants. And critically, they had no mechanism to calculate the true cost per transaction. For a Web3 infrastructure provider, that is the fundamental business metric. Without it, they could not price their node-as-a-service offering competitively, and profitability was at risk.

Second, technical challenges. Their ledger data storage was growing exponentially without any governance. Data transfer costs between nodes and availability zones were high because the network architecture had not been optimized for data locality. And there were significant idle and underutilized resources sitting across the infrastructure consuming budget.

Third, governance. Tag compliance was at only 45% across their cost allocation tags. That meant more than half their resources were either untagged or incorrectly tagged, making cost attribution impossible. Forecast accuracy had a variance of plus or minus 18% because of market volatility and the lack of a proper forecasting model. Savings Plans coverage was at only 25% on core compute, and Reserved Instance coverage was at just 12%. They were paying on-demand rates for the vast majority of their always-on infrastructure."

---

# PART 3: HOW WE STRUCTURED THE ENVIRONMENT (10:00 - 16:00)

"Before we could optimize anything, we needed to restructure the environment. We deployed AWS Control Tower to automate the setup of a landing zone, giving CloudRev a drift-resistant foundation.

The AWS Organizations structure we implemented looks like this:

Root, which is the Management Account. Under that, a Production OU containing Ethereum-Prod, Solana-Prod, and Polygon-Prod - one account per blockchain. A Non-Production OU containing Dev-Environment, Test-Environment, and Staging-Environment. And a Shared Services OU containing Networking-Hub, Security-Logging, and FinOps-Central.

We also established a Security OU for centralized logging via CloudTrail and AWS Config, with security tooling through GuardDuty. And a Sandbox OU as a gated environment for R&D with strict automated spending limits.

The reason we segregated Production and Non-Production into separate OUs is to prevent blast radius issues and to allow distinct cost allocation. Each blockchain gets its own production account, so costs are naturally isolated.

For the account lifecycle, we defined four stages. Stage 1 is Request and Approval - a business unit submits a request through an internal portal with a business justification, designated owner, cost center, and desired OU placement. The Cloud Governance Team reviews it. Stage 2 is Automated Provisioning - upon approval, the AWS Control Tower Account Factory provisions the account and places it in the approved OU, inheriting all preventative guardrails via Service Control Policies. We defined a standardized Account Blueprint using AWS CloudFormation that includes additional IAM roles, baseline VPCs, subnets, and core networking. A Control Tower Lifecycle Event triggers a pre-configured AWS Step Functions state machine that deploys this blueprint automatically - no manual intervention. Stage 3 is Operation and Governance - the account is continuously monitored by AWS Security Hub, Amazon GuardDuty, and AWS Config. Updates to the baseline are managed through the master Account Blueprint CloudFormation template and rolled out using CloudFormation StackSets. Stage 4 is Decommissioning - the account moves to a Suspended OU with highly restrictive SCPs, critical data and logs are archived to a central immutable S3 bucket in the Log Archive account, and after a 90-day waiting period, the account is formally closed using automated scripts.

For infrastructure as code, Terraform defines the entire CloudRev solution. We follow a four-phase GitOps workflow. Discovery and Scoping workshops with developers, operations, and security. Design with modular, reusable IaC templates and a GitFlow branching strategy. Implementation with a CI/CD pipeline that includes static analysis using tflint, security scanning using tfsec and checkov and AWS CloudFormation Guard, and validation using provider-native tools. And Deployment where infrastructure changes go through peer-reviewed pull requests that trigger the pipeline automatically. AWS CloudTrail provides a complete audit trail. Rollback is done by reverting the commit in Git and re-running the pipeline. All changes go through GitHub Actions. No manual configuration through the AWS Management Console.

For CloudRev specifically, the Terraform code mandates Graviton instance types - c6g.2xlarge - in the EKS cluster. The max_size of 25 in the scaling configuration allows dynamic scaling of validator nodes. And the code enforces five mandatory FinOps tags on every resource: Owner, Chain, Tenant, Node-Type, and Environment."

---

# PART 4: THE FINOPS TRANSFORMATION - INFORM (16:00 - 21:00)

"Now let me walk through the FinOps transformation itself. We aligned with the FinOps Foundation framework and the AWS Well-Architected Framework Cost Optimization Pillar. Three phases: Inform, Optimize, Operate.

Phase 1 was Inform - establishing visibility and allocation.

We configured the AWS Cost and Usage Report with hourly granularity and resource-level tagging. We integrated the Aquila Clouds FinOps Platform for AI-driven cost analysis and recommendations. We built custom dashboards in Amazon QuickSight to track unit economics - specifically cost per one million processed transactions. And we used Amazon Athena for SQL-based analysis of CUR data for deep-dive investigations.

The cost allocation strategy was designed to move beyond total blended cost and provide unit-level financial accountability. We implemented the mandatory tagging framework with five required tags: Chain for blockchain identifier, Tenant for customer or application, Node-Type for validator or RPC or archive, Environment for prod or dev or staging, and Owner for the responsible team.

Now, how did we actually enforce this? We used both preventative and detective controls.

On the preventative side: AWS Tag Policies within AWS Organizations define the allowed tag keys and their permitted values - for example, the Environment tag must be one of prod, dev, or test. Non-compliant tag operations are denied. We wrote Service Control Policies that deny ec2:RunInstances if the request does not include the mandatory cost-center and owner tags. That is a hard block. We used IAM Policies with condition keys like aws:RequestTag/environment to enforce tagging at the role level. And we integrated tagging checks directly into the CI/CD pipeline using AWS CloudFormation Guard and Checkov - if the Terraform templates are missing required tags, the pipeline fails before any resource is ever deployed.

On the detective side: We deployed AWS Config Rules using the required-tags managed rule to continuously scan and check if EC2 instances and S3 buckets have the required tags. Non-compliant resources are flagged on the AWS Config dashboard. We used AWS Resource Groups and Tag Editor to search for resources missing specific tags across all regions. And for automated remediation, we configured Amazon EventBridge rules that trigger an AWS Lambda function whenever AWS Config detects a non-compliant resource. The Lambda function either applies a default tag - for example, cost-center: unassigned - or sends a notification to the resource owner to correct the issue.

The result: tag compliance went from 45% to 98% across the five mandatory tags.

For the cost allocation dashboard, the primary visualization is the Aquila Clouds FinOps Cost and Unit Economics Dashboard. Monthly spend is filtered by mandatory FinOps Tags derived from the AWS CUR. The dashboard breaks down spend by Cost Center and Node-Type. It has a specific widget displaying the trend line for Unit Economics - cost per one million processed transactions - calculated by normalizing the allocated cost against the business metric. It shows a real-time Tag Compliance Score. And shared costs from central networking and security accounts are automatically allocated back to consuming teams based on predefined rules."

---

# PART 5: THE FINOPS TRANSFORMATION - OPTIMIZE (21:00 - 28:00)

"Phase 2 was Optimize - the actual cost reduction work. We ran focused optimization sprints targeting the most significant cost drivers: compute, storage, and data transfer.

Starting with compute optimization. We migrated 80% of eligible workloads - the validator nodes - to AWS Graviton-based EC2 instances. The result was a 20% price-performance improvement. This is directly aligned with the AWS best practice for cost-effective compute. The Terraform code enforces this by mandating the c6g.2xlarge instance type in the EKS cluster configuration.

We then went further. During Phase 3, Week 8, we submitted Change Request CR-CloudREV-2025-003 on September 15, 2025. The change was to extend the Graviton migration scope to include 8 RDS PostgreSQL instances, migrating them to Graviton-based db.r7g instance types, and to right-size and optimize 3 EKS clusters in the dev and staging environments. The original scope had covered production validator nodes only. The business justification was clear: production Graviton migration had already achieved 20% cost savings, extending to RDS would yield an additional 12 to 15% database cost reduction, and dev/staging environments were consuming 18% of total compute spend. At the time of the request, we were at 28% cost reduction and this change accelerated the path to the 34% target. The estimated effort was 2 weeks, absorbed within the Phase 3 buffer. Cost impact was zero - work covered under the existing SOW. This was approved by Shikha Rai, our Project Manager, and Harmeet Kaur, CloudRev's Technical Lead. Expected additional savings: $8,000 to $10,000 per month.

We enabled AWS Compute Optimizer for right-sizing recommendations across the environment. This identified idle and underutilized resources, which we reduced by 52%.

For commitment management, we implemented a comprehensive Compute Savings Plans strategy for their always-on infrastructure. Coverage increased from 25% to 85% of eligible compute spend. Utilization rate reached 99%, which exceeds the industry best practice threshold of 95%. We used Compute Savings Plans specifically because they are flexible across instance families - important when you are migrating to Graviton. The term was a mix of one-year and three-year commitments based on workload stability analysis.

Moving to storage optimization. We implemented Amazon S3 Intelligent-Tiering for terabytes of ledger archives. This provides automatic cost optimization without performance impact by moving infrequently accessed blockchain snapshots to lower-cost tiers. We configured S3 Lifecycle Policies for automated data archival and deletion of stale data.

For network optimization, we deployed AWS Global Accelerator to optimize network paths for validator node communication. We refined the architecture to reduce cross-AZ data transfer by 22%. We implemented data locality strategies and optimized node placement within availability zones. The method was straightforward - we analyzed the data flow patterns between nodes and repositioned workloads to minimize cross-AZ traffic."

---

# PART 6: THE FINOPS TRANSFORMATION - OPERATE (28:00 - 33:00)

"Phase 3 was Operate - governance and continuous improvement. This is what makes the savings sustainable.

For cost governance, we configured AWS Budgets with multi-dimensional alerts at three levels: account-level budgets, service-level budgets for EC2, EKS, and S3, and tag-based budgets per blockchain and per tenant. This gives CloudRev granular control over spend at every level.

We configured Aquila Clouds Anomaly Detection for real-time spend spike alerts. The dashboard shows Anomaly Detection Status and an explicit Mitigation Status - for example, Mitigated - confirming that detected events are being addressed through automated or guided remediation.

For budget overrun alerts, we configured notifications via Email and a specific webhook URL for integrating with a third-party tool. This satisfies the requirement for alerts via email and at least one additional third-party tool.

We also set up tag enforcement and clean-up through the dashboard, which identifies resources with Invalid Tags - tags that do not conform to the schema - showing a continuous strategy for tag purity.

For consolidated performance reporting, we have a dashboard dedicated to Commitment Optimization for Reserved Instances and Savings Plans, showing metrics like Total Effective Savings and RI Utilization. We also have reporting on historical SP/RI purchases and utilization, including an Unused RI list for optimizing future purchases. And an aggregate scorecard of Cost Saving Opportunities categorized by type - Right-Sizing, Resource Cleanup - with quantified Potential Savings for each.

For forecasting, we developed ML-based forecasting models using historical CUR data. The evidence shows historical cost trend analysis spanning February 2025 through August 2025, with predictive cost projections for September and October 2025. The Domains Spend Forecast is displayed at $3,809.30. The Average Monthly Burn-down Rate is 12%. Forecast accuracy improved from plus or minus 18% to plus or minus 5% variance, enabling proactive capacity planning despite market volatility.

For the pre-implementation cost planning, we followed a structured three-step process. Step 1 was Baseline Establishment - analyzing the AWS CUR and Cost Explorer to establish the initial spend and volatility baseline. Step 2 was Scenario Modeling - using the Aquila Clouds FinOps Optimizer to model the impact of Graviton migration and Savings Plans acquisition. Step 3 was Target Setting - defining cost targets tied directly to business outcomes.

We established a Cloud Center of Excellence - a cross-functional team spanning Engineering, Finance, and Operations - to oversee ongoing cloud spend. We developed the Cost per Transaction KPI and surfaced it to engineering and product teams so that architectural decisions are informed by cost intelligence.

For training, we conducted four sessions focused on the FinOps Operating Model and Web3 Unit Economics. All session recordings and documentation were uploaded to CloudRev's internal Confluence/Wiki. The training enabled teams to implement Graviton adoption and the Commitment Strategy independently, embedding cost intelligence and the operational excellence principle into their culture."

---

# PART 7: SECURE GOVERNANCE AND COMMON REQUIREMENTS (33:00 - 38:00)

"Let me now cover the secure account governance and common requirements.

For ACCT-001, our Standard Operating Procedures cover four areas. First, root account usage - the root user is never used for workload or day-to-day activities. Access is limited to essential account-level configurations only, such as billing setup, MFA, and support plan changes. IAM is implemented for all user and access management. Second, MFA on root - immediately after account creation, MFA is enabled using a virtual MFA device. Our SOP mandates periodic MFA validation every 90 days. Third, contact information - the root account email is set to a shared corporate email alias monitored by the Cloud/DevOps team. The phone number is set to an admin number backed by an OTP service for secure access recovery. Fourth, CloudTrail configuration - CloudTrail is enabled in all AWS regions, including future regions by default. Logs are delivered to a centralized S3 bucket with versioning enabled, S3 Object Lock in Governance mode for immutability, a dedicated IAM role for write-only access, and an explicit deny policy on delete actions.

For ACCT-002, identity security. We access customer-owned AWS accounts through temporary IAM roles for both programmatic access and AWS Management Console access. We leverage Identity Federation for enterprise user identities using SAML. IAM principals are granted minimum necessary privileges - we avoid wildcards in Action and Resource elements. Every AWS Partner individual who accesses an account uses dedicated credentials. For CloudRev specifically, we implemented roles like WebServerEC2S3Role with limited permissions, ensuring least privilege, and utilized AWS SSO for user access via federated identities.

For operational excellence - OPE-001 - we define, collect, and analyze workload health metrics. For CloudRev, we monitor invocation counts, error rates, and memory usage. Logs are aggregated in CloudWatch with dashboards for visualization. Alerts are configured for defined thresholds, for example error rates exceeding 5%. We provide a standardized runbook covering routine tasks for monitoring workload health via CloudWatch, along with troubleshooting scenarios for metrics like CPU, memory, and error rates.

For OPE-003, deployment readiness. We use a standardized deployment checklist. All deployments go through automated integration tests via GitHub Actions and manual validation for critical workloads. Automated tests cover code quality and security via Snyk before production release.

For network security - NETSEC-001 - Security Groups restrict traffic between the Internet and Amazon VPC and within the VPC itself. Network ACLs are configured at the subnet level for stateless filtering. We implement AWS WAF and GuardDuty for enhanced security monitoring and threat detection.

For data encryption - NETSEC-002 - all endpoints exposed to the Internet use TLS encryption for data in transit. All customer data stored in AWS services like S3 and RDS is encrypted at rest using native encryption features. AWS KMS is used to store and manage all cryptographic keys. For CloudRev, all external API requests and internal data storage follow encryption best practices using TLS for transit and AWS KMS for key management.

For reliability - REL-001 - we leverage Terraform for automating provisioning of Amazon EKS clusters and workloads. GitHub Actions handles CI/CD. All changes to the production environment are made via GitHub Actions and Terraform. No changes are made through the AWS Management Console.

For disaster recovery - REL-002 - we recommend an RTO of 4 hours and RPO of 1 hour for critical workloads on Amazon EKS. The recovery process leverages Amazon EKS snapshots, AWS Backup, and multi-AZ deployment. Kubernetes StatefulSets handle data persistence with automated failover. Customers are made aware of recovery plans and RTO/RPO expectations during onboarding, and regular testing of backup and recovery processes is communicated as part of routine operational reviews."

---

# PART 8: ARCHITECTURE RESILIENCE AND SCALABILITY (38:00 - 41:00)

"The CloudRev architecture prioritizes global low-latency performance, massive scalability for volatile transaction volumes, and resilience against regional events.

For resilience and high availability: AWS Global Accelerator provides resilience against regional failures - if an entire AWS Region becomes unhealthy, Global Accelerator automatically reroutes traffic to the next nearest healthy regional endpoint in seconds. All compute resources - EC2 nodes and EKS - are deployed in Auto Scaling Groups across multiple Availability Zones, so an AZ failure does not impact overall availability of validator nodes or dApp backends. Amazon DynamoDB is a native multi-AZ service that synchronously replicates data across three AZs, and DynamoDB Global Tables provide active-active multi-region capability. Amazon Managed Blockchain includes built-in high availability with automatic node health management and replacement.

For auto-scaling and elasticity: The Application Load Balancer scales automatically to handle fluctuating API traffic from dApps. EC2 Validator Nodes use Auto Scaling Group dynamic scaling policies to add or remove nodes based on network load during high transaction volume. Amazon EKS uses dual-layer scaling - the Horizontal Pod Autoscaler for rapid response to traffic spikes at the pod level, and the Cluster Autoscaler to add or remove EC2 worker nodes at the infrastructure level. Amazon DynamoDB is configured in on-demand mode, instantly scaling read and write capacity to handle virtually any traffic load without throttling - ideal for unpredictable blockchain workloads.

For the TCO analysis, we used AWS pricing calculators, customer workload specifications covering CPU, storage, and network usage, and existing cloud infrastructure data. We provided a detailed cost model covering infrastructure, ongoing management, and expected scaling costs with specific estimates for compute, storage, and data transfer. We aligned the cost structure with business objectives using value stream mapping to demonstrate how AWS services enhance operational efficiency."

---

# PART 9: RESULTS AND PROJECT CLOSURE (41:00 - 44:00)

"Let me now consolidate the results. All of these are documented across our Case Study, Project Closure Report, and Project Sign-Off and Acceptance Report.

Monthly AWS Spend came down from $105,000 to $69,300 - a 34% reduction. How we achieved this: Graviton migration of 80% of eligible workloads yielding 20% price-performance improvement, a layered Savings Plans strategy reaching 85% coverage with 99% utilization, rightsizing that reduced idle and underutilized resources by 52%, and S3 Intelligent-Tiering for terabytes of ledger archives.

Tagging Compliance went from 45% to 98% - a 53 percentage point improvement. How we achieved this: mandatory five-tag policy enforced through AWS Organizations Tag Policies and SCPs, with detective controls via AWS Config Rules and automated remediation through EventBridge and Lambda.

Forecast Accuracy improved from plus or minus 18% to plus or minus 5-6% variance. How we achieved this: ML-based forecasting models using historical CUR data through the Aquila Clouds FinOps Optimizer, with historical cost trend analysis spanning February through August 2025 and predictive projections for September and October 2025.

Savings Plans Coverage went from 25% to 85% with 99% utilization. How we achieved this: Compute Savings Plans flexible across instance families, with a mix of one-year and three-year commitments based on workload stability analysis. Reserved Instance coverage went from 12% to 85%.

Unit Cost per one million transactions was reduced by 40%. How we achieved this: the combination of all optimization efforts - Graviton, Savings Plans, rightsizing, storage tiering, and network optimization - normalized against the transaction volume KPI tracked through QuickSight dashboards.

Data transfer costs were cut by 22%. How we achieved this: AWS Global Accelerator for optimized network paths, architecture refinement with data locality strategies, and optimized node placement within availability zones.

Total Annualized Savings: over $428,000.

The Project Sign-Off and Acceptance Report confirms all deliverables were completed: OU Architecture for Security, Prod, and SDLC. FinOps Dashboards using QuickSight and Cost Explorer. Tagging Strategy and SCP Implementation. Savings Plans and RI Purchase Strategy. Operational Runbooks and Training.

Four training sessions were conducted. All session recordings and documentation were uploaded to CloudRev's internal Confluence/Wiki. Account ownership and all required credentials were verified by CloudRev's Lead Architect. Formal project support concluded in November 2025, with further assistance falling under the standard Managed Services Agreement.

The project was signed off by Lalit Bansal, Founder of CloudRev, on July 22, 2025, and by Sohrab Pawar, Director of Mist Avinya Technologies LLP, on July 24, 2025. The Project Closure Report was signed by Shikha Rai, our Project Manager, and Lalit Bansal on November 21, 2025."

---

# PART 10: BUSINESS IMPACT AND LESSONS LEARNED (44:00 - 45:00)

"Three business outcomes from this engagement.

First, improved profitability per transaction enabled more competitive pricing for CloudRev's node-as-a-service offering. Second, clear cost visibility provided investors with hard data on operational efficiency and scalability. Third, freed capital was reinvested in R&D for next-generation blockchain protocols.

From our Project Closure Report, two key lessons learned. Early Engineering Buy-in - success was accelerated by involving the engineering team in the Unit Economics discussion early on, rather than treating FinOps as a purely financial exercise. And Automation is Key - relying on manual tagging was the primary failure point; moving to automated AWS Resource Groups saved 20 plus hours of manual labor per month.

Our documented recommendations for CloudRev going forward: schedule quarterly reviews of underutilized EC2 and RDS instances using AWS Compute Optimizer. Leverage private offers for third-party SaaS tools to count toward AWS Enterprise Discount Program commits. And as CloudRev expands globally, use Account Factory to ensure new regions remain compliant with established SCPs.

I will close with the customer testimonial from the Head of Engineering at Rev: 'As a Web3 infrastructure provider, unit cost is everything. Mist Avinya and Aquila Clouds gave us the financial clarity to scale efficiently. We now know our exact cost-per-transaction, which is a game-changer for our business model.'

The complete technology stack used: Amazon EC2 with AWS Graviton processors, Amazon EKS, Amazon DynamoDB, Amazon S3 with Intelligent-Tiering, Amazon Managed Blockchain, AWS Global Accelerator, AWS Cost and Usage Report, AWS Cost Explorer, AWS Budgets, AWS Organizations with SCPs, AWS Compute Optimizer, AWS Compute Savings Plans, Amazon Athena, Amazon QuickSight, and the Aquila Clouds FinOps Optimizer.

All evidence is documented across our Statement of Work, Project Closure Report, Project Sign-Off and Acceptance Report, Change Request CR-CloudREV-2025-003, Case Study, Cloud Ops Practice Requirements, and the Cloud Operation Customer Example Requirements.

Thank you. We are happy to take any questions and walk through any specific evidence in detail."

---

# DOCUMENT REFERENCE MAP (For Quick Access During Q&A)

| When Auditor Asks About | Open This Document |
|---|---|
| Multi-account strategy, OU structure | Cloud Ops Practice Requirements (COBL-001) + ProjectClosure_CloudRev |
| Account lifecycle process | Cloud Ops Practice Requirements (COBL-002) |
| GitOps, IaC, Terraform | Cloud Ops Practice Requirements (COBL-003) |
| Tagging strategy and enforcement | Cloud Ops Practice Requirements (COBL-004) |
| Cost allocation, CUR, dashboards | Cloud Ops Practice Requirements (COCFM-001, 003) + Check List |
| Forecasting methodology | Cloud Ops Practice Requirements (COCFM-002) + Check List |
| Resource optimization, rightsizing | Cloud Ops Practice Requirements (COCFM-004) |
| Savings Plans, RI strategy | Cloud Ops Practice Requirements (COCFM-005) |
| Anomaly detection, budget alerts, training | Cloud Ops Practice Requirements (COCFM-006) |
| Change management process | Rev_Change_Request_CR-2025-003 |
| Project sign-off, deliverables | Project Sign-Off and Acceptance Report_Rev |
| Project closure, KPIs, lessons learned | ProjectClosure_CloudRev |
| Case study narrative, results table | CloudRev_Casestudy + CloudOps_CFM_CaseStudy_Rev_(Public) |
| IaC code, customer examples | Cloud Operation Cust Example Requirements (EXBL-002) |
| Cost allocation dashboard screenshots | Cloud Operation Cust Example Requirements (EXCFM-003) |
| Architecture diagram, HA, scaling | Check List (DOC-001) |
| Secure account governance, IAM | Check List (ACCT-001, ACCT-002) |
| Operational excellence, runbooks | Check List (OPE-001, OPE-002, OPE-003) |
| Network security, encryption | Check List (NETSEC-001, NETSEC-002) |
| Reliability, DR, RTO/RPO | Check List (REL-001, REL-002) |
| TCO analysis | Check List (COST-001) + Cloud Operation Cust Example Requirements (EXCFM-005) |
| SOW | Statement of Work_CloudRev.pdf |
