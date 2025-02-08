# Exploratory data analysis (EDA)

## 1. Initial data exploration

### 1.1: Load the datasets
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

### 1.2: Inspect the first few rows of each dataset
After loading the datasets, we inspect the first few rows using the `.head()` method. This gives us a preliminary look at the structure of the data, including the column names, data types, and a few initial values.

### Task 1.3: Obtain a Statistical Summary of the Data
We use the `.describe()` method to generate a summary of the central tendency, dispersion, and shape of the dataset’s distribution. This includes metrics such as mean, median, standard deviation, minimum, maximum, and quartiles.

### Task 1.4: Review the Data Types of Each Column
Using the `.info()` method, we review the data types of each column to ensure they are appropriate for the analysis. This step helps in identifying any data type mismatches that may need correction, such as numerical values stored as strings or dates that are not in datetime format.

## 2. Data Cleaning

### Task 2.1: Identify and Handle Missing Values
#### Task 2.1.1: Count Missing Values Per Column
We begin by counting the number of missing values in each column. This helps in understanding the extent of missing data and determining the necessary steps to handle it.

#### Task 2.1.2: Decide on a Strategy for Handling Missing Values
Based on the extent and nature of the missing values, we decide on a strategy to handle them. This could involve removing rows or columns with excessive missing values, imputing missing values using statistical methods, or using domain-specific knowledge to fill in gaps.

### Task 2.2: Verify and Correct Inconsistencies in the Data
We check for inconsistencies in the data, such as incorrect date formats, units of measurement, or out-of-range values. These inconsistencies are corrected to ensure data quality and reliability.

### Task 2.3: Remove Duplicate Entries
We identify and remove any duplicate rows in the datasets to prevent biased analysis results. Duplicate entries can arise from data collection or merging processes and need to be addressed.


