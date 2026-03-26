# Agritech Case Study — Full Audit Presentation Script
## AWS Cloud Operations Competency (Cloud Financial Management)
### Mist Avinya Technologies LLP | March 2026
### Estimated Duration: 30–45 Minutes

---

## SECTION 1: OPENING & ENGAGEMENT CONTEXT [~3 minutes]

**[Speaker Notes]**

"Good morning/afternoon. Thank you for your time today. My name is [Presenter Name] from Mist Avinya Technologies LLP. I'll be walking you through our Agritech customer engagement in detail — this is our third customer reference, referred to as KiwiKisan in our documentation, submitted under the Cloud Financial Management category of the AWS Cloud Operations Competency.

This engagement is a FinOps and IoT Platform Transformation project. It ran from September 8 to October 10, 2025 — approximately five weeks. The client is a private Agritech company operating in the Precision Agriculture and IoT space.

Before I go into the details, let me set the business context. This client operates a precision agriculture platform that uses thousands of in-field IoT sensors to monitor soil moisture and nutrient levels across farmland. Their platform's core value proposition is delivering real-time, actionable alerts to farmers — telling them when to irrigate, when to fertilize, and when to hold off. The accuracy and speed of these alerts directly impacts crop yield and farmer retention.

The reason they came to us was straightforward: their legacy architecture was too expensive, too slow, and too opaque for them to scale. They could not calculate what it cost them to monitor a single acre of farmland, which meant they could not price their subscriptions competitively for new markets. That is the business problem we solved."

---

## SECTION 2: BUSINESS CHALLENGES — THE 'WHY' [~5 minutes]

**[Speaker Notes]**

"Let me walk through the four specific business challenges we identified during our Discovery phase. These were documented in our Statement of Work and validated in the Project Closure Report.

**Challenge 1: High and Volatile Compute Costs**

The client's entire data processing pipeline was running on legacy EC2 instances — always-on, 24/7. These instances were provisioned for peak load but sat idle during off-peak hours. The result was high, volatile monthly AWS bills with no correlation between spend and actual sensor data volume. They were paying the same whether 100 sensors or 10,000 sensors were transmitting data. This is the classic fixed-cost trap for IoT workloads.

**Challenge 2: Data Processing Latency of 2–4 Hours**

Because the legacy pipeline used batch processing on EC2, farmer-facing alerts were delayed by 2 to 4 hours. In agriculture, that delay is critical. If a soil moisture reading drops below threshold at 6 AM and the farmer gets the alert at 10 AM, the window for effective irrigation may have already passed. This latency was directly impacting the platform's value to end users and was a key driver of churn.

**Challenge 3: Opaque Unit Economics — No 'Cost per Acre' Visibility**

This was the core FinOps challenge. The client had no way to calculate their unit cost — specifically, the cost to monitor one acre of farmland. Without this metric, their finance team could not build a sustainable pricing model. They were essentially guessing at subscription pricing for new markets. This is a textbook case of why unit economics matter in cloud financial management.

**Challenge 4: Operational Burden — 2 FTEs Dedicated to Infrastructure**

Managing the legacy EC2 fleet, handling patching, scaling, and troubleshooting required two full-time engineers. These were engineers who could have been building features and improving the platform, but were instead managing infrastructure. This operational overhead was a direct drag on the company's ability to innovate."

---

## SECTION 3: OUR METHODOLOGY — THE AWS CFM 4-PILLAR FRAMEWORK [~7 minutes]

**[Speaker Notes]**

"Now let me explain how we approached the solution. We implemented our FinOps and CloudOps program structured around the four pillars of the AWS Cloud Financial Management framework: See, Save, Plan, and Run. This is our standard methodology, documented in our FinOps Operating Model Playbook, and it maps directly to the COCFM requirements in the checklist.

**Pillar 1: SEE (Measurement) — Establishing Unit Economics**

The first thing we did was establish visibility. We configured AWS Cost Categories to group costs by two business-relevant dimensions: Farm Region and Sensor Type. This allowed us to break down the AWS bill not by service, but by business function.

We then set up the AWS Cost and Usage Report — the CUR — and integrated it into our Aquila Cloud FinOps platform. This gave us the raw data we needed to calculate the unit metric that mattered most to this client: Cost per Acre Monitored.

