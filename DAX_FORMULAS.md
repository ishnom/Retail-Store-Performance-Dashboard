# 📊 DAX Formulas for Retail Dashboard

## Core Metrics

### Total Orders
```dax
Total Orders = COUNT(retail_data[Order_No])
```
Counts the total number of orders in the dataset.

---

## Year-over-Year Analysis

### Total Orders 2015
```dax
Total Orders 2015 = 
CALCULATE(
    [Total Orders],
    'Date'[Year] = 2015
)
```
Filters total orders for fiscal year 2015.

### Total Orders 2016
```dax
Total Orders 2016 = 
CALCULATE(
    [Total Orders],
    'Date'[Year] = 2016
)
```
Filters total orders for fiscal year 2016.

### Orders Difference %
```dax
Orders diffrence% = 
DIVIDE(
    [Total Orders 2016] - [Total Orders 2015],
    [Total Orders 2015],
    0
)
```
Calculates the percentage change in orders between FY16 and FY15.

---

## Discount Analysis

### Discount Category
```dax
Discount_Category = 
IF(
    'retail_data'[Discount_percent] <= 5,
    "Low Discount",
    "High Discount"
)
```
Classifies discounts as Low (≤5%) or High (>5%).

### High Discount Orders
```dax
High discount orders = CALCULATE(
    COUNT(retail_data[Order_No]),
    retail_data[Discount_Category]="High Discount"
)
```
Counts orders with high discounts applied.

### High Discount Profit
```dax
High Discount profit = CALCULATE(
    [Total Profit],
    retail_data[Discount_Category]="High Discount"
)
```
Calculates total profit from high discount orders.

---

## Profitability Metrics

### Profit Margin %
```dax
Profit Margin % = 
DIVIDE(
    [Total Profit],
    [Total Sales],
    0
)
```
Calculates the profit margin as a percentage of total sales.

---

## Date Table

### Date Dimension Table
```dax
Date = 
ADDCOLUMNS(
    CALENDAR (
        MIN(retail_data[Order_date]),
        MAX(retail_data[Ship_Date])
    ),
    "Year", YEAR([Date]),
    "Month No", MONTH([Date]),
    "Month Name", FORMAT([Date], "MMM"),
    "Year-Month", FORMAT([Date], "YYYY-MM"),
    "Quarter", "Q" & FORMAT([Date], "Q")
)
```
Creates a date dimension table with:
- **Year**: Fiscal year
- **Month No**: Month number (1-12)
- **Month Name**: Abbreviated month name (Jan, Feb, etc.)
- **Year-Month**: Combined format (YYYY-MM)
- **Quarter**: Quarter designation (Q1, Q2, Q3, Q4)

---

## Notes
- All calculations use CALCULATE() for filtering context
- DIVIDE() function with 0 as default prevents division by zero errors
- Date table spans from the earliest Order_date to the latest Ship_Date
- Discount categories enable comparative analysis of promotional strategy effectiveness
