# E-commerce-Customer-Sales-Analytics
Analysis of the public dataset bigquery-public-data.thelook_ecommerce. SQL queries and RFM segmentation of customers, with a dashboard to visualize the corresponding data.

# Description
The fictional e-commerce company asked to study the behaviour of customers, in order to:
* expand the customer base
* increase customer retention

*Dataset* = bigquery-public-data.thelook_ecommerce

*Tools* = Google BigQuery, Power BI, Excel

# Database schema
Star schema, summarized in "Entity Relationship Diagram.png"

# Example queries
Do users always use the same IP address?
SELECT user_id, count(distinct(ip_address)) as n_ip_add
FROM `bigquery-public-data.thelook_ecommerce.events`
GROUP BY user_id
HAVING n_ip_add > 1
LIMIT 1000

Are there any items that were shipped before being created?
SELECT created_at, shipped_at
FROM `bigquery-public-data.thelook_ecommerce.order_items` 
WHERE shipped_at < created_at
LIMIT 100

Which are the top 10 products sold?
SELECT product_id, name, category, count(it.id) as n_items
FROM `bigquery-public-data.thelook_ecommerce.order_items` it
JOIN `bigquery-public-data.thelook_ecommerce.products` pro
  ON it.product_id = pro.id
WHERE it.status not in ("Cancelled", "Returned")
GROUP BY product_id, name, category
ORDER BY n_items DESC
LIMIT 10

# Insights
Summarized in the file "Purpose & Key findings.pptx".
Some example include: 
* 71% of the customers come from 3 countries (China, USA and Brazil)
* The average number of orders per customer is constant over countries (1.25 orders per customer)
* A large number of customers (~ 20%) never purchased anything
* There are little differences between the behaviour of customer segments in terms of revenue, country, gender and age bracket

# How to
The queries can be found in the sheets of "Data Understanding.xlsx", divided by topic (e.g. Data quality, Customer segmentation...)

The report can be downloaded at https://drive.google.com/uc?export=download&id=14WJEaM_g7GOgcOWjzgb7tdHiZvCJG-SX

