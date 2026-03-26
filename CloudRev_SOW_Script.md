# CloudRev SOW Explanation Script - AWS Cloud Operations Competency Audit
# Statement of Work: Mist Avinya Technologies LLP | CloudRev FinOps Transformation
# For Auditor Presentation

---

# OPENING

"I will now walk you through our Statement of Work for the CloudRev engagement. This SOW defines the scope, methodology, deliverables, project plan, team structure, and acceptance criteria for the FinOps Transformation Program. I will explain each section and then show how we actually executed against it."

---

# 1. EXECUTIVE SUMMARY

"The SOW opens with the Executive Summary. CloudRev is a leading Web3 infrastructure provider operating globally distributed blockchain validator nodes and transaction processing infrastructure. This engagement addresses critical financial operations challenges including skyrocketing compute costs, poor cost visibility, and inability to calculate unit economics.

The Business Objectives defined in the SOW were:
- Reduce overall AWS costs by around 30% within 120 days
- Establish a unit economics framework, specifically cost per million transactions
- Achieve 85% or higher Savings Plans coverage with 95% or higher utilization
- Improve tag compliance to 95% or higher across mandatory cost allocation tags

How we delivered against each of these:
The 30% cost reduction target - we achieved 34%. Monthly spend came down from $105,000 to $69,300. We did this through Graviton migration of 80% of eligible workloads yielding 20% price-performance improvement, a layered Savings Plans strategy, rightsizing that reduced idle resources by 52%, S3 Intelligent-Tiering for ledger archives, and architecture refinement that cut data transfer costs by 22%.

The unit economics framework - we established the Cost per 1 Million Processed Transactions KPI. We built custom dashboards in Amazon QuickSight and the Aquila Clouds platform. The CUR was configured with hourly granularity and resource-level tagging, feeding into Amazon Athena for SQL-based analysis. The unit cost per million transactions was reduced by 40%.

The Savings Plans target - we achieved 85% coverage with 99% utilization, exceeding the 95% utilization target. We used Compute Savings Plans flexible across instance families, with a mix of one-year and three-year commitments based on workload stability.

The tag compliance target - we achieved 98%, exceeding the 95% target. We enforced five mandatory tags: Chain, Tenant, Node-Type, Environment, and Owner through AWS Organizations Tag Policies, SCPs, IAM condition keys, CI/CD pipeline checks using Checkov and CloudFormation Guard, and detective controls via AWS Config Rules with EventBridge and Lambda for automated remediation.

The Expected Outcomes defined in the SOW were:
- Improved profitability per transaction enabling competitive pricing
- Clear cost visibility for investor reporting
- Freed capital for R&D investment in next-generation protocols
- Sustainable FinOps culture for continuous optimization

All four were achieved. The 40% unit cost reduction enabled competitive pricing for their node-as-a-service offering. Cost visibility provided investors with hard data on operational efficiency. Freed capital was reinvested in R&D. And we established a Cloud Center of Excellence with four training sessions, all recordings uploaded to CloudRev's Confluence/Wiki."

---

# 2. SCOPE OF WORK

"The SOW defines the objective as: Define and implement a secure, scalable multi-account AWS environment and FinOps framework aligned with AWS best practices to optimize Web3 infrastructure costs and operational efficiency.

The engagement is strictly limited to Cloud Financial Management, or CFM. Application code changes, infrastructure re-architecture beyond cost optimization, and workload migration are explicitly out of scope.

How we executed this: We restructured CloudRev's environment using AWS Control Tower and AWS Organizations. The multi-account structure we deployed isolates workloads by blockchain and environment:

Root - Management Account
- Production OU: Ethereum-Prod, Solana-Prod, Polygon-Prod
- Non-Production OU: Dev-Environment, Test-Environment, Staging-Environment
- Shared Services OU: Networking-Hub, Security-Logging, FinOps-Central

We also established a Security OU for centralized logging via CloudTrail and Config with GuardDuty, and a Sandbox OU for R&D with strict spending limits. This structure ensures security, compliance, cost attribution, and blast radius isolation - all within the CFM scope defined in the SOW."

---

# 3. METHODOLOGY - 4-PHASE PLAYBOOK APPROACH

"The SOW defines our methodology as a 4-Phase Playbook. Let me walk through each phase and explain exactly how we executed it.

PHASE 1: DISCOVERY

