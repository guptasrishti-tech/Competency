# Healthtech — Full Auditor Presentation Script
## AWS CloudOps Competency (Cloud Financial Management) Audit
### Mist Avinya Technologies LLP | Estimated Duration: 30–45 Minutes

---

## SECTION 1: OPENING & PARTNER INTRODUCTION [~3 Minutes]

**[SPEAKER NOTES: Start confident. Set the stage for who you are and why this engagement matters.]**

"Good morning/afternoon. Thank you for taking the time to review our submission for the AWS Cloud Operations Competency under the Cloud Financial Management (CFM) specialization.

My name is [Name], and I represent Mist Avinya Technologies LLP. We are an AWS Advanced Tier Partner, headquartered in Noida, Uttar Pradesh. We were founded in 2025 with a singular focus — helping businesses extract maximum value from AWS.

Our team consists of 15+ AWS certified professionals spanning delivery, sales, marketing, and operations. We hold 1 EKS Service Delivery Program certification and 2 Specializations in Pipeline.

For this audit, we are presenting our Healthtech engagement — a FinOps and HIPAA Compliance Transformation for a major AI medical imaging company. This is a private client, so we will refer to them as 'Healthtech' throughout.

Before I deep-dive into Healthtech, let me briefly set the context of our broader CloudOps and FinOps practice, because Healthtech is one of four active client engagements that demonstrate the maturity and repeatability of our methodology."

---

## SECTION 2: PRACTICE OVERVIEW & METHODOLOGY [~4 Minutes]

**[SPEAKER NOTES: Show the auditor this is not a one-off. You have a structured, repeatable practice.]**

"Our AWS practice is built around four core pillars: Security & Compliance, Cloud Migration, Managed Services, and Cost Optimization.

For FinOps specifically, we follow a structured methodology we call the Aquila Cloud Methodology, which is a 3-phase framework: Inform, Optimize, and Operate.

**Phase 1 — Inform:**
- CUR setup and cost visibility
- Tagging strategy and enforcement
- Cost Explorer dashboards
- Showback and chargeback models
- Budget alerts and anomaly detection

**Phase 2 — Optimize:**
- Compute rightsizing
- Savings Plans and Reserved Instances
- Storage optimization (for example, gp2 to gp3 migrations)
- Idle resource cleanup
- Well-Architected FinOps reviews

**Phase 3 — Operate:**
- SCPs and governance guardrails
- Continuous optimization reporting
- FinOps culture and team alignment
- Forecasting and variance analysis
- Monthly business review cadence

This methodology is applied consistently across all our engagements. It is not ad hoc — it is a documented, repeatable framework.

We also establish a Cloud Center of Excellence (CCoE) structure at every client, which includes a cross-functional team of Engineering, Finance, and Ops, a dedicated FinOps owner per business unit, executive sponsorship, and a regular cadence of meetings and reviews.

Our governance standards include mandatory tagging policy enforcement, AWS Organizations and SCP guardrails, budget thresholds per team and project, cost allocation rules, and naming conventions.

For accountability, we implement showback and chargeback reporting, monthly variance reviews with leadership, anomaly alerts with escalation paths, and real-time cost dashboards via QuickSight."

---

## SECTION 3: BRIEF OVERVIEW OF ALL 4 CASE STUDIES [~5 Minutes]

**[SPEAKER NOTES: Give the auditor the landscape. Show breadth across industries. Keep each one tight — 60-90 seconds max.]**

"We have four active client engagements that demonstrate our CloudOps and FinOps capabilities. Let me walk through each briefly before we deep-dive into Healthtech.

---

**Case Study 1: CloudRev — Digital Services**

CloudRev is a digital services platform. Their challenge was limited AWS cost visibility, inconsistent resource tagging, low commitment coverage, and difficulty forecasting during variable demand periods.

We implemented our cross-functional FinOps framework with CUR integrated into Aquila Cloud, mandatory tagging enforcement, compute rightsizing, and Savings Plans adoption.

