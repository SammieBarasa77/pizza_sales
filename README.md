# Data Project: The Urban Pizza Sales Analysis 

![logo](https://github.com/SammieBarasa77/walmart_sales/blob/main/assets/images/cover_final.png)

## Table of Contents 


1. [Introduction](#introduction)
   - [Project Overview](#project-overview)
   - [Key Objectives](#key-objectives)

2. [Database Setup and Queries](#database-setup-and-queries)
   - [Using the Database](#using-the-database)
   - [Key Performance Indicators (KPIs)](#key-performance-indicators-kpis)
     - [Total Revenue](#total-revenue)
     - [Average Order Value](#average-order-value)
     - [Total Pizzas Sold](#total-pizzas-sold)
     - [Total Orders](#total-orders)
     - [Average Pizzas Per Order](#average-pizzas-per-order)
   - [Trends Analysis](#trends-analysis)
     - [Average Daily Orders](#average-daily-orders)
     - [Monthly Orders Trends](#monthly-orders-trends)
   - [Sales by Category and Size](#sales-by-category-and-size)
     - [Percentage of Sales by Pizza Category](#percentage-of-sales-by-pizza-category)
     - [Percentage of Sales by Pizza Size](#percentage-of-sales-by-pizza-size)
   - [Top Sellers Analysis](#top-sellers-analysis)

3. [Advanced Sales Analysis](#advanced-sales-analysis)
   - [Customer Demographics and Behavior](#customer-demographics-and-behavior)
     - [Age Groups and Locations](#age-groups-and-locations)
     - [Order Frequency](#order-frequency)
     - [Purchase Channels](#purchase-channels)
   - [Profitability Analysis](#profitability-analysis)
     - [Cost Analysis](#cost-analysis)
     - [High-Cost, Low-Profit Items](#high-cost-low-profit-items)
   - [Time-Sensitive Insights](#time-sensitive-insights)
     - [Hourly Sales Patterns](#hourly-sales-patterns)
     - [Seasonal Menu Adjustments](#seasonal-menu-adjustments)
   - [Customer Retention Metrics](#customer-retention-metrics)
     - [Churn Rate](#churn-rate)
     - [Loyalty Effectiveness](#loyalty-effectiveness)
   - [Market Competition](#market-competition)
     - [Competitor Analysis](#competitor-analysis)
     - [Unique Selling Points](#unique-selling-points)
   - [Delivery and Fulfillment](#delivery-and-fulfillment)
     - [Average Delivery Times](#average-delivery-times)
     - [Order Fulfillment Errors](#order-fulfillment-errors)
   - [Online Presence and Reviews](#online-presence-and-reviews)
     - [Review Analysis](#review-analysis)
     - [Social Media Engagement](#social-media-engagement)
   - [Upselling and Cross-Selling Effectiveness](#upselling-and-cross-selling-effectiveness)
     - [Conversion Rates](#conversion-rates)
     - [Revenue Contribution](#revenue-contribution)

4. [Insights and Recommendations](#insights-and-recommendations)
   - [Recommendations](#recommendations)

5. [Appendix](#appendix)
   - [Data Source References](#data-source-references)

6. [Author](#author)
   - [Samuel Almario Barasa](#samuel-almario-barasa)
   - [Contact Information](#contact-information)

## 1. Introduction

### Project Overview 

The Urban Pizzas Sales Analysis project leverages SQL-based techniques to analyze sales data, providing actionable insights to enhance efficiency, optimize strategies, and improve customer satisfaction. The analysis is divided into two parts:

Core Metrics and Trends Analysis: Focuses on foundational metrics like total revenue, average order value, and order trends, as well as the performance of pizza categories and sizes.

Advanced Sales Insights: Explores customer behaviour, profitability, retention, market competition, delivery performance, and upselling effectiveness, offering a deeper understanding of the business

### Key Objectives

*To identify sales patterns and key performance indicators (KPIs) that reflect business health.*
*To understand customer preferences and purchasing behaviours for better-targeted marketing.*
*To analyze profitability and identify high-cost, low-profit items.*
*To uncover opportunities for improving customer retention and optimizing operations.*
*To provide actionable recommendations based on data-driven insights.*

## 2. Database Setup and Queries
### Using the Database

```sql
USE [Pizza DB];
SELECT * FROM [Pizza DB].[dbo].[Pizza_Data];
```

### Key Performance Indicators (KPIs):

#### Total Revenue
```sql
SELECT SUM(total_price) AS Total_Revenue FROM Pizza_Data;
```
Output

![Total revenue](https://github.com/SammieBarasa77/pizza_sales/blob/main/assets/images/total_rev.png)

#### Average Order Value (Revenue/Orders)
```sql
SELECT SUM(total_price) / COUNT(DISTINCT order_id) AS Average_order_value FROM Pizza_Data;

```
![Average Order Value](https://github.com/SammieBarasa77/pizza_sales/blob/main/assets/images/avg_order_value.png)

#### Total Pizzas Sold
```sql
SELECT SUM(quantity) AS Total_Pizzas_sold FROM Pizza_Data;
```
![Total Pizzas](https://github.com/SammieBarasa77/pizza_sales/blob/main/assets/images/total_pizzas.png)

#### Total Orders
```sql
SELECT COUNT(DISTINCT order_id) AS Total_Orders FROM Pizza_Data;
```
![Total Orders](https://github.com/SammieBarasa77/pizza_sales/blob/main/assets/images/total_orders.png)

#### Average Pizzas Per Order
```sql
SELECT 
CAST(CAST(SUM(quantity) AS DECIMAL(10, 2)) / 
CAST(COUNT(DISTINCT order_id) AS DECIMAL(10, 2)) AS DECIMAL(10, 2)) as Average_Pizza_Per_order 
FROM Pizza_Data;
```
![Average Pizzas Per Order](https://github.com/SammieBarasa77/pizza_sales/blob/main/assets/images/avg_pizza_orders.png)

### Trends Analysis

#### Average Daily Orders
```sql
SELECT DATENAME(DW, order_date) AS Order_Day, 
COUNT(DISTINCT order_id) AS Average_Daily_Orders 
FROM Pizza_Data 
GROUP BY DATENAME(DW, order_date);
```
![Average Daily Orders](https://github.com/SammieBarasa77/pizza_sales/blob/main/assets/images/avg_daily_trends.png)

#### Monthly Orders Trends

```sql
SELECT 
DATENAME(MONTH, order_date) AS Month_Name, 
COUNT(DISTINCT order_id) AS Total_Monthly_Orders 
FROM Pizza_Data 
GROUP BY DATENAME(MONTH, order_date) 
ORDER BY Total_Monthly_Orders DESC;
```
![Monthly Orders Trends](https://github.com/SammieBarasa77/pizza_sales/blob/main/assets/images/monthly_trends.png)

### Sales by Categories and Sizes

#### Percentage of Sales by Pizza Category (for Month 1)
```sql
SELECT pizza_category, 
SUM(total_price) AS Total_sales, 
SUM(unit_price) * 100 / 
(SELECT SUM(total_price) FROM Pizza_Data WHERE MONTH(order_date) = 1) AS Percentage_of_sales 
FROM Pizza_Data 
WHERE MONTH(order_date) = 1 
GROUP BY pizza_category 
ORDER BY Percentage_of_sales DESC;
```
![ Month 1](https://github.com/SammieBarasa77/pizza_sales/blob/main/assets/images/percent_sales_by_category.png)

#### Percentage of Sales by Pizza Size (Quarter 1):
```sql
SELECT pizza_size, 
CAST(SUM(total_price) AS DECIMAL(10, 2)) AS Total_sales, 
CAST(SUM(unit_price) * 100 / 
(SELECT SUM(total_price) 
FROM Pizza_Data WHERE DATEPART(QUARTER, order_date) = 1) AS DECIMAL(10, 2)) AS Percentage_of_sales 
FROM Pizza_Data 
WHERE DATEPART(QUARTER, order_date) = 1 
GROUP BY pizza_size 
ORDER BY Percentage_of_sales DESC;
```
![Quarter 1](https://github.com/SammieBarasa77/pizza_sales/blob/main/assets/images/percent_sales_by_size.png)

### Top Sellers by Revenue, Orders, and Quantity
```sql
SELECT TOP 5 
pizza_name, 
SUM(total_price) AS Total_Revenue, 
COUNT(DISTINCT order_id) AS Total_Orders, 
SUM(quantity) AS Total_Pizzas 
FROM Pizza_Data 
GROUP BY pizza_name 
ORDER BY Total_Revenue DESC;
```
![Top Sellers](https://github.com/SammieBarasa77/pizza_sales/blob/main/assets/images/total_sellers_rev_orders_.png)


*The following part of the project is an extended section of the project I did at a later stage of the project using a different SQL database, therefore I didn't include the outputs of the queries*
*For a thriving busing business entity, data analysis plays a very vital role. There are still many areas that require analysis and some of them include the following;*

## 3. Advanced Sales Analysis

### Customer Demographics and Behavior

#### Age Groups and Locations
```sql
-- Age Groups and Locations
SELECT 
    CASE 
        WHEN age BETWEEN 18 AND 25 THEN '18-25'
        WHEN age BETWEEN 26 AND 35 THEN '26-35'
        WHEN age BETWEEN 36 AND 50 THEN '36-50'
        ELSE '50+' 
    END AS Age_Group, 
    location, 
    COUNT(*) AS Order_Count
FROM Customer_Data
JOIN Orders ON Customer_Data.customer_id = Orders.customer_id
GROUP BY 
    CASE 
        WHEN age BETWEEN 18 AND 25 THEN '18-25'
        WHEN age BETWEEN 26 AND 35 THEN '26-35'
        WHEN age BETWEEN 36 AND 50 THEN '36-50'
        ELSE '50+' 
    END, location;
```

#### Order Frequency
```sql
SELECT 
    customer_id, 
    COUNT(order_id) AS Order_Count,
    CASE 
        WHEN COUNT(order_id) > 5 THEN 'Frequent'
        ELSE 'Infrequent'
    END AS Customer_Type
FROM Orders
GROUP BY customer_id;
```

#### Purchase Channels
```sql
SELECT 
    purchase_channel, 
    COUNT(order_id) AS Total_Orders
FROM Orders
GROUP BY purchase_channel;
```

### Profitability Analysis

#### Cost Analysis
```sql
SELECT 
    pizza_category, 
    pizza_size, 
    SUM(total_price) AS Total_Revenue, 
    SUM(ingredient_cost) AS Total_Cost, 
    (SUM(total_price) - SUM(ingredient_cost)) AS Profit
FROM Pizza_Data
GROUP BY pizza_category, pizza_size;
```

#### High-Cost, Low-Profit Items
```sql
SELECT 
    pizza_name, 
    SUM(total_price) AS Total_Revenue, 
    SUM(ingredient_cost) AS Total_Cost, 
    (SUM(total_price) - SUM(ingredient_cost)) AS Profit
FROM Pizza_Data
GROUP BY pizza_name
HAVING (SUM(total_price) - SUM(ingredient_cost)) < 0.1 * SUM(total_price); -- Low profit margin threshold
```

### Time-Sensitive Insights

#### Hourly Sales Patterns
```sql
SELECT 
    DATEPART(HOUR, order_time) AS Hour, 
    COUNT(order_id) AS Total_Orders
FROM Orders
GROUP BY DATEPART(HOUR, order_time)
ORDER BY Hour;
```

#### Seasonal Menu Adjustments
```sql
SELECT 
    pizza_category, 
    MONTH(order_date) AS Month, 
    SUM(quantity) AS Total_Sales
FROM Pizza_Data
GROUP BY pizza_category, MONTH(order_date)
ORDER BY Total_Sales DESC;
```

### Customer Retention Metrics

#### Churn Rate
```sql
SELECT 
    customer_id, 
    MAX(order_date) AS Last_Order_Date, 
    CASE 
        WHEN DATEDIFF(DAY, MAX(order_date), GETDATE()) > 90 THEN 'Churned'
        ELSE 'Active'
    END AS Customer_Status
FROM Orders
GROUP BY customer_id;
```

#### Loyalty Effectiveness
```sql
SELECT 
    customer_id, 
    COUNT(order_id) AS Total_Orders, 
    SUM(loyalty_points_used) AS Points_Redeemed
FROM Orders
GROUP BY customer_id
HAVING SUM(loyalty_points_used) > 0;
```

### Market Competition

#### Competitor Analysis

Here, there would be a need to collect competitor data in a table and then be analyzed:
```sql
SELECT 
    pizza_category, 
    AVG(price) AS Avg_Competitor_Price, 
    MAX(discount) AS Max_Competitor_Discount
FROM Competitor_Data
GROUP BY pizza_category;
```

#### Unique Selling Points 
```sql
SELECT 
    pizza_category, 
    COUNT(DISTINCT topping) AS Unique_Toppings
FROM Pizza_Data
GROUP BY pizza_category;
```

### Delivery and Fulfillment

#### Average Delivery Times
```sql
SELECT 
    AVG(DATEDIFF(MINUTE, order_time, delivery_time)) AS Avg_Delivery_Time
FROM Orders
WHERE delivery_time IS NOT NULL;
```

#### Order Fulfillment Errors
```sql
SELECT 
    COUNT(*) AS Total_Errors, 
    error_type
FROM Fulfillment_Errors
GROUP BY error_type;
```

### Online Presence and Reviews
 
#### Review Analysis
```sql
SELECT 
    rating, 
    COUNT(*) AS Review_Count, 
    AVG(rating) AS Avg_Rating
FROM Reviews
GROUP BY rating;
```

#### Social Media Engagement
```sql
SELECT 
    post_id, 
    COUNT(likes) AS Total_Likes, 
    COUNT(comments) AS Total_Comments, 
    COUNT(shares) AS Total_Shares
FROM Social_Media_Posts
GROUP BY post_id;
```

#### Upselling and Cross-Selling Effectiveness

#### Conversion Rates
```sql
SELECT 
    upsell_item, 
    COUNT(order_id) AS Orders_With_Upsell, 
    COUNT(*) * 100.0 / (SELECT COUNT(order_id) FROM Orders) AS Conversion_Rate
FROM Upsell_Orders
GROUP BY upsell_item;
```

#### Revenue Contribution
```sql
SELECT 
    upsell_item, 
    SUM(upsell_price) AS Total_Upsell_Revenue, 
    SUM(upsell_price) * 100.0 / (SELECT SUM(total_price) FROM Orders) AS Revenue_Percentage
FROM Upsell_Orders
GROUP BY upsell_item;
```

**Dashboard**
Page 1
![Dashboard](https://github.com/SammieBarasa77/pizza_sales/blob/main/assets/images/dash_1.png)
Page 2
![Dashboard](https://github.com/SammieBarasa77/pizza_sales/blob/main/assets/images/dash_2.png)

## 4. Insights and Recommendations

Average Pizza Orders per Order: *The average number of pizzas per order is 2.32, indicating typical customer order behaviour.*

Average Daily Order Trends: *The average daily orders for each day of the week show that Saturday has the highest average at 3,158 orders, followed by Wednesday at 3,024 and Monday at 2,794, suggesting varying demand patterns throughout the week.*

Monthly Order Trends: *The busiest months for orders are July (1,935 orders), May (1,853), and January (1,845), highlighting seasonality in customer demand.*

Total Revenue, Orders, and Pizzas Sold: *The overall totals are revenue of $817,860.05, total orders of 21,350, and total pizzas sold of 49,574, providing a sense of the business's scale and performance.*

Average Order Value: *The average order value is $38.37, representing the average amount spent per order.*

Sales by Pizza Category: *Sales breakdown by category shows Classic (26.86%), Supreme (25.21%), Veggie (17.06%), and Chicken (22.70%), indicating customer preferences.*

Sales by Pizza Size: *Sales by size reveal that Large pizzas account for 45.48%, Medium at 29.31%, Small at 21.71%, and XL/XXL at 1.74%, which can inform product mix and inventory decisions.*

### Recommendations

Based on the analysis, here are several recommendations to potentially increase sales:

Promote Peak Days: *Since Saturday has the highest average orders, consider implementing special promotions or discounts on this day to further boost sales. Additionally, targeting Wednesday and Monday with similar promotions could help increase orders.*

Seasonal Promotions: *Given the seasonal trends, particularly the high sales in July, consider launching summer-themed promotions or limited-time offers during peak months to attract more customers.*

Enhance Average Order Value: *With an average order value of $38.37, strategies such as upselling (e.g., suggesting sides or drinks) or bundling pizzas with complementary items could encourage customers to spend more per order.*

Focus on Popular Categories: *Since Classic and Supreme pizzas are the top-selling categories, consider expanding the menu with variations of these pizzas or introducing new flavours that align with customer preferences.*

Size Promotions: *With Large pizzas accounting for most sales, consider offering discounts on larger sizes or family packs to encourage larger orders, especially for group gatherings.*

Loyalty Programs: *Implementing a loyalty program could encourage repeat customers to order more frequently and increase overall sales.*

Targeted Marketing: *Utilize customer data to create targeted marketing campaigns that focus on specific demographics or customer preferences, enhancing engagement and driving sales.*

## 5. Appendix
### Data Source References
Feel free to download the dataset I used for this very analysis from here and do the analysis on your own if you would like to: 'https://github.com/SammieBarasa77/pizza_sales/blob/main/assets/docs/Pizza_Data.csv'

## 6. Author

### Samuel Almario Barasa
![LinkedIn](https://www.linkedin.com/in/samuel-barasa-b01488211/)

![Portfolio](https://sammiebarasa77.github.io/sammiebarasa-web.github.io/)


