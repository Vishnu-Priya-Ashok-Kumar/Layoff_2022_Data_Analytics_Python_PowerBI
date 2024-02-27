# Layoff 2022 Data Analytics
## Table of Contents

1. [Project Overview](#project-overview)
2. [Project Title](#project-title)
3. [Project Done By](#project-done-by)
4. [Client](#client)
5. [Operational Analytics Problem Statement](#operational-analytics-problem-statement)
6. [Project Deliverable](#project-deliverable)
7. [How does your solution provide value?](#how-does-your-solution-provide-value)
8. [Data Cleaning](#data-cleaning)
9. [Data Manipulation](#data-manipulation)
10. [Summary](#summary)
## Project Overview

The "Layoff 2022 Data Analytics" project aims to analyze the impact of layoffs in the tech industries from 2020 to 2022, specifically evaluating the effects of the COVID-19 pandemic. The project delves into understanding the trends, patterns, and implications of layoffs across various tech companies worldwide. It involves data collection, cleaning, manipulation, and visualization to derive meaningful insights.


### Project Title
ANALYSIS OF LAYOFF IN THE TECH INDUSTRIES FROM COVID 2020 – 2022 – "Evaluating Tech companies affected as a result of the pandemic"

### Project Done By
Vishnu Priya Ashok Kumar

### Client
Financial organizations and the Tech Companies

### Operational Analytics Problem Statement
Tech companies all across the world are battling the recession. Slow consumer spending, increasing central bank interest rates, and strong foreign currencies all point to a potential recession, and IT companies have already begun to lay off staff.

### Project Deliverable
The component of our solution will be based on the dashboard and the Analysis Report.
- The dashboard will contain information which will be summarized and displayed on different charts/graphs to analyze the Layoff’s in the Tech industry during the pandemic till present using Power BI.
- The analysis model would utilize a lot of queries to draw conclusions and to determine associations between variables within the dataset using Python-Jupyter Notebook.
- The Key Insights and recommendations are also delivered.

### How does your solution provide value?
The solution generated from our findings will provide both the Tech companies and the Financial Institutions significant information about the effects of COVID on their finances and workforce. We hope that this information will further act as a guide for these companies to create a better working system and to accommodate crises like this in the future.

## Data Cleaning
```python
import pandas as pd
import numpy as np

data = pd.read_excel('layoffs 2022.xlsx') # Importing Raw uncleaned dataset
data = data.dropna(how='all', axis=0) # Deleting totally blank rows
data = data.dropna(how='all', axis=1) # Deleting totally blank columns

df = pd.DataFrame(data)
df["total_laid_off"].fillna(0, inplace=True) # Replacing NaN values with 0 in total_laid_off column
df["percentage_laid_off"].fillna(0, inplace=True) # Replacing NaN values with 0 in percentage_laid_off column
df["funds_raised"].fillna(0, inplace=True) # Replacing NaN values with 0 in funds_raised column
df["stage"].fillna('Unknown', inplace=True) # Replacing NaN values with 'Unknown' in stage column

df.to_excel('FinalProject_cleaneddata.xlsx') # Exporting and saving cleaned dataset
```

## Data Manipulation

### Categorizing employees Laid off in each country
```python
LaidoffByCountry = df.groupby(['country'])['total_laid_off'].sum()
print(LaidoffByCountry)
```

### Grouping total laid off during each year
```python
GroupByYear = df.groupby(df['date'].dt.year)['total_laid_off'].agg(['sum'])
print(GroupByYear)
```

### Categorizing specific country and industry based on specific funding stage
```python
FundingStages = df.loc[df['stage'] == 'Acquired']
result = FundingStages.loc[(FundingStages['country'] == 'United States') & (FundingStages['industry'] == 'Media')]
result.head(50)
```

### Top 5 Countries where layoff has occurred
```python
df.groupby('country')['total_laid_off'].sum().sort_values(ascending=False).head().plot(ylabel="", figsize=(8,8), kind='pie', stacked=True, colormap='tab10')
```

### Total layoffs worldwide since 2020 in various industries
```python
import matplotlib.pyplot as plt
plt.figure(figsize=(10, 6))
plt.title("Total layoffs worldwide since 2020 in various industries")
plt.ylabel("Reported layoff Numbers")
dfIndustries2020 = df.groupby('industry').sum()['total_laid_off'].sort_values(ascending=False).plot(figsize=(16,8), kind='bar', stacked=True, colormap='cool')
```

### Annual layoffs across industries in every country
```python
import seaborn as sns
df['date'] = pd.to_datetime(df['date'])
dfIndustryAnnual = df.groupby([df.industry, df.date.dt.year]).sum()
dfIndustryAnnual = dfIndustryAnnual.reset_index()
plt.figure(figsize=(14, 10))
plt.xticks(rotation=90)
plt.title("Annual layoffs across industries in every country")
sns.set(style="white", palette="Set2", color_codes=True)
sns.barplot(data=dfIndustryAnnual.sort_values(by=['total_laid_off','date'], ascending=False), x="industry", y="total_laid_off", hue="date")
```

### Top 5 Companies that laid off Staff's in 2022
```python
df = df.set_index('date')
df2022Companies = df.loc[:'2022']
df2021Companies = df.loc[(df.index > '2021-01-01')&(df.index < '2022-01-01')]
df2020Companies = df.loc[(df.index > '2020-01-01')&(df.index < '2021-01-01')]
df2022CompaniesLayoffCount = df2022Companies.sort_values(by='total_laid_off', ascending=False)
df2022CompaniesLayoffCount.head()
```
## Summary

The COVID-19 pandemic has had profound effects on the global economy, with the tech industry being no exception. Layoffs in tech companies have become increasingly prevalent as businesses navigate through economic uncertainties. The "Layoff 2022 Data Analytics" project addresses this phenomenon by providing a comprehensive analysis of layoffs in the tech sector.

Key components of the project include:

- Data Collection: Raw data regarding layoffs in tech companies from 2020 to 2022 is gathered for analysis.
  
- Data Cleaning: The collected data undergoes cleaning processes to ensure accuracy and consistency, including handling missing values and formatting issues.
  
- Data Manipulation: Various data manipulation techniques are employed to categorize layoffs by country, analyze trends over time, and examine specific scenarios such as layoffs after acquisition.
  
- Data Visualization: The cleaned and manipulated data is visualized using charts, graphs, and dashboards to present insights effectively.
  
- Insights and Recommendations: The project concludes with extracting key insights and providing recommendations based on the analysis to aid tech companies and financial institutions in understanding the impact of COVID-19 on their workforce and finances.

Through this project, stakeholders gain valuable insights into the dynamics of layoffs in the tech industry, empowering them to make informed decisions and strategize for the future.
