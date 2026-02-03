# 📊 Loan Analytics Dashboard

A comprehensive business intelligence solution for analyzing loan performance, assessing portfolio risk, and driving strategic decision-making. This project transforms raw loan data into **actionable insights** through interactive dashboards and dynamic KPI metrics.

**Live Dashboard:** Power BI file included in this repository

---

## 🎯 Project Overview

### Objective
Provide stakeholders with real-time visibility into loan performance, repayment behavior, risk distribution, and growth trends using interactive dashboards.

### Key Features
- 📈 **Executive Summary Dashboard** - KPI snapshot and portfolio health metrics
- 📊 **Overview Dashboard** - Trends, geographic analysis, and category performance
- 🔍 **Detail Dashboard** - Granular record-level analysis for audit and investigations
- ⏰ **Time Intelligence** - Month-to-date (MTD) and month-over-month (MoM) tracking
- 📍 **Geographic Analysis** - Regional loan distribution and performance
- 💰 **Financial KPIs** - Funded amount, received amount, interest rates, DTI metrics
- ⚠️ **Risk Management** - Bad loan percentage, default tracking, quality metrics

---

## 📋 Primary Business Objectives

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
Executive-level snapshot of portfolio health for leadership assessment.

### Core KPIs

| KPI | Formula | Business Value |
|-----|---------|-----------------|
| **Total Loan Applicants** | COUNT of loan records | Application volume and growth measurement |
| **Total Funded Amount** | SUM(loan_amount) | Total capital deployed in loans |
| **Total Received Amount** | SUM(received_amount) | Cash collected from borrowers |
| **Average DTI** | AVG(debt_to_income) | Borrower leverage & affordability assessment |
| **Average Interest Rate** | AVG(interest_rate) | Portfolio yield & risk assessment |

### Time Intelligence Features

- **MTD (Month-to-Date):** Cumulative performance from month start
- **MoM (Month-over-Month):** Growth percentage comparing current and previous month
- **Loan Quality Breakdown:** Good loan % vs. Bad loan %
- **Loan Status Classification:** Fully Paid, Current, Charged Off

### Loan Status Categories

| Status | Meaning | Risk Level |
|--------|---------|-----------|
| **Fully Paid** | Loan completed successfully | ✅ Low |
| **Current** | Loan in good standing | ✅ Low |
| **Charged Off** | Loan defaulted/written off | ❌ High |

---

## 2️⃣ Loan Overview Dashboard

### Strategic Purpose
Identifies trends, patterns, and opportunities across time periods, regions, and categories.

### Key Analytical Views

**Temporal Analysis**
- Month-over-month funding trends
- Seasonal pattern identification
- Growth acceleration/deceleration tracking

**Geographic Performance**
- State-level loan distribution
- Regional quality comparisons
- Market penetration analysis

**Category Analysis**
- Loan purpose breakdown
- Product profitability assessment
- Loan type performance comparison

**Interactive Features**
- Dynamic measure selection (toggle between key metrics)
- Multi-dimensional filtering
- Drill-down capabilities

---

## 3️⃣ Loan Detail Dashboard

### Strategic Purpose
Granular record-level analysis for audit, investigation, and detailed reporting.

### Analysis Fields

| Field | Purpose |
|-------|---------|
| **Loan ID** | Unique identification |
| **Issue Date** | Timeline and cohort analysis |
| **Grade** | Credit quality classification (A-G) |
| **Purpose** | Loan category and product type |
| **Funded Amount** | Individual loan size |
| **Interest Rate** | Risk-adjusted pricing |
| **Installment** | Monthly payment size |
| **Received Amount** | Repayment collected |
| **Good vs Bad** | Performance classification |

### Use Cases

**Exception Investigation** - Analyze specific loan defaults
**Cohort Analysis** - Compare performance by origination period
**Risk Segmentation** - Evaluate grade-based performance patterns
**Product Deep Dive** - Assess profitability by loan type

---

## 🔑 Key Performance Indicators

### Financial KPIs

| KPI | Target | Threshold | Impact |
|-----|--------|-----------|--------|
| **Total Funded Amount** | Growth MoM | $1M+ monthly | Revenue generation |
| **Total Received Amount** | 95%+ collected | Collection efficiency | Cash flow |
| **Avg DTI Ratio** | <35% | <40% | Borrower affordability |
| **Avg Interest Rate** | >8% | >6% | Portfolio yield |

### Quality KPIs

| KPI | Target | Threshold | Impact |
|-----|--------|-----------|--------|
| **Good Loan %** | >85% | >75% | Portfolio safety |
| **Bad Loan %** | <15% | <25% | Default risk |
| **Fully Paid %** | >30% | >20% | Repayment success |
| **Charged Off %** | <5% | <10% | Loss rate |

### Growth KPIs

