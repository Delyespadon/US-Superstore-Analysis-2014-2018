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
  ## I) Data manipulation and analysis with python 

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
#### * Accross differents categories
```python
#2) analyze profit margin across Category 
anova_result2 = stats.f_oneway(df[df["Category"] == "Furniture"]['Profit Margin'],
                               df[df["Category"] == "Office Supplies"]['Profit Margin'],
                               df[df["Category"] == "Technology"]['Profit Margin'])
# Output results
print("F-value:", anova_result2.statistic)
print("P-value:", anova_result2.pvalue)

if anova_result2.pvalue < 0.05:
    print("Conclusion: There are significant differences in profit margins across produts category.")
else:
    print("Conclusion: There are no significant differences in profit margins across product category.")
```
```python
# Boxplot for profit margin by category
plt.figure(figsize=(8, 6))
sns.boxplot(data=df, x='category', y='Profit Margin', palette='Set2')
plt.title('Profit margin Distribution by category')
plt.xlabel('Category')
plt.ylabel('Profit margin')
plt.show()
```
 #### * Accross differents segments
 ```python
 #2)  profit margin across segment 
anova_result3 = stats.f_oneway(df[df["Segment"] == "Consumer"]['Profit Margin'],
                               df[df["segment"] == "Corporate"]['Profit Margin'],
                               df[df["Segment"] == "Home Office"]['Profit Margin'])
# Output results
print("F-value:", anova_result3.statistic)
print("P-value:", anova_result3.pvalue)

if anova_result3.pvalue < 0.05:
    print("Conclusion: There are significant differences in profit margins across segment.")
else:
    print("Conclusion: There are no significant differences in profit margins across segment.")
```
 ```python
# Boxplot for profit margin by segment
plt.figure(figsize=(8, 6))
sns.boxplot(data=df, x='segment', y='Profit Margin', palette='Set2')
plt.title('Profit margin distribution by segment')
plt.xlabel('segment')
plt.ylabel('Profit margin')
plt.show()
```
#### * Accross subcategory
```python
# Boxplot for profit margin by subcategory
plt.figure(figsize=(8, 6))
sns.boxplot(data=df, x='Sub-Category', y='Profit Margin', palette='Set2') 
plt.title('Profit Margin Distribution by Sub-category')
plt.xticks(rotation=45, fontsize=8) 
plt.xlabel('Sub-category')
plt.ylabel('Profit Margin (%)')
plt.tight_layout()  
plt.show()
```
#### c) Products category accross regions
```python
# Cross-tabulation: Category vs. Region
category_region = pd.crosstab(df['Category'], df['Region'])
print("Category vs. Region:")
category_region
# Stacked Bar Chart: Category vs. Region
category_region.plot(kind='bar', stacked=True, figsize=(10, 6))
plt.title('Category Distribution by Region')
plt.xlabel('Category')
plt.ylabel('Count')
plt.xticks(rotation=45)
plt.show()
```
#### d) Products sub-category accross regions
```python
# Heatmap: Sub-Category vs. Region
plt.figure(figsize=(10, 6))
sns.heatmap(subcategory_region, annot=True, fmt='d', cmap='Blues')
plt.title('Sub-Category Distribution by Region')
plt.xlabel('Region')
plt.ylabel('Sub-Category')
plt.show()
``` 
#### e) Profit margin based on category and regions
```python
# Cross-tabulation Heatmap: Product Category vs Region (Profit Margin)
ct = pd.crosstab(df['Category'], df['Region'], values=df['Profit Margin'], aggfunc='mean')
sns.heatmap(ct, annot=True, cmap='coolwarm')
plt.title('Average Profit Margin by Category and Region')
plt.show()
```
 ##### f) Profit prediction based on sales and discount
 ```python
  # profit prediction based on sales and discount
import pandas as pd 
df = pd.read_csv(r"C:/Users/Espadon/Desktop/Sample _Superstore.csv", encoding='ISO-8859-1')
df.head()
import statsmodels.api as sm
X = df[['Sales', 'Discount']]
X = sm.add_constant(X)
y = df['Profit']
model = sm.OLS(y, X).fit()
print(model.summary())
```

## II) Data visualisation with PowerBI
Sales Performance Report: Monthly Sales Analysis

### 1. Executive Summary:
This report evaluates sales performance using a Power BI dashboard designed to track key performance indicators (KPIs) such as sales, profit margins, and regional performance. The dashboard includes visualizations that illustrate sales trends, category performance, and profitability across regions, helping identify areas for improvement and strategic growth.

### 2. Dashboard Overview:
The dashboard features five primary KPIs: number of customers, number of orders, number of items sold, total sales, and profit. Filters for region, category, month, and year enable a focused monthly analysis. Visual elements include:

Column bar charts: Sales by region, sales by category, profit by region, profit by category, and sales by segment.

Line chart: Daily sales trend.

Scatter chart: Profit vs. sales comparison by segment.

## III) Keys Insights of the analysis 
- The **West** region generates the highest sales, but the **East** region has the highest profitability.
- **Technology** is the most profitable category, while **Office Supplies** has the highest sales volume.
- There is a negative correlation between **Discount** and **Profit**, suggesting excessive discounting reduces profitability.
- ANOVA results indicate significant differences in profit and sales across regions and categories.
- Chi-square results show a significant relationship between region and category sales.
- The Regression  model shows that increasing sales boosts profit, while offering discounts reduces profit. However, the model’s explanatory power is limited, and addressing multicollinearity or including additional predictors could improve its accuracy.

  ###  IV) Recommendations
1. Reduce discounts, especially in regions where profits are negatively impacted.
2. Focus marketing efforts on the **Technology** category to maximize profitability.
3. Investigate the reasons for lower profitability in the **South** and **Central** regions.
4. Use targeted promotions during high-sales periods like **December** to boost revenue.
5. Optimize inventory levels in high-performing regions to meet demand without overstocking
6. Focus on boosting sales and optimizing discount strategies to maximize profit.

 **Documentation**: Keep clear notes of each query's objective, approach, and results.

### 8. Project Publishing and Documentation
   - **Documentation**: Maintain well-structu
   - red documentation of the entire process in Markdown or a mySQL file
   - **Project Publishing**: Publish the completed project on GitHub or any other version control platform, including:
     - The `README.md` file (this document).
     - Jupyter Notebooks (if applicable).
     - SQL query scripts.
     - Data files (if possible) or steps to access them.


## Requirements


- **Python Programming Language**: python editors

- **Kaggle API Key** (for data downloading)

## Getting Started

   ```
1. Set up your Kaggle API, download the data, and follow the steps to load and analyze.

---

## Project Structure

```plaintext
|-- data/                     # Raw data and transformed data
|-- Python_script/              # SQL scripts for analysis and queries
|-- README.md                 # Project documentation
|-- main.py                   # Main script for loading, cleaning, and processing data
```

## Acknowledgments

- **Data Source**: Kaggle’s superstore  Sales Dataset
- **Inspiration**: Walmart’s business case studies on sales and supply chain optimization.

---
