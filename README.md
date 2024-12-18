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

#   WEEK 7
- **SQL database set up using SQLite DB Browser for the pharmaceutical data, ensuring 
data integrity and security**

- **Creation of facts and dimension tables**
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
- **ERD Creation using SQLITE**
![Pharm ERD Racheal Ilelaboye](https://github.com/user-attachments/assets/c8531558-70d2-47ae-a69e-50d5977681fb)


- **Initial data cleaning**
```SQL
---- Count missing values in each column
SELECT 
    COUNT(*) - COUNT(Sales) AS MissingValues, 
    Sales
FROM 
   Pharm_Data ;
```

# WEEK 8
- **Sales performance analysis**
```SQL
---------Total sales by Products
SELECT 
    ProductName,  SUM(Sales) AS TotalSales
FROM 
    Pharm_Data
GROUP BY  
    ProductName
ORDER BY 
    TotalSales DESC;
```
![1A](https://github.com/user-attachments/assets/3b67a47d-813f-4876-83ff-0b76380d120b)

```SQL
----------MONTHLY SALES TREND
SELECT 
    Month,
    SUM(Sales) AS TotalMonthlySales
FROM 
    Pharm_Data
GROUP BY 
    Month
ORDER BY 
     Month;
```
![1B](https://github.com/user-attachments/assets/18c446da-cb61-4bc6-8e62-8132ec553681)

- **Sales by location**
```SQL
-----CITIES WITH HIGH SALES VOLUME
SELECT 
    City, 
    SUM(Sales) AS TotalSalesVolume
FROM 
    Pharm_Data
GROUP BY 
    City
ORDER BY 
    TotalSalesVolume DESC
LIMIT 10;  
```
![2A](https://github.com/user-attachments/assets/011c328d-52f9-4d78-b621-1af23dbde71a)

```SQL
--------sales by Country
SELECT 
    Country, 
    SUM(Sales) AS TotalSalesVolume
FROM 
    Pharm_Data
GROUP BY 
    Country
ORDER BY 
    TotalSalesVolume DESC;  
```
![2B](https://github.com/user-attachments/assets/570441db-4664-48c4-9474-338c0e778aa6)
- **Customer Segmentation**
```SQL
------Total sales by customer type
SELECT 
    Channel, 
    SUM(Sales) AS TotalSalesVolume
FROM 
    Pharm_Data
GROUP BY 
    Channel
ORDER BY 
    TotalSalesVolume DESC;
```
![3A](https://github.com/user-attachments/assets/45d67760-e31d-4b55-aada-90ea5a8824f7)
```SQL
------Total sales by sector
SELECT 
    Sub_channel AS Sector, 
    SUM(Sales) AS TotalSales
FROM 
    Pharm_Data
GROUP BY 
    Sub_channel
ORDER BY 
    TotalSales DESC;
```
![3B](https://github.com/user-attachments/assets/edd9639d-7f54-4afb-94e0-53453e388f5a)
- **Product analysis**
``` SQL
---------top selling products within each drug class
WITH RankedProducts AS (
    SELECT 
        ProductClass,
        ProductName,
        SUM(Sales) AS TotalSales,
        RANK() OVER (PARTITION BY ProductClass ORDER BY SUM(Sales) DESC) AS SalesRank
    FROM 
        Pharm_Data
    GROUP BY 
        ProductClass, 
        ProductName)
SELECT 
    ProductClass,
    ProductName,
    TotalSales
FROM 
    RankedProducts
WHERE 
    SalesRank = 1
ORDER BY 
    ProductClass, TotalSales DESC;
```
![4A](https://github.com/user-attachments/assets/f47e060e-2b82-4657-a1a0-3c04e738b571)
```SQL
----average price for each drug class
SELECT 
    ProductClass,  avg(Price) AS Averageprice
FROM 
    Pharm_Data
GROUP BY  
    ProductClass
ORDER BY 
    Averageprice DESC;
```
![4B](https://github.com/user-attachments/assets/a6dcc412-286e-4046-ab52-97161da172df)
- **Sales representative performance**
```sql
------top performing sales rep
SELECT 
    NameofSalesRep,  sum(Sales) AS Totalsales
FROM 
    Pharm_Data
GROUP BY  
   NameofSalesRep
ORDER BY 
    Totalsales DESC
	LIMIT 10;
```
[5A](https://github.com/user-attachments/assets/5aa3a322-fbe0-4308-bc9c-3707a7ce0137)
```sql
------sales across teams
SELECT 
    SalesTeam,  sum(Sales) AS Totalsales
FROM 
    Pharm_Data
GROUP BY  
   SalesTeam
ORDER BY 
    Totalsales DESC;
```
![5B](https://github.com/user-attachments/assets/31529632-c5d9-4502-9e0f-9ad563c7f97b)
- **Time series analysis**
```sql
-----year over year growth in sales
WITH YearlySales AS (
    SELECT 
        Year,
        SUM(Sales) AS TotalSales
    FROM 
        Pharm_Data
    GROUP BY 
        Year
)
SELECT 
    Year,
    TotalSales,
    LAG(TotalSales) OVER (ORDER BY Year) AS PreviousYearSales,
    CASE 
        WHEN LAG(TotalSales) OVER (ORDER BY Year) IS NOT NULL THEN 
            ((TotalSales - LAG(TotalSales) OVER (ORDER BY Year)) / LAG(TotalSales) OVER (ORDER BY Year)) * 100
        ELSE 
            NULL
    END AS YoYGrowth
FROM 
    YearlySales
ORDER BY 
    Year;
```
![6A](https://github.com/user-attachments/assets/601ce23b-5d7b-487d-8ec5-9b4ebf712848)
```sql
----------seasonal sales patterns
SELECT 
    Month,
    SUM(Sales) OVER (PARTITION BY Month) AS MonthlySales
FROM 
    Pharm_Data
GROUP BY 
    YEAR, MONTH
ORDER BY 
    MonthlySales DESC;
```
![6B](https://github.com/user-attachments/assets/59be3ee9-0988-40b5-9a43-d1e40165c5cf)

# WEEK 9
- **Data validation with python**
```python
# Connect to database
conn = r"C:\Users\user\Desktop\3Signet\Week 7\Pharm.db"
connection = sqlite3.connect(conn)
# Read sqlite query results into a pandas DataFrame
connection = sqlite3.connect(r"C:\Users\user\Desktop\3Signet\Week 7\Pharm.db" )
df = pd.read_sql_query("SELECT * from Pharm_Data", connection)
```

```python
#To check for null values
df.isna().sum()

# Check for duplicates in the entire dataset
duplicates = df.duplicated()
print(f"Number of duplicate rows: {duplicates.sum()}")

# To confirm datatype consistency
print(df.dtypes)

## Using Z-scxore to identify outliers
from scipy import stats
df['z_score'] = np.abs(stats.zscore(df['Sales']))
outliers_z = df[df['z_score'] > 3]
print(outliers_z)

# Replacing outliers
df['Sales'] = np.where(df['Sales'] > upper_bound, upper_bound,
np.where(df['Sales'] < lower_bound, lower_bound,␣
↪df['Sales']))
```
- **Explorative data analysis with python**
```python
 # Descriptive statistics
df.describe().round(1)

# Filter negative sales values
negative_sales = df[df['Sales'] < 0]
print(negative_sales)

# DATA VALIDATION
# Create a new column for returns (negative values)
df['Returns'] = df['Sales'].apply(lambda x: x if x < 0 else 0)
# Create a new column for positive sales

# Summary of data validation checks
validation_report = {
'Missing Values': df.isnull().sum(),
'Duplicates': df.duplicated().sum(),
'Invalid Data Types': df.dtypes,
'Outliers': df[df['z_score'].abs() > 3].shape[0]
}
print(validation_report)
```

- **Sales analysis with python**
```python
# Group by year and sum the sales
sales_by_year = df.groupby('Year')['Sales'].sum()
print(sales_by_year)

df.groupby('Year')['Sales'].sum().plot(kind='bar')
plt.title("Sales by Year")
plt.show()
```
![sales x year](https://github.com/user-attachments/assets/c0d71153-da69-4099-96d6-fba13a65ed39)

```python
# Sales by City
sales_by_city = df.groupby('City')['Sales'].sum()
print(sales_by_city)

plt.figure(figsize=(10,6))
plt.plot(df.groupby('City')['Sales'].sum().sort_values(ascending=False))
plt.title("Sales by City")
plt.show()
```
![sales xcity](https://github.com/user-attachments/assets/585981e8-5b1e-45d8-a0f3-a050d9a905f5)

- **Customer demographics and purchasing pattern**
``` python
#Customer by Country
df.groupby('Country')['CustomerName'].count()

df.groupby('Country')['CustomerName'].count().plot(kind='bar')
plt.title("Customer by Country")
plt.show()
```
![customer x country](https://github.com/user-attachments/assets/e1b853a0-abdb-496f-a11a-91d4d80edaf5)

```python
# Customer by sales
df.groupby('CustomerName')['Sales'].sum()

plt.figure(figsize=(10,6))
plt.plot(df.groupby('CustomerName')['Sales'].sum().sort_values(ascending=False).
↪head(5))
plt.title("TOP 5 Customers by Sales")
plt.show()
```
![customer x sales](https://github.com/user-attachments/assets/f49996c4-2346-4d87-afe8-66773e2842c2)

- **Product performance**
```python
# Sales by Product
g= df.groupby('ProductName', as_index=False)['Sales'].sum().sort_values(by=␣
↪'Sales', ascending=False).head(10)
sns.barplot(data=g, x='ProductName', y='Sales', hue='ProductName', dodge=False).
↪set(xticklabels=[]);
```
![sales x pdt](https://github.com/user-attachments/assets/41a2d20a-c61a-4898-936d-1290f8305d15)

```python
plt.figure(figsize=(10,6))
plt.plot(df.groupby('ProductClass')['Sales'].sum())
plt.title("Sales by ProductClass")
plt.show()
```
![sales x pdtclass](https://github.com/user-attachments/assets/099a2d86-52bb-4dc8-80f0-3e746346538c)

```python
 # Product by Quantity Sold
df.groupby('ProductName')['Quantity'].sum().sort_values(ascending=False).
↪head(10)
plt.figure(figsize=(10,6))
plt.plot(df.groupby('ProductName')['Quantity'].sum().
↪sort_values(ascending=False).head(10))
plt.title("TOP 10 Products by Quantity Sold")
plt.show()
```
![pdt x qty sold](https://github.com/user-attachments/assets/4f4fcf86-ed61-4373-a2e0-d047f93952e4)


# WEEK 10
- **Geospatial analysis with python**
```python
# create a geopandas DataFrame
geometry = [Point(xy) for xy in zip(df['Longitude'], df['Latitude'])]
gdf = gpd.GeoDataFrame(df, geometry = geometry)
# create a geopandas DataFrame
geometry = [Point(xy) for xy in zip(df['Longitude'], df['Latitude'])]
gdf = gpd.GeoDataFrame(df, geometry = geometry)
```
![geospatial](https://github.com/user-attachments/assets/f655a1c0-7b0a-41d9-9747-57e1a683abf9)

```python
# filter the dataframe to include only long., lat., and transaction amount
df_filtered = df[['Longitude', 'Latitude', 'Sales']]
df_filtered.head(2)
# create a scatter plot to visualize the relationship between longitude, latitude, and Sales
plt.figure(figsize = (10, 8))
plt.scatter(df_filtered['Longitude'], df_filtered['Latitude'], c = df_filtered['Sales'],
           cmap = 'viridis')
plt.colorbar(label = 'Sales')
plt.xlabel('Longitude')
plt.ylabel('Latitude')
plt.title('Sales by Location')
plt.show()
```
![sales x location](https://github.com/user-attachments/assets/505bda00-a5f8-4b8a-a214-4b706f45cac1)

- **Channel Analysis**
```python
## Visualization
g= df.groupby('Channel', as_index=False)['Sales'].sum().sort_values(by='Sales', ascending=False).head(10)
sns.barplot(data=g, x='Channel', y='Sales', hue='Channel', dodge=False).set(xticklabels=[]);
```
![sales x channel](https://github.com/user-attachments/assets/d4e75358-4322-45ac-906b-c34b94757826)

```python
plt.figure(figsize=(10,6))
plt.plot(df.groupby('Channel')['Quantity'].sum())
plt.title("Quantity sold by channel")
plt.show()
```
![qty x channel](https://github.com/user-attachments/assets/59cfc08f-9653-4bd6-8338-429426111a91)



# WEEK 11
## Hypothesis Testing
- **Hypothesis 1** : Higher Product price correlate with lower sales quantity

**Method 1: Pearson correlation coefficient**
```python
# Calculate Pearson correlation coefficient and p-value
correlation, p_value = stats.pearsonr(df['Price'], df['Quantity'])
print(f'Correlation coefficient: {correlation}')
print(f'p-value: {p_value}')
```
**Result** :
Correlation coefficient: -0.0005318345925375115
p-value: 0.7886391604084931

**Interpretation** :
Negative value of correlation coefficient Indicates a negative relationship (as price 
increases, quantity decreases).
p-value ≥ 0.05 indicates no significant evidence of correlation.

**Conclusion** :
A negative correlation coefficient supports the hypothesis however, p-value >0.05 indicates 
that the hypothesis is not supported by the data.

**Method 2: Linear Regression**
```python
import statsmodels.api as sm

# Define independent (X) and dependent (y) variables
X = df['Price']
y = df['Quantity']

# Add a constant term for the intercept
X = sm.add_constant(X)

# Build and fit the regression model
model = sm.OLS(y, X).fit()

# Print the model summary
print(model.summary())
```

**Result**:
![hypo ols](https://github.com/user-attachments/assets/eacf740c-1e6c-40c7-bbab-4f189e42bf51)

**Interpretation** :
 - R-squared (0.000):
   o The model explains 0% of the variation in Quantity, suggesting that Price has 
no explanatory power over Quantity.
 - Coefficient of Price (-0.0018):
   o The negative coefficient suggests a slight negative relationship between Price
and Quantity (as price increases, quantity slightly decreases), but:
   o p-value (0.789) > 0.05: This relationship is not statistically significant, meaning 
this result could be due to chance.
 - Intercept (113.5975):
   o When Price is zero, the expected Quantity is approximately 113.6 units.
   o This value is significant (p-value = 0.000), but it doesn't imply a meaningful 
relationship with price.
 - F-statistic (0.07187) and Prob (F-statistic) (0.789):
   o The F-statistic tests if the model as a whole is significant. A very low F-statistic 
with a high p-value indicates that the model doesn’t significantly explain 
variations in Quantity.
 - Residual Diagnostics:
   o Skewness and Kurtosis: High skew (53.870) and kurtosis (5374.334) suggest 
that the residuals (errors) are not normally distributed, which can impact the 
validity of OLS assumptions.

**Conclusion**: 
There is no significant relationship between Price and Quantity.
The model has poor explanatory power (R-squared = 0), indicating that Price does not 
affect Quantity in this dataset.**The hypothesis is thereby rejected**

- **Hypothesis 2**: Sales performance varies significantly across different customer channel.

**Method** One-Way ANOVA 
```python
# Group sales data by customer channel
groups = [group['Sales'].values for name, group in df.groupby('Channel')]

# Perform One-Way ANOVA
f_statistic, p_value = stats.f_oneway(*groups)

print(f'F-statistic: {f_statistic}')
print(f'p-value: {p_value}')
```

**Result**:
F-statistic: 4.314565343723608
p-value: 0.03778842555381452

**Interpretation**:
A large F-statistic indicates that the variation between group means is greater than the variation within the groups, suggesting a potential difference among the groups.
Since p-value < 0.05, we reject the null hypothesis. 

**Conclusion**:
There is a significant difference in sales performance across different customer channels.
Since p-value < 0.05, **we reject the hypothesis.**

## Statistical Analysis
- **Correlation Analysis**
``python
# Correlation matrix for numerical variables
correlation_matrix = df[['Sales', 'Price', 'Quantity']].corr()
print(correlation_matrix)
``
**Result** : 
            Sales     Price  Quantity
Sales     1.000000  0.070665  0.899464
Price     0.070665  1.000000 -0.000532
Quantity  0.899464 -0.000532  1.000000

**Interpretation**:
 - Sales and Quantity are strongly positively correlated, suggesting that the quantity of items sold significantly impacts total sales.
 - Price has almost no correlation with either sales or quantity, implying that pricing changes do not strongly influence sales or quantities sold in this dataset.

- **Simple Linear Regression Analysis**
```python
# Simple Linear Regression
model = ols('Sales ~ Price + Quantity', data=df).fit()
print(model.summary())
```
**Result**:
![stat ols 1](https://github.com/user-attachments/assets/147ed2b8-8eed-4796-af4e-60ab315ceb33)

**Interpretation**:
- The model as a whole is statistically significant, meaning that at least one of the predictors (Price or Quantity) significantly impacts sales.
- For every one-unit increase in Price, sales increase by 110.43 units, holding Quantity 
constant.
- t-value = 83.172, p-value < 0.05: Price is a statistically significant predictor of Sales.
- For every one-unit increase in Quantity, sales increase by 421.9999 units, holding Price constant.
- Quantity has a much larger impact on Sales compared to Price, as indicated by the higher 
coefficient (421.9999 vs. 110.43).

- **Multiple Regression**
```python
# Multiple Regression
multiple_model = ols('Sales ~ Price + Quantity + C(Channel)', data=df).fit()
print(multiple_model.summary())
```
**Result**
![stat ols 2](https://github.com/user-attachments/assets/3cb7939e-b595-48b5-b94e-928febb4e44d)


- **ANOVA**
```PYTHON
# One-way ANOVA
anova_result = sm.stats.anova_lm(model, typ=2)
print(anova_result)
```

**Results**:
            sum_sq df F PR(>F)
Price 1.568093e+14 1.0 6.917605e+03 0.0
Quantity 2.506714e+16 1.0 1.105831e+06 0.0
Residual 5.759502e+15 254079.0 NaN NaN

**Interpretation**:
 - Quantity dominates the model, explaining significantly more variance in Sales than Price 
(as indicated by its much larger sum of squares and F-statistic).
 - Price, while significant, has a smaller impact on the variation in Sales compared to 
Quantity.

##Recommendations
1. Focus on Increasing Quantity
- Insight: Quantity is the most significant driver of sales, as evidenced by its high coefficient (421.99 in regression) and dominant contribution to the explained 
variance (sum of squares and F-statistic).
- Action:
 - Implement promotions, discounts, or bundling strategies to incentivize 
bulk purchases.
 - Optimize inventory management to ensure stock availability for highdemand products.
 - Leverage marketing campaigns targeting products with higher sales 
potential.

2. Price Optimization with Caution
- Insight: While Price has a statistically significant impact on sales, its influence is 
relatively smaller compared to Quantity (lower sum of squares and coefficient of 
110.43).
- Action:
 - Conduct price sensitivity analysis to identify optimal price points that 
maximize sales without reducing volume.
 - Use dynamic pricing strategies, adjusting prices based on demand 
patterns, seasons, or competitor pricing.
 - Avoid aggressive price increases that could adversely affect sales volume.
   
3. Reassess Channel Strategies
- Insight: The "Pharmacy" channel does not significantly impact sales compared to 
other channels (p = 0.385 for Channel in regression).
- Action:
 - Focus efforts on channels that consistently drive higher sales (e.g., retail or 
online).
 - Explore cross-channel strategies, such as leveraging retail and pharmacy 
for complementary benefits.
 - Evaluate if channel-specific factors, like customer preferences or 
marketing, can be further optimized.

4. Address Residual Variance
- Insight: Residual variance (5.759502e+15 in ANOVA) suggests some variation in 
sales remains unexplained by Price and Quantity.
- Action:
 - Investigate additional variables, such as customer demographics, region, 
or time of year, that could enhance the model.
 - Consider interaction terms (e.g., Price × Channel) to uncover nuanced 
relationships.
 - Use machine learning models like decision trees or random forests to 
capture non-linear relationships and improve predictions.

5. Monitor for Multicollinearity
- Insight: A condition number of 1.98e+03 in regression suggests potential 
multicollinearity between predictors.
- Action:
 -  Calculate the Variance Inflation Factor (VIF) to confirm multicollinearity.
 - If present, consider transforming variables or removing redundant ones to 
improve model reliability.

6. Refine Model with Diagnostic Checks
- Insight: Residual diagnostics (non-normality, skewness, and high kurtosis) 
indicate potential outliers or model limitations.
- Action:
 - Perform residual plots to identify patterns or outliers.
 - Apply transformations (e.g., log or square root) if necessary to stabilize 
variance and normalize residuals.
 - Reevaluate the model after addressing identified issues.
Strategic Priorities:
 - Short-term: Optimize pricing and drive immediate sales through quantity-increasing 
campaigns.
 - Medium-term: Analyze additional variables to improve model accuracy and identify 
unexplored sales drivers.
 - Long-term: Implement channel-specific and customer-segmented strategies for sustained 
growth

#WEEK 12
- Data modelling (Fact and dimension tables) and relationship between tables
![Racheal Ilelaboye model view](https://github.com/user-attachments/assets/71be0511-b62d-49ee-8afc-172aba4b5946)

- DAX Measures
![DAX](https://github.com/user-attachments/assets/8b0754cc-14be-406b-878d-cf1e5127000f)

- Dashboard
![PH 1](https://github.com/user-attachments/assets/3ce83bad-d074-4749-b36a-8319f183b8bb)
![PH 2](https://github.com/user-attachments/assets/27aa8256-6064-4004-b143-b065fc54f023)


- Final presentation slide summarizing the project
[View here]![Racheal Ilelaboye Presentaton.pptx](https://github.com/user-attachments/files/18172658/Racheal.Ilelaboye.Presentaton.pptx)

- Live demonstatration of dashboard


# CONCLUSION
This project successfully analyzed sales data using Excel, SQL,Python and Power BI to uncover meaningful insights into sales performance across different cities, product, and customer segments.

Key Findings:
- Sales Trends: Sales peaked in August.
- Top Products: Ionclotide is the best-performing product.
- Country Insights: Germany consistently outperformed poland in sales volume.
- Customer Segmentation: Key customer segments were identified, leading to targeted marketing opportunities.

# RECOMMENDATIONS
- Adjust Pricing: Introduce competitive pricing strategies or discounts.
- Product Enhancement: Upgrade features, quality, or packaging.
- Sales Training: Equip teams with updated strategies and tools.
- Feedback Mechanisms: Implement systems to gather customer feedback
- Geographic Expansion: Enter new regions or international markets.
- Benchmarking: Compare your business against competitors' pricing, products, and services.
Track progress and remain flexible.
![image](https://github.com/user-attachments/assets/a83a14d4-715b-4dff-b123-9195c8289aad)
