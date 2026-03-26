# Healthtech — Statement of Work (SOW) Explanation Script
## AWS CloudOps Competency Audit | Mist Avinya Technologies LLP
### Estimated Duration: 20–30 Minutes

---

## HOW TO USE THIS SCRIPT

This script walks through every section of the Healthtech Statement of Work, explains what was committed, and then explains exactly how it was delivered. Use this when the auditor asks you to walk through the SOW or when you need to demonstrate that every commitment was fulfilled.

---

## SECTION 1: EXECUTIVE SUMMARY WALKTHROUGH [~4 Minutes]

"Let me walk you through the Statement of Work for our Healthtech engagement. This SOW was signed on August 10, 2025 by Praveen Kumar, Head of New Business at Healthtech, and countersigned on September 20, 2025 by Sohrab Pawar, Co-founder of Mist Avinya Technologies LLP.

The engagement is titled 'AWS Cloud Operations — FinOps & Compliance Transformation Program.'

The client is a leading AI-powered medical imaging platform that assists radiologists in detecting early-stage anomalies from MRI and CT scans. They were facing rapid growth in AI workloads, increasing GPU infrastructure costs, and strict healthcare regulatory requirements. These created challenges in three areas: cost visibility, scalability, and compliance.

The SOW defines this engagement as implementing a CloudOps and FinOps transformation program on AWS to optimize infrastructure costs, improve unit economics, and establish a secure, HIPAA-compliant cloud environment.

---

**1.1 — Business Objectives (What We Committed To)**

The SOW defines six specific business objectives. Let me go through each one and explain how we delivered against it.

---

**Objective 1: Reduce AI inference infrastructure costs by at least 40%.**

How we delivered: We migrated the client's on-premises GPU workloads to Amazon SageMaker and Amazon EC2 G5 Instances with auto-scaling based on scan volume. We applied a 3-year Compute Savings Plan to the baseline production inference fleet. We used AWS Compute Optimizer to right-size the G5 instances — the initial sizing was over-provisioned, and Compute Optimizer's utilization data allowed us to downsize without impacting scan processing latency. The result was a 45% reduction in monthly AI inference infrastructure spend — from $10,000 to $5,500. This exceeds the 40% target defined in the SOW.

---

**Objective 2: Establish unit economics framework — cost per scan analyzed.**

How we delivered: We integrated the AWS Cost and Usage Report (CUR) with our Aquila Clouds FinOps Optimizer. We correlated costs from SageMaker inference endpoints, EC2 G5 compute, and S3 storage with application-level metadata — specifically the number of scans processed. This gave the client a clear, real-time 'cost per scan analyzed' KPI. The unit cost dropped from $10,000 to $4,800 — a 52% reduction. This KPI is now tracked on live dashboards accessible to both the Finance team and the Radiology IT team.

---

**Objective 3: Achieve 70–80% Savings Plans coverage for GPU inference workloads.**

How we delivered: The client had zero commitment coverage before our engagement — no Savings Plans, no Reserved Instances. We analyzed the baseline inference workload patterns and applied a 3-year Compute Savings Plan to the production GPU fleet. We achieved 80% Savings Plans coverage, which is at the top end of the 70–80% target defined in the SOW.

---

**Objective 4: Improve cost allocation tagging compliance to 95%+.**

How we delivered: We designed and implemented a mandatory tagging framework with four key tags: Hospital_ID, Application_Name, Environment, and Cost_Center. We enforced this through Service Control Policies (SCPs) that block the creation of resources without mandatory tags. We deployed AWS Config rules to continuously detect non-compliant resources. We achieved 95% tagging compliance across the environment, meeting the SOW target.

---

**Objective 5: Enable predictable cloud cost forecasting within ±5% variance.**

How we delivered: We established a trend-based forecasting model in AWS Budgets. The Aquila Clouds platform provides spend forecasting that shows both previous spend and projected spend. The Finance team receives proactive alerts if actual spend deviates from forecast. By tying costs to scan volume — a predictable business metric — the client now has a usage-based cost model that enables accurate forecasting.

---

**Objective 6: Ensure full encryption and security controls aligned with HIPAA compliance requirements.**

