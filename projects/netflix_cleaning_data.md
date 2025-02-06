# Netflix data cleaning project

This personal project aims to clean a dataset of Netflix titles by handling nulls and duplicates, removing unnecessary columns, and splitting columns to increase their utility. We use both Python and SQL to perform the data cleaning.

## Importing libraries and dataset

First, we import the necessary libraries and load the dataset.

```python
import pandas as pd
import seaborn as sns
from sqlalchemy import engine
from pandasql import sqldf
pysqldf = lambda q: sqldf(q, globals())

df = pd.read_csv("netflix_titles.csv")
df
```

## Handling null values

We check how many null values are in the dataset and in which columns they are located. Then, we replace the null values with the message 'Not given' to maintain the number of rows in the dataset.

```python
df.isnull().sum()
df.fillna("Not given", inplace=True)
df.isnull().sum()
```

## Checking for duplicates

We check if there are any duplicate rows in the dataset. No duplicates were found.

```python
df.duplicated().sum()
```

## Handling null values in the 'director' column

We investigate the most common collaborations between directors and movie casts. Then, we update the rows where the director is 'Not given' with the most common director for that cast.

```python
common_collaborations = df.groupby(['director', 'cast']).size().reset_index(name='count')
common_collaborations = common_collaborations.sort_values('count', ascending=False)
common_collaborations.head(30)

df.rename(columns={'cast': 'movie_cast'}, inplace=True)

query = """
SELECT *
FROM df
WHERE movie_cast = 'David Attenborough'
"""
pysqldf(query)

df.loc[(df['movie_cast'] == 'David Attenborough') & (df['director'] == 'Not given'), 'director'] = 'Alastair Fothergill'
```

## Handling Null Values in the 'country' Column

We analyze the most common combinations of director and country to reduce the number of 'Not given' entries in the 'country' column by populating the most common combinations with directors.

```python
common_country_director = df.groupby(['director', 'country']).size().reset_index(name='count')
common_country_director = common_country_director.sort_values('count', ascending=False)
common_country_director.head(50)

q = """
SELECT director, country, COUNT (*) AS frequence
FROM df
WHERE country = 'Not given'
GROUP BY director, country
ORDER BY director, frequence DESC
"""
freq = pysqldf(q)
freq.head(30).sort_values('frequence', ascending=False)

q = """SELECT *
FROM freq
WHERE frequence > 3
ORDER BY frequence DESC
LIMIT 30"""
pysqldf(q)

q = """SELECT director, country
FROM df
WHERE director IN ('Rajiv Chilaka', 'Suhas Kadav', 'Prakash Satam', 'Hidenori Inoue', 'Joey So', 'Rathindran R Prasad', 'Adriano Rudiman', 'Leigh Janiak' )
AND country != 'Not given'"""
pysqldf(q)

df.loc[(df['director'] == 'Rajiv Chilaka') | (df['director'] == 'Suhas Kadav'), 'country'] = 'India'
df.loc[(df['director'] == 'Hidenori Inoue') , 'country'] = 'Japan'
```

## Removing Rows with 'Not given' Values

We remove the rows where the 'date_added', 'rating', and 'duration' columns have the value 'Not given', as these rows do not significantly affect the analysis.

```python
df = df[df['date_added'] != 'Not given']
df = df[df['rating'] != 'Not given']
df = df[df['duration'] != 'Not given']
```

## Removing Unnecessary Columns

We remove the 'movie_cast' and 'description' columns because they are not necessary for the analysis.

```python
df.drop(columns=['movie_cast', 'description'], inplace=True)
df
```

## Handling the 'country' Column

We noticed that some rows in the 'country' column have more than one country. We decided to keep only the first country in each row, as we believe it is the correct country for the show.

```python
df['country'] = df['country'].apply(lambda x: x.split(',')[0])
df
```

## Results

The resulting DataFrame will not contain rows with null values or where the value of rating, duration, or date_added is 'Not given', and will only keep the first country in the country column.
