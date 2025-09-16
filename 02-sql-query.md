## 02-SQLquery

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

### Expected Result (similar to the sample data we inserted)

| TransactionType | TotalCount | TransactionVolume | SuccessCount | SuccessVolume | CustomerFailCount | SwitchFailCount | SystemFailCount |
| --------------- | ---------- | ----------------- | ------------ | ------------- | ----------------- | --------------- | --------------- |
| Airtime         | 3          | 900.00            | 1            | 500.00        | 2                 | 0               | 0               |
| Transfer        | 3          | 4500.00           | 1            | 2000.00       | 0                 | 1               | 1               |
| BillPayment     | 3          | 2150.00           | 1            | 200.00        | 0                 | 1               | 1               |

---

Now, this is **ready to feed directly into Power BI** for visualization.
Next step would be connecting Power BI → SQL Server → importing/running this query as a dataset.

