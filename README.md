# 🛍️ Customer Shopping Behavior Analysis

### End-to-End Data Analytics Project using Python, MySQL & Power BI

![Python](https://img.shields.io/badge/Python-3.x-blue?style=for-the-badge&logo=python&logoColor=white)
![Pandas](https://img.shields.io/badge/Pandas-Data%20Analysis-150458?style=for-the-badge&logo=pandas&logoColor=white)
![MySQL](https://img.shields.io/badge/MySQL-Database-4479A1?style=for-the-badge&logo=mysql&logoColor=white)
![Power BI](https://img.shields.io/badge/Power%20BI-Dashboard-F2C811?style=for-the-badge&logo=powerbi&logoColor=black)
![Jupyter](https://img.shields.io/badge/Jupyter-Notebook-F37626?style=for-the-badge&logo=jupyter&logoColor=white)

---

## 📊 Project Overview

This project analyzes customer shopping behavior for a retail business to identify purchasing patterns, customer segments, product performance, discount behavior, and factors influencing customer spending.

The project follows an **end-to-end data analytics workflow**, starting with data exploration and cleaning in Python, followed by advanced SQL analysis in MySQL and interactive business intelligence reporting in Power BI.

The goal is to transform raw customer data into **actionable business insights** that can support marketing, customer engagement, product strategy, and revenue optimization.

---

## 🎯 Business Problem

A retail company wants to better understand its customers' shopping behavior in order to:

- Improve customer engagement
- Increase sales and revenue
- Identify valuable customer segments
- Understand purchasing patterns
- Evaluate the impact of discounts
- Identify high-performing products
- Analyze subscription behavior
- Understand demographic differences in spending
- Optimize marketing and product strategies

### Key Business Question

> **How can the company leverage customer shopping data to identify trends, improve customer engagement, and optimize marketing and product strategies?**

---

# 🔄 Project Workflow

```text
                    RAW CUSTOMER DATA
                           │
                           ▼
                  ┌─────────────────┐
                  │     Python      │
                  │ Pandas + Jupyter│
                  └────────┬────────┘
                           │
                    Data Cleaning
                    Data Exploration
                   Feature Engineering
                           │
                           ▼
                  ┌─────────────────┐
                  │      MySQL      │
                  │  SQL Analysis   │
                  └────────┬────────┘
                           │
                    Business Queries
                    CTEs & Subqueries
                    CASE Statements
                    Window Functions
                           │
                           ▼
                  ┌─────────────────┐
                  │    Power BI     │
                  │   Dashboard     │
                  └────────┬────────┘
                           │
                           ▼
                 Interactive Insights
                  & Business Decisions
```

---

# 🗂️ Dataset

The dataset contains customer-level shopping information.

Each row represents a customer's latest purchase and contains information related to demographics, products, spending, promotions, shipping, subscriptions, and purchasing behavior.

### Main Features

| Column | Description |
|---|---|
| `customer_id` | Unique customer identifier |
| `age` | Customer age |
| `gender` | Customer gender |
| `item_purchased` | Product purchased |
| `category` | Product category |
| `purchase_amount` | Amount spent on the purchase |
| `location` | Customer/purchase location |
| `size` | Product size |
| `color` | Product color |
| `season` | Season associated with purchase |
| `review_rating` | Customer review rating |
| `subscription_status` | Subscription status |
| `shipping_type` | Shipping method |
| `discount_applied` | Whether a discount was applied |
| `previous_purchases` | Number of previous purchases |
| `payment_method` | Payment method |
| `frequency_of_purchases` | Purchase frequency |

---

# 🐍 Phase 1 — Data Analysis & Cleaning with Python

Python and Pandas were used for initial data exploration, cleaning, and feature engineering.

### Exploratory Data Analysis

The dataset was inspected using:

- `df.head()`
- `df.info()`
- `df.describe()`
- Missing-value analysis
- Categorical-value inspection
- Data consistency checks

### Missing Value Treatment

The `review_rating` column contained missing values.

Instead of replacing all missing values with the overall median, the missing ratings were imputed using the **median rating within each product category**.

This preserves differences between categories and provides a more context-aware imputation strategy.

```python
df['review_rating'] = (
    df.groupby('category')['review_rating']
      .transform(lambda x: x.fillna(x.median()))
)
```

### Column Standardization

Column names were standardized into **snake_case** to make them easier to work with in both Python and SQL.

Example:

```text
Customer ID → customer_id
Purchase Amount USD → purchase_amount
Review Rating → review_rating
```

### Feature Engineering

Two additional features were created:

#### 1. Age Group

Customers were segmented into four age groups using quantile-based binning:

- Young Adult
- Adult
- Middle-aged
- Senior

#### 2. Purchase Frequency Days

Text-based purchase frequencies were converted into numerical day intervals.

For example:

```text
Weekly → 7 days
Fortnightly → 14 days
Monthly → 30 days
Quarterly → 90 days
```

This makes purchase frequency easier to analyze quantitatively.

### Redundant Column Removal

`promo_code_used` was compared with `discount_applied`.

Since both columns contained the same information in this dataset, the redundant `promo_code_used` column was removed.

---

# 🗄️ Phase 2 — MySQL Analysis

The cleaned dataset was loaded into **MySQL** and analyzed using SQL.

The analysis focused on answering practical business questions rather than simply exploring the data.

## 🔍 Business Questions Answered

### 1. Revenue by Gender

Compared total revenue generated by male and female customers.

**Business use:** Helps identify differences in customer spending across demographic groups.

---

### 2. Customers Using Discounts but Spending Above Average

Identified customers who received a discount but still spent at or above the overall average purchase amount.

**Business use:** Helps identify customers who remain high-value even when promotional incentives are used.

---

### 3. Highest-Rated Products

Identified the top products based on average customer review rating.

**Business use:** Highly rated products can be prioritized in marketing campaigns and promotional strategies.

---

### 4. Standard vs Express Shipping

Compared average purchase amounts between standard and express shipping customers.

**Business use:** Helps evaluate whether customers choosing faster shipping demonstrate higher spending behavior.

---

### 5. Subscriber vs Non-Subscriber Spending

Compared:

- Customer count
- Average purchase amount
- Total revenue

between subscribers and non-subscribers.

**Business use:** Helps evaluate the commercial performance of the subscription program.

---

### 6. Products with the Highest Discount Rate

Calculated the percentage of purchases where discounts were applied for each product.

**Business use:** Identifies products that may rely heavily on discounts to generate sales.

---

### 7. Customer Loyalty Segmentation

Customers were segmented into:

| Segment | Previous Purchases |
|---|---:|
| New | 1 |
| Returning | 2–10 |
| Loyal | >10 |

This analysis was implemented using a SQL `CASE` statement and CTE.

**Business use:** Enables targeted strategies for different customer lifecycle stages.

---

### 8. Top 3 Products Within Each Category

Used SQL window functions to rank products within each category.

Key SQL concepts used:

- `CTE`
- `ROW_NUMBER()`
- `PARTITION BY`
- `GROUP BY`
- Aggregation

**Business use:** Helps identify the best-selling products within each product category.

---

### 9. Repeat Buyers & Subscription

Analyzed whether customers with more than five previous purchases were also subscribed.

**Business use:** Helps determine whether the subscription program is effectively capturing repeat customers.

---

### 10. Revenue by Age Group

Analyzed revenue contribution across customer age groups.

**Business use:** Helps identify the most valuable demographic segments for targeted marketing.

---

# 📈 Phase 3 — Power BI Dashboard

![Power BI Dashboard](images/dashboard_img.png)

The cleaned MySQL dataset was connected directly to **Power BI** to create an interactive customer behavior dashboard.

### Dashboard Features

The dashboard includes:

- 👥 Total Customers
- 💰 Average Purchase Amount
- ⭐ Average Review Rating
- 📊 Revenue by Category
- 📦 Sales by Category
- 👤 Revenue by Age Group
- 👥 Sales by Age Group
- 🧾 Subscription Status
- 🎯 Gender Filter
- 🛍️ Category Filter
- 🚚 Shipping Type Filter

### Dashboard Preview

> **Replace the image below with your actual Power BI screenshot.**

```markdown
![Customer Shopping Behavior Dashboard](images/customer_behavior_dashboard.png)
```

The dashboard is designed to allow users to interactively filter customer behavior by demographic and purchasing characteristics.

---

# 💡 Key Business Insights

The analysis revealed several useful patterns:

### 👥 Customer Loyalty

The dataset contains a large proportion of customers with significant previous purchase history, indicating a strong presence of returning/loyal customers.

### 💳 Subscription Opportunity

A significant portion of repeat buyers are not subscribed, suggesting an opportunity to improve subscription conversion through targeted offers and stronger membership benefits.

### 🏷️ Discount Dependency

Some products have substantially higher discount rates than others. These products should be monitored to determine whether discounts are driving incremental sales or simply reducing margins.

### 🚚 Shipping Behavior

Customers choosing express shipping demonstrate higher average purchase values in the analyzed dataset, suggesting a potential relationship between premium delivery preferences and customer spending.

### 👶 Demographic Revenue

Age-group analysis highlights differences in revenue contribution across customer segments, providing an opportunity for demographic-specific marketing strategies.

### ⭐ Product Quality

Products with consistently high review ratings can be prioritized for marketing campaigns, recommendations, and premium positioning.

---

# 📌 Business Recommendations

Based on the analysis, the following actions could be considered:

### 1. Improve Subscription Conversion

Target high-frequency and repeat customers who have not subscribed with personalized membership benefits or exclusive offers.

### 2. Optimize Discount Strategy

Identify products with high discount dependency and evaluate whether discounts are generating incremental revenue or unnecessarily reducing margins.

### 3. Promote High-Rated Products

Feature highly rated products prominently in marketing campaigns and recommendation systems.

### 4. Develop Customer-Specific Marketing

Use customer segments and demographics to create more targeted campaigns rather than using a single strategy for the entire customer base.

### 5. Evaluate Premium Delivery

Since express-shipping customers show higher average purchase values, the company could evaluate premium delivery options as part of a broader customer experience strategy.

### 6. Focus on High-Value Customer Segments

Prioritize loyal and high-spending customers with retention campaigns, personalized recommendations, and exclusive offers.

---

# 🛠️ Tools & Technologies

| Tool | Purpose |
|---|---|
| 🐍 Python | Data cleaning, manipulation & exploration |
| 🐼 Pandas | Data analysis and transformation |
| 📓 Jupyter Notebook | Development & EDA |
| 🗄️ MySQL | Database storage & SQL analysis |
| 📊 Power BI | Interactive dashboard & visualization |
| 🧮 SQL | Business analysis & data querying |
| 🔢 DAX | Power BI measures and calculations |
| 🔧 GitHub | Version control & portfolio |

---

# 🧠 SQL Concepts Demonstrated

This project includes practical implementation of:

- `SELECT`
- `WHERE`
- `GROUP BY`
- `HAVING`
- `ORDER BY`
- `CASE`
- Aggregate Functions
- Subqueries
- CTEs (`WITH`)
- `ROW_NUMBER()`
- `PARTITION BY`
- `JOIN` concepts
- Conditional aggregation
- Ranking
- Customer segmentation

---

# 📁 Project Structure

```text
customer-shopping-behavior-analysis/
│
├── 📂 data/
│   ├── customer_shopping_behavior.csv
│   └── cleaned_customer_shopping_behavior.csv
│
├── 📂 python/
│   └── customer_behavior_analysis.ipynb
│
├── 📂 sql/
│   └── customer_behavior_analysis.sql
│
├── 📂 powerbi/
│   └── customer_behavior_dashboard.pbix
│
├── 📂 images/
│   └── customer_behavior_dashboard.png
│
├── 📄 README.md
└── 📄 LICENSE
```

---

# 🔗 End-to-End Data Pipeline

```text
CSV Dataset
     ↓
Python / Pandas
     ↓
Data Cleaning
     ↓
Feature Engineering
     ↓
MySQL Database
     ↓
SQL Business Analysis
     ↓
Power BI
     ↓
Interactive Dashboard
     ↓
Business Insights & Recommendations
```

---

# 🚀 What I Learned

Through this project, I strengthened my practical understanding of:

- Data cleaning using Pandas
- Handling missing values intelligently
- Feature engineering
- Exploratory data analysis
- SQL-based business analysis
- Common Table Expressions
- Window functions
- Customer segmentation
- Conditional aggregation
- Connecting MySQL with Power BI
- Building interactive dashboards
- Translating analytical findings into business recommendations

Most importantly, this project helped me understand how **Python, SQL, and Power BI work together in a complete analytics workflow**.

---

# 👨‍💻 About Me

I'm a banking professional transitioning into **Data Analytics / Business Analytics**, with hands-on experience in MIS reporting, Excel dashboards, sales performance tracking, and business reporting.

I'm currently expanding my technical skills in:

**SQL • Python • Pandas • Power BI • Excel • Data Visualization**

This project is part of my data analytics portfolio and demonstrates my ability to take a dataset from **raw data → cleaning → analysis → visualization → business insights**.

---

⭐ **If you found this project useful, feel free to star the repository!**

**Made with Python, SQL & Power BI.**
