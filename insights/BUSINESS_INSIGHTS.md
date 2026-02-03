# Loan Analytics Dashboard - Business Insights & Strategy

## 📊 Project Overview

This **Loan Analytics Dashboard** is a comprehensive business intelligence solution designed to analyze loan performance, assess portfolio risk, and drive strategic decision-making. The dashboard transforms raw loan data into **actionable insights** through three interconnected analytical views, each serving distinct business needs.

**Project Objective:**
Provide stakeholders with real-time visibility into loan performance, repayment behavior, risk distribution, and growth trends using interactive dashboards and dynamic KPI metrics.

---

## 🎯 Business Objectives

### Primary Goals
1. **Portfolio Health Assessment** - Monitor overall loan portfolio performance and quality
2. **Risk Management** - Identify and track loan quality deterioration and default patterns
3. **Performance Tracking** - Measure month-to-date and month-over-month growth metrics
4. **Trend Analysis** - Identify temporal patterns and geographic/categorical trends
5. **Detail Investigation** - Enable granular, record-level drill-down for audit and exception analysis

---

## 📈 Dashboard Architecture

### Three-Tier Dashboard Structure

```
┌─────────────────────────────────────────────────┐
│      LOAN ANALYTICS DASHBOARD ECOSYSTEM        │
├─────────────────────────────────────────────────┤
│                                                 │
│  [Summary] → [Overview] → [Detail]             │
│     ↓           ↓            ↓                 │
│   Snapshot   Trends      Granular              │
│   & KPIs     & Patterns   Analysis             │
│                                                 │
└─────────────────────────────────────────────────┘
```

---

## 1️⃣ Loan Summary Dashboard

### Strategic Purpose
Provides **executive-level snapshot** of portfolio health, allowing leadership to assess overall performance at a glance.

### Core KPIs with Time Intelligence

#### Financial Metrics (with MTD & MoM Tracking)
| KPI | Formula | Business Value |
|-----|---------|-----------------|
| **Total Loan Applicants** | COUNT of loan records | Measures application volume and growth |
| **Total Funded Amount** | SUM(loan_amount) | Total capital deployed in loans |
| **Total Received Amount** | SUM(received_amount) | Cash collected from borrowers |
| **Average DTI** | AVG(debt_to_income) | Measures borrower leverage & affordability |
| **Average Interest Rate** | AVG(interest_rate) | Portfolio yield & risk assessment |

#### MTD Calculations
Each metric includes **Month-to-Date (MTD)** values showing cumulative performance from month start:
```
MTD_loan_amount = CALCULATE(TOTALMTD([Total_Funded_Amount],'Date Table'[Date]))
```
**Business Use:** Tracking progress toward monthly targets and comparing current pace to historical averages.

#### MoM Growth Analysis
Month-over-Month percentage change reveals **growth trends and momentum**:
```
MOM_loan_amount = DIVIDE(
    [MTD_loan_amount] - [PMTD_LOAN_AMOUNT],
    [PMTD_LOAN_AMOUNT]
)
```
**Business Use:** Identifying acceleration/deceleration in loan origination and funding.

### Loan Quality Breakdown

**Good Loan vs Bad Loan Analysis:**
- **Good Loan %** - Percentage of loans performing as expected
- **Bad Loan %** - Percentage of loans in default or charged-off status

```
Good loan % = CALCULATE([Total_Loan_Applicants],
    loan_data[Good vs Bad loan]="Good Loan") / [Total_Loan_Applicants]

Bad loan % = CALCULATE([Total_Loan_Applicants],
    loan_data[Good vs Bad loan]="BAd Loan") / [Total_Loan_Applicants]
```

**Business Insight:** A high bad loan percentage indicates portfolio risk and potential future losses.

### Loan Status Classification

Loans are categorized into three states:

| Status | Meaning | Risk Level | Action |
|--------|---------|-----------|--------|
| **Fully Paid** | Loan completed successfully | ✅ Low | N/A - Revenue realized |
| **Current** | Loan in good standing | ✅ Low | Continue monitoring |
| **Charged Off** | Loan defaulted/written off | ❌ High | Recovery/Loss management |

**Portfolio Health Score:**
```
Portfolio Health % = [Fully Paid] + [Current] / [Total Loans]
```
Higher scores indicate better portfolio quality and lower default risk.

### Key Visuals & Insights

