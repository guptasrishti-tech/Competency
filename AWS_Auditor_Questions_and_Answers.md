# AWS CloudOps Competency Audit — Auditor Questions & Prepared Answers
## Mist Avinya Technologies LLP | All 4 Customer References
### Prepared for Internal Team — Audit Readiness

---

## CATEGORY 1: PARTNER PRACTICE & METHODOLOGY

---

**Q1: Can you walk me through your FinOps methodology? Is it documented and repeatable, or was it built ad hoc for these engagements?**

A: Our methodology is called the Aquila Cloud Methodology. It is a documented, repeatable 3-phase framework applied consistently across all four engagements:
- Phase 1 — Inform: CUR setup, cost visibility, tagging strategy, Cost Explorer dashboards, showback/chargeback models, budget alerts and anomaly detection.
- Phase 2 — Optimize: Compute rightsizing, Savings Plans and RIs, storage optimization (e.g., gp2→gp3), idle resource cleanup, Well-Architected FinOps reviews.
- Phase 3 — Operate: SCPs and governance guardrails, continuous optimization reporting, FinOps culture and team alignment, forecasting and variance analysis, monthly business review cadence.

This is not ad hoc. It is the same framework applied to CloudRev, Franciscan, AgriTech, and Healthtech — adapted to each client's industry context but structurally identical.

---

**Q2: How do you establish a Cloud Center of Excellence (CCoE) at your clients? What does that look like in practice?**

A: We establish a CCoE as a cross-functional governance body at every engagement. The structure includes:
- A cross-functional team spanning Engineering, Finance, and Operations.
- A dedicated FinOps owner per business unit who is accountable for cost governance.
- Executive sponsorship at the leadership level to ensure organizational buy-in.
- A regular cadence of meetings and reviews — typically weekly budget reviews and monthly business reviews.

For Healthtech specifically, the CCoE involved the Radiology IT team and the Finance department. Department heads receive weekly automated budget alerts via AWS Budgets, and monthly variance reviews are conducted with leadership.

---

**Q3: You were founded in 2025 and already have four client engagements. How do you ensure quality with a relatively new practice?**

A: Three factors. First, our team of 15+ professionals are all AWS certified — this is not a team learning on the job. We hold an EKS Service Delivery Program certification and 2 Specializations in Pipeline. Second, we achieved AWS Advanced Tier Partner status within months of founding, which reflects the technical depth of the team. Third, our methodology is documented and repeatable — the Aquila Cloud Methodology, the multi-account strategy playbook, the tagging governance guide, the IaC methodology — these are all formalized artifacts, not tribal knowledge.

---

**Q4: What is Aquila Clouds? Is it a third-party tool or your own platform?**

A: Aquila Clouds is our FinOps platform — the Aquila Clouds FinOps Optimizer. It integrates with the AWS Cost and Usage Report (CUR) and provides capabilities including cost visibility dashboards, unit economics calculation, optimization recommendations, anomaly detection, forecasting, tag compliance monitoring, and governance enforcement. It is the visualization and intelligence layer that sits on top of AWS-native cost data. We use it across all four engagements.

---

## CATEGORY 2: CASE STUDY — HEALTHTECH (PRIMARY)

---

**Q5: Tell me about the Healthtech client. What was the business problem you were solving?**

A: Healthtech is a major AI medical imaging company. They provide a platform that assists radiologists by detecting early-stage anomalies in MRI and CT scans. Their AI inference engine requires high-performance GPU compute (NVIDIA A10G) and petabyte-scale storage for DICOM imaging data.

Before our engagement, they were running entirely on-premises. The six core challenges were:
1. Fixed on-premises GPU costs — high CapEx for hardware that sat idle during low-scan periods.
2. Opaque unit economics — no ability to calculate the cost of a single AI scan, which blocked their B2B pricing model.
3. Storage "Data Swamp" — exponential growth in medical images with no lifecycle or tiering strategy.
4. HIPAA compliance gaps — no automated encryption or real-time audit logging for PHI.
5. Research bottlenecks — 4-6 week delays for provisioning new GPU hardware for AI model testing.
6. Scalability issues — could not scale inference capacity for new hospital clients.

---

**Q6: What was the engagement timeline and who signed off on it?**