We deployed Amazon QuickSight dashboards to visualize this unit economics metric in real time. The engineering team and the finance team could now see, on a single dashboard, exactly how much it costs to monitor each acre, broken down by pipeline stage — ingestion, processing, analytics, and storage.

This directly addresses COCFM-001 (Cost Allocation Measurement and Accountability) and COCFM-003 (Cost Reporting and Visualization). The CUR integration, the cost categories, the tag-based allocation, and the QuickSight dashboards are all documented evidence for those controls.

**Pillar 2: SAVE (Optimization) — Serverless Migration and Storage Tiering**

This is where the architectural transformation happened. We migrated the entire legacy EC2-based data pipeline to a 100% serverless architecture. Let me be specific about what we replaced and why:

- EC2 instances for data processing were replaced with AWS Lambda. Lambda is event-driven — the client now only pays when sensor data is actually being ingested and processed. When no data is flowing, the cost is zero. This eliminated the idle compute problem entirely.

- For data ingestion, we implemented AWS IoT Core for secure device connectivity and Amazon Kinesis Data Streams for real-time streaming. This replaced the old polling-based approach that was both slow and expensive.

- For storage, we implemented Amazon Timestream for time-series sensor data with automated magnetic tiering. Historical soil data that was no longer actively queried was automatically moved to Amazon S3 Glacier Instant Retrieval. This gave us a 55% reduction in storage costs.

- We also migrated SageMaker training jobs to SageMaker Savings Plans, which brought commitment coverage from 0% to 75% for core analytics compute.

- We implemented S3 Lifecycle Policies to automate data movement between storage tiers based on access patterns.

This pillar directly addresses COCFM-004 (Optimize Cloud Costs Through Resource Optimization) and COCFM-005 (Optimize Cloud Costs Through Purchase Option Optimization).

**Pillar 3: PLAN (Forecasting) — 12-Month Spend Forecast**

Agriculture is seasonal. The client experiences peak sensor activity during planting and harvest seasons, and lower activity during off-seasons. We established a 12-month spend forecast in AWS Budgets with automated threshold alerts set at 5% variance. This means if actual spend deviates from forecast by more than 5%, the finance and engineering teams are immediately notified.

We used demand-driver-based forecasting tied to the unit economics model — specifically, forecasting spend based on projected acres monitored and sensor deployment schedules. This is not just linear regression on historical spend; it is business-metric-driven forecasting.

This addresses COCFM-002 (Planning and Forecasting) — including demand-driver-based forecasting, predictive cost estimates, and historical cost trend analysis.

**Pillar 4: RUN (Operations) — Continuous FinOps Governance**

Finally, we operationalized the FinOps practice. We deployed Aquila Cloud's FinOps Optimizer for automated anomaly detection — if there is a sudden spike in Lambda invocations or Kinesis throughput that does not correlate with expected sensor activity, the system flags it immediately.

We formalized a monthly 'Cost-to-Yield' review cadence between the Finance and Engineering teams. This is a recurring business review where the teams examine the unit cost trend, review any anomalies, and make optimization decisions together.

This addresses COCFM-006 (Financial Operations) — including anomaly detection, proactive monitoring, budget alerts, consolidated reporting, and FinOps training."

---

## SECTION 4: TECHNICAL ARCHITECTURE — MANAGEMENT & GOVERNANCE [~7 minutes]

**[Speaker Notes]**

"Let me now walk through the technical architecture in detail. This is relevant to multiple checklist requirements — EXBL-001 (Multi-Account Structure), EXBL-002 (Infrastructure-as-Code), and DOC-001 (Architecture with Scalability and High Availability).

**Multi-Account Strategy (EXBL-001)**

We organized the client's environment using a multi-account AWS Organizations setup. The accounts are grouped into separate Organizational Units:

- A Production OU for the live IoT data pipeline and farmer-facing services.
- A Data/IoT Processing OU for the ingestion and analytics workloads, keeping them isolated from the production application layer.
- A Shared Services OU for centralized logging, networking, and security tooling.
- A dedicated FinOps account that consolidates CUR-based cost reporting, budgeting, and Aquila Cloud integration.

