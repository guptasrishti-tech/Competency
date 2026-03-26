# Mist Avinya Technologies LLP — AWS CloudOps Competency (CFM) Audit Summary

**Prepared for:** AWS Competency Audit — Cloud Operations (FinOps)  
**Partner:** Mist Avinya Technologies LLP | AWS Advanced Tier Partner  
**Date:** March 2026

---

## Part 1: Case Study Summaries (All 4 Clients)

The PPT presents 4 client engagements under Mist Avinya's Cloud Operations & FinOps practice. Below is the summarized view of each:

---

### 1. CloudRev (Digital Services)

| Aspect | Details |
|---|---|
| **Challenge** | Limited AWS cost visibility, inconsistent resource tagging, low commitment coverage, difficulty forecasting during variable demand periods |
| **Solution** | Implemented cross-functional FinOps framework with CUR integrated into Aquila Cloud, mandatory tagging enforcement, compute rightsizing and Savings Plans adoption |
| **AWS Services** | EKS, Lambda, RDS, S3, CloudFront, Athena, Cost Explorer, AWS Budgets, AWS Organizations |
| **Results** | ~30% monthly AWS cost reduction within 120 days · >90% tag compliance · High Savings Plans utilization · Improved forecast accuracy |

---

### 2. Franciscan (EdTech)

| Aspect | Details |
|---|---|
| **Challenge** | Limited cost visibility across multiple environments, seasonal traffic spikes causing unpredictable AWS spend, inconsistent tagging, need for stronger governance |
| **Solution** | Cross-functional FinOps framework with CUR via Aquila Cloud, EC2 rightsizing and scheduling, S3 lifecycle policies, AWS Budgets with anomaly detection |
| **AWS Services** | EC2, Lambda, DynamoDB, S3, CloudFront, Athena, Cost Explorer, AWS Budgets, AWS Organizations |
| **Results** | 25–30% cost reduction · >95% tag compliance · Improved forecast accuracy during seasonal peaks · Centralized governance policies |

---

### 3. AgriTech (Agriculture Technology — Private)

| Aspect | Details |
|---|---|
| **Challenge** | High unpredictable data ingestion costs from IoT sensors, always-on EC2 instances expensive to maintain, batch processing meant farmer alerts took hours, unable to calculate cost per acre for pricing model |
| **Solution** | EC2 replaced with Lambda & Kinesis Data Analytics, AWS IoT Core for real-time sensor ingestion, Timestream + S3 Glacier for optimized storage, CUR integrated with Aquila Cloud for unit economics, full infrastructure managed via Terraform IaC |
| **AWS Services** | IoT Core, Kinesis Data Streams, Lambda, Amazon Timestream, S3, Glacier Instant Retrieval, SageMaker, Athena, CUR, AWS Budgets, Organizations |
| **Results** | 40% reduction in data processing costs · 48% lower cost per acre monitored · 90% faster farmer alerts (hours → minutes) · 55% storage cost reduction · 100% serverless pipeline achieved |

---

### 4. Healthtech (Healthcare Technology — Private)

| Aspect | Details |
|---|---|
| **Challenge** | Unpredictable GPU hardware costs, no cost-per-scan visibility, slow R&D cycles (provisioning took weeks), immense HIPAA compliance overhead on physical infrastructure, difficulty scaling inference for new hospital clients |
| **Solution** | Secure multi-account AWS Organization for HIPAA workloads, Compute Savings Plans for baseline inference fleet, SageMaker Spot Training for R&D cost reduction, S3 Intelligent-Tiering for DICOM image archives, KMS encryption + IAM + Secrets Manager for compliance, CUR integrated with Aquila Cloud for cost-per-scan KPI |
| **AWS Services** | SageMaker, EC2 G5, AWS HealthImaging, S3 Intelligent-Tiering, Lambda, KMS, Secrets Manager, CloudTrail, Athena, CUR, AWS Budgets, Organizations |
| **Results** | 45% reduction in AI inference costs · 52% lower cost per medical scan analyzed · 35% ML training cost reduction via Spot Training · 60% storage cost reduction via S3 Intelligent-Tiering · 100% HIPAA compliant encryption at rest & in transit |