How we delivered: We enforced 100% encryption for all data at rest using AWS KMS across S3 buckets and EBS volumes. All data in transit is encrypted. All Protected Health Information (PHI) is processed within a secure VPC with access controlled via IAM roles and AWS Secrets Manager. We deployed custom SCPs that prevent the launch of non-encrypted resources or unapproved instance types. AWS CloudTrail logs every API call to a central audit account. AWS Config continuously monitors for compliance deviations. A final HIPAA-readiness scan was performed at project closure — all production buckets and databases were verified as 100% encrypted.

---

**1.2 — Expected Outcomes**

The SOW also defined five expected outcomes. Let me confirm each:

First — 'Reduced infrastructure cost per medical scan enabling competitive pricing for healthcare providers.' Delivered. The 52% reduction in cost per scan allowed the client to offer more competitive subscription rates to hospital networks.

Second — 'Improved visibility into AI workload cost drivers across compute, storage, and training environments.' Delivered. The CUR integration with Aquila Clouds provides granular visibility broken down by service, environment, and the cost-per-scan KPI.

Third — 'Faster ML experimentation cycles through scalable cloud infrastructure.' Delivered. R&D provisioning time was reduced from 4–6 weeks to under 1 hour. The data science team can now spin up training environments in minutes using SageMaker.

Fourth — 'Enhanced compliance posture through secure data processing and audit-ready cloud architecture.' Delivered. 100% HIPAA-compliant encryption, CloudTrail audit logging, and automated Config guardrails.

Fifth — 'Establishment of a sustainable FinOps culture for continuous optimization.' Delivered. We established a Cloud Center of Excellence (CCoE) involving Radiology IT and Finance, with weekly budget reviews, monthly variance analysis, and anomaly detection workflows."

---

## SECTION 2: SCOPE OF WORK — METHODOLOGY WALKTHROUGH [~8 Minutes]

"The SOW defines a 4-Phase Playbook Approach as the methodology. Let me walk through each phase and explain exactly what was done.

---

**Phase 1: Discovery**

The SOW committed to three activities in this phase:

Activity 1 — Current State Assessment: Audit of existing infrastructure, GPU workloads, and storage costs for medical imaging datasets.

How we executed: We conducted a thorough audit of the client's on-premises data center. We documented the GPU hardware inventory — NVIDIA A10G processors — their utilization patterns, and the total cost of ownership. We identified that GPU hardware was sitting idle during low-scan volume periods, meaning the client was paying for peak capacity 24/7. We also audited the storage footprint — petabytes of DICOM medical images with no lifecycle or tiering strategy. Every image, regardless of access frequency, was stored at the same cost tier. We quantified the storage growth rate and projected future costs.

Activity 2 — Workload Categorization: Group workloads by AI inference, ML training, and data storage environments.

How we executed: We categorized the client's workloads into three distinct groups. First, production AI inference — the real-time scan analysis that hospitals depend on. This is the mission-critical workload requiring consistent GPU availability. Second, ML training — the R&D workload where data scientists train and retrain AI models. This is compute-intensive but not time-sensitive. Third, data storage — the DICOM image archive that grows continuously. Each category has different cost profiles, different scaling requirements, and different optimization strategies. This categorization directly informed our multi-account architecture design.

Activity 3 — Requirements Gathering: Define security baselines, compliance controls, and cost allocation framework.

How we executed: We worked with the client's stakeholders to define the HIPAA compliance requirements — encryption at rest and in transit, audit logging, access controls for PHI. We defined the cost allocation framework, identifying four mandatory tags: Hospital_ID, Application_Name, Environment, and Cost_Center. We established the security baselines that would be enforced via SCPs and AWS Config.

---

**Phase 2: Governance Blueprint**

The SOW committed to defining an AWS governance structure. Specifically:

- Organizing accounts into Production, Research/ML, and Shared Services OUs
- Implementing Service Control Policies (SCPs) to enforce regional, security, and spending guardrails
- Establishing a standard tagging framework (Hospital_ID, Application_Name, Environment, Cost_Center)

How we executed: We implemented AWS Organizations with AWS Control Tower, creating a Landing Zone with dedicated Organizational Units. The Production OU is HIPAA-gated — meaning SCPs enforce encryption, restrict instance types, and prevent any non-compliant resource creation. The Research/Sandbox OU allows the data science team to experiment with strong spending guardrails.

The SCPs we deployed include: denial of any EC2 instance launch without mandatory tags, denial of any S3 bucket creation without default encryption enabled, restriction to approved instance types only (preventing accidental use of expensive or non-compliant instance families), and regional restrictions to ap-south-1.