This structure ensures controlled blast radius — if something goes wrong in the analytics pipeline, it does not affect the farmer-facing application. It also ensures clear cost attribution, because each OU and account maps to a specific business function. Governance is centralized through OU-level controls, including mandatory tagging policies, spend guardrails via SCPs, anomaly alerts, and CUR-based cost visibility.

This aligns with the AWS Well-Architected Framework and the AWS Multi-Account Strategy whitepaper. The structure supports high-volume sensor growth, strong operational boundaries, and accurate unit economics.

**Infrastructure-as-Code with Terraform (EXBL-002)**

The entire infrastructure was deployed and managed using Terraform. No changes were made through the AWS Management Console. Let me highlight the key aspects of our IaC implementation:

- Terraform defines the entire serverless data pipeline — Kinesis streams, Lambda functions, Timestream databases, S3 buckets, IAM roles, and all governance policies.
- The Kinesis shard_count is set to 50, which allows the platform to scale instantly to handle data from millions of IoT sensors in the field. This is the scalability design element.
- The IaC provisions a 100% serverless architecture, which directly implements the cost optimization recommendation — eliminating compute waste and achieving the 40% reduction in data processing costs.
- The code enforces 4 mandatory FinOps tags on all resources: Owner, Environment, Application, and CostCenter. This ensures accurate unit economics calculation — every resource is tagged so its cost can be attributed to the correct pipeline stage and business function.

Our GitOps workflow follows a four-phase approach documented in our IaC Governance Guide (COBL-003): Discovery and Scoping, Design and Tool Selection, Implementation and Pipeline Construction, and Deployment and Operations. All changes go through peer-reviewed pull requests, automated validation using tflint and tfsec for security scanning, and are deployed via CI/CD pipeline using GitHub Actions.

**Serverless Architecture — Layer by Layer**

Let me walk through each layer of the architecture:

*Ingestion Layer:* AWS IoT Core handles secure device connectivity from thousands of field sensors. Data flows into Amazon Kinesis Data Streams for real-time streaming. This replaced the old manual polling approach. The key point here is that IoT Core scales automatically to handle connections from millions of devices and process billions of messages without pre-provisioning any infrastructure.

*Processing Layer:* AWS Lambda handles all event-driven compute. Lambda functions are triggered by data arriving in the Kinesis stream. The function concurrency scales automatically based on data volume — from zero to thousands of concurrent executions. The client only pays when sensor data is actually being processed. When no data is flowing, the cost is literally zero. This is the single biggest cost optimization in the entire engagement.

*Storage Layer:* Amazon Timestream stores the time-series sensor data with automated magnetic tiering. Active data stays in the memory tier for fast queries. Historical data automatically moves to the magnetic tier, and then to Amazon S3 Glacier Instant Retrieval for long-term retention. This automated tiering is what delivered the 55% storage cost reduction.

*Analytics Layer:* Amazon Athena is used for ad-hoc querying of CUR data and historical sensor data stored in S3. Amazon SageMaker provides ML-based insights for predictive analytics — crop yield predictions, anomaly detection in sensor readings, and so on.

*Governance Layer:* AWS Organizations with Service Control Policies enforce tagging compliance and prevent unauthorized resource provisioning. SCPs are attached at the OU level, so any new account or resource automatically inherits the governance rules.

**High Availability and Resilience (DOC-001)**

The architecture is inherently resilient due to its serverless, event-driven design. There are no EC2 instances to manage, no database failovers to configure. Every service in the pipeline — IoT Core, Kinesis, Lambda, Timestream, S3 — is a managed, regional service that automatically distributes workloads across multiple Availability Zones. If a single AZ becomes unavailable, these services continue to operate from the healthy AZs without any disruption to the data pipeline. This is high availability with minimal operational overhead."

---

## SECTION 5: COST ALLOCATION STRATEGY & TAGGING (EXCFM-002, COCFM-003, COBL-004) [~5 minutes]

**[Speaker Notes]**

"Let me now go deeper into the cost allocation strategy, because this is central to the FinOps story for this engagement.

**Tag Dictionary and Enforcement**

