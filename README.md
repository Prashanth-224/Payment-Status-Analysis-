
1. Understanding the Task
You want to:
•  Investigate payment status data (Success, Failed, etc.)
•  Identify issues or trends related to payment success and failure
________________________________________
2. Planning the SQL Queries
Key metrics to include:
•  Number of payments by status (Success, Failed, etc.)
•  Total payment amount by status
•  Payment success rate
•  Trends over time (e.g., monthly success/failure rates)
________________________________________

3. SQL Queries

a) Payments by Status
   
SELECT
  payment_status,
  COUNT(payment_id) AS total_payments,
  SUM(payment_amount) AS total_payment_amount
FROM
  payments
GROUP BY
  payment_status
ORDER BY
  total_payments DESC;
This query shows how many payments succeeded or failed and the total amount for each status.
________________________________________
b) Payment Success Rate
   
SELECT
  ROUND(
    100.0 * SUM(
      CASE
        WHEN payment_status = 'Success' THEN 1
        ELSE 0
      END
    ) / COUNT(*),
    2
  ) AS success_rate_percent
FROM
  payments;
This query calculates the overall payment success rate as a percentage.
________________________________________
c) Payment Status Trends Over Time
   
SELECT
    STRFTIME('%Y-%m', payment_date) AS month,
    payment_status,
    COUNT(payment_id) AS total_payments,
    SUM(payment_amount) AS total_payment_amount
FROM
    payments
GROUP BY
    month, payment_status
ORDER BY
    month, payment_status; 
This query shows how payment statuses change over time, helping you spot trends or issues in specific months.
________________________________________

4. Bonus: Payment Method vs Status Breakdown
   
SELECT
    payment_method,
    payment_status,
    COUNT(*) AS total_transactions,
    ROUND(SUM(payment_amount), 2) AS total_amount
FROM
    payments
GROUP BY
    payment_method, payment_status
ORDER BY
    payment_method, payment_status;

________________________________________

