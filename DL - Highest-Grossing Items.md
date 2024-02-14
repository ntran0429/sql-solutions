# DL - Highest-Grossing Items

Created: February 14, 2024 11:37 PM
Category: SQL
Reviewed: No

## The Question

Assume you're given a table containing data on Amazon customers and their spending on products in different category, write a query to identify the top two highest-grossing products within each category in the year 2022. The output should include the category, product, and total spend.

## My Solution

```sql
-- identify the top two highest-grossing products within each category 
-- in the year 2022. 
-- The output should include the category, product, and total spend.
WITH A AS
(
  SELECT
  category
  , product
  , SUM(spend) AS total_spend
  FROM product_spend
  WHERE DATE_PART('year', transaction_date) = 2022
  GROUP BY category, product -- aggregate to eliminate duplicates in product
)
, B AS
(
  SELECT
  category
  , product
  , total_spend
  , ROW_NUMBER() OVER(PARTITION BY category ORDER BY total_spend DESC) AS rank
  FROM A
)
SELECT
category
, product
, total_spend
FROM B
WHERE rank IN (1,2)
```

A cleaner code with only one CTE:

```sql
WITH A AS
(
  SELECT
  category
  , product
  , SUM(spend) AS total_spend
  , RANK() OVER(
            PARTITION BY category
            ORDER BY SUM(spend) DESC) AS rank
  FROM product_spend
  WHERE DATE_PART('year', transaction_date) = 2022
  GROUP BY category, product
)
SELECT
category
, product
, total_spend
-- , rank
FROM A
WHERE rank <= 2
```

## My Thought Process

CTE A computes the total gross for each product

CTE B uses a window function ROW_NUMBER() to rank the products by total spend within each category

the final query filters for the top two highest grossing products within each category

The final output is:

| category | product | total_spend |
| --- | --- | --- |
| appliance | washing machine | 439.80 |
| appliance | refrigerator | 299.99 |
| electronics | vacuum | 486.66 |
| electronics | wireless headset | 467.89 |

## The Data

### `product_spend` Table:

| Column Name | Type |
| --- | --- |
| category | string |
| product | string |
| user_id | integer |
| spend | decimal |
| transaction_date | timestamp |

### `product_spend` Example Input:

| category | product | user_id | spend | transaction_date |
| --- | --- | --- | --- | --- |
| appliance | refrigerator | 165 | 246.00 | 12/26/2021 12:00:00 |
| appliance | refrigerator | 123 | 299.99 | 03/02/2022 12:00:00 |
| appliance | washing machine | 123 | 219.80 | 03/02/2022 12:00:00 |
| electronics | vacuum | 178 | 152.00 | 04/05/2022 12:00:00 |
| electronics | wireless headset | 156 | 249.90 | 07/08/2022 12:00:00 |
| electronics | vacuum | 145 | 189.00 | 07/15/2022 12:00:00 |

### Example Output:

| category | product | total_spend |
| --- | --- | --- |
| appliance | refrigerator | 299.99 |
| appliance | washing machine | 219.80 |
| electronics | vacuum | 341.00 |
| electronics | wireless headset | 249.90 |

### Explanation:

Within the "appliance" category, the top two highest-grossing products are "refrigerator" and "washing machine."

In the "electronics" category, the top two highest-grossing products are "vacuum" and "wireless headset."

The dataset you are querying against may have different input & output - **this is just an example**!