We defined a Tag Dictionary during our Discovery phase workshop with the client's Finance, Engineering, and Operations teams. The mandatory tags for this engagement are:

- **Owner** — identifies the responsible team (e.g., Data-Science, IoT-Engineering)
- **Environment** — separates Prod, Dev, and QA costs
- **Application** — identifies the specific pipeline component (e.g., Ingestion, Processing, Analytics)
- **CostCenter** — maps to the internal finance structure for chargeback

These tags are enforced through multiple layers — this is our defense-in-depth approach to tagging compliance, which addresses COBL-004:

*Preventative Controls:*
- AWS Tag Policies within AWS Organizations define the allowed tag keys and permitted values. Non-compliant tag operations are denied.
- Service Control Policies provide a hard block — for example, denying Lambda function creation if the mandatory tags are not present.
- Terraform code enforces tags at the IaC level — every resource in the Terraform template includes the four mandatory tags. If a developer tries to deploy without tags, the pipeline fails before anything reaches AWS.

*Detective Controls:*
- AWS Config rules continuously scan the environment for resources missing required tags.
- Aquila Cloud provides real-time tag compliance dashboards showing coverage percentage by account and by tag key.

**Cost Allocation Dashboard (EXCFM-003)**

The Aquila Cloud dashboard provides three primary views for this client:

1. **Business Tag View (Application):** Costs are allocated across the key stages of the data pipeline — Ingestion, Processing, Analytics, and Storage. This view is essential because it demonstrates that the most significant cost component has successfully shifted from expensive processing (the old EC2 fleet) to higher-value analytics (SageMaker and Timestream).

2. **Environment Tag View:** Separates costs for Prod, Dev, and QA environments so budgets are tracked accurately and cost-savings efforts can be focused appropriately.

3. **Unit Economics View:** Tracks the trend of Unit Cost per Acre Monitored over time, confirming that the 48% reduction was sustained through operational efficiency, not just a one-time optimization.

The data source for all of this is the AWS Cost and Usage Report, ingested and processed by Aquila Cloud. The dashboard also shows tag compliance metrics — we achieved and maintained high compliance across all four mandatory tags."

---

## SECTION 6: COST PLANNING, FORECASTING & TCO (EXCFM-001, EXCFM-005) [~4 minutes]

**[Speaker Notes]**

"Before we began the implementation, we conducted a structured cost planning exercise. This is documented under EXCFM-001 and EXCFM-005.

**Pre-Implementation Cost Planning**

Our planning methodology followed three steps:

1. **Baseline Establishment:** We analyzed the historical AWS Cost and Usage Report to establish the existing spend baseline — specifically, the high cost of the EC2-based processing pipeline and the unit cost per acre under the old architecture. This gave us the 'before' picture.

2. **Scenario Modeling:** Using the Aquila Cloud FinOps Optimizer, we simulated the financial impact of the serverless migration. We modeled what the costs would look like with Lambda and Kinesis replacing EC2, with Timestream replacing the existing storage approach, and with the acquisition of Amazon SageMaker Savings Plans. We also modeled cost behavior under extreme scaling scenarios — what happens when the client goes from 10,000 sensors to 1 million sensors.

3. **Target Setting:** Based on the modeling, we set high-level cost and efficiency targets tied directly to the business objective of market expansion. The targets were: 40% reduction in processing costs, 45% or greater reduction in unit cost per acre, and 75% Savings Plans coverage within 120 days.

**Total Cost of Ownership Analysis (EXCFM-005)**

The TCO analysis went beyond just AWS infrastructure costs. We factored in:

- The elimination of 1.5 FTEs worth of infrastructure management effort (from 2 FTEs to 0.5 FTEs) — this is a real operational cost saving that does not show up in the AWS bill but is critical to the business case.
- The value of real-time alerting — faster alerts mean better crop yields, which means higher farmer retention and revenue.
- The scalability economics — with the serverless architecture, the client can scale from thousands to millions of sensors without a linear increase in headcount or infrastructure cost.

The engagement focus was FinOps and IoT Platform Transformation. The original platform was costly, slow, and relied on manual infrastructure management. The TCO analysis demonstrated that the transformation would deliver over $140,000 in annualized recurring savings, validated in our Project Closure Report."

