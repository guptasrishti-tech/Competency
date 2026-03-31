# Aquila Cloud — Evidence Request for AWS CloudOps Competency Audit
## Customer: Franciscan Solutions | Partner: Mist Avinya Technologies LLP

---

## Purpose

Hi Team,

We are preparing for our **AWS CloudOps Competency Audit** for the Franciscan Solutions engagement. The auditor will ask us to demonstrate specific capabilities and results through the Aquila Cloud platform. We need **screenshots, exports, and/or live demo access** for the items listed below.

Please provide each item as a **clearly labeled screenshot (PNG/PDF)** or **data export (CSV/Excel)** so we can include them in our evidence pack. Where possible, ensure the Franciscan account data is visible and the date range covers **July–August 2025** (engagement period).

> **Deadline:** Please share all evidence at least **5 business days before the audit date** so we can review and organize.

---

## 1. COST OVERVIEW & VISIBILITY

| # | What We Need | Details / Filters to Apply |
|---|-------------|---------------------------|
| 1.1 | **Cost Overview Dashboard** — Full screenshot | Show filters: Cloud Provider = AWS, with Service and Region dimensions visible. Must show "Top 10 Cloud Services by Cost" widget. |
| 1.2 | **Cost Trend View** — Month-over-month spend | Show at least 3 months of data with percentage change between months clearly visible (e.g., June → July → August 2025). |
| 1.3 | **Cost Breakdown by Service** | Drill-down showing cost split across EC2, EKS, RDS, S3, Lambda, DynamoDB, CloudFront for Franciscan accounts. |
| 1.4 | **"Add Dimension" Grouping** | Screenshot showing cost grouped by a custom dimension (e.g., by `cost_center` or `application_id` tag). This proves tag-based cost allocation works. |
| 1.5 | **Predictive Spend Forecast** | Screenshot showing both "Domains Previous Spend" and "Domains Spend Forecast" side by side for comparison. |
| 1.6 | **CUR Data Integration Logs** | Screenshot or export showing: Daily Aggregation Refresh status, Monthly Cost Aggregation status, and Bill Data Download confirmation. |

---

## 2. TAG COMPLIANCE & GOVERNANCE

| # | What We Need | Details / Filters to Apply |
|---|-------------|---------------------------|
| 2.1 | **Tag Compliance Overview** | Dashboard showing: Total Tagged Resources vs Untagged Resources count, and overall compliance percentage (should show ~96%). |
| 2.2 | **Tagging Coverage by Account** | Breakdown showing tag compliance per AWS Account ID (Production, Non-Production, Shared Services). |
| 2.3 | **Tag Purity Metrics** | Screenshot showing tag purity/quality score — resources with valid vs invalid tags. |
| 2.4 | **Filter by Tagging Status** | Two screenshots: one filtered to "Tagged" resources, one filtered to "Untagged" resources. |
| 2.5 | **Drill-down by Tag Key** | Screenshots showing resources filtered by each mandatory tag: `application_id`, `cost_center`, `client_id`, `environment`. |
| 2.6 | **Invalid Tags Report** | Screenshot showing resources flagged with "Invalid Tags" (tags not conforming to naming schema). This proves we have tag quality governance, not just tag existence. |
| 2.7 | **Before vs After Comparison** | If available — any historical view or report showing tag compliance improvement from ~58% to ~96% over the engagement period. |

---

## 3. RIGHT-SIZING RECOMMENDATIONS

| # | What We Need | Details / Filters to Apply |
|---|-------------|---------------------------|
| 3.1 | **Right-Sizing Recommendations List** | Full list of EC2/RDS right-sizing recommendations for Franciscan with current instance type, recommended instance type, and estimated savings. |
| 3.2 | **CPU Utilization Graph** | For at least 2-3 sample instances — detailed CPU utilization graph over time (not a point-in-time snapshot). |
| 3.3 | **Volume Read/Write IOPS Graph** | For at least 2-3 sample instances — IOPS metrics over time. |
| 3.4 | **Volume Read/Write Throughput Graph** | For at least 2-3 sample instances — Bytes/Second (throughput) metrics over time. |
| 3.5 | **Recommendation Summary with Savings** | Aggregate view showing total number of recommendations and total estimated annual savings. |

> **Why this matters:** The auditor will verify that right-sizing is based on actual utilization data (CPU, IOPS, throughput), not theoretical sizing. The time-series graphs are critical evidence.

---

## 4. ORPHANED & IDLE RESOURCES

