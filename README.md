# Customer-Segmentation.LITA

### Project Overview: Customer Segmentation for a Subscription Service
### Objective:
The primary goal of this project is to analyze customer data for a subscription service to identify segments, subscription patterns, and key trends related to cancellations and renewals. This analysis will provide insights into customer behavior, subscription preferences, and factors driving churn. The final deliverable will be a Power BI dashboard that visualizes these insights, allowing stakeholders to explore the data interactively.

### Summary:
This project involves:
- Analyzing customer data to uncover patterns in subscription plans, duration, and customer retention.
- Tracking key trends such as cancellations within the first six months, popular subscription types, and overall customer activity.
- Delivering actionable insights through an interactive Power BI dashboard.

### Tools and Techniques:
- Excel: Initial data analysis and summary reporting through PivotTables.
- SQL: Querying the dataset to extract insights from the subscription database.
- Power BI: Visualizing the data and creating an interactive dashboard.

### Pivot Table

![Regional Distribution](https://github.com/user-attachments/assets/597ed73a-dacf-4c6c-87a2-fbece9bdfbaf)

![Popular sub](https://github.com/user-attachments/assets/813d52af-80cc-41a8-aa4f-c1b0856c362d)

![Monthly Revenue](https://github.com/user-attachments/assets/c085ab9c-59cc-45fd-89c9-b363350845f2)

![Average sub](https://github.com/user-attachments/assets/d362c710-4608-48d8-bf5c-9125264781bb)

### Codes Used

``` SQL
--retrieve the total number of customers from each region--

SELECT
      COUNT(CustomerID),
      Region
FROM
      customerdata
GROUP BY
      Region;

--find the most popular subscription type by the number of customers--
SELECT
     COUNT(CustomerID) AS Customer_count,
     SubscriptionType
FROM
     customerdata
GROUP BY
     SubscriptionType
ORDER BY
     Customer_count DESC
LIMIT 1;

--find customers who canceled their subscription within 6 months.--

SELECT 
    CustomerID, 
    SubscriptionStart, 
    SubscriptionEnd,
    TIMESTAMPDIFF(day, SubscriptionStart, SubscriptionEnd) AS Duration_in_days
FROM 
    customerdata
WHERE 
   SubscriptionEnd IS NOT NULL
    AND TIMESTAMPDIFF(day, SubscriptionStart, SubscriptionEnd) <= 180;

--calculate the average subscription duration for all customers.--

SELECT 
	CustomerID,
    AVG(DATEDIFF(
        STR_TO_DATE(SubscriptionEnd, '%c/%e/%Y'), 
        STR_TO_DATE(SubscriptionStart, '%c/%e/%Y')
    )) AS average_subscription_duration
FROM 
    customerdata
WHERE
    SubscriptionStart IS NOT NULL
    AND SubscriptionEnd IS NOT NULL
    GROUP BY CustomerID;

--find customers with subscriptions longer than 12 months.--


--calculate total revenue by subscription type.--

SELECT 
    SubscriptionType, 
    SUM(Revenue) AS Total_revenue
FROM 
    customerdata
GROUP BY 
    SubscriptionType;


--find the top 3 regions by subscription cancellations.--

SELECT
    Region,
    COUNT(Canceled) AS Canceled_Subscription
FROM
    customerdata
WHERE
    Canceled LIKE '%TRUE%'
GROUP BY
    Region
ORDER BY
    Canceled_Subscription  DESC
    LIMIT 3;



--find the total number of active and canceled subscriptions.--

SELECT 
    COUNT(CASE WHEN Canceled = 'TRUE' THEN 1 END) AS Active_subscriptions,
    COUNT(CASE WHEN Canceled = 'FALSE' THEN 1 END) AS Canceled_subscriptions
FROM customerdata;

```
