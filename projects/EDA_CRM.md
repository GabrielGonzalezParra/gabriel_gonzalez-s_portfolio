# Descriptive Data Analysis (EDA) of a CRM
In this activity, we will perform a descriptive analysis of the data from GoodTasteMarket's CRM.

GoodTasteMarket is a U.S.-based supermarket chain with a strong presence in northern Europe, focused on food products. In order to redefine its business strategies and launch new marketing campaigns, GoodTasteMarket has initiated a project to analyze data from its CRM to gain useful insights into customer behavior and make informed decisions in the area of the marketing mix.

For this analysis, we have a dataset in .csv format called marketing_campaign.csv with the following description:

## Dataset Description:
ID: Unique customer identifier

Age: Customer's age

Education: Customer's education level
0 - 'Basic'
1 - 'Secondary'
2 - 'Bachelor's'
3 - 'Master's'
4 - 'Doctorate - PhD'

Marital_Status: Customer's marital status
0 - 'Single'
1 - 'Domestic Partnership'
2 - 'Married'
3 - 'Divorced'
4 - 'Widowed'

Income: Customer's annual household income

Children: Number of children in the customer's household

Seniority: Customer's tenure with the company

Recency: Number of days since the customer's last purchase

Complain: Customer complaints in the past 2 years (1 - if they have made any, 0 - if they have not made any)

MntWines: Amount spent on wine in the past 2 years

MntFruits: Amount spent on fruit in the past 2 years

MntMeatProducts: Amount spent on meat in the past 2 years

MntFishProducts: Amount spent on fish in the past 2 years

MntSweetProducts: Amount spent on sweets in the past 2 years

MntGoldProds: Amount spent on premium products in the past 2 years

MntTotalSpent: Total amount spent on any product

NumWebPurchases: Number of purchases made through the company's website

NumCatalogPurchases: Number of purchases made through catalog

NumStorePurchases: Number of purchases made through physical stores

NumTotalPurchases: Total number of purchases made through any channel

NumWebVisitsMonth: Number of visits to the company's website in the last month

```python

#We import the necessary libraries

import numpy as np
import pandas as pd
from matplotlib import pyplot as plt
import seaborn as sns
import plotly.express as px
```

```python

#We load the dataset

from google.colab import files
uploaded = files.upload()
```

```python

# We read the dataset

import io
data = pd.read_csv(io.BytesIO(uploaded['marketing_campaign.csv']), sep=',')
data.head(10)
```

| ID  | Income | Recency | MntWines | MntFruits | MntMeatProducts | MntFishProducts | MntSweetProducts | MntGoldProds | NumWebPurchases | NumStorePurchases | NumWebVisitsMonth | Complain | Education_Int | Marital_Status_Int | Children | MntTotalSpent | Age | NumTotalPurchases | Seniority |
| --- | ------ | ------- | -------- | --------- | ---------------- | --------------- | ---------------- | ------------ | ---------------- | ------------------ | ----------------- | -------- | ------------- | ------------------ | -------- | ------------- | --- | ------------------ | --------- |
| 0   | 5524   | 58138.0 | 58       | 635       | 88               | 546             | 172              | 88           | 8                | 4                  | 7                 | 0        | 2             | 0                  | 0        | 1617          | 66  | 22                | 3         |
| 1   | 2174   | 46344.0 | 38       | 11        | 1                | 6               | 2                | 1            | 6                | 2                  | 5                 | 0        | 2             | 0                  | 2        | 27            | 69  | 4                 | 1         |
| 2   | 4141   | 71613.0 | 26       | 426       | 49               | 127             | 111              | 21           | 42               | 8                  | 10                | 4        | 0             | 2                  | 1        | 776           | 58  | 20                | 2         |
| 3   | 6182   | 26646.0 | 26       | 11        | 4                | 20              | 10               | 3            | 5                | 2                  | 4                 | 6        | 0             | 2                  | 1        | 53            | 39  | 6                 | 1         |
| 4   | 5324   | 58293.0 | 94       | 173       | 43               | 118             | 46               | 27           | 15               | 5                  | 6                 | 5        | 0             | 4                  | 2        | 422           | 42  | 14                | 1         |
| 5   | 7446   | 62513.0 | 16       | 520       | 42               | 98              | 0                | 42           | 14               | 6                  | 10                | 6        | 0             | 3                  | 1        | 716           | 56  | 20                | 2         |
| 6   | 965    | 55635.0 | 34       | 235       | 65               | 164             | 50               | 49           | 27               | 7                  | 7                 | 6        | 0             | 2                  | 3        | 590           | 52  | 17                | 3         |
| 7   | 6177   | 33454.0 | 32       | 76        | 10               | 56              | 3                | 1            | 23               | 4                  | 4                 | 8        | 0             | 4                  | 2        | 169           | 38  | 8                 | 2         |
| 8   | 4855   | 30351.0 | 19       | 14        | 0                | 24              | 3                | 3            | 2                | 3                  | 2                 | 9        | 0             | 4                  | 1        | 46            | 49  | 5                 | 2         |
| 9   | 5899   | 5648.0  | 68       | 28        | 0                | 6               | 1                | 13           | 1                | 0                  | 20                | 0        | 4             | 1                  | 2        | 49            | 73  | 1                 | 1         |


