# DL - Compressed Mean

Created: January 29, 2024 9:35 PM
Category: SQL
Reviewed: No

## The Question

You're trying to **find the mean number of items per order** on Alibaba, rounded to 1 decimal place 
using a table with the corresponding number of orders for each item count.

## My Solution

```sql
SELECT
ROUND(CAST(SUM(r) AS DECIMAL) , 1) AS mean
FROM
(
	SELECT
	item_count
	, item_count * (1.0*order_occurrences) / SUM(order_occurrences) OVER() 
	    AS r
	FROM items_per_order
) A
```

cleaner code:

```sql
SELECT 
  SUM(item_count*order_occurrences)
    /SUM(order_occurrences) AS mean
FROM items_per_order;
```

## My Thought Process

mean number of items per order = 1*w1 + 2*w2 + 3*w3 + ... + 9*w9
calculate the weights first, then compute the sum product

another way is 

mean number of items per order = total items / total orders

The final output is:

| mean |
| --- |
| 3.9 |

## The Data

### `items_per_order` Table:

| Column Name | Type |
| --- | --- |
| item_count | integer |
| order_occurrences | integer |

### `items_per_order` Example Input:

| item_count | order_occurrences |
| --- | --- |
| 1 | 500 |
| 2 | 1000 |
| 3 | 800 |
| 4 | 1000 |

There are a total of 500 orders with one item per order, 1000 orders 
with two items per order, and 800 orders with three items per order."

### Example Output:

| mean |
| --- |
| 2.7 |

## Explanation

Let's calculate the arithmetic average:

Total items = `(1*500) + (2*1000) + (3*800) + (4*1000) = 8900`

Total orders = `500 + 1000 + 800 + 1000 = 3300`

Mean = `8900 / 3300 = 2.7`

The dataset you are querying against may have different input & output - **this is just an example**!