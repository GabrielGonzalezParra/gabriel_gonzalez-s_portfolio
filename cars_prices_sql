# CAR PRICE ANALYSIS IN SQL #

# Introduction

The automotive market is highly dynamic, with prices influenced by multiple factors such as brand reputation, engine size, fuel type, and mileage. In this project, we analyze a dataset of used car prices to uncover key trends and relationships between these variables.

# Objectives

**Uncover the most profitable car brands**

1.  Identify which brands retain the highest resale value over time

2.  Compare price depreciation across brands and models


**1.   Which brands retain the highest resale value over time?**

To answer this question, I calculated the average price of a new car for each brand and compared it with the average price of older cars from the same brand. 
I decided to analyze price trends by comparing new car prices with the average prices of cars at various ages.

- New cars average price vs 5 to 7 years old cars average price
- New cars average price vs 10 to 12 years old cars cars average price
- New cars average price vs 15 to 17 years old cars cars average price
- New cars average price vs 20 to 23 years old cars cars average price

First, I calculated the average price per brand for cars produced in the 2023:

```sql
SELECT brand,
	ROUND (CAST(AVG(price)AS NUMERIC),0) AS new_price_avg
	FROM cars
	WHERE year = 2023
	Group by brand
	Order by new_price_avg desc;
```

brand | new_price_avg
------- |--------------- 
Ford	|   12.725€
Chevrolet|	12.419€
Hyundai |	12.279€
Kia	  |  12.206€
Mercedes|	12.198€
Honda	  |  12.115€
Volkswagen|	12.074€
Toyota   |	11.994€
BMW	 |   11.965€
Audi	 |  11.764€


Then, I calculated all the price averages for each car age:

Price average for cars between 5 and 7 years old

```sql
SELECT brand,
	ROUND (CAST(AVG(price)AS NUMERIC),0) AS old1_price_avg
	FROM cars
	WHERE year = 2018
	OR year = 2017
	OR year = 2016
	Group by brand
	Order by old1_price_avg desc;
```
brand | old1_price_avg
------- |--------------- 
Audi	|10.818€
Chevrolet|	10.741€
BMW	|10.580€
Mercedes|	10.515€
Volkswagen|	10.496€
Honda	|10.493€
Ford	|10.437€
Toyota|	10.371€
Kia	|10.253€
Hyundai|	10.211€

Price average for cars between 10 and 12 years old

```sql
SELECT brand,
	ROUND (CAST(AVG(price)AS NUMERIC),0) AS old2_price_avg
	FROM cars
	WHERE year = 2013
	OR year = 2012
	OR year = 2011
	Group by brand
	Order by old2_price_avg desc;
```
brand | old2_price_avg
------- |--------------- 
Ford |	9.279€
Audi |	9.111€
Chevrolet |	9.093€
Mercedes |	9.023€
Honda |	8.952€
Hyundai |	8.840€
Kia |	8.797€
BMW |	8.737€
Volkswagen |	8.717€
Toyota |	8.556€

Price average for cars between 15 and 17 years old

```sql
SELECT brand,
	ROUND (CAST(AVG(price)AS NUMERIC),0) AS old3_price_avg
	FROM cars
	WHERE year = 2008
	OR year = 2007
	OR year = 2006
	Group by brand
	Order by old3_price_avg desc;
```
brand | old3_price_avg
------- |--------------- 
Kia |	7.815€
Volkswagen |	7.718€
Mercedes |	7.635€
Hyundai |	7.613€
Toyota |	7.467€
Chevrolet |	7.462€
Ford |	7.450€
BMW |	7.433€
Audi |	7.432€
Honda |	7.293€

Price average for cars between 20 and 23 years old

```sql
SELECT brand,
	ROUND (CAST(AVG(price)AS NUMERIC),0) AS old4_price_avg
	FROM cars
	WHERE year = 2003
	OR year = 2002
	OR year = 2001
	OR year = 2000
	Group by brand
	Order by old4_price_avg desc;
```
brand | old4_price_avg
------- |--------------- 
Mercedes |	6.033€
Volkswagen |	5.900€
Kia |	5.890€
Honda |	5.884€
Audi |	5.883€
BMW |	5.876€
Chevrolet |	5.870€
Toyota |	5.811€
Hyundai |	5.799€
Ford |	5.761€

Now I calculated the price variation between the average price of the 2023 cars and the average price of the older ones, I did this per brand so that i could compare the variation and see which brand retains more its value along the years.

```sql
WITH avg_prices AS
(
SELECT 
	brand,
AVG(CASE WHEN year = 2023 THEN price END) AS price_avg_2023,
AVG(CASE WHEN year IN (2015, 2016, 2017) THEN price END) AS price_avg_2015_2017
FROM cars
GROUP BY brand) 

SELECT 
	brand,
ROUND(CAST(((price_avg_2015_2017 / price_avg_2023) - 1) * 100 AS NUMERIC), 2) 
AS price_variation
FROM avg_prices
ORDER BY price_variation DESC;
```
brand | price_variation1
------- |--------------- 
Audi |	-8.04%
BMW |	-11.57%
Volkswagen |	-13.07%
Honda |	-13.38%
Chevrolet |	-13.51%
Toyota |	-13.53%
Mercedes |	-13.80%
Kia |	-15.00%
Hyundai |	-16.84%
Ford |	-17.98%

