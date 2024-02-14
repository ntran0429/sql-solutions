# DL - User's Third Transaction

Created: February 14, 2024 5:54 PM
Category: SQL
Reviewed: No

## The Question

Assume you are given the table below on Uber transactions made by users. Write a query to **obtain the third transaction of every user.** Output the user id, spend and transaction date.

## My Solution

```sql
-- obtain the third transaction of every user. 
-- Output the user id, spend and transaction date.
-- if an user does not have a third transaction, disregard

-- SELECT 
-- * 
-- FROM transactions
-- ORDER BY user_id, transaction_date

SELECT
user_id
, spend
, transaction_date
FROM
(
  SELECT 
  user_id
  , spend
  , transaction_date
  , ROW_NUMBER() OVER(PARTITION BY user_id ORDER BY transaction_date) 
			AS transaction_order
  FROM transactions
) A
WHERE transaction_order = 3
```

## My Thought Process

We first rank each user’s transactions by transaction date by using a window function ROW_NUMBER(), partitioned by user_id and order by transaction_date

We then filter for the transaction marked as ‘3’ for each user

Note that we can’t have a window function in a WHERE clause

We can also use a CTE for this.

Differences between a subquery and a CTE?

- **Subqueries** are used to nest queries within other queries, working from the inside out.
- **CTEs** are named, temporary result sets defined at the beginning of a query, providing a modular and readable way to structure complex queries. The CTE is evaluated only once and can be referenced multiple times in the main query.

Both methods give the same output and perform fairly similarly. Differences are CTE is reusable during the entire query session and more readable, whereas subquery can be used in FROM and WHERE clauses (within the main query only) and can act as a column with a single value.

The final output is:

| user_id | spend | transaction_date |
| --- | --- | --- |
| 111 | 89.60 | 02/05/2022 12:00:00 |
| 121 | 67.90 | 04/03/2022 12:00:00 |
| 263 | 100.00 | 07/12/2022 12:00:00 |

## The Data

### `transactions` Table:

| Column Name | Type |
| --- | --- |
| user_id | integer |
| spend | decimal |
| transaction_date | timestamp |

### `transactions` Example Input:

| user_id | spend | transaction_date |
| --- | --- | --- |
| 111 | 100.50 | 01/08/2022 12:00:00 |
| 111 | 55.00 | 01/10/2022 12:00:00 |
| 121 | 36.00 | 01/18/2022 12:00:00 |
| 145 | 24.99 | 01/26/2022 12:00:00 |
| 111 | 89.60 | 02/05/2022 12:00:00 |

### Example Output:

| user_id | spend | transaction_date |
| --- | --- | --- |
| 111 | 89.60 | 02/05/2022 12:00:00 |

The dataset you are querying against may have different input & output - **this is just an example**!