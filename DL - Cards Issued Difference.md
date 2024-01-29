# DL - Cards Issued Difference

Created: January 29, 2024 8:44 PM
Category: SQL
Reviewed: No

## The Question

Your team at JPMorgan Chase is preparing to launch a new credit card, and to gain some 
insights, you're analyzing how many credit cards were issued each month.

Write a query that **outputs the name of each credit card and the difference in the number of issued cards between the month with the highest issuance cards and the lowest issuance.** Arrange the results based on the largest disparity.

## My Solution

```sql
WITH total_issues AS
(SELECT
card_name
, issue_month
, issue_year
, SUM(issued_amount) AS total_issued
FROM monthly_cards_issued
GROUP BY card_name, issue_month, issue_year)

, max_min AS
(SELECT
card_name
, MAX(total_issued) AS max
, MIN(total_issued) AS min
FROM total_issues
GROUP BY card_name)

SELECT
card_name
, max - min AS difference
FROM max_min
ORDER BY max - min DESC
```

I made an unnecessary use of CTEs, a cleaner SQL query is below:

```sql
SELECT
card_name
, MAX(issued_amount) - MIN(issued_amount) AS difference
FROM monthly_cards_issued
GROUP BY card_name
ORDER BY difference DESC
```

## My Thought Process

Each row represents an issue amount per month for any particular credit card

we can do the aggregation (max - min) right away in the main query

The final output is:

| card_name | difference |
| --- | --- |
| Chase Sapphire Reserve | 30000 |
| Chase Freedom Flex | 15000 |

## The Data

### `monthly_cards_issued` Table:

| Column Name | Type |
| --- | --- |
| issue_month | integer |
| issue_year | integer |
| card_name | string |
| issued_amount | integer |

### `monthly_cards_issued` Example Input:

| card_name | issued_amount | issue_month | issue_year |
| --- | --- | --- | --- |
| Chase Freedom Flex | 55000 | 1 | 2021 |
| Chase Freedom Flex | 60000 | 2 | 2021 |
| Chase Freedom Flex | 65000 | 3 | 2021 |
| Chase Freedom Flex | 70000 | 4 | 2021 |
| Chase Sapphire Reserve | 170000 | 1 | 2021 |
| Chase Sapphire Reserve | 175000 | 2 | 2021 |
| Chase Sapphire Reserve | 180000 | 3 | 2021 |

### Example Output:

| card_name | difference |
| --- | --- |
| Chase Freedom Flex | 15000 |
| Chase Sapphire Reserve | 10000 |

### Explanation:

Chase Freedom Flex's best month was 70k cards issued and the worst month was 55k cards, so the difference is 15k cards.

Chase Sapphire Reserve’s best month was 180k cards issued and the worst month was 170k cards, so the difference is 10k cards.

The dataset you are querying against may have different input & output - **this is just an example**!