---

## SECTION 7: RESULTS — QUANTIFIED KPIs [~5 minutes]

**[Speaker Notes]**

"Now let me present the results. These are validated in our Project Closure Report and our Project Sign-Off and Acceptance Report, both signed by the client stakeholder — Abhinav Ahluwalia, Founder of the Agritech company — on October 12, 2025.

**Financial Results (After 120 Days):**

| KPI | Baseline | After Optimization | Improvement |
|-----|----------|-------------------|-------------|
| Monthly Data Processing Cost | High (EC2-based) | Serverless (Lambda/Kinesis) | **40% reduction** |
| Unit Cost per Acre Monitored | High (unable to calculate previously) | Calculated and optimized | **48% reduction** |
| Savings Plans Coverage | 0% | 75% for core analytics compute | **75% coverage achieved** |
| Farmer Alert Latency | 2–4 hours | Less than 5 minutes | **90% faster** |
| Storage Costs | High (single-tier) | Automated tiering (Timestream → S3 → Glacier) | **55% reduction** |
| Serverless Adoption | 0% | 100% of data pipeline | **100% serverless** |
| Total Annualized Savings | — | — | **~$140,000** |

**Operational Results:**

- Infrastructure management effort reduced from 2 FTEs to 0.5 FTEs.
- Tag compliance maintained at high levels across all four mandatory tags.
- Monthly 'Cost-to-Yield' review cadence established between Finance and Engineering.

**Business Outcomes:**

- **Competitive Advantage:** The 48% reduction in unit cost per acre allowed the client to offer more aggressive subscription pricing in new global markets. This is a direct business outcome — FinOps enabled a go-to-market strategy change.

- **Platform Value:** Real-time alerting — going from hours to minutes — significantly increased farmer retention and improved crop yields. The platform became genuinely valuable to its end users.

- **Scalability:** The serverless foundation allows the client to scale from thousands to millions of sensors without a linear increase in headcount or cost. The architecture scales with the business, not ahead of it.

These results directly map to the EXCFM-006 (Cloud Cost Optimization) requirement — we have documented the recommendations, which ones were executed, and the measured financial impact."

---

## SECTION 8: CUSTOMER ENABLEMENT & TRAINING (EXCFM-004) [~3 minutes]

**[Speaker Notes]**

"Customer enablement was a core part of this engagement. We did not just optimize and leave — we trained the client's teams to operate and continue optimizing independently.

**Training Program:**

Our training focus for this client was 'Serverless FinOps and IoT Efficiency.' The training program covered:

1. **Maintaining the Serverless Data Pipeline** — How the Lambda functions, Kinesis streams, and Timestream databases work together. How to troubleshoot issues. How to deploy changes through the Terraform pipeline.

2. **Interpreting Unit Economics via Aquila Cloud and QuickSight** — How to read the cost-per-acre dashboard. How to drill down by pipeline stage, by environment, by tag. How to identify when a cost anomaly is a real problem versus expected seasonal variation.

3. **Monthly Savings Plans Utilization Reviews** — How to review SP coverage and utilization. When to consider purchasing additional commitments. How to use the Aquila Cloud recommendations engine.

We conducted 3 deep-dive knowledge transfer sessions with the client's engineering and finance teams. These are documented in our Project Sign-Off and Acceptance Report.

**FinOps Culture Shift:**

One of the most impactful outcomes was cultural. Once the engineering team saw the 'Cost per Acre' dashboard, they proactively started optimizing their own Lambda function code to reduce execution time. This is documented in our Project Closure Report as a key lesson learned: 'Unit Metrics Drive Culture.' When engineers can see the direct cost impact of their code, they optimize without being asked. That is the goal of FinOps — embedding cost awareness into engineering decisions."

---

## SECTION 9: GOVERNANCE, SECURITY & OPERATIONAL EXCELLENCE [~3 minutes]

**[Speaker Notes]**

"Let me briefly cover the governance and security aspects, which map to the Common Customer Example Requirements.

**Secure AWS Account Governance (ACCT-001):**

