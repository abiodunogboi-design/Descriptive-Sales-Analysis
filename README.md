# Vespera Dynamics: Descriptive Sales Analysis Dashboard (Power BI)

> A Power BI dashboard that turns multi-year Egyptian sales records into clear answers on regional market share, governorate performance, seasonality, product demand and order value.
---
<img width="479" height="325" alt="Vespera Dashboard" src="https://github.com/user-attachments/assets/cf8a050f-679f-45f4-8d54-58a34e0db868" />

---

---

## 📌 At a Glance

| | |
|---|---|
| **Business** | Vespera Dynamics, selling Technology, Furniture and Office Supplies to Corporate, Consumer and Home Office customers across Egypt |
| **Problem** | Several years of sales records, but no clear view of how sales were performing |
| **Headline results** | **49.52M EGP** sales · **6.55M EGP** profit · **13%** margin · **2,000** orders · **9,483** units · **24.76K EGP** average order value |
| **Tools** | Power BI Desktop, Power Query, DAX |
| **Model** | Star schema: 1 fact table, 4 dimension tables and a calendar table |
| **Period** | Year buttons for 2023 to 2026; figures below combine all years |
| **Currency** | Egyptian pounds (EGP) |
| **Data source** | AI-generated synthetic dataset created for portfolio practice; it contains no real company or customer data |

---

## 🎯 Business Problem

*This is a simulated business scenario built on a synthetic dataset, so the findings show how the analysis works and not the performance of a real company.*

Executives had kept sales records for years but could not see how the business was doing. They asked six questions, and each one maps to a dashboard visual:

| Business question | Where the dashboard answers it |
|---|---|
| What share of the business does each region drive, and where should we expand? | Sales Regional Distribution (donut) |
| Are we meeting our financial goals for volume, profit and efficiency? | KPI cards: sales, profit, profit margin |
| Which governorates drive the most revenue, and which underperform? | Total Revenue per State (bar) |
| In which months do sales peak or drop against the previous month? | Total Sales vs Total Sales (PM) (line) |
| Which product category earns the most, and which needs a push? | Total Sales by Category (column) |
| What does a typical transaction look like, and are we upselling? | Average Order Value, Total Orders, Total Qty |

---

## 🗂️ About the Data

| Area | What is captured |
|---|---|
| **Time** | Order Date and Ship Date, to measure fulfilment cycles and demand over time |
| **Geography** | Four levels: Country (Egypt), Region (Upper Egypt, Delta, Canal, Cairo, Sinai, Red Sea, Alexandria), State/Governorate and City, plus Postal Code |
| **Products** | Category (Technology, Furniture, Office Supplies), Sub-Category (e.g. Phones, Chairs, Fasteners) and Product Name |
| **Customers** | Customer ID and Segment (Corporate, Consumer, Home Office) |
| **Financials** | Sales, Quantity, Discount and Profit per line item |
| **Logistics** | Ship Mode, from Same Day to Standard Class |

---

## 🧹 Data Preparation (Power Query)

| Step | What was done |
|---|---|
| 1. Inspection | Imported the raw file and examined its structure, columns and data quality |
| 2. Data types | Set correct types for dates, numbers and categories so calculations work |
| 3. Quality checks | Examined the data for missing values, duplicates and inconsistent entries, and fixed the issues found |
| 4. Standardization | Standardized column names and category values for consistent reporting |
| 5. Transformation | Used Power Query to shape the data for analysis; reconciled totals to the source files and flagged anomalous transactions |
| 6. Validation | Reviewed the result to confirm it was ready for modelling, DAX and visuals |

---

## 🧱 Data Model

A **star schema** keeps filtering predictable and measures consistent.

```
 Customer Dim ──┐
 Location Dim ──┤
 Order Dim ─────┼──►  Fact Table
 Product Dim ───┤
 Calendar Date ─┘
```

| Table | Fields |
|---|---|
| **Fact Table** | Customer ID, Product ID, Order ID, Postal Code, Row ID, Sales, Quantity, Discount, Profit |
| **Customer Dim** | Customer ID, Customer Name, Segment |
| **Location Dim** | Postal Code, Country, Region, State, City |
| **Order Dim** | Order ID, Order Date, Ship Date, Ship Mode |
| **Product Dim** | Product ID, Category, Sub-Category, Product Name |
| **Calendar Date** | Date, Year, Quarter, Month, Month Name |

**Relationship rules:** one-to-many from each dimension to the fact table; single-direction filtering by default; a dedicated Date table marked as the date table; no many-to-many relationships.

