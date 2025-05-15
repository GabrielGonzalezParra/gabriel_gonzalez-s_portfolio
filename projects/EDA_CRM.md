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

---

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

---

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

The strategy we could follow to handle the missing values is to replace them with the median, as it is representative of typical behavior, simple, effective, and avoids bias due to missing data.

---

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

---


```python

# We visualize the histogram of the "Age" variable
plt.figure(figsize=(12, 10))
plt.hist(data['Age'], bins=25);

```

![image](https://github.com/user-attachments/assets/cf12bdc4-84cd-4f21-8e11-3b91dc98760b)


The most common age is around 50 years old. The youngest individuals are around 18 years old, and the oldest are around 85. There are anomalous values, as there are individuals over 120 years old, which is impossible in reality and is likely an incorrect value.

---

```python

# We plot the histogram of the "MntWines" variable

plt.figure(figsize=(12, 10))
plt.hist(data['MntWines'], bins=25);

```

![image](https://github.com/user-attachments/assets/3dd3c61e-c6eb-4473-af36-609ac88a185e)

The histogram shows that the vast majority of users spend small amounts on wine, with the distribution of higher spending amounts gradually decreasing—that is, large amounts of wine purchases are less frequent in the overall distribution.

---

```python

# We create the correlation matrix between the variables
correlation_matrix = data.corr(method='pearson')

# Create a heatmap using seaborn
plt.figure(figsize=(15, 10))
sns.heatmap(correlation_matrix, annot=True, cmap='coolwarm', vmin=-1, vmax=1)
plt.title('Correlation Matrix (Values Above Threshold)')
plt.show()

```

![image](https://github.com/user-attachments/assets/2d575ee3-b254-4404-b7c4-fcb54c37eea9)


The correlation matrix shows the linear relationship between variables. The values range from -1 to 1. The closer a value is to 1, the stronger the positive linear relationship—meaning that if one variable increases, the other tends to increase as well. If the value is close to -1, the relationship is negative, meaning that when one variable increases, the other tends to decrease.

There is a strong relationship between the variables MntWines and MntTotalSpent, which indicates that wine consumption has a significant influence on the increase in total spending.

There is also a high correlation between NumStorePurchases and NumTotalPurchases, but this relationship is expected, as in-store purchases are part of the total purchases.

We can also find a high correlation between MntMeatProducts and MntTotalSpent, which is a similar case to what happens with wine.

On the other hand, there is no strong negative relationship between any of the variables. The most notable negative correlation is between Income and NumWebVisitsMonth, which suggests that the higher the annual income, the fewer monthly visits to the website.

---

```python

# We visualize the outliers in each variable using a boxplot
plt.figure(figsize=(15, 8))
sns.boxplot(data=data)
plt.xticks(rotation=45)  # Rotate x-axis labels for better visualization
plt.title('Boxplot for Outlier Detection')
plt.show()

```

![image](https://github.com/user-attachments/assets/e5fba62a-b719-4116-ba8f-e2c27d21a207)


There are outliers in all variables except ID, Recency, NumStorePurchases, and NumTotalPurchases.


```python
# We generate statistics for the variables that contain outliers
Outliers = ['Income', 'MntWines', 'MntFruits', 'MntMeatProducts', 'MntFishProducts', 'MntSweetProducts',
            'MntGoldProds', 'NumWebPurchases', 'NumCatalogPurchases', 'NumWebVisitsMonth', 'Complain',
            'Education_Int', 'Marital_Status_Int', 'Children', 'MntTotalSpent', 'Age', 'Seniority']
data[Outliers].describe()

```

| Statistic     | Income      | MntWines   | MntFruits | MntMeatProducts | MntFishProducts | MntSweetProducts | MntGoldProds | NumWebPurchases | NumCatalogPurchases | NumWebVisitsMonth | Complain | Education_Int | Marital_Status_Int | Children | MntTotalSpent | Age   | Seniority |
|---------------|-------------|------------|-----------|------------------|------------------|-------------------|---------------|------------------|----------------------|--------------------|----------|----------------|---------------------|----------|----------------|--------|-----------|
| **Count**     | 2216.000000 | 2240.000000 | 2240.000000 | 2240.000000       | 2240.000000       | 2240.000000        | 2240.000000    | 2240.000000       | 2240.000000           | 2240.000000         | 2240.000000 | 2240.000000    | 2240.000000         | 2240.000000 | 2240.000000     | 2240.000000 | 2240.000000 |
| **Mean**      | 52247.251354 | 303.935714 | 26.302232  | 166.950000        | 37.525446         | 27.062946          | 44.021875     | 4.084821          | 2.662054              | 5.316518            | 0.009375 | 2.460268       | 1.480357            | 0.950446 | 605.798214     | 54.194196 | 1.971875   |
| **Std**       | 25173.076661 | 336.597393 | 39.773434  | 225.715373        | 54.628979         | 41.280498          | 52.167439     | 2.778714          | 2.923101              | 2.426645            | 0.096391 | 1.004337       | 1.047154            | 0.751803 | 602.249288     | 11.984069 | 0.684554   |
| **Min**       | 1730.000000  | 0.000000   | 0.000000   | 0.000000          | 0.000000          | 0.000000           | 0.000000      | 0.000000          | 0.000000              | 0.000000            | 0.000000 | 0.000000       | 0.000000            | 0.000000 | 5.000000       | 27.000000 | 1.000000   |
| **25%**       | 35303.000000 | 23.750000  | 1.000000   | 16.000000         | 3.000000          | 1.000000           | 9.000000      | 2.000000          | 0.000000              | 3.000000            | 0.000000 | 2.000000       | 1.000000            | 0.000000 | 68.750000      | 46.000000 | 2.000000   |
| **50%**       | 51381.500000 | 173.500000 | 8.000000   | 67.000000         | 12.000000         | 8.000000           | 24.000000     | 4.000000          | 2.000000              | 6.000000            | 0.000000 | 2.000000       | 2.000000            | 1.000000 | 396.000000     | 53.000000 | 2.000000   |
| **75%**       | 68522.000000 | 504.250000 | 33.000000  | 232.000000        | 50.000000         | 33.000000          | 56.000000     | 6.000000          | 4.000000              | 7.000000            | 0.000000 | 3.000000       | 2.000000            | 1.000000 | 1045.500000    | 64.000000 | 2.000000   |
| **Max**       | 666666.000000| 1493.000000| 199.000000 | 1725.000000       | 259.000000        | 263.000000         | 362.000000    | 27.000000         | 28.000000             | 20.000000           | 1.000000 | 4.000000       | 4.000000            | 3.000000 | 2525.000000    | 130.000000| 3.000000   |


The impact of outliers can include, first of all, the distortion of descriptive statistics, as the mean is affected by these values, and although the median and quartiles are more robust, they are also influenced. This also affects the interpretation of graphs and histograms, making them harder to interpret and less visually clear. Finally, the presence of outliers can also affect statistical models, skewing certain coefficients.

---

Once we have examined how the data is distributed, identified any missing or anomalous values, and carried out the necessary cleaning and transformations, we will conduct a more in-depth analysis of the variables we believe provide relevant insights into our customers’ behavior and characteristics. This information may include: spending based on age, the preferred sales channel used, or the education level of those who purchase a specific type of product.


```python

# We analyze the education level of the customers. To do this, we visualize it with a pie chart.
counts = data['Education_Int'].value_counts()

# We add the legend
labels = ['Grado', 'PhD', 'Master', 'Secundaria', 'Basica']
plt.pie(counts, labels=labels, autopct='%1.1f%%')

# Add the title
plt.title('Distribucion educativa de nuestros clientes')

# Draw the chart
plt.show()

![image](https://github.com/user-attachments/assets/6f23f362-e2ea-450f-ac2a-76ab293ade93)

```pyton

# We analyze the marital status of the customers. To do this, we visualize it with a pie chart
counts = data['Marital_Status_Int'].value_counts()

# Add the legend
labels = ['Soltero', 'Pareja de hecho', 'Casado', 'Divorciado', 'Viudo']
plt.pie(counts, labels=labels, autopct='%1.1f%%')
# Add the title
plt.title('Estado civil')
# Plot the chart
plt.show()

```
![image](https://github.com/user-attachments/assets/15f5ccc4-064b-420e-9f00-ca667159c65c)

```python

#Analizamos la relación entre el estado civil del cliente y el gasto y lo representamos gráficamente.
To_Plot = [ "Marital_Status_Int", "MntTotalSpent"]
print("Relación entre las variables seleccionadas")
plt.figure()
# Crear un pairplot con las combinaciones de las variables seleccionadas
sns.pairplot(data[To_Plot])
# Mostrar el pairplot
plt.show()

```

``` python

# We analyze the relationship between the number of children and total spending. We visualize it with a boxplot

# We set the figure size
plt.figure(figsize = (10, 6))

# We create the violin plot
sns.violinplot(x = 'Children', y = 'MntTotalSpent', data = data, palette = 'cool')

# We define the title of the figure and each axis
plt.title('Distribucion del gasto en función del número de hijos en la familia')
plt.xlabel('Nº Hijos')
plt.ylabel('Gasto Total')

plt.show()

```

![image](https://github.com/user-attachments/assets/01251823-5fe4-42d8-9655-504938dd6218)


```python

# We analyze the relationship between household income (Income) and spending (MntTotalSpent). We visualize it with a scatter plot

# Set the figure size
plt.figure(figsize = (10, 6))

# Create the scatter plot
plt.scatter(data['Income'], data['MntTotalSpent'], color = 'blue', alpha = 0.1)

# Set the axis labels
plt.xlabel('Income')
plt.ylabel('MntTotalSpent')

# Set the chart title
plt.title('Income vs MntTotalSpent')
plt.show()

```

![image](https://github.com/user-attachments/assets/b89322ad-6fe0-4153-abbc-9b8447745092)


```python

# We analyze the relationship between the client's education level and total spending
sns.barplot(x = data['Education_Int'],y = data['MntTotalSpent']);
plt.title('Gasto total en función del nivel educativo del cliente');

```

![image](https://github.com/user-attachments/assets/4e542e20-a707-4e72-a88a-ad8b2bc8d659)

```python
# We analyze the relationship between spending and customer complaints.
sns.barplot(x = data['Complain'],y = data['MntTotalSpent'].mean());
plt.title('Gasto medio en función de si el cliente ha presentado o no reclamaciones');

```

![image](https://github.com/user-attachments/assets/bd6414f8-21c4-4da9-9ae6-61f7b77a9184)

```python

# We analyze sales by channel. We visualize it with a bar chart
suma_columna1 = data['NumWebPurchases'].sum()
suma_columna2 = data['NumCatalogPurchases'].sum()
suma_columna3 = data['NumStorePurchases'].sum()

# Create a DataFrame with the sums of each column
df_suma = pd.DataFrame({'Columna': ['NumWebPurchases', 'NumCatalogPurchases', 'NumStorePurchases'],
                        'Suma': [suma_columna1, suma_columna2, suma_columna3]})

# Create the bar chart for the sum of each column
plt.figure(figsize=(10, 6))
df_suma.plot(x='Columna', y='Suma', kind='bar', color='skyblue')
plt.xlabel('Canal')
plt.ylabel('Nº de Ventas')
plt.title('Nº de Ventas totales en cada canal')
plt.show()

```

Total Spending Based on the Customer's Education Level
Customers with higher education levels tend to spend more. It can be seen that education categories 2, 3, and 4 have significantly higher spending compared to customers with a lower education level (level 0). This could suggest that customers with higher education levels have greater purchasing power or are more likely to make larger purchases.

Total Number of Sales by Channel
This shows the total number of sales across three different channels: web, catalog, and physical store. It is observed that in-store purchases (NumStorePurchases) exceed both online purchases (NumWebPurchases) and catalog purchases (NumCatalogPurchases). This indicates a clear customer preference for shopping in person.

Total Spending Based on Number of Children
Customers without children (0) show significantly higher spending compared to those with 1, 2, or 3 children. As the number of children increases, both the average spending and the variability in spending decrease notably.

This could be due to higher disposable income in households without dependents, or due to different consumption preferences, prioritizing essential goods and purchasing a higher quantity of fewer items rather than diversifying their shopping basket.

Spending by Product Type
Based on the total sales of each product, wine and meat stand out as the top products, with wine being the clear favorite. This might indicate that wine consumers also tend to be meat consumers.

```python

# Analysis of the relationship between Recency and Marital Status.

marital_status_labels = {
    0: 'Single',
    1: 'Married',
    2: 'Widow',
    3: 'Divorced',
    4: 'Together'
}
data['Marital_Status_Label'] = data['Marital_Status_Int'].map(marital_status_labels)

plt.figure(figsize=(10, 6))
sns.boxplot(x='Marital_Status_Label', y='Recency', data=data)
plt.title('Recency vs. Marital Status')
plt.xlabel('Marital Status')
plt.ylabel('Recency')
plt.show()

#Chart

plt.figure(figsize=(10,6))
sns.violinplot(x='Marital_Status_Label', y='Recency', data=data)
plt.title('Recency vs. Marital Status (Violin Plot)')
plt.xlabel('Marital Status')
plt.ylabel('Recency')
plt.show()
print(data.groupby('Marital_Status_Label')['Recency'].describe())

```
![image](https://github.com/user-attachments/assets/9a17daf2-a8df-4589-bcd8-3b80dfed7c99)
![image](https://github.com/user-attachments/assets/fa88e572-65cd-463d-908e-a4ab48483bde)

```
                      count       mean        std  min    25%   50%    75%  \
Marital_Status_Label                                                         
Divorced              232.0  49.487069  28.728612  0.0  25.75  51.0  75.25   
Married               580.0  50.106897  28.546099  0.0  26.00  51.0  75.00   
Single                485.0  49.195876  28.697712  0.0  25.00  50.0  74.00   
Together               77.0  49.142857  28.771657  0.0  28.00  48.0  75.00   
Widow                 866.0  48.288684  29.503519  0.0  23.00  48.0  73.00   

                       max  
Marital_Status_Label        
Divorced              99.0  
Married               99.0  
Single                99.0  
Together              99.0  
Widow                 99.0

```

```python

# Analysis of the relation between Marital Status and Seniority

marital_status_labels = {
    0: 'Single',
    1: 'Married',
    2: 'Widow',
    3: 'Divorced',
    4: 'Together'
}
data['Marital_Status_Label'] = data['Marital_Status_Int'].map(marital_status_labels)

# Chart
plt.figure(figsize=(10, 6))
sns.boxplot(data=data, x='Marital_Status_Label', y='Seniority')
plt.title('Relación entre Seniority y Estado Civil')
plt.xlabel('Estado Civil')
plt.ylabel('Antigüedad del Cliente (Seniority)')
plt.tight_layout()
plt.show()

```

![image](https://github.com/user-attachments/assets/26eadf35-a5fe-485b-be43-c874d2960a18)


``` python

# Analysis of the relation between the frequency and the number of monthly visits

plt.figure(figsize=(8, 5))
sns.histplot(data['NumWebVisitsMonth'], bins=10, kde=True)
plt.title('Frecuencia de Visitas Web Mensuales')
plt.xlabel('Número de Visitas Web por Mes')
plt.ylabel('Frecuencia')
plt.tight_layout()
plt.show()

```

![image](https://github.com/user-attachments/assets/d4d34a37-cb7e-4e49-bc9f-0616a1c0c025)


**1. Customer Typology**
Most of our customers are between 36 and 65 years old, not married or without a registered partner (combining widowed, single, and divorced into one category), have at least one child, and possess a high level of education.

**2. Customer Behavior**
Wine and meat are the star products, with wine being by far the clear favorite. Most customers prefer to shop in physical stores or spend more there, making it the predominant channel. On the other hand, we have found that most customers visit the website between 4 and 8 times per month. Finally, we have observed that customers with a partner but not married have the highest customer seniority.

**Conclusions**

We observe that a higher level of education correlates positively with higher spending.

Although customers without children tend to spend more on average, the median expenditure is higher among families with children, making this segment particularly worth considering.

Income is not a determining factor for spending levels, as the data shows that higher income does not imply higher expenditure.

Customers spend significantly more in physical stores compared to online.

The seniority variable suggests that most customers have been acquired recently or do not have a long-standing relationship with the brand. The most loyal customers are those in a registered partnership, followed by married customers. This is confirmed by our analysis of marital status and the recency variable, as they exhibit the lowest recency values.

Customers who have submitted complaints tend to have lower incomes.