The SOW states: Current State Assessment - audit of existing AWS account structure and compute spend. Categorization - group workloads by blockchain protocol and environment, Prod and Dev. Requirements Gathering - define security baselines and cost allocation, specifically Unit Economics.

How we executed Phase 1: We conducted workshops with key stakeholders from Engineering, Finance, and Operations. We analyzed the AWS Cost and Usage Report and Cost Explorer to establish the baseline - $105,000 monthly spend, 45% tag compliance, 25% Savings Plans coverage, plus or minus 18% forecast variance, and 12% Reserved Instance coverage. We categorized workloads by blockchain - Ethereum, Solana, Polygon - and by environment. We identified the core business metric as cost per million transactions. We documented all findings in a Discovery and Assessment Report that defined the design principles for the engagement.

PHASE 2: GOVERNANCE BLUEPRINT

The SOW states: An AWS governance structure was defined by organizing accounts into Production, Non-Production, and Shared Services OUs, implementing Service Control Policies, and defining mandatory tags - Owner, Project, Blockchain_Type, Environment.

How we executed Phase 2: We designed the OU architecture mapped to CloudRev's business structure. We created the Target State Architecture including OU diagrams, network topology, and the governance plan. We defined the five mandatory tags - Chain, Tenant, Node-Type, Environment, Owner - and designed the enforcement strategy using both preventative controls (Tag Policies, SCPs that deny ec2:RunInstances without mandatory tags, IAM condition keys, CI/CD pipeline checks) and detective controls (AWS Config required-tags rule, Resource Groups, EventBridge plus Lambda remediation). We designed the Account Blueprint using AWS CloudFormation for standardized account provisioning through Control Tower Account Factory. We defined the Savings Plans and commitment strategy based on workload stability analysis.

PHASE 3: DEPLOYMENT

The SOW states: Visibility - Configure Consolidated Billing and link Aquila Clouds for real-time cost tracking.

How we executed Phase 3: We deployed AWS Control Tower and the full OU architecture. We configured the AWS Cost and Usage Report with hourly granularity and resource-level tagging. We integrated the Aquila Clouds FinOps Platform for AI-driven cost analysis. We built custom dashboards in Amazon QuickSight for unit economics tracking. We set up Amazon Athena for SQL-based CUR analysis. We executed the compute optimization - migrating 80% of eligible workloads to Graviton instances (c6g.2xlarge in EKS), enabling AWS Compute Optimizer for rightsizing, implementing the Savings Plans strategy. We deployed S3 Intelligent-Tiering and lifecycle policies. We configured AWS Global Accelerator and optimized node placement for data transfer reduction. We set up AWS Budgets with multi-dimensional alerts and Aquila Clouds anomaly detection.

During Phase 3, Week 8, we submitted Change Request CR-CloudREV-2025-003 on September 15, 2025 to extend the Graviton migration scope to 8 RDS PostgreSQL instances (db.r7g) and right-size 3 EKS clusters in dev/staging. At that point we were at 28% cost reduction and this change accelerated the path to the 34% target. It was approved by Shikha Rai and Harmeet Kaur at zero additional cost under the existing SOW.

PHASE 4: OPERATIONAL READINESS

The SOW states: Handoff - Deliver runbooks for account lifecycle management and cost anomaly response.

How we executed Phase 4: We conducted four comprehensive training sessions focused on the FinOps Operating Model and Web3 Unit Economics. All session recordings and documentation were uploaded to CloudRev's internal Confluence/Wiki. We delivered operational runbooks covering account lifecycle management, cost anomaly response, tagging governance, and budget alert escalation. We established the Cloud Center of Excellence - a cross-functional team spanning Engineering, Finance, and Operations. We completed the knowledge transfer and verified all account ownership credentials with CloudRev's Lead Architect. We set up the continuous optimization cadence including quarterly rightsizing reviews using AWS Compute Optimizer."

---

# 4. CHANGE MANAGEMENT PROCESS

"The SOW defines a formal Change Management Process. It states: To maintain audit integrity and workload stability, which is critical for blockchain nodes, a 3-Step Audit-Ready Workflow is followed.

Step 1 - Submission: A formal Change Request with Technical Justification and Scope change.
Step 2 - Impact Analysis: Mist Avinya assesses impacts on the Cloud Budget and Project Timeline.
Step 3 - Approval: Written authorization from the CloudRev Head of Engineering is required before any production change.

How we executed this: We have one documented Change Request that demonstrates this process in action - CR-CloudREV-2025-003.

