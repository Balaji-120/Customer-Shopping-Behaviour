# Customer Shopping Behavior Analysis

End-to-end analytics project on a retail customer transaction dataset, moving data through **Python (cleaning) → MySQL via SQLAlchemy (querying) → Power BI (dashboarding)**, with a written report summarizing the findings.

## Project Pipeline

1. **Data Cleaning — Python / pandas** (`Customer_Behaviour_Analysis.ipynb`)
   - Loaded `customer_shopping_behavior.csv` and profiled it with `.info()` / `.describe()`.
   - Imputed missing `Review Rating` values using the median rating per product `Category`.
   - Standardized column names to snake_case (e.g. `Purchase Amount (USD)` → `purchase_amount`).
   - Engineered `age_group` (Young Adult / Adult / Middle-aged / Senior) via quantile binning on `age`.
   - Engineered `purchase_frequency_days` by mapping `frequency_of_purchases` to a numeric day count.
   - Dropped `promo_code_used` after confirming it was identical to `discount_applied`.
   - Loaded the cleaned DataFrame into MySQL using `sqlalchemy.create_engine` + `df.to_sql()`.

2. **Analysis — SQL / MySQL** (`customer_shopping_behavior.sql`)
   - 10 business questions answered against the `customer` table using `GROUP BY`, subqueries, `CASE` expressions, and window functions (`ROW_NUMBER`).
   - Covers revenue by gender, discount behavior, top-rated and top-selling products, shipping comparisons, subscription impact, customer segmentation, and age-group revenue contribution.

3. **Dashboard — Power BI** (`customer_shoppping.pbix`)
   - Interactive dashboard built on the cleaned dataset with slicers for gender, category, age group, and subscription status.

4. **Report** (`Customer_Shopping_Behavior_Report.docx`)
   - Formatted write-up of methodology, dataset overview, all 10 findings with tables and insights, and recommendations.

## Files in This Project

| File | Description |
|---|---|
| `customer_shopping_behavior.csv` | Raw source dataset (3,900 rows × 18 columns) |
| `Customer_Behaviour_Analysis.ipynb` | Python/pandas cleaning and feature engineering notebook |
| `customer_shopping_behavior.sql` | SQL schema and 10 analytical queries |
| `customer_shoppping.pbix` | Power BI dashboard file |
| `Customer_Shopping_Behavior_Report.docx` | Final written report |

## Dataset Summary

- **Records:** 3,900 customers
- **Total revenue:** $233,081
- **Categories:** Clothing, Footwear, Outerwear, Accessories
- **Locations:** 50 U.S. states
- **Payment methods:** Credit Card, Debit Card, PayPal, Venmo, Cash, Bank Transfer
- **Subscribers:** 1,053 (27.0%)

## Key Findings

- Male customers generated ~2.1x the revenue of female customers ($157,890 vs. $75,191).
- 839 customers used a discount but still spent above the $59.76 average purchase amount.
- Gloves, Sandals, Boots, Hat, and Handbag are the top 5 products by average review rating.
- Express shipping customers spend slightly more on average than Standard ($60.48 vs. $58.46).
- Subscribers and non-subscribers spend nearly the same per transaction — the subscription program isn't currently driving higher basket size.
- Hat, Sneakers, Coat, Sweater, and Pants have the highest discount usage rates (~47–50%).
- ~80% of customers fall into the "Loyal" segment (11+ previous purchases); only ~2% are "New."
- Product purchase volumes are evenly spread within each category — no single item dominates.
- Repeat buyers (5+ previous purchases) are not more likely to subscribe (non-subscribers outnumber subscribers ~2.6:1).
- Revenue is fairly evenly distributed across age groups ($55.8K–$62.1K), with Young Adults contributing marginally the most.

See `Customer_Shopping_Behavior_Report.docx` for full detail and recommendations.

## How to Reproduce

1. **Clean the data:** run `Customer_Behaviour_Analysis.ipynb` top to bottom in a Python environment with `pandas`, `sqlalchemy`, and `pymysql` installed.
2. **Load to MySQL:** update the connection credentials in the notebook's last cell, then run it to create the `customer_behaviour` database and `customer` table.
   > ⚠️ The notebook currently has a hardcoded MySQL password — replace it with an environment variable or config file before sharing or reusing this notebook.
3. **Run the analysis:** execute the queries in `customer_shopping_behavior.sql` against the loaded table (MySQL client or Workbench).
4. **View the dashboard:** open `customer_shoppping.pbix` in Power BI Desktop.

## Requirements

- Python 3.x, `pandas`, `sqlalchemy`, `pymysql`
- MySQL Server 8.x
- Power BI Desktop
