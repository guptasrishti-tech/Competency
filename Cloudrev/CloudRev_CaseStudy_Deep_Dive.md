# Case Study Summaries + CloudRev Deep Dive
## AWS Cloud Operations Competency Audit - Mist Avinya Technologies LLP
## Format Matching PPT: Challenge | Solution | Results | AWS Services

---

## INTRODUCTION (To the Auditor)

"We have four customer case studies that demonstrate our FinOps practice across different industries. Each engagement followed our structured Inform, Optimize, Operate methodology using the Aquila Clouds platform integrated with AWS-native cost governance. Let me give you a quick summary of all four, and then we will deep-dive into CloudRev."

---

## CASE STUDY 1: CLOUDREV
### Digital Services | ~30% AWS cost reduction in 120 days

CHALLENGE:
Limited AWS cost visibility, inconsistent resource tagging, low commitment coverage and difficulty forecasting during variable demand periods.

SOLUTION:
Implemented cross-functional FinOps framework with CUR integrated into Aquila Cloud, mandatory tagging enforcement, compute rightsizing and Savings Plans adoption.

RESULTS:
~30% monthly AWS cost reduction within 120 days. Over 90% tag compliance. High Savings Plans utilization. Improved forecast accuracy.

AWS SERVICES:
EKS, Lambda, RDS, S3, CloudFront, Athena, Cost Explorer, AWS Budgets, AWS Organizations.

---

## CASE STUDY 2: FRANCISCAN
### EdTech | 25-30% cost reduction, 95% tag compliance

CHALLENGE:
Limited cost visibility across multiple environments, seasonal traffic spikes causing unpredictable AWS spend, inconsistent tagging and need for stronger governance.

SOLUTION:
Cross-functional FinOps framework with CUR via Aquila Cloud, EC2 rightsizing and scheduling, S3 lifecycle policies and AWS Budgets with anomaly detection.

RESULTS:
25-30% cost reduction over engagement. Over 95% tag compliance. Improved forecast accuracy during seasonal peaks. Centralized governance policies.

AWS SERVICES:
EC2, Lambda, DynamoDB, S3, CloudFront, Athena, Cost Explorer, AWS Budgets, AWS Organizations.

---

## CASE STUDY 3: AGRITECH (KiwiKisan)
### Agriculture Technology | Ongoing FinOps optimization on AWS

CHALLENGE:
High unpredictable data ingestion costs from IoT sensors. Always-on EC2 instances expensive to maintain. Batch processing meant farmer alerts took hours. Unable to calculate cost per acre for pricing model.

SOLUTION:
EC2 replaced with Lambda and Kinesis Data Analytics. AWS IoT Core for real-time sensor ingestion. Timestream and S3 Glacier for optimized storage. CUR integrated with Aquila Cloud for unit economics. Full infrastructure managed via Terraform IaC.

RESULTS:
40% reduction in data processing costs. 48% lower cost per acre monitored. 90% faster farmer alerts - hours to minutes. 55% storage cost reduction. 100% serverless pipeline achieved.

AWS SERVICES:
IoT Core, Kinesis Data Streams, Lambda, Amazon Timestream, S3, Glacier Instant Retrieval, SageMaker, Athena, CUR, AWS Budgets, Organizations.

---

## CASE STUDY 4: HEALTHTECH (Genepowerx)
### Healthcare Technology | Secure cloud ops with cost governance

CHALLENGE:
Unpredictable GPU hardware costs, no cost-per-scan visibility. Slow R&D cycles - provisioning new hardware took weeks. Immense HIPAA compliance overhead on physical infrastructure. Difficulty scaling inference for new hospital clients.

Expected Outcomes 
Cost Reduction: ~30% reduction in monthly AWS spend.

Tagging Compliance: 95%+ across 5 mandatory cost-allocation tags.

Reserved Coverage: 85%+ Savings Plans coverage for compute.

Forecast Accuracy: Within ±5% variance.

Unit Economics: Establish a "Cost per 1 Million Transactions" baseline

