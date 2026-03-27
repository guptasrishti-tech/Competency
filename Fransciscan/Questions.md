# AWS CloudOps Competency Audit — Complete Auditor Q&A, Tricks & Tactics Guide
## Franciscan Solutions — FinOps Transformation Program
### Mist Avinya Technologies LLP

---

## 📌 HOW TO USE THIS DOCUMENT

This is your complete audit preparation guide. For every question:
- **Question** — What the auditor will ask
- **Why They Ask** — The hidden intent behind the question
- **Answer** — Ready-to-speak response with evidence references
- **🎯 Trick to Tackle** — How to handle follow-ups, traps, and curveballs

> **Golden Rules for the Audit:**
> 1. Never guess. If you don't know, say "I'll get back to you with the exact evidence."
> 2. Always tie answers back to SOW language and signed documents.
> 3. Speak in metrics — numbers beat narratives.
> 4. If the auditor pushes on something you didn't do, own the scope boundary. "That was explicitly out of scope per the SOW."
> 5. Keep answers structured: What → How → Evidence → Result.

---

## CATEGORY 1: ENGAGEMENT & SOW FUNDAMENTALS

---

### Q1. Can you describe the customer and the nature of this engagement?

**Why They Ask:** They want to verify you actually understand the customer's business, not just the tech. Auditors test whether this was a real, deep engagement or a surface-level project.

**Answer:**
Franciscan Solutions is an Education Technology (EdTech) company that provides K-12 schools with digitized ERP and mobile platforms. They operate a multi-region AWS deployment across Production, Non-Production, and Shared Services accounts. The engagement was a FinOps Transformation Program — specifically scoped under Cloud Financial Management (CFM). The SOW was signed on July 10, 2025, and the project was formally completed and signed off on August 29, 2025. The engagement was delivered within a 120-day project lifecycle as defined in the SOW.

**🎯 Trick to Tackle:**
- The auditor may ask "How many students does Franciscan serve?" or "What's their revenue model?" — these test depth. If you don't know exact student count, pivot: "The key metric we tracked was Cost per 1,000 Daily Active Students, which directly ties to their business model of per-school SaaS licensing."
- Don't over-explain the customer's business. Keep it tight: industry → workload → engagement type → timeline.

---

### Q2. What was the primary business problem you were solving?

**Why They Ask:** They want to see if you solved a real business problem or just did generic optimization.

**Answer:**
Franciscan faced unpredictable seasonal spend spikes during academic admission and exam cycles. This seasonality obscured the true unit cost per student and limited budget predictability. They had only 58% tag compliance, no unit economics visibility (could not calculate cost-per-student), no anomaly detection, and no proactive cost governance. The SOW explicitly states the goal: "Establish a robust FinOps operating model to manage seasonal cost spikes and improve unit economics for school management ERP and mobile applications."

**🎯 Trick to Tackle:**
- Auditor trap: "So this was just a cost-cutting exercise?" → NO. Frame it as a FinOps transformation — cost visibility, governance, predictability, and unit economics. Cost reduction was an outcome, not the sole objective.
- Always quote the SOW goal verbatim. It shows you worked from a defined scope, not ad-hoc.

---

### Q3. What were the specific business objectives defined in the SOW?

**Why They Ask:** They're checking if you had measurable targets, not vague goals.

**Answer:**
The SOW defines three business objectives:
1. **Cost Predictability** — Stabilize forecast variance to within ±3% of actual monthly spend despite academic cycles.
2. **Commitment Strategy** — Uplift Savings Plans/RI coverage to 70%+ with >95% utilization.
3. **Anomaly Resolution** — Automate anomaly detection and route alerts to resource owners within minutes.

All three were met or exceeded. SP coverage reached 72% (target 70%+), SP utilization hit 98% (target >95%), and anomaly detection was automated with alerts routed via email and third-party webhook.

**🎯 Trick to Tackle:**
- If auditor asks "Did you meet all three?" — answer with specific numbers, not "yes." Numbers = credibility.
- If they push on forecast variance (±3%), be ready: "We implemented ML-based seasonal forecasting that accounts for admission and exam cycles, validated against 6+ months of CUR data."
- Never say "approximately" when you have exact numbers. Say "72%" not "around 70%."

