# Customer Shopping Behavior Analysis

End-to-end data analytics project: clean a retail dataset with **Python**, store it in **PostgreSQL**, answer business questions with **SQL**, and present the findings in an interactive **Power BI** dashboard.

**Author:** Frejus Ibatta | [GitHub](https://github.com/Elombrasco) | [LinkedIn](https://linkedin.com/in/frejus-ibatta)

---

## Overview

A retailer wants to understand who its customers are and how they buy. This project answers questions such as:

- Which customer groups (gender, age, subscription status) generate the most revenue?
- Do subscribers or discount users spend more?
- Which products are rated highest and are the most discounted?
- How loyal is the customer base?

## Dataset

- **File:** `customer_shopping_behavior.csv`
- **Size:** 3,900 purchases, 18 columns
- **Content:** customer demographics (age, gender, location), purchase details (item, category, amount, size, color, season), and behavior (subscription, shipping type, discount, previous purchases, purchase frequency, review rating)
- **Data quality:** 37 missing values in `Review Rating`; no other nulls

## Tools

| Purpose | Tool |
|---|---|
| Data cleaning and EDA | Python (pandas), Jupyter Notebook |
| Database | PostgreSQL, SQLAlchemy, psycopg2 |
| Analysis | SQL (aggregations, CTEs, window functions, CASE) |
| Visualization | Power BI |

## Steps

### 1. Load the data (Python)
Loaded the CSV with pandas and inspected it with `head()`, `info()`, `describe()`.

### 2. Exploratory data analysis
Checked data types, summary statistics and missing values. Found 37 missing review ratings.

### 3. Data cleaning and feature engineering
- Filled missing `Review Rating` values with the **median rating of each product category**
- Standardized column names (lowercase, underscores)
- Created `age_group` (4 quartile-based groups: Young Adult 18-31, Adult 32-44, Middle-aged 45-57, Senior 58-70)
- Created `purchase_frequency_days` (converts labels like "Weekly" or "Quarterly" into days)
- Dropped `promo_code_used` after verifying it was identical to `discount_applied` in every row

### 4. Load into PostgreSQL
Connected with SQLAlchemy (credentials stored in a `.env` file, not committed) and wrote the cleaned DataFrame to a `customer` table.

### 5. SQL analysis
`customer_behavior_analysis.sql` contains 10 business queries:

1. Revenue by gender
2. Discount users who spent above the average purchase
3. Top 5 products by average rating
4. Average spend: Standard vs Express shipping
5. Subscribers vs non-subscribers (revenue and average spend)
6. Top 5 products by discount rate
7. Customer segmentation: New / Returning / Loyal
8. Top 3 products per category (window function)
9. Are repeat buyers more likely to subscribe?
10. Revenue by age group

## Dashboard

An interactive **Power BI** dashboard summarizes the key metrics for business users.

![Customer Behavior Dashboard](images/dashboard.png)

**What it shows**
- **KPI cards:** total customers (3.9K), average amount spent ($59.76), average review rating (3.75)
- **Subscription split:** 27% subscribers vs 73% non-subscribers (donut chart)
- **Revenue and sales by category:** Clothing and Accessories lead
- **Amount spent and sales by age group**
- **Slicers:** filter everything by subscription status, gender, category and shipping type

## Results

Key findings from the 3,900 purchases (total revenue: **$233,081**, average purchase: **$59.76**):

- **Revenue by gender:** male customers generate $157,890 vs $75,191 for female customers, mostly because they make about twice as many purchases (2,652 vs 1,248). Average spend per purchase is almost identical (about $60).
- **Subscription does not increase spend:** subscribers spend $59.49 on average vs $59.87 for non-subscribers. Only 27% of purchases come from subscribers, which suggests an opportunity to grow subscriptions.
- **Discounts are widely used:** 43% of purchases used a discount. Hats (50%), sneakers (49.7%) and coats (49.1%) are the most discounted items.
- **Shipping:** average spend is similar across shipping types ($58.46 to $60.73).
- **Loyalty:** 80% of customers (3,116) are "Loyal" (more than 10 previous purchases); only 83 are new.
- **Age:** revenue is spread fairly evenly across the four age groups; the youngest group (18-31) is slightly ahead at $62,143.
- **Categories:** Clothing drives the most revenue ($104,264), followed by Accessories ($74,200).
- **Ratings:** top-rated products are Gloves, Sandals and Boots, but averages are close (3.78 to 3.86).

**Business takeaway:** spending per purchase is remarkably uniform across segments, so growth should come from purchase volume, subscription uptake and targeting the most-discounted products, rather than from any single high-spending segment.

## How to Run

1. **Clone the repo**
   ```bash
   git clone https://github.com/Elombrasco/<repo-name>.git
   cd <repo-name>
   ```
2. **Install dependencies**
   ```bash
   pip install pandas psycopg2-binary sqlalchemy python-dotenv jupyter
   ```
3. **Create a `.env` file** with your PostgreSQL credentials
   ```
   POSTGRES_USER=your_user
   POSTGRES_PASSWORD=your_password
   POSTGRES_HOST=localhost
   POSTGRES_PORT=5432
   POSTGRES_DB=customer_behavior
   ```
4. **Create the database** `customer_behavior` in PostgreSQL.
5. **Run the notebook** `Customer_Shopping_Behavior_Analysis.ipynb` to clean the data and load the `customer` table.
6. **Run the queries** in `customer_behavior_analysis.sql` (pgAdmin, DBeaver or `psql`).

## Full Report

A detailed write-up of the method, findings and recommendations is available in [`Customer_Behavior_Analysis_Report.pdf`](Customer_Behavior_Analysis_Report.pdf).

## Repository Structure

```
├── customer_shopping_behavior.csv
├── Customer_Shopping_Behavior_Analysis.ipynb
├── customer_behavior_analysis.sql
├── Customer_Behavior_Analysis_Report.pdf
├── images/
│   └── dashboard.png
└── README.md
```

## Next Steps

- Add a presentation deck
- Add purchase dates to the analysis to study trends over time