- Date Submitted: September 15, 2025
- Submitted By: Aquila Clouds FinOps Team
- Project Phase: Phase 3 - Cost Optimization, Week 8
- Priority: High
- Impact: Medium
- Estimated Effort: 2 weeks, Weeks 9 to 10

The Change Description was to extend AWS Graviton migration scope to include additional RDS database instances and expand EKS cluster optimization to cover development and staging environments. Original scope covered production validator nodes only.

The Technical Justification: Production Graviton migration had achieved 20% cost savings. Extending to RDS would yield additional 12 to 15% database cost reduction. Dev/staging environments were consuming 18% of total compute spend. This accelerated the path to the 34% cost reduction target - at the time of submission we were at 28%.

The Scope: Migrate 8 RDS PostgreSQL instances to Graviton-based db.r7g instance types. Right-size and optimize 3 EKS clusters in dev/staging environments. Extend Savings Plans coverage to include new Graviton RDS instances. Out of scope: application code changes (Graviton is binary-compatible) and production workload migration (already completed).

The Impact Analysis: Technical Impact was Low - Graviton compatibility had been validated in Phase 3 with minimal risk. Timeline Impact was a 2-week extension absorbed within the Phase 3 buffer. Cost Impact was zero - work covered under existing SOW. Expected Savings were an additional $8,000 to $10,000 per month.

The Approvals: Shikha Rai, Mist Avinya Project Manager - Approved. Harmeet Kaur, CloudRev Technical Lead - Approved.

This demonstrates a mature, documented change management process that maintains audit integrity while allowing scope optimization when justified by data."

---

# 5. ASSUMPTIONS, DEPENDENCIES AND OUT OF SCOPE

"The SOW states: The project assumes availability of relevant AWS access based on the principle of least privilege. Successful execution depends on timely stakeholder participation for cost optimization decisions, onboarding of required tools or licensing if applicable, and collaboration with technical and finance teams during periodic review sessions to validate cost and operational metrics.

How we managed this: For AWS access, we followed our Standard Operating Procedures - accessing customer-owned accounts through temporary IAM roles for both programmatic and console access, leveraging Identity Federation using SAML. IAM principals were granted minimum necessary privileges with no wildcards in Action and Resource elements. Every Mist Avinya individual used dedicated credentials.

For stakeholder participation, the Project Closure Report documents that early engineering buy-in was a key success factor. We involved the engineering team in the Unit Economics discussion early on rather than treating FinOps as a purely financial exercise.

The SOW states Out of Scope: This engagement is strictly limited to Cloud Financial Management. We maintained this boundary throughout. The Change Request CR-2025-003 explicitly noted that application code changes were out of scope - Graviton migration is binary-compatible and does not require code changes."

---

# 6. HIGH-LEVEL PROJECT PLAN AND TEAM

"The SOW includes a High-Level Project Plan with timeline and a Project Team and Effort allocation. It states: To satisfy the auditor, ensure you document the actual hours spent. Below is the standard allocation for a high-maturity AWS Partner engagement.

How we executed this: The engagement ran from August 4, 2025 to November 20, 2025 as documented in the Project Closure Report. Our Project Manager was Shikha Rai, who is documented on both the Project Closure Report and the Change Request. The CloudRev stakeholder was Lalit Bansal, Founder. The CloudRev Technical Lead was Harmeet Kaur, documented on the Change Request approval.

The SOW also includes a RACI Matrix defining:
- R = Responsible, does the work
- A = Accountable, owns the outcome and final approval
- C = Consulted, provides input and expertise
- I = Informed, kept up to date on progress

This RACI was applied across all project activities. For example, for the Change Request, Mist Avinya was Responsible for the technical analysis and submission, CloudRev's Technical Lead was Accountable for approval, and the broader stakeholder group was Informed of the scope change and its impact."

---

# 7. ACCEPTANCE CRITERIA

"The SOW defines four acceptance criteria. The engagement is considered successfully completed when these four conditions are met and signed off by the Customer.

Condition 1 - Financial Governance: The FinOps Operating Model is active, featuring automated cost dashboards, a defined unit economics framework, and validated cost-per-transaction metrics.

