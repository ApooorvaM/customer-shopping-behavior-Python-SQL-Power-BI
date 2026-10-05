````markdown
# Customer Behaviour Analysis | Python + SQL Server + Power BI

An end-to-end customer analytics project that transforms raw CSV data into business insights using **Python, SQL Server, and Power BI**.

The project focuses on understanding customer purchasing behaviour, product performance, discount usage, subscription behaviour, customer loyalty, demographics, and purchasing patterns.

---

## 📌 Table of Contents

- [Project Overview](#-project-overview)
- [Business Problem](#-business-problem)
- [Project Objectives](#-project-objectives)
- [Tools & Technologies](#-tools--technologies)
- [Project Workflow](#-project-workflow)
- [Dataset](#-dataset)
- [Data Preparation & Cleaning](#-data-preparation--cleaning)
- [Feature Engineering](#-feature-engineering)
- [Exploratory Data Analysis](#-exploratory-data-analysis)
- [SQL Analysis](#-sql-analysis)
- [Power BI Dashboard](#-power-bi-dashboard)
- [Business Questions](#-business-questions)
- [Key Business Insights](#-key-business-insights)
- [Business Recommendations](#-business-recommendations)
- [Project Challenges & Learnings](#-project-challenges--learnings)
- [Project Structure](#-project-structure)
- [How to Run the Project](#-how-to-run-the-project)
- [Skills Demonstrated](#-skills-demonstrated)
- [Limitations](#-limitations)
- [Future Improvements](#-future-improvements)
- [Conclusion](#-conclusion)

---

# 📊 Project Overview

This project analyzes customer shopping behaviour through a complete data analytics pipeline.

Instead of treating Python, SQL, and Power BI as separate tools, the project connects them into one workflow:

**Raw CSV → Python Data Preparation → Feature Engineering → SQL Server → Business Analysis → Power BI Dashboard → Business Recommendations**

The objective was not only to analyze the data, but also to understand what the analysis could mean from a business perspective.

The analysis covers:

- Customer demographics
- Revenue contribution
- Product performance
- Customer ratings
- Discount behaviour
- Subscription behaviour
- Customer loyalty
- Purchase frequency
- Shipping preferences
- Age-group revenue
- Product popularity within categories

---

# 🎯 Business Problem

A retail business has customer-level shopping data but needs to understand:

- Which customer segments generate the most revenue?
- Do subscribers spend more than non-subscribers?
- Are repeat customers more likely to subscribe?
- Which products perform best?
- Which products depend heavily on discounts?
- Which customer groups contribute the most revenue?
- Does shipping type have any relationship with purchase value?
- Which products receive the highest customer ratings?

The goal is to transform these questions into measurable analyses and present the results in an interactive Power BI dashboard.

---

# 🎯 Project Objectives

The project was designed to:

1. Clean and validate the raw customer dataset.
2. Identify missing values and inconsistencies.
3. Standardize categorical variables.
4. Engineer useful analytical features.
5. Load the prepared data into SQL Server.
6. Answer business questions using SQL.
7. Analyze customer and product behaviour.
8. Build an interactive Power BI dashboard.
9. Convert analytical results into business recommendations.

---

# 🛠️ Tools & Technologies

| Tool | Purpose |
|---|---|
| Python | Data cleaning, EDA and feature engineering |
| Pandas | Data manipulation and transformation |
| NumPy | Numerical operations |
| Matplotlib / Seaborn | Exploratory visualizations |
| SQL Server | Business analysis and querying |
| SQLAlchemy | Python → SQL Server connection |
| pyodbc | SQL Server database connectivity |
| Power BI | Interactive dashboard and visualization |
| GitHub | Project documentation and version control |

---

# 🔄 Project Workflow

```text
                    ┌─────────────────┐
                    │   Raw CSV Data  │
                    └────────┬────────┘
                             │
                             ▼
                    ┌─────────────────┐
                    │     Python      │
                    │  Data Cleaning  │
                    │      + EDA      │
                    └────────┬────────┘
                             │
                             ▼
                    ┌─────────────────┐
                    │ Feature         │
                    │ Engineering     │
                    └────────┬────────┘
                             │
                             ▼
                    ┌─────────────────┐
                    │   SQL Server    │
                    │ Data Storage    │
                    └────────┬────────┘
                             │
                             ▼
                    ┌─────────────────┐
                    │ SQL Business    │
                    │    Analysis     │
                    └────────┬────────┘
                             │
                             ▼
                    ┌─────────────────┐
                    │    Power BI     │
                    │    Dashboard    │
                    └────────┬────────┘
                             │
                             ▼
                    ┌─────────────────┐
                    │    Business     │
                    │    Insights     │
                    └─────────────────┘
````

---

# 📁 Dataset

The dataset contains **3,900 customer records** and originally contains **18 columns**.

### Dataset attributes

| Column                 | Description                       |
| ---------------------- | --------------------------------- |
| Customer ID            | Unique customer identifier        |
| Age                    | Customer age                      |
| Gender                 | Customer gender                   |
| Item Purchased         | Product purchased                 |
| Category               | Product category                  |
| Purchase Amount (USD)  | Purchase value                    |
| Location               | Customer location                 |
| Size                   | Product size                      |
| Color                  | Product color                     |
| Season                 | Purchase season                   |
| Review Rating          | Customer review rating            |
| Subscription Status    | Subscription status               |
| Shipping Type          | Shipping method                   |
| Discount Applied       | Whether discount was applied      |
| Promo Code Used        | Whether promotional code was used |
| Previous Purchases     | Number of previous purchases      |
| Payment Method         | Payment method                    |
| Frequency of Purchases | Customer purchase frequency       |

---

# 🧹 Data Preparation & Cleaning

Python was used as the first stage of the analytical workflow.

## 1. Dataset inspection

The dataset was loaded using Pandas and inspected using:

* `df.info()`
* `df.describe()`
* `df.isnull().sum()`
* Unique-value checks
* Duplicate checks

The initial dataset contained:

* **3,900 rows**
* **18 columns**
* **37 missing Review Rating values**

---

## 2. Missing-value treatment

The missing values were present in the `Review Rating` column.

Instead of using one global median, missing ratings were replaced using the **median rating within each product category**.

```python
df['Review Rating'] = (
    df.groupby('Category')['Review Rating']
      .transform(lambda x: x.fillna(x.median()))
)
```

This preserves differences in rating behaviour between categories.

---

## 3. Standardizing purchase frequency

The dataset contained inconsistent labels representing the same purchase frequency.

For example:

```text
Fortnightly → Bi-Weekly
Every 3 Months → Quarterly
```

The values were standardized using:

```python
df['Frequency of Purchases'] = df['Frequency of Purchases'].replace({
    'Fortnightly': 'Bi-Weekly',
    'Every 3 Months': 'Quarterly'
})
```

---

## 4. Duplicate validation

Customer IDs were checked for duplicate occurrences.

The analysis showed that each Customer ID occurred once in the dataset.

Therefore, no duplicate customer records were removed.

---

## 5. Column standardization

Column names were converted into lowercase snake_case format.

For example:

```text
Customer ID
        ↓
customer_id

Purchase Amount (USD)
        ↓
purchase_amount
```

This made the dataset easier to work with in Python and SQL.

---

## 6. Redundant column check

`discount_applied` and `promo_code_used` were compared to determine whether both columns contained different information.

The comparison showed no records where the values differed.

Therefore, `promo_code_used` was identified as redundant for this dataset.

---

# ⚙️ Feature Engineering

Two new features were created to make the data more useful for analysis.

---

## 1. Age Group

Customers were grouped into four age categories:

| Age   | Age Group   |
| ----- | ----------- |
| < 25  | Young Adult |
| 25–34 | Adult       |
| 35–49 | Middle Aged |
| 50+   | Senior      |

Python implementation:

```python
def age_grp(x):
    if x < 25:
        return 'young_adult'
    elif 25 <= x < 35:
        return 'adult'
    elif 35 <= x < 50:
        return 'middle_aged'
    else:
        return 'senior'

df['age_group'] = df['age'].apply(age_grp)
```

This feature was later used for age-group revenue analysis.

---

## 2. Purchase Frequency in Days

The categorical purchase frequency was converted into a numeric interval.

| Purchase Frequency | Days |
| ------------------ | ---: |
| Weekly             |    7 |
| Bi-Weekly          |   14 |
| Monthly            |   30 |
| Quarterly          |   90 |
| Annually           |  365 |

This creates a numeric representation of purchase frequency that can be used for further analysis.

---

# 📈 Exploratory Data Analysis

Exploratory analysis was performed in Python before loading the prepared data into SQL Server.

The analysis examined:

* Customer demographics
* Product categories
* Purchase amount distribution
* Review ratings
* Subscription status
* Shipping types
* Discount usage
* Previous purchases
* Payment methods
* Purchase frequency
* Age groups

The purpose of EDA was to understand the dataset and identify data-quality issues before performing business analysis.

---

# 🗄️ Loading Data into SQL Server

After data preparation, the cleaned DataFrame was loaded into SQL Server.

The database used was:

```text
customer_behaviour
```

The table created was:

```text
customer
```

Python was connected to SQL Server using SQLAlchemy and the SQL Server ODBC driver.

Conceptually:

```text
Python DataFrame
       ↓
   SQLAlchemy
       ↓
    pyodbc
       ↓
 SQL Server
       ↓
 customer table
```

---

# 🔎 SQL Analysis

The SQL stage focuses on answering business questions rather than simply demonstrating SQL syntax.

The project uses:

* `GROUP BY`
* `SUM()`
* `AVG()`
* `COUNT()`
* `CASE WHEN`
* Subqueries
* CTEs
* `ROW_NUMBER()`
* `PARTITION BY`
* Conditional aggregation
* `TOP`
* `ORDER BY`
* `ROUND()`

---

# 💡 Business Questions

## Q1. What is the total revenue generated by male vs. female customers?

```sql
SELECT 
    gender,
    SUM(purchase_amount) AS total_revenue_by_gender
FROM customer
GROUP BY gender;
```

### Business purpose

Compare revenue contribution across genders.

---

## Q2. Which customers used a discount but still spent more than the average purchase amount?

```sql
SELECT 
    customer_id,
    purchase_amount
FROM customer
WHERE discount_applied = 'Yes'
  AND purchase_amount > (
      SELECT AVG(purchase_amount)
      FROM customer
  );
```

### Business purpose

Identify high-value customers who purchased using discounts.

---

## Q3. Which are the top 5 products with the highest average review rating?

```sql
SELECT TOP 5
    item_purchased,
    AVG(review_rating) AS avg_rating
FROM customer
GROUP BY item_purchased
ORDER BY avg_rating DESC;
```

### Business purpose

Identify products associated with stronger customer satisfaction.

---

## Q4. Compare average purchase amounts between Standard and Express Shipping.

```sql
SELECT 
    shipping_type,
    AVG(purchase_amount) AS avg_revenue_by_shipping
FROM customer
WHERE shipping_type IN ('Express', 'Standard')
GROUP BY shipping_type;
```

### Business purpose

Understand whether customers choosing different shipping options have different average purchase values.

---

## Q5. Do subscribed customers spend more?

```sql
SELECT 
    subscription_status,
    ROUND(AVG(purchase_amount), 2) AS avg_spend,
    ROUND(SUM(purchase_amount), 2) AS total_revenue
FROM customer
GROUP BY subscription_status
ORDER BY avg_spend DESC;
```

### Business purpose

Compare customer value between subscribers and non-subscribers.

---

## Q6. Which 5 products have the highest percentage of purchases with discounts applied?

```sql
SELECT TOP 5
    item_purchased,
    ROUND(
        100.0 *
        SUM(
            CASE 
                WHEN discount_applied = 'Yes' THEN 1
                ELSE 0
            END
        ) / COUNT(item_purchased),
        2
    ) AS pct
FROM customer
GROUP BY item_purchased
ORDER BY pct DESC;
```

### Business purpose

Identify products with high discount dependence.

---

## Q7. Segment customers into New, Returning and Loyal.

```sql
WITH cte_customer_segment AS
(
    SELECT 
        customer_id,
        previous_purchases,
        CASE
            WHEN previous_purchases < 10 
                THEN 'New'

            WHEN previous_purchases >= 10 
                 AND previous_purchases <= 25
                THEN 'Returning'

            WHEN previous_purchases > 25
                THEN 'Loyal'
        END AS customer_segment
    FROM customer
)

SELECT 
    customer_segment,
    COUNT(customer_id) AS customer_count
FROM cte_customer_segment
GROUP BY customer_segment;
```

### Segmentation logic

```text
< 10 previous purchases
        ↓
      New

10–25 previous purchases
        ↓
   Returning

> 25 previous purchases
        ↓
      Loyal
```

### Business purpose

Understand the distribution of customers across different loyalty levels.

---

## Q8. What are the top 3 most purchased products within each category?

```sql
WITH cte_rank AS
(
    SELECT 
        item_purchased,
        category,
        COUNT(item_purchased) AS item_count,

        ROW_NUMBER() OVER (
            PARTITION BY category
            ORDER BY COUNT(item_purchased) DESC
        ) AS rk

    FROM customer

    GROUP BY 
        item_purchased,
        category
)

SELECT 
    item_purchased,
    category,
    rk
FROM cte_rank
WHERE rk <= 3;
```

### Business purpose

Identify category-level product leaders instead of looking only at overall product popularity.

---

## Q9. Are repeat buyers also likely to subscribe?

```sql
SELECT 
    subscription_status,
    COUNT(customer_id)
FROM customer
WHERE previous_purchases > 5
GROUP BY subscription_status;
```

### Business purpose

Explore the relationship between repeat purchasing behaviour and subscription adoption.

---

## Q10. What is the revenue contribution of each age group?

```sql
SELECT 
    age_group,
    SUM(purchase_amount) AS revenue_by_age_group
FROM customer
GROUP BY age_group;
```

### Business purpose

Identify which age segments contribute most to revenue.

---

# 📊 Power BI Dashboard

The final stage of the project is an interactive Power BI dashboard.

The dashboard brings together the analysis from Python and SQL into a business-facing reporting layer.

## Dashboard Focus Areas

### Customer Overview

* Total customers
* Total revenue
* Average purchase amount
* Subscription status

### Customer Demographics

* Revenue by gender
* Revenue by age group
* Customer distribution by age group

### Product Performance

* Top products
* Product category performance
* Average review ratings
* Top products within each category

### Customer Loyalty

* New customers
* Returning customers
* Loyal customers
* Previous purchase behaviour

### Discount & Subscription Analysis

* Discount usage
* Products with high discount penetration
* Subscriber vs non-subscriber spending
* Repeat buyers vs subscription status

### Shipping Behaviour

* Standard vs Express purchase values
* Shipping type distribution

---

# 🖼️ Power BI Dashboard

## Dashboard Preview

> Replace the image path below with your actual dashboard screenshot.

```text
![Power BI Dashboard](images/powerbi_dashboard.png)
```


## 📸 Dashboard Screenshots

### Executive Overview

```markdown
![Executive Dashboard](<img width="428" height="241" alt="Screenshot 2026-10-05 120754" src="https://github.com/user-attachments/assets/bea43ab1-1bdc-4449-9744-28e109cbfdff" />
)
```


# 📥 Power BI File

Upload your Power BI `.pbix` file to the repository and add the link here.

```markdown
[Download Power BI Dashboard](dashboard.pbix)
```

Recommended folder:

```text
PowerBI/
└── customer_behaviour_analysis.pbix
```

---

# 🔗 Project Files

| File                                | Description                                       |
| ----------------------------------- | ------------------------------------------------- |
| `customer_behaviour_analysis.ipynb` | Python data cleaning, EDA and feature engineering |
| `customer_shopping_behavior.csv`    | Original dataset                                  |
| `customer_behaviour_analysis.sql`   | SQL business analysis queries                     |
| `customer_behaviour_analysis.pbix`  | Power BI dashboard                                |
| `images/`                           | Power BI dashboard screenshots                    |

---

# 🧠 Key Business Insights

The analysis was designed to answer the following business questions:

### Customer Value

* Which demographic segments generate the most revenue?
* Do subscribers have higher average spending?
* Which customers represent high-value discounted purchases?

### Customer Loyalty

* How many customers fall into New, Returning and Loyal segments?
* Are repeat buyers more likely to subscribe?

### Product Performance

* Which products receive the highest average ratings?
* Which products are most frequently purchased?
* Which products dominate each category?

### Promotions

* Which products have the highest discount penetration?
* Are discounts being used by high-value customers?

### Purchasing Behaviour

* How does shipping type relate to purchase amount?
* Which age groups contribute the most revenue?
* How frequently do customers purchase?

---

# 💼 Business Recommendations

Based on the analytical framework, the business can consider:

### 1. Segment customers by loyalty

Different customer segments should receive different retention strategies.

```text
New
 ↓
Onboarding / first-to-second purchase strategy

Returning
 ↓
Repeat purchase incentives

Loyal
 ↓
Retention + premium offers + subscription opportunities
```

---

### 2. Optimize discount strategy

Products with high discount penetration should be evaluated for:

* Margin impact
* Promotion dependency
* Incremental revenue
* Customer response

---

### 3. Use high-rated products strategically

Products with strong customer ratings can be considered for:

* Featured placement
* Cross-selling
* Upselling
* Promotional campaigns

---

### 4. Evaluate subscription value

Subscriber and non-subscriber behaviour should be compared before expanding subscription incentives.

The objective should be to identify whether subscription status is associated with stronger customer value.

---

### 5. Use demographic insights carefully

Age-group and gender revenue patterns can support targeted marketing and customer segmentation.

However, demographic characteristics should be considered alongside purchasing behaviour rather than being used in isolation.

---

# 🧩 Project Challenges & Learnings

## Challenge 1 — Missing data

There were missing values in the Review Rating column.

### Learning

Instead of automatically dropping missing records, I evaluated the data and used **category-level median imputation** to retain the observations while preserving category-specific behaviour.

---

## Challenge 2 — Inconsistent categorical labels

The purchase-frequency column contained different labels representing the same frequency.

For example:

```text
Fortnightly
Bi-Weekly
```

and

```text
Every 3 Months
Quarterly
```

### Learning

Data cleaning is not only about missing values. Inconsistent categorical labels can also affect analysis and aggregation.

---

## Challenge 3 — Redundant information

`discount_applied` and `promo_code_used` contained identical information in the dataset.

### Learning

Checking for redundant columns before analysis helps prevent unnecessary duplication and keeps the analytical dataset simpler.

---

## Challenge 4 — Connecting multiple tools

The project required moving from:

```text
Python → SQL Server → Power BI
```

### Learning

A real analytics workflow often requires more than one tool. Understanding how the tools connect is as important as knowing the individual syntax.

---

# 📂 Project Structure

```text
customer-behaviour-analysis/
│
├── README.md
│
├── data/
│   └── dataset.csv
│
├── Python/
│   └── EDA_in_python.ipynb
│
├── SQL/
│   └── SQLQuery1.sql
│
├── PowerBI/
│   └── dashboard.pbix


---

# ▶️ How to Run the Project

## Step 1 — Clone the repository

```bash
git clone https://github.com/YOUR_USERNAME/customer-behaviour-analysis.git
```

---

## Step 2 — Install Python libraries

```bash
pip install pandas numpy matplotlib seaborn sqlalchemy pyodbc
```

---

## Step 3 — Run the Python notebook

Open:

```text
Python/customer_behaviour_analysis.ipynb
```

Run the notebook to:

* Load the CSV
* Inspect the data
* Handle missing values
* Standardize categorical values
* Validate duplicates
* Engineer features
* Prepare the final dataset

---

## Step 4 — Load data into SQL Server

The cleaned DataFrame can be loaded into SQL Server using SQLAlchemy.

Database:

```text
customer_behaviour
```

Table:

```text
customer
```

---

## Step 5 — Run SQL analysis

Open:

```text
SQL/customer_behaviour_analysis.sql
```

Run the queries in SQL Server Management Studio.

---

## Step 6 — Open Power BI

Open:

```text
PowerBI/customer_behaviour_analysis.pbix
```

Refresh the data if required.

---

# 🧰 Skills Demonstrated

## Python

* Pandas
* Data cleaning
* Missing-value treatment
* Data profiling
* Duplicate validation
* Categorical standardization
* Feature engineering
* Exploratory Data Analysis

## SQL

* `SELECT`
* `WHERE`
* `GROUP BY`
* `ORDER BY`
* `SUM()`
* `AVG()`
* `COUNT()`
* `ROUND()`
* `CASE WHEN`
* Subqueries
* CTEs
* Window functions
* `ROW_NUMBER()`
* `PARTITION BY`
* Conditional aggregation

## Power BI

* Data visualization
* Dashboard development
* KPI reporting
* Business storytelling
* Interactive analysis

## Business Analysis

* Problem formulation
* Customer segmentation
* Product analysis
* Revenue analysis
* Discount analysis
* Subscription analysis
* Business recommendations

---

# ⚠️ Limitations

There are a few limitations to the current dataset.

### 1. No transaction date

The dataset does not contain a transaction/order date.

Therefore, the project cannot perform:

* Monthly revenue trends
* Year-over-year growth
* Cohort analysis
* Retention curves
* Seasonality over time

---

### 2. Previous purchases is not transaction history

`previous_purchases` provides a count rather than individual historical transactions.

Therefore, customer segmentation is based on the available purchase-count attribute rather than a full customer transaction history.

---

### 3. Customer-level dataset

Each Customer ID occurs once in the dataset.

Therefore, the analysis should be interpreted as analysis of observed customer records rather than a longitudinal transaction-level customer database.

---

# 🚀 Future Improvements

If transaction-level data becomes available, the project can be expanded with:

* Customer Lifetime Value (CLV)
* RFM segmentation
* Cohort analysis
* Customer retention analysis
* Churn prediction
* Monthly revenue trends
* Year-over-year growth
* Repeat purchase rate
* Average Order Value
* Discount effectiveness
* Customer acquisition analysis
* Product recommendation analysis
* A/B testing of promotional campaigns

A future version could also introduce statistical testing to determine whether observed differences between customer groups are statistically significant.

---

# 📌 Project Takeaway

The main goal of this project was not simply to demonstrate knowledge of Python, SQL or Power BI individually.

The goal was to demonstrate an **end-to-end analytics mindset**:

```text
Business Problem
       ↓
Data Understanding
       ↓
Data Quality
       ↓
Data Cleaning
       ↓
Feature Engineering
       ↓
Exploratory Analysis
       ↓
Business Questions
       ↓
SQL Analysis
       ↓
Power BI Reporting
       ↓
Business Insights
       ↓
Recommendations
```

This project demonstrates how raw customer data can be transformed into structured analysis and ultimately into business-facing insights.

---

# 👩‍💻 Author

**Apoorva Maheshwari**

Data Analyst | Python | SQL | Power BI | Excel

[LinkedIn](https://www.linkedin.com/in/apoorva-maheshwari-05b1b2b8/) • [GitHub](https://github.com/ApooorvaM)

---

⭐ If you found this project useful, feel free to explore the repository and connect with me.


