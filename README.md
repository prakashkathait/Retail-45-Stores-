# Python(Jupyter Notebook)- Retail-45 Stores Analysis

## Introduction and Objective

### Background Information
The retail industry is highly competitive and dynamic, with various factors influencing sales 
performance across different stores. Understanding these factors is crucial for making informed decisions that 
can enhance profitability and customer satisfaction. In this project, we analyze sales data from 45 stores belonging
to a major retail chain. The dataset includes information on weekly sales, store characteristics, and economic indicators.

### Objectives
The primary objectives of this analysis are:
1. **Identify Trends**: Analyze trends in weekly sales over time to understand seasonal patterns and other temporal effects.
2. **Evaluate Store Performance**: Compare sales performance across different store types and sizes to identify which types
    and sizes are most profitable.
3. **Impact of Economic Factors**: Assess the impact of external economic factors, such as fuel price, CPI, and unemployment
    rate, on sales performance.
4. **Holiday Effect**: Determine how holidays influence sales and identify the most lucrative holiday periods.


By achieving these objectives, we aim to provide insights that can help the retail chain enhance its operational strategies
and improve overall performance.

## Data Description

### Dataset Source
The dataset used in this analysis is provided by [Retail Company], containing sales data from 45 stores over several years.

### Overview of the Dataset
The dataset captures various aspects of store performance, including sales, store type, size, and economic indicators. The primary goal is to analyze the factors affecting sales and derive actionable insights for improving retail performance.

### Structure of the Dataset
- **Rows**: 8190
- **Columns**: 14

### Key Features (Columns)

1. **Store**: Integer
   - Identifier for the store.
   
2. **Date**: Date
   - The date of the recorded sales.
   
3. **Weekly_Sales**: Float
   - Total sales for the store for a particular week.
   
4. **Holiday_Flag**: Integer
   - Indicates whether the week included a holiday (1) or not (0).
   
5. **Temperature**: Float
   - Temperature in the region during the week.
   
6. **Fuel_Price**: Float
   - Cost of fuel in the region during the week.
   
7. **CPI**: Float
   - Consumer Price Index in the region.
   
8. **Unemployment**: Float
   - Unemployment rate in the region.
   
9. **Store_Type**: String
   - Type of the store (e.g., A, B, C).

10. **Store_Size**: Integer
    - Size of the store in square feet.

### Example Records
Here are a few example records from the dataset:

| Store | Date       | Weekly_Sales | Holiday_Flag | Temperature | Fuel_Price | CPI  | Unemployment | Store_Type | Store_Size |
|-------|------------|--------------|--------------|-------------|------------|------|--------------|------------|------------|
| 1     | 2020-01-10 | 15000.25     | 0            | 45.3        | 2.75       | 215.1| 8.5          | A          | 2000       |
| 2     | 2020-01-10 | 25000.60     | 1            | 42.8        | 2.80       | 214.8| 8.3          | B          | 3000       |
| 3     | 2020-01-17 | 18000.45     | 0            | 47.5        | 2.90       | 216.5| 8.7          | A          | 2500       |

### Data Dictionary 
- **Store**: Identifier for the store.
- **Date**: Date of the recorded sales.
- **Weekly_Sales**: Total sales for the store for a particular week.
- **Holiday_Flag**: Indicates whether the week included a holiday.
- **Temperature**: Temperature in the region during the week.
- **Fuel_Price**: Cost of fuel in the region during the week.
- **CPI**: Consumer Price Index in the region.
- **Unemployment**: Unemployment rate in the region.
- **Store_Type**: Type of the store.
- **Store_Size**: Size of the store in square feet.

### Summary
The dataset provides comprehensive sales and economic data across multiple stores, which will be used to analyze sales trends, identify factors affecting sales, and develop recommendations for improving retail performance.

### Steps followed 

- Importing libraries 
        
         import pandas as pd
         import matplotlib.pyplot as plt

- Reading csv. files by using pandas library on the jupyter notebook.
        
      feature_df = pd.read_csv(r'D:\Kaggle Projects\Retail\Features data set.csv')
      stores_df = pd.read_csv(r'D:\Kaggle Projects\Retail\stores data-set.csv')
      sales_df = pd.read_csv(r'D:\Kaggle Projects\Retail\sales data-set.csv')