---

## Part 2: Healthtech — Deep Dive (Auditor-Ready Elaboration)

This section provides the full in-depth breakdown of the Healthtech engagement, covering every point from the PPT and the detailed case study documents.

---

### Client Profile

| Field | Detail |
|---|---|
| Customer | Major Healthtech Company (AI Medical Imaging) |
| Industry | HealthTech & Diagnostics |
| Region | ap-south-1 |
| Engagement Period | August 10, 2025 – September 20, 2025 |
| Engagement Type | FinOps & HIPAA Compliance Transformation |
| Workloads | Amazon SageMaker, Amazon EC2 (GPU), AWS HealthImaging, Amazon S3, AWS Lambda, AWS KMS |
| CloudOps Focus | FinOps, Security & HIPAA Compliance |

---

### Business Context

Healthtech provides a mission-critical AI platform that assists radiologists by detecting early-stage anomalies in MRI and CT scans. Their globally distributed AI inference engine requires high-performance GPU compute (NVIDIA A10G) and petabyte-scale storage for medical DICOM imaging data. The platform was hampered by a costly and inflexible on-premises data center.

---

### Challenges Identified

**Cost & Scale Management:**
- Fixed on-premises costs — high CapEx for GPU hardware that sat idle during low-scan volume periods
- Opaque unit economics — inability to calculate the exact cost of a single AI scan, making it impossible to create a profitable B2B pricing model for hospitals
- Storage "Data Swamp" — exponential growth in high-resolution medical images led to soaring storage costs without a lifecycle or tiering strategy

**Compliance & Operational:**
- HIPAA gaps — legacy environment lacked automated encryption and real-time audit logging required for PHI (Protected Health Information)
- Research bottlenecks — data science team faced 4–6 week delays in getting infrastructure for testing new AI models, stifling innovation
- Scalability issues — difficulty scaling inference capacity to meet demands of new hospital clients

---

### Solution Architecture

Mist Avinya architected a secure, multi-account AWS Organization dedicated to HIPAA-compliant workloads, with separation of data storage, ML training, and production inference environments.

**1. Visibility & Unit Economics**
- AWS Cost and Usage Report (CUR) integrated with Aquila Clouds FinOps Optimizer
- Established primary KPI: "cost per scan analyzed" by correlating SageMaker, EC2, and S3 costs with application metadata
- Real-time forecasting and cost-per-scan dashboards

**2. Compute Optimization**
- Migrated legacy GPU workloads to Amazon SageMaker and Amazon EC2 G5 Instances with auto-scaling based on scan volume
- 3-year Compute Savings Plan applied to baseline production inference fleet
- Amazon SageMaker Spot Training implemented for all R&D workloads

**3. Storage Optimization**
- Petabytes of DICOM images migrated to Amazon S3
- S3 Intelligent-Tiering enabled to automatically move less-frequently accessed scans to cheaper tiers
- AWS HealthImaging for fast access to active scans

**4. Security & HIPAA Compliance (Governance-as-Code)**
- AWS KMS encrypts all data at rest in S3 and EBS
- All PHI processed within a secure VPC; access controlled via IAM roles and AWS Secrets Manager
- Custom Service Control Policies (SCPs) prevent launch of non-encrypted resources or unapproved instance types
- All actions logged via AWS CloudTrail to a central audit account
- Automated HIPAA guardrails via AWS Config and AWS KMS — 100% encryption compliance

**5. Automation**
- Event-driven pipeline using AWS Lambda: new scans uploaded to S3 automatically trigger SageMaker inference endpoints
- Inference endpoints backed by GPU-powered EC2 instances in an Auto Scaling Group

**6. Governance & Accountability**
- Cloud Center of Excellence (CCoE) established
- Department heads receive weekly automated budget alerts via AWS Budgets
- Showback/chargeback reporting and monthly variance reviews

---

### Architecture Flow (Summary)