---

## 🧮 DAX Measures

```dax
Total Sales          = SUM('Fact Table'[Sales])
Total Profit         = SUM('Fact Table'[Profit])
Profit Margin        = DIVIDE([Total Profit], [Total Sales])
Average Order Value  = DIVIDE([Total Sales], [Total Orders])
Total Sales PM       = CALCULATE([Total Sales], DATEADD('Calendar Date'[Date], -1, MONTH))
Calendar Date        = CALENDARAUTO()
```

| Measure | Purpose |
|---|---|
| Total Sales / Total Profit | Base measures that all other calculations build on |
| Profit Margin | Share of sales kept as profit |
| Average Order Value | Average revenue per order, used to judge purchasing health and upselling |
| Total Sales PM | Previous month's sales, a baseline for month-over-month trends |
| Calendar Date | A continuous calendar generated automatically from the dates in the data |

---

## 📊 The Dashboard

| KPI | Value |
|---|---|
| Total Sales | 49.52M EGP |
| Total Qty | 9,483 units |
| Total Orders | 2,000 |
| Total Profit | 6.55M EGP |
| Profit Margin | 13% |
| Average Order Value | 24.76K EGP |

**Visuals:** Total Revenue per State · Sales Regional Distribution · Total Sales by Category · Total Sales vs Total Sales (PM). The year buttons (2023 to 2026) filter the page.

---

## 🔎 Key Insights & Recommendations

| # | Insight | Recommendation |
|---|---|---|
| 1 | **Healthy, high-value orders.** 49.52M EGP of sales and 6.55M EGP profit (13% margin) from 2,000 orders, about **4.7 units per order**, at an average order value of 24.76K EGP. | Judge future promotions on profit as well as sales, since a 13% margin leaves little room for deep discounts. |
| 2 | **Sales are concentrated in a few regions.** Upper Egypt contributes **33.97%**, Delta 20.63% and Canal 17.18%, so these three regions drive **72%** of sales. Alexandria is the smallest at **4.57%**. | Protect the top regions, and investigate why Alexandria lags before spending on expansion there. |
| 3 | **Governorate spread is wide.** South Sinai and Luxor lead at about 3.7M EGP each. Asyut is lowest at **1.8M EGP**, less than half of the leaders. Alexandria is also low at 2.3M EGP. | Run a targeted review and local promotions in Asyut and Alexandria. |
| 4 | **Technology is the revenue engine.** It clearly outsells Furniture, while Office Supplies is a distant third. | Give Office Supplies a promotional push, for example bundles with Technology purchases. |
| 5 | **Seasonal peak and trough.** Monthly sales peak in **April** (just above 5.0M EGP), fall to a low in **October and November** (about 3.5M EGP, roughly 30% below the peak) and recover in December. | Schedule promotions for the October to November dip to smooth revenue. |

*Chart-based values (category and monthly figures) are read from the dashboard and are approximate.*

*Insights describe the generated dataset. The goal is to demonstrate the method: KPI design, star-schema modelling, DAX and dashboard storytelling.*

---

## 🚀 Expected Business Impact

These are the outcomes the recommendations aim for. They are not measured in this project.

- **A steadier revenue year:** promotions timed for the October to November trough could flatten the seasonal dip.
- **Growth in weaker markets:** Alexandria (4.57% of sales) and Asyut (1.8M EGP) are the clearest room for improvement.
- **Protected order value:** growth should come without eroding the 24.76K EGP average order value or the 13% margin.

---

## ⚠️ Limitations & Next Steps

- **Synthetic data:** the dataset was generated with AI, so patterns such as the April peak and the regional split are artifacts of the generated data, not real market behavior.
- **Descriptive only:** the dashboard explains what happened and does not forecast.
- **Months combine all years,** so the seasonal view can blend growth trends with seasonality.
- **No cost or inventory data,** so promotion results can't be quantified.
- **Next steps:**
  - year-over-year growth measures
  - discount-versus-profit analysis
  - a fulfilment-time view built from Order Date and Ship Date
  - sub-category drill-through
  - sales forecasting

---

## 📁 Repository Structure

```
├── README.md
├── Vespera_Dynamics_Sales_Analysis.pbix
├── images/
│   ├── vespera-dashboard.png
│   └── data-model.png
└── data/
    └── README.md
```

Open the `.pbix` file in Power BI Desktop to explore the report.

---

## 👤 Author

**Sunday Ogboi**, Data Analyst focused on data analytics, business intelligence and practical data workflows.
