# Audit-Ready Script: 4 Case Study Overview + Introduction
# Duration: 7-8 Minutes
# Mist Avinya Technologies LLP | AWS Cloud Operations Competency - CFM

---

# INTRODUCTION (0:00 - 2:00)

"Good morning. Thank you for your time today. My name is [Name], I am [Title] at Mist Avinya Technologies LLP. We are headquartered in Noida, Uttar Pradesh, and we are an AWS Advanced Tier Partner with 15+ AWS certified professionals across delivery, sales, marketing, and operations. We hold 1 EKS Service Delivery Program and 2 Specializations in Pipeline.

We are here today for the AWS Cloud Operations Competency validation under the Cloud Financial Management category. Our practice is built around a structured FinOps methodology - Inform, Optimize, Operate - using AWS-native cost governance services integrated with the Aquila Clouds FinOps platform.

What differentiates us is that we tie cloud costs directly to business metrics. We call this unit economics. For every customer, we identify the one business KPI that matters most - cost per transaction, cost per student, cost per acre, cost per scan - and we build the entire FinOps framework around making that number visible, trackable, and optimizable.

We serve customers across four verticals: Digital Services, EdTech, AgriTech, and HealthTech. We have 35+ active clients with average project values ranging from $10K to $100K+.

For this competency validation, we are presenting four customer references. Let me give you a quick overview of each before we deep-dive into our lead case study."

---

# CASE STUDY 1: CLOUDREV (2:00 - 3:30)
# Digital Services | ~30% AWS cost reduction in 120 days

"Our first and lead customer reference is CloudRev, or Rev. They are a Web3 infrastructure provider in the Blockchain and Digital Assets space, running 24/7 validator nodes and transaction processing across ap-south-1 and ap-south-2.

The challenge was limited AWS cost visibility, inconsistent resource tagging at 45% compliance, low commitment coverage at only 25%, and difficulty forecasting during volatile market-driven demand. Their monthly spend was $105,000 and they had no way to calculate their true cost per transaction.

What we did: we implemented a cross-functional FinOps framework with the AWS Cost and Usage Report integrated into Aquila Cloud for centralized visibility. We enforced mandatory tagging using AWS Organizations and SCPs - five tags: Chain, Tenant, Node-Type, Environment, and Owner. We performed compute rightsizing by migrating 80% of eligible workloads to AWS Graviton, and adopted a layered Savings Plans strategy for their always-on infrastructure.

The results within 120 days: approximately 30% monthly AWS cost reduction - specifically 34%, bringing spend from $105,000 to $69,300. Tag compliance reached 98%. Savings Plans coverage went to 85% with 99% utilization. Unit cost per million transactions dropped by 40%. Total annualized savings exceeded $428,000.

AWS services used: EKS, Lambda, RDS, S3, CloudFront, Athena, Cost Explorer, AWS Budgets, and AWS Organizations."

---

# CASE STUDY 2: FRANCISCAN (3:30 - 4:45)
# EdTech | 25-30% cost reduction, 95% tag compliance

"Our second customer reference is Franciscan, an EdTech provider operating a student portal and learning management platform.

The challenge was limited cost visibility across multiple environments, seasonal traffic spikes during admissions and exam periods causing unpredictable AWS spend, inconsistent tagging at 58% baseline, and a need for stronger governance. Commitment coverage was at only 18%.

What we did: we implemented a cross-functional FinOps framework with the CUR integrated via Aquila Cloud. We performed EC2 rightsizing and scheduling based on historical usage patterns to handle seasonal demand. We applied S3 lifecycle policies for storage optimization and configured AWS Budgets with anomaly detection. Mandatory tagging was enforced using AWS Organizations - four tags: Owner, Environment, CostCenter, and Application. We deployed Aurora Serverless with auto_pause to scale down during school holidays.