SOLUTION:
Secure multi-account AWS Organization for HIPAA workloads. Compute Savings Plans for baseline inference fleet. SageMaker Spot Training for R&D cost reduction. S3 Intelligent-Tiering for DICOM image archives. KMS encryption, IAM, Secrets Manager for compliance. CUR integrated with Aquila Cloud for cost-per-scan KPI.

RESULTS:
45% reduction in AI inference costs. 52% lower cost per medical scan analyzed. 35% ML training cost reduction via Spot Training. 60% storage cost reduction via S3 Intelligent-Tiering. 100% HIPAA compliant encryption at rest and in transit.

AWS SERVICES:
SageMaker, EC2 G5, AWS HealthImaging, S3 Intelligent-Tiering, Lambda, KMS, Secrets Manager, CloudTrail, Athena, CUR, AWS Budgets, Organizations.

---

## CLOUDREV - FULL DEEP DIVE FOR AUDITOR

"Now let me take you through CloudRev in depth. This is our lead case study and it demonstrates the full scope of our FinOps practice - from visibility and governance through optimization and customer enablement."

---

### 1. CUSTOMER PROFILE

"Rev is a Web3 infrastructure provider that operates a globally distributed network of blockchain validator nodes and transaction processing infrastructure for decentralized applications - dApps - and enterprise clients.

- Industry: Blockchain and Digital Assets
- Regions: ap-south-1 (Mumbai) and ap-south-2 (Hyderabad)
- Engagement Period: August to November 2025 (120 days)
- Engagement Type: FinOps Transformation
- Core Workloads: Amazon EC2 for validator nodes, Amazon EKS for dApp backends, Amazon Managed Blockchain, Amazon DynamoDB for off-chain state data, and Amazon S3 for ledger archives

Their workloads are compute-intensive and run 24/7 with volatile, market-driven demand patterns. The project was formally signed off on November 25, 2025. The Project Closure Report was signed by our Project Manager Shikha Rai and the CloudRev stakeholder Lalit Bansal, Founder of CloudRev."

---

### 2. BUSINESS CHALLENGES IN DETAIL

"CloudRev came to us with three categories of challenges, all documented in our Case Study and Project Closure Report:

COST MANAGEMENT:
- Skyrocketing compute costs for 24/7 validator nodes and transaction processing infrastructure generating high, unpredictable costs
- Poor cost visibility - inability to attribute costs to specific blockchains, dApps, or tenants
- No mechanism to calculate the true cost per transaction, which was threatening profitability - and for a Web3 provider, unit cost is everything

TECHNICAL:
- Massive, unoptimized ledger data storage growing exponentially without governance
- High data transfer costs between nodes and availability zones due to inefficient network architecture
- Significant idle and underutilized resources across the infrastructure

GOVERNANCE:
- Tag compliance was at only 45% across cost allocation tags
- Forecast accuracy had a variance of plus or minus 18% due to market volatility
- Savings Plans adoption was at only 25% coverage on core compute workloads
- Reserved Instance coverage was at only 12%

Their monthly AWS spend was at a baseline of $105,000. The combination of these challenges meant CloudRev could not calculate their true cost-per-transaction."

Expected Outcomes 
Cost Reduction: ~30% reduction in monthly AWS spend.

Tagging Compliance: 95%+ across 5 mandatory cost-allocation tags.

Reserved Coverage: 85%+ Savings Plans coverage for compute.

Forecast Accuracy: Within ±5% variance.

Unit Economics: Establish a "Cost per 1 Million Transactions" baseline.

### 3. SOLUTION - FINOPS FRAMEWORK (INFORM, OPTIMIZE, OPERATE)

"We established a comprehensive FinOps practice aligned with the FinOps Foundation framework and the AWS Well-Architected Framework Cost Optimization Pillar. Here is how we executed each phase:

PHASE 1 - INFORM (Visibility and Allocation):