```python
# We show the number of columns and rows of the dataset

data.shape
```

(2240, 21)

First, there are missing values in the Income variable, as there are 2,216 values counted, while the other variables have 2,240 values. Regarding the means, there is a significant difference between the third quartile and the maximum in the Income variable, which can distort the mean. A similar situation occurs in MntWines and MntMeatProducts, where the mean deviates significantly from the median due to consumers who spend much more than the average.

On the other hand, outliers may be caused by extreme maximum values that are far removed from the third quartiles, as seen in Income, MntMeatProducts, MntGoldProds, MntTotalSpent, NumWebPurchases, and NumWebVisitsMonth. Another maximum that suggests it could be an outlier is the 130-year value for the Age variable.

Finally, looking at the quartiles, we find the Children variable relevant, as the majority of individuals have 1 child. In the Age variable, we also have a large portion of users between the ages of 46 and 64.

```python

# We generate statistics for a specific variable, the "Income" variable

data['Income'].describe()
```

| Statistic | Income         |
|-----------|----------------|
| count     | 2216.000000    |
| mean      | 52247.251354   |
| std       | 25173.076661   |
| min       | 1730.000000    |
| 25%       | 35303.000000   |
| 50%       | 51381.500000   |
| 75%       | 68522.000000   |
| max       | 666666.000000  |



The mean is higher than the median, which may occur because it is affected by one or more outliers. 

The median (51,382) is very close to the mean, which usually suggests symmetry, but due to the extreme maximum value, we know there are values that are out of the ordinary. 

The income range for the middle 50% is between Q1 and Q3, that is, between €35,303 and €68,522. 

However, outliers can be observed, as although most incomes are clustered below €70,000, the maximum value of €666,666 is extremely high and represents a clear outlier, which likely affects both the mean and the standard deviation, increasing both values.


```python

# We check what type of variables the dataset contains and which values are not null
data.info()

```

| #   | Column               | Non-Null Count | Dtype   |
|-----|----------------------|----------------|---------|
| 0   | ID                   | 2240 non-null  | int64   |
| 1   | Income               | 2216 non-null  | float64 |
| 2   | Recency              | 2240 non-null  | int64   |
| 3   | MntWines             | 2240 non-null  | int64   |
| 4   | MntFruits            | 2240 non-null  | int64   |
| 5   | MntMeatProducts      | 2240 non-null  | int64   |
| 6   | MntFishProducts      | 2240 non-null  | int64   |
| 7   | MntSweetProducts     | 2240 non-null  | int64   |
| 8   | MntGoldProds         | 2240 non-null  | int64   |
| 9   | NumWebPurchases      | 2240 non-null  | int64   |
| 10  | NumCatalogPurchases  | 2240 non-null  | int64   |
| 11  | NumStorePurchases    | 2240 non-null  | int64   |
| 12  | NumWebVisitsMonth    | 2240 non-null  | int64   |
| 13  | Complain             | 2240 non-null  | int64   |
| 14  | Education_Int        | 2240 non-null  | int64   |
| 15  | Marital_Status_Int   | 2240 non-null  | int64   |
| 16  | Children             | 2240 non-null  | int64   |
| 17  | MntTotalSpent        | 2240 non-null  | int64   |
| 18  | Age                  | 2240 non-null  | int64   |
| 19  | NumTotalPurchases    | 2240 non-null  | int64   |
| 20  | Seniority            | 2240 non-null  | int64   |

dtypes: float64(1), int64(20)  
memory usage: 367.6 KB


All variables are numeric, 20 of them contain integer values, except for the "Income" variable, which contains decimals. Additionally, only that variable has null values, specifically 24.


```python

We choose the "Complain" variable to see the different values it takes and determine what type of variable it is
print('Complain: ', data['Complain'].unique())

```

Complain:  [0 1]

As can be seen, the "Complain" variable is categorical and binary, as it only contains the values 0 and 1.

```python

# We visualize the histograms of the numerical variables to understand their distribution

data.hist(figsize=(16, 20), bins=50, xlabelsize=8, ylabelsize=8);

```

![image](https://github.com/user-attachments/assets/e9cc2f5c-2d0d-446c-ab13-1b0d31ced2e2)


```python

# We visualize the histogram of the "Age" variable
plt.figure(figsize=(12, 10))
plt.hist(data['Age'], bins=25);

```

![image](https://github.com/user-attachments/assets/cf12bdc4-84cd-4f21-8e11-3b91dc98760b)


The most common age is around 50 years old. The youngest individuals are around 18 years old, and the oldest are around 85. There are anomalous values, as there are individuals over 120 years old, which is impossible in reality and is likely an incorrect value.

```python

# We plot the histogram of the "MntWines" variable

plt.figure(figsize=(12, 10))
plt.hist(data['MntWines'], bins=25);

```

![image](https://github.com/user-attachments/assets/3dd3c61e-c6eb-4473-af36-609ac88a185e)

The histogram shows that the vast majority of users spend small amounts on wine, with the distribution of higher spending amounts gradually decreasing—that is, large amounts of wine purchases are less frequent in the overall distribution.




