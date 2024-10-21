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
![Active customers](https://github.com/user-attachments/assets/13d83a43-14f3-475d-9275-02479308f925)

![Canceled subscription](https://github.com/user-attachments/assets/475a1e47-95a9-4f1a-95e4-9ec64aa0beef)

![Popular sub](https://github.com/user-attachments/assets/a056b4e9-86d8-40ad-8653-5b841b3425cc)

![Total revenue by region](https://github.com/user-attachments/assets/516dc4bf-c28f-4bf9-bf2c-ad21a5004ad6)

![Monthly revenue](https://github.com/user-attachments/assets/f633fe5e-1cba-4b7f-a390-cd79c49b563c)

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
    CustomerName, 
    SubscriptionStart, 
    SubscriptionEnd,
    DATEDIFF(STR_TO_DATE(SubscriptionEnd, '%c/%e/%Y'), 
             STR_TO_DATE(SubscriptionStart, '%c/%e/%Y')) AS Subscription_duration_days
FROM 
    customerdata
WHERE 
    Canceled = 'TRUE'  -- Only include customers who canceled--
    AND DATEDIFF(STR_TO_DATE(SubscriptionEnd, '%c/%e/%Y'), 
                 STR_TO_DATE(SubscriptionStart, '%c/%e/%Y')) <= 180  -- Within 6 months--
    AND SubscriptionStart IS NOT NULL
    AND SubscriptionEnd IS NOT NULL;



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
SELECT 
    DISTINCT CustomerID, 
    CustomerName, 
    DATEDIFF(STR_TO_DATE(SubscriptionEnd, '%c/%e/%Y'), 
            STR_TO_DATE(SubscriptionStart, '%c/%e/%Y')) AS subscription_duration_days
FROM 
    customerdata
WHERE 
    DATEDIFF(STR_TO_DATE(SubscriptionEnd, '%c/%e/%Y'), 
            STR_TO_DATE(SubscriptionStart, '%c/%e/%Y')) > 365
    AND SubscriptionStart IS NOT NULL
    AND SubscriptionEnd IS NOT NULL;


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
