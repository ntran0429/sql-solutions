# DL - LinkedIn SQL Interview Question

## The Question

Given a table of candidates and their skills, you're tasked with finding the candidates best suited for an open Data Science job. **You want to find candidates who are proficient in Python, Tableau, and PostgreSQL.**

Write a query to list the candidates who possess all of the required skills for the job. Sort the output by candidate ID in ascending order.

**Assumption:**

- There are no duplicates in the `candidates` table.


## My Solution

```sql
WITH skills AS 
(
  SELECT
  candidate_id
  , skill
  FROM candidates
  WHERE skill IN('Python','Tableau','PostgreSQL')
),
skills_agg AS
(
  SELECT
  candidate_id
  , STRING_AGG(skill, ', ') AS skills
  FROM skills
  GROUP BY candidate_id
)
SELECT
candidate_id
FROM skills_agg
WHERE skills LIKE '%Python%'
AND skills LIKE '%Tableau%'
AND skills LIKE '%PostgreSQL%'

-- STRING_AGG ( expression, separator [order_by_clause] )
-- https://stackoverflow.com/questions/71149172/concatenate-distinct-values-in-a-group-by
```

## My Thinking Process
We'll start by using the IN operator to find candidates which have some of the required skills (the first CTE).

Then, we aggregate at the candidate level by concatenating their skills (the second CTE).

Finally, we can search for the three required skills and figure out which candidates have all three (the final SELECT query).

The final output:
| candidate_id |
| --- |
| 123 |
| 147 |

## The Data
 
### **`candidates` Table:**

| Column Name | Type |
| --- | --- |
| candidate_id | integer |
| skill | varchar |

### **`candidates` Example Input:**

| candidate_id | skill |
| --- | --- |
| 123 | Python |
| 123 | Tableau |
| 123 | PostgreSQL |
| 234 | R |
| 234 | PowerBI |
| 234 | SQL Server |
| 345 | Python |
| 345 | Tableau |

### **Example Output:**

| candidate_id |
| --- |
| 123 |
