# DL - Cities With Completed Trades



## The Question

This is the same question as problem #2 in the SQL Chapter of [Ace the Data Science Interview](https://amzn.to/3kF79Fx)!

Assume you're given the tables containing completed trade orders and user details in a Robinhood trading system.

Write a query to **retrieve the top three cities that have the highest number of completed trade orders** listed in descending order. Output the city name and the corresponding number of completed trade orders.

## My Solution

```sql
SELECT
U.city
, COUNT(T.order_id) AS total_orders
FROM trades T
JOIN users U
ON T.user_id = U.user_id
WHERE status = 'Completed'
GROUP BY U.city
ORDER BY COUNT(T.order_id) DESC
LIMIT 3
```

## My Thought Process

First filter for completed trades in ‘trades’ table.

JOIN ‘trades with ‘users’ to get the user’s attributes for each trade record

Aggregate by city and count the number of orders

Order from largest to smallest number of orders

Limit to the first 3 records

The final output is:

| city | total_orders |
| --- | --- |
| San Francisco | 4 |
| Boston | 3 |
| Denver | 2 |

## The Data

### **`trades` Table:**

| Column Name | Type |
| --- | --- |
| order_id | integer |
| user_id | integer |
| price | decimal |
| quantity | integer |
| status | string('Completed' ,'Cancelled') |
| timestamp | datetime |

### **`trades` Example Input:**

| order_id | user_id | price | quantity | status | timestamp |
| --- | --- | --- | --- | --- | --- |
| 100101 | 111 | 9.80 | 10 | Cancelled | 08/17/2022 12:00:00 |
| 100102 | 111 | 10.00 | 10 | Completed | 08/17/2022 12:00:00 |
| 100259 | 148 | 5.10 | 35 | Completed | 08/25/2022 12:00:00 |
| 100264 | 148 | 4.80 | 40 | Completed | 08/26/2022 12:00:00 |
| 100305 | 300 | 10.00 | 15 | Completed | 09/05/2022 12:00:00 |
| 100400 | 178 | 9.90 | 15 | Completed | 09/09/2022 12:00:00 |
| 100565 | 265 | 25.60 | 5 | Completed | 12/19/2022 12:00:00 |

### **`users` Table:**

| Column Name | Type |
| --- | --- |
| user_id | integer |
| city | string |
| email | string |
| signup_date | datetime |

### **`users` Example Input:**

| user_id | city | email | signup_date |
| --- | --- | --- | --- |
| 111 | San Francisco | mailto:rrok10@gmail.com | 08/03/2021 12:00:00 |
| 148 | Boston | mailto:sailor9820@gmail.com | 08/20/2021 12:00:00 |
| 178 | San Francisco | mailto:harrypotterfan182@gmail.com | 01/05/2022 12:00:00 |
| 265 | Denver | mailto:shadower_@hotmail.com | 02/26/2022 12:00:00 |
| 300 | San Francisco | mailto:houstoncowboy1122@hotmail.com | 06/30/2022 12:00:00 |

### **Example Output:**

| city | total_orders |
| --- | --- |
| San Francisco | 3 |
| Boston | 2 |
| Denver | 1 |

### **Explanation**

In the given dataset, San Francisco has the highest number of completed trade orders with 3 orders. Boston holds the second position with 2 orders, and Denver ranks third with 1 order.

The dataset you are querying against may have different input & output - **this is just an example**!