We follow our standard Security Engagement SOP for this client:
- Root account is never used for workload activities — access is limited to essential account-level configurations only.
- MFA is enabled on the root account immediately after creation, with periodic validation every 90 days.
- Root account email is set to a shared corporate email alias monitored by the Cloud/DevOps team.
- CloudTrail is enabled in all AWS regions. Logs are delivered to a centralized S3 bucket with versioning enabled, S3 Object Lock in Governance mode for immutability, and explicit deny policies on delete actions.

**IAM Best Practices (ACCT-002):**

All access to the client's AWS accounts uses temporary IAM roles — no long-lived credentials. IAM principals are granted minimum necessary privileges. Each Mist Avinya team member who accessed the account used dedicated credentials. We leveraged Identity Federation using SAML for enterprise user identities.

**Network Security (NETSEC-001):**

Security Groups restrict traffic at the instance level. Network ACLs provide stateless filtering at the subnet level. AWS WAF and GuardDuty are implemented for enhanced security monitoring and threat detection.

**Data Encryption (NETSEC-002):**

All data in transit is encrypted using TLS. All data at rest in S3, Timestream, and other services is encrypted using native encryption features with AWS KMS for key management.

**Workload Health KPIs (OPE-001):**

We monitor Lambda invocation counts, error rates, and memory usage. Kinesis iterator age and throughput. Timestream query latency. All metrics are aggregated in CloudWatch with dashboards and alerts set for defined thresholds — for example, Lambda error rates exceeding 5% trigger an immediate alert.

**Deployment Readiness (OPE-003):**

All deployments go through our standardized checklist: automated integration tests via GitHub Actions, security scanning via Snyk, and manual validation for critical workloads before production release."

---

## SECTION 10: SUPPORTING EVIDENCE & ARTIFACTS [~2 minutes]

**[Speaker Notes]**

"To summarize the evidence artifacts we have available for this engagement:

1. **Statement of Work (SOW)** — Fully documented project scope, deliverables, and engagement terms. This is the SOW for the Agritech FinOps and Serverless Transformation project.

2. **Project Closure Report** — Documents the executive summary, infrastructure and governance architecture, FinOps implementation across all four pillars, KPI results, lessons learned, and recommendations. Signed off by both parties.

3. **Project Sign-Off and Acceptance Report** — Formal acceptance document confirming all deliverables were reviewed, tested, and handed over. Includes the financial summary of achievement — $140,000 annualized savings, 40% processing cost reduction, 48% unit cost reduction, 75% Savings Plans coverage. Signed by Abhinav Ahluwalia (Founder, Agritech Company) and Sohrab Pawar (Director, Mist Avinya) on October 12, 2025.

4. **Terraform IaC Code** — The actual Terraform templates used to deploy the serverless pipeline, demonstrating EXBL-002 compliance.

5. **Aquila Cloud Dashboards** — Screenshots and live access showing cost allocation, unit economics, tag compliance, anomaly detection, and Savings Plans utilization.

6. **QuickSight Unit Metric Dashboard** — Real-time cost-per-acre visualization.

7. **Standard Operating Procedures (SOPs)** — Documented workflows for monthly cost-to-yield reviews and commitment purchasing.

8. **Training Materials** — The FinOps training plan focused on Serverless FinOps and IoT Efficiency."

---

## SECTION 11: LESSONS LEARNED & FORWARD RECOMMENDATIONS [~2 minutes]

**[Speaker Notes]**

"Before I close, let me share the lessons learned and our forward-looking recommendations, which are documented in the Project Closure Report.

**Lessons Learned:**

1. **Serverless is the Ultimate FinOps Tool for IoT Workloads.** For workloads with variable sensor activity, moving from fixed costs (EC2) to variable costs (Lambda) provided the single biggest impact on the bottom line. The cost now scales exactly with data volume — when sensors are quiet, the bill is near zero.

2. **Unit Metrics Drive Culture.** Once the engineering team had visibility into the 'Cost per Acre' metric, they proactively optimized their Lambda function code to reduce execution time. This is the FinOps cultural shift — engineers making cost-aware decisions without being told to.

**Forward Recommendations:**

1. **Graviton Migration:** Evaluate moving any remaining specialized analytics containers to AWS Graviton3 processors for a further 20% price-performance improvement.