- Checking for the data types 
    
      feature_df.info()
      stores_df.info()
      sales_df.info()
      #Or we can use the dtypes 

- Checking for null values 
         
       feature_df.isna().sum()

#### Feature Engineering
- Changing the datatype of the date column to 'datetime' format.
          
      #Converting the dataFrame into date format 
      feature_df['Date'] = pd.to_datetime(feature_df['Date'], format='%d/%m/%Y')
      sales_df['Date'] = pd.to_datetime(sales_df['Date'], format='%d/%m/%Y')

- Extracting month and Year from the date column

      feature_df['Month'] = feature_df['Date'].dt.month 
      feature_df['Year'] = feature_df['Date'].dt.year

Replacing the null values with 0 in order to maintain the cosistency in the data. As i have found large number of null values in the columns (Markdown-1 to 5)
     
      feature_df[['MarkDown1','MarkDown2','MarkDown3','MarkDown4','MarkDown5']] =feature_df[['MarkDown1','MarkDown2','MarkDown3','MarkDown4','MarkDown5']].fillna(0)
- Imputing Missing values in CPI and Unemployment column with forward and backward fill.

      feature_df[['CPI','Unemployment']] = feature_df[['CPI','Unemployment']].fillna(method='ffill').fillna(method='bfill')
 
### Merging of all files and stores them in a seperate Dataframe variable(merged_df)
      merged_df = pd.merge(sales_df,feature_df, on=['Store','Date'])
      merged_df = pd.merge(merged_df,stores_df, on ='Store')
- Renaming of the columns 
- Checking for duplicacy 

### EDA (Exploratory Data Analysis)

##### 1.Sales trend over the time 
    
     weekly_sales = merged_df.groupby('Date')['Weekly_Sales'].sum().reset_index()

#### Plotting the total weekly sales over time 
      plt.figure(figsize=(14,8))
      plt.plot(weekly_sales['Date'],weekly_sales['Weekly_Sales'],color='blue',label='Weekly_Sales')
      plt.xlabel('Date')
      plt.ylabel('Total Weekly Sales')
      plt.title('Total Weekly Sales Over Time')
      plt.legend()
      plt.grid(True)
      plt.show()
