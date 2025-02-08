# Space Launch Analysis 

## Project Overview

This project analyzes historical space launches using a dataset of missions. The analysis focuses on launch trends by country, year, and other relevant factors such as mission success rates and price evolution. The project is implemented using Python, leveraging Pandas for data manipulation, SQL queries for structured analysis, and Matplotlib/Seaborn for data visualization.

## Objectives

* Perform data cleaning to handle missing values and ensure data consistency.

* Identify trends in space launches over time.

* Analyze the number of launches by country.

* Evaluate the success and failure rates of missions.

* Examine price evolution of space missions.

## Step-by-Step Process

### 1. Data Loading and Exploration

Load the dataset from a CSV file using Pandas.

Display the dataset to understand its structure.

```python
import pandas as pd

df = pd.read_csv('mission_launches.csv')
```
### 2. Handling Missing Data

Check for missing values in the dataset.

Replace null values with 0 to ensure data integrity.

```python
df.isnull().sum()
df.fillna(0, inplace=True)
df.isnull().sum()
```

### 3. Converting and Extracting Date Information

Convert the 'Date' column to datetime format.

Extract the 'Year' from the date column.

```python
df['Date'] = pd.to_datetime(df['Date'], utc=True, errors='coerce')
df['Year'] = df['Date'].dt.year.fillna(0).astype(int)
df = df[df['Year'] != 0]
```

### 4. Extracting Country Information

Extract the country name from the 'Location' column.

```python
from pandasql import sqldf
pysqldf = lambda q: sqldf(q, globals())

q = """
SELECT Country, COUNT(*) as Launches
FROM df
GROUP BY Country
ORDER BY Launches DESC
"""
Launches_country = pysqldf(q)
```

### 5. Space Launches by Country

Count launches per country using SQL queries via pandasql.

```python
from pandasql import sqldf
pysqldf = lambda q: sqldf(q, globals())

q = """
SELECT Country, COUNT(*) as Launches
FROM df
GROUP BY Country
ORDER BY Launches DESC
"""
Launches_country = pysqldf(q)
```

Visualize launches by country.

```python
import matplotlib.pyplot as plt

plt.figure(figsize=(16, 8))
plt.bar(Launches_country['Country'], Launches_country['Launches'])
plt.ylabel('Launches')
plt.xlabel('Country')
plt.title('Number of launches by country')
plt.xticks(rotation=45, ha='right')
plt.show()
```
![Number_launches_country](https://github.com/user-attachments/assets/5e64b41c-d352-45b7-b4db-4b1ba78acd49)

### 6. Space Launches Over Time by Country

Analyze launches per year per country.

```python
q = """
SELECT Year, Country, COUNT(*) as Launches
FROM df
GROUP BY Year, Country
ORDER BY Launches DESC
"""
Launches_country_year = pysqldf(q)
```

### 7. Price Evolution of Missions

Analyze and visualize the price of space missions over time.

```python
q = """
WITH Avg_Price_Per_Year AS (
    SELECT Year, ROUND(AVG(Price), 2) as Average_Price
    FROM df
    WHERE Price > 0
    GROUP BY Year
    ORDER BY Average_Price ASC
)
SELECT *
FROM Avg_Price_Per_Year
"""
Avg_Price_Per_Year = pysqldf(q)
Avg_Price_Per_Year
"""
plt.figure(figsize=(16, 8))

plt.bar(Avg_Price_Per_Year['Year'], Avg_Price_Per_Year['Average_Price'])
plt.title('Price evolution over the years')
plt.xlabel('Year')
plt.ylabel('Price')
plt.show()
```
![pricesavg](https://github.com/user-attachments/assets/6949cfd4-fecd-4853-99f0-8c6754da5494)


### 8. Launches by Month

Extract month from the date column and analyze launch frequency.

```python
df['Month'] = df['Date'].dt.month

q = """
SELECT Month, COUNT(*) as Launches
FROM df
GROUP BY Month
ORDER BY Month DESC
"""
Launches_Month = pysqldf(q)
```

Plot launches by month.

```python
plt.figure(figsize=(16, 8))
plt.bar(Launches_Month['Month'], Launches_Month['Launches'])
plt.xlabel('Month')
plt.ylabel('Launches')
plt.title('Launches by Month')
plt.show()
```

![months](https://github.com/user-attachments/assets/721f2c5b-736e-477e-9cbf-8ecf4fd5e4f4)

### 9. Mission Risk Analysis

Calculate success, failure, and partial failure rates over time.

```python
q = """
SELECT Year,
SUM(CASE WHEN Mission_Status = 'Success' THEN 1 ELSE 0 END) as Success,
SUM(CASE WHEN Mission_Status = 'Failure' THEN 1 ELSE 0 END) as Failure,
SUM(CASE WHEN Mission_Status = 'Partial Failure' THEN 1 ELSE 0 END) as Partial_Failure,
COUNT(*) as Total,
ROUND(SUM(CASE WHEN Mission_Status = 'Success' THEN 1 ELSE 0 END) * 100.0 / COUNT(*), 2) as Success_Rate,
ROUND(SUM(CASE WHEN Mission_Status = 'Failure' THEN 1 ELSE 0 END) * 100.0 / COUNT(*), 2) as Failure_Rate,
ROUND(SUM(CASE WHEN Mission_Status = 'Partial Failure' THEN 1 ELSE 0 END) * 100.0 / COUNT(*), 2) as Partial_Failure_Rate
FROM df
GROUP BY Year
ORDER BY Year ASC
"""
Mission_Risk = pysqldf(q)
```

Visualize mission risk over time.

```python
plt.figure(figsize=(16, 8))
plt.plot(Mission_Risk['Year'], Mission_Risk['Success_Rate'], marker='o', label='Success Rate')
plt.plot(Mission_Risk['Year'], Mission_Risk['Failure_Rate'], marker='o', label='Failure Rate')
plt.plot(Mission_Risk['Year'], Mission_Risk['Partial_Failure_Rate'], marker='o', label='Partial Failure Rate')
plt.xlabel('Year')
plt.ylabel('Percentage')
plt.title('Mission Risk Over Time')
plt.legend()
plt.show()
```

![risk](https://github.com/user-attachments/assets/b24e27bb-b165-4130-8dae-1c10a7793665)