A: The engagement ran from August 10, 2025 to September 29, 2025. It was governed by a formal Statement of Work (SOW). The project was signed off on September 30, 2025 by:
- Client side: Praveen Kumar, Head of New Business at Healthtech.
- Vendor side: Sohrab Pawar, Director at Mist Avinya Technologies LLP.

Both the Project Closure Report and the Project Sign-Off & Acceptance Report are available as supporting documentation.

---

**Q7: How did you calculate the "cost per scan analyzed" KPI? Walk me through the data flow.**

A: We correlated three cost streams from the CUR — Amazon SageMaker inference endpoint costs, EC2 G5 GPU compute costs, and S3 storage costs — with application-level metadata that tracks the number of scans processed. The CUR data is exported to an S3 bucket, then ingested by the Aquila Clouds FinOps Optimizer. Aquila Clouds overlays the cost data with the scan volume data to produce a per-scan unit cost. This is displayed on a real-time dashboard accessible to both the Finance team and the Radiology IT team. Amazon Athena is also used to query the CUR data directly for ad hoc analysis.

---

**Q8: You claim 45% reduction in monthly infrastructure spend. What was the baseline and what drove the reduction?**

A: The baseline monthly infrastructure spend was $10,000 on-premises. Post-optimization on AWS, it dropped to $5,500 — a 45% reduction. The key drivers were:
- Compute Savings Plans: Moving from 0% to 80% coverage on the GPU inference fleet via a 3-year Compute Savings Plan.
- GPU rightsizing: AWS Compute Optimizer identified that the initial G5 instance sizing was over-provisioned. We downsized without impacting scan processing latency.
- SageMaker Spot Training: 35% reduction in ML training job costs by using spare EC2 capacity for non-time-sensitive R&D workloads.
- S3 Intelligent-Tiering: 60% reduction in storage costs by automatically moving infrequently accessed DICOM images to cheaper tiers.
- Auto-scaling: The inference fleet now scales based on scan volume instead of running at peak capacity 24/7.

---

**Q9: Why did you choose Compute Savings Plans over Reserved Instances for the GPU fleet?**

A: Compute Savings Plans offer more flexibility than RIs. They apply across instance families, sizes, OS, tenancy, and regions. For an AI inference workload where the instance mix may evolve — for example, migrating from G5 to a newer GPU generation in the future — Savings Plans provide the same discount level without locking into a specific instance type. This was the right choice for a workload that is likely to evolve as GPU technology advances.

---

**Q10: How does the SageMaker Spot Training handle interruptions? Isn't there a risk of losing training progress?**

A: SageMaker Managed Spot Training handles this natively. When a Spot instance is reclaimed by AWS, SageMaker automatically checkpoints the training job — saving the model state and progress. When Spot capacity becomes available again, it resumes from the last checkpoint. There is no data loss and no manual intervention required. This is why Spot is appropriate for training workloads (which are fault-tolerant) but not for production inference (which requires continuous availability).

---

**Q11: Walk me through the HIPAA compliance architecture. How do you ensure PHI is protected?**

A: The HIPAA compliance architecture has five layers:
1. Multi-account isolation: A secure multi-account AWS Organization with dedicated OUs. The Production OU is HIPAA-gated. ML Training, Inference, and PHI Storage run in separate, isolated accounts.
2. Encryption: AWS KMS encrypts all data at rest in S3 buckets and EBS volumes. All data in transit is encrypted via TLS. This is 100% coverage — verified in the final HIPAA-readiness scan.
3. Access control: All PHI is processed within a secure VPC. Access is controlled via IAM roles and AWS Secrets Manager. No hardcoded credentials exist anywhere.
4. Preventive controls: Custom SCPs prevent the launch of non-encrypted resources or unapproved instance types. This is governance-as-code — the SCP blocks non-compliant actions at the API level.
5. Audit trail: AWS CloudTrail logs every API call across the environment to a central audit account. AWS Config continuously monitors for compliance deviations.

---

**Q12: What happens if someone tries to launch a non-encrypted resource in the Healthtech environment?**

A: The SCP blocks it at the API level. We deployed custom Service Control Policies that deny the creation of any resource that does not have encryption enabled. For example, an S3 bucket without default KMS encryption or an EBS volume without encryption will be denied before it is created. This is a preventive control — it does not detect after the fact, it prevents the action from happening in the first place.

