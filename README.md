
# Personal Finance SQL Tracker

A multi-user SQL-based personal finance tracker with features like expense categorization, monthly budgeting, recurring transactions, running balance, and audit logging. This project demonstrates the use of advanced SQL techniques including Common Table Expressions (CTEs), window functions, subqueries, and triggers.

---

##  Project Overview

This project allows users to:
- Track income and expense transactions
- Set and compare monthly budgets
- Manage recurring payments
- Monitor category-wise spending trends
- Calculate running balances
- Maintain an audit log for transaction changes

---

##  Database Schema

### 1. users
- user_id (INT, PK)
- name (VARCHAR)
- email (VARCHAR)

### 2. transactions
- transaction_id (INT, PK)
- user_id (FK)
- transaction_date (DATE)
- amount (DECIMAL)
- category_id (FK)
- type (Income/Expense)
- payment_method (VARCHAR)
- notes (TEXT)

### 3. categories
- category_id (INT, PK)
- category_name (VARCHAR)
- category_type (Income/Expense)

### 4. budgets
- budget_id (INT, PK)
- user_id (FK)
- category_id (FK)
- month_year (VARCHAR, 'YYYY-MM')
- budget_amount (DECIMAL)

### 5. recurring_transactions
- recurring_id (INT, PK)
- user_id (FK)
- category_id (FK)
- amount (DECIMAL)
- recurrence_type (Monthly)
- start_date (DATE)
- end_date (DATE)

### 6. audit_log
- log_id (INT, PK)
- transaction_id (FK)
- action (INSERT/UPDATE/DELETE)
- action_time (TIMESTAMP)

---

##  Key SQL Features

- ✅ Aggregations and Group By
- ✅ Common Table Expressions (CTEs)
- ✅ Window Functions for running balance
- ✅ Subqueries for filtering and ranking
- ✅ Triggers for audit logging
- ✅ JOINs for cross-table insights

---

##  Example Queries

### 1. Running Balance (Window Function)
```sql
SELECT transaction_date, type, amount,
  SUM(CASE WHEN type = 'Income' THEN amount ELSE -amount END)
  OVER (PARTITION BY user_id ORDER BY transaction_date) AS running_balance
FROM transactions
WHERE user_id = 1;
```

### 2. Budget vs. Actual Comparison (CTE + JOIN)
```sql
WITH monthly_expenses AS (
  SELECT category_id, DATE_FORMAT(transaction_date, '%Y-%m') AS month_year,
         SUM(amount) AS spent
  FROM transactions
  WHERE type = 'Expense' AND user_id = 1
  GROUP BY category_id, month_year
)
SELECT b.month_year, c.category_name, b.budget_amount,
       COALESCE(me.spent, 0) AS actual_spent,
       b.budget_amount - COALESCE(me.spent, 0) AS remaining
FROM budgets b
JOIN categories c ON b.category_id = c.category_id
LEFT JOIN monthly_expenses me ON b.category_id = me.category_id AND b.month_year = me.month_year
WHERE b.user_id = 1;
```

---

##  Resume Statement

> Built a multi-user personal finance tracker using SQL with advanced analytics features. Used window functions to compute running balances, CTEs to compare budgets with actuals, and triggers for transaction audit logging. Designed normalized schema and implemented complex queries for spending insights.

---

##  Folder Structure

```
personal-finance-sql-tracker/
│
├── schema.sql
├── sample_data.sql
├── advanced_queries.sql
├── triggers.sql
├── views.sql
└── README.md
```

---

##  Tags

`sql` `data-analysis` `personal-finance` `budget-tracker` `advanced-sql` `postgres` `mysql`

---
