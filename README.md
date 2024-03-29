# Layoff 2022 Data Analytics Python Project


## Table of Contents

1. [Project Overview](#project-overview)
2. [Project Title](#project-title)
3. [Project Done By](#project-done-by)
4. [Clients](#clients)
5. [Operational Analytics Problem Statement](#operational-analytics-problem-statement)
6. [Project Deliverable](#project-deliverable)
7. [Understanding Data](#understanding-data)
8. [Methods of Identifying and Analyzing Data](#methods-of-identifying-and-analyzing-data)
9. [Data Cleaning](#data-cleaning)
10. [Data Manipulation](#data-manipulation)
11. [Utilizing Power BI for Data Visualization](#utilizing-power-bi-for-data-visualization)
12. [Generating Key Insights Using Power BI](#generating-key-insights-using-power-bi)
13. [Creating Dashboard for Layoff 2022 using Power BI](#creating-dashboard-for-layoff-2022-using-power-bi)
14. [Integration of CRISP-DM Methodology](#integration-of-crisp-dm-methodology)
15. [Summary](#summary)


## Project Overview

The "Layoff 2022 Data Analytics" project aims to analyze the impact of layoffs in the tech industries from 2020 to 2022, specifically evaluating the effects of the COVID-19 pandemic. The project delves into understanding the trends, patterns, and implications of layoffs across various tech companies worldwide. It involves data collection, cleaning, manipulation, and Power BI is utilized for dynamic data visualization and interactive dashboard creation, enhancing the project's analytical capabilities.


### Project Title
Analysis Of Layoff In The Tech Industries From Covid 2020 – 2022 – "Evaluating Tech companies affected as a result of the pandemic"

### Project Done By
Vishnu Priya Ashok Kumar

### Clients
Financial organizations and the Tech Companies

### Operational Analytics Problem Statement
Tech companies all across the world are battling the recession. Slow consumer spending, increasing central bank interest rates, and strong foreign currencies all point to a potential recession, and IT companies have already begun to lay off staff.

### Project Deliverable
The component of our solution will be based on the dashboard and the Analysis Report.
- The dashboard will contain information which will be summarized and displayed on different charts/graphs to analyze the Layoff’s in the Tech industry during the pandemic till present using Power BI.
- The analysis model would utilize a lot of queries to draw conclusions and to determine associations between variables within the dataset using Python-Jupyter Notebook.
- The Key Insights and recommendations are also delivered.

### Understanding Data

- Layoff dataset concentrates on tech companies across all Industries
- 1415 companies from 55 countries were employed in the research 
- Funding stage was grouped into various stages
    1. Seed funding – The first financial support a company gets. Investors invest in a great idea. Usually from family and friends
    2. Series A : follows seed funding. However long-term goals in generating profit are expected from the investors. Funds raised vary from $2 to 15 million
    3. Series B : Comes after Series A. They are used to grow businesses further. Funding in this stage is for businesses with valuations of $30 to $60 million
    4. Series C : The funding supports successful businesses to double profits. Like merging with other businesses or buying off companies or increasing branches and outlets
    5. Series D, E, F, G, H, I, and  J : Are various stages of funding that most companies don’t go through due to financial expectation 
    6. IPO (Initial Public Offering) : Is when a private company sells stock to the public to generate more funds. For most companies, this comes after Series C
    7. Private equity : Is an investment partnership with matured companies. Private equity firms manage the portfolio of companies to increase their worth. 
    8. Acquired : Refers to purchasing of majority of shares of another company to control the operations and the assets.
    9. Unknown :  The funding source is not stated.


### Methods of identifying and analysing data

- Data was obtained from https://www.kaggle.com/datasets/swaptr/layoffs-2022
  
- Cleaning and manipulation of data were processed using Python-Jupyter Notes
  
- Analysis and dashboard were created using Power BI and Python-Jupyter Note


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
![image](https://github.com/priya-ak/Python-Project-Layoff-Analysis-2022/assets/67804361/57005703-d873-4737-b59f-52485b994abe)

## Data Manipulation

### Categorizing employees Laid off in each country
```python
LaidoffByCountry = df.groupby(['country'])['total_laid_off'].sum()
print(LaidoffByCountry)
```
![image](https://github.com/priya-ak/Python-Project-Layoff-Analysis-2022/assets/67804361/68f4fe48-37e9-4a4c-a299-ef8a9560bd1e)

### Grouping total laid off during each year
```python
GroupByYear = df.groupby(df['date'].dt.year)['total_laid_off'].agg(['sum'])
print(GroupByYear)
```
![image](https://github.com/priya-ak/Python-Project-Layoff-Analysis-2022/assets/67804361/f4e0ab0a-da47-4b06-b8fb-05aaed127247)

### Categorizing specific country and industry based on specific funding stage
```python
FundingStages = df.loc[df['stage'] == 'Acquired']
result = FundingStages.loc[(FundingStages['country'] == 'United States') & (FundingStages['industry'] == 'Media')]
result.head(50)
```
![image](https://github.com/priya-ak/Python-Project-Layoff-Analysis-2022/assets/67804361/50fa2e64-4872-4923-b5a2-3541ac79d139)

### Top 5 Countries where layoff has occurred
```python
df.groupby('country')['total_laid_off'].sum().sort_values(ascending=False).head().plot(ylabel="", figsize=(8,8), kind='pie', stacked=True, colormap='tab10')
```
![image](https://github.com/priya-ak/Python-Project-Layoff-Analysis-2022/assets/67804361/39798a65-378e-409c-ae9c-b38daa331193)

### Total layoffs worldwide since 2020 in various industries
```python
import matplotlib.pyplot as plt
plt.figure(figsize=(10, 6))
plt.title("Total layoffs worldwide since 2020 in various industries")
plt.ylabel("Reported layoff Numbers")
dfIndustries2020 = df.groupby('industry').sum()['total_laid_off'].sort_values(ascending=False).plot(figsize=(16,8), kind='bar', stacked=True, colormap='cool')
```
![image](https://github.com/priya-ak/Python-Project-Layoff-Analysis-2022/assets/67804361/cd21af07-6a58-4c19-b8b4-e1eb696aa5a9)

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
![image](https://github.com/priya-ak/Python-Project-Layoff-Analysis-2022/assets/67804361/3fa7faa6-00c0-417c-a0c0-94f688be2216)

### Top 5 Companies that laid off Staff's in 2022
```python
df = df.set_index('date')
df2022Companies = df.loc[:'2022']
df2021Companies = df.loc[(df.index > '2021-01-01')&(df.index < '2022-01-01')]
df2020Companies = df.loc[(df.index > '2020-01-01')&(df.index < '2021-01-01')]
df2022CompaniesLayoffCount = df2022Companies.sort_values(by='total_laid_off', ascending=False)
df2022CompaniesLayoffCount.head()
```
![image](https://github.com/priya-ak/Python-Project-Layoff-Analysis-2022/assets/67804361/71884807-2379-4524-a817-762cfba69d6a)

## Utilizing Power BI for Data Visualization

In addition to the data manipulation techniques performed using Python and Jupyter Notebook, the project also utilizes Power BI for creating interactive and insightful visualizations. Power BI enables the creation of dynamic dashboards and reports to present the analyzed data in a user-friendly format.

### Generating Key Insights Using Power BI
The project extracts key insights from the analyzed data using Power BI. Through interactive visualizations and reports, stakeholders can gain a deeper understanding of the trends, patterns, and implications of layoffs in the tech industry.

![image](https://github.com/Vishnu-Priya-Ashok-Kumar/Python-Project-Layoff-Analysis-2022/assets/67804361/8196f789-ba9f-450c-ac75-4b05f49a3494)


## Creating Dashboard for layout 2022 using Power BI

The project includes the creation of a comprehensive dashboard for Layoff 2022 using Power BI. This dashboard provides a centralized view of the analyzed data, allowing users to explore and interact with the information effectively.

![image](https://github.com/Vishnu-Priya-Ashok-Kumar/Python-Project-Layoff-Analysis-2022/assets/67804361/1b982575-7c1f-41bf-8975-f799efc08e7f)

### Integration of CRISP-DM Methodology

The project follows the Cross-Industry Standard Process for Data Mining (CRISP-DM) methodology, which includes the following phases:

1. **Business Understanding**: Understanding the objectives and requirements of the business.
2. **Data Understanding**: Collecting and familiarizing with the data to identify how data needs to be presented for manipulation.
3. **Data Preparation**: Cleaning and converting data to a useful form.
4. **Modeling**: Applying appropriate modeling techniques to extract quality information aligned with the business cause.
5. **Evaluation**: Assessing results generated using various parameters.
6. **Deployment**: Submitting and presenting outcomes to the clients.

## Summary

The COVID-19 pandemic has had profound effects on the global economy, with the tech industry being no exception. Layoffs in tech companies have become increasingly prevalent as businesses navigate through economic uncertainties. The "Layoff 2022 Data Analytics" project addresses this phenomenon by providing a comprehensive analysis of layoffs in the tech sector.

Key components of the project include:

- Data Collection: Raw data regarding layoffs in tech companies from 2020 to 2022 is gathered for analysis.
  
- Data Cleaning: The collected data undergoes cleaning processes to ensure accuracy and consistency, including handling missing values and formatting issues.
  
- Data Manipulation: Various data manipulation techniques are employed to categorize layoffs by country, analyze trends over time, and examine specific scenarios such as layoffs after acquisition.
  
- Data Visualization: The cleaned and manipulated data is visualized using charts, graphs, and dashboards to present insights effectively.
  
- Insights and Recommendations: The project concludes with extracting key insights and providing recommendations based on the analysis to aid tech companies and financial institutions in understanding the impact of COVID-19 on their workforce and finances.

Through this project, stakeholders gain valuable insights into the dynamics of layoffs in the tech industry, empowering them to make informed decisions and strategize for the future.
