# DL - Page With No Likes

Created: January 18, 2024 4:16 PM
Category: SQL
Reviewed: No

## The Question

Assume you're given two tables containing data about Facebook Pages and their respective likes (as in "Like a Facebook Page").

Write a query to **return the IDs of the Facebook pages that have zero likes.** The output should be sorted in ascending order based on the page IDs.

## My Solution

```sql
SELECT 
A.page_id
FROM pages A
LEFT JOIN page_likes B
ON A.page_id = B.page_id
WHERE B.page_id IS NULL
ORDER BY A.page_id
```

## My Thinking Process

I comprehend that ‘page_likes’ table only contains page ids with likes. In other words, pages without likes do not have their ids in this table. So I perform a LEFT JOIN to preserve the records in ‘pages’ table, leaving some nulls in the page_id field of ‘page_likes’:

```sql
SELECT 
*
FROM pages A
LEFT JOIN page_likes B
ON A.page_id = B.page_id
ORDER BY A.page_id
```

The output:

| page_id | page_name | user_id | page_id | liked_date |
| --- | --- | --- | --- | --- |
| 20001 | SQL Solutions | 111 | 20001 | 04/08/2022 00:00:00 |
| 20001 | SQL Solutions | 156 | 20001 | 07/25/2022 00:00:00 |
| 20001 | SQL Solutions | 125 | 20001 | 07/19/2022 00:00:00 |
| 20045 | Brain Exercises | 255 | 20045 | 07/19/2022 00:00:00 |
| 20045 | Brain Exercises | 121 | 20045 | 03/12/2022 00:00:00 |
| 20701 | Tips for Data Analysts | NULL | NULL | NULL |
| 31111 | Postgres Crash Course | 144 | 31111 | 06/21/2022 00:00:00 |
| 31111 | Postgres Crash Course | 125 | 31111 | 07/04/2022 00:00:00 |
| 32728 | Break the thread | NULL | NULL | NULL |

Then I filter for records where those nulls occur:

```sql
SELECT 
A.page_id
FROM pages A
LEFT JOIN page_likes B
ON A.page_id = B.page_id
WHERE B.page_id IS NULL
ORDER BY A.page_id
```

The final output:

| page_id |
| --- |
| 20701 |
| 32728 |

## The Data

### **`pages` Table:**

| Column Name | Type |
| --- | --- |
| page_id | integer |
| page_name | varchar |

### **`pages` Example Input:**

| page_id | page_name |
| --- | --- |
| 20001 | SQL Solutions |
| 20045 | Brain Exercises |
| 20701 | Tips for Data Analysts |

### **`page_likes` Table:**

| Column Name | Type |
| --- | --- |
| user_id | integer |
| page_id | integer |
| liked_date | datetime |

### **`page_likes` Example Input:**

| user_id | page_id | liked_date |
| --- | --- | --- |
| 111 | 20001 | 04/08/2022 00:00:00 |
| 121 | 20045 | 03/12/2022 00:00:00 |
| 156 | 20001 | 07/25/2022 00:00:00 |

### **Example Output:**

| page_id |
| --- |
| 20701 |

The dataset you are querying against may have different input & output - **this is just an example**!