# Duka Smart Electronics | Retail Sales & Customer Analytics

**A PostgreSQL portfolio project by Calvince Okoth**  
**Domain:** Retail Analytics · **Technology:** PostgreSQL · **Level:** Beginner to Advanced

> **From 950 retail records to business decisions.**  
> This project uses SQL to investigate customer value, product profitability, sales trends and customer inactivity for a growing Kenyan electronics retailer.

---

## 1. Executive overview

Duka Smart Electronics is a fictional Kenyan retailer selling phones, laptops, accessories, televisions and home appliances across ten towns. The business has recorded its customer and order activity since January 2025, but management has not yet used the data to guide decisions.

As the Junior Data Analyst, I use PostgreSQL to build a relational database, validate the records and answer 46 business questions. The analysis moves from foundational SQL queries to CTEs, window functions, a reporting view and a PL/pgSQL function.

The project is designed to answer four management priorities:

1. **Customer value:** Who are the highest-spending customers, and who may have stopped buying?
2. **Product economics:** Which products and categories generate revenue and gross profit?
3. **Sales momentum:** Is monthly revenue increasing or decreasing?
4. **Business action:** What should management prioritise based on the evidence?

The deliverable is not just a set of SQL exercises. It is a reproducible retail analytics workflow that turns transactional data into decision-ready information.

## 2. Dataset at a glance

| Dataset component | Volume / coverage |
|---|---:|
| Customers | 150 |
| Orders | 800 |
| Total records | 950 |
| Product categories | 5 |
| Customer locations | 10 Kenyan towns |
| Customer segments | Retail, Corporate, VIP |
| Payment methods | 4 |
| Order statuses | Completed, Pending, Cancelled |
| Trading history | January 2025 – September 2026 |

**Locations:** Nairobi, Mombasa, Kisumu, Nakuru, Eldoret, Thika, Nyeri, Meru, Machakos and Kiambu.

**Product categories:** Electronics, Accessories, Home Appliances, Computing and Mobile Phones.

The customer names are fictional. This dataset is intended for portfolio and analytical practice, not as a representation of a real company's confidential records.

## 3. Business rules and metric definitions

These definitions are applied consistently throughout the analysis.

| Measure | Definition |
|---|---|
| Revenue | `quantity * unit_price * (1 - discount / 100.0)` |
| Cost | `quantity * cost_price` |
| Gross profit | `revenue - cost` |
| Gross profit margin | `profit / NULLIF(revenue, 0) * 100` |
| Money metrics | Completed orders only |
| Order counts | All orders, regardless of status |
| Customer counts | Distinct customers |
| Latest data date | `MAX(order_date)` from the orders table |
| Reporting month | `DATE_TRUNC('month', order_date)` |
| Discount | Percentage; for example, `10` means 10% off |

**Important analytical choices**

- Revenue, cost, profit, spending and average order value include **Completed** orders only.
- Order counts include Completed, Pending and Cancelled orders unless a question explicitly asks for a particular status.
- The dataset's latest order date is used as the reference date for inactivity analysis. The computer's current date is not used.
- `NULLIF` protects margin calculations from division by zero.
- A customer's spending is the sum of completed-order revenue, after discount.
- In Q27, average customer spending is calculated across all customers in each segment; customers with no completed revenue contribute zero.
- In Q28, popularity is measured by the number of orders per category among VIP customers, regardless of status.
- In Q34, customers with no completed order are not classified as inactive; there is no completed-order date from which to measure inactivity.
- In Q41, all customers are included in the four spending groups, including customers with zero completed revenue.

## 4. Database model

The project uses two related tables.

### `customers`

| Column | Description |
|---|---|
| `customer_id` | Primary key; unique customer identifier |
| `customer_name` | Customer's fictional full name |
| `gender` | Male or Female |
| `city` | Customer's town |
| `registration_date` | Date the customer joined |
| `customer_segment` | Retail, Corporate or VIP |

### `orders`

| Column | Description |
|---|---|
| `order_id` | Primary key; unique order identifier |
| `customer_id` | Foreign key to `customers.customer_id` |
| `order_date` | Transaction date |
| `product_name` | Product ordered |
| `product_category` | Product category |
| `quantity` | Units ordered |
| `unit_price` | Selling price per unit before discount, KSh |
| `cost_price` | Cost per unit, KSh |
| `discount` | Discount percentage, from 0 to 100 |
| `payment_method` | M-Pesa, Cash, Card or Bank Transfer |
| `order_status` | Completed, Pending or Cancelled |

**Relationship:** One customer can place many orders; each order belongs to one customer.

## 5. Analytical coverage: 46 SQL questions

The `queries.sql` file contains all 46 questions, each labelled `-- Q1` through `-- Q46`.

| Section | Questions | Analytical focus |
|---|---:|---|
| A | Q1–Q10 | SELECT, filtering, sorting, pattern matching and DISTINCT |
| B | Q11–Q20 | Aggregations, grouping, revenue and cancellation metrics |
| C | Q21–Q28 | INNER/LEFT JOIN, customer spending and segmentation |
| D | Q29–Q34 | Subqueries, CTEs, category profitability and inactivity |
| E | Q35–Q42 | RANK, ROW_NUMBER, DENSE_RANK, LAG, NTILE and running totals |
| F | Q43–Q46 | Reporting view, PL/pgSQL function, customer value bands and management report |

### Advanced SQL demonstrated

