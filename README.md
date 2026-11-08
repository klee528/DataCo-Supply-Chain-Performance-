# DataCo Supply Chain Performance Dashboards (Power BI)

A 3-page Power BI report analyzing sales, customers, and delivery performance on the public **DataCo Smart Supply Chain** dataset (Kaggle). The project covers data modeling, KPI design, RFM segmentation, and, most importantly, data quality checks that changed how the results should be read.

> **Note:** DataCo is a simulated dataset. The goal of this project is to demonstrate an analysis workflow, not to evaluate a real business.

## Highlights

- Built a star-schema model and a 3-page report (Sales, Customer, Shipping) with year-over-year KPIs in DAX.
- Detected a **structural break in the data** on Oct 2, 2017: items per order dropped from ~3 to exactly 1 within two days. Ruled out customer, product, and regional causes, then limited comparisons to valid periods. This prevented a false 40-67% revenue decline from being reported.
- Segmented ~12K customers with RFM scoring, with thresholds calibrated from distribution checks rather than guessed.
- Found a **57% late-delivery rate** that stayed flat across 2015-2017.
- Documented dataset limitations so conclusions are not overstated.

## Dataset

| Item | Value |
|---|---|
| Source | DataCo Smart Supply Chain for Big Data Analysis (Kaggle) |
| Period | Jan 2015 - Jan 2018 |
| Size | ~180K order-item rows, ~65.7K orders, ~20.6K customers |
| Comparable period used | Jan 2015 - Sep 30, 2017 (see Data Quality) |

## Data Model

Star schema with two fact tables at different grains:

- `fact_order`: one row per order (customer, dates, shipping days, delivery status)
- `fact_order_item`: one row per product in an order (sales, discount, profit, quantity)
- Dimensions: `Date`, `dim_customer`, `dim_geography`, `dim_order_info`, `dim_product`
- `RFM`: calculated table (one row per customer with an order before Oct 1, 2017)

Relationships are one-to-many, single direction, from dimensions down to facts, and `fact_order` filters `fact_order_item` on `order_id`.

## Report Pages

### 1. Sales Overview
- KPIs (Oct 2016 - Sep 2017 vs prior year): revenue, profit, margin %, orders, AOV.
- Monthly revenue by year, monthly YoY growth, orders vs. order line items, items per order, and revenue with margin by month.
- A data note explains why figures from Oct 2, 2017 onward are not comparable.

### 2. Customer
- KPIs: active customers, revenue per customer, orders per customer, repeat rate, % at risk.
- RFM groups (Champions, Loyal, Potential Loyalist, Need Attention, At Risk, Hibernating) with revenue and customers per group.
- Data-health view of frequency distribution, a rule-based purchase-cycle risk flag, and a prescriptive table ranking groups by profit at risk.
- Active vs. returning customers by month.

### 3. Shipping
- KPIs: late delivery rate, average actual vs. scheduled shipping days, average delay, orders.
- Late rate by month and by shipping mode, actual vs. scheduled days by mode, and a prioritization table.

## Key Findings (Oct 2016 - Sep 2017 vs prior year)

| Area | Result |
|---|---|
| Revenue | 11.44M (+3.5%) |
| Profit | 1.39M (+7.2%), margin 12.1% (flat) |
| Orders | 20.85K (about flat) |
| Active customers | ~10.1K (flat), repeat rate 61.3% |
| Profit at risk (rule-based) | ~0.9M, concentrated in At Risk and Hibernating |
| Late delivery rate | 57.3%, flat over time |
| Shipping mode | First Class 100% late, Second Class ~80%, Same Day ~48%, Standard ~40% |

Overall: revenue, profit, and customer base are stable with no sign of decline in the reliable period. Delivery performance is the main weakness.

## Data Quality Finding

From Oct 2, 2017, every order contains exactly 1 item (previously ~3), quantity per line becomes 1.00, and average unit price jumps and then drifts back. Order counts rise ~20% while line items and revenue fall.

- Items per order: 2.89 (Oct 1), 1.79 (Oct 2), 1.00 (from Oct 3).
- Customer, product, and regional breakdowns all shifted at the same time, which points to a change in how the data was recorded or generated rather than real buying behavior.
- The source of the change could not be confirmed.

**Decision:** revenue, order-structure metrics, RFM, and YoY comparisons are limited to data through Sep 30, 2017. Shipping late rate is unaffected by the break.

## Limitations

- **Simulated data:** customers buy very regularly and none lapse beyond 365 days, so RFM separates customers less sharply than in real retail data. "At Risk" and "Hibernating" do not mean customers have actually churned.
- **Short comparable window:** 33 months, not enough to conclude seasonality or long-term trends. January 2018 contains only one month.
- **Rule-based, not validated:** the purchase-cycle risk flag, profit at risk, and suggested actions are heuristics, not machine-learning predictions.
- **Extreme shipping results:** First Class being 100% late reflects how the data was generated, not real carrier performance.

## Tools

Power BI, DAX, Power Query

## Repository Contents

```
.
├── README.md
├── DataCo_Dashboard.pbix     # Power BI report (add your file)
├── screenshots/              # Dashboard images (add yours)
│   ├── sales.png
│   ├── customer.png
│   └── shipping.png
└── docs/
    └── report.pdf            # Business summary (optional)
```

## Screenshots

![Sales](screenshots/sales.png)
![Customer](screenshots/customer.png)
![Shipping](screenshots/shipping.png)

## How to Use

1. Download the dataset from Kaggle (DataCo Smart Supply Chain for Big Data Analysis).
2. Open `DataCo_Dashboard.pbix` in Power BI Desktop and update the data source path if prompted.
3. Refresh the model.

## Author

Your Name | [LinkedIn](https://www.linkedin.com/) | [Email](mailto:you@example.com)
