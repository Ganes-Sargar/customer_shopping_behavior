# 🛍️ Customer Shopping Behavior Analysis

An end-to-end data analytics project that explores 3,900 retail transactions to uncover customer segments, purchase drivers, and revenue trends — and turns those insights into an interactive Power BI dashboard and business recommendations.

**Author:** Ganesh Sargar ([Ganes-Sargar](https://github.com/Ganes-Sargar)) · B.Tech CSE (IoT, Cyber Security & Blockchain), Shivaji University — Annasaheb Dange College of Engineering & Technology (ADCET), Ashta

---

## 📖 Project Overview

Retail companies sit on huge volumes of transactional data, but raw data alone doesn't drive decisions — insight does. This project walks through a complete, real-world analytics workflow: taking a messy retail transactions dataset and turning it into clean data, structured SQL analysis, and a stakeholder-ready dashboard with concrete business recommendations.

The dataset covers **3,900 customer transactions across 18 attributes** — demographics (age, gender, location), purchase details (item, category, amount, season, size, color), and shopping behavior (discounts, promo codes, subscription status, review ratings, shipping type, payment method, purchase frequency).

**The core business question driving this project:**
> How can a retail company leverage consumer shopping data to identify trends, improve customer engagement, and optimize marketing and product strategies?

To answer it, the project was broken into four connected stages, each handled with a different tool suited to the job:

1. **Clean and prepare the data** in Python — handle missing values, standardize formatting, and engineer new features that make later analysis possible.
2. **Answer specific business questions** in SQL — load the cleaned data into PostgreSQL and write structured queries covering revenue, segmentation, discounts, and loyalty.
3. **Visualize the findings** in Power BI — build an interactive dashboard so non-technical stakeholders can filter and explore the data themselves.
4. **Translate findings into action** — summarize insights into a written report, a stakeholder presentation, and a set of business recommendations.

The result is a project that mirrors how a Data Analyst / Data Engineer would actually work in industry: moving data through Python → SQL → BI → business communication, rather than treating each as an isolated exercise.

### 🎯 Objectives

- Identify which customer segments (by gender, age, subscription status) generate the most revenue
- Understand how discounts and promo codes influence purchase behavior and product performance
- Classify customers into New, Returning, and Loyal segments based on purchase history
- Compare shipping preferences and their relationship to spend
- Surface the products and categories driving the most orders and the highest ratings
- Package all of the above into a dashboard and a set of actionable recommendations for the business

### 🧩 What Makes This Project End-to-End

Unlike a single-notebook analysis, this repo captures the *full pipeline* a working analyst would produce:

| Layer | What it shows |
|---|---|
| Raw business problem | A real stakeholder brief, not just a dataset |
| Data engineering | Cleaning, imputation, feature engineering, loading into a relational database |
| Analytical SQL | Window functions, CTEs, aggregations, subqueries — not just `SELECT *` |
| BI / Visualization | An interactive, filterable dashboard, not static charts |
| Business communication | A written report, a slide deck, and recommendations tied directly to the data |

---

## 📌 Business Problem

A retail company wants to understand shifting customer purchasing patterns across demographics, product categories, and sales channels, and to identify which factors — discounts, reviews, seasons, or payment preferences — drive repeat purchases and loyalty.

> **Question:** How can the company leverage consumer shopping data to identify trends, improve customer engagement, and optimize marketing and product strategies?

**Deliverables required by the brief:**
1. Data preparation and modeling in Python
2. Structured SQL analysis simulating business transactions
3. An interactive Power BI dashboard
4. A written report and stakeholder presentation
5. A well-organized GitHub repository containing all of the above

Full problem statement: [`docs/Business_Problem_Statement.pdf`](docs/Business_Problem_Statement.pdf)

---

## 🗂️ Dataset

| Detail | Value |
|---|---|
| Rows | 3,900 |
| Columns | 18 |
| Missing data | 37 values in `Review Rating` |
| Demographics | Age, Gender, Location |
| Purchase details | Item Purchased, Category, Purchase Amount, Season, Size, Color |
| Behavior signals | Discount Applied, Promo Code Used, Previous Purchases, Frequency of Purchases, Review Rating, Shipping Type, Payment Method, Subscription Status |

---

## 🧰 Tech Stack

| Stage | Tools |
|---|---|
| Data Cleaning & Feature Engineering | Python (pandas) |
| Structured Analysis | SQL (PostgreSQL) |
| Visualization | Power BI |
| Reporting | Markdown / PDF / PowerPoint |

---

## 📂 Repository Structure

```
├── data/                    # Raw dataset (not tracked in Git — see data/README.md)
├── notebooks/
│   └── Customer_Shopping_Behavior_Analysis.ipynb   # Cleaning, feature engineering, DB load
├── sql/
│   └── customer_behavior_sql_queries.sql           # 10 business-question SQL queries
├── dashboard/
│   └── customer_behavior_dashboard.pbix            # Power BI interactive dashboard
├── reports/
│   └── Customer_Shopping_Behavior_Analysis.pdf      # Full written report
├── presentation/
│   └── Customer-Shopping-Behavior-Analysis.pptx    # Stakeholder presentation
├── docs/
│   └── Business_Problem_Statement.pdf              # Original business brief
├── requirements.txt
├── .env.example
└── README.md
```

---

## 🧹 1. Data Preparation (Python)

- Loaded 3,900 rows × 18 columns with `pandas`
- Checked structure with `df.info()` and summary statistics with `df.describe()`
- Checked for null values across all columns with `df.isnull().sum()`
- Imputed 37 missing `review_rating` values using the **median rating per product category** (rather than a global median, to preserve category-level rating differences)
- Standardized column names to `snake_case` for readability and consistency (e.g. `Purchase Amount (USD)` → `purchase_amount`)
- Engineered two new features:
  - `age_group` — customers binned into four quartile-based groups: Young Adult / Adult / Middle-aged / Senior
  - `purchase_frequency_days` — numeric mapping of purchase frequency text into days (e.g. "Weekly" → 7, "Quarterly" → 90), enabling numeric analysis of purchase cadence
- Ran a data consistency check comparing `discount_applied` and `promo_code_used`, confirmed they were fully redundant, and dropped the duplicate column
- Connected the cleaned DataFrame to PostgreSQL (also included: reusable connection patterns for MySQL and SQL Server) and loaded it into a `customer` table for SQL analysis

📓 See [`notebooks/Customer_Shopping_Behavior_Analysis.ipynb`](notebooks/Customer_Shopping_Behavior_Analysis.ipynb)

---

## 🗃️ 2. SQL Analysis

Ten business questions were answered directly in PostgreSQL, using aggregations, subqueries, `CASE` logic, and window functions (`ROW_NUMBER() OVER (PARTITION BY ...)`):

1. **Revenue by gender** — total revenue generated by male vs. female customers
2. **High-spending discount users** — customers who used a discount but still spent above the average purchase amount
3. **Top 5 products by rating** — highest average review rating per item
4. **Shipping type comparison** — average purchase amount for Standard vs. Express shipping
5. **Subscribers vs. non-subscribers** — average spend and total revenue by subscription status
6. **Discount-dependent products** — top 5 products with the highest percentage of discounted purchases
7. **Customer segmentation** — New / Returning / Loyal, based on number of previous purchases
8. **Top 3 products per category** — using `ROW_NUMBER()` window function partitioned by category
9. **Repeat buyers & subscriptions** — whether customers with more than 5 previous purchases are more likely to subscribe
10. **Revenue by age group** — total revenue contribution of each engineered age group

📄 See [`sql/customer_behavior_sql_queries.sql`](sql/customer_behavior_sql_queries.sql)

---

## 📊 3. Key Insights

| Question | Finding |
|---|---|
| Revenue by gender | Male customers generated **$157,890** vs. **$75,191** from female customers |
| Discount-driven high spenders | 839 customers used a discount yet still spent above the average purchase amount |
| Top-rated products | Gloves (3.86), Sandals (3.84), Boots (3.82), Hat (3.80), Skirt (3.78) |
| Shipping type | Express shipping customers spend slightly more on average ($60.48 vs. $58.46) |
| Subscribers vs. non-subscribers | Non-subscribers generate far more total revenue ($170,436 vs. $62,645), though average spend is similar (~$59–60) |
| Discount-dependent products | Hat (50%), Sneakers (49.7%), Coat (49.1%), Sweater (48.2%), Pants (47.4%) of purchases used a discount |
| Customer segments | 3,116 Loyal · 701 Returning · 83 New |
| Top products per category | Jewelry, Sunglasses, Belt (Accessories) · Blouse, Pants, Shirt (Clothing) · Sandals, Shoes, Sneakers (Footwear) · Jacket, Coat (Outerwear) |
| Repeat buyers & subscriptions | Most repeat buyers (>5 purchases) are still non-subscribers (2,518 vs. 958) — loyalty isn't converting into subscriptions |
| Revenue by age group | Fairly even across groups, led by Young Adults ($62,143), followed by Middle-aged, Adult, and Senior |

**What these numbers suggest together:** the business already has a large, loyal customer base (3,116 "Loyal" customers) that is *not* converting into paid subscribers, and revenue is currently concentrated among non-subscribers and male customers — both signal clear, low-risk opportunities to grow subscription revenue and rebalance marketing spend toward underrepresented segments.

---

## 📈 4. Power BI Dashboard

An interactive dashboard lets stakeholders filter by subscription status, gender, category, and shipping type, and explore:

- **KPI cards:** number of customers, average purchase amount, average review rating
- **% of customers by subscription status** — donut chart
- **Revenue by category** and **sales by category** — bar charts
- **Revenue by age group** and **sales by age group** — bar charts
- **Slicers:** subscription status, gender, category, and shipping type, so any stakeholder can drill into a specific segment without touching SQL or Python

📁 Open [`dashboard/customer_behavior_dashboard.pbix`](dashboard/customer_behavior_dashboard.pbix) in Power BI Desktop to explore it live.

---

## 💡 5. Business Recommendations

- **Boost subscriptions** — promote exclusive benefits for subscribers, since non-subscribers currently drive the majority of revenue but aren't locked in
- **Customer loyalty programs** — reward repeat buyers to convert more Returning customers into the Loyal segment, and specifically target the large pool of loyal-but-unsubscribed customers
- **Review discount policy** — several products rely on discounts for close to 50% of sales; balance promotional pricing against margins
- **Product positioning** — feature top-rated and best-selling products more prominently in campaigns
- **Targeted marketing** — focus on high-revenue age groups and express-shipping users, who show slightly higher spend
- **Close the gender revenue gap** — investigate why female customers generate less than half the revenue of male customers, and whether product mix or marketing targeting is a factor

---

## ▶️ How to Run This Project Locally

1. **Clone the repo**
   ```bash
   git clone https://github.com/Ganes-Sargar/customer-shopping-behavior-analysis.git
   cd customer-shopping-behavior-analysis
   ```

2. **Set up a virtual environment and install dependencies**
   ```bash
   python -m venv venv
   source venv/bin/activate   # Windows: venv\Scripts\activate
   pip install -r requirements.txt
   ```

3. **Add the dataset**
   Place `customer_shopping_behavior.csv` inside the `data/` folder (see [`data/README.md`](data/README.md)).

4. **Set up local database credentials**
   ```bash
   cp .env.example .env
   # then edit .env with your local PostgreSQL credentials
   ```

5. **Run the notebook**
   ```bash
   jupyter notebook notebooks/Customer_Shopping_Behavior_Analysis.ipynb
   ```

6. **Explore the SQL queries** in `sql/customer_behavior_sql_queries.sql` against your loaded `customer` table.

7. **Open the dashboard** in Power BI Desktop: `dashboard/customer_behavior_dashboard.pbix`

---

## 🚀 Future Scope

- Add time-series analysis if transaction dates become available, to study seasonality beyond the existing `Season` column
- Build a churn-prediction or subscription-propensity model using scikit-learn on top of the cleaned dataset
- Extend the SQL layer with a proper star schema (fact/dimension tables) for a more scalable warehouse-style setup
- A/B test the discount-policy recommendation using a controlled subset of products

---

## 🔗 Connect

- GitHub: [Ganes-Sargar](https://github.com/Ganes-Sargar)
- LinkedIn: [ganesh-sargar](https://www.linkedin.com/in/ganesh-sargar-319386298)

---

*This project was completed as an independent data analytics case study covering the full pipeline: Python data cleaning → SQL analysis → Power BI visualization → business reporting.*
