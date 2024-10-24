# Telecom-Customer-Churn-Analysis
---
## Tools Used - SQL , POWERBI, PYTHON.
---
## Table of Content
---
1. [Project Objective](#project-objective)  
2. [ETL Framework](#etl-framework)  
3. [Dataset Description](#dataset-description)  
4. [Project Overflow](#project-overflow)  
5. [PowerBI Dashboard](#powerbi-dashboard)  
6. [Predictions](#predictions)

---
# Project Objective
---
## Project Objective
This project aims to build an **ETL pipeline and Power BI dashboard** to effectively utilize customer data and achieve the following goals:
1. **Analyze Customer Data** at multiple levels:
   - **Demographic**: Age, Gender, Income.
   - **Geographic**: Region, City.
   - **Payment & Account Info**: Payment type, billing cycles.
   - **Services**: Subscriptions and usage patterns.
2. **Study the Churner Profile** to identify segments requiring marketing intervention.
3. **Predict Future Churners** to enable proactive customer retention strategies.

---

## ETL Framework
---
Our Framework uses Below Components:
1.  CSV file - This is our source file
2.  SQL Server Management Studio (SSMS) - We will use its inbuilt import wizard to transform and load the data
3.  SQL Server Database - This is where our final data will be loaded and host our data warehouse, tables and views for final usage.
---
## Dataset Description
---
No. of rows=6419, No. of columns=32. The dataset consists of customer information of a telecom company like Customer_ID, Gender, Age, Married, State, Number_of_Referrals, Tenure_in_Months, Value_Deal, Phone_Service, Multiple_Lines, Internet_Service, Internet_Type, Online_Security, Online_Backup, Device_Protection_Plan, Premium_Support, Streaming_TV, Streaming_Movies, Streaming_Music, Unlimited_Data, Contract, Paperless_Billing, Payment_Method, Monthly_Charge, Total_Charges, Total_Refunds, Total_Extra_Data_Charges, Total_Long_Distance_Charges, Total_Revenue, Customer_Status, Churn_Category, Churn_Reason.

---
## Project Overflow
---
### SQL (SSMS)
1.  Created a new database `db_Churn` in SQL Server Management Studio, after connecting to server.
2.  Imported our CSV file using `Import Flat File` in SSMS and modified the columns by checking Allow Nulls checkbox for every column except for Candidate_ID (Primary Key), and changing bit datatype to varchar50 datatype to avoid errors on importing.
3.  After importing, performed data analysis using SQL queries.
4.  Created a new table `prod_Churn` from our previous table `stg_Churn` by replacing NULL values by 'None' or 'No' values.
5.  Created two views `vw_ChurnData` and `vw_JoinData` to be used later in predictive analysis.

## PowerBI
6.Connected Power BI to our SQL Server and imported `prod_Churn` table into our Power Query Editor using Transform Data option.
7.Created a new custom column ```Churn_Status``` and changed the datatype to Whole Number.
```dax
if [Customer_Status] = "Churned" then 1 else 0
```
8.Created a new custom column ```Monthly_Charge_Status.```
``` dax
if [Monthly_Charge] < 20 then "<20"
  else if [Monthly_Charge] < 50 then "20-50"
  else if [Monthly_Charge] < 100 then "50-100"
  else ">100"
```
9.Created a new table ```mapping_AgeGrp``` by referencing the ```prod_Churn``` table and removed all other columns except Age column and removed duplicates from Age column.
10.Created a new custom column Age_Group in mapping_AgeGrp table.
``` dax
if [Age] < 20 then "<20"
  else if [Age] < 35 then "20-35"
  else if [Age] < 50 then "35-50"
  else ">50"
```
11.Created a new custom column ```AgeGrpSorting``` in ```mapping_AgeGrp``` table and changed datatype to Whole Number.
``` dax
 if [Age_Group] = "<20" then 1
  else if [Age_Group] = "20-35" then 2
  else if [Age_Group] = "35-50" then 3
  else 4
```
12.Created a new table ```mapping_TenureGrp``` by referncing the ```prod_Churn``` table and removed all other columns except Tenure_in_Months column and removed duplicates.
13.Created a new custom column ```Tenure_Group``` in ```mapping_TenureGrp``` table.
``` dax
if [Tenure_in_Months] < 6 then "< 6 Months"
  else if [Tenure_in_Months] < 12 then "6-12 Months"
  else if [Tenure_in_Months] < 18 then "12-18 Months"
  else if [Tenure_in_Months] < 24 then "18-24 Months"
  else ">= 24 Months"
```






  