| Visual | Purpose | Insight Type |
|--------|---------|--------------|
| KPI Cards with MoM % | Quick metric scan | Executive summary |
| Gauge Charts | Progress vs. targets | Performance tracking |
| Donut Charts (Good/Bad) | Loan quality breakdown | Risk assessment |
| Stacked Bar (Loan Status) | Status distribution | Portfolio composition |

---

## 2️⃣ Loan Overview Dashboard

### Strategic Purpose
Identifies **trends, patterns, and opportunities** across time periods, geographic regions, and loan categories.

### Temporal Analysis

**Month-over-Month Growth Trends:**
```
Line Chart: [MTD_loan_amount] by Month
- Shows funding velocity over time
- Identifies seasonal patterns
- Reveals growth acceleration/deceleration
```

**Business Insights:**
- **Upward trend** → Strong market demand, successful origination efforts
- **Downward trend** → Market saturation, competitive pressure, or process issues
- **Cyclical pattern** → Seasonal factors (e.g., funding spikes in Q4)

### Geographic Performance Analysis

**Loan Distribution by Location (Map View):**
- Identifies geographic concentration risk
- Reveals high-performing regions
- Highlights underpenetrated markets

**Business Questions Answered:**
- Which states have highest loan volume?
- Are there geographic quality differences (good vs. bad loans)?
- Should we expand in certain regions or consolidate efforts?

### Category-Based Analysis

**Tree Map: KPI Performance by Loan Purpose:**
- **Loan Purposes:** Auto refinance, Credit card consolidation, Education, etc.
- **Size represents:** Volume or funded amount
- **Color represents:** Good loan percentage or average interest rate

**Business Insights:**
- Identify highest-margin loan products
- Find products with quality issues
- Optimize product mix for profitability

### Interactive Filtering Strategy

**Dynamic Measure Selection:**
```dax
Select Measure = {
    ("Total_Loan_Applicants", NAMEOF('loan_data'[Total_Loan_Applicants]), 0),
    ("Total_Funded_Amount", NAMEOF('loan_data'[Total_Funded_Amount]), 1),
    ("Total Received Amount", NAMEOF('loan_data'[Total Received Amount]), 2)
}
```

**Enables:** Users can toggle between metrics without changing dashboard layout, reducing clutter and improving UX.

---

## 3️⃣ Loan Detail Dashboard

### Strategic Purpose
Enables **granular record-level analysis** for audit, exception investigation, and detailed reporting.

### Tabular Analysis Fields

| Field | Analysis Purpose |
|-------|------------------|
| **Loan ID** | Unique identification for tracking |
| **Issue Date** | Timeline filtering and cohort analysis |
| **Grade** | Credit quality classification (A-G) |
| **Purpose** | Loan category and product analysis |
| **Funded Amount** | Individual loan size assessment |
| **Interest Rate** | Risk-adjusted pricing evaluation |
| **Installment Amount** | Monthly payment size & borrower burden |
| **Received Amount** | Actual repayment collected |
| **Good vs Bad** | Performance classification |

### Analytical Use Cases

**1. Exception Investigation**
```
Question: Why did this particular loan default?
Process: Filter by [Bad Loan], examine [Interest Rate], [DTI], [Grade]
Action: Identify if pricing was adequate or underwriting was weak
```

**2. Cohort Analysis**
```
Question: How did loans from Q3 2024 perform?
Process: Filter by [Issue Date], analyze [Good vs Bad], [Received Amount]
Action: Assess whether specific origination period had quality issues
```

**3. Risk Segmentation**
```
Question: Which high-grade loans have payment issues?
Process: Filter by [Grade] (A-C), analyze [Received Amount] trend
Action: Identify grade deterioration or origination/servicing issues
```

**4. Product Performance Deep Dive**
```
Question: How profitable is the auto refinance product?
Process: Filter by [Purpose]="Auto Refinance", compare [Interest Rate] to [Charged Off] rate
Action: Optimize pricing and risk management for this product line
```

---

## 🔑 Key Performance Indicators (KPIs) - Comprehensive View

### Financial KPIs

| KPI | Target | Threshold | Business Impact |
|-----|--------|-----------|-----------------|
| **Total Funded Amount** | Growth MoM | $1M+ monthly | Revenue generation |
| **Total Received Amount** | 95%+ of funded | Collection efficiency | Cash flow & profitability |
| **Avg DTI Ratio** | <35% | <40% | Borrower affordability |
| **Avg Interest Rate** | >8% | >6% | Portfolio yield |

