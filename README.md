# [Supermart Analytics]
> *Querying the sales database to generate insights.*

---

## Project Type Flags

- Database creation
- Exploratory Data Analysis (EDA)
- SQL Analysis

---

## Table of Contents
1. [Project Overview](#1-project-overview)
2. [Objectives](#2-objectives)
3. [Project Scope & Tools](#3-project-scope--tools)
4. [Data Workflow](#4-data-workflow)
5. [Database Schema and Table Relationship](#5-Database-Schema-and-Table-Relationship)
6. [Key Insights](#6-key-insights)
7. [Recommendations](#7-recommendations)
8. [Author](#8-author)

---

## 1. Project Overview

SuperMart is a Nigerian retail chain operating across six regions (North, South, East, West, North Central, South-South), serving customers in 30 cities. The company sells 68 products across 8 
categories and is staffed by 35 employees organised into three tiers: Regional Managers, Sales Managers, and Sales Reps. 
As a Data Analyst, I have been tasked with querying the sales database to generate insights on revenue performance, employee effectiveness, customer behaviour, and inventory.
This project analysed transactional data from 2021 to 2024 to uncover actionable insights.

**Problem Statement:** How can Olist increase revenue, improve customer retention, and reduce logistics inefficiencies?  
Specifically:  
- Which products and regions drive revenue?  
- Why do customers not return after their first purchase?  
- What is causing orders cancellations by customers?
- How do freight costs vary by region and product category, and what does that imply for profitability?


  The SQL Queries used to analyze and aggregate the data for this project can be found here: (https://tinyurl.com/pufb695a)


---

## 2. Objectives

- **Primary Objective:** To identify the key drivers of revenue, customer churn, and logistics inefficiencies in the Olist marketplace
- **Secondary Objective 1:** To quantify sales performance
- **Secondary Objective 2:** To evaluate customer retention and logistics effectiveness

---

## 3. Project Scope & Tools

### Scope

| Dimension | Details |
|-----------|---------|
| **In Scope** | **Data sources:** 7 interconnected tables (`regions`, `categories`, `employees`, `customers`, `products`, `orders`, `order_items`).|
| **Out of Scope** | cost of goods sold data were excluded |
| **Time Period** | Jan 2021 - June 2024 |
| **Granularity** | order_items (each product in an order).  **Order‑level:** orders (each order). **Customer‑level:** customers (for segmentation and lifetime value). **Employee-level:** employees (for performance tracking).  **monthly aggregates** for time‑series analysis (revenue trends). |

### Tools & Technologies

| Category | Tool(s) Used |
|----------|-------------|
| Data Storage & Processing | MYSQL |
| Analysis | SQL queries |
| Version Control | GitHub |
| Documentation | Markdown |

---

## 4. Data Workflow

1. **Source:**  PostgreSQL script converted to MySQL for Supermart analytics.
   **Format:** SQL script with `CREATE TABLE` and `INSERT` statements.
   **Size:** 7 tables with sample data.

2. **Ingestion:** Ran the MySQL setup script in MySQL Workbench.
   **Database:** Created a `supermart` database and executed the script to create tables and insert data.

3. **Cleaning:** 
   **Data types:** Ensured columns were correctly typed (`INT`, `VARCHAR`, `DECIMAL`, `DATE`).
   **Foreign keys:** Added relationships to maintain data integrity.  

4. **Analysis:** SQL queries for EDA, aggregations, joins, subqueries, CTEs, and business reporting.  
   - **Key analysis:** Sales performance, customer segmentation, employee performance, product trends.

5. **Output:**  
  - **SQL scripts**  used for analysis.  
  - **This documentation** complete pipeline, findings, and recommendations.

---

## 5. Database Schema and Table Relationship

**Table > Key Columns** 
- **regions** > region_id, region_name 
- **categories** > category_id, category_name 
**employees** > employee_id, first_name, last_name, role, region_id, hire_date, salary 
**customers** > customer_id, first_name, last_name, email, city, country, registration_date 
**products** > product_id, product_name, category_id, unit_price, stock_quantity 
**orders** > order_id, customer_id, employee_id, order_date, status, shipping_city 
**order_items** > order_item_id, order_id, product_id, quantity, unit_price, discount 

Table Relationships 
regions ─────────< employees 
categories ──────< products 
customers ────────< orders >────── employees 
orders ───────────< order_items >── products 
---

## 6. Key Insights

**Insight 1: Revenue is volume‑driven, not price‑driven – most products are low‑price, low‑volume.**

 **Findings:** A scatter plot of price vs. quantity (bubble size = total revenue) showed that the vast majority of products cluster in the bottom‑left quadrant (low price, low volume). Only a handful of products drive revenue through high volume (bottom‑right) or high price (top‑left). The top 10 products by revenue and units sold account for a disproportionate share of sales (Health_Beauty, Watches_Gifts, Bed_Bath_Table, Sports_Leisure, Computer_Accessories, Furniture_Decor, Cool_Stuff, Housewares, Auto, Garden_Tools). 
 Visuals(https://tinyurl.com/ycxn9tau)
 Visuals(https://tinyurl.com/53b8zcc4)

 **Meaning:** Olist’s catalog is dominated by slow‑moving, low‑value items. The business relies on a few “hero” products. This creates vulnerability: if those products face stockouts or competition, overall revenue could drop significantly. Rationalising the portfolio, discontinue or discount bottom‑left products, promote heavy‑hitters, and experiment with bundling to move volume.

 


**Insight 2: 90% of customers never return, zero repeat purchases, over‑estimated delivery days and geographic concentration.**

**Findings:** Churn rate is 90% (customers with no purchase in the last 90 days). All customers are one‑time buyers and most of the converted leads from the total leads generated (B2B customers) were from an unknown channel (16.65%) . Cancellations in Sao Paulo (the largest market) are strongly correlated with estimated delivery days being far too high, even when the seller is geographically close. Scatter plot of distance (km) vs. estimated delivery days shows a cluster of canceled orders at short distance (<500 km) with high estimates (>15 days) which cost the Brazilian e-commerce R$95,235 in potential revenue.
Visuals(https://tinyurl.com/23errwv3)
Visuals(https://tinyurl.com/4z2b2vca)

 **Meaning:** The delivery estimation algorithm is broken for short distances. Customers trust the platform but are forced to cancel when they see unrealistic long promises. The lack of repeat purchases also signals no loyalty programme, no post‑purchase engagement, and no incentive to return. Fixing the estimate logic (e.g., reduce to 3‑5 days for short distances) could recover a significant portion of lost revenue and potentially improve retention. Also, add tracking parameters to uncover real source of generated leads for optimization

 


**Insight 3: Freight costs eat disproportionately into revenue for remote regions and heavy product categories.**

**Findings:** Freight cost as a percentage of product price is 2× higher in northern states (AM, RR, PA) than in Sao Paulo, even for identical products. Heavy categories (furniture, electronics) have freight percentages >20% in remote areas. Despite that, sellers are heavily concentrated in Sao Paulo, forcing long, expensive shipments.
Visuals(https://tinyurl.com/4z2b2vca)

**Meaning:** The current logistics model is unfair to both customers and sellers in remote regions. Olist is missing out on potential demand because shipping is prohibitively expensive and slow. Opening regional fulfilment centres (e.g., Manaus, Fortaleza) and incentivising local sellers could slash delivery times and freight costs, making those markets profitable.




**Insight 4: Credit cards dominate, but 52% of orders use instalments and long‑term instalments carry higher default risk.**

**Findings:** 75% of orders use credit cards, and 52% of orders are paid in instalments (1‑12+ instalments). Orders with 7+ instalments (12% of total) have 40% higher average order value but also show a higher rate of cancelled payments (as inferred from payment approval delays and cancellations). Full payment (1 instalment) accounts for 48% of orders.
Visuals(https://tinyurl.com/ycxn9tau)

**Meaning:** While instalments drive higher basket sizes, they also introduce financial risk. Olist should implement tiered fraud checks: flag orders with >6 instalments, high value, and new accounts for manual review. Also, offering a small discount for full payment could improve cash flow and reduce default exposure. The data supports that most customers can afford to pay upfront – 48% already do.

---

## 7. Recommendations

**1. Sales & Product Strategy**
- Since	customers are more inclined towards not too cost and not too cheap products hence, the top performing products being volume based. Promote top‑performing products by Featuring the 10 revenue‑driving Stock Keeping Units on homepage and in retargeting campaigns to stabilize and increase revenue. 
- Rationalize the catalog; Discontinue or discount bottom‑left quadrant products (low price, low volume) to reduce inventory costs and simplify operations. 

**2. Customer Retention & Churn Reduction**

- Though, the e-comm brand always acquire more customers which increases its revenue yearly but it struggles with one-time buyers and most of the products have a long replacement cycle. Launch a loyalty program to encourage repeat purchase and post-purchase email sequence can also be established to keep the brand on the top of mind
- Fix delivery estimation algorithm for short distances by reducing the estimates from 10–15 days to 5–7 days. This directly addresses the main reason for cancellations in São Paulo. It would help recover lost freight revenue and lower cancellations.

**3. Logistics & Freight Optimization**
- Open regional fulfilment centres; Pilot in Manaus (Amazonas) and Fortaleza (Ceará) to serve the North and Northeast. It would cut delivery time from >20 days to <7 days, reduce freight cost %. 
- Recruit sellers in underserved states to reduce average distance per order and improve delivery speed. some of the distant regions make more purchase.

**4. Payment & Risk Management**
- Offer a small discount for full payment (1-2%) to Incentivize customers to pay upfront, improve cash flow and reduce default risk. 

**5. Marketing & Lead Conversion**
- Shuffling between two channel is what is best but the best performing channel is unknown. The “unknown” channel should be investigated by adding a tracking parameter to all marketing campaigns to identify the source that currently drives 16% conversion (the highest) and scale the best‑performing channel while it substitutes with Paid search (12%) and can as well replace with Organic search (11%) to reduce cost of marketing.

---

## 8. Author

**[Eritosin Salami]**
[Data Analyst]

- 🔗 [www.linkedin.com/in/eritosin-salami]
- 💼 [https://github.com/EritosinSalami]
- 📧 [salamieritosinlearn@gmail.com]

---

*Last updated: [June 2026]*
