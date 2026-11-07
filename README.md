# Duka Smart Electronics
## Retail Sales, Customer Intelligence & Profitability Analytics

**Data Analytics Project | PostgreSQL | SQL | Business Intelligence**

**Analyst:** Calvince Okoth

---

## Business Overview

Duka Smart Electronics is a growing Kenyan electronics retailer serving customers across ten towns, with a product portfolio spanning mobile phones, computers, accessories, televisions and home appliances.

As the business expands, understanding sales performance, customer purchasing behaviour and product profitability becomes increasingly important.

This project focuses on transforming the company's transactional data into structured business intelligence that supports management decision-making.

Using PostgreSQL, I developed a relational data environment and conducted an in-depth analysis of customer activity, sales performance, product economics and revenue trends.

The objective is to identify business performance patterns, uncover opportunities and provide management with information that can support sustainable growth.

## Business Intelligence Objectives

The analysis focuses on five key business areas.

**1. Revenue Performance**
- Evaluate overall sales performance.
- Analyse monthly revenue trends.
- Identify changes in sales momentum.
- Examine order completion and cancellation patterns.

**2. Customer Intelligence**
- Identify the highest-value customers.
- Compare purchasing behaviour across customer segments.
- Analyse customer spending patterns.
- Identify potentially inactive customers.

**3. Product Performance**
- Determine revenue contribution by product.
- Identify high-performing product categories.
- Evaluate product costs and gross profitability.
- Compare sales performance against product economics.

**4. Customer Segmentation**
- Analyse Retail, Corporate and VIP customer groups.
- Rank customers according to completed-order spending.
- Identify valuable customer groups.
- Understand differences in purchasing activity.

**5. Management Reporting**
- Develop reusable monthly sales reporting.
- Create customer-level revenue calculations.
- Produce structured outputs to support business decisions.

## Data Overview

The analysis uses a fictional but structured retail dataset representing business activity from January 2025 to September 2026.

| Data component | Volume |
|---|---:|
| Customer records | 150 |
| Order records | 800 |
| Total records | 950 |
| Product categories | 5 |
| Customer locations | 10 |
| Customer segments | 3 |
| Payment methods | 4 |

**Business locations:** Nairobi, Mombasa, Kisumu, Nakuru, Eldoret, Thika, Nyeri, Meru, Machakos and Kiambu.

**Product categories:** Electronics, Accessories, Home Appliances, Computing and Mobile Phones.

*The dataset contains fictional customer information and is used to demonstrate a realistic retail analytics scenario.*

## Technology Stack

| Technology | Application |
|---|---|
| PostgreSQL | Relational database management |
| SQL | Data extraction, transformation and analysis |
| DBeaver | Database administration and query execution |
| Git | Version control |
| GitHub | Project documentation and collaboration |

## Analytical Methodology

The project follows a structured analytics workflow.

**Database Engineering**

- Designed a relational database structure.
- Established primary and foreign key relationships.
- Implemented data integrity rules.
- Loaded customer and transaction records.

**Data Validation**

- Verified record volumes.
- Checked customer-order relationships.
- Reviewed transaction statuses.
- Established consistent financial calculation rules.

**Exploratory and Business Analysis**

- Examined customer distribution and purchasing patterns.
- Analysed revenue by product and category.
- Evaluated gross profit and profit margins.
- Investigated monthly sales performance.
- Examined customer inactivity and cancellation rates.

**Advanced SQL Analytics**

Applied advanced PostgreSQL capabilities to perform multi-level business analysis, including:

- Common Table Expressions (CTEs).
- Subqueries and conditional aggregation.
- INNER JOIN and LEFT JOIN.
- Window functions and customer rankings.
- Monthly comparisons using LAG.
- Customer spending segmentation using NTILE.
- Running totals and cumulative calculations.

**Reporting and Database Programming**

- Developed a reusable `monthly_sales_summary` database view.
- Implemented a PL/pgSQL function for calculating completed-order customer revenue.
- Used structured SQL outputs to support management reporting.

## Business Metrics and Calculation Standards

Financial calculations follow consistent business rules.

| Metric | Calculation |
|---|---|
| Revenue | Quantity × Unit Price × (1 − Discount / 100) |
| Total Cost | Quantity × Cost Price |
| Gross Profit | Revenue − Total Cost |
| Gross Profit Margin | Gross Profit / Revenue × 100 |
| Average Order Value | Completed Revenue / Completed Orders |
| Cancellation Rate | Cancelled Orders / Total Orders × 100 |

All financial performance metrics use completed transactions only. Order activity metrics consider all order statuses unless otherwise specified.

This approach ensures that pending and cancelled transactions do not distort revenue and profitability reporting.

## Analytical Outputs

The project is structured to generate the following business intelligence outputs:

| Report | Business purpose |
|---|---|
| Customer Revenue Ranking | Understand customer contribution |
| Product Performance | Identify revenue-generating products |
| Category Profitability | Evaluate gross profit by category |
| Monthly Sales Trends | Monitor revenue movement |
| Customer Segmentation | Understand customer value |
| Customer Inactivity | Identify potential retention opportunities |
| Cancellation Analysis | Investigate transaction completion |
| Monthly Sales View | Support recurring performance reporting |

Actual findings and quantified recommendations are maintained separately in `insights.md` and are based on executed SQL results.

## Project Architecture

The repository is organised to maintain a reproducible analytics workflow.

```text
retail-sales-sql/
│
├── README.md
│
├── 01_create_database.sql
├── 02_create_tables.sql
├── 03_insert_customers.sql
├── 04_insert_orders.sql
├── 05_verify_data.sql
│
├── queries.sql
├── insights.md
│
└── screenshots/
    ├── 05_verify_data.png
    ├── revenue_by_category.png
    ├── top_customers.png
    └── monthly_revenue.png
```

## Business Impact

The analytical framework provides management with a clearer view of retail operations.

It supports:

- **Revenue management:** Understanding sales contribution and monthly performance.
- **Customer retention:** Identifying valuable customers and potential inactivity.
- **Product strategy:** Distinguishing high-revenue products from high-profit products.
- **Operational monitoring:** Understanding order completion and cancellation patterns.
- **Strategic planning:** Supporting decisions using measurable business performance indicators.

The resulting analysis provides a foundation for improving customer engagement, product prioritisation and revenue management.

## Future Development

The analytical environment can be extended through:

- Power BI integration for interactive executive dashboards.
- Automated sales reporting.
- Customer lifetime value analysis.
- Customer retention and cohort analysis.
- Inventory and demand forecasting.
- Query optimisation and indexing.

## About the Analyst

**Calvince Okoth | Data Analyst | Aspiring Data Engineer**

I am interested in building data solutions that connect database technologies, business analytics and decision-making.

My technical toolkit includes PostgreSQL, SQL, Microsoft Excel and Power BI, supported by experience in data annotation, quality assurance and structured data validation.

My approach to analytics is centred on understanding business requirements, maintaining data quality, developing reliable analytical solutions and translating data into meaningful business insights.

**Connect with me:**

- GitHub: https://github.com/Calvincekoth
- LinkedIn: https://www.linkedin.com/in/calvince-okoth-85792b25a/

---

**Project:** Duka Smart Electronics  
**Domain:** Retail & Commercial Analytics  
**Database:** PostgreSQL  
**Data Coverage:** 150 Customers | 800 Orders  
**Analytical Focus:** Revenue Performance | Customer Intelligence | Product Profitability | Business Reporting

*Turning retail data into business intelligence that drives better decisions.*