---

**Q13: How is CloudTrail protected from deletion or tampering?**

A: CloudTrail logs are delivered to a centralized, dedicated S3 bucket in the Log Archive account with the following protections:
- S3 Versioning enabled — prevents overwriting of log files.
- S3 Object Lock in Governance mode — enforces immutability during the retention period.
- A dedicated IAM role with write-only access — no user or role has delete permissions.
- An explicit Deny policy on s3:DeleteObject and s3:DeleteBucket actions via bucket policy.
- CloudTrail log file integrity validation is enabled to detect any tampering.

---

**Q14: You mention Terraform for IaC. Can you show me evidence of the IaC implementation?**

A: Yes. The entire Healthtech environment is defined in Terraform. This includes the multi-account Organization structure, SageMaker endpoints, EC2 Auto Scaling Groups, S3 buckets with lifecycle policies, IAM roles, and SCPs. The Terraform code demonstrates:
- IaC Tool Usage: Terraform declaratively provisions all core components for repeatable, non-impacting deployment.
- Scalability Design: The Auto Scaling Group is configured for a maximum of 50 GPU instances, ensuring the inference platform scales during peak diagnostic usage.
- Compliance: The S3 resource enforces 100% KMS encryption at rest and mandatory HIPAA-Status tagging.
- Cost Optimization: S3 Intelligent-Tiering is enabled in the Terraform code, directly leading to the 60% storage cost reduction.

All changes go through a Git-based CI/CD pipeline with static analysis, security scanning (tfsec, checkov), and validation before deployment.

---

**Q15: What deliverables were handed over to the client at project closure?**

A: Four core deliverables were completed and accepted:
1. Initial FinOps Dashboards — Aquila Clouds and Cost Explorer dashboards, fully operational.
2. Automated Encryption and Tagging SCPs — deployed and enforced across the Organization.
3. GPU Savings Plans and RI Strategy — 3-year Compute Savings Plans applied, 80% coverage.
4. Operational Runbooks (SOPs) and Training — documented and handed over with technical deep-dive sessions for the Radiology IT and Finance teams.

Additionally, all root credentials and IAM administrative roles were verified and transferred to the Healthtech Head of Business. A final HIPAA-readiness scan confirmed 100% encryption compliance.

---

**Q16: What is the ongoing engagement model after project closure?**

A: Formal project implementation concluded on September 29, 2025. All deliverables, credentials, and operational runbooks were handed over. Ongoing optimization is supported under a standard advisory agreement. The CCoE that the client now runs internally ensures continuity. The Aquila Clouds platform continues to provide real-time visibility and recommendations.

---

## CATEGORY 3: CASE STUDIES — CLOUDREV, FRANCISCAN, AGRITECH

---

**Q17: For CloudRev, how did you achieve ~30% cost reduction in 120 days? What were the primary levers?**

A: CloudRev is a digital services platform running on EKS. The primary levers were:
- Cross-functional FinOps framework with CUR integrated into Aquila Cloud for full cost visibility.
- Mandatory tagging enforcement — tag compliance went from 45% baseline to over 96-98% across 5 mandatory tags.
- Compute rightsizing and migration to AWS Graviton processors, delivering a 34% cost reduction on compute.
- Savings Plans adoption — coverage went from 25% to 85% utilization.
- Unit economics established: cost per 1 million processed transactions became the primary KPI.

---

**Q18: For Franciscan, how did you handle the seasonal traffic spikes that were causing unpredictable spend?**

A: Franciscan is an EdTech platform with predictable seasonal patterns — admissions and exam periods drive traffic spikes. We addressed this through:
- EC2 rightsizing and scheduling — scaling down during school holidays and low-traffic periods. Aurora Serverless with auto_pause ensures the database scales to zero during idle periods.
- S3 lifecycle policies to manage storage costs over time.
- AWS Budgets with anomaly detection to flag unexpected spend spikes immediately.
- Trend-based forecasting using historical data to predict seasonal cost patterns with improved accuracy.
- Tag compliance improved from 58% baseline to 96% across 4 mandatory tags (Owner, Environment, CostCenter, Application).
- The core unit economics KPI was cost per 1,000 daily active students.

