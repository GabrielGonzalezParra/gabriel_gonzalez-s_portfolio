# Exploratory data analysis (EDA)

## Initial data exploration

### Load the datasets
The first step involves loading the datasets into the analysis environment. This includes the following datasets:
- House price index by region (2007-2023)
- Consumer price index by region (2007-2023)
- Average salaries by region (2007-2023)
- Number of house sales by region (2007-2023)

```python
import pandas as pd

house_prices = pd.read_csv('housing_price_index.csv', delimiter=';')
prices = pd.read_csv('prices_index.csv', delimiter=';')
salaries = pd.read_csv('salaries_avg.csv', delimiter=';')
sales = pd.read_csv('houses_sales.csv', delimiter=';')
```

### Inspect the first few rows of each dataset
After loading the datasets, we inspect the first few rows using the `.head()` method. This gives us a preliminary look at the structure of the data, including the column names, data types, and a few initial values.

```python
print(house_prices.head())
print(prices.head())
print(salaries.head())
print(sales.head())
```
#### house_prices

| Region       | Period  | Total  |
|--------------|---------|--------|
| 01 Andalucía | 2023T4 | 5,3    |
| 01 Andalucía | 2023T3 | 5,8    |
| 01 Andalucía | 2023T2 | 4,5    |
| 01 Andalucía | 2023T1 | 4,2    |
| 01 Andalucía | 2022T4 | 6,5    |

#### prices

| Region       | Period  | Total  |
|--------------|---------|--------|
| 01 Andalucía | 2023M12 | 3,3    |
| 01 Andalucía | 2023M11 | 3,5    |
| 01 Andalucía | 2023M10 | 3,8    |
| 01 Andalucía | 2023M09 | 3,8    |
| 01 Andalucía | 2023M08 | 2,9    |

#### salaries

| Region       | Period  | Total     |
|--------------|---------|-----------|
| 01 Andalucía | 2023    | 2.100,74  |
| 01 Andalucía | 2022    | 1.906,75  |
| 01 Andalucía | 2021    | 1.864,47  |
| 01 Andalucía | 2020    | 1.837,33  |
| 01 Andalucía | 2019    | 1.773,03  |

#### sales

| Region       | Period  | Total   |
|--------------|---------|---------|
| 01 Andalucía | 2023M12 | 7.498   |
| 01 Andalucía | 2023M11 | 9.939   |
| 01 Andalucía | 2023M10 | 8.809   |
| 01 Andalucía | 2023M09 | 8.263   |
| 01 Andalucía | 2023M08 | 10.333  |

The "Period" column in the datasets is recorded in different formats: quarterly (Q), monthly (M), and yearly.
This needs to be standardized for consistent analysis.

### Review the data types of each column
Using the `.info()` method, we review the data types of each column to ensure they are appropriate for the analysis. This step helps in identifying any data type mismatches that may need correction, such as numerical values stored as strings or dates that are not in datetime format.

## 2. Data cleaning

### Identify and handle missing values

#### Count missing values per column
We begin by counting the number of missing values in each column.

```python
print(house_prices.isnull().sum())
print(prices.isnull().sum())
print(salaries.isnull().sum())
print(sales.isnull().sum())
```
#### house_prices (null values)

| Column  | Missing Values |
|---------|----------------|
| Region  | 0              |
| Period  | 0              |
| Total   | 0              |

#### prices (null values)

| Column  | Missing Values |
|---------|----------------|
| Region  | 0              |
| Period  | 0              |
| Total   | 0              |

#### salaries (null values)

| Column  | Missing Values |
|---------|----------------|
| Region  | 0              |
| Period  | 0              |
| Total   | 0              |

#### sales (null values)

| Column  | Missing Values |
|---------|----------------|
| Region  | 0              |
| Period  | 0              |
| Total   | 0              |

There is no need for imputation or removal of rows/columns due to missing data. 

### Identifying duplicates
After checking the null values, we continue by identifying the duplicated values. 

```python
print(house_prices.duplicated().sum())
print(prices.duplicated().sum())
print(salaries.duplicated().sum())
print(sales.duplicated().sum())
```

There aren't duplicates in any of the four datasets. 

### Standardize the period column to yearly data
The "Period" column dates are given in three different formats (quarter, month, and year). We will standardize this column to yearly data by calculating the mean of the quarterly and monthly data.

#### First, we change the 'Period' column for a 'year' column. 

```python
# Extract the year from the 'Period' column and create a new 'year' column
house_prices['year'] = house_prices['Period'].str[:4]

# Drop the 'Period' column
house_prices = house_prices.drop(columns=['Period'])

# Display the updated DataFrame
house_prices.head()
```

```python

# Extract the year from the 'Period' column and create a new 'year' column
prices['year'] = prices['Period'].str[:4]

# Drop the 'Period' column
prices = prices.drop(columns=['Period'])

# Display the updated DataFrame
prices.head()
```

```python

# Extract the year from the 'Period' column and create a new 'year' column
prices['year'] = prices['Period'].str[:4]

# Drop the 'Period' column
prices = prices.drop(columns=['Period'])

# Display the updated DataFrame
prices.head()
```

The 'salaries' Dataset already contains yearly data so we don't need to change it. 

#### Then, we calculate the main of the quarterly and monthly data for the 'house_prices' and 'prices' tables and the summatory for the in order to switch the data type to yearly. 

```python
# Change the 'Total' column to float
house_prices['Total']=house_prices['Total'].astype(str)
house_prices['Total'] = house_prices['Total'].str.replace(',', '.').astype(float)
```

```python
# Create a new DataFrame containing the yearly house prices index value
house_prices_yearly = house_prices.groupby(['year', 'Region'])['Total'].mean().reset_index()
```

```python
# Round the values to two decimal places
house_prices_yearly = house_prices_yearly.round(2)
````

#### 'house_prices' table

```python
# Change the 'Total' column to float
house_prices['Total']=house_prices['Total'].astype(str)
house_prices['Total'] = house_prices['Total'].str.replace(',', '.').astype(float)

# Create a new DataFrame containing the yearly house prices index value
house_prices_yearly = house_prices.groupby(['year', 'Region'])['Total'].mean().reset_index()

# Round the values to two decimal places
house_prices_yearly = house_prices_yearly.round(2)
````

#### 'prices' table

```python
# Change the 'Total' column to float
prices['Total'] = prices['Total'].astype(str)
prices['Total'] = prices['Total'].str.replace(',', '.').astype(float)

# Create a new DataFrame containing the yearly costumer prices index value
prices_yearly = prices.groupby(['year', 'Region'])['Total'].mean().reset_index()

# Round the values to two decimal places
prices_yearly.round(2)
```

#### 'sales' table

```python
# Change the 'Total' column to integer
sales['Total'] = sales['Total'].astype(int)

# Create a new DataFrame containing the yearly number of houses sold
sales_yearly = sales.groupby(['year', 'Region'])['Total'].sum().reset_index()
```