---

### Q4. What is the scope boundary of this engagement? What is explicitly out of scope?

**Why They Ask:** This is a critical compliance question. They want to see you operated within defined boundaries.

**Answer:**
The scope is strictly limited to Cloud Financial Management and infrastructure governance. The SOW explicitly excludes:
- Application-level code refactoring
- Optimization of non-AWS environments
- Security remediation unrelated to cost governance
- Ongoing managed services (requires a separate agreement)

We operated within these boundaries throughout the engagement. Any work outside scope would have required a formal Change Request through our 3-step audit-ready workflow.

**🎯 Trick to Tackle:**
- Auditor trap: "Did you do any security work?" → YES, but only security related to cost governance (SCPs, IAM Identity Center, encryption enforcement). NOT security remediation. Know the difference.
- If they ask "What if the customer asked you to do something out of scope?" → "We have a formal Change Request process. Any out-of-scope work requires documented justification, impact analysis, and written CTO approval."
- This question is a setup for Q22 (Change Management). Keep your answer consistent.

---

### Q5. How was the SOW formally accepted? Can you show evidence of customer sign-off?

**Why They Ask:** They need proof this is a real, completed engagement — not a fabricated reference.

**Answer:**
The SOW was signed by Rajeev Francis (Founder, Franciscan Solutions) and Sohrab Pawar (Director, Mist Avinya Technologies LLP) on July 10, 2025. The Project Sign-Off & Acceptance Report was signed on August 29, 2025, confirming all deliverables were reviewed, tested, and formally handed over. We can provide both signed documents as evidence.

**🎯 Trick to Tackle:**
- Have both documents ready to show IMMEDIATELY. Don't fumble looking for them.
- If auditor asks "Can I see the actual signatures?" — yes, both documents have named signatories with dates.
- If they ask about the gap between SOW signing and completion — "The SOW defined a 120-day lifecycle. We completed within that window."

---

## CATEGORY 2: METHODOLOGY & EXECUTION

---

### Q6. Walk me through your methodology. How did you structure this engagement?

**Why They Ask:** They want to see a repeatable, structured approach — not a one-off improvisation.

**Answer:**
We followed a 4-phase methodology aligned with the FinOps Foundation framework — Inform, Optimize, Operate:
- **Phase 1: Discovery (Week 1)** — Workload assessment mapping ERP and mobile app dependencies across EKS and RDS. Collected 6+ months of CUR data. Defined "Cost per 1,000 Daily Active Students" as the unit economics metric.
- **Phase 2: Governance Blueprint (Week 2)** — Organized accounts into OUs, implemented SCPs for regional restrictions/encryption/spending controls, established 4 mandatory tags enforced via Tag Policies and SCPs.
- **Phase 3: Deployment (Weeks 3-5)** — Cost optimization (EBS gp2→gp3, rightsizing, S3 lifecycle, Savings Plans), QuickSight/Athena dashboard deployment, Aquila Cloud integration.
- **Phase 4: Operational Readiness (Week 6)** — Anomaly detection configuration, knowledge transfer with SOPs, 3 training sessions, final KPI validation and handoff.

**🎯 Trick to Tackle:**
- Auditor will test if this is a real methodology or just a slide deck. Be ready to go deep on ANY phase.
- If they ask "Why only 6 weeks?" — "The SOW defined a 120-day lifecycle, but the active implementation was 6 weeks. The remaining time was for stabilization, monitoring, and formal sign-off."
- If they ask "Is this the same methodology you use for other customers?" — YES. Reference the Cloud Ops Practice Requirements document which defines standardized methodologies.

---

### Q7. How did you use Infrastructure as Code (IaC) in this engagement?

**Why They Ask:** IaC is a core AWS competency requirement. They want to see real implementation, not just "we use Terraform."

**Answer:**
All infrastructure is managed via Terraform IaC following a GitOps-driven workflow. The Git repository is the single source of truth. Our CI/CD pipeline includes:
- Static analysis using tflint for Terraform linting
- Security scanning using tfsec and Checkov
- Tag compliance checks that fail the pipeline if mandatory tags are missing
- Provider-native validation tools

