
# Data Analyst Project - Sales Management
* Powered by Power Bi and SQLServer 2022


# Data Analyst Project - Sales Management

A brief description of what this project does and who it's for




## Screenshots

* Sales Overview Page:

![Screenshot 2024-07-29 213127](https://github.com/user-attachments/assets/9b7b90cb-07bb-4811-8cb7-decbb5511e30)
#

* Data Model: 
![Screenshot 2024-07-29 213422](https://github.com/user-attachments/assets/f7ea09ac-ea2b-4269-aee4-7f0974a83f43)



#

* Sample SQL query in SQL Server 
![Screenshot 2024-07-29 214729](https://github.com/user-attachments/assets/20fe7134-a781-4919-aa9e-1c36c0cfc0db)



## Business Needs and Requirements

The business request for this data analyst project was an executive salses report for sales managers. Based on the requirements expressed in the business request, the following user stories were defined to fulfill delivery and ensure that the project fulfilled the business needs:
#

| No # | As a (role)           | I want (request / demand)                            | So that I (user value)                                    | Acceptance Criteria                                             |
|------|------------------------|------------------------------------------------------|------------------------------------------------------------|------------------------------------------------------------------|
| 1    | Sales Manager          | To get a dashboard overview of internet sales       | Can follow better which customers and products sell the best | A Power BI dashboard which updates data once a day              |
| 2    | Sales Representative   | A detailed overview of Internet Sales per Customers | Can follow up my customers that buy the most and who we can sell more to | A Power BI dashboard which allows me to filter data for each customer |
| 3    | Sales Representative   | A detailed overview of Internet Sales per Products  | Can follow up my Products that sell the most               | A Power BI dashboard which allows me to filter data for each Product  |
| 4    | Sales Manager          | A dashboard overview of internet sales              | Follow sales over time against budget                      | A Power BI dashboard with graphs and KPIs comparing against budget |

## SQL: Creating Tables, Data Cleaning, and Data Transformation

To create the necessary data model for doing analysis and fulfilling the business needs defined in the user stories, the following tables were extracted using SQL.

* Fact_Budget (Fact Table)

```SQL
USE [AdventureWorksDW2022]


CREATE TABLE IF NOT EXISTS [dbo].[Fact_Budget](
	[Date] [date] NOT NULL,
	[Budget] decimal (6, 0) NOT NULL
) ON [PRIMARY]


INSERT INTO Fact_Budget (Date, Budget) VALUES
('01/01/2020', 800000),
('02/01/2020', 800000),
('03/01/2020', 900000),
('04/01/2020', 900000),
('05/01/2020', 950000),
('06/01/2020', 950000),
('07/01/2020', 980000),
('08/01/2020', 980000),
('09/01/2020', 990000),
('10/01/2020', 990000),
('11/01/2020', 995000),
('12/01/2020', 999000),
('01/01/2021', 800000),
('02/01/2021', 800000),
('03/01/2021', 900000),
('04/01/2021', 900000),
('05/01/2021', 950000),
('06/01/2021', 950000),
('07/01/2021', 980000),
('08/01/2021', 980000),
('09/01/2021', 990000),
('10/01/2021', 990000),
('11/01/2021', 995000),
('12/01/2021', 999000),
('01/01/2022', 850000),
('02/01/2022', 850000),
('03/01/2022', 900000),
('04/01/2022', 900000),
('05/01/2022', 950000),
('06/01/2022', 950000),
('07/01/2022', 980000),
('08/01/2022', 980000),
('09/01/2022', 990000),
('10/01/2022', 990000),
('11/01/2022', 995000),
('12/01/2022', 999000),
('01/01/2023', 900000),
('02/01/2023', 900000),
('03/01/2023', 950000),
('04/01/2023', 950000),
('05/01/2023', 980000),
('06/01/2023', 980000),
('07/01/2023', 990000),
('08/01/2023', 990000),
('09/01/2023', 995000),
('10/01/2023', 995000),
('11/01/2023', 999000),
('12/01/2023', 999000),
('01/01/2024', 950000),
('02/01/2024', 950000),
('03/01/2024', 980000),
('04/01/2024', 980000),
('05/01/2024', 990000),
('06/01/2024', 990000),
('07/01/2024', 995000),
('08/01/2024', 995000),
('09/01/2024', 999000),
('10/01/2024', 999000),
('11/01/2024', 999000),
('12/01/2024', 999000);


```

* Fact_Internet_Sales (Fact Table):

```SQL

SELECT [ProductKey]
      ,[OrderDateKey]
      ,[DueDateKey]
      ,[ShipDateKey]
      ,[CustomerKey]
      --,[PromotionKey]
      --,[CurrencyKey]
      --,[SalesTerritoryKey]
      ,[SalesOrderNumber]
      --,[SalesOrderLineNumber]
      --,[RevisionNumber]
      ,[OrderQuantity]
      ,[UnitPrice]
      --,[ExtendedAmount]
      --,[UnitPriceDiscountPct]
      ,[DiscountAmount]
      --,[ProductStandardCost]
      ,[TotalProductCost]
      ,[SalesAmount]
      --,[TaxAmt]
      --,[Freight]
      --,[CarrierTrackingNumber]
      --,[CustomerPONumber]
      --,[OrderDate]
      --,[DueDate]
      --,[ShipDate]
  FROM [AdventureWorksDW2022].[dbo].[FactInternetSales]
  where left ([OrderDateKey], 4) >= Year(GETDATE()) - 2 
  order by OrderDateKey asc

  ```
#

* Dim_Date (Dimensional Table)
```SQL

SELECT [DateKey]
      ,[FullDateAlternateKey] as Date
      --,[DayNumberOfWeek]
      ,[EnglishDayNameOfWeek]as Day
      --,[SpanishDayNameOfWeek]
      --,[FrenchDayNameOfWeek]
      --,[DayNumberOfMonth]
      --,[DayNumberOfYear]
      ,[WeekNumberOfYear] as WeekNo
      ,CASE [EnglishMonthName] WHEN 'January' THEN 'Jan' WHEN 'February' THEN 'Feb' WHEN 'March' THEN 'Mar' WHEN 'April' THEN 'Apr' WHEN 'May' THEN 'May' WHEN 'June' THEN 'Jun' WHEN 'July' THEN 'Jul' WHEN 'August' THEN 'Aug' WHEN 'September' THEN 'Sep' WHEN 'October' THEN 'Oct' WHEN 'November' THEN 'Nov' WHEN 'December' THEN 'Dec' END AS Month
      --,[FrenchMonthName]
      ,[MonthNumberOfYear] as MonthNo
      ,[CalendarQuarter]
      ,[CalendarYear] as Year
      --,[CalendarSemester]
      --,[FiscalQuarter]
      --,[FiscalYear]
      --,[FiscalSemester]
  FROM [AdventureWorksDW2022].[dbo].[DimDate]
  where calendarYear >= '2019'
  order by Year desc


```
#

* Dim_Product (Dimensional Table)

```SQL
SELECT [ProductKey]
      --,[ProductAlternateKey]
      --,[ProductSubcategoryKey]
      --,[WeightUnitMeasureCode]
      --,[SizeUnitMeasureCode]
      ,	p.[EnglishProductName] as "Product Name" 
	  , pc.[EnglishProductCategoryName] as "Product Category Name"
	  , ps.[EnglishProductSubcategoryName] as "Product Subcategory Name"
      --,[SpanishProductName]
      --,[FrenchProductName]
      --,[StandardCost]
      --,[FinishedGoodsFlag]
      ,p. [Color] as "Product Color"
      --,[SafetyStockLevel]
      --,[ReorderPoint]
      --,[ListPrice]
      ,p.[Size] as "Product Size"
      --,[SizeRange]
      --,[Weight]
      --,[DaysToManufacture]
      ,p.[ProductLine] as "Product Line"
      --,[DealerPrice]
      --,[Class]
      --,[Style]
      ,p.[ModelName] as "Product Model Name"
      --,[LargePhoto]
      ,p.[EnglishDescription] as "Product Description"
      --,[FrenchDescription]
      --,[ChineseDescription]
      --,[ArabicDescription]
      --,[HebrewDescription]
      --,[ThaiDescription]
      --,[GermanDescription]
      --,[JapaneseDescription]
      --,[TurkishDescription]
      --,[StartDate]
      --,[EndDate]
      ,ISNULL (p.[Status], 'Outdated') as "Product Status"
  FROM [dbo].[DimProduct] p
  left join [dbo].[DimProductSubcategory] ps on ps.ProductSubcategoryKey = p.ProductSubcategoryKey
  left join [dbo].[DimProductCategory] pc on pc.ProductCategoryKey = ps.ProductCategoryKey
  order by p.productkey


```
#

* Dim_Customers (Dimensional Table)

```SQL
SELECT c.[CustomerKey] as CustomerKey
      --,[GeographyKey]
      --,[CustomerAlternateKey]
      --,[Title]
      ,c.[FirstName] as "First Name"
      --,[MiddleName]
      ,c.[LastName] as "Last Name"
	  ,c.[FirstName] + ' ' + c.[LastName] as "Full Name"
      --,[NameStyle]
      --,[BirthDate]
      --,[MaritalStatus]
      --,[Suffix]
      ,CASE c.[Gender] when 'M' then 'Male' when 'F' then 'Female' end as [Gender]
      --,[EmailAddress]
      --,[YearlyIncome]
      --,[TotalChildren]
      --,[NumberChildrenAtHome]
      --,[EnglishEducation]
      --,[SpanishEducation]
      --,[FrenchEducation]
      --,[EnglishOccupation]
      --,[SpanishOccupation]
      --,[FrenchOccupation]
      --,[HouseOwnerFlag]
      --,[NumberCarsOwned]
      --,[AddressLine1]
      --,[AddressLine2]
      --,[Phone]
      ,[DateFirstPurchase]
      --,[CommuteDistance]
	  ,g.[City] as "Customer City"
  FROM [AdventureWorksDW2022].[dbo].[DimCustomer] c
  Left join [AdventureWorksDW2022].[dbo].[DimGeography] g on g.GeographyKey = c.GeographyKey
  ORDER BY CustomerKey asc


```


## Dataflows, Semantic models, and Data models

* We created Dataflows to connect Power Bi to SQL Server Bi (Using Gateway):

![Screenshot 2024-07-29 213801](https://github.com/user-attachments/assets/7f64e573-1741-49d2-9082-d4ac5700b036)


#
* Table View in Dataflow:

![Screenshot 2024-07-29 213900](https://github.com/user-attachments/assets/bc284f83-8fab-435a-ac9b-dea8f599ae9f)
#
* Power Query View (Dataflow)

![Screenshot 2024-07-29 214031](https://github.com/user-attachments/assets/7acdddbb-9425-419a-9a76-bb96aff91784)


* We created Data Models to help us get the right data for our analysis:

![Screenshot 2024-07-29 213422](https://github.com/user-attachments/assets/f7ea09ac-ea2b-4269-aee4-7f0974a83f43)






## Power Bi: Dashboard


* Sales Overview Page:

![Screenshot 2024-07-29 213127](https://github.com/user-attachments/assets/9b7b90cb-07bb-4811-8cb7-decbb5511e30)
#

## View the Power BI Report

[View Power BI Report](https://app.powerbi.com/view?r=eyJrIjoiZGJiYmU0ZDktZjY1Ni00ODk5LWIxYzItZGRhYzk5MjM2ODllIiwidCI6IjJiNDA4NGFjLTBmMzMtNGU1YS1hYTY0LTdkYmNmN2QxMjU2ZiIsImMiOjl9)