- **CTEs:** break multi-stage analysis into readable, testable steps.
- **Window functions:** rank customers and products, calculate running totals and compare monthly results.
- **Time-series analysis:** use `DATE_TRUNC` and `LAG` to measure month-over-month performance.
- **Customer segmentation:** apply `NTILE(4)` to group customers by completed-order spending.
- **Reusable reporting:** create `monthly_sales_summary` as a database view.
- **Database programming:** create a PL/pgSQL function that returns completed-order revenue for a supplied customer ID.
- **Performance investigation:** use `EXPLAIN ANALYZE` as the stretch task to compare query plans before and after indexing.

## 6. Repository structure

```text
retail-sales-sql/
├── README.md
├── 01_create_database.sql
├── 02_create_tables.sql
├── 03_insert_customers.sql
├── 04_insert_orders.sql
├── 05_verify_data.sql
├── queries.sql
├── insights.md
└── screenshots/
    ├── 05_verify_data.png
    ├── revenue_by_category.png
    ├── top_customers.png
    └── monthly_revenue.png
```

Keep the original setup and data-loading scripts in the repository. The `queries.sql` file contains the 46 analytical answers. Add `insights.md` and screenshots after running the queries and verifying the actual results.

## 7. Setup and execution

### Prerequisites

- PostgreSQL
- pgAdmin or DBeaver, or the `psql` command-line client
- Git (for version control and publishing)

### Step 1 — Download the project files

Download all six supplied dataset files into one project folder:

- `README.md`
- `01_create_database.sql`
- `02_create_tables.sql`
- `03_insert_customers.sql`
- `04_insert_orders.sql`
- `05_verify_data.sql`

Add the supplied `queries.sql` file from this repository.

### Step 2 — Create the database

Connect to the default `postgres` database and run:

```bash
psql -U postgres -d postgres -f 01_create_database.sql
```

### Step 3 — Create tables

Connect to `retail_analytics`, then run:

```bash
psql -U postgres -d retail_analytics -f 02_create_tables.sql
```

### Step 4 — Load customers and orders

Run the customer script before the order script because orders reference customers through a foreign key.

```bash
psql -U postgres -d retail_analytics -f 03_insert_customers.sql
psql -U postgres -d retail_analytics -f 04_insert_orders.sql
```

### Step 5 — Verify the data

```bash
psql -U postgres -d retail_analytics -f 05_verify_data.sql
```

The verification should confirm **150 customers and 800 orders**. Save a screenshot of the successful output in `screenshots/`.

### Step 6 — Run the analysis

```bash
psql -U postgres -d retail_analytics -f queries.sql
```

You can also open `queries.sql` in pgAdmin or DBeaver and execute each numbered section individually. Check that the active connection is `retail_analytics`.

> If your setup scripts use a different database name or schema, follow the supplied scripts and update the connection instructions accordingly. Do not run the database-creation script while connected to the database it is trying to create.

## 8. Business insights and management recommendations

Create an `insights.md` file after executing the SQL. Include at least five findings. Each finding should contain:

1. **Evidence:** the actual SQL result, with the value and relevant period/category/customer.
2. **Interpretation:** what the result means for the business.
3. **Recommendation:** a specific action management could take.

Suggested insight areas:

- Top five customers by completed-order revenue.
- Highest-revenue and highest-gross-profit products.
- Most profitable product category by gross profit.
- Month-over-month revenue movement.
- Customers whose latest completed order is more than 90 days before the dataset's latest order date.
- Cancellation rate and the operational question it raises.

Do not invent findings or write recommendations as if a pattern has been proven before running the queries. Distinguish revenue from profit: a product with high sales is not necessarily the product with the highest gross profit.

## 9. Quality checks

Before publishing, confirm that:

- `05_verify_data.sql` reports 150 customers and 800 orders.
- Every question from Q1 to Q46 has a query under the matching comment.
- Completed-order-only rules are applied to all money metrics.
- Discounts are treated as percentages, not currency amounts.
- Customer counts use distinct customer identifiers where appropriate.
- Q34 uses the latest order date in the dataset.
- Q43's view counts all orders but calculates financial measures from Completed orders only.
- Q44 returns zero for a customer ID with no matching completed revenue.
- The view and function execute successfully.
- Business insights are supported by query outputs.

## 10. Assessment and project scope

The original assignment is designed for seven days and 100 marks.

| Assessment area | Weight |
|---|---:|
| Database setup and schema | 10% |
| SQL correctness — Q1–Q28 | 30% |
| SQL correctness — Q29–Q46 | 30% |
| Business insights | 20% |
| Code quality and documentation | 10% |

**Stretch goals**

- Use `EXPLAIN ANALYZE` on a slow query, add a justified index and compare execution plans.
- Build a monthly cohort-retention analysis to measure repeat purchasing.
- Connect PostgreSQL to Power BI, Excel or Metabase and create a one-page dashboard from the reporting view.

## 11. About the analyst

**Calvince Okoth — Data Analyst | Aspiring Data Engineer**

I am building practical capability in data analytics and data engineering, with an interest in using databases and analytical tools to solve real business problems.

My toolkit includes PostgreSQL, SQL, Excel and Power BI, supported by experience in data annotation, quality assurance and structured data review.

This project demonstrates my approach to analytical work: understand the business question, validate the data, write transparent SQL, interpret the evidence and communicate an actionable recommendation.

- **GitHub:** [Calvincekoth](https://github.com/Calvincekoth)
- **LinkedIn:** [Calvince Okoth](https://www.linkedin.com/in/calvince-okoth-85792b25a/)

---

**Project:** Duka Smart Electronics — Retail Sales & Customer Analytics  
**Database:** PostgreSQL · **Dataset:** 150 customers / 800 orders  
**Focus:** SQL Analytics · Customer Intelligence · Product Profitability · Business Intelligence

*Turning transactional data into decisions that move businesses forward.*