```sql
WITH avg_prices AS
(
SELECT 
	brand,
AVG(CASE WHEN year = 2023 THEN price END) AS price_avg_2023,
AVG(CASE WHEN year IN (2011, 2012, 2013) THEN price END) AS price_avg_2011_2013
FROM cars
GROUP BY brand) 

SELECT 
	brand,
ROUND(CAST(((price_avg_2011_2013 / price_avg_2023) - 1) * 100 AS NUMERIC), 2) 
AS price_variation2
FROM avg_prices
ORDER BY price_variation2 DESC;
```
brand | price_variation2
------- |--------------- 
Audi |	-22.55%
Mercedes |	-26.03%
Honda |	-26.10%
Chevrolet |	-26.79%
BMW |	-26.98%
Ford |	-27.08%
Volkswagen |	-27.80%
Kia |	-27.93%
Hyundai |	-28.01%
Toyota |	-28.66%

```sql
WITH avg_prices AS
(
SELECT 
	brand,
AVG(CASE WHEN year = 2023 THEN price END) AS price_avg_2023,
AVG(CASE WHEN year IN (2006, 2007, 2008) THEN price END) AS price_avg_2006_2008
FROM cars
GROUP BY brand) 

SELECT 
	brand,
ROUND(CAST(((price_avg_2006_2008 / price_avg_2023) - 1) * 100 AS NUMERIC), 2) 
AS price_variation3
FROM avg_prices
ORDER BY price_variation3 DESC;
```
brand | price_variation3
------- |--------------- 
Kia |	-35.98%
Volkswagen |	-36.08%
Audi |	-36.83%
Mercedes |	-37.41%
Toyota |	-37.74%
BMW |	-37.88%
Hyundai |	-38.00%
Honda |	-39.80%
Chevrolet |	-39.92%
Ford |	-41.45%

```sql
WITH avg_prices AS
(
SELECT 
	brand,
AVG(CASE WHEN year = 2023 THEN price END) AS price_avg_2023,
AVG(CASE WHEN year IN (2000, 2001, 2002, 2003) THEN price END) AS price_avg_2000_2003
FROM cars
GROUP BY brand) 

SELECT 
	brand,
ROUND(CAST(((price_avg_2000_2003 / price_avg_2023) - 1) * 100 AS NUMERIC), 2) 
AS price_variation
FROM avg_prices
ORDER BY price_variation DESC;
```
brand | price_variation4
------- |--------------- 
Audi |	-49.99%
Mercedes |	-50.54%
BMW |	-50.89%
Volkswagen |	-51.14%
Honda |	-51.43%
Toyota |	-51.55%
Kia |	-51.74%
Chevrolet |	-52.74%
Hyundai |	-52.77%
Ford |	-54.73%

**2.  Compare price depreciation across brands and models**

To analyze this, we compare the average prices of various car models from different brands over multiple time periods.

```sql
SELECT cp23.brand, cp23.model,
ROUND(CAST(((cp5_7.avg_16_18 / cp23.avg_23) - 1) * 100 AS NUMERIC), 2) AS model_price_variation_1,
ROUND(CAST(((cp10_12.avg_11_13 / cp23.avg_23) - 1) * 100 AS NUMERIC), 2) AS model_price_variation_2,
ROUND(CAST(((cp15_17.avg_06_08 / cp23.avg_23) - 1) * 100 AS NUMERIC), 2) AS model_price_variation_3,
ROUND(CAST(((cp20_23.avg_00_03 / cp23.avg_23) - 1) * 100 AS NUMERIC), 2) AS model_price_variation_4
FROM cp23
LEFT JOIN cp5_7 ON cp23.model = cp5_7.model AND cp23.brand = cp5_7.brand
LEFT JOIN cp10_12 ON cp23.model = cp10_12.model AND cp23.brand = cp10_12.brand
LEFT JOIN cp15_17 ON cp23.model = cp15_17.model AND cp23.brand = cp15_17.brand
LEFT JOIN cp20_23 ON cp23.model = cp20_23.model AND cp23.brand = cp20_23.brand
ORDER BY 3 desc;
```

