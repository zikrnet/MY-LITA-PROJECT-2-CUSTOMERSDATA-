# MY-LITA-PROJECT-2-CUSTOMERSDATA-

# Project 2: Customer Segmentation for a Subscription Service 

Summary: This project involves analyzing customer data for a subscription service to identify segments and trends. Your goal is to understand customer behavior, track subscription types,  and identify key trends in cancellations and renewals.  The final deliverable is a Power BI  dashboard that presents your analysis.

## Data Description

The dataset includes the following fields;
*  Customer ID:  Unique customer identifeir
*  Subscription Type:  Basic, Premium and Standard
*  Subscription Start/End Dates
*  Cancellation Date
*  Demographics:  Region
*  Subscription Duration
* Revenue

![customerdataexcel](https://github.com/user-attachments/assets/e3ba9d16-d83b-4783-86e1-9c27bb9cb181)

## Instructions: 

## 1. Excel: 

o Analyze customer data using pivot tables to find subscription patterns.

Using PivotTables to analyze customer data for subscription pattern is an effective way to uncover trends and insights.

a.  Analyze Subscription Patterns:  This analysis helps to understand how many customers are subscribed to each subscription type or plan

![S pattern](https://github.com/user-attachments/assets/02161983-4a51-4410-968d-9b181dfc1be1)

b.  Analyze Subscription Status:  This analysis helps to track the current status of customers and spot any churn patterns

![S status](https://github.com/user-attachments/assets/054e545d-8d9f-4e8e-abf9-2253681af9ef)

c.  Subscription Trends over Time:  How subscriptions have changed over time, to know the number of new subscribers per month or year

![S over time](https://github.com/user-attachments/assets/e59203f3-ae3b-4491-b91c-677aef956f5c)

d.  Revenue Analysis by Subscription Plan:  To analyze the revenue generated by different subscription plans

![revenue analysis](https://github.com/user-attachments/assets/6facc16d-8e78-4cf7-b695-f67de276d7fa)

e.  Regional Analysis of Subscription Patterns:  The data includes customer regions and to analyze how subscriptions vary across different locations

![regional analysis](https://github.com/user-attachments/assets/14ac1756-e2ff-4cf6-ad53-b2efc2f64dde)

This provides insights into geographic patterns, such as which regions have the most subscribers or which plans are popular in certain areas

f.  Subscription Renewal:  To analyze subscription renewal, compare the number of active vs. cancelled subscriptions over a period


![subscription renewal](https://github.com/user-attachments/assets/509827d9-4381-47bc-ad59-81aff51681a3)


o Calculate the average subscription duration and identify the most popular subscription types. 

o Create any other interesting reports

# Pivort Charts for Visualization

To make analysis more visualluy appealing and easier to understand, I create a Pivot Charts from the PivotTable data

![chart](https://github.com/user-attachments/assets/5b52f10f-ddf1-4095-88f3-b9685e9f802c)


## 2. SQL: 

Hint – You need to load the dataset into your SQL Server environment to write and validate your queries. 

```
Create database LITA_CUSTOMER_DATA

select * from [dbo].[CustomerData]
```

![create table](https://github.com/user-attachments/assets/4a6eff40-eacc-4cbd-9182-6572670d0f88)


### Write queries to extract key insights based on the following questions.  
o  retrieve the total number of customers from each region. 

```
SELECT Region,
COUNT(customerid) AS TotalCustomers
FROM [dbo].[CustomerData]
GROUP BY Region
ORDER BY 
TotalCustomers DESC;
```

![retrieve](https://github.com/user-attachments/assets/9cd3611f-7bab-48c8-b99c-0e2dd86385be)


o  find the most popular subscription type by the number of customers.

Having a table called ``` Subscriptions ``` with the following relevent columns;

1.  ```CustomerID```:  Unique identifier for each customer
2.  ```SubscriptionType```:  The type of subscription each customer has

```
SELECT 
SubscriptionType,
COUNT(customerid) AS
NumberofCustomers
FROM [dbo].[CustomerData]
GROUP BY 
SubscriptionType
ORDER BY
NumberofCustomers DESC;
```

![most popular](https://github.com/user-attachments/assets/603ef975-e5a7-4a19-86d6-f9f4d80b6a10)


o  find customers who canceled their subscription within 6 months. 

To find the customers who canceled their subscription within 6 months, I need to identify the relevant tables and columns that store the cancellation information, including;

1.  Customer Identification: Usually a ```CustomerID```
2.  Cancellation Date: A column that records when the subscription was canceled that's ```CancelDate```
3.  Start Date: The date when the subscription started that's ```StartDate```

```
SELECT CustomerId,
SubscriptionStart,
Canceled
FROM [dbo].[CustomerData]
WHERE
Canceled IS NOT NULL
AND DATEDIFF(Month, SubscriptionStart,Canceled)<=6;
```

![6months](https://github.com/user-attachments/assets/06d7415c-b0a9-4979-8443-c246be0b4c4d)

o  calculate the average subscription duration for all customers.

I need to determine the duration of each customers subscription by substracting the subscription strart date from the cancellation date

Having a table called ```CustomersData``` with the following relevant columns;
1.  ```CustomerID```: Unique identifier for each customer
2.  ```SubscriptionStart```:  The date when the subscription started
3.  ```Canceled```: The date when the subscription was canceled

```
SELECT 
AVG(DATEDIFF(DAY, SubscriptionStart, 
COALESCE(SubscriptionEnd,Canceled,GETDATE()))) AS
AverageDurationDays
FROM [dbo].[CustomerData]
WHERE
SubscriptionStart IS NOT NULL;
```


![average](https://github.com/user-attachments/assets/4a7cbe99-7e3d-40a9-aa75-b4a18afe13dc)


o  find customers with subscriptions longer than 12 months. 

To find customers with subscriptions longer than 12 months, I need to calcullate the duration of each customer's subscription by comparing the ```SubscriptionStart``` of the subscription to either the ```Canceled```

A table called ```CustomersData``` with the following relevant columns;
1.  ```CustomerID```:  Unique identifier for each customer
2.  ```SubscriptionStart```:  The date when the subscription started
3.  ```Canceled``:  The date when the subscription was canceled

```
SELECT CustomerId,
SubscriptionStart,
Canceled,
DATEDIFF(MONTH, SubscriptionStart,COALESCE(Canceled, GETDATE())) AS
SubscriptionDurationMonths
FROM [dbo].[CustomerData]
WHERE
DATEDIFF(MONTH,SubscriptionStart, COALESCE(Canceled, GETDATE()))>12;
```

![12MONTHS](https://github.com/user-attachments/assets/f6d285ab-4047-4a3b-9c7d-0e53cd0e263a)


o  calculate total revenue by subscription type. 

To calculate total revenue by subcription type, I typically have a table that includes subscription details, including a column for the subscription type and a column for the revenue generated form each subscription

A table called ```CustomerData``` with the following relevant columns;

1.  ```SubscriptionType```:  The type of subscription (e.g. Basic, Standard, Premium)
2.  ```Revenue```:  The revenue generated from each subscription


```
SELECT SubscriptionType,
SUM(Revenue) AS TotalRevenue
FROM
[dbo].[CustomerData]
GROUP BY	
SubscriptionType
ORDER BY
TotalRevenue DESC;
```

![totalrevenue](https://github.com/user-attachments/assets/590ae529-4a2d-4444-807d-81dd585da169)


o  find the top 3 regions by subscription cancellations. 

To find the top 3 regions by subscription cancellations, I typically need to have a table that includes the following;

1.  Region:  The geographical area where the customer is located
2.  Canceled:  The date when the subscription was canceled

```
SELECT Region,
COUNT(*) AS CancellationCount
FROM [dbo].[CustomerData]
WHERE 
Canceled is NOT NULL
GROUP BY
Region
ORDER BY
CancellationCount DESC
OFFSET 0 Rows FETCH NEXT 3 ROWS ONLY;
```


![top 3](https://github.com/user-attachments/assets/3391e194-99fa-45e9-b70b-999fd401a6c0)


o  find the total number of active and canceled subscriptions.

To find the total number of active and canceled subscriptions. I typically need to identify the relevant columns in my customerdata table. The key columns will usually include;

1.  Canceled:  The date the subscription was canceled
2.  CustomerID:  or a similar identifier to count subcriptions

```
SELECT 
CASE 
WHEN Canceled is NULL THEN 'Active'
ELSE 'Canceled'
END AS SubscriptionStatus,
COUNT(*) AS TotalSubscriptions
FROM [dbo].[CustomerData]
GROUP BY
CASE
WHEN Canceled IS NULL THEN
'Active'
ELSE 'Canceled'
END;
```

![canceled](https://github.com/user-attachments/assets/15c08778-070d-4483-b0ad-cc1de7e936c2)

# POWERBI

Build a Power BI dashboard that visualizes key customer segments, cancellations, and subscription trends. Include slicers for interactive analysis