The rollback mechanism is to revert the Git commit and re-run the pipeline. For failed deployments, the pipeline auto-rolls back to the last known good state. Evidence includes Terraform code for scalable EKS node groups as documented in the Customer Example Requirements.

**🎯 Trick to Tackle:**
- Auditor may ask to see actual Terraform code or pipeline config. Have a sample ready.
- If they ask "What if someone bypasses the pipeline and creates resources manually?" → "SCPs enforce tag compliance at the API level regardless of how resources are created. The pipeline is the primary control, SCPs are the backstop."
- Know the difference between tflint (linting), tfsec (security), and Checkov (compliance). Don't mix them up.

---

### Q8. How did you handle the Discovery phase? What data did you collect?

**Why They Ask:** Discovery quality determines engagement quality. They want to see rigor.

**Answer:**
During Discovery (Week 1), we:
- Audited the full multi-account environment — Production, Non-Production, and Shared Services
- Mapped all ERP and mobile application workloads running on Amazon EKS and Amazon RDS
- Collected 6+ months of Cost & Usage Report (CUR) data with hourly granularity and resource-level detail
- Identified compute, storage, and networking dependencies across EC2, Lambda, DynamoDB, S3, and CloudFront
- Our Solutions Architect (CFM) spent ~40 hours on this phase — designing the multi-account structure, workload isolation, and conducting an AWS Well-Architected review

**🎯 Trick to Tackle:**
- If auditor asks "Why 6 months of CUR data?" → "To capture at least two full academic cycles (admission + exam seasons) for accurate seasonal forecasting."
- If they ask "Did you do a Well-Architected Review?" → YES, explicitly mentioned in the SOW. The Solutions Architect conducted it during Discovery.
- Don't say "we looked at everything." Be specific about what you audited and why.

---

### Q9. What project team did you deploy? What were their roles?

**Why They Ask:** They want to verify you have qualified, specialized staff — not generalists wearing multiple hats.

**Answer:**
The SOW defines a specific team structure:
- **Solutions Architect (CFM) — ~40 hours:** AWS multi-account design, workload isolation, Well-Architected review
- **Data Analyst (FinOps) — ~30 hours:** CUR data ingestion, cost allocation analysis, unit cost modeling, anomaly detection config
- **Cloud Security Engineer — ~20 hours:** SCP guardrails, IAM Identity Center setup, governance policy alignment
- **Project Manager — ~15 hours:** Milestone tracking, change log management, stakeholder reporting
- **Customer SMEs — Ad-hoc:** ERP/mobile workflow validation, infrastructure scaling inputs

This is not a generalist team — each role had defined audit-critical responsibilities aligned to SOW deliverables.

**🎯 Trick to Tackle:**
- Auditor trap: "Are these people AWS certified?" → Be ready with certification details. Reference the company PPT: "15+ AWS-certified professionals."
- If they ask "Did the same person do multiple roles?" → Emphasize role separation. Each role has distinct hours and responsibilities.
- If they push on hours ("Only 40 hours for the architect?") → "These are implementation hours. The methodology is repeatable and tooling-assisted, which makes the team efficient."

---

## CATEGORY 3: TAGGING & GOVERNANCE

---

### Q10. What is your tagging strategy? How do you enforce it?

**Why They Ask:** Tagging is the foundation of cost governance. Weak tagging = weak practice. This is a make-or-break question.

**Answer:**
We implemented 4 mandatory cost allocation tags: `application_id`, `cost_center`, `client_id`, and `environment`. These were defined in a formal Tag Dictionary with approved values.

Enforcement uses both preventative and detective controls:

**Preventative:**
- AWS Tag Policies within Organizations define allowed tag keys and permitted values
- SCPs deny `ec2:RunInstances` (and similar actions) if mandatory tags are missing — hard block
- IAM Policy conditions (`aws:RequestTag/environment`) force tag application at creation time
- CI/CD pipeline checks via CloudFormation Guard and Checkov fail deployments missing required tags

**Detective:**
- AWS Config Rules (required-tags managed rule) continuously scan for non-compliant resources
- AWS Resource Groups & Tag Editor provide real-time views of untagged resources across all regions
- EventBridge + Lambda automation auto-applies default tags or notifies resource owners on violations

**Result:** Tag compliance improved from 58% to 96%.

