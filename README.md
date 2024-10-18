# MY-LITA-PROJECT-2-CUSTOMERSDATA-

# Project 2: Customer Segmentation for a Subscription Service 

## Summary: This project involves analyzing customer data for a subscription service to identify segments and trends. Your goal is to understand customer behavior, track subscription types,  and identify key trends in cancellations and renewals.  The final deliverable is a Power BI  dashboard that presents your analysis.

## Instructions: 

## 1. Excel: 

o Analyze customer data using pivot tables to find subscription patterns.

o Calculate the average subscription duration and identify the most popular subscription types. 

o Create any other interesting reports.

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
o  find the total number of active and canceled subscriptions.
