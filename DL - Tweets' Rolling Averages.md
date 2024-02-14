# DL - Tweets' Rolling Averages

Created: February 14, 2024 10:05 PM
Category: SQL
Reviewed: No

## The Question

Given a table of tweet data over a specified time period, calculate the 3-day rolling average of tweets for each user. Output the user ID, tweet date, and rolling averages rounded to 2 decimal places.

Notes:

- A rolling average, also known as a moving average or running mean is a time-series technique that examines trends in data over a specified period of time.
- In this case, we want to determine how the tweet count for each user changes over a 3-day period.

## My Solution

```sql
-- Given a table of tweet data over a specified time period, 
-- calculate the 3-day rolling average of tweets for each user. 
-- Output the user ID, tweet date, and rolling averages rounded to 2 decimal places.

SELECT
user_id
, tweet_date
, ROUND(AVG(tweet_count) OVER(
    PARTITION BY user_id 
    ORDER BY tweet_date 
    ROWS BETWEEN 2 PRECEDING AND CURRENT ROW), 2)
      AS rolling_avg_3d
FROM tweets
```

## My Thought Process

rolling average is a use case of window function where 3-day is a window

we will use AVG(), partitioned by user_id, and ordered by tweet_date ascending for the current value and the two preceding values

The final output is:

| user_id | tweet_date | rolling_avg_3d |
| --- | --- | --- |
| 111 | 06/01/2022 00:00:00 | 2.00 |
| 111 | 06/02/2022 00:00:00 | 1.50 |
| 111 | 06/03/2022 00:00:00 | 2.00 |
| 111 | 06/04/2022 00:00:00 | 2.67 |
| 111 | 06/05/2022 00:00:00 | 4.00 |
| 111 | 06/06/2022 00:00:00 | 4.33 |
| 111 | 06/07/2022 00:00:00 | 5.00 |
| 199 | 06/01/2022 00:00:00 | 7.00 |
| 199 | 06/02/2022 00:00:00 | 6.00 |
| 199 | 06/03/2022 00:00:00 | 7.00 |
| 199 | 06/04/2022 00:00:00 | 5.00 |
| 199 | 06/05/2022 00:00:00 | 6.00 |
| 199 | 06/06/2022 00:00:00 | 3.67 |
| 199 | 06/07/2022 00:00:00 | 4.00 |
| 254 | 06/01/2022 00:00:00 | 1.00 |
| 254 | 06/02/2022 00:00:00 | 1.00 |
| 254 | 06/03/2022 00:00:00 | 1.33 |
| 254 | 06/04/2022 00:00:00 | 1.33 |
| 254 | 06/05/2022 00:00:00 | 2.00 |
| 254 | 06/06/2022 00:00:00 | 1.67 |
| 254 | 06/07/2022 00:00:00 | 2.33 |

## The Data

### `tweets` Table:

| Column Name | Type |
| --- | --- |
| user_id | integer |
| tweet_date | timestamp |
| tweet_count | integer |

### `tweets` Example Input:

| user_id | tweet_date | tweet_count |
| --- | --- | --- |
| 111 | 06/01/2022 00:00:00 | 2 |
| 111 | 06/02/2022 00:00:00 | 1 |
| 111 | 06/03/2022 00:00:00 | 3 |
| 111 | 06/04/2022 00:00:00 | 4 |
| 111 | 06/05/2022 00:00:00 | 5 |

### Example Output:

| user_id | tweet_date | rolling_avg_3d |
| --- | --- | --- |
| 111 | 06/01/2022 00:00:00 | 2.00 |
| 111 | 06/02/2022 00:00:00 | 1.50 |
| 111 | 06/03/2022 00:00:00 | 2.00 |
| 111 | 06/04/2022 00:00:00 | 2.67 |
| 111 | 06/05/2022 00:00:00 | 4.00 |

The dataset you are querying against may have different input & output - **this is just an example**!