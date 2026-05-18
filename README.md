# 📊 Microsoft 365 Licensing Dashboard

> A Power BI operational dashboard for enterprise Microsoft 365 license governance — tracking consumption, compliance, and security posture across organizational units.

---

## Overview

This project is an enterprise **Microsoft 365 Licensing & Compliance Dashboard** built for IT administrators and license managers to monitor subscription utilization, detect over-limit SKUs, enforce compliance standards, and audit identity risks in real time.

The solution combines a live semantic model with a two-page command center layout — an **Executive Summary** for license health and consumption, and a **Compliance & Audit** view for identity governance and account hygiene.

> ⚠️ **All data is synthetic and anonymized.** No real organizational, user, or identity data is included. This project is strictly for portfolio and demonstration purposes.

---

## Objectives

| Goal | Description |
|------|-------------|
| **License Optimization** | Monitor prepaid vs. consumed licenses to prevent overage and waste |
| **Overage Alerting** | Surface SKUs exceeding allocation with actionable reconciliation prompts |
| **Compliance Enforcement** | Track MFA adoption, inactive accounts, and external identity exposure |
| **Audit Readiness** | Provide a filterable audit list of external and inactive accounts for IT review |

---

## Dashboard Pages

### Page 1 — Executive Summary

Provides a high-level snapshot of license health across the tenant.
<img width="1325" height="784" alt="Gemini_Generated_Image_obagdpobagdpobag" src="https://github.com/user-attachments/assets/57194424-4d41-4899-9b7a-f4829f35169d" />


**KPI Cards:**
- Total Prepaid licenses
- Total Consumed licenses
- Availability %
- Over-Limit SKU count

**Visuals:**
- **SKU Health Overview** — horizontal bar chart color-coded by status (green = healthy, amber = at capacity, red = over-limit)
- **Top Consuming Groups** — ranked bar chart showing license consumption by organizational unit
- **Action Required Banner** — dynamic alert listing all SKUs in overage with license shortfall counts

---

### Page 2 — Compliance & Audit

Supports identity governance and account remediation workflows.
<img width="1385" height="768" alt="Gemini_Generated_Image_8ii5sz8ii5sz8ii5 (1)" src="https://github.com/user-attachments/assets/6114e85a-b6c4-480b-9be1-58ca5039fc96" />



**KPI Cards:**
- Inactive Users (90-day threshold)
- External Identities
- Compliant Accounts
- Suspended Accounts

**Visuals:**
- **Compliance Health Check** — horizontal bar chart tracking Licensed Active %, MFA Success %, Guest Review %, and Inactive Remediation %
- **External & Inactive Accounts Audit List** — tabular audit log with Display Name, Title, Last Sign-in, Account Status, and Identity Flag (External / Inactive 90d), color-coded for rapid triage

---

## Architecture

```
Microsoft 365 Tenant (Graph API / Admin Export)
        ↓
Power Automate — Scheduled Data Extraction & Orchestration
        ↓
License & Identity Data Ingestion
        ↓
Power Query — Transformation & Classification
        ↓
Power BI Semantic Model (Star Schema)
        ↓
Executive Summary  |  Compliance & Audit
```

---

## Key DAX Measures

**Availability %**
```dax
Availability% =
DIVIDE([Total Prepaid] - [Total Consumed], [Total Prepaid], 0)
```

**Over-Limit SKUs**
```dax
Over-Limit SKUs =
COUNTROWS(
    FILTER(
        SKU_Summary,
        SKU_Summary[Consumed] > SKU_Summary[Prepaid]
    )
)
```

**Overage Alert Message**
```dax
Overage Alert =
VAR OverLimitSKUs = FILTER(SKU_Summary, [Consumed] > [Prepaid])
RETURN
    "Action Required: Significant overages detected in " &
    CONCATENATEX(OverLimitSKUs, [SKU_Name] & " (" & [Overage] & " licenses)", ", ") &
    ". Immediate reconciliation or SKU upgrade required."
```

**Inactive Remediation %**
```dax
Inactive Remediation% =
DIVIDE(
    CALCULATE(COUNTROWS(Users), Users[Status] = "Remediated"),
    CALCULATE(COUNTROWS(Users), Users[InactiveDays] >= 90),
    0
)
```

---

## Data Pipeline & Model Schema

| Phase | Tool / Asset | Applied Logic |
|-------|-------------|---------------|
| Data Source | M365 Admin Export / Synthetic CSV | License SKU data, user identity records |
| Transformation | Power Query | SKU classification, overage flagging, identity type parsing |
| Semantic Model | Power BI | Star schema — Users, SKUs, Groups, Dates |
| Alerting Logic | DAX | Dynamic banner generation for overages |
| UI Design | Power BI Theme | Clean light-mode enterprise UI with semantic color coding |

---

## Color Logic & Status Encoding

| Color | Meaning |
|-------|---------|
| 🟢 Green | Healthy — within license limits |
| 🟠 Amber | At capacity — monitor closely |
| 🔴 Red | Over-limit or compliance risk |
| 🔵 Blue | Informational / suspended status |

---

## Skills Demonstrated

- Enterprise license governance modeling in Power BI
- Identity & compliance KPI design (MFA, External Identity, Inactive Users)
- Dynamic DAX alerting with CONCATENATEX and FILTER
- UX design for IT operations — clear status encoding, audit-ready tables
- Power Query data classification and transformation pipelines

---

## Data Privacy & Compliance

- All user accounts, license counts, and organizational data are **synthetic**
- No real Microsoft tenant, user identity, or email data is included
- Designed strictly for **portfolio and demonstration purposes**

---