1. DICOM scans uploaded to a secure Amazon S3 bucket
2. S3 event triggers an AWS Lambda function
3. Lambda retrieves the image and invokes an Amazon SageMaker inference endpoint (GPU-backed EC2 Auto Scaling Group)
4. Results stored in a separate, encrypted S3 bucket
5. All actions logged via AWS CloudTrail to a central audit account
6. CUR exported to a separate S3 bucket for analysis by Aquila Clouds and Amazon Athena

---

### Financial & Operational Results

| KPI | Before (On-Premises) | After (120 days on AWS) | Improvement |
|---|---|---|---|
| Monthly Infrastructure Spend | $10,000 | $5,500 | 45% Reduction |
| Unit Cost / Scan Analyzed | $10,000 | $4,800 | 52% Reduction |
| Savings Plans Coverage | 0% (N/A) | 80% | Optimization |
| Peak ML Training Job Cost | $100 | $65 | 35% Reduction |
| Storage Cost (per TB) | $100 | $40 | 60% Reduction |
| R&D Provisioning Time | 4–6 Weeks | < 1 Hour | 99% Faster |

---

### AWS Best Practices Alignment

| Pillar | Implementation |
|---|---|
| Cost Optimization | AWS Compute Optimizer for right-sizing G5 GPU instances; SageMaker Managed Spot Training for 35% savings on AI model re-training |
| Operational Excellence | Automated HIPAA guardrails via AWS Config and AWS KMS; 100% encryption compliance for all patient data |
| Security | KMS encryption at rest & in transit; IAM + Secrets Manager; SCPs; CloudTrail audit logging |
| Accountability | CCoE with weekly automated budget alerts via AWS Budgets |

---

### AWS Services Used (Complete List)

| Service | Purpose |
|---|---|
| Amazon SageMaker | ML Training & Inference (including Spot Training) |
| Amazon EC2 G5 Instances | GPU compute for inference fleet |
| AWS HealthImaging | Fast access to active medical scans |
| Amazon S3 (Intelligent-Tiering) | DICOM image storage with automated tiering |
| AWS Lambda | Event-driven scan processing pipeline |
| AWS KMS | Encryption of all data at rest |
| AWS Secrets Manager | Secure credential management |
| AWS CloudTrail | Audit logging for HIPAA compliance |
| Amazon Athena | CUR analysis and querying |
| AWS CUR | Cost and Usage Report for FinOps visibility |
| AWS Budgets | Budget alerts and anomaly detection |
| AWS Organizations | Multi-account governance and SCPs |
| AWS Config | Compliance guardrails |
| Aquila Clouds FinOps Optimizer | Recommendations, governance, forecasting, cost-per-scan dashboards |

---

### Business Value & Why It Matters

- **Market Competitiveness:** 52% reduction in unit costs allowed Healthtech to offer more competitive subscription rates to hospital networks
- **Regulatory Confidence:** Fully auditable HIPAA posture on AWS became a key sales differentiator when bidding for large healthcare contracts
- **Innovation Velocity:** Shift from weeks to minutes for research environments allowed the AI team to double their "model-to-market" speed

---

### Partner Differentiators (The Mist Avinya Edge)

- **Unit Economics Focus:** Built a financial model that tied AWS spend directly to medical scans (cost-per-scan KPI)
- **Industry Expertise:** Deep understanding of healthcare compliance (HIPAA) and AI/ML infrastructure (NVIDIA GPU optimization)
- **Sustainable Governance:** Integration of Aquila Clouds ensures the customer can maintain savings long after the engagement ends

---

### Customer Testimonial

> "Mist Avinya and Aquila Clouds didn't just move us to the cloud — they transformed our business. We can now scale on demand, our costs are directly tied to our revenue, and our compliance posture has never been stronger. This has allowed us to focus on what we do best: saving lives."  
> — CTO, Healthtech

---

*This document consolidates all information from the Mist Avinya PPT presentation and the Healthtech case study documents for AWS CloudOps Competency (CFM) audit review.*