AWS Services and Tools Deployed:
- AWS Cost and Usage Report: Configured with hourly granularity and resource-level tagging
- Aquila Clouds FinOps Platform: Integrated for AI-driven cost analysis and recommendations
- Amazon QuickSight: Custom dashboards for unit economics tracking - cost per one million transactions
- Amazon Athena: SQL-based analysis of CUR data for deep-dive investigations

Cost Allocation Strategy:
We implemented a mandatory tagging framework with five required tags:
- Chain (blockchain identifier)
- Tenant (customer/application)
- Node-Type (validator, RPC, archive)
- Environment (prod, dev, staging)
- Owner (team responsible)

These were enforced via AWS Organizations Service Control Policies. The result: tag compliance went from 45% to 98%.

How we enforced tagging - we used both preventative and detective controls:
- Preventative: AWS Tag Policies within Organizations defining allowed tag keys and permitted values. SCPs that deny ec2:RunInstances if the request does not include mandatory tags. IAM Policies with condition keys like aws:RequestTag/environment. And tagging checks integrated into the CI/CD pipeline using AWS CloudFormation Guard and Checkov that fail the pipeline if templates are missing required tags.
- Detective: AWS Config Rules using the required-tags managed rule to check EC2 instances and S3 buckets. AWS Resource Groups and Tag Editor to search for resources missing specific tags. Custom automation using Amazon EventBridge and AWS Lambda for automated remediation - when Config detects a non-compliant resource, EventBridge triggers a Lambda function that either applies a default tag or sends a notification to the resource owner."

---

"PHASE 2 - OPTIMIZE (Cost Reduction):

Compute Optimization:
- AWS Graviton Migration: We migrated 80% of eligible workloads - validator nodes - to Graviton-based EC2 instances. Result: 20% price-performance improvement. This is aligned with the AWS best practice for cost-effective compute.
- The IaC mandates Graviton instance types - c6g.2xlarge - in the EKS cluster. The max_size of 25 in the scaling configuration allows dynamic scaling of validator nodes to meet volatile transaction volume.
- Through Change Request CR-CloudREV-2025-003, submitted September 15, 2025 during Phase 3 Week 8, we extended the Graviton migration scope to include 8 RDS PostgreSQL instances migrating to Graviton-based db.r7g instance types, and right-sized 3 EKS clusters in dev/staging environments. This was approved by our Project Manager Shikha Rai and CloudRev Technical Lead Harmeet Kaur. Zero additional cost - work covered under existing SOW. Expected additional savings: $8,000 to $10,000 per month. The business justification was that production Graviton migration had already achieved 20% cost savings, extending to RDS would yield additional 12-15% database cost reduction, and dev/staging environments were consuming 18% of total compute spend.
- AWS Compute Optimizer: Enabled for right-sizing recommendations. Reduced idle and underutilized resources by 52%.
- AWS Compute Savings Plans: Comprehensive strategy for always-on infrastructure. Coverage increased from 25% to 85%. Utilization rate: 99% - exceeding the industry best practice threshold of 95%. Flexible commitment model using Compute Savings Plans across instance families. Term: mix of one-year and three-year commitments based on workload stability.

Storage Optimization:
- Amazon S3 Intelligent-Tiering: Implemented for terabytes of ledger archives. Automatic cost optimization without performance impact. Reduced storage costs for infrequently accessed blockchain snapshots.
- S3 Lifecycle Policies: Configured for automated data archival and deletion.

Network Optimization:
- AWS Global Accelerator: Optimized network paths for validator node communication.
- Architecture Refinement: Reduced cross-AZ data transfer by 22%. Implemented data locality strategies and optimized node placement within availability zones.

PHASE 3 - OPERATE (Governance and Continuous Improvement):

Cost Governance:
- AWS Budgets: Configured with multi-dimensional alerts - account-level budgets, service-level budgets for EC2, EKS, and S3, and tag-based budgets per blockchain and per tenant.
- Aquila Clouds Anomaly Detection: Real-time spend spike alerts. The dashboard shows Anomaly Detection Status and an explicit Mitigation Status - for example Mitigated - confirming automated or guided remediation.
- Budget Overrun Alerts: Configured to notify via Email and a specific webhook URL for integrating with a third-party tool - satisfying the requirement for alerts via email and at least one additional third-party tool.
- AWS Organizations: Multi-account structure isolating blockchains and environments. Consolidated billing for volume discounts. Centralized cost management.

