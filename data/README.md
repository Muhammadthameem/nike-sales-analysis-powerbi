#  Nike Sales Analysis — Power BI Dashboard

An interactive sales and product analytics dashboard built in **Microsoft Power BI**, analyzing Nike sales performance across products, categories, regions, store types, customer age groups, and time periods.


---

## 📌 Overview

This project transforms raw Nike sales transaction data into a fully interactive Power BI report covering:

- Overall sales, revenue, and order performance
- Product- and category-level performance
- Regional and store-type performance
- Customer age-group purchasing behavior
- Monthly, quarterly, and yearly revenue trends
- Year-over-year (YoY) revenue and margin changes
- Seasonal sales patterns and profit performance

The dashboard is fully interactive — users can filter by **Year**, **Region**, **Category**, **Product**, and **Date Range** across three linked report pages.

---


## 🎯 Objective

To turn 3,000 raw Nike sales transactions into a business-oriented analytics tool that answers:

- Which regions, categories, and products drive the most revenue and profit?
- How does performance change month to month and year over year?
- Which customer age groups and store types contribute most to sales?
- What seasonal patterns exist in Nike's sales data?

---

## 🛠️ Tools & Technologies

| Tool / Feature | Purpose |
|---|---|
| Power BI Desktop | Report authoring |
| Power Query | Data import & transformation |
| DAX | Calculated columns & measures |
| Data Modeling | Star-schema relationships |
| Date/Time Intelligence | YoY and previous-period comparisons |
| Field Parameters | Month / Year / Quarter granularity switch |
| Slicers & Conditional Formatting | Interactive filtering |
| Image-based Slicer | Visual product selector |

---

## 📂 Dataset

Two structured CSV datasets power the model:

**1. `Nike_Sales_3000.csv`** — 3,000 transaction-level records
`Order_ID · Order_Date · Product_ID · Region · Store_Type · Units_Sold · Discount · Revenue · Profit · Customer_Age · Gender · Payment_Method`

**2. `Nike_Products.csv`** — 20-product master table
`Product_ID · SKU · Product_Name · Category · Unit_Price · image_url`

Product data was deliberately kept separate from transactions to avoid duplication across 3,000 rows.

**Date range:** January 2023 – September 2025
> ⚠️ 2025 data is **incomplete** (partial year through September). It is intentionally excluded from annual comparisons — all "strongest year" claims refer to complete years only.

---

## 🧮 Data Model

A star-schema model with a fact table and two dimension tables:

```
Nike_Products (1) ────< Nike_Sales_3000 >──── (1) DateTable
   Product_ID              Product_ID / Order_Date         Date
```

- **Nike_Products** — dimension table (product, category, price, image)
- **Nike_Sales_3000** — fact table (one row per transaction)
- **DateTable** — dedicated date dimension built with `CALENDAR()` + `ADDCOLUMNS()`, marked as the official Date Table for time intelligence

---

## 🔢 Key DAX Measures

```dax
Total Revenue = SUM(Nike_Sales_3000[Revenue])

Avg Profit Margin =
DIVIDE(SUM(Nike_Sales_3000[Profit]), SUM(Nike_Sales_3000[Revenue]), 0) * 100

Revenue PY =
CALCULATE([Total Revenue], SAMEPERIODLASTYEAR(DateTable[Date]))

Revenue YoY % =
DIVIDE([Total Revenue] - [Revenue PY], [Revenue PY], 0) * 100

Avg Revenue per Order =
DIVIDE([Total Revenue], [Total Orders], 0)

Age Group =
SWITCH(
    TRUE(),
    Nike_Sales_3000[Customer_Age] < 25, "18-24",
    Nike_Sales_3000[Customer_Age] < 35, "25-34",
    Nike_Sales_3000[Customer_Age] < 45, "35-44",
    Nike_Sales_3000[Customer_Age] < 55, "45-54",
    "55+"
)
```

Revenue itself is derived rather than stored directly:

```
Revenue = Units Sold × Unit Price × (1 − Discount / 100)
```

Additional measures cover total orders, units, profit, previous-year values, and YoY change for orders and units — all following the same reusable pattern.

---

## 📊 Report Pages

| Page | Focus |
|---|---|
| **Landing Page** | Nike-themed cover with navigation buttons to all three report pages |
| **Sales Overview** | Revenue, orders, and profit margin KPIs; revenue by region, store type, and category; monthly/quarterly trends |
| **Product Analysis** | Image-based product slicer; revenue and units by region and age group; product-level trends over time |
| **Trend Analysis** | Month/Year/Quarter field parameter; YoY revenue growth; seasonal revenue heatmap |

---

## 💡 Key Insights

- **October 2023** recorded the highest monthly revenue; **May 2023** recorded the lowest.
- **2024** was the strongest complete year, generating **$273,317.15** (~$273.3K) in total revenue.
- **2025** is excluded from annual comparisons — the dataset only covers January–September 2025.
- The **25–34** age group accounts for the largest share of units sold.
- **Footwear** leads all categories in both total profit and average revenue per order.

**Headline numbers:**

| Metric | Value |
|---|---|
| Total Revenue | $705.88K |
| Total Orders | ~3K |
| Total Units | ~9K |
| Avg Profit Margin | 28.81% |
| Avg Revenue per Order | $235.29 |

---

## 🔁 Project Workflow

```
Raw Sales Data → Data Preparation → Data Modeling → DAX Measures
→ Interactive Visualizations → Dashboard → Business Insights
```

---

## 📁 Repository Structure

```
├── Nike_Sales_Analysis.pbix        # Power BI report file
├── data/
│   ├── Nike_Sales_3000.csv
│   └── Nike_Products.csv
├── screenshots/
│   ├── landing-page.png
│   ├── sales-overview.png
│   ├── product-analysis.png
│   └── trend-analysis.png
└── README.md
```

---

## 📌 Notes on Interpretation

2025 is a partial year in this dataset. Any year-over-year or "best year" comparison in this report treats **2024** as the most recent complete year rather than comparing it against incomplete 2025 figures.
