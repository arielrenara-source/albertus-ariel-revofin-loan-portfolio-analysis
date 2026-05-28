# albertus-ariel-revofin-loan-portfolio-analysis
Loan portfolio quality analysis for RevoFin (fintech lending) — TKB30 scoring, cohort analysis across 176K loans ($2.80B OS), and 2012 anomaly root cause investigation using BigQuery SQL.

# 💳 RevoFin Loan Portfolio Analysis

**Program:** RevoU Finance Industry Project (FIP)  
**Author:** Albertus Ariel Renara Pranata — Bellatrix, Team 4  
**Tools:** BigQuery (SQL)  
**Dataset:** RevoFin FIP — `customer` & `loan` tables

---

## 📌 Business Background

RevoFin is a fintech lending platform managing a multi-billion dollar loan portfolio across multiple US states and borrower segments. This analysis evaluates portfolio health using industry-standard metrics (TKB30, ENR, OS) and investigates a cohort-level anomaly in the 2012 vintage.

---

## 🎯 Objective

Assess overall loan portfolio quality, identify underperforming cohorts, and determine the root cause of the 2012 anomaly through demographic and financial driver analysis.

---

## 📊 Executive Summary

| Metric | Value |
|--------|-------|
| Total Active Loans | 176,087 |
| Outstanding Balance (OS) | $2.80B |
| Eligible Net Revenue (ENR) | $2.80B |
| TKB30 Quality Score | **98.11% — EXCELLENT** |
| DPD>30 at Risk | $53M (1.89%) |

---

## 🗂️ Dataset Overview

| Table | Rows | Key Columns |
|-------|------|-------------|
| `customer` | ~176K | `customer_id`, `annual_inc`, `home_ownership`, `emp_length`, `addr_state` |
| `loan` | ~176K | `loan_id`, `customer_id`, `funded_amount`, `int_rate`, `loan_status`, `issue_year`, `purpose` |

**Loan Status Classification:**

| Status | Share | DPD>30 | Notes |
|--------|-------|--------|-------|
| Fully Paid | 58.3% | No | Completed — no active risk |
| Current | 37.4% | No | Performing within terms |
| In Grace Period | 1.2% | No | ≤15 days past due |
| Late (31–120 days) | 2.1% | **YES** | Contributes to TKB30 calc |
| Default | 1.0% | **YES** | Written-off, excluded from ENR |

---

## 🔬 Metric Definitions
Outstanding Balance (OS)   = Σ funded_amount (all active loans)
Eligible Net Revenue (ENR) = Σ funded_amount WHERE loan_status ≠ 'Default'
TKB30                      = 1 − (OS_DPD>30 / OS_Total) × 100%
DPD>30                     = Late (31–120 days) + Default

---

## 📈 Key Findings

### 1. TKB30 by Cohort (2012–2019)

| Cohort | Loans | OS ($M) | DPD>30 ($M) | TKB30 | Avg Rate | Assessment |
|--------|-------|---------|-------------|-------|----------|------------|
| 2012 | 1,218 | $21.0 | $0.79 | 96.25% | 18.36% | ⚠️ Anomaly |
| 2013 | 4,183 | $73.7 | $2.30 | 96.88% | 15.20% | Below Avg |
| 2014 | 10,249 | $184.3 | $6.32 | 96.57% | 14.90% | Below Avg |
| 2015 | 22,638 | $416.8 | $10.91 | 97.38% | 13.40% | Average |
| 2016 | 31,574 | $580.4 | $13.51 | 97.67% | 13.10% | Average |
| 2017 | 43,018 | $770.2 | $21.25 | 97.24% | 12.70% | Average |
| 2018 | 37,605 | $544.3 | $7.33 | 98.65% | 12.20% | ✅ Good |
| 2019 | 25,602 | $209.3 | $2.36 | 98.87% | 12.00% | ✅ Best |

**Trend:** Steady improvement from 96.25% (2012) → 98.87% (2019), reflecting tightening underwriting standards.

### 2. 2012 Anomaly Deep-Dive

**Hypothesis tested:** Were lower-quality borrowers responsible?

| Factor | 2012 Cohort | Others Avg | Signal |
|--------|-------------|------------|--------|
| Mortgage Holders | 52.68% | 49.04% | ✅ Better |
| Avg Employment Length | 6.59 yrs | 5.85 yrs | ✅ Better |
| Median Annual Income | $69,000 | $67,000 | ✅ Better |

**Verdict: Demographics are NOT the cause.**

**Root cause identified — Financial Drivers:**

| Driver | 2012 Cohort | Other Cohorts | Gap |
|--------|-------------|---------------|-----|
| Avg Interest Rate | 18.36% | 12.93% | +5.43 pp 🔴 |
| Debt Consolidation Share | 64.29% | 55.15% | +9.14 pp 🔴 |

---

## 💡 Strategic Recommendations

| Priority | Recommendation |
|----------|---------------|
| 🔴 R1 | Implement rate cap policy — flag loans >16% for enhanced underwriting review |
| 🔴 R2 | Strengthen debt consolidation screening — stricter DTI, limit segment share to ≤55% |
| 🟡 R3 | Develop vintage monitoring dashboard — early-warning at cohort TKB30 < 97.5% |
| 🟡 R4 | Leverage strong demographic profiles — preferential pricing for mortgage holders & long-tenured employees |
| 🟢 R5 | Scale 2018–2019 underwriting criteria as new baseline (TKB30 > 98.6%, rates < 12.5%) |

## 📬 Contact

**Albertus Ariel Renara Pranata**  
arielrenara@gmail.com  
[LinkedIn — Ariel Renara](https://www.linkedin.com/in/ariel-renara)
