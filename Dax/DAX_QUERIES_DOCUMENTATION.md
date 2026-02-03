# Loan Dashboard DAX Queries Documentation

## Overview
This document provides a comprehensive guide to the DAX queries used in the Loan Dashboard. It includes essential measures, calculated tables, and common patterns for KPI development.

---

## Table of Contents
1. [Core Measures](#core-measures)
2. [Time Intelligence Measures](#time-intelligence-measures)
3. [Loan Quality Measures](#loan-quality-measures)
4. [Date Table](#date-table)
5. [Dynamic Measure Selection](#dynamic-measure-selection)
6. [Pattern Guidelines](#pattern-guidelines)

---

## Core Measures

### Total_Funded_Amount
Calculates the total loan amount funded across all records.

```dax
Total_Funded_Amount = SUM(loan_data[loan_amount])
```

**Purpose:** Base measure for all funded amount calculations.

**Usage:** Use this as the foundation for time-based calculations and month-over-month comparisons.

---

## Time Intelligence Measures

### MTD_loan_amount (Month-to-Date)
Calculates the total funded amount from the beginning of the month to the current date.

```dax
MTD_loan_amount = CALCULATE(TOTALMTD([Total_Funded_Amount],'Date Table'[Date]))
```

**Purpose:** Tracks month-to-date performance metrics.

**Key Components:**
- `TOTALMTD()` - Applies month-to-date logic
- `[Total_Funded_Amount]` - The measure being aggregated
- `'Date Table'[Date]` - The date column for context

---

### PMTD_LOAN_AMOUNT (Previous Month-to-Date)
Calculates the total funded amount for the same period in the previous month.

```dax
PMTD_LOAN_AMOUNT = 
CALCULATE(
    [Total_Funded_Amount],
    DATESMTD(
        DATEADD('Date Table'[Date], -1, MONTH)
    )
)
```

**Purpose:** Enables comparison with the previous month's performance.

**Key Components:**
- `DATEADD()` - Shifts date context back by 1 month
- `DATESMTD()` - Applies month-to-date filter on the shifted dates
- `CALCULATE()` - Evaluates the measure in the new context

---

### MOM_loan_amount (Month-over-Month Growth)
Calculates the percentage change in funded amount compared to the previous month.

```dax
MOM_loan_amount = DIVIDE(
    [MTD_loan_amount] - [PMTD_LOAN_AMOUNT],
    [PMTD_LOAN_AMOUNT]
)
```

**Purpose:** Tracks month-over-month growth rate as a percentage.

**Formula:** 
- Numerator: Current month - Previous month
- Denominator: Previous month value
- Result: Growth percentage (positive = increase, negative = decrease)

**Note:** Use `IFERROR()` to handle division by zero:
```dax
MOM_loan_amount = DIVIDE(
    [MTD_loan_amount] - [PMTD_LOAN_AMOUNT],
    [PMTD_LOAN_AMOUNT],
    0
)
```

---

## Loan Quality Measures

### Bad Loan %
Calculates the percentage of bad loans out of total loan applicants.

```dax
Bad loan % = CALCULATE([Total_Loan_Applicants],loan_data[Good vs Bad loan]="BAd Loan")/[Total_Loan_Applicants]
```

**Purpose:** Measures loan default/quality risk.

**Components:**
- `CALCULATE()` - Filters for "BAd Loan" category only
- Divides filtered count by total applicants
- Result: Percentage value (0-1)

---

### Good Loan %
Calculates the percentage of good loans out of total loan applicants.

```dax
good loan % = CALCULATE([Total_Loan_Applicants],loan_data[Good vs Bad loan]="Good Loan")/[Total_Loan_Applicants]
```

**Purpose:** Measures loan portfolio quality and performance.

**Note:** `Bad loan %` + `Good loan %` should equal 100%

---

## Date Table

### Date Table Creation
Creates a continuous date dimension table for time intelligence calculations.

```dax
Date Table = 
CALENDAR(
    MIN(loan_data[issue_date]),
    MAX(loan_data[issue_date])
)
```

**Purpose:** Provides a continuous date range for time-based calculations.

**Key Features:**
- `CALENDAR()` - Generates all dates between start and end
- `MIN()` - First date in loan_data
- `MAX()` - Last date in loan_data
- Creates one row per calendar day
- Essential for TOTALMTD(), DATEADD(), and other time functions

**Best Practices:**
- Mark this table as a Date Table in Power BI
- Ensure it covers the entire range of your data
- Add a calculated column for fiscal periods if needed

---

## Dynamic Measure Selection

### Select Measure
Allows users to dynamically switch between different KPIs using a slicer.

```dax
Select Measure = {
    ("Total_Loan_Applicants", NAMEOF('loan_data'[Total_Loan_Applicants]), 0),
    ("Total_Funded_Amount", NAMEOF('loan_data'[Total_Funded_Amount]), 1),
    ("Total Received Amount", NAMEOF('loan_data'[Total Received Amount]), 2)
}
```

**Purpose:** Creates a dynamic parameter for measure selection.

**Structure:**
- First element: Display name shown to users
- Second element: Actual measure name (using NAMEOF for safety)
- Third element: Sort order (for consistent ordering)

**Usage Pattern:**
1. Create this parameter table
2. Use it in a slicer
3. Reference in measures using conditional logic

**Example Implementation:**
```dax
Dynamic_Measure = 
SWITCH(
    SELECTEDVALUE('Select Measure'[Measure Name]),
    "Total_Loan_Applicants", [Total_Loan_Applicants],
    "Total_Funded_Amount", [Total_Funded_Amount],
    "Total Received Amount", [Total Received Amount],
    BLANK()
)
```

---

## Pattern Guidelines

### How to Create Similar KPIs

#### 1. **Base Measure Pattern**
```dax
[New_KPI_Base] = SUM(loan_data[column_name])
```
Start with a simple aggregation of your source data.

#### 2. **MTD Pattern**
```dax
[New_KPI_MTD] = CALCULATE(TOTALMTD([New_KPI_Base],'Date Table'[Date]))
```
Apply this formula to any base measure for month-to-date calculation.

#### 3. **PMTD Pattern**
```dax
[New_KPI_PMTD] = 
CALCULATE(
    [New_KPI_Base],
    DATESMTD(DATEADD('Date Table'[Date], -1, MONTH))
)
```
Compare performance with the previous period.

#### 4. **Growth Rate Pattern**
```dax
[New_KPI_MOM] = DIVIDE(
    [New_KPI_MTD] - [New_KPI_PMTD],
    [New_KPI_PMTD],
    0
)
```
Calculate percentage change month-over-month.

#### 5. **Quality/Category Percentage Pattern**
```dax
[Category_Percentage] = 
CALCULATE(
    [Base_Measure],
    table[category_column]="desired_category"
) / [Base_Measure]
```
Use to calculate percentages for any categorical breakdown.

---

## Common Mistakes to Avoid

1. **Missing Date Table Relationship**
   - Ensure Date Table is marked as a date table in Power BI
   - Verify relationship exists between loan_data and Date Table

2. **Case Sensitivity**
   - DAX strings are case-sensitive: "BAd Loan" ≠ "Bad Loan"
   - Match exact column values in your data

3. **Division by Zero**
   - Always use DIVIDE() with error handling
   - Include a third parameter for zero results

4. **Context Dependency**
   - CALCULATE() measures depend on filter context
   - Test measures with different slicers applied

5. **Performance Issues**
   - Avoid nested CALCULATE() statements
   - Use DATEADD() efficiently for date comparisons

---

## Recommendations

- **Format Numbers:** Apply appropriate formatting (%, currency, thousands separator) in Power BI
- **Add Tooltips:** Include measure descriptions for better user understanding
- **Document Changes:** Maintain version history for DAX formula updates
- **Test Edge Cases:** Verify measures work correctly with empty periods and null values
- **Optimize Queries:** Use query folding and minimize calculated columns in large datasets

---

## Version History

| Version | Date | Changes |
|---------|------|---------|
| 1.0 | 2026-02-03 | Initial documentation of core DAX queries |

---

## Related Files
- `dax.md` - Original DAX query file
- Power BI Dashboard: Loan Dashboard Report

---

**Last Updated:** February 3, 2026
