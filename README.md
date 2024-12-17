# ABC PHARMACEUTICAL COMPANY
An end-to-end data analysis project using Excel, Python, SQL, and Power BI to analyze sales data, identify trends, and generate interactive dashboards

# PROJECT OVERVIEW

## Aim
This project aims to overcome these challenges by conducting a 
comprehensive data and geospatial analysis, revealing intricate patterns and correlations. By 
leveraging advanced analytical techniques, the company can uncover hidden insights, refine 
marketing strategies, and improve overall operational efficiency.

## Objectives
The objectives of this analysisis to:
- **Identify Sales Trends:** Uncover trends in sales data over time, including peak periods and 
sales dips.
- **Understand Customer Behavior:** Examine customer demographics and purchasing patterns 
to identify key customer segments and behaviors.
- **Evaluate Product Performance:** Assess the performance of different products, identifying 
best-sellers and underperforming items.
- **Analyze Geospatial Impact:** Understand the impact of geography on sales to identify high 
and low-performing regions.
- **Channel Performance:** Compare sales performance across different customer channels 
(e.g., Hospital vs. Pharmacy).
- **Optimize Sales Strategies:** Provide recommendations for improving sales strategies based 
on data insights.
- **Enhance Product Offerings:** Suggest optimizations for product offerings based on 
performance and customer preferences.

# DATA SOURCE
The dataset for this analysis is an excel file gotten from 3Signet [Official Website](https://www.3signet.com/)

#TOOLS USED
- MS Excel [Download here](https://www.microsoft.com/en-us/microsoft-365/excel)

  Initial data cleaning and exploration

- DB SQLITE [Download here](https://sqlitebrowser.org/dl/)

   Querying and aggregating data from a relational database

- Python [Download here](https://www.python.org/downloads/)

   Explorative Data Analysis

- PowerBI [Download here](https://www.microsoft.com/en-us/download/details.aspx?id=58494)

   Building interactive dashboards and visualizations

- MS Powerpoint [Download here](https://www.microsoft.com/en/microsoft-365/powerpoint)

   Presentation slide preparation

- Github [Signup here](https://github.com/join)

  Portfolio building

- Zoom [Download here](https://zoom.us/download?os=win)

   Live demonstration

#   WEEK 1
- SQL database set up using SQLite DB Browser for the pharmaceutical data, ensuring 
data integrity and security

- Creation of facts and dimension tables
```SQL
-------------Creating fact table
CREATE TABLE Sales(
    SalesId INTEGER PRIMARY KEY NOT NULL,
	Sales 	INTEGER  NOT NULL,
    Month 	TEXT  NOT NULL,
	Year date  NOT NULL,
	ProductId INTEGER  NOT NULL,
	CustomerId INTEGER  NOT NULL,
	Name_of_sales_Rep TEXT  NOT NULL,
	Manager TEXT  NOT NULL,
	Sales_Team TEXT NOT NULL,
	
	FOREIGN KEY (ProductId) REFERENCES Products(ProductId),
	FOREIGN KEY (CustomerId) REFERENCES Location (CustomerId));
	
	-------Dimension Tables
	CREATE TABLE Products(
	ProductName TEXT NOT NULL,
	ProductClass TEXT NOT NULL,
	Quantity_Price INTEGER NOT NULL,
	ProductId INTEGER PRIMARY KEY NOT NULL);
	
  CREATE TABLE Location(
	Distributor TEXT NOT NULL,
	CustomerId INTEGER PRIMARY KEY NOT NULL,
	CustomerName TEXT NOT NULL,
	City TEXT NOT NULL,
	CHANNEL TEXT NOT NULL,
	COUNTRY TEXT NOT NULL,
	Sub_channel TEXT NOT NULL,
	Latitude INTEGER NOT NULL,
	Longitude  INTEGER NOT NULL);
```
- ERD Creation using SQLITE


- Initial data cleaning
```SQL
---- Count missing values in each column
SELECT 
    COUNT(*) - COUNT(Sales) AS MissingValues, 
    Sales
FROM 
   Pharm_Data ;
```