**🎯 Trick to Tackle:**
- Auditor will almost certainly ask "Why not 100%?" → "Some AWS resources don't support tagging (e.g., certain managed service sub-resources). 96% represents near-complete coverage of taggable resources."
- If they ask "Show me the SCP that blocks untagged resources" → Have the SCP JSON ready or reference the Cloud Ops Practice Requirements document.
- Know the difference between Tag Policies (define allowed values) and SCPs (deny actions). Don't confuse them.
- If they ask "What about resources created before your engagement?" → "We ran a Tag Clean-Up initiative using Aquila Cloud to identify and remediate legacy untagged resources."

---

### Q11. You mentioned 58% to 96% tag compliance. How do you measure and track this?

**Why They Ask:** They want to verify the metric is real and continuously tracked, not a one-time snapshot.

**Answer:**
Tag compliance is tracked through the Aquila Cloud FinOps Platform dashboard which displays:
- Total Tagged Resources vs Untagged Resources
- Tagging Coverage by Account (e.g., per AWS Account ID)
- Tag purity metrics showing compliance percentage

We also use AWS Config Rules for continuous scanning and AWS Resource Groups for real-time untagged resource visibility. The dashboard allows filtering by Tagging Status (Tagged/Untagged) and drilling down by specific Tag Keys (application_id, cost_center, client_id).

**🎯 Trick to Tackle:**
- If auditor asks "Can you show me this dashboard right now?" → Be ready to demo or show screenshots from the Customer Example Requirements document.
- If they ask "How often is this refreshed?" → "AWS Config Rules scan continuously. The Aquila Cloud dashboard refreshes with CUR data on a daily aggregation cycle."
- Don't say "we check it periodically." Say "it's continuously monitored with automated alerting."

---

### Q12. How do you handle resources with invalid or incorrect tags?

**Why They Ask:** Creating tags is easy. Maintaining tag quality is hard. They want to see ongoing governance.

**Answer:**
We ran a dedicated Tag Clean-Up initiative. The Aquila Cloud dashboard identifies resources with "Invalid Tags" — tags that do not conform to the naming schema. For automated remediation, we use EventBridge + Lambda: when AWS Config detects a non-compliant resource, EventBridge triggers a Lambda function that either auto-applies a default tag (e.g., `cost-center: unassigned`) or sends a notification to the resource owner for correction.

**🎯 Trick to Tackle:**
- Auditor trap: "What happens if someone puts a wrong value in a tag?" → "Tag Policies define allowed values for each tag key. If someone tries to apply an unapproved value, the Tag Policy blocks it."
- If they ask about tag key naming conventions → "We use snake_case (application_id, not ApplicationId) as defined in our Tag Dictionary."

---

### Q13. How are your governance guardrails (SCPs) structured?

**Why They Ask:** SCPs are the backbone of AWS governance. They want to see thoughtful design, not copy-paste policies.

**Answer:**
SCPs are implemented at the AWS Organizations level and attached to OUs. They enforce:
- **Regional restrictions** — limiting resource creation to approved AWS regions
- **Encryption requirements** — enforcing encryption standards
- **Spending controls** — budget-related guardrails
- **Tag enforcement** — denying resource creation without mandatory tags

All SCP changes follow a testing process — changes are first tested in a dedicated Policy Staging OU before being applied to production OUs. This prevents governance misconfigurations from impacting live workloads.

**🎯 Trick to Tackle:**
- Key differentiator: mention the Policy Staging OU. Most partners don't test SCPs before applying them. This shows maturity.
- If auditor asks "What if an SCP accidentally blocks a legitimate action?" → "That's why we use the Policy Staging OU. SCPs are tested against non-production workloads before promotion."
- If they ask "Are SCPs inherited?" → YES, SCPs are inherited down the OU hierarchy. Child OUs get the intersection of parent and child SCPs.

---

## CATEGORY 4: COST OPTIMIZATION & FINOPS

---

### Q14. What specific cost optimization actions did you take?

**Why They Ask:** They want specifics, not generalities. "We optimized costs" means nothing. They want actions, metrics, and evidence.