| KPI | Interpretation | Action |
|-----|-----------------|--------|
| **MoM Growth >10%** | Accelerating originations | Scale operations |
| **MoM Decline <-5%** | Market pressure | Investigate causes |
| **Flat Growth 0-5%** | Stable but stagnant | Innovate channels |

---

## 📊 DAX Formulas & Measures

### Base Measures

```dax
Total_Loan_Applicants = COUNTA(loan_data[Loan ID])
Total_Funded_Amount = SUM(loan_data[loan_amount])
Total Received Amount = SUM(loan_data[received_amount])
Average_DTI = AVERAGE(loan_data[debt_to_income])
Average_Interest_Rate = AVERAGE(loan_data[interest_rate])
```

### Time Intelligence Measures

```dax
-- Month-to-Date Calculation
MTD_loan_amount = CALCULATE(TOTALMTD([Total_Funded_Amount],'Date Table'[Date]))

-- Previous Month-to-Date
PMTD_LOAN_AMOUNT = 
CALCULATE(
    [Total_Funded_Amount],
    DATESMTD(DATEADD('Date Table'[Date], -1, MONTH))
)

-- Month-over-Month Growth
MOM_loan_amount = DIVIDE(
    [MTD_loan_amount] - [PMTD_LOAN_AMOUNT],
    [PMTD_LOAN_AMOUNT],
    0
)
```

### Quality Measures

```dax
Good loan % = CALCULATE([Total_Loan_Applicants],
    loan_data[Good vs Bad loan]="Good Loan") / [Total_Loan_Applicants]

Bad loan % = CALCULATE([Total_Loan_Applicants],
    loan_data[Good vs Bad loan]="BAd Loan") / [Total_Loan_Applicants]
```

### Date Table

```dax
Date Table = 
CALENDAR(
    MIN(loan_data[issue_date]),
    MAX(loan_data[issue_date])
)
```

**Requirements:**
- Mark as official date table in Power BI
- Must cover entire data range
- Supports TOTALMTD(), DATEADD(), and time functions

### Dynamic Measure Selection

```dax
Select Measure = {
    ("Total_Loan_Applicants", NAMEOF('loan_data'[Total_Loan_Applicants]), 0),
    ("Total_Funded_Amount", NAMEOF('loan_data'[Total_Funded_Amount]), 1),
    ("Total Received Amount", NAMEOF('loan_data'[Total Received Amount]), 2)
}
```

---

## 🔄 DAX Pattern Guidelines

### Pattern 1: Base Measure
```dax
[New_KPI_Base] = SUM(loan_data[column_name])
```

### Pattern 2: MTD Calculation
```dax
[New_KPI_MTD] = CALCULATE(TOTALMTD([New_KPI_Base],'Date Table'[Date]))
```

### Pattern 3: PMTD Comparison
```dax
[New_KPI_PMTD] = 
CALCULATE(
    [New_KPI_Base],
    DATESMTD(DATEADD('Date Table'[Date], -1, MONTH))
)
```

### Pattern 4: Growth Rate
```dax
[New_KPI_MOM] = DIVIDE(
    [New_KPI_MTD] - [New_KPI_PMTD],
    [New_KPI_PMTD],
    0
)
```

### Pattern 5: Category Percentage
```dax
[Category_Percentage] = 
CALCULATE(
    [Base_Measure],
    table[category_column]="desired_category"
) / [Base_Measure]
```

---

## 💡 Business Insights & Risk Signals

### Portfolio Performance Indicators

| Indicator | Action |
|-----------|--------|
| Good Loan % declining >5% MoM | Review underwriting standards |
| Increasing MTD with positive MoM | Strong market momentum - scale operations |
| Charged Off rate increasing MoM | Enhance servicing and borrower contact |
| High geographic concentration | Diversify geographically |

### Risk Management Framework

| Signal | Severity | Response |
|--------|----------|----------|
| Bad Loan % > 20% | 🔴 High | Review underwriting standards |
| Charged Off increasing MoM | 🟠 Medium | Enhance servicing, contact borrowers |
| Avg DTI > 40% | 🟠 Medium | Tighten origination criteria |
| Regional concentration > 30% | 🟡 Low | Diversify geographically |

---

## 🎓 Best Practices & Key Learnings

1. **Time Intelligence Foundation** - A well-designed Date Table is essential for all comparisons
2. **Consistent Patterns** - Reusable measure formulas reduce errors and ensure scalability
3. **Progressive Disclosure** - Three-dashboard approach matches user needs
4. **Dynamic Filtering** - Measure selectors and slicers empower users without clutter
5. **Case Sensitivity** - DAX strings are case-sensitive ("BAd Loan" ≠ "Bad Loan")
6. **Division by Zero** - Always use DIVIDE() with error handling
7. **Format Consistency** - Apply appropriate formatting (%, currency) for clarity

---

## ⚠️ Common DAX Mistakes to Avoid