How we met this: The Aquila Clouds FinOps Cost and Unit Economics Dashboard is active and operational. It provides real-time cost visibility filtered by mandatory FinOps tags, with a specific widget tracking the Cost per 1 Million Processed Transactions trend line. AWS Budgets are configured with multi-dimensional alerts at account, service, and tag levels. Anomaly detection is active with automated mitigation. The cost-per-transaction metric is validated and surfaced to engineering and product teams through QuickSight dashboards. The Project Sign-Off confirms this deliverable as Completed - FinOps Dashboards using QuickSight and Cost Explorer.

Condition 2 - Realized Value: Cost optimization initiatives are fully implemented, demonstrating a 30% or higher reduction in monthly spend, or a clear path validated by AWS Cost Explorer.

How we met this: We achieved 34% reduction - from $105,000 to $69,300 monthly. Total annualized savings exceeded $428,000. The specific initiatives: Graviton migration (20% price-performance gain), Savings Plans (25% to 85% coverage, 99% utilization), rightsizing (52% reduction in idle resources), S3 Intelligent-Tiering for ledger archives, and network optimization (22% data transfer cost reduction). The unit cost per million transactions dropped by 40%. All validated through AWS Cost Explorer and the Aquila Clouds platform. The Project Sign-Off confirms: Total Annualized Savings $428,000, Monthly Spend Reduction 34%, Unit Cost Optimization 40% reduction.

Condition 3 - Operational Handoff: Final knowledge transfer is completed, supported by Standard Operating Procedures and SOPs.

How we met this: Four training sessions were conducted focused on the FinOps Operating Model and Web3 Unit Economics. All session recordings and documentation were uploaded to CloudRev's internal Confluence/Wiki. Operational runbooks were delivered covering account lifecycle management, cost anomaly response, tagging governance, and budget alert escalation. A Cloud Center of Excellence was established as a cross-functional team. Account ownership and all required credentials were verified by CloudRev's Lead Architect. The Project Sign-Off confirms this deliverable as Completed - Operational Runbooks and Training.

Condition 4 (implied from the deliverable checklist): Infrastructure governance is in place.

How we met this: The OU Architecture for Security, Prod, and SDLC is deployed and operational. The Tagging Strategy and SCP Implementation is enforced - 98% compliance. The Savings Plans and RI Purchase Strategy is active - 85% coverage, 99% utilization. All confirmed as Completed in the Project Sign-Off deliverable checklist."

---

# 8. PROJECT SIGN-OFF

"The formal acceptance of the SOW deliverables is documented in the Project Sign-Off and Acceptance Report.

The report confirms that the FinOps Transformation Program for CloudRev has been completed in accordance with the Statement of Work. The project transitioned CloudRev's AWS environment into a governed, optimized, and scalable architecture.

The Deliverable Checklist with stakeholder initials - all marked Completed:
- OU Architecture (Security, Prod, SDLC)
- FinOps Dashboards (QuickSight/Cost Explorer)
- Tagging Strategy and SCP Implementation
- Savings Plans and RI Purchase Strategy
- Operational Runbooks and Training

Financial Summary of Achievement:
- Total Annualized Savings: $428,000
- Monthly Spend Reduction: 34%
- Unit Cost Optimization: 40% reduction in cost per 1M transactions

Handover Notes:
- Account Ownership: All required credentials verified by CloudRev's Lead Architect
- Knowledge Transfer: 4 training sessions conducted; all session recordings and documentation uploaded
- Support Transition: Formal project support concludes November 2025. Further assistance falls under the standard Managed Services Agreement.

Signed by Lalit Bansal, Founder of CloudRev, on July 22, 2025.
Signed by Sohrab Pawar, Director of Mist Avinya Technologies LLP, on July 24, 2025.

The Project Closure Report was additionally signed by Shikha Rai, Mist Project Manager, and Lalit Bansal on November 21, 2025, confirming all deliverables defined in the SOW were met and all documentation, runbooks, and dashboard ownership were transferred to the CloudRev internal team."

---

# CLOSING

"To summarize the SOW walkthrough: every objective, every phase, every acceptance criterion defined in the Statement of Work has been met and exceeded. The 30% cost reduction target was exceeded at 34%. The 85% Savings Plans target was met at 85% with 99% utilization. The 95% tag compliance target was exceeded at 98%. The unit economics framework is active and operational. The knowledge transfer is complete. And the project has been formally signed off by both parties.

The supporting evidence is documented across the Project Sign-Off and Acceptance Report, the Project Closure Report, the Change Request CR-CloudREV-2025-003, the Case Study, and the Cloud Ops Practice Requirements. We are happy to pull up any specific document."
