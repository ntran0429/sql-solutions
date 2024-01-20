# DL - Duplicate Job Listings

Created: January 19, 2024 9:56 PM
Category: SQL
Reviewed: No

## The Question

This is the same question as problem #8 in the SQL Chapter of [Ace the Data Science Interview](https://amzn.to/3kF79Fx)!

Assume you're given a table containing job postings from various companies on the LinkedIn platform. Write a query to retrieve the count of companies that have posted duplicate job listings.

Definition:

- Duplicate job listings are defined as two job listings within the **same company that share identical titles and descriptions.**

## My Solution

```sql
SELECT
COUNT(company_id) AS duplicate_companies
FROM
(
  SELECT
  company_id
  , title
  , description
  , COUNT(job_id)
  FROM job_listings
  GROUP BY company_id, title, description
  HAVING COUNT(job_id) > 1
) A
```

## My Thought Process

Because, duplicate listings have identical company_id, title, description, I use a subquery to aggregate the counts by those fields. Any listing with a count > 1 is considered a duplicate.

```sql
SELECT
  company_id
  , title
  , description
  , COUNT(job_id)
 FROM job_listings
 GROUP BY company_id, title, description
 HAVING COUNT(job_id) > 1
```

| company_id | title | description | count |
| --- | --- | --- | --- |
| 827 | Data Scientist | Data scientist uses data to understand and explain the phenomena around them, and help organizations make better decisions. | 2 |
| 244 | Software Engineer | Software engineers design and create computer systems and applications to solve real-world problems. | 2 |
| 345 | Data Analyst | Data analyst reviews data to identify key insights into a business's customers and ways the data can be used to solve problems. | 2 |

Then I count the number of records output by the subquery to get the number of companies that posted duplicate listings.

The final output is:

| duplicate_companies |
| --- |
| 3 |

## The Data

### **`job_listings` Table:**

| Column Name | Type |
| --- | --- |
| job_id | integer |
| company_id | integer |
| title | string |
| description | string |

### **`job_listings` Example Input:**

| job_id | company_id | title | description |
| --- | --- | --- | --- |
| 248 | 827 | Business Analyst | Business analyst evaluates past and current business data with the primary goal of improving decision-making processes within organizations. |
| 149 | 845 | Business Analyst | Business analyst evaluates past and current business data with the primary goal of improving decision-making processes within organizations. |
| 945 | 345 | Data Analyst | Data analyst reviews data to identify key insights into a business's customers and ways the data can be used to solve problems. |
| 164 | 345 | Data Analyst | Data analyst reviews data to identify key insights into a business's customers and ways the data can be used to solve problems. |
| 172 | 244 | Data Engineer | Data engineer works in a variety of settings to build systems that collect, manage, and convert raw data into usable information for data scientists and business analysts to interpret. |

### **Example Output:**

| duplicate_companies |
| --- |
| 1 |

### **Explanation:**

There is one company ID 345 that posted duplicate job listings. The duplicate listings, IDs 945 and 164 have identical titles and descriptions.

The dataset you are querying against may have different input & output - **this is just an example**!