# Capstone-Project-SQL
SQL analysis project for Kultra Mega Stores order data (2009-2012)

## Project Title: Business Intelligence Analysis of Kultra Mega Stores (2009-2012)

### Project Overview
This project analyzes historical order data from Kultra Mega Stores (KMS), a Lagos based company specializing in office supplies and furniture, with a focus on its Abuja division. Using SQL Server, I performed an analysis of sales trends, customer behavior, regional performance and shipping cost efficiency from 2009-2012.

### Data Source
This data was provided by The Incubator Hub as part of the final project requirements for the Digital SkillUp Africa Program.

### Tools Used
- SQL Server

### Project Objectives
- Identify top-performing product categories and regions
- Analyze sales and shipping patterns
- Uncover high-value and low-value customers
- Evaluate if shipping costs align with order priorities
- Provide actionable recommendations to management

### Exploratory Data Analysis
EDA involved the exploring of the data to answer some questions about the data such as:
1. What are the top-performing product categories and regions?
2. Who are the most and least valuable customers?
3. Which customer segments are underperforming?
4. Are high-priority orders receiving appropriate shipping methods?
5. How can revenue be increased from low-spending customers?

### Key Insights
- The bottom 10 customers are mostly from the Consumer Segment(Home Offices/Small Businesses) typically purchasing only Office Supplies(likely low margin items like Paper, Pens, Folders etc)
- Low-Priority orders were sometimes shipped via Express Air, increasing costs unnecessarily.
- Shipping modes did not always align with order priority potentially wasting money and risking customer dissatisfaction on urgent/High Priority Orders.

### SQL Queries
**Case Scenario 1**

1. Which product category had the highest sales?
``` SQL
select Product_Category, SUM(sales) as HighestSales
from KMS
group by Product_Category
order by HighestSales desc
```

2. What are the top 3 and bottom 3 regions in terms of sales?
```SQL
---Top 3 regions in terms of Sales---
select top 3
  Region, SUM(Sales) as TotalSales
from KMS
group by Region
order by TotalSales desc

----Bottom 3 Regions in terms of Sales----
select top 3
  Region, SUM(Sales) as TotalSales
from KMS
group by Region
order by TotalSales asc
```

3. What were the total sales of Appliances in Ontario?
```SQL
----Total Sales of Appliances in Ontario---
Select SUM(Sales) as TotalApplianceSale
from KMS
Where Province = 'ontario'
and product_sub_category = 'Appliances'
```

4. Advise on how Management can increase Revenue from Bottom 10 customers
```SQL
select top 10
  customer_name, Customer_Segment, sum(sales) as TotalSales
  from KMS
 group by Customer_Name, Customer_Segment
 order by TotalSales asc
```

5. KMS incurred the most shipping cost using which shipping method?
```SQL
select Ship_mode, SUM(shipping_cost) as TotalShippingCost
from KMS
group by Ship_mode
order by TotalShippingCost desc
```

**Case Scenario 2**

6. Who are the most valuable customers, and what products or services do they typically purchase?
```SQL
----Most valuable customers---
select top 5
    customer_name, SUM(sales) as Totalsales
	from KMS 
	group by Customer_Name
	order by Totalsales desc

----What products do they buy---
select customer_name, product_category,COUNT(*) as PurchaseCount
from KMS
where Customer_Name in ('Emily Phan','Deborah Brumfield','Roy Skaria','Sylvia Foulston','Grant Carrol')
group by Customer_Name, product_category
```

7. Which small business customer had the highest sales?
```SQL
select top 1
  customer_name ,SUM(sales) as TotalSales
from KMS
where customer_segment = 'Small Business'
group by customer_name
order by TotalSales desc
```

8. Which corporate customer placed the most number of orders in 2009-2012?
```SQL
select top 1 customer_name, COUNT(order_id) as OrderCount
 from KMS
 where Customer_Segment = 'Corporate'
 and order_date between '2009-01-01' and '2012-12-31'
 group by customer_name
 order by COUNT(order_id) DESC
```

9. Which consumer customer was the most profitable one?
```SQL
select top 1
  customer_name, SUM(profit) as TotalProfit
  from KMS
  where customer_segment = 'Consumer'
  group by customer_name
  order by TotalProfit desc
```

10. Which customer returned items, and what segment do they belong to?
```SQL
select KMS.customer_name, KMS.customer_segment, order_status.order_id, Order_status.status
 from KMS
 join order_status
 on order_status.order_id = KMS.order_id
```

11. If the delivery truck is the most economical but the slowest shipping method and Express Air is the fastest but the most expensive one, do you think the company approriately spent shipping costs based on Order Priority?
```SQL
 select order_priority, ship_mode, COUNT(Order_ID ) as OrderCount,
 Sum (shipping_cost) as TotalShippingCost,SUM(sales) - SUM(profit) as EstimatedShippingCost,
 AVG(datediff(day,[order_date],[ship_date]))as AvgShipDays
 from KMS
 group by order_priority, ship_mode
 order by TotalShippingCost desc
```