---

**Q19: For AgriTech, you claim 100% serverless pipeline. What does that architecture look like?**

A: The AgriTech platform processes data from IoT sensors for precision agriculture. The architecture is:
1. AWS IoT Core receives real-time sensor data from millions of field devices.
2. Amazon Kinesis Data Streams (50 shards) ingests the high-volume data stream.
3. AWS Lambda processes the data — scales from zero to thousands of concurrent executions automatically.
4. Amazon Timestream stores time-series data for analytics.
5. Amazon S3 with Glacier Instant Retrieval handles long-term storage.
6. Amazon SageMaker runs ML models for predictive analytics.

This replaced the legacy always-on EC2 instances. The result: 40% reduction in data processing costs, 48% lower cost per acre monitored, farmer alerts went from 2-4 hours to under 5 minutes, and 55% storage cost reduction. The entire pipeline is managed via Terraform IaC.

---

**Q20: How do you ensure consistency across all four engagements? They are in very different industries.**

A: The methodology is consistent; the implementation is adapted. Every engagement follows the same Aquila Cloud Methodology (Inform, Optimize, Operate), the same multi-account strategy playbook, the same tagging governance framework, and the same IaC approach via Terraform. What changes is the industry-specific context:
- CloudRev: Web3/blockchain unit economics (cost per transaction).
- Franciscan: EdTech seasonal patterns (cost per daily active student).
- AgriTech: IoT/serverless unit economics (cost per acre monitored).
- Healthtech: Healthcare compliance + AI/ML (cost per scan analyzed).

The framework is the same. The KPIs and compliance requirements are adapted per industry.

---

## CATEGORY 4: MULTI-ACCOUNT STRATEGY & GOVERNANCE (COBL-001, COBL-002)

---

**Q21: Describe your multi-account strategy methodology. How do you design the OU structure for a new client?**

A: We follow a documented 4-phase playbook:
- Phase 1 — Discovery and Assessment (Weeks 1-2): Requirements gathering workshops with Security, Infrastructure, Finance, and Application teams. Deliverable: Discovery & Assessment Report.
- Phase 2 — Strategy and Design (Weeks 3-4): OU design mapping the customer's business structure. Deliverable: Target State Architecture & Design Document with OU diagrams, network topology, and governance plan.
- Phase 3 — Implementation and Automation (Weeks 5-7): Deploy AWS Control Tower, configure Account Factory, implement centralized logging and security (GuardDuty, Security Hub). Deliverable: Deployed Landing Zone and automated Account Vending Machine.
- Phase 4 — Handover and Operational Readiness (Week 8): Training, Cost Management dashboards, operational runbooks. Deliverable: Runbooks, as-built diagrams, knowledge transfer.

Our standard OU structure includes: Security OU (Log Archive, Audit), Infrastructure OU (shared services, networking), Workload OUs (Prod, Dev, Test), Sandbox OU (with spending guardrails), and Policy Staging OU (for testing SCPs before promotion).

---

**Q22: How do you handle the account lifecycle — creation, operation, and decommissioning?**

A: We define four lifecycle stages:
1. Request & Approval: Formal request via ITSM with business justification, designated owner, cost center, and target OU.
2. Automated Provisioning: AWS Control Tower Account Factory provisions the account. A Control Tower Lifecycle Event triggers an AWS Step Functions state machine that deploys the "Account Blueprint" CloudFormation template — additional IAM roles, baseline VPCs, security configurations — all automated, no manual intervention.
3. Operation & Governance: Continuous monitoring via Security Hub, GuardDuty, and AWS Config. Updates to the baseline are managed via CloudFormation StackSets rolled out to the fleet.
4. Decommissioning: Account moved to a "Suspended" OU with restrictive SCPs. Data archived to a central immutable S3 bucket. After a 90-day waiting period, the account is formally closed via automated scripts.

---

**Q23: What risks do you identify in multi-account strategies and how do you mitigate them?**

A: Three primary risks:
1. OU design misalignment with business structure — Mitigated by engaging all stakeholders (business, finance, technical) during Phase 1 Discovery.
2. Weak tagging compliance undermining cost allocation — Mitigated by enforcing mandatory tag policies via AWS Organizations and detecting non-compliant resources via AWS Config rules.
3. Security gaps from misconfigured SCPs — Mitigated by testing all SCP changes in a dedicated Policy Staging OU against non-production accounts before promoting to other OUs.

