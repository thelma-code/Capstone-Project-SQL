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
select top 1
Product_Category, SUM(sales) as HighestSales
from KMS
group by Product_Category
order by HighestSales desc
```
[Technology,5984248.30
Uploading R1New.csv…]()

2. What are the top 3 and bottom 3 regions in terms of sales?
```SQL
---Top 3 regions in terms of Sales---
select top 3
  Region, SUM(Sales) as TotalSales
from KMS
group by Region
order by TotalSales desc
```

[West,3597549.33
Ontario,3063212.55
Prarie,2837304.59
ploading R2a.csv…]()

```SQL
----Bottom 3 Regions in terms of Sales----
select top 3
  Region, SUM(Sales) as TotalSales
from KMS
group by Region
order by TotalSales asc
```
[Nunavut,116376.47
Northwest Territories,800847.34
Yukon,975867.39
g R2b.csv…]()

3. What were the total sales of Appliances in Ontario?
```SQL
----Total Sales of Appliances in Ontario---
Select SUM(Sales) as TotalApplianceSale
from KMS
Where Province = 'ontario'
and product_sub_category = 'Appliances'
```
[202346.84
oading R3.csv…]()

4. Advise on how Management can increase Revenue from Bottom 10 customers
```SQL
select top 10
  customer_name, Customer_Segment, sum(sales) as TotalSales
  from KMS
 group by Customer_Name, Customer_Segment
 order by TotalSales asc
```
[Jeremy Farry,Small Business,85.72
Michelle Ellison,Home Office,93.22
Natalie DeCherney,Consumer,125.90
Nicole Fjeld,Small Business,153.03
Katrina Edelman,Corporate,180.76
Bart Pistole,Consumer,195.83
Dorothy Dickinson,Corporate,198.08
Christine Kargatis,Small Business,293.22
Eric Murdock,Home Office,343.33
Chris McAfee,Consumer,350.18
ading R4.csv…]()

----Since the bottom 10 customers are mostly from the Consumer Segment(Home Offices/Small Businesses) and they primarily buy Office Supplies(likely low margin items like Paper, Pens, Folders etc), management could introduce bundled value packs eg Office Starter Kits which might include Office Supplies + small tech items(USB, drives, etc) to encourage bulk buying. Management could also introduce Volume Discounts eg "Buy more, Save More" discounts since small businesses and home users may be price sensitive

5. KMS incurred the most shipping cost using which shipping method?
```SQL
select Ship_mode, SUM(shipping_cost) as TotalShippingCost
from KMS
group by Ship_mode
order by TotalShippingCost desc
```
[Delivery Truck,51971.94
Regular Air,48008.19
Express Air,7850.91
ding R5.csv…]()

**Case Scenario 2**

6. Who are the most valuable customers, and what products or services do they typically purchase?
```SQL
----Most valuable customers---
select top 5
    customer_name, SUM(sales) as Totalsales
	from KMS 
	group by Customer_Name
	order by Totalsales desc
```
[Emily Phan,117124.43
Deborah Brumfield,97433.14
Roy Skaria,92542.16
Sylvia Foulston,88875.76
Grant Carroll,88417.00
oading R6.csv…]()

```SQL
----What products do they buy---
select customer_name, product_category,COUNT(*) as PurchaseCount
from KMS
where Customer_Name in ('Emily Phan','Deborah Brumfield','Roy Skaria','Sylvia Foulston','Grant Carrol')
group by Customer_Name, product_category
```
[Deborah Brumfield,Furniture,4
Emily Phan,Furniture,1
Roy Skaria,Furniture,8
Sylvia Foulston,Furniture,10
Deborah Brumfield,Office Supplies,8
Emily Phan,Office Supplies,5
Roy Skaria,Office Supplies,12
Sylvia Foulston,Office Supplies,9
Deborah Brumfield,Technology,8
Emily Phan,Technology,4
Roy Skaria,Technology,6
Sylvia Foulston,Technology,5
ding R6b.csv…]()

7. Which small business customer had the highest sales?
```SQL
select top 1
  customer_name ,SUM(sales) as TotalSales