Forecasting and Planning:
- Developed ML-based forecasting models using historical CUR data
- Improved forecast accuracy from plus or minus 18% to plus or minus 5% variance
- Enabled proactive capacity planning despite market volatility

Cloud Center of Excellence:
- Established a cross-functional team to oversee ongoing cloud spend
- Developed the Cost per Transaction KPI, aligning engineering decisions with business value"

**Real Outcomes (Final Results)**
Cost Reduction: 34% ($105k → $69.3k) — Exceeded target.

Tagging Compliance: 98% — Exceeded target.

Reserved Coverage: 85% — Met target.

Forecast Accuracy: ±6% — Minor Gap.

Unit Economics: 40% reduction in cost per 1 million transactions.

### 4. MULTI-ACCOUNT STRATEGY AND OU ARCHITECTURE

"Following AWS Best Practices, we restructured CloudRev's environment using AWS Control Tower to automate the setup of a landing zone, providing a drift-resistant foundation.

The AWS Organizations structure implemented:

Root (Management Account)
- Production OU: Ethereum-Prod, Solana-Prod, Polygon-Prod
- Non-Production OU: Dev-Environment, Test-Environment, Staging-Environment
- Shared Services OU: Networking-Hub, Security-Logging, FinOps-Central

Security OU: Centralized logging through CloudTrail and Config, security tooling via GuardDuty.
Infrastructure OU: Shared services including networking and centralized egress.
Workload OUs: Segregated into Production and Non-Production to prevent blast radius issues and allow for distinct cost allocation.
Sandbox OU: Gated environment for R&D with strict automated spending limits.

The account lifecycle follows four stages:
- Stage 1: Request and Approval through internal portal with business justification, designated owner, cost center, and desired OU placement
- Stage 2: Automated Provisioning via AWS Control Tower Account Factory, with Account Blueprint deployed through AWS Step Functions state machine triggered by Control Tower Lifecycle Events
- Stage 3: Operation and Governance with continuous monitoring by AWS Security Hub, Amazon GuardDuty, and AWS Config, updates via CloudFormation StackSets
- Stage 4: Decommissioning through Suspended OU with restrictive SCPs, data archival to immutable S3 bucket, and formal closure after 90-day waiting period"

---

### 5. INFRASTRUCTURE AS CODE

"Terraform defines the entire CloudRev solution. Our methodology follows a four-phase GitOps workflow:

Phase 1 - Discovery and Scoping: Workshops with developers, operations, and security teams to identify application needs, networking constructs - VPC architecture, subnets, routing tables, security groups, NACLs - and compliance requirements.

Phase 2 - Design and Tool Selection: Modular, reusable IaC templates with a standardized Git repository structure. GitFlow branching strategy for dev, staging, and production.

Phase 3 - Implementation and Pipeline Construction: CI/CD pipeline with automated testing - static analysis using cfn-lint or tflint, security scanning using tfsec, checkov, or AWS CloudFormation Guard, and validation using provider-native tools.

Phase 4 - Deployment and Operations: Infrastructure changes deployed by merging peer-reviewed pull requests. AWS CloudTrail provides a complete audit trail. Rollback by reverting the commit in Git and re-running the pipeline.

For CloudRev specifically:
- IaC mandates Graviton instance types c6g.2xlarge in the EKS cluster
- max_size of 25 in the scaling config allows dynamic scaling of validator nodes
- Code enforces 5 mandatory FinOps tags: Owner, Chain, Tenant, Node-Type, Environment
- All changes go through GitHub Actions - no manual configuration through the AWS Management Console"

---

### 6. COST PLANNING AND FORECASTING (Pre-Implementation)

"Before implementation, we executed a structured three-step modeling process:

