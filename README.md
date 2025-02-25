# US-Superstore-Analysis-2014-2018 using python and PowerBI for a comprehensive dashboard  # 
## About the data set 
The Sample Superstore dataset represents sales data from a retail company. It includes information on orders, products, customers, and shipping across different regions in the United States. The key variables in this dataset are:
•	Sales: Revenue generated from each order.
•	Profit: The profit earned from each order.
•	Discount: Discounts applied to products.
•	Category & Sub-Category: Product categories like Furniture, Office Supplies, and Technology.
•	Region: Geographic regions (East, West, South, Central).
•	Order Date & Ship Date: Dates of order placement and shipping.
•	Segment: Customer segments like Consumer, Corporate, and Home Office.
•	Ship Mode: Different shipping methods used.
This dataset is ideal for exploring the relationships between sales, profit, and product categories across different regions. It helps identify profitable categories, assess the impact of discounts on profits, and analyze regional performance

## Objectives
-	Analyze qualitative-qualitative, qualitative-quantitative, and quantitative-quantitative relationships that will help in optimizing its sales strategy.
-	Create an interactive dashboard to analyze key KPIs such as sales, margins, and regional performance that helps to track sales performance by region and category to identify areas for improvement.

## Dataset

The data for this project is sourced from the Kaggle dataset:

- **Dataset Link:**(https://www.kaggle.com/datasets/vivek468/superstore-dataset-final)

## 1. Libraries importations 
```python
# Import necessary libraries
import pandas as pd
import numpy as np
import seaborn as sns
import matplotlib.pyplot as plt
from scipy import stats
import math
```
## 2. Data Loading 
```python
# Load dataset
df = pd.read_csv(r"C:/Users/Espadon/Desktop/Sample _Superstore.csv", encoding='ISO-8859-1')
df.head()
```
## 3. Data set cleaning 
```python
# Checking for duplicated values
print("Number of duplicated rows:", df.duplicated().sum())

# 2) Checking for missing values
print("\nMissing values per column:", df.isna().sum())

# Dropping duplicates
df = df.drop_duplicates()
```
## 4. Data set cleaning
```python
# Number of rows and columns
print("Shape of the dataset (rows, columns):", df.shape)

# Or get a concise summary of the DataFrame
print("\nSummary of the dataset:")
df.info()

# Data preparation 
df['Profit Margin'] = df['Profit'] / df['Sales'] * 100
df['Profit Margin'].head() 
```
## 5.Exploratory Data Analysis(EDA)
### a) Basics information and Summary Statistics
```python
# Basic information
print(df.info())
print(df.describe())

# Unique categories
print(df['Category'].unique())
print(df['Region'].unique())
print(df["Segment"].unique())
print(df['Sub-Category'].unique())

# Count of orders by region
print(df['Region'].value_counts())

#Summary Statistics
summary_stats = df[['Sales', 'Profit', 'Discount', 'Profit Margin']].describe()
summary_stats 
```
### b) Profit margin
#### * Accross regions
```python
 Analyze the profit margin by regions 
# ANOVA Test:
anova_result1 = stats.f_oneway(df[df['Region'] == 'East']['Profit Margin'],
                              df[df['Region'] == 'West']['Profit Margin'],
                              df[df['Region'] == 'South']['Profit Margin'],
                              df[df['Region'] == 'Central']['Profit Margin'])
# Output results
print("F-value:", anova_result1.statistic)
print("P-value:", anova_result1.pvalue)

if anova_result1.pvalue < 0.05:
    print("Conclusion: There are significant differences in profit margins across regions.")
else:
    print("Conclusion: There are no significant differences in profit margins across regions.")
```
```python
# Boxplot for profit margin by region
plt.figure(figsize=(8, 6))
sns.boxplot(data=df, x='Region', y='Profit Margin', palette='Set2')
plt.title('Profit margin  Distribution by Region')
plt.xlabel('Region')
plt.ylabel('Profit margin')
plt.show()
```