**Answer:**
Seven specific optimization actions:
1. **EC2/RDS Right-Sizing** — Used Compute Optimizer + Aquila Cloud based on CPU utilization, IOPS, and throughput metrics. Example: t3.xlarge instances downsized with quantified annual savings.
2. **Orphaned Resource Cleanup** — Identified unattached EBS volumes, idle EC2 instances. Dashboard shows count, 30-day cost, and potential savings.
3. **EBS GP2 to GP3 Migration** — 2,056 GP2 volumes identified. GP3 offers better baseline performance at lower cost.
4. **EBS Snapshot Management** — Policy-based snapshot identification and removal. Tracks Total Snapshots and Daily/Monthly Snapshot Cost.
5. **Savings Plans Adoption** — Coverage from 18% to 72%, utilization at 98%.
6. **S3 Lifecycle Policies** — Transition infrequently accessed data to cheaper storage classes, expire old objects.
7. **EC2 Scheduling** — Scheduling compute for seasonal demand patterns.

**🎯 Trick to Tackle:**
- Auditor will pick one and go deep. Be ready to explain ANY of the seven in detail.
- If they ask "How much did each action save?" → Have the Aquila Cloud dashboard ready showing per-action savings.
- On GP2→GP3: "GP3 provides 3,000 IOPS and 125 MB/s throughput baseline at 20% lower cost than GP2. For 2,056 volumes, the savings are significant."
- On orphaned resources: "These are resources costing money but serving no workload. It's the lowest-hanging fruit in any optimization."

---

### Q15. How do you validate that right-sizing recommendations are based on actual utilization?

**Why They Ask:** They want to ensure you're not blindly downsizing and breaking workloads.

**Answer:**
Right-sizing recommendations are generated from actual utilization data, not theoretical sizing. The Aquila Cloud dashboard shows detailed graphs for:
- CPU Utilization
- Volume Read/Write IOPS
- Volume Read/Write Bytes/Second (throughput)

These metrics are collected over time to ensure recommendations reflect real workload patterns, not point-in-time snapshots. The inclusion of IOPS and throughput metrics is also relevant to establishing unit economics for storage-intensive workloads.

**🎯 Trick to Tackle:**
- Auditor trap: "What if a workload spikes once a month and you downsize based on average utilization?" → "We analyze peak utilization, not just averages. Recommendations account for P95/P99 utilization to ensure headroom for spikes."
- If they ask "Who approves right-sizing changes?" → "Per the RACI matrix, Franciscan is Accountable. We recommend, they approve via the Change Request process."
- Never say "we just used Compute Optimizer." Show that you validated recommendations with multiple data sources.

---

### Q16. How did you approach Savings Plans and Reserved Instances?

**Why They Ask:** SP/RI strategy is a core FinOps competency. They want to see analysis, not just purchases.

**Answer:**
We analyzed historical compute usage patterns across EKS, EC2, Lambda, and RDS workloads to determine the optimal SP mix (Compute SP vs EC2 SP). Per the RACI matrix, Mist Avinya was Consulted on SP/RI purchase decisions while Franciscan was Responsible and Accountable — we provided analysis and recommendations, they made the buying decisions.

Results:
- SP coverage: 18% → 72% (SOW target was 70%+)
- SP utilization: 98% (SOW target was >95%)
- Unused RIs identified (e.g., Amazon RDS) and flagged for optimization

The Aquila Cloud dashboard tracks total reservations, purchased units, resources covered, total reservation spend, unused reservation fees, and RI utilization.

**🎯 Trick to Tackle:**
- Auditor trap: "Why not 100% SP coverage?" → "100% coverage would over-commit. The 72% target covers steady-state baseline compute. The remaining 28% is on-demand to handle seasonal spikes (exam/admission cycles) without waste."
- If they ask "Compute SP vs EC2 SP — which did you recommend and why?" → "Compute SPs for flexibility across EC2, Lambda, and Fargate. EC2 SPs only where we had high-confidence, stable instance families."
- RACI is critical here: emphasize that YOU did not purchase SPs. The customer did. You advised.

---

### Q17. How do you handle cost anomaly detection?

**Why They Ask:** Anomaly detection separates reactive from proactive cost management.

