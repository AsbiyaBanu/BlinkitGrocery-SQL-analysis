# 🛒 Blinkit Grocery Sales Analysis (SQL)

A SQL-based exploratory analysis of Blinkit grocery sales data, covering data cleaning, KPI calculation, and business-question-driven reporting on item, outlet, and pricing performance.

## 📌 Project Overview

This project analyses a Blinkit grocery sales dataset to uncover which item types, outlet formats, and outlet locations drive the most revenue. The workflow moves from raw data cleanup through core KPI definition to granular, business-focused queries — the same structure used in a real-world retail analytics workflow.

**Tools used:** SQL (SQL Server syntax)

## 🗂️ Dataset

The dataset (`BlinkITGrocery`) contains item- and outlet-level grocery sales records, including:

| Column | Description |
|---|---|
| `item_type` | Product category |
| `item_fat_content` | Fat content label (raw data contained inconsistent labels) |
| `item_mrp` | Maximum retail price of the item |
| `sales` | Sales value for the record |
| `outlet_size` | Size classification of the outlet |
| `outlet_type` | Outlet format (e.g. grocery store, supermarket) |
| `outlet_location` | Outlet's location tier/type |

## 🧹 Data Cleaning

The raw data contained inconsistent labelling in `item_fat_content` (e.g. `'LF'`, `'low fat'`, `'reg'`). This was standardised into two clean categories, `'Low Fat'` and `'Regular'`, using `UPDATE` statements, then verified with a `DISTINCT` check:

```sql
UPDATE BlinkITGrocery SET item_fat_content = 'Low Fat'
WHERE item_fat_content IN ('LF', 'low fat');

UPDATE BlinkITGrocery SET item_fat_content = 'Regular'
WHERE item_fat_content = 'reg';

SELECT DISTINCT item_fat_content FROM BlinkITGrocery;
```

## 📊 Key Performance Indicators (KPIs)

Three headline KPIs were calculated to establish an overall performance baseline:

1. **Total Sales Revenue** — `SUM(sales)` across all records
2. **Average Sales per Item** — `AVG(sales)` across all records
3. **Total Number of Items** — `COUNT(*)` of all records

## 🔍 Business Questions Answered

| # | Question | Technique Used |
|---|---|---|
| 1 | Which item types generate the most revenue? | `GROUP BY item_type` + `SUM`/`COUNT`, ranked with `ORDER BY` |
| 2 | How does sales performance vary by outlet size? | `GROUP BY outlet_size` + `SUM`/`AVG`/`COUNT` |
| 3 | How does sales performance vary by outlet type? | `GROUP BY outlet_type` + `SUM`/`AVG`/`COUNT` |
| 4 | Which price range (MRP band) sells the most? | `CASE WHEN` price-bucketing + aggregation |
| 5 | Which outlet type + location combination performs best? | `GROUP BY` on two dimensions, `LIMIT 10` |

### Example: Price-Range Segmentation

Items were segmented into four MRP bands to see whether budget or premium items drive more revenue:

```sql
SELECT
  CASE
    WHEN item_mrp < 50 THEN 'Budget (under 50)'
    WHEN item_mrp BETWEEN 50 AND 100 THEN 'Mid (50-100)'
    WHEN item_mrp BETWEEN 100 AND 200 THEN 'Premium (100-200)'
    ELSE 'Luxury (200+)'
  END AS price_range,
  COUNT(*) AS item_count,
  ROUND(SUM(sales), 2) AS total_sales
FROM BlinkITGrocery
GROUP BY price_range
ORDER BY total_sales DESC;
```

## 🛠️ SQL Skills Demonstrated

- Data cleaning and standardisation with conditional `UPDATE` statements
- KPI definition using aggregate functions (`SUM`, `AVG`, `COUNT`)
- Single- and multi-dimension `GROUP BY` analysis
- Conditional bucketing with `CASE WHEN` for price-range segmentation
- Ranking and filtering results with `ORDER BY` and `LIMIT`

## 🚀 How to Use

1. Import the dataset into a SQL Server (or compatible) database as `BlinkITGrocery`.
2. Run `SQL_Blinkit_grocery_project.sql` sequentially — the script is organised into three stages: data check, data cleaning, and KPI/business-question queries.
3. Each query is labelled with a comment describing the business question it answers.

## 📈 Next Steps

Potential extensions to this analysis include visualising the outputs in Power BI or Tableau, adding time-based trend analysis if a date column is available, and building a customer- or transaction-level view if more granular data becomes available.

---
**Author:** Asbiya Banu Iqbal Hussain
**Portfolio:** [github.com/AsbiyaBanu/Data-Analyst-Portfolio](https://github.com/AsbiyaBanu/Data-Analyst-Portfolio)
