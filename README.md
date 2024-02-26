# E Commerce Store

## Overview
Global Superstore is an online retailer based in New York, boasting a broad product catalog and aiming to be a one-stop-shop for its customers. Global The superstore’s
clientele,hailing from 147 different countries, can browse through an endless offering with more than 10,000 products. This large selection comprises three main categories: office supplies (e.g., staples), furniture (e.g., chairs), and technology (e.g., smartphones).The objective of this project is to help Global Superstore analyze and draw out meaningful insight from the Superstore dataset which would aid management in making informed decisions to maximise sales to improve performance and profitability.

## Data Source
The dataset used for this analysis is in csv format  [download here](https://docs.google.com/spreadsheets/d/1nxESpFzWjlGDMGDVLH69xmDzIl9l6OEq/edit#gid=633280281)

## Tools Used for Analysis
-  SQL Server for  Data Cleaning and Exploratory Data Analysis
-  Power BI for Data Visualization and Reports

## Data Cleaning 
- Dropped Column postal code having null
- Data Formatting

## Exploratory Data Analysis
EDA involved exploring the data to answer key questions;
-  What are the three countries that generated the highest total profit for Global Superstore in 2014? For each of these three countries, find the three products with the highest total 
   profit. Specifically, what are the products’ names and the total profit for each product?
-  Identify the 3 subcategories with the highest average shipping cost in the United States.
-  Assess Nigeria’s profitability (i.e., total profit) for 2014. How does it compare to other African
   countries?
- What factors might be responsible for Nigeria’s poor performance? 
- Identify the product subcategory that is the least profitable in Southeast Asia.
- Is there a specific country in Southeast Asia where Global Superstore should stop offering the
   subcategory identified in your analysis?
- Which city is the least profitable (in terms of average profit) in the United States? For this analysis,
  discard the cities with less than 10 Orders.Why is this city’s average profit so low?
- Which product subcategory has the highest average profit in Australia?
- Who are the most valuable customers and what do they purchase?

## Data Modelling
```sql
SELECT * FROM GlobalSuperstore

 --Data cleaning
ALTER TABLE Globalsuperstore
DROP COLUMN Postal_code

--TOP 3 countries with the highest total profit in 2014
 SELECT TOP 3 country,SUM(profit) AS total_profit
 FROM GlobalSuperstore
 WHERE Order_Date BETWEEN 
      '2014-01-01' AND '2014-12-31'
 GROUP BY country
 ORDER BY 2 DESC

 --Products with the highest sales in the United States
 SELECT TOP 3 Product_Name,SUM(profit) AS US_totalprofit
 FROM GlobalSuperstore
 WHERE country ='United States'AND
     Order_Date BETWEEN  '2014-01-01' AND '2014-12-31'
 GROUP BY Product_Name
 ORDER BY 2 DESC
 
 --Products with the highest sales in India
 SELECT TOP 3 Product_Name,SUM(profit) AS India_totalprofit
 FROM GlobalSuperstore
 WHERE country ='India'AND
       Order_Date BETWEEN  '2014-01-01' AND '2014-12-31'
 GROUP BY  Product_Name
 ORDER BY 2 DESC

 --Products with the highest sales in China
 SELECT TOP 3 Product_Name,SUM(profit) AS China_totalprofit
 FROM GlobalSuperstore
 WHERE country ='China' AND
      Order_Date BETWEEN  '2014-01-01' AND '2014-12-31'
 GROUP BY Product_Name
 ORDER BY 2 DESC
 
 -- Subcategories with the highest average shipping cost in the US
SELECT TOP 3 Sub_category,AVG(shipping_cost) AS US_Avgshippingcost
FROM GlobalSuperstore
WHERE Country ='United States'
GROUP BY Sub_Category
ORDER BY 2 DESC

--Analyzing Nigeria's profitability in 2014,and compare to other Africa countries
CREATE VIEW Africa_Profitablity2014 AS
SELECT TOP 40 Country,AVG(Discount) AS Avg_Discount
                     ,SUM(Quantity) AS Total_QuantityOrder
                     ,SUM(Profit) AS total_profit
					 ,AVG(Shipping_Cost) AS Avg_Shipmentcost 
FROM Globalsuperstore
WHERE region ='Africa' AND 
    Order_date BETWEEN '2014-01-01' AND '2014-12-31'
GROUP BY Country
ORDER BY 4 ASC

SELECT * FROM Africa_Profitablity2014 

--Identifying the least profitably product subcategory in Southeast Asia
SELECT Sub_category
      ,SUM(Profit) AS Least_profit         
FROM GlobalSuperstore
WHERE region ='Southeast Asia' 
GROUP BY Sub_category
ORDER BY 2 ASC

--City with the least average profit in the United States
--and why is the average profit low
SELECT City,AVG(Profit) AS Avg_profit, 
           COUNT(DISTINCT Order_ID) AS Order_count
 FROM GlobalSuperstore
 WHERE Country LIKE '%states' 
GROUP BY City
HAVING COUNT(DISTINCT Order_ID) >= '10'
ORDER BY 2 ASC

--Product Subcategory with the highest average profit in Austrialia
SELECT Sub_Category,AVG(Profit) AS Avg_profit
      FROM GlobalSuperstore
	  WHERE Region = 'Oceania'
GROUP BY Sub_Category
ORDER BY 2 DESC

--The most valuable customers and what they purchase
CREATE VIEW
Valuable_Customers AS
SELECT TOP 10 Customer_name,Category
     ,SUM(Sales) AS TotalAmountSpent
     ,SUM(Profit) AS Margin
	 ,SUM(quantity) AS Orders
      FROM GlobalSuperstore
GROUP BY Customer_name,Category
ORDER BY 4 DESC
```