---

## CATEGORY 5: IaC & GitOps (COBL-003)

---

**Q24: Describe your GitOps and IaC methodology. How do you ensure infrastructure changes are safe?**

A: Our IaC methodology follows four phases:
1. Discovery & Scoping: Workshops to identify compute, storage, networking, and compliance requirements. Deliverable: Requirements & Tooling Recommendation Report.
2. Design & Tool Selection: Modular IaC templates, Git branching strategy (GitFlow), CI/CD pipeline design. Deliverable: Target Architecture & CI/CD Pipeline Design Document.
3. Implementation & Pipeline Construction: Write IaC code in modular components. Build CI/CD pipeline with automated checkpoints:
   - Static Analysis (Linting): cfn-lint for CloudFormation, tflint for Terraform.
   - Security Scanning: tfsec, checkov, or CloudFormation Guard for misconfigurations.
   - Validation: Provider-native tools (aws cloudformation validate-template).
4. Deployment & Operations: Changes deployed by merging peer-reviewed pull requests. Monitoring via CI/CD dashboards and CloudTrail. Rollback by reverting the Git commit and re-running the pipeline.

The Git repository is the single source of truth. No manual console changes are permitted.

---

**Q25: How do you handle rollbacks if a Terraform deployment fails?**

A: The primary rollback mechanism is to revert the commit in Git and re-run the pipeline. For CloudFormation, the native rollback feature automatically reverts to the last known good state on failure. For Terraform, we use state locking and plan/apply workflows — a failed apply does not corrupt the state. The CI/CD pipeline is configured to detect failures and halt promotion to production. All deployment history is tracked in Git and CloudTrail.

---

## CATEGORY 6: TAGGING STRATEGY (COBL-004)

---

**Q26: How do you define a tagging strategy for a new client? Walk me through the process.**

A: Two phases:
- Phase 1 — Discovery & Tag Dictionary Workshop: We conduct a workshop with Finance, IT Operations, Security, and Application teams to identify the key dimensions they need to track. The output is a formal Tag Dictionary — a document listing all approved tags, their purpose, allowed values, and which resources they apply to. Example mandatory tags: cost-center, environment, application-id, owner.
- Phase 2 — Strategy & Governance Design: We define which tags are mandatory and design the enforcement controls — both preventive and detective.

---

**Q27: What specific tools and tactics do you use to enforce tagging compliance?**

A: We use a combination of preventive and detective controls:

Preventive (blocking non-compliance):
- AWS Tag Policies in Organizations — define allowed tag keys and permitted values. Non-compliant tag operations are denied.
- SCPs — deny actions like ec2:RunInstances if mandatory tags (cost-center, owner) are missing.
- IAM Policy conditions — force developers to apply tags when creating resources.
- AWS Service Catalog TagOptions — automatically apply tags to all provisioned products.
- CI/CD pipeline checks — CloudFormation Guard or Checkov fails the pipeline if templates are missing required tags.

Detective (finding non-compliance):
- AWS Config managed rules (e.g., required-tags) continuously scan for non-compliant resources.
- AWS Resource Groups & Tag Editor — search for resources missing specific tags across all regions.
- Automated remediation — EventBridge + Lambda: when Config detects a non-compliant resource, Lambda either applies a default tag or notifies the owner.

---

## CATEGORY 7: CLOUD FINANCIAL MANAGEMENT (COCFM-001 to COCFM-006)

---

**Q28: How do you enable cost allocation measurement and accountability for your clients?**

A: We enable cost allocation through four capabilities:
- Filtering and grouping of all AWS costs by organization, account, resource type, tag, cost category, and region — powered by the CUR integrated with Aquila Clouds.
- Unit economics creation — for each client we define a business-relevant unit cost KPI (cost per transaction for CloudRev, cost per 1,000 DAU for Franciscan, cost per acre for AgriTech, cost per scan for Healthtech).
- Shared service cost allocation — shared costs (networking, security accounts) are automatically allocated back to consuming teams based on predefined rules or consumption metrics.
- Granular CUR integration — the CUR is the foundational data source, ingested and processed daily with automated aggregation.