**Answer:**
We deployed anomaly detection through two mechanisms:
1. **AWS Budgets** — Configured with automated alerts monitoring Daily Average Cost, Daily Variance, Forecasted Cost for the month, and Predicted Cost at end of month.
2. **Aquila Cloud** — AI-driven anomaly detection with explicit Anomaly Detection Status and Mitigation Status tracking. When an anomaly is detected, the system flags it and tracks whether mitigation has been applied.

Alerts are routed via email AND a third-party webhook (ITSM/chat integration) — meeting the SOW requirement to "route alerts to resource owners within minutes."

**🎯 Trick to Tackle:**
- If auditor asks "What's the threshold for an anomaly?" → "The system uses ML-based baselines that account for seasonal patterns. A deviation beyond the expected range triggers an alert — it's not a fixed dollar threshold."
- If they ask "How fast do you respond?" → "Alerts are routed within minutes via webhook. The bi-weekly governance review tracks anomaly resolution as a standing item."
- Mention both AWS-native (Budgets) AND third-party (Aquila Cloud) — shows defense in depth.

---

### Q18. How do you provide cost visibility and reporting?

**Why They Ask:** Dashboards are evidence. They want to see what the customer actually sees.

**Answer:**
Cost visibility is delivered through multiple layers:
- **Aquila Cloud Overview Dashboard** — Filters by Cloud Provider, Service, Region with "Add Dimension" for further grouping. Shows Top 10 Cloud Services by Cost.
- **Cost Trend View** — Month-over-month changes in actual spend with percentage change between months.
- **Amazon QuickSight CID Dashboards** — Cost-per-DAU tracking, service-level breakdowns, budget burn-down views.
- **CUR Integration** — Automated Daily Aggregation Refresh, Monthly Cost Aggregation, and Bill Data Download processes confirmed in system logs.
- **Predictive Estimates** — Both "Domains Previous Spend" and "Domains Spend Forecast" displayed together for comparison.

**🎯 Trick to Tackle:**
- Be ready to show or demo dashboards. Screenshots from the Customer Example Requirements document are your evidence.
- If auditor asks "Who has access to these dashboards?" → "Engineering and Finance teams. Access is role-based — Finance sees cost allocation views, Engineering sees resource-level optimization views."
- If they ask "How often are dashboards updated?" → "CUR data refreshes daily. QuickSight dashboards reflect the latest available data."

---

### Q19. Can you explain your unit economics approach?

**Why They Ask:** Unit economics is the gold standard of FinOps maturity. This differentiates you from basic cost-cutters.

**Answer:**
The SOW defines the unit economics metric as "Cost per 1,000 Daily Active Students (DAU)." We:
- Correlated AWS spend data with student activity data
- Built this metric into QuickSight dashboards for continuous tracking (not a one-time calculation)
- Shared unit economics data with Product and Engineering teams for architectural decision-making
- Used the Aquila Cloud dashboard to drill down into costs by specific Tag Keys (application_id, cost_center, client_id) to attribute costs to business units

Before our engagement, Franciscan could not calculate cost-per-student. Now it is a tracked KPI shared across teams.

**🎯 Trick to Tackle:**
- This is your strongest differentiator. Lean into it.
- If auditor asks "How do you get student activity data?" → "Franciscan provides DAU metrics from their application layer. We correlate this with AWS spend data in QuickSight."
- If they ask "Is this metric still being tracked after your engagement?" → "Yes. The dashboard is self-service and the metric is part of the bi-weekly governance review cadence we established."
- If they push: "What decisions has Franciscan made based on this metric?" → "It enables them to evaluate the cost impact of adding new schools or features — true business-level cloud economics."

---

## CATEGORY 5: MULTI-ACCOUNT STRATEGY & ACCOUNT LIFECYCLE

---

### Q20. How is the AWS multi-account environment structured?

**Why They Ask:** Multi-account strategy is a core AWS best practice. They want to see you follow the Landing Zone model.

**Answer:**
Franciscan operates across three account types: Production, Non-Production, and Shared Services. These are organized into Organizational Units (OUs) within AWS Organizations. Each OU has appropriate SCPs attached for governance. We follow the AWS recommended OU structure:
- **Security OU** — Centralized security tools and monitoring
- **Infrastructure OU** — Shared services like networking
- **Workload OUs** — Business application accounts segregated by environment
- **Sandbox OU** — Experimentation with spending guardrails
- **Policy Staging OU** — Testing new/modified SCPs before production deployment