The tagging framework was implemented with both preventative controls (SCPs blocking untagged resources) and detective controls (AWS Config rules flagging non-compliant resources). Tag policies within AWS Organizations enforce the allowed tag keys and permitted values.

---

**Phase 3: Deployment**

The SOW committed to: Configure consolidated billing and integrate AWS Cost and Usage Report (CUR) with FinOps analytics platform for cost tracking.

How we executed: We enabled consolidated billing across the AWS Organization. We configured the CUR to export detailed cost and usage data to a dedicated S3 bucket. This CUR data is ingested by the Aquila Clouds FinOps Optimizer, which processes it to provide:

- Service-level and tag-level cost breakdowns
- The cost-per-scan KPI by correlating AWS costs with scan volume metadata
- Real-time spend tracking and alerts
- Trend analysis and forecasting
- Anomaly detection

We also configured Amazon Athena to query the CUR data directly for ad-hoc analysis. The Finance team can run custom queries against the billing data without needing engineering support.

Beyond the visibility layer, the deployment phase also included the core infrastructure buildout:

- Amazon SageMaker endpoints for ML inference and training
- EC2 G5 Auto Scaling Groups for the GPU inference fleet
- S3 buckets with Intelligent-Tiering for DICOM storage
- AWS HealthImaging for active scan access
- The event-driven Lambda pipeline for automated scan processing
- KMS encryption keys, IAM roles, Secrets Manager configuration
- CloudTrail logging to the central audit account

All infrastructure was deployed via Terraform — Infrastructure as Code. Every resource is version-controlled and deployed through an automated CI/CD pipeline.

---

**Phase 4: Operational Readiness**

The SOW committed to two activities:

Activity 1 — Handoff: Deliver operational runbooks for cost monitoring, workload optimization, and compliance reporting.

How we executed: We created and delivered comprehensive operational runbooks covering:
- How to monitor costs using Aquila Clouds dashboards and AWS Cost Explorer
- How to review and act on rightsizing recommendations from AWS Compute Optimizer
- How to manage Savings Plans coverage and evaluate renewal options
- How to respond to budget alerts and anomaly detections
- How to run HIPAA compliance checks using AWS Config
- How to review CloudTrail audit logs
- Standard operating procedures for the CCoE monthly review cadence

Activity 2 — Training: Conduct knowledge transfer sessions for engineering, operations, and finance teams.

How we executed: We conducted technical deep-dive sessions for three audiences:
- The Radiology IT team — focused on SageMaker cost tracking, infrastructure scaling, and compliance monitoring
- The Finance team — focused on AWS Budgets, Aquila Clouds reporting, cost allocation, and the showback/chargeback model
- The Operations team — focused on the operational runbooks, incident response procedures, and the CCoE governance cadence

All training materials and runbooks were formally handed over as part of the project closure."

---

## SECTION 3: PROJECT MANAGEMENT & CHANGE MANAGEMENT [~3 Minutes]

"The SOW defines a governance structure and a change management process. Let me explain both.

---

**Governance Hub**

The SOW specifies weekly sync meetings to review CloudOps maturity and cost optimization progress.

How we executed: Throughout the 6-week engagement, we held weekly governance meetings with the client's technical and business stakeholders. These meetings covered:
- Progress against the project plan milestones
- Cost optimization metrics — tracking the reduction trajectory week over week
- Any blockers or dependencies requiring client action
- Compliance status updates
- Upcoming activities for the next sprint

---

**RACI Matrix**

The SOW defines the responsibility model: Partner (Responsible) for Architecture and FinOps implementation; Customer (Accountable) for Policy approval and commitment purchases.

How we executed: Mist Avinya was responsible for all architecture design, governance implementation, cost optimization execution, and FinOps platform configuration. The client was accountable for approving the governance policies (SCPs, tag policies), authorizing the Compute Savings Plans purchase, and providing environment access. This separation ensured that the client retained control over financial commitments and policy decisions while we executed the technical work.

---

**Change Management Process**

The SOW defines a 3-step workflow for any scope modifications:

Step 1 — Submission: Formal Change Request with technical and business justification.
Step 2 — Impact Analysis: Partner evaluates impact on project scope, cost optimization strategy, and timeline.
Step 3 — Approval: Written authorization from the client's technology leadership before implementation.

How we executed: This process was in place throughout the engagement. Any deviation from the original scope required a formal change request. This ensured operational stability and compliance integrity were maintained. No scope changes were implemented without written client approval."

---