1. **Missing Date Table Relationship** - Verify relationship between loan_data and Date Table
2. **Case Sensitivity Issues** - Match exact column values in your data
3. **Division by Zero** - Use DIVIDE() with third parameter for error handling
4. **Context Dependency** - Test measures with different slicers applied
5. **Performance Issues** - Avoid nested CALCULATE() statements; optimize DATEADD() usage

---

## 🎯 Dashboard Usage Scenarios

### Executive (CFO)
- **Dashboard:** Summary
- **Query:** "Is March MTD on track?"
- **Actions:** Check funding amount vs. target, assess quality, approve adjustments

### Operations Manager
- **Dashboard:** Overview
- **Query:** "Which regions are underperforming?"
- **Actions:** View geographic distribution, compare regional quality, adjust strategy

### Credit Risk Manager
- **Dashboard:** Detail
- **Query:** "Which loans became bad recently?"
- **Actions:** Filter by recent bad loans, analyze grade/DTI/rate, adjust underwriting

---

## 📁 Project Structure

```
loan-dashboard/
├── README.md                           # This file
├── BUSINESS_INSIGHTS.md                # Detailed business analysis and insights
├── DAX_QUERIES_DOCUMENTATION.md        # Technical DAX formulas and patterns
├── Loan_Analytics_Dashboard.pbix       # Power BI dashboard file
└── data/
    └── loan_data.csv                   # Source loan data (example)
```

---

## 🚀 Business Value Delivered

### Operational Benefits
- ✅ Real-time visibility into portfolio performance
- ✅ Automated trend detection to catch issues early
- ✅ Geographic insights for strategic expansion
- ✅ Product performance analysis for optimization

### Financial Benefits
- ✅ Improved collection through early bad loan detection
- ✅ Optimized pricing based on product & risk analysis
- ✅ Reduced losses through better risk management
- ✅ Revenue growth tracking and forecasting

### Strategic Benefits
- ✅ Data-driven decision making at all levels
- ✅ Competitive insight through market trend analysis
- ✅ Risk mitigation with early warning indicators
- ✅ Scalability through automated KPI framework

---

## 📊 Data Flow

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
Business Insights & Decisions
```

**Recommended Refresh:** Daily to ensure MTD metrics are current for decision-making

---

## 📞 Key Stakeholders

| Role | Primary Dashboard | Key KPIs |
|------|-------------------|----------|
| **CFO** | Summary | Total Funded, MoM %, Received Amount |
| **Loan Manager** | Overview | Regional distribution, Trends |
| **Risk Manager** | Detail | Bad Loan %, Charged Off rate |
| **Product Manager** | Overview | KPIs by loan purpose |
| **Operations** | Summary | Total Applicants, MTD metrics |

---

## 📚 Documentation Files

- **[BUSINESS_INSIGHTS.md](BUSINESS_INSIGHTS.md)** - Complete business strategy, KPI analysis, and dashboard usage scenarios
- **[DAX_QUERIES_DOCUMENTATION.md](DAX_QUERIES_DOCUMENTATION.md)** - Technical measure specifications and DAX formula patterns
- **Loan_Analytics_Dashboard.pbix** - Power BI file with all visualizations

---

## ✅ Implementation Checklist

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

## 🔧 Technical Requirements

- **Power BI Desktop** (Latest version recommended)
- **Loan Data Source** - CSV or database with required columns:
  - Loan ID
  - Issue Date
  - Loan Amount
  - Received Amount
  - Interest Rate
  - DTI (Debt-to-Income)
  - Grade (Credit Grade)
  - Purpose (Loan Category)
  - Loan Status (Fully Paid, Current, Charged Off)
  - Good vs Bad Loan classification

---

## 📈 Key Metrics at a Glance

| Metric | Current | Target | Status |
|--------|---------|--------|--------|
| Total Funded Amount (MTD) | - | $1M+ | - |
| Good Loan % | - | >85% | - |
| Bad Loan % | - | <15% | - |
| Avg DTI | - | <35% | - |
| Avg Interest Rate | - | >8% | - |
| Collection Rate | - | 95%+ | - |

---

## 🤝 Contributing

To add new KPIs or improve the dashboard:

1. Follow DAX pattern guidelines outlined in this documentation
2. Test all measures with various filter contexts
3. Document new measures in DAX_QUERIES_DOCUMENTATION.md
4. Update business insights if new patterns emerge
5. Schedule testing during off-peak hours

---

## 📞 Support & Questions

For questions about:
- **Business Strategy:** See [BUSINESS_INSIGHTS.md](BUSINESS_INSIGHTS.md)
- **DAX Formulas:** See [DAX_QUERIES_DOCUMENTATION.md](DAX_QUERIES_DOCUMENTATION.md)
- **Dashboard Usage:** Refer to Dashboard Usage Scenarios section above

---

## 📅 Last Updated
February 3, 2026

---

**Built with ❤️ for data-driven decision making**