---

**Q29: How do you handle cost planning and forecasting before a project begins?**

A: We follow a structured 3-step pre-engagement modeling process:
1. Baseline Establishment: Analyze the CUR and Cost Explorer (or on-premises usage data for migrations) to establish the initial spend and volatility baseline.
2. Scenario Modeling: Use Aquila Clouds FinOps Optimizer to model optimization scenarios — e.g., the financial impact of Savings Plans, rightsizing, storage tiering, or architecture changes.
3. Target Setting: Define clear, time-bound financial targets tied directly to business outcomes.

For Healthtech specifically, we modeled the TCO of the on-premises GPU environment, then simulated scenarios for AWS HealthImaging storage, SageMaker Spot Training, and Savings Plans for the inference fleet. The targets focused on compliance, efficiency, and R&D speed — aligned with the client's growth strategy.

---

**Q30: Show me how cost reporting and visualization works. What does the dashboard look like?**

A: The primary visualization is the Aquila Clouds FinOps Cost & Unit Economics Dashboard. It ingests CUR data filtered by mandatory FinOps tags. Key components:
- Primary Cost Grouping: Spend broken down by mandatory tags (CostCenter, Owner, Application).
- Unit Economics widget: Trend line showing the unit cost KPI over time (e.g., cost per scan for Healthtech).
- Tag Compliance gauge: Real-time tag compliance score — for Healthtech this reached 95%.
- Shared Services Allocation: Shared costs automatically allocated back to consuming teams.
- Anomaly Detection: Flags unexpected spend spikes with mitigation status.

The dashboard supports both single-account and multi-account consolidated billing views.

---

**Q31: What resource optimization recommendations did you make for Healthtech?**

A: The key optimization recommendations executed were:
- EC2 G5 GPU rightsizing via AWS Compute Optimizer — downsized over-provisioned instances without impacting latency.
- 3-year Compute Savings Plans for the baseline inference fleet — coverage went from 0% to 80%.
- SageMaker Managed Spot Training for R&D — 35% reduction in training costs.
- S3 Intelligent-Tiering for DICOM archives — 60% storage cost reduction.
- AWS HealthImaging for active scan access — purpose-built for medical imaging.
- Event-driven Lambda pipeline replacing always-on compute — zero cost when no scans are processing.

Recommendations left for future execution: expand Spot Training usage for up to 70% R&D savings, and migrate non-GPU sidecar microservices to AWS Graviton3 for 20% better price-performance.

---

**Q32: How do you handle cost anomaly detection? What happens when an anomaly is detected?**

A: Anomaly detection is configured through AWS Budgets and the Aquila Clouds platform. When an anomaly is detected:
1. The system flags the anomaly with a detection status and identifies the root cause (e.g., unexpected spike in a specific service or account).
2. Alerts are sent via email and at least one additional channel (webhook to ITSM or chat application).
3. A mitigation workflow is triggered — either automated remediation or escalation to the responsible team.
4. The anomaly and its mitigation status are tracked on the dashboard for audit purposes.

Budget overrun alerts are configured with thresholds per team and project, with proactive monitoring comparing forecasted spend to actual spend.

---

**Q33: What training do you provide to the customer on Cloud Financial Management?**

A: Each engagement includes a structured CFM training plan. For Healthtech, the training focused on:
- Secure FinOps and ML Cost Optimization
- HIPAA-compliant cost allocation methodology
- SageMaker Spot Training usage and cost tracking
- S3 Intelligent-Tiering management
- Cost-per-scan KPI interpretation and decision-making
- AWS Budgets configuration and alert management
- Aquila Clouds dashboard navigation and reporting

Training is delivered to both technical teams (Radiology IT) and business teams (Finance). The goal is to embed cost intelligence into operational decisions so the client is self-sufficient after handover.

---

## CATEGORY 8: ACCOUNT SECURITY (ACCT-001, ACCT-002)

---

**Q34: Describe your SOP for secure AWS account creation. How do you handle root account security?**