from KMS
where customer_segment = 'Small Business'
group by customer_name
order by TotalSales desc
```
[Dennis Kane,75967.59
ding R7.csv…]()

8. Which corporate customer placed the most number of orders in 2009-2012?
```SQL
select top 1 customer_name, COUNT(order_id) as OrderCount
 from KMS
 where Customer_Segment = 'Corporate'
 and order_date between '2009-01-01' and '2012-12-31'
 group by customer_name
 order by COUNT(order_id) DESC
```
[Adam Hart,27
ing R8.csv…]()

9. Which consumer customer was the most profitable one?
```SQL
select top 1
  customer_name, SUM(profit) as TotalProfit
  from KMS
  where customer_segment = 'Consumer'
  group by customer_name
  order by TotalProfit desc
```
[Emily Phan,34005.44
ing R9.csv…]()

10. Which customer returned items, and what segment do they belong to?
```SQL
select KMS.customer_name, KMS.customer_segment, order_status.order_id, Order_status.status
 from KMS
 join order_status
 on order_status.order_id = KMS.order_id
```
[Dorothy Badders,Home Office,678,Returned
Grant Carroll,Corporate,9927,Returned
Grant Carroll,Corporate,9927,Returned
Grant Carroll,Corporate,9927,Returned
Edward Hooks,Consumer,11911,Returned
Michelle Lonsdale,Home Office,12096,Returned
Michelle Lonsdale,Home Office,12096,Returned
Carlos Soltero,Small Business,12704,Returned
Carlos Soltero,Small Business,12704,Returned
Dorothy Badders,Home Office,15106,Returned
loading R10New.csv…]()

11. If the delivery truck is the most economical but the slowest shipping method and Express Air is the fastest but the most expensive one, do you think the company approriately spent shipping costs based on Order Priority?
```SQL
 select order_priority, ship_mode, COUNT(Order_ID ) as OrderCount,
 Sum (shipping_cost) as TotalShippingCost,SUM(sales) - SUM(profit) as EstimatedShippingCost,
 AVG(datediff(day,[order_date],[ship_date]))as AvgShipDays
 from KMS
 group by order_priority, ship_mode
 order by TotalShippingCost desc
```
[High,Delivery Truck,248,11206.88,1338507.99,1
Low,Delivery Truck,250,11131.61,1330425.13,3
Critical,Delivery Truck,228,10783.82,1221663.12,1
Low,Regular Air,1280,10263.62,1371489.21,4
High,Regular Air,1308,10005.01,1315867.53,1
Not Specified,Regular Air,1277,9734.08,1282456.87,1
Medium,Delivery Truck,205,9461.62,976613.94,1
Medium,Regular Air,1225,9418.72,1304914.51,1
Not Specified,Delivery Truck,215,9388.01,1086999.65,1
Critical,Regular Air,1180,8586.76,1123422.35,1
Critical,Express Air,200,1742.10,198004.42,1
Medium,Express Air,201,1633.59,247361.93,1
Low,Express Air,190,1551.63,191295.12,4
Not Specified,Express Air,180,1470.06,194416.98,1
High,Express Air,212,1453.53,208773.18,1
ng R11.csv…]()

--- based on the count of orders and shipping cost by order priority: only 12.20% of High & Criritcal Priority Orders were shipped using Express Air, while 14.09% of High & Critical Priority orders were shipped with the Delivery Truck, 73.69% where shipped using Regular Air.
 ---13.58% of Low & Medium Priority orders were shipped with the Delivery Truck, 11.67% of Low & Medium Priority orders were shipped using ExpressAir, and 74.75% of Low & Medium Priority Orders were shipped with Regular Air.

 **This suggests that KMS did not always align Shipping Modes with Order Priority,potentially wasting money and risking customer dissatisfaction on urgent/High Priority Orders.**

### Recommendations
- Bundle low value items to increase order Value
- Introduce loyalty incentives for repeat small business customers
- Align shipping method strictly with order priority








