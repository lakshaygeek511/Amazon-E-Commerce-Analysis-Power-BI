<div align="center">

# 🛒 Amazon E-Commerce Sales Analysis ( FreeLance Project )
### Power BI + SQL End-to-End Business Intelligence Project

![Power BI](https://img.shields.io/badge/Power%20BI-F2C811?style=for-the-badge&logo=powerbi&logoColor=black)
![SQL](https://img.shields.io/badge/SQL-4479A1?style=for-the-badge&logo=mysql&logoColor=white)
![DAX](https://img.shields.io/badge/DAX-FF6B6B?style=for-the-badge&logo=microsoft&logoColor=white)
![Power Query](https://img.shields.io/badge/Power%20Query-217346?style=for-the-badge&logo=microsoft&logoColor=white)

</div>

---

## 📌 Project Overview

This project delivers a comprehensive analysis of **Amazon E-Commerce sales data** using **Power BI** and **SQL** to uncover key business insights across customer behavior, product performance, delivery efficiency, and revenue trends for one of my freelance clients. An NDA is enforce for this project so i am not allowed to disclose the official files on public platform.
However this ReadMe shows Snaps of Dashboards & info of dataset built in Power BI for my client.

As a **Business Analyst at Amazon**, the objective was to empower stakeholders with data-driven insights to:

- ✅ Maintain and sustain current performance levels
- 📈 Identify strategic growth opportunities
- 😊 Improve overall customer experience
- 🚚 Optimize delivery and product performance
- 💡 Provide actionable, data-backed recommendations

---

## 📊 Dashboard

### 🖥️ Executive Overview ( Main Page )
<img width="1540" height="964" alt="image" src="https://github.com/user-attachments/assets/2cf4d460-fd12-4be4-bf30-b1af595485ec" />

---

### 📦 Products Analysis
<img width="1596" height="996" alt="image" src="https://github.com/user-attachments/assets/b29e74a8-267a-4a81-b65b-20484e009168" />


---

### 🔍 Individual Product Deep Dive
<img width="1536" height="972" alt="image" src="https://github.com/user-attachments/assets/f8b7d3c2-d399-4fac-91e0-22080b89b4b9" />


---

## 📊 Dataset Information

| Property | Details |
|----------|---------|
| **Tables Used** | Customers, Orders |
| **Dataset Size** | ~1.13 Lakh Rows (~1,13,000 records) |
| **Unique Products** | 44 |
| **Data Range** | 2015 – 2020 |

**Tools & Technologies:**

| Tool | Purpose |
|------|---------|
| **Power BI** | Dashboard Development & DAX Measures |
| **SQL** | Advanced Queries & Analytics |
| **Power Query (M Language)** | Data Cleaning & Transformation |

---

## 🧹 Data Cleaning & Preparation

Data was cleaned and prepared using **Power Query** before loading into Power BI:

- 🗑️ Removed last 5 columns in Orders table (columns 18–22) — consisted entirely of null values
- 🗑️ Deleted rows with null values in critical columns like `OrderID`, `ProductName`, `Unit Price` (~1% of data)
- 📋 Rearranged column order in Orders table for better readability
- 🔄 Standardized data types — Date, Numeric, and Text columns
- 🔗 Established a **Many-to-One** relationship between Orders and Customers tables using `CustomerID`
- 🧩 Replaced missing `Reason` values using a DAX calculated column:

```dax
Reason = IF(
    Orders[Status] = "Delivered", "Not Applicable",
    IF(
        Orders[Status] = "Returned" && ISBLANK(Orders[Reason]),
        "Unknown Reason",
        Orders[Reason]
    )
)
```

- 📐 Created calculated columns: `Delivery Days`, `Customer Category`
- 🔍 Retrieved Customer Age into Orders table using `LOOKUPVALUE()`:

```dax
Customer Age = LOOKUPVALUE(
    Customers[Customer Age],
    Customers[CustomerID],
    Orders[CustomerID]
)
```

- ✅ Verified data consistency and formatting throughout

> The dataset was transformed into a **structured, analysis-ready format** with no formatting issues detected.

---

## 📈 Key Business Metrics

### 💰 Total Sales
```dax
Total Sales = SUM(Orders[Sale Price])
```
> **Total Sales: 107.24 Million**

---

### 🧾 Total Orders
```dax
Total Orders = COUNT(Orders[OrderID])
```

---

### 👥 Unique Customers
```dax
Unique Customers = DISTINCTCOUNT(Orders[CustomerID])
```

---

### 🛍️ Unique Products
```dax
Unique Products = DISTINCTCOUNT(Orders[Product])
```
> **44 unique products available**

---

### 📦 Total Returned Products
```dax
Total Returned Products = CALCULATE(
    COUNTROWS(Orders),
    Orders[Status] = "Returned"
)
```
> **Total Returns: 31K**

---

### 🚚 Average Delivery Days
```dax
Delivery Days = DATEDIFF(Orders[OrderDate], Orders[Delivery Date], DAY)

Avg Delivery Days = CALCULATE(
    AVERAGE(Orders[Delivery Days]),
    Orders[Status] = "Delivered"
)
```
> **Average Delivery Time: 9.41 Days**

---

### 🗓️ Previous Year Monthly Sales (Time Intelligence)
```dax
Calendar = CALENDAR(MIN(Orders[OrderDate]), MAX(Orders[OrderDate]))

Prev_Year_Month_Revenue = CALCULATE(
    [Total Sales],
    SAMEPERIODLASTYEAR('Calendar'[Date])
)
```

---

### 🏷️ Fashion Category + Shipped from Abroad Sales
```dax
-- Total sales for Product Category "Fashion" & Delivery Type "Shipped from Abroad"
-- Result: 4.14M
```

---

## 📊 Key Dashboard Insights

### 1️⃣ Revenue Trends

- 📉 Stable revenue from **2015–2019** (~16–17M per year with minimal deviation)
- 🚀 **Major spike in 2020** — revenue jumped to ~24M (~50% increase), driven by customer growth of ~7.7K new customers
- 📅 **January** recorded the highest monthly revenue (**10.1M**)
- 📉 **June and September** were the lowest revenue months (under 7M)

---

### 2️⃣ Product Category Performance

| Category | Revenue |
|----------|---------|
| 📱 Phones & Tablets | **39M** (Highest) |
| 💻 Electronics | **33M** |
| 👗 Fashion | **12M** |
| 💄 Health & Beauty | **12M** |
| 🏠 Home & Office | **12M** |

> **Top Sub-Categories:** Digital Cameras & Tablets (highest revenue)
> **Top Products:** Canon EOS 600D, Canon EOS 60D, Amazon Fire HD

**Low-Selling Products:** Hemani Ultra Slime Tea, Clera Radiance Oil Control Toner, Yazole Leather Wrist Watch

---

### 3️⃣ Customer Segmentation (Loyalty Buckets)

```dax
Customer Category =
IF(Orders[Sale Price] > 5000, "Platinum",
IF(Orders[Sale Price] > 3000, "Gold",
IF(Orders[Sale Price] > 1000, "Silver", "Bronze")))
```

| Category | Customers | Revenue | Insight |
|----------|-----------|---------|---------|
| 🥉 Bronze | 87K | 30M | Volume-driven — most customers |
| 🥈 Silver | 15K | 26M | Mid-tier spending |
| 🥇 Gold | 7K | 29M | High revenue per customer |
| 💎 Platinum | 3K | 23M | Comparable revenue to larger tiers |

> Gold and Platinum customers generate **disproportionately high revenue** relative to their count.

---

### 4️⃣ Delivery Performance

| Delivery Type | Avg. Delivery Days |
|---------------|-------------------|
| ⚡ Express | **3.5 Days** |
| 📦 Standard Delivery | **10 Days** |
| 🌍 Shipped from Abroad | **15 Days** |

> - All product categories have a **consistent average wait time of ~9.5 days**
> - Shipping cost is **equal across all delivery types** — customers may prefer Express over Standard
> - No relationship found between **product category and shipping charges**

---

### 5️⃣ Product Returns Analysis

| Category | Return Orders |
|----------|--------------|
| 💄 Health & Beauty | **9.7K** (Highest) |
| 👗 Fashion | **9K** |
| 💻 Electronics | Lowest returns |

---

### 6️⃣ Ratings & Sales Relationship

- 📊 **Low-rated products (1–3) generate higher total revenue** than high-rated ones — suggesting popularity does not guarantee satisfaction
- 🔄 Returned products predominantly belong to **rating 1, 2, and 3**
- 💰 No significant relationship found between **unit price and product rating**
- 📏 Average product price shows minimal variation across rating groups

---

### 7️⃣ Location-Based Revenue

**Top 5 Revenue-Generating Regions:**

1. 📍 Greater Accra
2. 📍 Ashanti
3. 📍 Western
4. 📍 Weija
5. 📍 Upper West

> These **top 5 locations contribute over 70%** of total revenue — strong geographic demand clusters.

---

## 🧠 Advanced SQL Analysis

Comprehensive SQL analytics covering:

- 🏆 Top 5 Valuable Customers (Composite Score)
- 📈 Month-over-Month Growth Rate
- 📉 Rolling 3-Month Average Revenue by Category
- 📊 Revenue Growth vs. Previous Year
- 💰 Customers generating 30%+ above average revenue
- 🔝 Top 3 Product Categories with Highest Sales Increase
- ⏱️ Consecutive Order Gap Analysis
- 💸 Discount Application (15% for loyal customers with 10+ orders)

---

### Query 1 — Top 5 Most Valuable Customers (Composite Score)

```sql
SELECT
    CustomerID,
    SUM(SalePrice)   AS Total_Revenue,
    COUNT(OrderID)   AS Order_Frequency,
    AVG(SalePrice)   AS Avg_Order_Value,
    (
        (SUM(SalePrice)  * 0.5) +
        (COUNT(OrderID)  * 0.3) +
        (AVG(SalePrice)  * 0.2)
    ) AS Composite_Score
FROM orders
GROUP BY CustomerID
ORDER BY Composite_Score DESC
LIMIT 5;
```

---

### Query 2 — Month-over-Month Revenue Growth Rate

```sql
WITH month_group AS (
    SELECT
        LEFT(OrderDate, 7) AS Month,
        SUM(SalePrice)     AS Total_revenue
    FROM Orders
    GROUP BY LEFT(OrderDate, 7)
),
prev_data AS (
    SELECT *,
        LAG(Total_revenue) OVER (ORDER BY Month) AS Previous_month_revenue
    FROM month_group
)
SELECT *,
    ROUND(((Total_revenue - Previous_month_revenue) * 100) / Previous_month_revenue, 2) AS Growth_rate
FROM prev_data;
```

---

### Query 3 — Rolling 3-Month Average Revenue by Category

```sql
WITH category_data AS (
    SELECT
        ProductCategory,
        LEFT(OrderDate, 7) AS Month,
        SUM(SalePrice)     AS Total_revenue
    FROM Orders
    GROUP BY ProductCategory, LEFT(OrderDate, 7)
)
SELECT *,
    ROUND(AVG(Total_revenue) OVER (
        PARTITION BY ProductCategory
        ORDER BY Month
        ROWS BETWEEN 2 PRECEDING AND CURRENT ROW
    ), 2) AS Rolling_average_3_month
FROM category_data;
```

---

### Query 4 — Customers with 30%+ Above Average Revenue

```sql
WITH Customer_data AS (
    SELECT
        CustomerID,
        SUM(SalePrice) AS Total_revenue
    FROM orders
    GROUP BY CustomerID
)
SELECT *
FROM Customer_data
WHERE Total_revenue > (SELECT AVG(Total_revenue) * 1.3 FROM Customer_data)
ORDER BY Total_revenue DESC;
```

---

### Query 5 — Top 3 Product Categories by Sales Increase

```sql
WITH year_sales AS (
    SELECT
        ProductCategory,
        YEAR(OrderDate)  AS order_year,
        SUM(SalePrice)   AS total_sales
    FROM orders
    GROUP BY ProductCategory, YEAR(OrderDate)
),
sales_growth AS (
    SELECT *,
        LAG(total_sales) OVER (
            PARTITION BY ProductCategory
            ORDER BY order_year
        ) AS previous_sales
    FROM year_sales
)
SELECT *,
    (total_sales - previous_sales) AS sales_increase
FROM sales_growth
ORDER BY sales_increase DESC
LIMIT 3;
```

---

### Query 6 — 15% Discount for High-Frequency Customers (10+ Orders)

```sql
WITH customer_list AS (
    SELECT CustomerID
    FROM orders
    GROUP BY CustomerID
    HAVING COUNT(*) >= 10
)
UPDATE orders
SET SalePrice = SalePrice * 0.85
WHERE CustomerID IN (SELECT * FROM customer_list);
```

---

### Query 7 — Average Days Between Consecutive Orders

```sql
WITH qualify AS (
    SELECT CustomerID
    FROM orders
    GROUP BY CustomerID
    HAVING COUNT(*) >= 5
),
Orders_data AS (
    SELECT
        CustomerID,
        OrderDate,
        DATEDIFF(OrderDate, LAG(OrderDate) OVER (
            PARTITION BY CustomerID ORDER BY OrderDate
        )) AS day_diff
    FROM orders
    WHERE CustomerID IN (SELECT CustomerID FROM qualify)
),
Average_data AS (
    SELECT CustomerID, AVG(day_diff) AS avg_diff
    FROM Orders_data
    GROUP BY CustomerID
)
SELECT AVG(avg_diff) AS Average_days_between_orders
FROM Average_data;
```

---

## 💡 Key Insights Summary

| # | Insight |
|---|---------|
| 1 | Revenue is **heavily driven by Phones & Electronics** categories |
| 2 | **2020 saw a ~50% revenue spike** — driven by 7.7K new customer growth |
| 3 | **Low ratings (1–3) dominate** over high ratings (4–5), with higher return rates |
| 4 | **31K product returns** — quality improvement required, especially in Health & Beauty and Fashion |
| 5 | **Bronze customers dominate by count** but not by per-user revenue |
| 6 | **Delivery time is consistent** (~9.5 days) across all product categories |
| 7 | **No relationship** found between shipping charges and product category |
| 8 | **Top 5 locations account for 70%+** of total revenue |
| 9 | Second half of year shows **significantly lower revenue** than first half |
| 10 | Canon EOS 600D & 60D are **high-priced but underperforming** in sales volume |

---

## 🚀 Strategic Recommendations

### 🛍️ Product & Revenue
- Focus marketing on **Phones, Tablets & Electronics** — top revenue drivers
- **Bundle low-selling products** with best-sellers + offer combo discounts
- Run **special promotions for rating 4–5 products** to boost sales volume
- Provide **EMI options** for high-cost products (Canon DSLRs) to improve accessibility

### 👥 Customer Loyalty
- **Convert Bronze → Silver** via personalized offers, coupons, and product pairings
- **Retain Platinum customers** with exclusive deals on premium products
- **Target Gold customers** — highest potential for Platinum conversion
- Implement a **formal loyalty rewards program** with points, cashback, and tier benefits

### 🚚 Delivery & Operations
- **Promote Express Delivery** to improve satisfaction (reduces wait by ~6.5 days vs Standard)
- **Optimize international shipping logistics** to reduce the 15-day wait
- **Expand warehouses in top cities** to achieve 4–5 day local delivery
- Focus faster delivery on **high-revenue categories** for better customer satisfaction

### 📍 Regional & Seasonal Marketing
- **Double down on Greater Accra & Ashanti** — introduce new products in leading regions
- Design **custom regional strategies** for Western, Weija & Upper West
- Partner with **local social media influencers** to boost regional sales
- Run targeted promotions in the **second half of the year** (June–December) to offset the seasonal dip
- Leverage **festival & holiday sales** (Christmas, regional events) for Q3–Q4 campaigns

### ⭐ Ratings & Quality
- **Improve product quality** in Health & Beauty and Fashion to reduce 31K returns
- Show **exact product photos** and detailed size/spec charts to manage expectations
- **Investigate low-rated products** through customer surveys and feedback loops
- Maintain product quality rigorously **during major sale events**

---

## 🛠️ Skills Demonstrated

**Power BI & DAX**
- Dashboard Development (Executive, Category, Product-level views)
- DAX Functions: `CALCULATE`, `DISTINCTCOUNT`, `DATEDIFF`, `LOOKUPVALUE`, `SAMEPERIODLASTYEAR`, `COUNTROWS`, `IF`, `FILTER`
- Time Intelligence: Calendar Table, Previous Year Comparisons
- Calculated Columns & Measures

**SQL**
- CTEs (Common Table Expressions)
- Window Functions: `LAG()`, `AVG() OVER()`, `PARTITION BY`
- Aggregation Functions: `SUM`, `COUNT`, `AVG`
- `UPDATE` with Subqueries
- Month-over-Month & Year-over-Year Analysis

**Data Engineering**
- Power Query / M Language for data transformation
- Data Modeling & Relationship Management (Many-to-One)
- Data Cleaning & Quality Assurance

**Business Intelligence**
- Customer Segmentation & Loyalty Bucketing
- Product Performance Analysis
- Revenue Trend Analysis
- Geographical Sales Distribution
- Delivery Efficiency Evaluation
- Business Strategy Development

---

## 🎯 Conclusion

This project demonstrates a **complete end-to-end business intelligence pipeline** — from raw data cleaning and transformation to advanced SQL analytics, interactive Power BI dashboards, and strategic business recommendations.

The analysis enables Amazon to:
- 📊 Monitor KPIs in real-time through interactive dashboards
- 🎯 Target the right customers with the right offers
- 🚀 Optimize revenue across categories, regions, and seasons
- 💡 Make data-driven decisions to improve satisfaction and reduce returns

---

<div align="center">

**⭐ If you found this project helpful, please give it a star!**

*Built with ❤️ using Power BI, SQL & DAX*

</div>
