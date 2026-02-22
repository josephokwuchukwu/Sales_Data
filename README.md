
# 🏪 Juka Shop Data Warehouse

A complete **Sales Data Warehouse** built on **Snowflake** for retail analytics and business intelligence.

![Snowflake](https://img.shields.io/badge/Snowflake-29B5E8?style=for-the-badge&logo=snowflake&logoColor=white)
![SQL](https://img.shields.io/badge/SQL-4479A1?style=for-the-badge&logo=postgresql&logoColor=white)
![Data Warehouse](https://img.shields.io/badge/Data_Warehouse-FF6B6B?style=for-the-badge&logo=databricks&logoColor=white)

## 📋 Table of Contents

- [Overview](#overview)
- [Architecture](#architecture)
- [Database Schema](#database-schema)
- [Features](#features)
- [Installation](#installation)
- [Sample Data](#sample-data)
- [Queries & Analytics](#queries--analytics)
- [ERD Diagram](#erd-diagram)
- [Usage Examples](#usage-examples)
- [Performance Optimization](#performance-optimization)
- [Contributing](#contributing)
- [License](#license)

---

## 🎯 Overview

**KUJA Shop Data Warehouse** is a dimensional data model designed for analyzing retail sales data. Built using **Snowflake's** modern cloud data platform, this project demonstrates best practices in:

- ⭐ Star schema design
- 📊 Fact and dimension modeling
- 🔗 Referential integrity
- 🚀 Query optimization
- 📈 Business intelligence reporting

### Key Metrics Tracked

- 💰 Sales Revenue & Profitability
- 👥 Customer Lifetime Value
- 📦 Product Performance
- 🏪 Store Analytics
- 👔 Employee Performance
- 📅 Time-based Trends

---

## 🏗️ Architecture

### Data Model: Star Schema

The warehouse implements a **star schema** with one central fact table surrounded by dimension tables:
```
        DIM_DATE          DIM_CUSTOMER
            \                 /
             \               /
              \             /
               FACT_SALES  ────── DIM_PRODUCT
              /             \
             /               \
            /                 \
        DIM_STORE          DIM_EMPLOYEE
```

### Technology Stack

- **Database**: Snowflake Data Cloud
- **Query Language**: SQL (Snowflake SQL dialect)
- **Data Model**: Dimensional (Star Schema)
- **Sample Data**: 100+ transactions, 10 customers, 15 products

---

## 📊 Database Schema

### Database Structure
```
KUJA_SHOP_DATA (Database)
└── SALES_DWH (Schema)
    ├── Dimension Tables
    │   ├── DIM_DATE
    │   ├── DIM_CUSTOMER
    │   ├── DIM_PRODUCT
    │   ├── DIM_STORE
    │   └── DIM_EMPLOYEE
    │
    ├── Fact Tables
    │   └── FACT_SALES
    │
    └── Views
        └── VW_SALES_SUMMARY
```

---

### 📅 DIM_DATE - Date Dimension

Stores calendar attributes for time-based analysis.

| Column | Type | Description |
|--------|------|-------------|
| `DATE_ID` | NUMBER(38,0) | Primary Key (Auto-increment) |
| `DATE_ACTUAL` | DATE | Actual calendar date |
| `DAY_NAME` | VARCHAR(10) | Day of week name |
| `MONTH_NAME` | VARCHAR(10) | Month name |
| `MONTH_NUMBER` | NUMBER(2,0) | Month number (1-12) |
| `QUARTER` | NUMBER(1,0) | Quarter (1-4) |
| `YEAR` | NUMBER(4,0) | Year |
| `IS_WEEKEND` | BOOLEAN | Weekend flag |

**Sample Data:**
```sql
DATE_ID | DATE_ACTUAL | DAY_NAME  | MONTH_NAME | YEAR
--------|-------------|-----------|------------|------
1       | 2024-01-01  | Monday    | January    | 2024
2       | 2024-01-02  | Tuesday   | January    | 2024
```

---

### 👥 DIM_CUSTOMER - Customer Dimension

Contains customer profile and demographic information.

| Column | Type | Description |
|--------|------|-------------|
| `CUSTOMER_ID` | NUMBER(38,0) | Primary Key (Auto-increment) |
| `CUSTOMER_CODE` | VARCHAR(50) | Unique customer code |
| `CUSTOMER_NAME` | VARCHAR(200) | Full customer name |
| `EMAIL` | VARCHAR(200) | Email address |
| `PHONE` | VARCHAR(50) | Phone number |
| `CITY` | VARCHAR(100) | City |
| `COUNTRY` | VARCHAR(100) | Country |
| `CUSTOMER_TYPE` | VARCHAR(50) | Type: Regular, VIP, Wholesale |
| `REGISTRATION_DATE` | DATE | Registration date |

**Sample Data:**
```sql
CUSTOMER_ID | CUSTOMER_CODE | CUSTOMER_NAME      | CITY           | CUSTOMER_TYPE
------------|---------------|--------------------|----------------|---------------
1           | CUST001       | Adebayo Ogunlesi   | Lagos          | VIP
2           | CUST002       | Chioma Nwosu       | Abuja          | Regular
```

---

### 📦 DIM_PRODUCT - Product Dimension

Product catalog with pricing and classification.

| Column | Type | Description |
|--------|------|-------------|
| `PRODUCT_ID` | NUMBER(38,0) | Primary Key (Auto-increment) |
| `PRODUCT_CODE` | VARCHAR(50) | Unique product code |
| `PRODUCT_NAME` | VARCHAR(200) | Product name |
| `CATEGORY` | VARCHAR(100) | Product category |
| `BRAND` | VARCHAR(100) | Brand name |
| `UNIT_PRICE` | NUMBER(10,2) | Selling price |
| `COST_PRICE` | NUMBER(10,2) | Cost price |
| `STOCK_QUANTITY` | NUMBER(10,0) | Current stock level |

**Sample Data:**
```sql
PRODUCT_ID | PRODUCT_CODE | PRODUCT_NAME        | CATEGORY    | UNIT_PRICE
-----------|--------------|---------------------|-------------|------------
1          | PROD001      | Samsung Galaxy A54  | Electronics | 185000.00
2          | PROD002      | HP Laptop 15s       | Electronics | 320000.00
```

---

### 🏪 DIM_STORE - Store Dimension

Store locations and attributes.

| Column | Type | Description |
|--------|------|-------------|
| `STORE_ID` | NUMBER(38,0) | Primary Key (Auto-increment) |
| `STORE_CODE` | VARCHAR(50) | Unique store code |
| `STORE_NAME` | VARCHAR(200) | Store name |
| `CITY` | VARCHAR(100) | City location |
| `REGION` | VARCHAR(100) | Geographic region |
| `MANAGER_NAME` | VARCHAR(200) | Store manager |

**Sample Data:**
```sql
STORE_ID | STORE_CODE | STORE_NAME              | CITY           | REGION
---------|------------|-------------------------|----------------|-------------
1        | STORE001   | KUJA Shop Lagos Main    | Lagos          | Southwest
2        | STORE002   | KUJA Shop Abuja Plaza   | Abuja          | North Central
```

---

### 👔 DIM_EMPLOYEE - Employee Dimension

Employee information including sales representatives.

| Column | Type | Description |
|--------|------|-------------|
| `EMPLOYEE_ID` | NUMBER(38,0) | Primary Key (Auto-increment) |
| `EMPLOYEE_CODE` | VARCHAR(50) | Unique employee code |
| `FULL_NAME` | VARCHAR(200) | Full name |
| `JOB_TITLE` | VARCHAR(100) | Job title |
| `DEPARTMENT` | VARCHAR(100) | Department |
| `HIRE_DATE` | DATE | Hire date |

**Sample Data:**
```sql
EMPLOYEE_ID | EMPLOYEE_CODE | FULL_NAME           | JOB_TITLE            | DEPARTMENT
------------|---------------|---------------------|----------------------|------------
1           | EMP001        | Oluwaseun Adeyemi   | Sales Representative | Sales
2           | EMP002        | Ngozi Okonkwo       | Senior Sales Rep     | Sales
```

---

### 💰 FACT_SALES - Sales Fact Table

**Grain**: One row per order line item

Central fact table containing sales transactions and measures.

| Column | Type | Description |
|--------|------|-------------|
| `SALES_ID` | NUMBER(38,0) | Primary Key (Auto-increment) |
| `DATE_ID` | NUMBER(38,0) | Foreign Key → DIM_DATE |
| `CUSTOMER_ID` | NUMBER(38,0) | Foreign Key → DIM_CUSTOMER |
| `PRODUCT_ID` | NUMBER(38,0) | Foreign Key → DIM_PRODUCT |
| `STORE_ID` | NUMBER(38,0) | Foreign Key → DIM_STORE |
| `EMPLOYEE_ID` | NUMBER(38,0) | Foreign Key → DIM_EMPLOYEE |
| `ORDER_ID` | VARCHAR(50) | Order identifier |
| `ORDER_DATE` | DATE | Transaction date |
| `QUANTITY` | NUMBER(10,2) | Quantity sold |
| `UNIT_PRICE` | NUMBER(10,2) | Unit selling price |
| `DISCOUNT_AMOUNT` | NUMBER(10,2) | Discount applied |
| `TAX_AMOUNT` | NUMBER(10,2) | Tax amount |
| `TOTAL_AMOUNT` | NUMBER(10,2) | Total transaction amount |
| `COST_AMOUNT` | NUMBER(10,2) | Total cost |
| `PROFIT_AMOUNT` | NUMBER(10,2) | Gross profit |
| `ORDER_STATUS` | VARCHAR(50) | Order status |
| `PAYMENT_METHOD` | VARCHAR(50) | Payment method used |

**Sample Data:**
```sql
SALES_ID | ORDER_ID      | PRODUCT_NAME      | QUANTITY | TOTAL_AMOUNT | PROFIT_AMOUNT
---------|---------------|-------------------|----------|--------------|---------------
1        | ORD-2024-0001 | Samsung Galaxy    | 2.00     | 370000.00    | 70000.00
2        | ORD-2024-0002 | HP Laptop         | 1.00     | 320000.00    | 60000.00
```

---

## ✨ Features

### 🔑 Key Capabilities

- ✅ **Dimensional Modeling**: Optimized star schema for analytics
- ✅ **Referential Integrity**: Foreign key constraints (informational in Snowflake)
- ✅ **Auto-increment Keys**: Surrogate keys with AUTOINCREMENT
- ✅ **Temporal Analysis**: Date dimension with calendar attributes
- ✅ **Sample Data**: Pre-populated with realistic test data
- ✅ **Pre-built Views**: Ready-to-use analytical views
- ✅ **Query Optimization**: Clustered tables for performance
- ✅ **Comprehensive Documentation**: Full schema documentation

### 📊 Analytics Capabilities

- 💵 Revenue and profitability analysis
- 🛒 Product performance tracking
- 👥 Customer segmentation and lifetime value
- 🏪 Store comparison and regional analysis
- 👨‍💼 Employee/salesperson performance
- 📅 Time-series trends (daily, monthly, quarterly, yearly)
- 💳 Payment method analysis
- 🏷️ Discount and promotion effectiveness

---

## 🚀 Installation

### Prerequisites

- Snowflake account (free trial available at [snowflake.com](https://signup.snowflake.com/))
- Snowflake user with CREATE DATABASE privileges
- SQL client (Snowsight, SnowSQL, or any SQL IDE)

### Step 1: Clone Repository
```bash
git clone https://github.com/yourusername/kuja-shop-dwh.git
cd kuja-shop-dwh
```

### Step 2: Connect to Snowflake
```sql
-- Using SnowSQL
snowsql -a <your_account> -u <your_username>

-- Or use Snowsight web interface
-- https://<your_account>.snowflakecomputing.com
```

### Step 3: Execute Setup Scripts

Run the SQL scripts in order:
```sql
-- 1. Create database and schema
@scripts/01_create_database.sql

-- 2. Create dimension tables
@scripts/02_create_dimensions.sql

-- 3. Create fact tables
@scripts/03_create_facts.sql

-- 4. Load sample data
@scripts/04_load_sample_data.sql

-- 5. Create views
@scripts/05_create_views.sql
```

### Step 4: Verify Installation
```sql
-- Check table counts
SELECT 'DIM_DATE' AS TABLE_NAME, COUNT(*) AS ROW_COUNT FROM DIM_DATE
UNION ALL
SELECT 'DIM_CUSTOMER', COUNT(*) FROM DIM_CUSTOMER
UNION ALL
SELECT 'DIM_PRODUCT', COUNT(*) FROM DIM_PRODUCT
UNION ALL
SELECT 'DIM_STORE', COUNT(*) FROM DIM_STORE
UNION ALL
SELECT 'DIM_EMPLOYEE', COUNT(*) FROM DIM_EMPLOYEE
UNION ALL
SELECT 'FACT_SALES', COUNT(*) FROM FACT_SALES;
```

**Expected Results:**
```
TABLE_NAME      | ROW_COUNT
----------------|----------
DIM_DATE        | 730
DIM_CUSTOMER    | 10
DIM_PRODUCT     | 15
DIM_STORE       | 5
DIM_EMPLOYEE    | 8
FACT_SALES      | 100
```

---

## 📁 Sample Data

### Data Volume

| Table | Rows | Description |
|-------|------|-------------|
| DIM_DATE | 730 | 2 years (2024-2025) |
| DIM_CUSTOMER | 10 | Sample customers from Nigeria |
| DIM_PRODUCT | 15 | Electronics and Fashion products |
| DIM_STORE | 5 | Stores across Nigerian cities |
| DIM_EMPLOYEE | 8 | Sales representatives |
| FACT_SALES | 100+ | Sample transactions |

### Data Characteristics

- **Geographic Coverage**: Lagos, Abuja, Kano, Port Harcourt, Ibadan
- **Product Categories**: Electronics, Fashion
- **Brands**: Samsung, Apple, HP, Nike, Adidas, Gucci, Zara
- **Price Range**: ₦22,000 - ₦420,000
- **Customer Types**: Regular (60%), VIP (30%), Wholesale (10%)
- **Payment Methods**: Card (60%), Cash (20%), Bank Transfer (20%)

---

## 🔍 Queries & Analytics

### Basic Queries

#### 1. Total Sales by Month
```sql
SELECT 
    YEAR,
    MONTH_NAME,
    COUNT(DISTINCT ORDER_ID) AS TOTAL_ORDERS,
    SUM(QUANTITY) AS TOTAL_ITEMS_SOLD,
    SUM(TOTAL_AMOUNT) AS TOTAL_REVENUE,
    SUM(PROFIT_AMOUNT) AS TOTAL_PROFIT,
    ROUND(AVG(TOTAL_AMOUNT), 2) AS AVG_ORDER_VALUE
FROM VW_SALES_SUMMARY
GROUP BY YEAR, MONTH_NAME, MONTH_NUMBER
ORDER BY YEAR, MONTH_NUMBER;
```

#### 2. Top 5 Products by Revenue
```sql
SELECT 
    PRODUCT_NAME,
    CATEGORY,
    BRAND,
    COUNT(DISTINCT ORDER_ID) AS ORDERS,
    SUM(QUANTITY) AS UNITS_SOLD,
    SUM(TOTAL_AMOUNT) AS TOTAL_REVENUE,
    SUM(PROFIT_AMOUNT) AS TOTAL_PROFIT
FROM VW_SALES_SUMMARY
GROUP BY PRODUCT_NAME, CATEGORY, BRAND
ORDER BY TOTAL_REVENUE DESC
LIMIT 5;
```

#### 3. Customer Lifetime Value
```sql
SELECT 
    CUSTOMER_NAME,
    CUSTOMER_TYPE,
    CUSTOMER_CITY,
    COUNT(DISTINCT ORDER_ID) AS TOTAL_ORDERS,
    SUM(TOTAL_AMOUNT) AS LIFETIME_VALUE,
    ROUND(AVG(TOTAL_AMOUNT), 2) AS AVG_ORDER_VALUE
FROM VW_SALES_SUMMARY
GROUP BY CUSTOMER_NAME, CUSTOMER_TYPE, CUSTOMER_CITY
ORDER BY LIFETIME_VALUE DESC
LIMIT 10;
```

#### 4. Store Performance Comparison
```sql
SELECT 
    STORE_NAME,
    STORE_CITY,
    REGION,
    COUNT(DISTINCT ORDER_ID) AS TOTAL_ORDERS,
    SUM(TOTAL_AMOUNT) AS REVENUE,
    SUM(PROFIT_AMOUNT) AS PROFIT,
    ROUND(AVG(TOTAL_AMOUNT), 2) AS AVG_ORDER_VALUE
FROM VW_SALES_SUMMARY
GROUP BY STORE_NAME, STORE_CITY, REGION
ORDER BY REVENUE DESC;
```

#### 5. Employee Performance
```sql
SELECT 
    EMPLOYEE_NAME,
    JOB_TITLE,
    COUNT(DISTINCT ORDER_ID) AS ORDERS_CLOSED,
    SUM(TOTAL_AMOUNT) AS TOTAL_SALES,
    SUM(PROFIT_AMOUNT) AS TOTAL_PROFIT,
    ROUND(AVG(TOTAL_AMOUNT), 2) AS AVG_DEAL_SIZE
FROM VW_SALES_SUMMARY
WHERE EMPLOYEE_NAME IS NOT NULL
GROUP BY EMPLOYEE_NAME, JOB_TITLE
ORDER BY TOTAL_SALES DESC;
```

### Advanced Analytics

#### Weekend vs Weekday Sales
```sql
SELECT 
    CASE WHEN D.IS_WEEKEND THEN 'Weekend' ELSE 'Weekday' END AS DAY_TYPE,
    COUNT(DISTINCT F.ORDER_ID) AS ORDERS,
    SUM(F.TOTAL_AMOUNT) AS REVENUE,
    ROUND(AVG(F.TOTAL_AMOUNT), 2) AS AVG_ORDER_VALUE
FROM FACT_SALES F
JOIN DIM_DATE D ON F.DATE_ID = D.DATE_ID
GROUP BY DAY_TYPE;
```

#### Product Category Performance
```sql
SELECT 
    CATEGORY,
    COUNT(DISTINCT PRODUCT_CODE) AS PRODUCTS_IN_CATEGORY,
    COUNT(DISTINCT ORDER_ID) AS ORDERS,
    SUM(QUANTITY) AS UNITS_SOLD,
    SUM(TOTAL_AMOUNT) AS REVENUE,
    SUM(PROFIT_AMOUNT) AS PROFIT,
    ROUND(SUM(PROFIT_AMOUNT) / NULLIF(SUM(TOTAL_AMOUNT), 0) * 100, 2) AS PROFIT_MARGIN_PCT
FROM VW_SALES_SUMMARY
GROUP BY CATEGORY
ORDER BY REVENUE DESC;
```

---

## 🎨 ERD Diagram
```
┌─────────────────────┐       ┌─────────────────────┐       ┌─────────────────────┐
│    DIM_DATE         │       │   DIM_CUSTOMER      │       │   DIM_PRODUCT       │
├─────────────────────┤       ├─────────────────────┤       ├─────────────────────┤
│ • DATE_ID (PK)      │       │ • CUSTOMER_ID (PK)  │       │ • PRODUCT_ID (PK)   │
│   DATE_ACTUAL       │       │   CUSTOMER_CODE     │       │   PRODUCT_CODE      │
│   DAY_NAME          │       │   CUSTOMER_NAME     │       │   PRODUCT_NAME      │
│   MONTH_NAME        │       │   EMAIL             │       │   CATEGORY          │
│   QUARTER           │       │   CITY              │       │   BRAND             │
│   YEAR              │       │   CUSTOMER_TYPE     │       │   UNIT_PRICE        │
└──────────┬──────────┘       └──────────┬──────────┘       └──────────┬──────────┘
           │                             │                             │
           └─────────────────────────────┼─────────────────────────────┘
                                         │
                                         ↓
                          ┌──────────────────────────────┐
                          │       FACT_SALES             │
                          ├──────────────────────────────┤
                          │ • SALES_ID (PK)              │
                          │ • DATE_ID (FK)               │
                          │ • CUSTOMER_ID (FK)           │
                          │ • PRODUCT_ID (FK)            │
                          │ • STORE_ID (FK)              │
                          │ • EMPLOYEE_ID (FK)           │
                          │   ORDER_ID                   │
                          │   QUANTITY                   │
                          │   UNIT_PRICE                 │
                          │   TOTAL_AMOUNT               │
                          │   PROFIT_AMOUNT              │
                          └──────────────────────────────┘
                                         ↑
           ┌─────────────────────────────┼─────────────────────────────┐
           │                             │                             │
┌──────────┴──────────┐       ┌──────────┴──────────┐
│   DIM_STORE         │       │   DIM_EMPLOYEE      │
├─────────────────────┤       ├─────────────────────┤
│ • STORE_ID (PK)     │       │ • EMPLOYEE_ID (PK)  │
│   STORE_CODE        │       │   EMPLOYEE_CODE     │
│   STORE_NAME        │       │   FULL_NAME         │
│   CITY              │       │   JOB_TITLE         │
│   REGION            │       │   DEPARTMENT        │
└─────────────────────┘       └─────────────────────┘
```

### Relationships

| Fact Table | Foreign Key | Dimension Table | Cardinality |
|------------|-------------|-----------------|-------------|
| FACT_SALES | DATE_ID | DIM_DATE | Many-to-One |
| FACT_SALES | CUSTOMER_ID | DIM_CUSTOMER | Many-to-One |
| FACT_SALES | PRODUCT_ID | DIM_PRODUCT | Many-to-One |
| FACT_SALES | STORE_ID | DIM_STORE | Many-to-One |
| FACT_SALES | EMPLOYEE_ID | DIM_EMPLOYEE | Many-to-One |

---

## 💡 Usage Examples

### Scenario 1: Monthly Revenue Dashboard
```sql
-- Create a monthly KPI view
CREATE OR REPLACE VIEW VW_MONTHLY_KPI AS
SELECT 
    DATE_TRUNC('MONTH', ORDER_DATE) AS MONTH,
    COUNT(DISTINCT ORDER_ID) AS ORDERS,
    COUNT(DISTINCT CUSTOMER_ID) AS UNIQUE_CUSTOMERS,
    SUM(TOTAL_AMOUNT) AS REVENUE,
    SUM(PROFIT_AMOUNT) AS PROFIT,
    AVG(TOTAL_AMOUNT) AS AVG_ORDER_VALUE
FROM FACT_SALES
GROUP BY DATE_TRUNC('MONTH', ORDER_DATE)
ORDER BY MONTH;

-- Query the view
SELECT * FROM VW_MONTHLY_KPI;
```

### Scenario 2: Customer Segmentation
```sql
-- Segment customers by purchase frequency
SELECT 
    C.CUSTOMER_NAME,
    C.CUSTOMER_TYPE,
    COUNT(F.ORDER_ID) AS PURCHASE_COUNT,
    SUM(F.TOTAL_AMOUNT) AS TOTAL_SPENT,
    CASE 
        WHEN COUNT(F.ORDER_ID) >= 10 THEN 'Frequent Buyer'
        WHEN COUNT(F.ORDER_ID) >= 5 THEN 'Regular Buyer'
        ELSE 'Occasional Buyer'
    END AS SEGMENT
FROM DIM_CUSTOMER C
LEFT JOIN FACT_SALES F ON C.CUSTOMER_ID = F.CUSTOMER_ID
GROUP BY C.CUSTOMER_NAME, C.CUSTOMER_TYPE
ORDER BY PURCHASE_COUNT DESC;
```

### Scenario 3: Product Inventory Alert
```sql
-- Find low-stock products
SELECT 
    PRODUCT_CODE,
    PRODUCT_NAME,
    CATEGORY,
    STOCK_QUANTITY,
    CASE 
        WHEN STOCK_QUANTITY = 0 THEN '🔴 Out of Stock'
        WHEN STOCK_QUANTITY < 20 THEN '🟡 Low Stock'
        ELSE '🟢 In Stock'
    END AS STOCK_STATUS
FROM DIM_PRODUCT
WHERE STOCK_QUANTITY < 30
ORDER BY STOCK_QUANTITY;
```

---

## ⚡ Performance Optimization

### Clustering

Tables are clustered on frequently filtered columns:
```sql
-- FACT_SALES is clustered on ORDER_DATE and STORE_ID
ALTER TABLE FACT_SALES CLUSTER BY (ORDER_DATE, STORE_ID);

-- DIM_CUSTOMER is clustered on CUSTOMER_CODE
ALTER TABLE DIM_CUSTOMER CLUSTER BY (CUSTOMER_CODE, CUSTOMER_STATUS);

-- DIM_PRODUCT is clustered on CATEGORY
ALTER TABLE DIM_PRODUCT CLUSTER BY (CATEGORY, PRODUCT_STATUS);
```

### Search Optimization

Enabled on text search columns:
```sql
ALTER TABLE DIM_CUSTOMER ADD SEARCH OPTIMIZATION 
    ON EQUALITY(CUSTOMER_CODE, EMAIL, CUSTOMER_NAME);

ALTER TABLE DIM_PRODUCT ADD SEARCH OPTIMIZATION 
    ON EQUALITY(PRODUCT_CODE, SKU, BARCODE);
```

### Query Tips

1. **Use the pre-built view** `VW_SALES_SUMMARY` for most analytics
2. **Filter on clustered columns** (ORDER_DATE, STORE_ID) for best performance
3. **Use DATE_ID** instead of ORDER_DATE in joins for better performance
4. **Leverage QUALIFY** clause for ranking queries (Snowflake feature)

---

## 📂 Project Structure
```
kuja-shop-dwh/
├── README.md
├── LICENSE
├── .gitignore
├── scripts/
│   ├── 01_create_database.sql
│   ├── 02_create_dimensions.sql
│   ├── 03_create_facts.sql
│   ├── 04_load_sample_data.sql
│   ├── 05_create_views.sql
│   └── 99_cleanup.sql
├── queries/
│   ├── basic_analytics.sql
│   ├── customer_analysis.sql
│   ├── product_analysis.sql
│   └── performance_queries.sql
├── docs/
│   ├── SCHEMA.md
│   ├── ERD.png
│   └── SETUP_GUIDE.md
└── examples/
    └── sample_outputs.md
```

---

## 🤝 Contributing

Contributions are welcome! Here's how you can help:

### How to Contribute

1. **Fork the repository**
2. **Create a feature branch**
```bash
   git checkout -b feature/AmazingFeature
```
3. **Commit your changes**
```bash
   git commit -m 'Add some AmazingFeature'
```
4. **Push to the branch**
```bash
   git push origin feature/AmazingFeature
```
5. **Open a Pull Request**

### Contribution Ideas

- 🎨 Add more sample data
- 📊 Create additional analytical queries
- 🔧 Optimize existing queries
- 📝 Improve documentation
- 🐛 Report bugs or issues
- ✨ Add new features (e.g., aggregate tables, materialized views)

---


---

## 🙏 Acknowledgments

- **Snowflake** for their amazing cloud data platform
- **Kimball Group** for dimensional modeling methodology
- **Open Source Community** for inspiration and best practices

---



---# 🚀 Quick Start
```bash
# Clone the repo
git clone https://github.com/yourusername/kuja-shop-dwh.git

# Navigate to directory
cd kuja-shop-dwh

# Connect to Snowflake and run setup
snowsql -a <account> -u <username> -f scripts/01_create_database.sql
```

---

## 📈 Roadmap

- [x] Core dimensional model
- [x] Sample data
- [x] Basic analytics queries
- [ ] Aggregate fact tables
- [ ] Slowly Changing Dimensions (SCD Type 2)
- [ ] Data quality checks
- [ ] ETL automation with dbt
- [ ] Power BI dashboard templates
- [ ] Python data generator
- [ ] Performance benchmarks