| Brand       | Model       | Price Variation 1 | Price Variation 2 | Price Variation 3 | Price Variation 4 |
|------------|------------|------------------|------------------|------------------|------------------|
| Toyota     | RAV4       | -2.94%           | -25.04%          | -37.37%          | -50.98%          |
| Mercedes   | GLA        | -2.96%           | -24.71%          | -37.05%          | -50.69%          |
| Audi       | A4         | -5.94%           | -19.84%          | -33.61%          | -53.71%          |
| Audi       | Q5         | -6.24%           | -27.95%          | -35.76%          | -46.76%          |
| Hyundai    | Sonata     | -6.25%           | -20.85%          | -28.05%          | -49.06%          |
| Kia        | Sportage   | -6.37%           | -20.96%          | -32.53%          | -45.07%          |
| BMW        | X5         | -6.81%           | -26.02%          | -35.93%          | -49.74%          |
| Volkswagen | Tiguan     | -8.32%           | -22.14%          | -34.32%          | -42.92%          |
| Chevrolet  | Malibu     | -8.41%           | -22.09%          | -43.14%          | -56.32%          |
| Ford       | Fiesta     | -10.22%          | -18.60%          | -33.57%          | -53.40%          |
| BMW        | 5 Series   | -10.63%          | -27.75%          | -34.19%          | -53.40%          |
| Honda      | Civic      | -12.74%          | -27.68%          | -44.80%          | -50.32%          |
| Audi       | A3         | -13.00%          | -20.66%          | -39.97%          | -49.87%          |
| Honda      | Accord     | -13.24%          | -26.53%          | -35.48%          | -50.75%          |
| Volkswagen | Passat     | -14.64%          | -26.89%          | -38.68%          | -54.60%          |
| Honda      | CR-V       | -14.65%          | -24.76%          | -39.85%          | -53.53%          |
| BMW        | 3 Series   | -14.94%          | -26.99%          | -43.39%          | -48.74%          |
| Chevrolet  | Impala     | -15.24%          | -29.06%          | -38.36%          | -50.63%          |
| Kia        | Optima     | -15.98%          | -27.73%          | -36.77%          | -51.50%          |
| Hyundai    | Tucson     | -16.83%          | -30.44%          | -36.15%          | -52.52%          |
| Toyota     | Camry      | -16.85%          | -30.65%          | -39.49%          | -51.95%          |
| Chevrolet  | Equinox    | -17.04%          | -28.90%          | -38.89%          | -49.96%          |
| Mercedes   | C-Class    | -17.36%          | -25.60%          | -38.74%          | -52.62%          |
| Ford       | Focus      | -17.69%          | -27.49%          | -39.51%          | -51.36%          |
| Volkswagen | Golf       | -18.03%          | -35.83%          | -36.77%          | -55.53%          |
| Mercedes   | E-Class    | -19.18%          | -27.45%          | -36.35%          | -47.43%          |
| Toyota     | Corolla    | -19.49%          | -28.70%          | -35.82%          | -50.55%          |
| Ford       | Explorer   | -22.34%          | -32.36%          | -48.64%          | -58.83%          |
| Kia        | Rio        | -23.00%          | -32.79%          | -36.85%          | -56.86%          |
| Hyundai    | Elantra    | -23.17%          | -29.34%          | -45.10%          | -54.55%          |

![model_price_variation](https://github.com/user-attachments/assets/6892f9de-71ab-478d-a733-30f2806305a0)

**Models with the best value retention over time:**

These models show less depreciation, even after more than 20 years:

* Toyota RAV4: Maintains the lowest depreciation across all age categories, especially in the 5-7 year range.
* BMW 3 Series: Although it depreciates more in older models, it still retains relatively high values even after 20+ years.
* Honda CR-V: Follows a similar trend to the RAV4, with lower depreciation compared to other models.
* Volkswagen Golf: Shows stability across all age categories, with less value loss in vehicles older than 15 years.
* Mercedes E-Class: Depreciates less in models older than 15 years, holding value better than many others.


**Models with the Highest Depreciation**

These models experience the steepest price drops over time:

* Chevrolet Malibu: One of the models with the highest depreciation across all age groups.
* Ford Fiesta: Loses value quickly, especially in the 10-12 year range, with the trend continuing for older models.
* Hyundai Elantra: Experiences one of the steepest declines in the 20+ year range.
* Ford Explorer: Shows one of the greatest value losses in older models, especially in the 20+ year range.
* Chevrolet Equinox: Suffers a sharp decline in models older than 15 years, with values below average.

**Models with Notable Differences Across Age Groups**

Some models exhibit interesting trends depending on their age:

* BMW 5 Series: Has a sharp decline in the 5-7 year range but stabilizes better in older models.
* Audi A3: Experiences strong depreciation in its early years but holds its value better in the 15-17 year range compared to other models.
* Ford Focus: Despite steep depreciation in the 10-12 year range, the decline slows down in models older than 15 years.
* Mercedes C-Class: Loses more value in the 5-7 year range than in models older than 15 years, where it retains value better.

**Conclusion**

The analysis of car price depreciation over time reveals distinct trends among different brands.

**Best Resale Value:**

Audi, BMW, and Mercedes retain value better than other brands, showing the lowest depreciation rates over time.
Audi leads with a -8.04% drop in the first 5–7 years and -49.99% after 20–23 years.

**Higher Depreciation Rates:**

Ford, Hyundai, and Chevrolet show the highest depreciation, losing over 50% of their value after two decades.
Ford experiences the steepest decline, with a -17.98% drop within the first 5–7 years and -54.73% after 20–23 years.

**Model-Level Insights:**

Toyota RAV4, Mercedes GLA, and Audi A4 have the slowest depreciation among models.
Volkswagen Tiguan and Hyundai Sonata show steeper declines compared to their respective brands.
