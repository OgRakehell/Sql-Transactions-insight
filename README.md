# Sql-Transactions-insight
From raw transactions to clear insights — a hands-on SQL + Power BI training project for daily reporting.

## Overview
At my workplace, I developed an internal SQL reporting system for analyzing mobile and USSD transactions.  
Due to confidentiality, I cannot share the original company repository.  
This project is a **replica** using synthetic data, designed to demonstrate the same structure and logic without exposing sensitive details.  

The goal:  
- Help new team members understand how we break down transaction performance.  
- Provide a simple daily report pipeline: from SQL query → to Power BI dashboard.

---
📂 Project Structure & Docs

 1. [Setup](01-setup.md)  
 → Setting up the database & inserting sample data

 2. [SQLquery](02-sql-query.md)  
 → The SQL query explained + screenshots of results

 3. [PowerBi Connection](03-powerbi-connection.md)  
 → How to connect SQL → Power BI

 4. [Visuals](04-visuals.md)  
 → Building visuals in Power BI

 5. [Refresh](05-refresh.md )  
 → (Optional) How to refresh data (DirectQuery / scheduled refresh)

- screenshots/
 → Folder for images used in docs

---

## What this project covers
- **SQL query for daily reporting**  
  - Total transaction count  
  - Transaction volume  
  - Successful vs failed counts  
  - Categorized failures (Customer, Switch, System)  

- **Power BI connection**  
  - How to connect Power BI directly to SQL Server  
  - How to build a simple visualization from the query  

This is intentionally simplified: we’re not breaking down every possible component, only the essentials needed to train and onboard quickly.

---

## Example SQL Query
```sql
SELECT
    TransactionType,
    COUNT(1) AS TotalCount,
    SUM(Amount) AS TransactionVolume,
    SUM(CASE WHEN ResponseCode = '00' THEN 1 ELSE 0 END) AS SuccessCount,
    SUM(CASE WHEN ResponseCode = '00' THEN Amount ELSE 0 END) AS SuccessVolume,
    SUM(CASE WHEN ResponseCode IN ('427','55','05','06','94','21','52','62','61','07','53','83','13') 
             OR ResponseCode IS NULL THEN 1 ELSE 0 END) AS CustomerFailCount,
    SUM(CASE WHEN ResponseCode IN ('Forbidden','ServiceUnavailable','208','BadGateway','InternalServerError',
                                   '444','MethodNotAllowed','Unauthorized','434','NotFound','BadRequest','429',
                                   'GatewayTimeout','E18','E21','E28','90000','90091','100','E22','90013','E19',
                                   'E51','E27','E13','435','485','RECHARGE_FAILED','PaymentRequired','92',
                                   '15','16','81','91','97','08','57') THEN 1 ELSE 0 END) AS SwitchFailCount,
    SUM(CASE WHEN ResponseCode IN ('63','12','96','34','0','99','30','26',
                                   'Error:Connection has already been closed.',
                                   'ORA-00028: your session has been killed','31','03') THEN 1 ELSE 0 END) AS SystemFailCount
FROM Transactions
WHERE CAST(TransactionDate AS DATE) = '2025-09-14'
GROUP BY TransactionType;
```
---
## Sample Output
<img width="834" height="146" alt="image" src="https://github.com/user-attachments/assets/bf13d3ec-58b2-4b23-a716-2049975ad3f1" />

---
## How to Use

1. Clone the repo.

2. Set up the Transactions table with sample data (01-setup.md).

3. Run the SQL query (02-sql-query.md).

4. Connect to Power BI (03-powerbi-connection.md).

5. Build visuals (04-visuals.md).

6. (Optional) Learn how to refresh data (05-refresh.md).

---
## Why this project matters

This project shows:

- How to turn raw transactional logs into daily insights.

- How SQL and BI tools can simplify operational reporting.

- A practical way to onboard new analysts into transaction monitoring.
