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
| SQL Server | Business analysis and querying |
| SQLAlchemy | Python → SQL Server connection |
| pyodbc | SQL Server database connectivity |
| Power BI | Interactive dashboard and visualization |

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

## 1. Age Group

Customers were grouped into four age categories:

| Age   | Age Group   |
| ----- | ----------- |
| < 25  | Young Adult |
| 25–34 | Adult       |
| 35–49 | Middle Aged |
| 50+   | Senior      |



This feature was later used for age-group revenue analysis.



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

```
Python was connected to SQL Server using SQLAlchemy and the SQL Server ODBC driver.

Conceptually:

Python DataFrame
       ↓
   SQLAlchemy
       ↓
    pyodbc
       ↓
 SQL Server
       ↓
 customer table


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

 Q1. What is the total revenue generated by male vs. female customers?
 
 Q2. Which customers used a discount but still spent more than the average purchase amount?
 
 Q3. Which are the top 5 products with the highest average review rating?
 
 Q4. Compare average purchase amounts between Standard and Express Shipping.

 Q5. Do subscribed customers spend more?
 
 Q6. Which 5 products have the highest percentage of purchases with discounts applied?
 
 Q7. Segment customers into New, Returning and Loyal.
 
 Q8. What are the top 3 most purchased products within each category?
 
 Q9. Are repeat buyers also likely to subscribe?
 
 Q10. What is the revenue contribution of each age group?


# 📊 Power BI Dashboard

The final stage of the project is an interactive Power BI dashboard.

The dashboard brings together the analysis from Python and SQL into a business-facing reporting layer.

<img width="428" height="241" alt="Screenshot 2026-10-05 120754" src="https://github.com/user-attachments/assets/bf2224fb-a517-4fe6-bac2-c696f3582428" />




# 📥 Power BI File

Upload your Power BI `.pbix` file to the repository and add the link here.

[Download Power BI Dashboard](dashboard.pbix)



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


