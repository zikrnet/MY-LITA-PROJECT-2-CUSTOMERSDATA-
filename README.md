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
o  calculate the average subscription duration for all customers. 
o  find customers with subscriptions longer than 12 months. 
o  calculate total revenue by subscription type. 
o  find the top 3 regions by subscription cancellations. 
o  find the total number of active and canceled subscriptions.