Step 1 - Baseline Establishment: The AWS Cost and Usage Report and Cost Explorer were analyzed to establish the initial spend and volatility baseline - where forecast accuracy was at plus or minus 18% variance due to volatile market conditions.

Step 2 - Scenario Modeling: The Aquila Clouds FinOps Optimizer was used to model optimization scenarios, including the impact of migrating compute to AWS Graviton processors and the acquisition of a specific Savings Plans commitment level.

Step 3 - Target Setting: High-level cost targets tied directly to business outcomes were defined.

The evidence supports historical cost trend analysis displaying cost data from February 2025 through August 2025. The Top 10 Subdomain Budget and Budget Burn-down table includes cost projections for September 2025 and October 2025. The Average Monthly Burn-down Rate is 12%. The Domains Spend Forecast is explicitly displayed at $3,809.30."

---

### 7. COST ALLOCATION STRATEGY

"The cost allocation strategy was designed to move beyond total blended cost and provide unit-level financial accountability for compute-intensive Web3 infrastructure.

Accounts and OUs:
- Root: Management Account
- OU Infrastructure: Shared Services (Networking, Logging)
- OU Production: Production workload accounts per blockchain/region
- OU Non-Production: Dev/Test accounts

Mandatory Cost Allocation Tags:
- Owner (e.g., Finance, Blockchain-X-Team)
- Environment (e.g., Prod, Dev, Staging)
- Application (e.g., ValidatorNode-X, TransactionProcessor)

Cost Categories: Costs categorized by service, environment, and unit of business - cost-per-transaction.

Policy Enforcement: AWS Organizations centrally enforced the mandatory 5-tag policy - compliance raised from 45% to 98%.
Data Analysis: AWS CUR fed data into Aquila Clouds FinOps Optimizer for anomaly detection, optimization recommendations, and unit cost calculation."

---

### 8. COST ALLOCATION DASHBOARD (EXCFM-003)

"The primary visualization is the Aquila Clouds FinOps Cost and Unit Economics Dashboard. It enables filtering and grouping of all AWS costs, aligning spend directly with business context.

Data Source: Monthly spend filtered by mandatory FinOps Tags - CostCenter, Owner, App-Name - derived directly from the AWS Cost and Usage Report ingested by Aquila Clouds.

Key Dashboard Components:
- Primary Cost Grouping: Spend broken down by mandatory tags like Cost Center (e.g., EIY-A23) and Node-Type (Validator for CloudRev)
- Unit Economics: A trend line widget displaying the true cost of delivery - Cost per 1 Million Processed Transactions - calculated by normalizing allocated cost against the business metric
- Tag Compliance: Real-time Tag Compliance Score showing the uplift from 45% to 98% across the five mandatory tags
- Shared Services Allocation: Shared costs from central networking and security accounts automatically allocated back to consuming teams based on predefined rules or consumption metrics

The cost trend view highlights month-over-month changes in actual spend, making it easier to identify cost reductions, anomalies, or unexpected spikes. By leveraging AWS-native reporting and analytics services, this setup enables accurate cost attribution, transparent reporting, and informed financial decision-making."

---

### 9. ARCHITECTURE - RESILIENCE AND SCALABILITY (DOC-001)

"The CloudRev architecture prioritizes global low-latency performance, massive scalability for volatile transaction volumes, and resilience against regional events.

Resilience and High Availability:
- AWS Global Accelerator: Provides resilience against regional failures. If an entire AWS Region becomes unhealthy, Global Accelerator automatically reroutes traffic to the next nearest healthy regional endpoint in seconds.
- Compute Tier (EC2 Nodes and EKS): All compute resources deployed in Auto Scaling Groups across multiple AZs. An AZ failure will not impact overall availability of validator nodes or dApp backends.
- Amazon DynamoDB: Native multi-AZ service, synchronously replicates data across three AZs. DynamoDB Global Tables provide active-active multi-region database for global resilience.
- Amazon Managed Blockchain: Built-in high availability with automatic node health management and replacement.

