
# Online Retail Power BI Dashboard – Customer Analytics & Funnel Analysis

## 📌 Project Overview
This project analyzes an online retail transactional dataset to uncover insights about revenue performance, customer behavior, and retention. The dashboard was built in Power BI using a star-schema data model and advanced DAX measures.  

The project focuses on answering key business questions such as:
- How is revenue trending over time?
- Who are the top customers and products?
- How frequently do customers purchase?
- How many customers become repeat buyers?

---

## 🧱 Data Model
- Fact Table: Online_retail_cleaned_1_1  
- Dimension Table: DimDate  
- Relationship: Fact[InvoiceDate] → DimDate[Date]  

Data cleaning included:
- Removing negative and zero quantities  
- Removing cancelled/return orders  
- Handling blank and missing CustomerID values  

---

## 📊 Dashboard Pages

### Page 1 – Executive Overview
- KPI Cards: Total Revenue, Orders, Total Customers, AOV, Total Quantity  
- Monthly Revenue Trend (Line Chart)  
- Top 10 Products by Revenue (Bar Chart)  
- Top Countries by Revenue (Bar Chart)  

### Page 2 – Customer Analysis
- KPI Cards: Total Customers, AOV, Revenue per Customer, Avg Orders per Customer, Repeat Customers  
- Top 10 Customers by Revenue  
- Customer Order Frequency Distribution  
- Revenue by Customer Segment  

### Page 3 – Funnel Analysis (Customer Retention)
Transactional funnel built using customer purchase behavior:

Stages:
- Total Customers  
- First-Time Customers (exactly one invoice)  
- Repeat Customers (more than one invoice)  

KPI Cards:
- Total Customers  
- First-Time Customers  
- Repeat Customers  
- Repeat Customer Share %  

Key Insight:
The vast majority of identified customers make repeat purchases, indicating strong retention. A small portion of customers purchase only once, representing an opportunity for targeted re-engagement and loyalty campaigns.

---

## 🧮 Key DAX Measures (Examples)

```DAX
Total_customers =
DISTINCTCOUNT(Online_retail_cleaned_1_1[CustomerID])

Repeat Customers =
CALCULATE(
    DISTINCTCOUNT(Online_retail_cleaned_1_1[CustomerID]),
    FILTER(
        VALUES(Online_retail_cleaned_1_1[CustomerID]),
        [Customer Orders] > 1
    )
)

First Time Customers =
CALCULATE(
    DISTINCTCOUNT(Online_retail_cleaned_1_1[CustomerID]),
    FILTER(
        VALUES(Online_retail_cleaned_1_1[CustomerID]),
        CALCULATE(COUNT(Online_retail_cleaned_1_1[InvoiceNo])) = 1
    )
)

Repeat Customer Share % =
DIVIDE(
    [Repeat Customers],
    [Repeat Customers] + [First Time Customers],
    0
)