![Sales vs time](https://github.com/prakashkathait/Retail-45-Stores-/assets/166843819/cec9ec35-89f1-459a-8824-cd9ebf28d753)
 
##### 2.Sales by store Type

      sales_by_type = merged_df.groupby('Type')['Weekly_Sales'].mean().reset_index()
      sales_by_type

#### Plotting sales by stores

      plt.figure(figsize=(8,5))
      plt.bar(sales_by_type['Type'],sales_by_type['Weekly_Sales'],color='green',label='Average sales by store type')
      plt.xlabel('Store Type')
      plt.ylabel('Average Weekly Sales')
      plt.title('Average Weekly Sales by Store Type')
      plt.legend()
      plt.grid(False)
      plt.show()
![sales by stores](https://github.com/prakashkathait/Retail-45-Stores-/assets/166843819/0bb6c072-3007-4607-a7fc-36b657eeff85)
### Plot sales by store size 

##### Groupby store size and calculate average sales 
      sales_by_size = merged_df.groupby('Size')['Weekly_Sales'].mean().reset_index()

#### Plot sales by store size 

      plt.figure(figsize=(7,7))
      plt.scatter(sales_by_size['Size'],sales_by_size['Weekly_Sales'],color='red',label ='Average sales by store size')
      plt.xlabel('Store Size')
      plt.ylabel('Average Weekly sales')
      plt.title('Average Weekly Sales by Store Size')
      plt.show()
![sales by storesize](https://github.com/prakashkathait/Retail-45-Stores-/assets/166843819/1ef7b405-fea4-442b-bc69-87a3d9550a74)


##### 3.Impact of holidays 
      sales_by_holiday = merged_df.groupby('IsHoliday')['Weekly_Sales'].mean().reset_index()

#### Plot sales by holiday
      plt.figure(figsize=(4,4))
      plt.bar(sales_by_holiday['IsHoliday'],sales_by_holiday['Weekly_Sales'],color='purple')
      plt.xticks(ticks=[0,1],labels=['False','True'])
      plt.xlabel('Holiday')
      plt.ylabel('Average Weekly Sales')
      plt.title('Average Weekly Sales by Holiday')
      plt.legend()
      plt.show()

![average sale by holiday](https://github.com/prakashkathait/Retail-45-Stores-/assets/166843819/8d5fd1ce-af03-4c30-b5fa-8a088f53225f)

## 4.External factors 

Impact of Temperature, Fuel Price,CPI,Unemployement 

#### Sales By External Factors Impacts
      plt.figure(figsize=(10,6))
      plt.scatter(merged_df['Temperature'],merged_df['Weekly_Sales'],color='orange',label='Sales vs. Temperature')

      plt.xlabel('Temperature')
      plt.ylabel('Weekly Sales')
      plt.title('Weekly Sales Vs. Temperature')
      plt.legend()
      plt.grid(True)
      plt.show()


## Final Insights and Recommendations

### Key Insights
1. **Total Weekly Sales Over Time**:
   - Observed seasonal trends with significant peaks during spring season (oct - nov).

2. **Average Weekly Sales by Store Type**:
   - Store Type A generates the highest average weekly sales, followed by Type B and Type C.

3. **Average Weekly Sales by Store Size**:
   - Larger stores generally have higher weekly sales, but there is an optimal size range where sales maximize.

4. **Average Weekly Sales by Holiday**:
   - Sales are significantly higher during holiday periods compared to non-holiday periods.

5. **Weekly Sales vs. CPI**:
   - There is a correlation between consumer price index (CPI) and weekly sales, indicating the impact of inflation on consumer spending.

### Recommendations
1. **Focus on Store Type A**:
   - Invest in expanding and optimizing Store Type A as it generates the highest sales.

2. **Optimize Store Sizes**:
   - Consider the optimal store size range for new openings to maximize sales.

3. **Leverage Holidays for Promotions**:
   - Plan marketing and promotional activities around holidays to capitalize on higher consumer spending.

4. **Monitor Economic Indicators**:
   - Keep track of economic indicators like CPI to anticipate and strategize around changes in consumer spending behavior.


## Conclusion

### Summary of Key Findings
1. **Total Weekly Sales Over Time**: The analysis revealed significant seasonal trends, with sales peaking during holiday seasons and certain promotional periods.
2. **Store Performance**: Store Type A consistently showed the highest average weekly sales, while larger stores generally outperformed smaller ones.
3. **Economic Factors**: There was a noticeable correlation between economic indicators, such as CPI and fuel prices, and weekly sales, indicating that macroeconomic conditions impact consumer spending.
4. **Holiday Effect**: Sales were significantly higher during holiday weeks, highlighting the importance of holiday promotions and inventory management.

### Implications
These findings underscore the importance of strategic planning around seasonal trends and holidays. By focusing on high-performing store types and sizes, and taking economic conditions into account, the retail chain can better optimize its operations and marketing efforts.

### Recommendations
1. **Focus on High-Performing Stores**: Invest in expanding and optimizing Store Type A, which has shown the highest sales.
2. **Leverage Holidays**: Enhance promotional activities and inventory management during holiday periods to maximize sales.
3. **Monitor Economic Indicators**: Regularly track economic conditions to adjust pricing and marketing strategies accordingly.
4. **Optimize Store Size**: Consider the optimal store size for new openings to balance costs and sales efficiency.

### Limitations
- The analysis is based on historical data, which may not fully capture future trends.
- External factors, such as competitor actions and changing consumer behavior, were not considered in this analysis.

### Future Work
- Conduct a deeper analysis of customer demographics and behavior to better tailor marketing strategies.
- Explore the impact of online sales and omnichannel strategies on overall performance.
- Perform predictive modeling to forecast future sales trends and plan accordingly.

By addressing these areas, the retail chain can continue to refine its strategies and achieve sustained growth.