A: Our SOP covers four mandatory criteria:
1. Root Account Usage: Root user is never used for workload or day-to-day activities. Access is limited to essential account-level configurations only (billing setup, MFA, support plan changes). An SCP at the Organization level denies all actions from root except explicitly allowed tasks. All daily operations use IAM Identity Center.
2. MFA on Root: Enabled immediately after account creation using a virtual MFA device. Periodic validation every 90 days. Monitored via AWS Config rule "root-account-mfa-enabled."
3. Contact Information: Root email set to a shared corporate email alias monitored by the Cloud Ops team. Phone number set to a corporate admin number with OTP service. Alternate contacts configured for Billing, Operations, and Security.
4. CloudTrail Protection: Enabled in all regions including future regions. Logs delivered to a centralized S3 bucket with versioning, S3 Object Lock (Governance mode), write-only IAM role, explicit deny on delete actions, and log file integrity validation.

This SOP is documented and applied to all four client engagements.

---

**Q35: How do your team members access customer AWS accounts? What is your IAM strategy?**

A: Our IAM SOP covers two criteria:

Standard Access Approach:
- Console access: All access routed through AWS IAM Identity Center with SAML 2.0 federation against the customer's enterprise IdP. No long-lived console passwords. Sessions are time-bound (max 8 hours).
- Programmatic access: All CLI/SDK access uses temporary credentials via AWS STS AssumeRole. No long-lived IAM access keys are created. Engineers use "aws sso login" for short-lived tokens. CI/CD pipelines use OIDC federation — no static secrets.
- Cross-account access: Role assumption with session limits (1 hour for privileged, 4 hours for read-only). Trust policies require MFA and source IP conditions.

IAM Best Practices:
- Least privilege: No wildcards in Action/Resource elements. Policies scoped to specific ARNs with tag-based conditions. IAM Access Analyzer enabled Organization-wide with weekly reviews.
- Dedicated credentials: Every individual uses dedicated, individually assigned credentials. No shared accounts or generic logins. All activity traceable via CloudTrail principal ID.
- Permission Sets: Standardized sets (ReadOnlyAdmin, PowerUser-Dev, DeploymentRole, SecurityAuditor) assigned per OU.

---

## CATEGORY 9: PROJECT DELIVERY & CUSTOMER SATISFACTION

---

**Q36: How do you scope and define project deliverables? Do you have standard SOW templates?**

A: Yes. We have standard Statement of Work templates for Cloud Operations projects that are customized to each client's needs. The Healthtech SOW was dated August 10, 2025 and defined the engagement scope, deliverables, timeline, and acceptance criteria. The SOW details the implementation of a secure, auditable AWS Landing Zone with key deliverables including the OU Design Document, IaC-enforced SCP Guardrails, dedicated Security/Log Archive Accounts, and FinOps dashboards.

---

**Q37: How do you assign project management to engagements?**

A: A Project Manager is assigned to each engagement. For Healthtech, Shikha Rai served as the Mist Project Manager. The PM ensures the project remains on time and within budget, manages the deliverable checklist, coordinates with client stakeholders, and oversees the formal handover process.

---

**Q38: What is your customer acceptance process?**

A: We have a formal acceptance process. For Healthtech:
- A deliverable checklist was maintained with four categories: FinOps Dashboards, Automated SCPs, Savings Plans Strategy, and Operational Runbooks/Training. Each was marked as Completed and signed by the Healthtech stakeholder.
- A final HIPAA-readiness scan was performed to verify 100% encryption compliance.
- Knowledge transfer sessions were conducted for the Radiology IT and Finance teams.
- The Project Sign-Off & Acceptance Report was signed by both parties on September 30, 2025.

---

**Q39: What lessons did you learn from the Healthtech engagement?**

A: Two key lessons:
1. Compliance as an Enabler: Integrating HIPAA guardrails into the CI/CD pipeline initially slowed deployment velocity. However, it ultimately reduced the "Cost of Quality" by eliminating manual audit preparation time. The upfront investment in automated compliance paid for itself.
2. GPU Rightsizing: Initial sizing for AI inference was over-provisioned. Using AWS Compute Optimizer, we downsized to G5 instances without impacting scan processing latency. The lesson: always validate sizing with real utilization data, not estimates.

---

## CATEGORY 10: ARCHITECTURE & TECHNICAL DEEP-DIVE

---

**Q40: How does the Healthtech architecture handle high availability and scaling?**