The AWS services involved were EKS, Lambda, RDS, S3, CloudFront, Athena, Cost Explorer, AWS Budgets, and AWS Organizations.

Results: approximately 30% monthly AWS cost reduction within 120 days, greater than 90% tag compliance, high Savings Plans utilization, and improved forecast accuracy.

---

**Case Study 2: Franciscan — EdTech**

Franciscan is an education technology company. They had limited cost visibility across multiple environments, seasonal traffic spikes causing unpredictable AWS spend, inconsistent tagging, and a need for stronger governance.

We deployed our FinOps framework with CUR via Aquila Cloud, EC2 rightsizing and scheduling, S3 lifecycle policies, and AWS Budgets with anomaly detection.

Services used: EC2, Lambda, DynamoDB, S3, CloudFront, Athena, Cost Explorer, AWS Budgets, and AWS Organizations.

Results: 25 to 30% cost reduction, greater than 95% tag compliance, improved forecast accuracy during seasonal peaks, and centralized governance policies.

---

**Case Study 3: AgriTech — Agriculture Technology (Private)**

AgriTech had high unpredictable data ingestion costs from IoT sensors, always-on EC2 instances that were expensive to maintain, batch processing that meant farmer alerts took hours, and they were unable to calculate cost per acre for their pricing model.

We replaced EC2 with Lambda and Kinesis Data Analytics, implemented AWS IoT Core for real-time sensor ingestion, Timestream plus S3 Glacier for optimized storage, CUR integrated with Aquila Cloud for unit economics, and full infrastructure managed via Terraform IaC.

Services: IoT Core, Kinesis Data Streams, Lambda, Amazon Timestream, S3, Glacier Instant Retrieval, SageMaker, Athena, CUR, AWS Budgets, and Organizations.

Results: 40% reduction in data processing costs, 48% lower cost per acre monitored, 90% faster farmer alerts — from hours to minutes, 55% storage cost reduction, and 100% serverless pipeline achieved.

---

**Case Study 4: Healthtech — Healthcare Technology (Private)**

This is our primary case study for today's audit. I will now go into the full deep-dive."

---

## SECTION 4: HEALTHTECH — CLIENT PROFILE & BUSINESS CONTEXT [~3 Minutes]

**[SPEAKER NOTES: Set the scene. The auditor needs to understand who this client is and why the engagement was necessary.]**

"Healthtech is a major healthcare technology company specializing in AI-powered medical imaging. They provide a mission-critical AI platform that assists radiologists by detecting early-stage anomalies in MRI and CT scans. This is life-saving technology.

Here is the engagement profile:

- Industry: HealthTech and Diagnostics
- Region: ap-south-1
- Engagement Period: August 10, 2025 through September 20, 2025
- Engagement Type: FinOps and HIPAA Compliance Transformation
- Primary Workloads: Amazon SageMaker, Amazon EC2 GPU instances, AWS HealthImaging, Amazon S3, AWS Lambda, AWS KMS
- CloudOps Focus Areas: FinOps, Security, and HIPAA Compliance

Their AI inference engine is globally distributed and requires high-performance GPU compute — specifically NVIDIA A10G processors — along with petabyte-scale storage for medical DICOM imaging data.

Before our engagement, this entire platform was running on a costly and inflexible on-premises data center. That is the starting point."

---

## SECTION 5: HEALTHTECH — CHALLENGES IDENTIFIED [~4 Minutes]

