# DL - Teams Power Users



## The Question

Write a query to identify the **top 2 Power Users who sent the highest number of messages on Microsoft Teams in August 2022.** Display the IDs of these 2 users along with the total number of messages they sent. Output the results in descending order based on the count of the messages.

Assumption:

- No two users have sent the same number of messages in August 2022 (so no need for a window function).

## My Solution

```sql
SELECT
sender_id
, COUNT(message_id)
FROM messages
WHERE DATE_PART('year', sent_date) = 2022
AND DATE_PART('month', sent_date) = 8
GROUP BY sender_id
ORDER BY COUNT(message_id) DESC
LIMIT 2
```

Note we can also use the `EXTRACT()` function extracts a specific component (i.e. year, month, day, hour, or minute) from a date or timestamp.

eg. EXTRACT(MONTH FROM sent_date)

## My Thought Process

Notice we want to compute number of messages per user. This means we need a GROUP BY.

First we filter for messages sent in 8/2022 using the WHERE clause

Then we order by the message count and LIMIT to only the top 2 users

The final output is:

| sender_id | count |
| --- | --- |
| 3601 | 4 |
| 2520 | 3 |

## The Data

### **`messages` Table:**

| Column Name | Type |
| --- | --- |
| message_id | integer |
| sender_id | integer |
| receiver_id | integer |
| content | varchar |
| sent_date | datetime |

### **`messages` Example Input:**

| message_id | sender_id | receiver_id | content | sent_date |
| --- | --- | --- | --- | --- |
| 901 | 3601 | 4500 | You up? | 08/03/2022 00:00:00 |
| 902 | 4500 | 3601 | Only if you're buying | 08/03/2022 00:00:00 |
| 743 | 3601 | 8752 | Let's take this offline | 06/14/2022 00:00:00 |
| 922 | 3601 | 4500 | Get on the call | 08/10/2022 00:00:00 |

### **Example Output:**

| sender_id | message_count |
| --- | --- |
| 3601 | 2 |
| 4500 | 1 |

The dataset you are querying against may have different input & output - **this is just an example**!