A: Three layers of resilience and scaling:
- SageMaker Endpoints: Deployed with instances in multiple Availability Zones. If an AZ becomes unavailable, SageMaker routes inference requests to healthy instances in other AZs automatically.
- Core Services (S3, Lambda, KMS): Inherently highly available and multi-AZ by default. No extra configuration needed.
- SageMaker Auto Scaling: The inference endpoint has an Auto Scaling policy monitoring invocation rate. It adds GPU instances during peak hours and removes them during quiet periods.
- S3: Scales automatically to accommodate any volume of incoming DICOM data.
- Lambda: Scales from zero to thousands of concurrent executions based on upload volume.

---

**Q41: How do you handle data encryption for Healthtech given HIPAA requirements?**

A: Encryption is enforced at every layer:
- At rest: AWS KMS encrypts all data in S3 buckets and EBS volumes. The SCP prevents creation of any unencrypted resource.
- In transit: All endpoints use TLS encryption. All internal service communication within the VPC uses encrypted channels.
- Key management: AWS KMS is the sole key management solution. Keys are managed centrally with rotation policies.
- Secrets: AWS Secrets Manager handles all credentials — no hardcoded secrets anywhere.
- Audit: AWS CloudTrail logs all KMS key usage and access events to the central audit account.

The final HIPAA-readiness scan verified 100% encryption compliance across all production buckets and databases.

---

**Q42: How do you handle network security in the Healthtech environment?**

A: Network security follows AWS best practices:
- Security Groups restrict traffic between the internet and VPC, and within the VPC itself — only necessary ports and protocols are allowed.
- Network ACLs provide stateless filtering at the subnet level for inbound and outbound traffic.
- All PHI processing occurs within private subnets with no direct internet access.
- VPC Endpoints are used for AWS service access (S3, SageMaker, KMS) to keep traffic within the AWS network.
- AWS WAF and GuardDuty provide enhanced security monitoring and threat detection.

---

**Q43: What is your disaster recovery approach for Healthtech?**

A: The architecture is inherently resilient due to its use of managed, multi-AZ services:
- SageMaker endpoints span multiple AZs with automatic failover.
- S3 provides 99.999999999% durability with cross-AZ replication by default.
- Lambda is a regional service with built-in multi-AZ availability.
- CloudTrail logs are stored in a versioned, Object Lock-protected S3 bucket — immutable and durable.

For the inference pipeline, if an AZ fails, SageMaker automatically routes to healthy instances. For data, S3's native durability ensures no data loss. The RTO target is under 4 hours and RPO is under 1 hour for critical workloads, achieved through the managed service architecture and automated scaling.

---

**Q44: You mention the engagement period was August to September 2025 — roughly 6-7 weeks. Is that realistic for a migration of this complexity?**

A: Yes, and here is why. This was not a lift-and-shift of a massive legacy estate. The scope was focused: migrate the AI inference workload and DICOM storage to AWS, establish the FinOps governance framework, and implement HIPAA compliance controls. The team of 15+ certified professionals worked in parallel streams — compute migration, storage migration, compliance setup, and FinOps implementation. The use of Terraform IaC and AWS Control Tower accelerated the deployment significantly. The 120-day results metric refers to the measurement period after the initial deployment, not the implementation timeline alone.

---

**Q45: If I wanted to verify these results independently, what evidence could you provide?**

A: We can provide:
- The AWS Cost and Usage Report (CUR) data showing before and after spend.
- Aquila Clouds dashboard screenshots showing cost trends, unit economics, and tag compliance.
- AWS Budgets configuration showing thresholds and alert history.
- AWS Config compliance reports showing encryption and tagging status.
- CloudTrail logs demonstrating the audit trail.
- The Terraform codebase showing the IaC implementation.
- The Project Closure Report with KPI metrics signed by both parties.
- The Project Sign-Off & Acceptance Report signed by Praveen Kumar (Healthtech) and Sohrab Pawar (Mist Avinya).
- The Statement of Work defining the engagement scope and deliverables.

---

*This document contains 45 anticipated auditor questions with prepared answers, covering all audit categories: Partner Practice, Case Studies (all 4 clients), Multi-Account Strategy, IaC/GitOps, Tagging, Cloud Financial Management, Account Security, Project Delivery, and Architecture. All answers are based on actual engagement documentation — no assumptions.*
