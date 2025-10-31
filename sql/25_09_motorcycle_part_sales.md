---
title: 'Analyzing Motorcycle Part Sales'
author: 'Chiawei Wang'
date: 'September 2025'
---

`This document presents a technical case study using SQL to analyze motorcycle part sales data from a company operating three warehouses. The focus is on calculating net revenue from wholesale sales by product line, month, and warehouse.`

# Technical Report: Analyzing Motorcycle Part Sales with SQL

## Background

The company operates three warehouses and offers a diverse range of motorcycle parts through both retail and wholesale channels. Payment options include credit card, cash, and bank transfer, each with its own transaction fee. To support strategic planning, it is important to analyze the net revenue from wholesale sales, broken down by product line, month, and warehouse.

## Research Questions

1. What is the net revenue generated from wholesale sales for each product line?
2. How does this net revenue vary month-to-month and across different warehouses?

## Data Overview

The dataset includes the following columns:

| Column         | Type    | Description                                                     |
| -------------- | ------- | --------------------------------------------------------------- |
| `order_number` | VARCHAR | Unique order number                                             |
| `date`         | DATE    | Date of the order (June to August 2021)                         |
| `warehouse`    | VARCHAR | Warehouse where the order was placed: Central, North, or West   |
| `client_type`  | VARCHAR | Indicates if the order is Retail or Wholesale                   |
| `product_line` | VARCHAR | Type of product ordered                                         |
| `quantity`     | INT     | Number of products ordered                                      |
| `unit_price`   | FLOAT   | Price per product (in dollars)                                  |
| `total`        | FLOAT   | Total price of the order (in dollars)                           |
| `payment`      | VARCHAR | Payment method: Credit card, Transfer, or Cash                  |
| `payment_fee`  | FLOAT   | Percentage of the total charged as a fee for the payment method |

## Approach

1. Conditionally converting date values
2. Calculating net revenue
3. Filtering, grouping, and sorting the results

```sql
SELECT product_line,
       warehouse,
       CASE WHEN EXTRACT('month' from date) = 6 THEN 'June'
            WHEN EXTRACT('month' from date) = 7 THEN 'July'
            WHEN EXTRACT('month' from date) = 8 THEN 'August'
            END as month,
       SUM(total) - SUM(payment_fee) AS net_revenue
FROM sales
WHERE client_type = 'Wholesale'
GROUP BY product_line, warehouse, month
ORDER BY product_line, warehouse, month DESC, net_revenue DESC;
```
## Results

| product_line          | warehouse | month   | net_revenue |
| --------------------- | --------- | ------- | ----------- |
| Braking System        | Central   | June    | 3684.89     |
| Braking System        | Central   | July    | 3778.65     |
| Braking System        | Central   | August  | 3039.41     |
| Braking System        | North     | June    | 1487.77     |
| Braking System        | North     | July    | 2594.44     |
| Braking System        | North     | August  | 1770.84     |
| Braking System        | West      | June    | 1212.75     |
| Braking System        | West      | July    | 3060.93     |
| Braking System        | West      | August  | 2500.67     |
| Electrical System     | Central   | June    | 2904.93     |
| Electrical System     | Central   | July    | 5577.62     |
| Electrical System     | Central   | August  | 3126.43     |
| Electrical System     | North     | June    | 2022.5      |
| Electrical System     | North     | July    | 1710.13     |
| Electrical System     | North     | August  | 4721.12     |
| Electrical System     | West      | July    | 449.46      |
| Electrical System     | West      | August  | 1241.84     |
| Engine                | Central   | June    | 6548.85     |
| Engine                | Central   | July    | 1827.03     |
| Engine                | Central   | August  | 9528.71     |
| Engine                | North     | July    | 1007.14     |
| Engine                | North     | August  | 2324.19     |
| Frame & Body          | Central   | June    | 5111.34     |
| Frame & Body          | Central   | July    | 3135.13     |
| Frame & Body          | Central   | August  | 8657.99     |
| Frame & Body          | North     | June    | 4910.12     |
| Frame & Body          | North     | July    | 6154.61     |
| Frame & Body          | North     | August  | 7898.89     |
| Frame & Body          | West      | June    | 2779.74     |
| Frame & Body          | West      | August  | 829.69      |
| Miscellaneous         | Central   | June    | 1878.07     |
| Miscellaneous         | Central   | July    | 3118.44     |
| Miscellaneous         | Central   | August  | 1739.76     |
| Miscellaneous         | North     | June    | 513.99      |
| Miscellaneous         | North     | July    | 2404.65     |
| Miscellaneous         | North     | August  | 1841.4      |
| Miscellaneous         | West      | June    | 2280.97     |
| Miscellaneous         | West      | July    | 1156.8      |
| Miscellaneous         | West      | August  | 813.43      |
| Suspension & Traction | Central   | June    | 3325        |
| Suspension & Traction | Central   | July    | 6456.72     |
| Suspension & Traction | Central   | August  | 5416.7      |
| Suspension & Traction | North     | June    | 8065.74     |
| Suspension & Traction | North     | July    | 3714.28     |
| Suspension & Traction | North     | August  | 4923.69     |
| Suspension & Traction | West      | June    | 2372.52     |
| Suspension & Traction | West      | July    | 2939.32     |
| Suspension & Traction | West      | August  | 1080.79     |

## Insights

- **Suspension & Traction Leads Revenue:** The Suspension & Traction product line consistently delivers the highest net revenue across all warehouses, indicating strong and steady demand.
- **Central Warehouse Outperforms:** The Central warehouse generates the most net revenue overall, suggesting it serves the largest customer base or operates with greater efficiency.
- **Frame & Body Shows Growth:** The Frame & Body product line demonstrates notable revenue growth, especially in August, pointing to possible seasonal trends or successful promotions.
- **Engine Sales Spike in August:** Engine-related products see a significant net revenue increase in August, which may be linked to seasonal maintenance cycles or targeted sales campaigns.
- **Warehouse and Month Variability:** Net revenue varies not only by product line but also by warehouse and month, emphasizing the importance of localized and time-sensitive sales strategies.

## Conclusion

The analysis of motorcycle part sales reveals key insights into product line performance, warehouse efficiency, and seasonal trends. The Suspension & Traction product line stands out as a consistent revenue driver, while the Central warehouse leads in overall net revenue. Understanding these patterns can inform inventory management, marketing strategies, and operational improvements to enhance profitability across all channels.

`Any questions, please reach out!`

Chiawei Wang, PhD\
Data & Product Analyst\
<chiawei.w@outlook.com>
