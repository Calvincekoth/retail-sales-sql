# Duka Smart Electronics | Retail Sales & Customer Analytics

![PostgreSQL](https://img.shields.io/badge/PostgreSQL-18-336791?logo=postgresql&logoColor=white)
![SQL](https://img.shields.io/badge/SQL-46%20queries-blue)
![Level](https://img.shields.io/badge/Level-Beginner%20to%20Advanced-success)
![Data](https://img.shields.io/badge/Data-150%20customers%20%7C%20800%20orders-informational)

**A PostgreSQL portfolio project by [Calvince Okoth](https://www.linkedin.com/in/calvince-okoth-85792b25a/)**

> Customer value, product profitability, sales momentum and customer inactivity for a growing Kenyan electronics retailer, answered with SQL from basic queries to window functions and a PL/pgSQL function.

---

## 1. Business problem

Duka Smart Electronics is a fictional Kenyan retailer selling phones, laptops, accessories, televisions and home appliances across ten towns. It has recorded customer and order data since January 2025, but management has not used it to make decisions.

As the Junior Data Analyst, I built a PostgreSQL database, validated the data and answered 46 business questions to support four priorities:

| Priority | Question |
|---|---|
| Customer value | Who spends the most, and who may have stopped buying? |
| Product economics | Which products and categories earn revenue, and which earn profit? |
| Sales momentum | Is monthly revenue growing or falling? |
| Business action | What should management do next, based on the evidence? |

## 2. Key findings

All money figures use **Completed orders only** (see [Business rules](#4-business-rules)).

| Metric | Result |
|---|---:|
| Revenue | KSh 24,155,371.70 |
| Cost | KSh 21,264,450.00 |
| Gross profit | KSh 2,890,921.70 |
| Gross margin | 12.0% |
| Cancellation rate (all orders) | 11.25% |
| Customers with at least one Completed order | 132 of 150 |

**1. Discounts are turning some sales into losses.**
62 Completed order lines sold below cost, about **KSh -142k** in total. The worst products are the Hotpoint Refrigerator (16 lines), Hisense 43in Smart TV (13) and iPhone 13 (8). On big-ticket items with thin margins, a 10 to 20% discount pushes the sale under cost.
*Recommendation:* set discount caps per product based on its margin, and review discount approval for refrigerators, TVs and phones.

**2. Revenue and profit tell different stories.**
Mobile Phones earns the most revenue (KSh 6.94M) and the most profit in shillings. But **Accessories earns a 42.4% margin** on only KSh 1.08M of revenue, against roughly 10 to 11% for every other category.
*Recommendation:* push accessory bundles with phone and laptop sales to lift overall margin.

| Category | Revenue (KSh) | Gross profit (KSh) | Margin |
|---|---:|---:|---:|
| Mobile Phones | 6,942,060.65 | 728,760.65 | 10.5% |
| Computing | 6,596,221.70 | 700,021.70 | 10.6% |
| Electronics | 5,515,359.20 | 547,359.20 | 9.9% |
| Home Appliances | 4,024,234.05 | 458,034.05 | 11.4% |
| Accessories | 1,077,496.10 | 456,746.10 | 42.4% |

**3. Nearly one in three customers is inactive.**
47 customers have a last Completed order more than 90 days before the latest date in the data (30 Sep 2026).
*Recommendation:* run a win-back campaign, starting with the highest past spenders.

**4. VIP customers spend the most.**
Average completed spend is about KSh 253k for VIP, 171k for Corporate and 141k for Retail (all customers in each segment, including those with no purchases). The top five customers are Brian Chebet, Peter Ndungu, Lucy Njoroge, Nancy Korir and Purity Ochieng.
*Recommendation:* give VIP and Corporate accounts dedicated follow-up, and check whether Retail customers can be moved up.

**5. Strong growth, with a sharp seasonal dip.**
Revenue peaked in December 2025 at about KSh 2.03M, fell to about **KSh 322k in January 2026**, then rebuilt to over KSh 2M by August and September 2026.
*Recommendation:* plan stock and promotions around the January slowdown.

Full evidence, with query references, is in [`insights.md`](insights.md).

## 3. Dataset

| Component | Coverage |
|---|---|
| Customers | 150 |
| Orders | 800 (634 Completed, 76 Pending, 90 Cancelled) |
| Categories | Electronics, Accessories, Home Appliances, Computing, Mobile Phones |
| Locations | Nairobi, Mombasa, Kisumu, Nakuru, Eldoret, Thika, Nyeri, Meru, Machakos, Kiambu |
| Segments | Retail, Corporate, VIP |
| Payment methods | M-Pesa, Cash, Card, Bank Transfer |
| Period | 1 January 2025 to 30 September 2026 |

The data is synthetic and generated for practice. Customer names and genders were assigned independently, so some genders do not match first names. I did not analyse by gender for that reason.

### Schema

```text
customers (1) ───< orders (many)

customers: customer_id (PK), customer_name, gender, city, registration_date, customer_segment
orders:    order_id (PK), customer_id (FK), order_date, product_name, product_category,
           quantity, unit_price, cost_price, discount, payment_method, order_status
```

Constraints enforce valid segments, statuses, payment methods, positive quantities and discounts between 0 and 100. Indexes on `orders(customer_id)` and `orders(order_date)` speed up the joins and date filters.

## 4. Business rules

| Measure | Definition |
|---|---|
| Revenue | `quantity * unit_price * (1 - discount / 100.0)` |
| Cost | `quantity * cost_price` |
| Gross profit | `revenue - cost` |
| Gross margin | `profit / NULLIF(revenue, 0) * 100` |
| Money metrics | Completed orders only |
| Order counts | All statuses, unless a question says otherwise |
| Reference date | `MAX(order_date)`, not the computer's date |
| Reporting month | `DATE_TRUNC('month', order_date)` |

Key choices: `NULLIF` prevents division by zero; segment averages include customers with no completed revenue; customers with no completed order are not labelled inactive, since there is no purchase date to measure from.

## 5. What the 46 queries cover

| Section | Questions | Focus |
|---|---|---|
| A | Q1 to Q10 | SELECT, WHERE, ORDER BY, LIKE, DISTINCT |
| B | Q11 to Q20 | Aggregations, GROUP BY, revenue and cancellation metrics |
| C | Q21 to Q28 | INNER and LEFT JOIN, customer spending, segments |
| D | Q29 to Q34 | Subqueries, CTEs, category profitability, inactivity |
| E | Q35 to Q42 | RANK, DENSE_RANK, ROW_NUMBER, LAG, NTILE, running totals |
| F | Q43 to Q46 | Reporting view, PL/pgSQL function, value bands, management report |

**Advanced techniques used**

- **CTEs** to break multi-step analysis into readable stages
- **Window functions** for rankings, running totals and month-over-month change with `LAG`
- **Time-series analysis** with `DATE_TRUNC`
- **Segmentation** with `NTILE(4)` on completed-order spending
- **A reusable view**, `monthly_sales_summary`
- **A PL/pgSQL function** that returns completed revenue for a customer ID, and 0 when there is none
- **Extra analysis** in `06_advanced_analysis.sql`: customer metrics, RFM segmentation and discount-band analysis

## 6. Repository structure

```text
retail-sales-sql/
├── README.md
├── 01_create_database.sql
├── 02_create_tables.sql        (also supplied as schema.sql)
├── 03_insert_customers.sql
├── 04_insert_orders.sql
├── 05_verify_data.sql
├── queries.sql                 (Q1 to Q46)
├── 06_advanced_analysis.sql
├── insights.md
└── screenshots/
    ├── 05_verify_data.png
    ├── revenue_by_category.png
    ├── top_customers.png
    └── monthly_revenue.png
```

## 7. How to run it

**Requirements:** PostgreSQL, and either `psql`, pgAdmin or DBeaver.

```bash
# 1. Create the database (connect to postgres, not retail_analytics)
psql -U postgres -d postgres -f 01_create_database.sql

# 2. Create tables, then load customers before orders
psql -U postgres -d retail_analytics -f 02_create_tables.sql
psql -U postgres -d retail_analytics -f 03_insert_customers.sql
psql -U postgres -d retail_analytics -f 04_insert_orders.sql

# 3. Verify, then run the analysis
psql -U postgres -d retail_analytics -f 05_verify_data.sql
psql -U postgres -d retail_analytics -f queries.sql
psql -U postgres -d retail_analytics -f 06_advanced_analysis.sql
```

If PostgreSQL is not on the default port, add `-p <port>` to each command. Every script is safe to re-run. In pgAdmin, create the `retail_analytics` database from the menu instead of running file 01.

**Expected verification results:** 150 customers, 800 orders, 0 orders without a customer, 0 orders before registration, last order date 2026-09-30, and completed revenue of 24,155,371.70.

## 8. Quality checks

- Row counts and referential integrity verified with `05_verify_data.sql`
- Money metrics use Completed orders only; discounts are treated as percentages
- Customer counts use `COUNT(DISTINCT customer_id)`
- Inactivity is measured from the dataset's latest order date, not today's date
- The view and function run without errors, and the function returns 0 for an unknown customer
- Every insight in `insights.md` comes from a query output in this repository

## 9. Limitations and next steps

- The data is synthetic and covers 21 months, so the seasonality findings are indicative, not proven.
- Customers who register and never buy cannot be analysed for inactivity.
- **Next:** a monthly cohort-retention analysis, an `EXPLAIN ANALYZE` comparison before and after indexing, and a Power BI or Metabase dashboard built on the reporting view.

## 10. About the analyst

**Calvince Okoth, Data Analyst | Aspiring Data Engineer**

I work with PostgreSQL, SQL, Excel and Power BI, with a background in data annotation, quality assurance and structured data review. This project shows how I work: understand the business question, validate the data, write transparent SQL, interpret the evidence and recommend a clear action.

- **GitHub:** [Calvincekoth](https://github.com/Calvincekoth)
- **LinkedIn:** [Calvince Okoth](https://www.linkedin.com/in/calvince-okoth-85792b25a/)

---

*Duka Smart Electronics is a fictional business. All data is synthetic.*
