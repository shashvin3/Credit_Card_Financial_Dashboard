# 💳 Credit Card Financial Dashboard

An end-to-end Power BI analytics project that turns raw credit card transaction and customer data into an interactive weekly dashboard — covering revenue, transactions, interest, customer demographics, card categories, activation, and delinquency.

---

## 📌 Project Overview

The objective of this project is to develop a comprehensive **Credit Card Financial Dashboard** that provides real-time insight into key performance metrics and trends, enabling stakeholders to monitor and analyze credit card operations effectively.

The project combines:

- **SQL (MySQL)** for database creation and data preparation
- **Excel / CSV** data as the source data
- **Power BI** for data modeling and visualization
- **DAX** for calculated columns and measures
- **Interactive dashboards** for analysis and reporting

---

## 🎯 Project Objectives

- Analyze credit card transaction and customer data
- Monitor revenue and transaction performance
- Track weekly performance trends across quarters
- Analyze customer demographics and segments
- Monitor activation and delinquency rates for risk management
- Analyze credit card categories and transaction behavior
- Identify key business insights from the dashboard

---

## 🛠️ Tools & Technologies

| Tool | Purpose |
|---|---|
| **Power BI** | Data modeling, DAX, and dashboard visualization |
| **MySQL** | Database creation and SQL queries |
| **Microsoft Excel** | Source data (CSV/XLSX) |
| **DAX** | Calculated columns and measures |

---

## 🔄 Project Workflow

```text
Excel / CSV Data
      ↓
MySQL Database (ccdb)
      ↓
SQL Tables (cc_detail, cust_detail)
      ↓
Data Import into Power BI
      ↓
Data Processing & Modeling
      ↓
DAX Calculations
      ↓
Interactive Power BI Dashboard
      ↓
Insights & Reporting
```

---

## 🗄️ Data & SQL

The project uses a MySQL database (**`ccdb`**) with two main tables.

### 1. `cc_detail` — Credit Card Data

Contains information related to:

- Card category
- Annual fees
- Activation status
- Customer acquisition cost
- Credit limit
- Revolving balance
- Transaction amount
- Transaction count
- Utilization ratio
- Expense type
- Interest earned
- Delinquency status

### 2. `cust_detail` — Customer Data

Contains information related to:

- Customer age
- Gender
- Dependent count
- Education
- Marital status
- State
- Income
- Customer job
- Car ownership
- House ownership
- Personal loan
- Customer satisfaction score

`CreditCard_Sql_Query.sql` creates the `ccdb` database and both tables. The source CSV/Excel data was then imported into MySQL using the **MySQL Workbench Table Data Import Wizard**.

---

## 📊 Data Processing & DAX

DAX was used inside Power BI to create the calculated columns and measures needed for analysis.

### Age Group

Customers were bucketed into five age bands:

```dax
AgeGroup =
SWITCH(
    TRUE(),
    'cust_detail'[customer_age] < 30, "20-30",
    'cust_detail'[customer_age] >= 30 &&
        'cust_detail'[customer_age] < 40, "30-40",
    'cust_detail'[customer_age] >= 40 &&
        'cust_detail'[customer_age] < 50, "40-50",
    'cust_detail'[customer_age] >= 50 &&
        'cust_detail'[customer_age] < 60, "50-60",
    'cust_detail'[customer_age] >= 60, "60+",
    "unknown"
)
```

### Income Group

Customers were bucketed into three income bands:

```dax
IncomeGroup =
SWITCH(
    TRUE(),
    'cust_detail'[income] < 35000, "Low",
    'cust_detail'[income] >= 35000 &&
        'cust_detail'[income] < 70000, "Med",
    'cust_detail'[income] >= 70000, "High",
    "unknown"
)
```

### Week Number

Used to identify and sort weeks for period-over-period comparisons:

```dax
week_num2 =
WEEKNUM('cc_detail'[week_start_date])
```

### Revenue

A calculated column was created by combining annual fees, transaction amount, and interest earned:

```dax
Revenue =
'cc_detail'[annual_fees]
+ 'cc_detail'[total_trans_amt]
+ 'cc_detail'[interest_earned]
```

### Current Week Revenue

```dax
Current_week_Revenue =
CALCULATE(
    SUM('cc_detail'[Revenue]),
    FILTER(
        ALL('cc_detail'),
        'cc_detail'[week_num2] =
        MAX('cc_detail'[week_num2])
    )
)
```

### Previous Week Revenue

```dax
Previous_week_Revenue =
CALCULATE(
    SUM('cc_detail'[Revenue]),
    FILTER(
        ALL('cc_detail'),
        'cc_detail'[week_num2] =
        MAX('cc_detail'[week_num2]) - 1
    )
)
```

`Current_week_Revenue` and `Previous_week_Revenue` power the week-over-week (WoW) comparison shown on the dashboard, independent of whichever slicers are currently applied.

---

## 📈 Dashboard Analysis

The Power BI dashboard is organized into four analytical areas:

### 💰 Revenue Analysis

- Overall revenue
- Weekly revenue performance
- Revenue contribution by customer segment
- Revenue contribution by card category

### 💳 Transaction Analysis

- Total transaction amount
- Transaction count
- Transaction trends by quarter
- Card category contribution
- Expense type analysis

### 👥 Customer Analysis

- Age groups
- Income groups
- Gender
- Education
- Marital status
- Customer job

### 📊 Performance & Risk Analysis

- Activation rate
- Delinquency rate
- Credit utilization
- Interest earned
- Annual fees

---

## 🔍 Key Insights

- Overall revenue was approximately **$57M**
- Total interest earned was approximately **$8M**
- Total transaction amount was approximately **$46M**
- Male customers contributed approximately **$31M** in revenue, compared with **$26M** from female customers
- **Blue** and **Silver** card categories contributed approximately **93%** of overall transactions
- **Texas, New York,** and **California** together contributed approximately **68%** of revenue
- Overall activation rate was approximately **57.5%**
- Overall delinquency rate was approximately **6.06%**
- Revenue increased by approximately **28.8%** week-over-week in the reported period

---

## 📁 Repository Structure

| File | Description |
|---|---|
| `Credit_Card_Financial_Project.pbix` | Power BI project file — data model, DAX, and dashboard |
| `credit_card.xlsx` | Source dataset — credit card / transaction data |
| `customer.xlsx` | Source dataset — customer data |
| `CreditCard_Sql_Query.sql` | SQL script for database and table creation |
| `Credit_Card_Report.pdf` | Exported Power BI report — Transaction Report |
| `Credit_Card_Customer_Report.pdf` | Exported Power BI report — Customer Report |

---

## 🌟 Project Highlights

- Built an interactive, multi-page Power BI dashboard
- Connected customer and credit card transaction data through a relational data model
- Used MySQL for database and table creation
- Used DAX for calculated columns and week-over-week revenue analysis
- Delivered weekly performance analysis with quarter-level trend comparisons
- Analyzed customer demographics and transaction behavior across multiple segments
- Generated actionable business insights from dashboard visualizations
