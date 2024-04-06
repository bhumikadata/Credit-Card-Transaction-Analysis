# Credit-Card-Transaction-Analysis

# Problem Statement

You are given a history of credit card transaction data for the people of India across cities. The task is to find out how many days each city took to reach a cumulative spend of 1500 from its first day of transactions.

![Day29](https://github.com/bhumikadata/Credit-Card-Transaction-Analysis/assets/131578649/117b84cc-adeb-4017-973f-5aa59a2f80a4)


# SQL Code

```
-- Calculate cumulative spend for each city
WITH cumulative_spend AS (
    SELECT
        city,
        transaction_date,
        SUM(amount) OVER (PARTITION BY city ORDER BY transaction_date) AS total_amount
    FROM
        credit_card_transactions
)
-- Find the date when cumulative spend reaches 1500 for each city
SELECT
    city,
    MIN(transaction_date) AS first_transaction_date,
    MIN(transaction_date) + INTERVAL '1 DAY' * (total_amount >= 1500) AS tran_date_1500,
    DATEDIFF(MIN(transaction_date) + INTERVAL '1 DAY' * (total_amount >= 1500), MIN(transaction_date)) AS no_of_days
FROM
    cumulative_spend
GROUP BY
    city, total_amount
HAVING
    total_amount >= 1500
```


# Business Impact

#### Performance Analysis: 

This analysis can provide insights into the spending behavior of customers in different cities. It can help the company understand which cities are more active in terms of credit card transactions and how quickly they reach certain spending thresholds.

#### Customer Engagement: 

Understanding the spending patterns can aid in targeted marketing efforts and customer engagement strategies. For example, the company can offer promotions or rewards to encourage more spending in cities where it takes longer to reach the 1500 threshold.

#### Risk Management: 

Identifying cities where customers reach the spending threshold quickly can also help in risk management. It can indicate potential fraudulent activities or high-risk transactions that require closer scrutiny.