Auto-Scaling and Elasticity:
- Application Load Balancer: Scales automatically to handle fluctuating API traffic from dApps.
- EC2 Validator Nodes: Auto Scaling Group uses dynamic scaling policies to add or remove nodes based on network load or blockchain-specific metrics during high transaction volume.
- Amazon EKS (Dual-Layer): Horizontal Pod Autoscaler and Cluster Autoscaler allow the dApp backend to scale seamlessly in response to volatile demand.
- Amazon DynamoDB: Configured in on-demand mode, instantly scales read and write capacity to handle virtually any traffic load without throttling - ideal for unpredictable blockchain workloads."

---

### 10. TRAINING PLAN (EXCFM-004)

"The Cloud Financial Management training plan for CloudRev focused on the FinOps Operating Model and Web3 Unit Economics. Training enabled teams to implement Graviton adoption and a Commitment Strategy to reduce Cost-per-Transaction by over 40%.

Four training sessions were conducted. All session recordings and documentation were uploaded to CloudRev's internal Confluence/Wiki. The training established a Cloud Center of Excellence - a cross-functional team spanning Engineering, Finance, and Operations - and empowered engineering and product teams by integrating real-time unit-cost KPIs into their decision cycles, successfully embedding cost intelligence and the operational excellence principle."

---

### 11. TOTAL COST OF OWNERSHIP (EXCFM-005)

"The TCO analysis for CloudRev addressed the challenge of skyrocketing compute costs for 24/7 validator nodes, poor cost visibility by chain and tenant, and the inability to link cloud spend to the true cost per transaction.

Inputs for Estimating Cost: AWS pricing calculators, customer workload specifications - CPU, storage, network usage - and existing cloud infrastructure. We factored in the cost of managed services like EKS and EC2 and any third-party services.

Cost Estimates and Model: A detailed cost model covering infrastructure, ongoing management, and expected scaling costs. Specific estimates for compute (EC2 instances, spot pricing), storage (S3, EBS), and data transfer costs.

Business Value Analysis: Aligned the solution's cost structure with business objectives using value stream mapping to show how AWS services enhance operational efficiency and reduce long-term costs."

---

### 12. CHANGE MANAGEMENT EVIDENCE (PRJ-005)

"Change Request CR-CloudREV-2025-003:
- Date Submitted: September 15, 2025
- Submitted By: Aquila Clouds FinOps Team
- Project Phase: Phase 3 - Cost Optimization (Week 8)
- Priority: High | Impact: Medium
- Estimated Effort: 2 weeks (Weeks 9-10)
- Cost Impact: $0 - within existing SOW

Change Description: Extend AWS Graviton migration scope to include additional RDS database instances and expand EKS cluster optimization to cover development and staging environments. Original scope covered production validator nodes only.

Business Justification:
- Production Graviton migration achieved 20% cost savings; extending to RDS will yield additional 12-15% database cost reduction
- Dev/staging environments currently consume 18% of total compute spend; optimization will reduce waste
- Accelerates path to 34% cost reduction target - currently at 28%

In Scope: Migrate 8 RDS PostgreSQL instances to Graviton-based db.r7g instance types. Right-size and optimize 3 EKS clusters in dev/staging environments. Extend Savings Plans coverage to include new Graviton RDS instances.

Out of Scope: Application code changes (Graviton is binary-compatible). Production workload migration (already completed in original scope).

Approvals: Shikha Rai (Mist Avinya Project Manager) - Approved. Harmeet Kaur (CloudRev Technical Lead) - Approved."

---

### 13. PROJECT SIGN-OFF AND CLOSURE

"The Project Sign-Off and Acceptance Report confirms the FinOps Transformation Program was completed in accordance with the Statement of Work. The project transitioned CloudRev's AWS environment into a governed, optimized, and scalable architecture.

Deliverable Checklist - All Completed:
- OU Architecture (Security, Prod, SDLC)
- FinOps Dashboards (QuickSight/Cost Explorer)
- Tagging Strategy and SCP Implementation
- Savings Plans and RI Purchase Strategy
- Operational Runbooks and Training