| # | What We Need | Details / Filters to Apply |
|---|-------------|---------------------------|
| 4.1 | **Orphaned Resources Dashboard** | Screenshot showing: unattached EBS volumes, idle EC2 instances — with count, 30-day cost, and potential savings for each. |
| 4.2 | **Idle Resource Details** | List of identified idle resources with resource ID, type, region, and monthly cost. |

---

## 5. EBS OPTIMIZATION

| # | What We Need | Details / Filters to Apply |
|---|-------------|---------------------------|
| 5.1 | **GP2 to GP3 Migration Dashboard** | Screenshot showing the 2,056 GP2 volumes identified for migration, with estimated savings. |
| 5.2 | **EBS Snapshot Management** | Screenshot showing: Total Snapshots count, Daily Snapshot Cost, Monthly Snapshot Cost, and any snapshots flagged for removal. |

---

## 6. SAVINGS PLANS & RESERVED INSTANCES

| # | What We Need | Details / Filters to Apply |
|---|-------------|---------------------------|
| 6.1 | **SP/RI Overview Dashboard** | Screenshot showing: Total Reservations, Purchased Units, Resources Covered, Total Reservation Spend. |
| 6.2 | **SP Coverage** | Screenshot clearly showing Savings Plans coverage at **72%** (or current value). |
| 6.3 | **SP Utilization** | Screenshot clearly showing Savings Plans utilization at **98%** (or current value). |
| 6.4 | **Unused Reservation Fees** | Screenshot showing any unused RI fees (e.g., Amazon RDS unused RIs flagged for optimization). |
| 6.5 | **RI Utilization Breakdown** | Per-reservation utilization view showing which RIs are well-utilized vs underutilized. |
| 6.6 | **Before vs After** | If available — any view showing SP coverage improvement from 18% → 72% over the engagement period. |

> **Why this matters:** The auditor will specifically ask about SP coverage (target 70%+) and utilization (target >95%). These numbers must be clearly visible in the screenshots.

---

## 7. ANOMALY DETECTION & BUDGET ALERTS

| # | What We Need | Details / Filters to Apply |
|---|-------------|---------------------------|
| 7.1 | **Anomaly Detection Dashboard** | Screenshot showing: Anomaly Detection Status (enabled/active) and any detected anomalies with timestamps. |
| 7.2 | **Mitigation Status Tracking** | Screenshot showing at least one anomaly with its Mitigation Status (mitigated/pending). This proves we don't just detect — we track resolution. |
| 7.3 | **Budget Alert Configuration** | Screenshot showing configured alerts with: Daily Average Cost, Daily Variance, Forecasted Cost for month, Predicted Cost at end of month. |
| 7.4 | **Alert Routing Configuration** | Screenshot showing alert delivery via both **email** AND **third-party webhook** (ITSM/chat integration). Both channels must be visible. |
| 7.5 | **Sample Anomaly Alert** | If available — a sample alert notification (email or webhook payload) showing an actual anomaly that was detected and routed. |

> **Why this matters:** The SOW requires "route alerts to resource owners within minutes." The auditor will verify both detection AND routing mechanisms are in place.

---

## 8. COST SAVINGS SCORECARD

| # | What We Need | Details / Filters to Apply |
|---|-------------|---------------------------|
| 8.1 | **Cost Savings Opportunity Scorecard** | Overall savings scorecard showing total identified savings, realized savings, and pending opportunities. |
| 8.2 | **Per-Action Savings Breakdown** | If available — savings broken down by optimization type (right-sizing, orphaned cleanup, GP2→GP3, snapshots, SP/RI, S3 lifecycle, scheduling). |

---

## 9. TRAINING & SUPPORT SECTION

| # | What We Need | Details / Filters to Apply |
|---|-------------|---------------------------|
| 9.1 | **Training & Support Page** | Screenshot of the Aquila Cloud "Training & Support" section showing available resources including "FinOps Best Practices" materials. |

> **Why this matters:** This proves our platform includes built-in knowledge transfer resources, supporting our claim of 3 training sessions delivered.

---

## DELIVERY FORMAT

Please provide evidence in the following format:

