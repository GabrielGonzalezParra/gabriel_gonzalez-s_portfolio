
# Guía Completa de R – Análisis de Datos Masivos

Esta guía está diseñada como un recorrido paso a paso por los principales temas y técnicas abordadas en el curso, desde la carga y limpieza de datos hasta el modelado estadístico, predicciones y clustering.

---

## 1. Carga e inspección de datos

Importamos un conjunto de datos real y utilizamos funciones exploratorias para conocer su estructura, variables y valores faltantes. Esto es fundamental antes de cualquier análisis.

```r
library(readr)
datos <- read_csv("housing_in_london_monthly_variables.csv")
summary(datos)  # estadísticas básicas de cada columna
str(datos)      # estructura de las variables (tipos de dato)
colSums(is.na(datos))  # cantidad de NAs por variable
```

---

## 2. Limpieza de datos

Eliminamos filas que contienen valores perdidos en la variable `houses_sold`, clave para el análisis de precios y demanda.

```r
datos_filtrados <- datos[!is.na(datos$houses_sold), ]
```

También podrías imputar valores (rellenar con la media o mediana), pero aquí optamos por eliminar para asegurar la calidad del análisis.

---

## 3. Manipulación con `dplyr`

Creamos nuevas columnas como el volumen de negocios (`precio * cantidad`) y extraemos estadísticas agrupadas por barrio (`area`).

```r
library(dplyr)

datos_filtrados <- datos %>%
  filter(!is.na(houses_sold)) %>%
  mutate(volumen_negocios = average_price * houses_sold)

precios <- datos_filtrados %>%
  group_by(area) %>%
  summarise(precio_medio = mean(average_price)) %>%
  arrange(desc(precio_medio))
```

---

## 4. Estadística descriptiva

Identificamos patrones globales en los datos, como tendencia central y variabilidad. Muy útil para comparativas entre barrios o meses.

```r
summary(datos_filtrados)
mean(datos_filtrados$average_price)
var(datos_filtrados$average_price)
```

---

## 5. Covarianza y correlación

Ambas herramientas analizan relaciones entre variables numéricas:

- **Covarianza**: dirección de la relación (positiva o negativa).
- **Correlación**: además del signo, indica la fuerza de esa relación (valores entre -1 y 1).

```r
cov(datos_filtrados$average_price, datos_filtrados$houses_sold)
cor(datos_filtrados$average_price, datos_filtrados$houses_sold)
```

---

## 6. Regresión lineal simple

Buscamos modelar cómo varía una variable (ventas) en función de otra (precio). Esto permite hacer predicciones e interpretar relaciones causales.

```r
modelo <- lm(houses_sold ~ average_price, data = datos_filtrados)
summary(modelo)
```

---

## 7. Regresión logarítmica (log-log)

Transformamos ambas variables a logaritmos para interpretar los resultados como elasticidades: cambios porcentuales.

```r
modelo_log <- lm(log(houses_sold) ~ log(average_price), data = datos_filtrados)
summary(modelo_log)
```

---

## 8. Series temporales y predicción

Convertimos una variable en serie temporal con `ts()` y aplicamos un modelo ARIMA para prever valores futuros.

```r
library(forecast)

serie_ts <- ts(datos_filtrados$average_price, start = c(1995, 1), frequency = 12)
modelo_arima <- auto.arima(serie_ts)
pred <- forecast(modelo_arima, h = 12)
plot(pred)
```

---

## 9. Regresión logística binaria

Aplicamos un modelo para predecir si una persona sobrevive (1) o no (0) en base a variables como edad, sexo y clase del Titanic.

```r
library(titanic)
data("titanic_train")
datos_titanic <- titanic_train
datos_titanic <- datos_titanic[!is.na(datos_titanic$Age), ]

modelo_log <- glm(Survived ~ Age + Sex + as.factor(Pclass), data = datos_titanic, family = "binomial")
summary(modelo_log)
```

---

## 10. Validación del modelo predictivo

Dividimos el dataset en entrenamiento y test para comprobar si el modelo generaliza a datos nuevos. Comprobamos precisión con matriz de confusión.

```r
library(caTools)
set.seed(123)
split <- sample.split(datos_titanic$Survived, SplitRatio = 0.8)
train <- subset(datos_titanic, split == TRUE)
test <- subset(datos_titanic, split == FALSE)

modelo_train <- glm(Survived ~ Age + Sex + as.factor(Pclass), data = train, family = "binomial")
pred_test <- predict(modelo_train, newdata = test, type = "response")
pred_test_bin <- ifelse(pred_test > 0.5, 1, 0)

library(caret)
confusionMatrix(as.factor(pred_test_bin), as.factor(test$Survived))
```

---

## 11. Predicción de casos individuales

Probamos con perfiles ficticios qué probabilidad tendrían de sobrevivir.

```r
predict(modelo_train, newdata = data.frame(Age = 22, Sex = "male", Pclass = 3), type = "response")
predict(modelo_train, newdata = data.frame(Age = 18, Sex = "female", Pclass = 1), type = "response")
```

---

## 12. Interpretación con odds ratios

Convertimos los coeficientes a razones de probabilidad (odds) para interpretar su efecto sobre el resultado.

```r
exp(coef(modelo_train))
```

---

## 13. Árboles de decisión

Visualizamos un árbol que clasifica las observaciones con reglas simples y jerárquicas.

```r
library(rpart)
library(rpart.plot)

modelo_arbol <- rpart(Survived ~ Age + Sex + as.factor(Pclass), data = datos_titanic, method = "class")
rpart.plot(modelo_arbol)
```

---

## 14. Series temporales con conjunto de validación

Entrenamos el modelo con datos antiguos y validamos las predicciones con datos recientes. Evaluamos el error.

```r
entrenamiento <- ts(datos$UK[1:227], start = c(2000, 1), frequency = 12)
validacion <- ts(datos$UK[228:239], start = c(2018, 12), frequency = 12)

modelo <- auto.arima(entrenamiento)
pred <- forecast(modelo, h = 12)

accuracy(pred$mean, validacion)
```

---

## 15. Clusterización con K-means

Agrupamos observaciones en clústeres con características similares (no supervisado). Requiere solo variables numéricas.

```r
data("iris")
datos <- iris
datos_num <- datos[, -5]

library(factoextra)
fviz_nbclust(datos_num, kmeans)

modelo_cluster <- kmeans(datos_num, 3)
table(datos$Species, modelo_cluster$cluster)
```

---

## 16. Interpretación con árboles de decisión

Creamos un árbol para explicar cómo se han formado los clústeres, con reglas basadas en las variables originales.

```r
arbol_cluster <- rpart(as.factor(modelo_cluster$cluster) ~ ., data = datos, method = "class")
rpart.plot(arbol_cluster)
```

---

## 17. Escalado previo al clustering

Estandarizamos los datos para evitar que variables con grandes rangos dominen el análisis.

```r
datos_scaled <- scale(datos_num)
modelo_cluster_scaled <- kmeans(datos_scaled, 3)
table(datos$Species, modelo_cluster_scaled$cluster)
```