2. **Anomaly Detection Enhancement:** Fully enable AWS Cost Anomaly Detection with integration into the team's Slack channel for immediate notification of sensor 'chatty' behavior — sensors that are transmitting more data than expected, which could indicate a malfunction or a cost anomaly.

3. **Commitment Deepening:** Re-evaluate Savings Plans coverage in Q1 2026 as more sensors are deployed globally. As the baseline grows, there is an opportunity to increase commitment coverage beyond 75%."

---

## SECTION 12: CLOSING & Q&A [~2 minutes]

**[Speaker Notes]**

"To wrap up — this Agritech engagement demonstrates our end-to-end Cloud Financial Management capability:

- We identified the business problem: opaque unit economics and high infrastructure costs.
- We applied the AWS CFM 4-pillar framework: See, Save, Plan, Run.
- We executed a full serverless migration using IaC, with governance enforced through AWS Organizations and SCPs.
- We delivered measurable, validated results: 40% cost reduction, 48% unit cost reduction, 90% faster alerts, $140,000 annualized savings.
- We enabled the customer with training, dashboards, and a continuous FinOps operating model.
- All of this is backed by signed project documentation, Terraform code, and live dashboard evidence.

I am happy to take any questions. I also have the SOW, Closure Report, Sign-Off Report, Terraform code, and dashboard screenshots available if you would like to review any specific artifact in detail.

Thank you."

---

## QUICK REFERENCE: CHECKLIST REQUIREMENT MAPPING

| Checklist ID | Requirement | Where Addressed in This Script |
|---|---|---|
| COBL-001 | Multi-Account Strategy | Section 4 — Multi-Account Strategy |
| COBL-002 | Account Lifecycle | Section 4 — AWS Organizations setup |
| COBL-003 | GitOps / IaC | Section 4 — Terraform and GitOps workflow |
| COBL-004 | Tagging Strategy | Section 5 — Tag Dictionary and Enforcement |
| COCFM-001 | Cost Allocation & Measurement | Section 3 (Pillar 1: See) and Section 5 |
| COCFM-002 | Planning & Forecasting | Section 3 (Pillar 3: Plan) and Section 6 |
| COCFM-003 | Cost Reporting & Visualization | Section 5 — Cost Allocation Dashboard |
| COCFM-004 | Resource Optimization | Section 3 (Pillar 2: Save) |
| COCFM-005 | Purchase Option Optimization | Section 3 (Pillar 2: Save) — Savings Plans |
| COCFM-006 | Financial Operations | Section 3 (Pillar 4: Run) |
| EXBL-001 | Multi-Account Structure (Customer) | Section 4 |
| EXBL-002 | IaC (Customer) | Section 4 — Terraform details |
| EXBL-003 | Case Study Quality | Entire script — narrative with KPIs |
| EXCFM-001 | Cost Planning & Forecasting (Customer) | Section 6 |
| EXCFM-002 | Cost Allocation Strategy (Customer) | Section 5 |
| EXCFM-003 | Cost Allocation Dashboard (Customer) | Section 5 |
| EXCFM-004 | CFM Training (Customer) | Section 8 |
| EXCFM-005 | TCO Analysis (Customer) | Section 6 |
| EXCFM-006 | Cost Optimization (Customer) | Section 7 |
| DOC-001 | Architecture Diagram | Section 4 — Architecture walkthrough |
| ACCT-001 | Secure Account Governance | Section 9 |
| ACCT-002 | IAM Best Practices | Section 9 |
| OPE-001 | Workload Health KPIs | Section 9 |
| OPE-002 | Runbooks | Section 9 |
| OPE-003 | Deployment Readiness | Section 9 |
| NETSEC-001 | VPC / Network Security | Section 9 |
| NETSEC-002 | Data Encryption | Section 9 |
| REL-001 | Automated Deployment / IaC | Section 4 |
| REL-002 | DR / RTO / RPO | Section 4 — HA discussion |
| COST-001 | TCO / Cost Modeling | Section 6 |

---

*Script prepared for AWS Cloud Operations Competency Audit — Mist Avinya Technologies LLP, March 2026*
