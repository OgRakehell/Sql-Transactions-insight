## 01-setup.md – Setting Up the Database
---
## Why Setup is Needed

In my workplace, we already have a production database containing millions of mobile and USSD transactions.
For confidentiality reasons, I cannot expose that dataset or the internal company schema.

This project is a replica, built with synthetic data that mimics the structure of our real transactions.
The goal is not to copy production exactly, but to:

- Show how queries and reports are structured.

- Provide a training playground where new analysts can practice without accessing sensitive data.

## Requirements
- Microsoft SQL Server (2019 or later recommended)
- SQL Server Management Studio (SSMS)
- Power BI Desktop (latest version)

--
## 1. Create a Database
```sql
CREATE DATABASE BankTransactionsDB;
USE BankTransactionsDB;
```
---
## 2. Create the Transactions Table
 ```sql
CREATE TABLE Transactions (
    TransactionID INT IDENTITY(1,1) PRIMARY KEY,
    TransactionType VARCHAR(50),
    Amount DECIMAL(18,2),
    ResponseCode VARCHAR(50),
    TransactionDate DATETIME
);
```
---
## 3. Insert Sample Data
```sql
INSERT INTO Transactions (TransactionType, Amount, ResponseCode, TransactionDate)
VALUES
('Airtime', 500, '00', '2025-09-14 08:23:11'),
('Airtime', 100, '427', '2025-09-14 09:45:20'),
('Transfer', 2000, '00', '2025-09-14 10:12:05'),
('Transfer', 1500, '96', '2025-09-14 11:10:45'),
('BillPayment', 750, 'ServiceUnavailable', '2025-09-14 12:01:30'),
('BillPayment', 200, '00', '2025-09-14 13:22:55'),
('Transfer', 1000, 'GatewayTimeout', '2025-09-14 14:19:12'),
('Airtime', 300, NULL, '2025-09-14 15:05:50'),
('BillPayment', 1200, '12', '2025-09-14 16:45:00');
```
----

## 4. Run Your Query

Now you can run your report query exactly as you wrote it. It’ll classify successes, failures, and breakdowns into Customer, Switch, System categories.