**🎯 Trick to Tackle:**
- Auditor may ask "Why separate accounts instead of VPCs?" → "Accounts provide hard blast-radius isolation, separate billing boundaries, and independent IAM boundaries. VPCs alone don't provide billing or IAM isolation."
- If they ask about the Policy Staging OU → This is a maturity indicator. Emphasize it: "We test all SCP changes in a staging OU before applying to production. This prevents governance misconfigurations."
- Know the difference between OUs and accounts. OUs are containers for governance; accounts are the actual isolation boundaries.

---

### Q21. How do you manage the account lifecycle?

**Why They Ask:** Account sprawl is a real problem. They want to see you have a process from birth to death.

**Answer:**
We follow a 4-stage account lifecycle: Request → Provision → Operate → Retire.

- **Request:** Business unit submits a request with justification, designated owner, cost center, and desired OU placement.
- **Provision:** 100% automated using AWS Control Tower Account Factory. A CloudFormation-based "Account Blueprint" is deployed automatically via a Step Functions state machine triggered by Control Tower Lifecycle Events.
- **Operate:** Continuous monitoring via Security Hub, GuardDuty, and AWS Config. Baseline updates managed through CloudFormation StackSets.
- **Retire:** Account moved to a "Suspended" OU with restrictive SCPs. Data archived to immutable S3 in the Log Archive account. After 90-day waiting period, account formally closed via automated scripts.

**🎯 Trick to Tackle:**
- The Retire phase is where auditors dig deep. Know the 90-day waiting period and immutable S3 archival.
- If they ask "What's in the Account Blueprint?" → "Baseline security controls, CloudTrail logging, Config Rules, GuardDuty enrollment, and mandatory tag enforcement."
- If they ask "What happens to data in a retired account?" → "Archived to immutable S3 in the Log Archive account before closure. Data retention follows compliance requirements."

---

## CATEGORY 6: CHANGE MANAGEMENT & PROJECT GOVERNANCE

---

### Q22. How did you manage changes during the engagement?

**Why They Ask:** Change management is an audit fundamental. No change management = no audit pass.

**Answer:**
The SOW defines a 3-Step "Audit-Ready" Workflow:
1. **Submission** — A formal Change Request (CR) is logged documenting the technical justification (e.g., scaling infrastructure for exam-season traffic).
2. **Impact Analysis** — Mist Avinya assesses the impact on Unit Cost-per-Student and Project Budget.
3. **Approval** — Written authorization from the Franciscan CTO is mandatory before any production environment change. No verbal approvals.

This process maintained audit integrity throughout the engagement. Every change was documented, assessed for cost impact, and formally approved.

**🎯 Trick to Tackle:**
- Auditor trap: "Did you ever make a change without going through this process?" → The answer is NO. If there was an emergency, say: "Even emergency changes follow the CR process — they're just expedited with post-facto documentation."
- Emphasize "written authorization" and "no verbal approvals." This is audit gold.
- If they ask "How many CRs were raised?" → Have the number ready or say "The change log is maintained by our Project Manager and available for review."

---

### Q23. What was the RACI structure for this engagement?

**Why They Ask:** RACI shows professional engagement governance and clear accountability.

**Answer:**
| Task | Mist Avinya | Franciscan |
|------|-------------|------------|
| Discovery & Inventory Assessment | R (Responsible) | C (Consulted) |
| AWS Organization & OU Design | R (Responsible) | A (Accountable) |
| SCP Definition | R (Responsible) | A (Accountable) |
| Tagging Schema Implementation | R (Responsible) | I (Informed) |
| Savings Plans & RI Purchase Decisions | C (Consulted) | R/A (Responsible & Accountable) |

This separation is important — Mist Avinya did the implementation work, but Franciscan retained accountability for architecture sign-off and commitment purchase decisions.

**🎯 Trick to Tackle:**
- The SP/RI row is critical. Auditors want to see that the CUSTOMER made purchase decisions, not the partner. This avoids conflict of interest.
- If auditor asks "Who is accountable if something goes wrong?" → "Franciscan is Accountable for architecture and purchase decisions. We are Responsible for implementation quality."
- Know RACI definitions cold: R = does the work, A = owns the outcome, C = provides input, I = kept informed.