### Quality KPIs

| KPI | Target | Threshold | Business Impact |
|-----|--------|-----------|-----------------|
| **Good Loan %** | >85% | >75% | Portfolio safety |
| **Bad Loan %** | <15% | <25% | Default risk |
| **Fully Paid %** | >30% | >20% | Repayment success |
| **Charged Off %** | <5% | <10% | Loss rate |

### Growth KPIs

| KPI | Target | Interpretation | Action |
|-----|--------|-----------------|--------|
| **MoM Growth** | >10% | Accelerating originations | Scale operations |
| **MoM Decline** | <-5% | Market pressure | Investigate root cause |
| **Flat Growth** | 0-5% | Stable but stagnant | Innovate new channels |

---

## 📊 Data Architecture & DAX Foundation

### Date Table - Time Intelligence Backbone

```dax
Date Table = 
CALENDAR(
    MIN(loan_data[issue_date]),
    MAX(loan_data[issue_date])
)
```

**Purpose:** Enables all time-based calculations (MTD, MoM, YTD, etc.)

**Technical Importance:**
- Must be marked as official date table in Power BI
- Creates continuous date range (no gaps)
- Supports TOTALMTD(), DATEADD(), and advanced time functions

### Measure Design Pattern

All KPIs follow a hierarchical pattern:

```
1. Base Measure (simple aggregation)
   └─ Total_Funded_Amount = SUM(loan_data[loan_amount])

2. Time-Intelligent Measure (MTD)
   └─ MTD_loan_amount = CALCULATE(TOTALMTD([Base], 'Date'[Date]))

3. Comparative Measure (PMTD)
   └─ PMTD_LOAN_AMOUNT = CALCULATE([Base], DATESMTD(DATEADD(...)))

4. Growth Metric (MoM %)
   └─ MOM_loan_amount = DIVIDE([MTD] - [PMTD], [PMTD])
```

This pattern ensures:
- Consistency across all KPIs
- Scalability for new metrics
- Easy maintenance and updates

---

## 💡 Business Insights & Observations

### Portfolio Performance Indicators

**1. Loan Quality Trend**
- Monitor good loan % monthly
- If declining >5% MoM → Origination quality issue
- If improving → Effective risk management

**2. Funding Acceleration**
- Increasing MTD with positive MoM → Strong market momentum
- Use to forecast quarterly funding targets

**3. Collection Efficiency**
- Compare [Total Received] to [Total Funded]
- Track monthly collection rates
- Identify slowdown in repayment patterns

**4. Product Mix Profitability**
- High interest rate + low bad loan % = Ideal products
- Low interest rate + high bad loan % = Needs repricing

### Risk Management Signals

| Signal | Severity | Response |
|--------|----------|----------|
| Bad Loan % > 20% | 🔴 High | Review underwriting standards |
| Charged Off increasing MoM | 🟠 Medium | Enhance servicing, contact borrowers |
| Avg DTI > 40% | 🟠 Medium | Tighten origination criteria |
| Regional concentration > 30% | 🟡 Low | Diversify geographically |

---

## 🎯 Dashboard Usage Scenarios

### Executive Scenario
**User:** Chief Financial Officer
```
Dashboard: Summary Dashboard
Query: "Is our March MTD on track?"
Action: 
  1. Check Total_Funded_Amount MTD vs. target
  2. View MoM % to assess pace vs. last month
  3. Review Good Loan % for quality assurance
Decision: Approve/adjust monthly targets
```

### Operations Scenario
**User:** Loan Operations Manager
```
Dashboard: Overview Dashboard
Query: "Which regions are underperforming?"
Action:
  1. View geographic map of loan distribution
  2. Compare good loan % by region
  3. Analyze trends for problem regions
Decision: Allocate resources or adjust strategy by region
```

### Risk Scenario
**User:** Credit Risk Manager
```
Dashboard: Detail Dashboard
Query: "Which loans became bad recently?"
Action:
  1. Filter for Recent Issue Date + Bad Loan status
  2. Analyze Grade, DTI, Interest Rate of bad loans
  3. Compare to fully paid loans in same cohort
Decision: Adjust underwriting model or repricing strategy
```

---

## 📈 KPI Formulas Summary

### Base Measures
```dax
Total_Loan_Applicants = COUNTA(loan_data[Loan ID])
Total_Funded_Amount = SUM(loan_data[loan_amount])
Total Received Amount = SUM(loan_data[received_amount])
```