```
📁 Aquila_Audit_Evidence/
├── 01_Cost_Overview/
│   ├── 1.1_Cost_Overview_Dashboard.png
│   ├── 1.2_Cost_Trend_MoM.png
│   ├── 1.3_Cost_By_Service.png
│   ├── 1.4_Cost_By_Dimension.png
│   ├── 1.5_Spend_Forecast.png
│   └── 1.6_CUR_Integration_Logs.png
├── 02_Tag_Compliance/
│   ├── 2.1_Tag_Compliance_Overview.png
│   ├── 2.2_Coverage_By_Account.png
│   ├── 2.3_Tag_Purity.png
│   ├── 2.4_Tagged_Filter.png
│   ├── 2.4_Untagged_Filter.png
│   ├── 2.5_Drilldown_application_id.png
│   ├── 2.5_Drilldown_cost_center.png
│   ├── 2.5_Drilldown_client_id.png
│   ├── 2.5_Drilldown_environment.png
│   ├── 2.6_Invalid_Tags.png
│   └── 2.7_Before_After_Compliance.png
├── 03_Right_Sizing/
│   ├── 3.1_Recommendations_List.png
│   ├── 3.2_CPU_Utilization_Graphs.png
│   ├── 3.3_IOPS_Graphs.png
│   ├── 3.4_Throughput_Graphs.png
│   └── 3.5_Savings_Summary.png
├── 04_Orphaned_Resources/
│   ├── 4.1_Orphaned_Dashboard.png
│   └── 4.2_Idle_Resource_Details.png
├── 05_EBS_Optimization/
│   ├── 5.1_GP2_to_GP3_Migration.png
│   └── 5.2_Snapshot_Management.png
├── 06_Savings_Plans_RI/
│   ├── 6.1_SPRI_Overview.png
│   ├── 6.2_SP_Coverage_72pct.png
│   ├── 6.3_SP_Utilization_98pct.png
│   ├── 6.4_Unused_RI_Fees.png
│   ├── 6.5_RI_Utilization_Breakdown.png
│   └── 6.6_Before_After_SP.png
├── 07_Anomaly_Detection/
│   ├── 7.1_Anomaly_Dashboard.png
│   ├── 7.2_Mitigation_Status.png
│   ├── 7.3_Budget_Alert_Config.png
│   ├── 7.4_Alert_Routing_Email_Webhook.png
│   └── 7.5_Sample_Alert.png
├── 08_Savings_Scorecard/
│   ├── 8.1_Overall_Scorecard.png
│   └── 8.2_Per_Action_Breakdown.png
└── 09_Training/
    └── 9.1_Training_Support_Page.png
```

---

## PRIORITY ITEMS (Must-Have for Audit)

If time is limited, these are the **absolute must-haves** — the auditor will almost certainly ask for these:

| Priority | Item | Why Critical |
|----------|------|-------------|
| 🔴 P0 | 2.1 Tag Compliance Overview (96%) | Core audit metric — proves governance works |
| 🔴 P0 | 6.2 SP Coverage (72%) | SOW target was 70%+ — must show we met it |
| 🔴 P0 | 6.3 SP Utilization (98%) | SOW target was >95% — must show we met it |
| 🔴 P0 | 7.1 Anomaly Detection Dashboard | Proves proactive cost management |
| 🔴 P0 | 7.4 Alert Routing (email + webhook) | SOW requires "route alerts within minutes" |
| 🟠 P1 | 3.2-3.4 Utilization Graphs (CPU/IOPS) | Proves right-sizing is data-driven |
| 🟠 P1 | 1.2 Cost Trend MoM | Shows cost reduction over time |
| 🟠 P1 | 4.1 Orphaned Resources Dashboard | Shows waste identification |
| 🟠 P1 | 5.1 GP2→GP3 Migration (2,056 volumes) | Specific optimization evidence |
| 🟡 P2 | 1.5 Spend Forecast | Shows predictive capability |
| 🟡 P2 | 8.1 Savings Scorecard | Overall savings summary |
| 🟡 P2 | 9.1 Training & Support Page | Supports knowledge transfer claim |

---

## IMPORTANT NOTES

1. **Date Range:** All screenshots should show data from the **Franciscan Solutions** account during the **July–August 2025** engagement period where possible.
2. **Account Visibility:** Ensure the Franciscan AWS Account IDs or account names are visible in the screenshots (Production, Non-Production, Shared Services).
3. **No PII:** Blur or redact any personally identifiable information if visible.
4. **Resolution:** Screenshots should be high-resolution and readable when zoomed in. PDF format preferred for print quality.
5. **Live Demo:** If the auditor requests a live walkthrough, we may need temporary read-only access to the Franciscan dashboards in Aquila Cloud. Please confirm if this is possible.

---

## CONTACT

If you have questions about any item or need clarification on what exactly the auditor expects, reach out to us immediately. We're happy to jump on a quick call to walk through this list together.

Thank you for your support — this evidence is critical for our AWS Competency audit success.