---

### Q24. How did you govern the engagement on an ongoing basis?

**Why They Ask:** They want to see structured governance, not ad-hoc check-ins.

**Answer:**
The SOW establishes a Governance Hub with bi-weekly reviews analyzing "Unit Cost per 1,000 DAU." These were structured review meetings — not informal check-ins — covering:
- Cost-per-DAU trend analysis
- Budget burn-down reviews
- Cost anomaly review
- Optimization progress tracking
- Action items tracked and assigned via the RACI matrix

Our Project Manager (~15 hours) handled milestone tracking, change log management, and stakeholder reporting.

**🎯 Trick to Tackle:**
- If auditor asks "Do you have meeting minutes?" → "Yes, our Project Manager maintained documentation of all governance reviews."
- Emphasize "bi-weekly" and "structured." This shows discipline.
- If they ask "Who attended these reviews?" → "Stakeholders from both sides as defined in the RACI — our PM, Solutions Architect, and Franciscan's CTO/technical leads."

---

## CATEGORY 7: KNOWLEDGE TRANSFER & OPERATIONAL HANDOFF

---

### Q25. What training did you deliver?

**Why They Ask:** Knowledge transfer proves the engagement creates lasting value, not dependency.

**Answer:**
We delivered 3 deep-dive training sessions to the Engineering and Finance teams covering:
- Cloud Center of Excellence (CCoE) operating model
- FinOps best practices
- Aquila Cloud platform usage and dashboard interpretation

The Aquila Cloud interface includes a "Training & Support" section with resources for "FinOps Best Practices," confirming that our methodology includes knowledge transfer and best practices guidance. This is documented in the Project Sign-Off Report.

**🎯 Trick to Tackle:**
- If auditor asks "Were these recorded?" → If yes, great. If not, say "Training was delivered live with hands-on exercises. SOPs and runbooks serve as the reference material."
- If they ask "How do you know the training was effective?" → "The Project Sign-Off Report confirms the customer accepted the knowledge transfer. The CCoE model assigns internal ownership, ensuring the team can operate independently."
- Three sessions is a good number. Don't undersell it.

---

### Q26. What documentation did you hand over?

**Why They Ask:** Documentation = sustainability. No docs = the customer can't operate without you.

**Answer:**
The operational handoff included:
- SOPs for monthly cost reviews and commitment management
- Governance guidelines documenting SCPs, Tag Policies, Config Rules, and IAM conditions
- Operational runbooks for ongoing operations
- As-built diagrams of the AWS environment
- Dashboard access and interpretation guides

The Project Sign-Off Report confirms: "Knowledge Transfer: 3 deep-dive training sessions were conducted for the Engineering and Finance teams covering the CCoE operating model."

**🎯 Trick to Tackle:**
- If auditor asks to see a specific SOP → Have at least one ready to show.
- If they ask "Are these documents version-controlled?" → "Yes, documentation is maintained alongside IaC in the Git repository."
- Don't just list documents. Emphasize that they enable independent operation.

---

### Q27. How does Franciscan continue operating after your engagement ends?

**Why They Ask:** This is the sustainability test. They want to see you didn't create a dependency.

**Answer:**
The Operate phase of our methodology was specifically designed for sustainability:
- Monthly business review cadence established
- CCoE operating model in place with cross-functional participation
- Real-time dashboards accessible to Engineering and Finance teams
- Automated governance (SCPs, Config Rules, EventBridge+Lambda) runs continuously without manual intervention
- SOPs and runbooks enable the team to handle cost reviews and commitment management independently

Formal project support concluded on August 29, 2025. Any further engagement requires a separate support agreement.

**🎯 Trick to Tackle:**
- Auditor trap: "So if something breaks, they're on their own?" → "The automated governance continues running. The SOPs cover common scenarios. For complex issues, they can engage us under a separate support agreement — but the system is designed to be self-sustaining."
- Emphasize automation: "SCPs don't need someone to run them. Config Rules scan continuously. EventBridge+Lambda remediates automatically."
- The CCoE model is key — it assigns internal ownership to Franciscan teams.