The results: 25 to 30% cost reduction over the engagement - specifically 29%. Tag compliance improved from 58% to over 95%, reaching 96%. Savings Plans coverage went from 18% to 72% with 98% utilization. The core unit cost per 1,000 daily active students was reduced by 35%. Centralized governance policies were established across the multi-account environment. A FinOps Cloud Center of Excellence was set up to forecast Cost per 1,000 Daily Active Users.

AWS services used: EC2, Lambda, DynamoDB, S3, CloudFront, Athena, Cost Explorer, AWS Budgets, and AWS Organizations."

---

# CASE STUDY 3: KIWIKISAN - AGRITECH (4:45 - 6:00)
# Agriculture Technology | Ongoing FinOps optimization on AWS

"Our third customer reference is KiwiKisan, a precision agriculture company using IoT sensors for farm monitoring.

They had four core challenges: high unpredictable data ingestion costs from IoT sensors, always-on EC2 instances that were expensive to maintain, batch processing that meant farmer alerts took hours instead of real-time, and an inability to calculate cost per acre for their pricing model.

What we did: we replaced EC2 with Lambda and Kinesis Data Analytics to build a fully serverless pipeline. AWS IoT Core was implemented for real-time sensor ingestion. Amazon Timestream and S3 Glacier were used for optimized time-series and archival storage. The CUR was integrated with Aquila Cloud for unit economics - specifically cost per acre monitored. The full infrastructure was managed via Terraform IaC.

The results: 40% reduction in data processing costs. 48% lower cost per acre monitored. Farmer alerts went from hours to minutes - a 90% improvement. Storage costs were reduced by 55% using Timestream and S3 lifecycle policies. We achieved a 100% serverless pipeline, eliminating operational overhead from 2 FTEs to 0.5 FTEs.

AWS services used: IoT Core, Kinesis Data Streams, Lambda, Amazon Timestream, S3, Glacier Instant Retrieval, SageMaker, Athena, CUR, AWS Budgets, and AWS Organizations."

---

# CASE STUDY 4: GENEPOWERX - HEALTHTECH (6:00 - 7:15)
# Healthcare Technology | Secure cloud ops with cost governance

"Our fourth customer reference is Genepowerx, an AI diagnostics company processing medical imaging.

The challenges: unpredictable GPU hardware costs with no cost-per-scan visibility, slow R&D cycles where provisioning new hardware took 4 to 6 weeks, immense HIPAA compliance overhead on physical infrastructure, and difficulty scaling inference capacity for new hospital clients.

What we did: we built a secure multi-account AWS Organization for HIPAA workloads with dedicated OUs for PHI Storage, ML Training, ML Inference, and Security/Audit. Compute Savings Plans were implemented for the baseline inference fleet - coverage went from 10% to 80%. Amazon SageMaker Spot Training was used for R&D cost reduction. S3 Intelligent-Tiering was applied for DICOM image archives. KMS encryption, IAM, and Secrets Manager ensured full compliance. The CUR was integrated with Aquila Cloud for the cost-per-scan KPI.

The results: 45% reduction in AI inference costs. 52% lower cost per medical scan analyzed. 35% ML training cost reduction via SageMaker Spot Training. 60% storage cost reduction via S3 Intelligent-Tiering. 100% HIPAA compliant encryption at rest and in transit. R&D provisioning time went from 4 to 6 weeks to less than 1 hour.

AWS services used: SageMaker, EC2 G5, AWS HealthImaging, S3 Intelligent-Tiering, Lambda, KMS, Secrets Manager, CloudTrail, Athena, CUR, AWS Budgets, and AWS Organizations."

---

# TRANSITION TO DEEP DIVE (7:15 - 7:45)

"As you can see, across all four engagements, the common thread is our Inform, Optimize, Operate methodology, mandatory tagging enforcement via AWS Organizations, unit economics tied to a business-specific KPI, and the Aquila Clouds platform integrated with AWS-native cost governance.

Now, let me take you into a detailed deep-dive on CloudRev, our lead case study, where I will walk through the SOW, the methodology execution, the OU architecture, the tagging enforcement, the optimization work, the change management process, and the final results with how we achieved each number."

---
