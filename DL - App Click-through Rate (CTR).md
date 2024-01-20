# DL - App Click-through Rate (CTR)

Created: January 19, 2024 10:45 PM
Category: SQL
Reviewed: No

## The Question

This is the same question as problem #1 in the SQL Chapter of [Ace the Data Science Interview](https://amzn.to/3kF79Fx)!

Assume you have an events table on Facebook app analytics. Write a query to **calculate the click-through rate (CTR) for the apps in 2022** and round the results to 2 decimal places.

Definition and note:

- Percentage of click-through rate (CTR) = 100.0 * Number of clicks / Number of impressions
- To avoid integer division, multiply the CTR by 100.0, not 100.

## My Solution

```sql
WITH event_counts AS (
	SELECT
	app_id
	, event_type
	, COUNT(event_type) AS event_count
	FROM events
	WHERE EXTRACT(YEAR FROM timestamp) = 2022
	GROUP BY event_type, app_id
	ORDER BY app_id
)
SELECT
app_id
, ROUND(
  100.0 *
  MAX(event_count) FILTER(WHERE event_type = 'click') /
  MAX(event_count) FILTER(WHERE event_type = 'impression')
  , 2) 
    AS ctr
FROM event_counts
GROUP BY app_id
```

## My Thought Process

First I filter for apps in 2022.

Then for each event type and app, I count the number of occurrences using a CTE.

```sql
SELECT
app_id
, event_type
, COUNT(event_type) AS event_count
FROM events
WHERE EXTRACT(YEAR FROM timestamp) = 2022
GROUP BY event_type, app_id
ORDER BY app_id
```

| app_id | event_type | event_count |
| --- | --- | --- |
| 123 | click | 2 |
| 123 | impression | 3 |
| 234 | click | 1 |
| 234 | impression | 3 |

I use FILTER and WHERE to take the corresponding click count and impression count for each app and apply the formula provided. MAX is used because FILTER needs to follow an aggregate function.

The final output is:

| app_id | ctr |
| --- | --- |
| 123 | 66.67 |
| 234 | 33.33 |

## The Data

### **`events` Table:**

| Column Name | Type |
| --- | --- |
| app_id | integer |
| event_type | string |
| timestamp | datetime |

### **`events` Example Input:**

| app_id | event_type | timestamp |
| --- | --- | --- |
| 123 | impression | 07/18/2022 11:36:12 |
| 123 | impression | 07/18/2022 11:37:12 |
| 123 | click | 07/18/2022 11:37:42 |
| 234 | impression | 07/18/2022 14:15:12 |
| 234 | click | 07/18/2022 14:16:12 |

### **Example Output:**

| app_id | ctr |
| --- | --- |
| 123 | 50.00 |
| 234 | 100.00 |

### **Explanation**

Let's consider an example of App 123. This app has a click-through rate (CTR) of 50.00% because out of the 2 impressions it received, it got 1 click.

To calculate the CTR, we divide the number of clicks by the number of impressions, and then multiply the result by 100.0 to express it as a percentage. In this case, 1 divided by 2 equals 0.5, and when multiplied by 100.0, it becomes 50.00%. So, the CTR of App 123 is 50.00%.

The dataset you are querying against may have different input & output - **this is just an example**!