**[SPEAKER NOTES: Be specific. The auditor wants to see that you understood the client's problems deeply before jumping to solutions.]**

"We identified two categories of challenges during our discovery phase: Cost and Scale Management challenges, and Compliance and Operational challenges.

**Cost and Scale Management Challenges:**

First, fixed on-premises costs. The client had high capital expenditure on GPU hardware that sat idle during low-scan volume periods. They were paying for peak capacity 24/7, regardless of actual utilization. There was no elasticity.

Second, opaque unit economics. This was a critical business problem. The client had no ability to calculate the exact cost of a single AI scan. Without this number, they could not create a profitable B2B pricing model for the hospitals they serve. They were essentially pricing blind.

Third, what we called the storage 'Data Swamp.' There was exponential growth in high-resolution medical images — MRIs, CT scans, DICOM files — and storage costs were soaring. There was no lifecycle policy, no tiering strategy. Every image, whether accessed daily or untouched for years, was stored at the same cost tier.

**Compliance and Operational Challenges:**

Fourth, HIPAA gaps. The legacy on-premises environment lacked automated encryption and real-time audit logging. For a company handling Protected Health Information — PHI — this was a significant compliance risk. Manual audit preparation was consuming enormous operational overhead.

Fifth, research bottlenecks. The data science team faced 4 to 6 week delays every time they needed infrastructure for testing new AI models. Provisioning new GPU hardware was a manual, slow process that was stifling innovation and slowing their model-to-market cycle.

Sixth, scalability issues. As Healthtech onboarded new hospital clients, they struggled to scale their inference capacity to meet demand. The on-premises infrastructure simply could not flex.

These six challenges formed the basis of our engagement scope."

---

## SECTION 6: HEALTHTECH — SOLUTION ARCHITECTURE [~8 Minutes]

**[SPEAKER NOTES: This is the core of the audit. Walk through each solution component methodically. Tie every action back to a challenge.]**

"We architected a secure, multi-account AWS Organization dedicated to HIPAA-compliant workloads. The design principle was separation of concerns — we isolated data storage, ML training, and production inference into separate environments. Let me walk through each solution component.

---

**6.1 — Multi-Account Strategy and Security Foundation**

We implemented AWS Organizations with AWS Control Tower, creating a Landing Zone with dedicated Organizational Units for Production — which was HIPAA-gated — and Research/Sandbox environments.

We enforced 100% encryption for all data at rest and in transit using AWS KMS and AWS Secrets Manager. All PHI is processed within a secure VPC, and access is strictly controlled via IAM roles.

We deployed custom Service Control Policies — SCPs — that prevent the launch of non-encrypted resources or unapproved instance types. This is governance-as-code. No human can accidentally spin up a non-compliant resource. The SCP blocks it at the API level.

All actions across the environment are logged via AWS CloudTrail to a central audit account. This gives us a complete, immutable audit trail — which is exactly what HIPAA requires.

We also implemented automated HIPAA guardrails via AWS Config, which continuously scans the environment for compliance deviations.

---

**6.2 — Visibility and Unit Economics (The 'See' Phase)**

This directly addresses the opaque unit economics challenge. We integrated the AWS Cost and Usage Report — CUR — with our Aquila Clouds FinOps Optimizer.

The key deliverable here was establishing a primary KPI: cost per scan analyzed. We achieved this by correlating costs from Amazon SageMaker, EC2 GPU instances, and S3 storage with application-level metadata — specifically, the number of scans processed.

This gave the client, for the first time, a clear line of sight from AWS spend to business output. They could now see that each medical scan costs them exactly X dollars to process. This was transformational for their pricing model.

We built real-time forecasting dashboards and cost-per-scan dashboards that the Finance team and Radiology IT team could access directly.

---

**6.3 — Compute Optimization (The 'Save' Phase)**

We migrated the legacy GPU workloads from on-premises hardware to Amazon SageMaker and Amazon EC2 G5 Instances. The G5 instances are powered by NVIDIA A10G GPUs, which is exactly what their AI inference models require.

Critically, we enabled auto-scaling based on scan volume. So during low-scan periods, the fleet scales down. During peak periods, it scales up. This directly solves the idle GPU problem.

We applied a 3-year Compute Savings Plan to the baseline production inference fleet. This brought their Savings Plans coverage from 0% to 80%, which is a significant commitment optimization.

For R&D workloads — AI model re-training — we implemented Amazon SageMaker Managed Spot Training. This uses spare EC2 capacity at up to 70% discount. Since model training is not time-sensitive and can tolerate interruptions, Spot is the right fit. This delivered a 35% reduction in ML training job costs.

We also used AWS Compute Optimizer to right-size the G5 GPU instances. The initial sizing was over-provisioned. Compute Optimizer's data-driven recommendations allowed us to downsize without impacting scan processing latency.

---

**6.4 — Storage Optimization**

The client had petabytes of DICOM medical images. We migrated all of this to Amazon S3.

We enabled S3 Intelligent-Tiering, which automatically moves less-frequently accessed scans to cheaper storage tiers. A scan from 3 years ago that nobody is accessing does not need to sit in the same tier as yesterday's scan. Intelligent-Tiering handles this automatically based on access patterns.

For active scans that need fast retrieval, we implemented AWS HealthImaging, which is purpose-built for medical imaging workloads.

The result: 60% reduction in storage costs per TB.

---

**6.5 — Automation Pipeline**

We built an event-driven processing pipeline using AWS Lambda. Here is how it works:

1. A DICOM scan is uploaded to a secure Amazon S3 bucket.
2. The S3 upload event triggers an AWS Lambda function.
3. The Lambda function retrieves the image and invokes an Amazon SageMaker inference endpoint.
4. The inference endpoint is backed by GPU-powered EC2 instances in an Auto Scaling Group.
5. The AI analysis results are stored in a separate, encrypted S3 bucket.
6. All actions are logged via AWS CloudTrail to the central audit account.

This is fully automated. No manual intervention. Scans flow in, results flow out, and every step is logged and encrypted.

---

**6.6 — Governance and Accountability (The 'Run' Phase)**

We established a Cloud Center of Excellence — CCoE — involving both the Radiology IT team and the Finance department. This is not just a technical initiative; it is a cross-functional governance body.

Department heads receive weekly automated budget alerts via AWS Budgets. We set up trend-based forecasting models that alert the Finance team if R&D spend exceeds defined thresholds.

We implemented showback and chargeback reporting so each business unit can see their AWS consumption. Monthly variance reviews are conducted with leadership.

We also set up anomaly detection — if there is an unexpected spike in spend, the system flags it immediately and triggers an escalation path.

---

**6.7 — Infrastructure as Code**

All infrastructure for this engagement was deployed and managed via Terraform. This is not click-ops. Every resource — the SageMaker endpoints, the EC2 instances, the S3 buckets, the IAM roles, the SCPs — is defined in code, version-controlled, and deployed through an automated CI/CD pipeline.

This means the environment is fully reproducible, auditable, and drift-free. If someone manually changes a resource, the pipeline detects the drift and can remediate it."

---

## SECTION 7: HEALTHTECH — CFM FRAMEWORK ALIGNMENT [~4 Minutes]

**[SPEAKER NOTES: This is critical for the auditor. Map your work explicitly to the AWS CFM four pillars. Use the exact AWS terminology.]**

"Let me now map our Healthtech engagement explicitly to the four pillars of the AWS Cloud Financial Management framework. This is how we structured the implementation.

**Pillar 1 — See (Visibility):**

We deployed the Aquila Clouds FinOps Optimizer to provide granular visibility into GPU utilization and cost-per-model. The CUR data is ingested, processed, and made available through dashboards that show service-level and tag-level cost breakdowns. Real-time spend tracking and alerts are active. The client can see exactly where every dollar is going, broken down by service, by environment, and by the cost-per-scan KPI.

**Pillar 2 — Save (Optimization):**

We migrated static EC2 GPU instances to Amazon SageMaker with Compute Savings Plans, achieving 80% coverage. We implemented SageMaker Spot Training for R&D. We enabled S3 Intelligent-Tiering for storage. We used Compute Optimizer for GPU rightsizing. Every optimization is data-driven — based on actual utilization metrics, not guesswork.

**Pillar 3 — Plan (Forecasting):**

We established a trend-based forecasting model in AWS Budgets. The Finance team receives proactive alerts if R&D spend or inference costs are trending above thresholds. The Aquila Clouds platform provides spend forecasting that shows both previous spend and projected spend, enabling the client to plan capacity and budget accurately.

**Pillar 4 — Run (Operations):**

We established the CCoE involving Radiology IT and Finance. We implemented SCPs and governance guardrails. We set up continuous optimization reporting, monthly business review cadence, and anomaly detection with mitigation workflows. The operational model is self-sustaining — the client can maintain these savings and governance practices long after our engagement ends."

---

## SECTION 8: HEALTHTECH — FINANCIAL AND OPERATIONAL RESULTS [~3 Minutes]

**[SPEAKER NOTES: Present the numbers clearly. These are the proof points the auditor is looking for.]**

"Here are the measurable outcomes from the Healthtech engagement, comparing the on-premises baseline to the post-optimization state after 120 days on AWS.

**Monthly Infrastructure Spend:** Reduced from $10,000 to $5,500 — a 45% reduction.

**Unit Cost per Scan Analyzed:** Reduced from $10,000 to $4,800 — a 52% reduction. This is the KPI that transformed their business model.

**Savings Plans Coverage:** Went from 0% — they had no commitment instruments — to 80% coverage on the inference fleet.

**Peak ML Training Job Cost:** Reduced from $100 to $65 — a 35% reduction through SageMaker Spot Training.

**Storage Cost per TB:** Reduced from $100 to $40 — a 60% reduction through S3 Intelligent-Tiering.

**R&D Provisioning Time:** Reduced from 4 to 6 weeks to under 1 hour — a 99% improvement. The data science team can now spin up a training environment in minutes instead of waiting weeks for hardware procurement.

**Tagging Compliance:** Achieved 95% tag compliance across the environment.

**HIPAA Compliance:** 100% encryption at rest and in transit for all patient data. All production buckets and databases verified as fully encrypted via AWS KMS.

**Total Annualized Savings:** Over $54,000 realized in the first 120 days, based on the run rate.

These are not projections. These are actual, measured results from the engagement."

---

## SECTION 9: HEALTHTECH — AWS SERVICES DEEP-DIVE [~3 Minutes]

**[SPEAKER NOTES: The auditor may want to understand how each service was used. Be ready to explain the 'why' behind each choice.]**

"Let me walk through the complete set of AWS services used in this engagement and the specific purpose of each:

- **Amazon SageMaker** — Used for both ML model training (with Spot Training for cost savings) and production inference endpoints. This replaced the on-premises GPU hardware.

- **Amazon EC2 G5 Instances** — The GPU compute backbone for the inference fleet. G5 instances use NVIDIA A10G GPUs, which are optimized for ML inference workloads. These are in an Auto Scaling Group.

- **AWS HealthImaging** — Purpose-built service for fast access to active medical scans. Used for the current working set of DICOM images.

- **Amazon S3 with Intelligent-Tiering** — Primary storage for the petabyte-scale DICOM archive. Intelligent-Tiering automatically moves data between access tiers based on usage patterns.

- **AWS Lambda** — The event-driven glue in the architecture. Triggers scan processing when new images are uploaded to S3. Serverless, so there is zero cost when no scans are being processed.

- **AWS KMS** — Encrypts all data at rest across S3 buckets and EBS volumes. This is a HIPAA requirement for PHI.

- **AWS Secrets Manager** — Manages credentials and secrets securely. No hardcoded credentials anywhere in the environment.

- **AWS CloudTrail** — Provides the complete audit trail. Every API call across the environment is logged to a central audit account. This is essential for HIPAA audit readiness.

- **Amazon Athena** — Used to query the CUR data stored in S3. Powers the cost analysis and unit economics calculations.

- **AWS Cost and Usage Report (CUR)** — The foundational data source for all FinOps visibility. Exported to S3 and analyzed via Athena and Aquila Clouds.

- **AWS Budgets** — Configured with budget thresholds, anomaly detection, and automated alerts to the Finance team.

- **AWS Organizations** — Provides the multi-account structure with OUs for Production and Research. SCPs are attached at the OU level.

- **AWS Config** — Continuous compliance monitoring. Detects any resource that deviates from the HIPAA baseline.

- **AWS Compute Optimizer** — Provided the data-driven recommendations for right-sizing the G5 GPU instances.

- **Aquila Clouds FinOps Optimizer** — Our FinOps platform that provides recommendations, governance, forecasting, and the cost-per-scan dashboards."

---

## SECTION 10: PROJECT DELIVERY & HANDOVER [~3 Minutes]

**[SPEAKER NOTES: Show the auditor that this was a properly managed engagement with formal deliverables and sign-off.]**

"Let me speak to the project delivery and closure.

The engagement ran from August 10, 2025 to September 29, 2025. It was formally managed with a Statement of Work, defined deliverables, and a structured handover process.

**Deliverables completed and accepted by the client:**

1. Initial FinOps Dashboards — Aquila Clouds and Cost Explorer dashboards, fully operational.
2. Automated Encryption and Tagging SCPs — deployed and enforced across the Organization.
3. GPU Savings Plans and RI Strategy — 3-year Compute Savings Plans applied, 80% coverage achieved.
4. Operational Runbooks and SOPs — documented and handed over.
5. Training and Knowledge Transfer — technical deep-dive sessions conducted for the Radiology IT and Finance teams covering AWS Budgets, Aquila Clouds reporting, and SageMaker cost tracking.

**Handover specifics:**

- All root credentials and IAM administrative roles for the new AWS Organization were verified and transferred to the Healthtech Head of Business.
- A final HIPAA-readiness scan was performed. All production buckets and databases are verified as 100% encrypted via AWS KMS.
- Formal project implementation concluded on September 29, 2025. Ongoing optimization is supported under the standard advisory agreement.

**Project Sign-Off:**

The project was formally signed off by both parties:
- Client side: Praveen Kumar, Head of New Business at Healthtech — signed September 30, 2025.
- Vendor side: Sohrab Pawar, Director at Mist Avinya Technologies LLP — signed September 30, 2025.

The Project Closure Report and the Project Sign-Off and Acceptance Report are both available as supporting documentation."

---

## SECTION 11: LESSONS LEARNED & RECOMMENDATIONS [~2 Minutes]

**[SPEAKER NOTES: This shows maturity. The auditor wants to see that you reflect and improve.]**

"Two key lessons learned from this engagement:

**Lesson 1 — Compliance as an Enabler:**
Integrating HIPAA guardrails into the CI/CD pipeline initially slowed deployment velocity. However, it ultimately reduced the 'Cost of Quality' by eliminating manual audit preparation time. The upfront investment in automated compliance paid for itself.

**Lesson 2 — GPU Rightsizing:**
The initial sizing for AI inference was over-provisioned. Using AWS Compute Optimizer, we were able to downsize to G5 instances without impacting scan processing latency. The lesson: always validate sizing with real utilization data, not estimates.

**Recommendations we left with the client:**

First, for non-time-sensitive AI model re-training, expand the use of SageMaker Managed Spot Training to further reduce R&D costs by up to 70%.

Second, evaluate moving non-GPU sidecar microservices — API layers and supporting services — to AWS Graviton3 instances for a 20% better price-performance ratio."

---

## SECTION 12: BUSINESS VALUE & PARTNER DIFFERENTIATORS [~2 Minutes]

**[SPEAKER NOTES: Close strong. Tie everything back to business outcomes and what makes Mist Avinya different.]**

"Let me close with the business value this engagement delivered and what differentiates our approach.

**Business Value:**

Market Competitiveness — The 52% reduction in unit cost per scan allowed Healthtech to offer more competitive subscription rates to hospital networks. This directly impacted their revenue model.

Regulatory Confidence — Achieving a fully auditable HIPAA posture on AWS became a key sales differentiator when Healthtech bids for large healthcare contracts. Hospitals want to see compliance before they sign.

Innovation Velocity — The shift from 4-6 weeks to under 1 hour for R&D provisioning allowed the AI team to double their model-to-market speed. They can iterate on AI models faster, which means better diagnostic accuracy for patients.

**What differentiates Mist Avinya:**

Unit Economics Focus — We did not just migrate servers. We built a financial model that ties AWS spend directly to medical scans. The cost-per-scan KPI is a business metric, not just a cloud metric.

Industry Expertise — We have deep understanding of healthcare compliance requirements — HIPAA — and AI/ML infrastructure, specifically NVIDIA GPU optimization on AWS.

Sustainable Governance — The integration of Aquila Clouds ensures the customer can maintain these savings and governance practices long after our engagement ends. This is not a one-time optimization. It is a self-sustaining operating model."

---

## SECTION 13: CLOSING & Q&A [~1 Minute]

**[SPEAKER NOTES: Keep the close brief and confident. Open the floor.]**

"To summarize: the Healthtech engagement demonstrates AWS CloudOps maturity across all dimensions — cost optimization, compliance, operational excellence, and governance. We solved for cost, compliance, and speed simultaneously, enabling a life-saving healthcare platform to scale with a lean, efficient, and fully governed financial model.

The client's CTO said it well: 'Mist Avinya and Aquila Clouds didn't just move us to the cloud — they transformed our business. We can now scale on demand, our costs are directly tied to our revenue, and our compliance posture has never been stronger. This has allowed us to focus on what we do best: saving lives.'

I am happy to take any questions. We also have the full case study document, the Project Closure Report, the Project Sign-Off and Acceptance Report, the Statement of Work, and the Cloud Ops Practice Requirements documentation available for your review."

---

## APPENDIX: ANTICIPATED AUDITOR QUESTIONS & SUGGESTED RESPONSES

**Q: How did you calculate the cost-per-scan KPI?**
A: "We correlated the CUR data — specifically the costs from SageMaker inference endpoints, EC2 G5 compute, and S3 storage — with application-level metadata that tracks the number of scans processed. This correlation is done through Aquila Clouds, which ingests the CUR and overlays it with the scan volume data. The result is a per-scan unit cost that the Finance team can track in real time."

**Q: How do you ensure HIPAA compliance is maintained after the engagement?**
A: "Three mechanisms. First, the SCPs are enforced at the Organization level — they cannot be bypassed without changing the policy itself. Second, AWS Config continuously monitors for compliance deviations and flags any non-compliant resources. Third, CloudTrail provides a complete audit trail. The CCoE we established conducts monthly reviews to ensure ongoing compliance."

**Q: Why Compute Savings Plans instead of Reserved Instances?**
A: "Compute Savings Plans offer more flexibility than RIs. They apply across instance families, sizes, OS, tenancy, and regions. For a workload like AI inference where the instance mix may evolve — for example, moving from G5 to a newer generation — Savings Plans provide the same discount without locking into a specific instance type."

**Q: How do you handle the Spot Training interruptions?**
A: "SageMaker Managed Spot Training handles this natively. When a Spot instance is reclaimed, SageMaker automatically checkpoints the training job. When capacity becomes available again, it resumes from the last checkpoint. There is no data loss and no manual intervention required. This is why Spot is appropriate for training but not for production inference."

**Q: What happens if tagging compliance drops?**
A: "We have both preventative and detective controls. Preventatively, SCPs block the creation of resources without mandatory tags. Detectively, AWS Config rules continuously scan for non-compliant resources. If a resource is found without required tags, an EventBridge rule triggers a Lambda function that either applies a default tag or sends a notification to the resource owner for remediation."

**Q: Can you speak to the IaC implementation?**
A: "The entire Healthtech environment is defined in Terraform. This includes the multi-account Organization structure, the SageMaker endpoints, the EC2 Auto Scaling Groups, the S3 buckets with lifecycle policies, the IAM roles, and the SCPs. All changes go through a Git-based CI/CD pipeline with static analysis, security scanning via tools like tfsec and checkov, and validation before deployment. No manual changes are permitted."

**Q: What is the ongoing engagement model?**
A: "The formal project implementation concluded on September 29, 2025. All deliverables, credentials, and operational runbooks were handed over. Ongoing optimization is supported under a standard advisory agreement. The CCoE the client now runs internally ensures continuity."

---

*End of Script*
