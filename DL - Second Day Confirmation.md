# DL - Second Day Confirmation

Created: January 29, 2024 8:01 PM
Category: SQL
Reviewed: No

## The Question

Assume you're given tables with information about TikTok user sign-ups and confirmations through 
email and text. New users on TikTok sign up using their email addresses, and upon sign-up, each user receives a text message confirmation to activate their account.

Write a query to **display the user IDs of those who did not confirm their sign-up on the first day, but confirmed on the second day.**

Definition:

- `action_date` refers to the date when users activated their accounts and confirmed their sign-up through text messages.

## My Solution

```sql
SELECT
E.user_id
-- , E.signup_date
-- , T.action_date
FROM emails E
JOIN texts T
ON E.email_id = T.email_id
WHERE T.signup_action = 'Confirmed'
AND DATE_PART('day', T.action_date - E.signup_date) = 1.0
```

## My Thought Process

action_date indicates the when a user took a signup_action

action_date - signup_date to get the difference between when the user signs up and when he/she confirms

The final output is:

| user_id | signup_date | action_date |
| --- | --- | --- |
| 1052 | 07/09/2022 00:00:00 | 07/10/2022 00:00:00 |
| 1235 | 07/25/2022 00:00:00 | 07/26/2022 00:00:00 |

## The Data

### `emails` Table:

| Column Name | Type |
| --- | --- |
| email_id | integer |
| user_id | integer |
| signup_date | datetime |

### `emails` Example Input:

| email_id | user_id | signup_date |
| --- | --- | --- |
| 125 | 7771 | 06/14/2022 00:00:00 |
| 433 | 1052 | 07/09/2022 00:00:00 |

### `texts` Table:

| Column Name | Type |
| --- | --- |
| text_id | integer |
| email_id | integer |
| signup_action | string ('Confirmed', 'Not confirmed') |
| action_date | datetime |

### `texts` Example Input:

| text_id | email_id | signup_action | action_date |
| --- | --- | --- | --- |
| 6878 | 125 | Confirmed | 06/14/2022 00:00:00 |
| 6997 | 433 | Not Confirmed | 07/09/2022 00:00:00 |
| 7000 | 433 | Confirmed | 07/10/2022 00:00:00 |

### Example Output:

| user_id |
| --- |
| 1052 |

### Explanation:

Only User 1052 confirmed their sign-up on the second day.

The dataset you are querying against may have different input & output - **this is just an example**!