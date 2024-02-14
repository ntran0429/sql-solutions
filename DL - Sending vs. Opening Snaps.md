# DL -

Created: February 14, 2024 6:14 PM
Category: SQL
Reviewed: No

## The Question

Assume you're given tables with information on Snapchat users, including their ages and time spent sending and opening snaps.

Write a query to obtain a breakdown of the time spent sending vs. opening snaps as a percentage of total time spent on these activities grouped by age group. Round the percentage to 2 decimal places in the output.

Notes:

- Calculate the following percentages:
    - time spent sending / (Time spent sending + Time spent opening)
    - Time spent opening / (Time spent sending + Time spent opening)
- To avoid integer division in percentages, multiply by 100.0 and not 100.

## My Solution

```sql
-- obtain a breakdown of the time spent sending vs. opening snaps 
-- as a percentage of total time spent on these activities 
-- grouped by age group. 
-- Round the percentage to 2 decimal places in the output.
-- age_bucket	| send_perc	| open_perc
WITH A AS
(
  SELECT
  A.user_id
  , A.activity_type
  , A.time_spent
  , A.activity_date
  , B.age_bucket
  FROM activities A
  JOIN age_breakdown B
  ON A.user_id = B.user_id
)
, B AS
(
  SELECT
  age_bucket
  , SUM(time_spent) FILTER(WHERE activity_type = 'send') AS total_spent_send
  , SUM(time_spent) FILTER(WHERE activity_type = 'open') AS total_spent_open
  , SUM(time_spent) FILTER(WHERE activity_type = 'send')
    + SUM(time_spent) FILTER(WHERE activity_type = 'open')
      AS total_time_spent
  FROM A
  GROUP BY age_bucket
)
SELECT
age_bucket
, ROUND(total_spent_send / total_time_spent * 100.0, 2) AS send_perc
, ROUND(total_spent_open / total_time_spent * 100.0, 2) AS open_perc
FROM B
```

A cleaner code can be achieved using SUM and FILTER:

```sql
SELECT
B.age_bucket
, ROUND(SUM(A.time_spent) FILTER(WHERE A.activity_type = 'send') /
  SUM(A.time_spent) * 100.0, 2)
    AS send_perc
, ROUND(SUM(A.time_spent) FILTER(WHERE A.activity_type = 'open') /
  SUM(A.time_spent) * 100.0, 2)
    AS open_perc
FROM activities A
JOIN age_breakdown B
ON A.user_id = B.user_id
WHERE activity_type IN ('send', 'open')
GROUP BY B.age_bucket
```

## My Thought Process

CTE A is to get the age group for each user

CTE B is to calculate the numerators and denominator for the final query, at the age bucket level

the final main query performs the calculations

The final output is:

| age_bucket | send_perc | open_perc |
| --- | --- | --- |
| 21-25 | 54.31 | 45.69 |
| 26-30 | 82.26 | 17.74 |
| 31-35 | 37.84 | 62.16 |

## The Data

### `activities` Table

| Column Name | Type |
| --- | --- |
| activity_id | integer |
| user_id | integer |
| activity_type | string ('send', 'open', 'chat') |
| time_spent | float |
| activity_date | datetime |

### `activities` Example Input

| activity_id | user_id | activity_type | time_spent | activity_date |
| --- | --- | --- | --- | --- |
| 7274 | 123 | open | 4.50 | 06/22/2022 12:00:00 |
| 2425 | 123 | send | 3.50 | 06/22/2022 12:00:00 |
| 1413 | 456 | send | 5.67 | 06/23/2022 12:00:00 |
| 1414 | 789 | chat | 11.00 | 06/25/2022 12:00:00 |
| 2536 | 456 | open | 3.00 | 06/25/2022 12:00:00 |

### `age_breakdown` Table

| Column Name | Type |
| --- | --- |
| user_id | integer |
| age_bucket | string ('21-25', '26-30', '31-25') |

### `age_breakdown` Example Input

| user_id | age_bucket |
| --- | --- |
| 123 | 31-35 |
| 456 | 26-30 |
| 789 | 21-25 |

### Example Output

| age_bucket | send_perc | open_perc |
| --- | --- | --- |
| 26-30 | 65.40 | 34.60 |
| 31-35 | 43.75 | 56.25 |

### Explanation

Using the age bucket 26-30 as example, the time spent sending snaps was 5.67 and the time spent opening snaps was 3.

To calculate the percentage of time spent sending snaps, we divide 
the time spent sending snaps by the total time spent on sending and 
opening snaps, which is 5.67 + 3 = 8.67.

So, the percentage of time spent sending snaps is 5.67 / (5.67 + 3) =
 65.4%, and the percentage of time spent opening snaps is 3 / (5.67 + 3)
 = 34.6%.

The dataset you are querying against may have different input & output - **this is just an example**!