### Time Intelligence Measures
```dax
MTD_loan_amount = CALCULATE(TOTALMTD([Total_Funded_Amount],'Date Table'[Date]))

PMTD_LOAN_AMOUNT = CALCULATE([Total_Funded_Amount], 
    DATESMTD(DATEADD('Date Table'[Date], -1, MONTH)))

MOM_loan_amount = DIVIDE([MTD_loan_amount] - [PMTD_LOAN_AMOUNT], 
    [PMTD_LOAN_AMOUNT], 0)
```

### Quality Measures
```dax
Good loan % = CALCULATE([Total_Loan_Applicants],
    loan_data[Good vs Bad loan]="Good Loan") / [Total_Loan_Applicants]

Bad loan % = CALCULATE([Total_Loan_Applicants],
    loan_data[Good vs Bad loan]="BAd Loan") / [Total_Loan_Applicants]
```

### Dynamic Selection
```dax
Select Measure = {
    ("Total_Loan_Applicants", NAMEOF('loan_data'[Total_Loan_Applicants]), 0),
    ("Total_Funded_Amount", NAMEOF('loan_data'[Total_Funded_Amount]), 1),
    ("Total Received Amount", NAMEOF('loan_data'[Total Received Amount]), 2)
}
```

---

## 🔄 Data Flow & Refresh Strategy

```
Source Data (Loan_Data)
    ↓
Date Table (CALENDAR)
    ↓
Base Measures (SUM, COUNT, AVG)
    ↓
Time-Intelligent Measures (MTD, PMTD)
    ↓
Growth Metrics (MoM %)
    ↓
Dashboard Visualizations
    ↓
Business Insights
```

**Refresh Recommendation:** Daily refresh to ensure MTD metrics are current and accurate for decision-making.

---

## 🚀 Business Value Delivered

### Operational Benefits
- ✅ **Real-time visibility** into portfolio performance
- ✅ **Automated trend detection** to catch issues early
- ✅ **Geographic insights** for strategic expansion
- ✅ **Product performance** analysis for optimization

### Financial Benefits
- ✅ **Improved collection** through early bad loan detection
- ✅ **Optimized pricing** based on product & risk analysis
- ✅ **Reduced losses** through better risk management
- ✅ **Revenue growth** tracking and forecasting

### Strategic Benefits
- ✅ **Data-driven decision making** at all levels
- ✅ **Competitive insight** through market trend analysis
- ✅ **Risk mitigation** with early warning indicators
- ✅ **Scalability** through automated KPI framework

---

## 📋 Implementation Checklist

- ✅ Create Date Table with complete date range
- ✅ Build base measures for all core metrics
- ✅ Implement MTD and PMTD calculations
- ✅ Calculate growth rates (MoM)
- ✅ Create quality percentage measures
- ✅ Build Summary Dashboard with KPI cards
- ✅ Create Overview Dashboard with trends and maps
- ✅ Develop Detail Dashboard with drill-down capability
- ✅ Add interactive slicers for dynamic filtering
- ✅ Implement dynamic measure selector
- ✅ Schedule daily data refresh
- ✅ Train stakeholders on dashboard usage

---

## 📚 Documentation References

- [DAX_QUERIES_DOCUMENTATION.md](DAX_QUERIES_DOCUMENTATION.md) - Technical measure specifications
- Power BI File: `Loan_Analytics_Dashboard.pbix`

---

## 📞 Key Stakeholders

| Role | Primary Dashboard | KPIs of Interest |
|------|-------------------|------------------|
| **CFO** | Summary | Total Funded, MoM %, Received Amount |
| **Loan Manager** | Overview | Regional distribution, Trends |
| **Risk Manager** | Detail | Bad Loan %, Charged Off rate |
| **Product Manager** | Overview | KPIs by loan purpose |
| **Operations** | Summary | Total Applicants, MTD metrics |

---

## 🎓 Key Learnings & Best Practices

1. **Time Intelligence Foundation** - A well-designed Date Table is essential for all comparisons
2. **Consistent Patterns** - Reusable measure formulas reduce errors and ensure scalability
3. **Progressive Disclosure** - Three-dashboard approach matches user needs (summary → trends → details)
4. **Dynamic Filtering** - Measure selectors and slicers empower users without cluttering visuals
5. **Quality Metrics** - Good/Bad loan tracking is critical for risk management

---

**Last Updated:** February 3, 2026  


