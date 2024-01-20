# DL - Average Post Hiatus

Created: January 19, 2024 12:52 AM
Category: SQL
Reviewed: No

## The Question

Given a table of Facebook posts, for each user who posted at least twice in 2021, write a query to find the number of days between each user’s first post of the year and last post of the year in the year 2021. Output the user and number of the days between each user's first and last post.

## My Solution

```sql
-- identify users with at least two posts in 2021
WITH post_counts AS (
SELECT 
user_id
, COUNT(*) post_count
FROM posts
WHERE DATE_PART('year', post_date) = '2021'
GROUP BY user_id
)
, user_atleast_twice AS (
SELECT
user_id
FROM post_counts
WHERE post_count >= 2
)
-- then use JOIN to get attributes from those users
SELECT
A.user_id
, DATE_PART('day',MAX(B.post_date) - MIN(B.post_date)) 
    AS days_between
FROM user_atleast_twice A
JOIN posts B
ON A.user_id = B.user_id
GROUP BY A.user_id

-- SELECT
-- user_id
-- , post_date
-- FROM posts
-- WHERE DATE_PART('year', post_date::DATE) = 2021 
-- AND user_id IN('151652','661093')
```

A cleaner code is achieved with HAVING to filter for users with at least two posts:

```sql
SELECT 
user_id
, MAX(post_date::DATE) - MIN(post_date::DATE) AS days_between
FROM posts
WHERE DATE_PART('year', post_date::DATE) = 2021 
GROUP BY user_id
HAVING COUNT(post_id)>1;
```

## My Thought Process

First, filter for user ids with at least two posts. I achieve this with two CTEs.

The final SELECT is to calculate the days between the first and last post for each user.

The final output is:

| user_id | days_between |
| --- | --- |
| 151652 | 307 |
| 661093 | 206 |

## The Data

### **`posts` Table:**

| Column Name | Type |
| --- | --- |
| user_id | integer |
| post_id | integer |
| post_date | timestamp |
| post_content | text |

### **`posts` Example Input:**

| user_id | post_id | post_date | post_content |
| --- | --- | --- | --- |
| 151652 | 599415 | 07/10/2021 12:00:00 | Need a hug |
| 661093 | 624356 | 07/29/2021 13:00:00 | Bed. Class 8-12. Work 12-3. Gym 3-5 or 6. Then class 6-10. Another day that's gonna fly by. I miss my girlfriend |
| 004239 | 784254 | 07/04/2021 11:00:00 | Happy 4th of July! |
| 661093 | 442560 | 07/08/2021 14:00:00 | Just going to cry myself to sleep after watching Marley and Me. |
| 151652 | 111766 | 07/12/2021 19:00:00 | I'm so done with covid - need travelling ASAP! |

### **Example Output:**

| user_id | days_between |
| --- | --- |
| 151652 | 2 |
| 661093 | 21 |

The dataset you are querying against may have different input & output - **this is just an example**!