Financial Summary:
- Total Annualized Savings: $428,000
- Monthly Spend Reduction: 34%
- Unit Cost Optimization: 40% reduction in cost per 1M transactions

Signed by Lalit Bansal (Founder, CloudRev) on 22 July 2025 and Sohrab Pawar (Director, Mist Avinya Technologies LLP) on 24 July 2025.

Account Ownership: All required credentials verified by CloudRev's Lead Architect.
Knowledge Transfer: 4 training sessions conducted; all recordings and documentation uploaded.
Support Transition: Formal project support concluded November 2025. Further assistance under standard Managed Services Agreement."

---

### 14. CONSOLIDATED RESULTS TABLE

| KPI | Before | After (120 days) | Improvement |
|---|---|---|---|
| Monthly AWS Spend | $105,000 | $69,300 | 34% Reduction |
| Tagging Compliance | 45% | 98% | +53 percentage points |
| Forecast Accuracy (Variance) | plus/minus 18% | plus/minus 5-6% | High Precision |
| Reserved/SP Coverage | 12-25% | 85% | Optimization Peak |
| SP Utilization | - | 99% | Above 95% best practice |
| Idle/Underutilized Resources | Baseline | Reduced | 52% Reduction |
| Unit Cost per 1M Transactions | Baseline (100) | 60 | 40% Reduction |
| Monthly Data Transfer Cost | Baseline (100) | 78 | 22% Reduction |
| Total Annualized Savings | - | $428,000 | - |

---

### 15. CUSTOMER TESTIMONIAL

"As a Web3 infrastructure provider, unit cost is everything. Mist Avinya and Aquila Clouds gave us the financial clarity to scale efficiently. We now know our exact cost-per-transaction, which is a game-changer for our business model."
- Head of Engineering, Rev

---

### 16. BUSINESS IMPACT

"Three key outcomes:

1. Improved profitability per transaction enabled more competitive pricing for their node-as-a-service offering
2. Clear cost visibility provided investors with hard data on operational efficiency and scalability
3. Freed capital was reinvested in R&D for next-generation blockchain protocols

Lessons Learned from the Project Closure Report:
- Early Engineering Buy-in: Success was accelerated by involving the engineering team in the Unit Economics discussion early on, rather than treating FinOps as a purely financial exercise
- Automation is Key: Relying on manual tagging was the primary failure point; moving to automated AWS Resource Groups saved 20+ hours of manual labor per month

Final Recommendations documented:
- Continuous Rightsizing: Schedule quarterly reviews of underutilized EC2 and RDS instances using AWS Compute Optimizer
- Marketplace Optimization: Leverage private offers for third-party SaaS tools to count toward AWS Enterprise Discount Program commits
- Expansion of Control Tower: As CloudRev expands globally, use Account Factory to ensure new regions remain compliant with established SCPs"

---

### 17. TECHNOLOGIES USED (Complete List)

Core AWS Services:
- Amazon EC2 (with AWS Graviton processors)
- Amazon EKS (Elastic Kubernetes Service)
- Amazon DynamoDB
- Amazon S3 (with Intelligent-Tiering)
- Amazon Managed Blockchain
- AWS Global Accelerator

Cost Management and Governance:
- AWS Cost and Usage Report (CUR)
- AWS Cost Explorer
- AWS Budgets
- AWS Organizations (with SCPs)
- AWS Compute Optimizer
- AWS Compute Savings Plans

Analytics and Reporting:
- Amazon Athena
- Amazon QuickSight
- Aquila Clouds FinOps Optimizer (recommendations, governance, forecasting)

---

## CLOSING

"This case study demonstrates comprehensive AWS CloudOps competency with a strong focus on FinOps practices. The engagement delivered significant cost savings through strategic use of AWS services, improved business outcomes enabling competitive advantage, sustainable practices with 98% tag compliance and high forecast accuracy, and AWS best practices alignment across Cost Optimization and Operational Excellence pillars. We are happy to take any questions."
