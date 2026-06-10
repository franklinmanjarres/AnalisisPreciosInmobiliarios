# Predicción de Precios de Vivienda — Mercado Inmobiliario Francés

**Tipo de proyecto:** Machine Learning — Regresión supervisada  
**Sector:** Real Estate  
**Datos:** 37,368 registros de viviendas en Francia  
**Modelo final:** HuberRegressor con estandarización  
**Resultado:** MAE de 166,935 € — mejora del 19.7% sobre la línea base

---

## El problema de negocio

Las plataformas inmobiliarias y los agentes de bienes raíces necesitan estimar el precio de venta de una propiedad antes de publicarla. Una estimación imprecisa genera pérdidas para el vendedor o alarga el tiempo de venta. Este proyecto construye un modelo que predice el precio de una vivienda a partir de sus características físicas, energéticas y de ubicación, reduciendo la incertidumbre en la valoración.

---

## Resultados

| Métrica | Valor |
|---|---|
| MAE validación | 166,935 € |
| RMSE validación | 289,054 € |
| Mejora sobre línea base | 19.7% |
| Registros analizados | 37,368 |
| Variables en el modelo final | 47 |

**MAE (Error Absoluto Medio):** en promedio, el modelo se equivoca en 166,935 € por vivienda. Para una propiedad de 300,000 €, el error es de aproximadamente el 55% — margen esperable dado que el dataset incluye desde estudios hasta mansiones de 2.3 M€.

**RMSE (Raíz del Error Cuadrático Medio):** penaliza más los errores grandes. El valor de 289,054 € revela que las viviendas de lujo concentran la mayor parte del error, lo que es consistente con un modelo generalista no especializado en el segmento premium.

---

## Metodología

```
Datos brutos (37,368 registros, 26 variables)
        |
        v
Limpieza y tratamiento de nulos
- Eliminación de city (8,643 categorías únicas)
- Imputación: mediana para numéricas, moda para categóricas
        |
        v
Selección de variables numéricas
- Correlación de Pearson con price
- Descarte de variables con |r| < 0.05 (9 variables)
- Detección de redundancia entre predictoras (|r| >= 0.85)
        |
        v
Árbol de Decisión como preprocesamiento
- Binarización de price (umbral = mediana 255,250 €)
- DecisionTreeClassifier (max_depth=6)
- Selección de 6 variables con mayor poder discriminativo
        |
        v
Codificación y estandarización
- OneHotEncoder sobre 4 variables categóricas → 47 columnas
- StandardScaler → media 0, desviación estándar 1
- Partición 75% entrenamiento / 25% validación
        |
        v
Modelado con validación cruzada (KFold, 10 folds)
- Línea base: MAE 207,801 €
- Regresión Lineal: MAE 178,217 €
- HuberRegressor sin estandarizar: MAE 198,268 €
- HuberRegressor estandarizado: MAE 165,223 € (ganador)
        |
        v
Modelo final: HuberRegressor estandarizado
MAE 166,935 € — RMSE 289,054 € — Mejora 19.7%
```

---

## Visualizaciones

### Correlación de variables numéricas con el precio

![Correlación con price]((<p align="center">
  <img src="assets/correlacion.jpeg" width="550" alt="Matriz de Correlacion entre variables">
</p>))

Las variables con mayor correlación positiva son `nb_rooms` y `nb_bedrooms`. Nueve variables fueron descartadas por correlación inferior a 0.05 con el precio.

---

 ### Selección de variables — Árbol de Decisión


El árbol identificó `postal_code` como la variable con mayor poder discriminativo (importancia 0.38), seguida de `nb_rooms` (0.30) y `nb_bedrooms` (0.16). Esto confirma que ubicación y tamaño son los factores que mejor separan viviendas caras de baratas.

---

### Comparación de modelos

![Comparación de modelos](<p align="center">
  <img src="assets/4t.jpeg" width="550" alt="Comparacion de modelos">
</p>)

La estandarización fue el paso determinante. Sin ella, el HuberRegressor obtuvo un MAE de 198,268 €. Con variables estandarizadas, el mismo modelo alcanzó 165,223 € — una mejora de 33,000 € solo por escalar correctamente las variables.

---

### Distribución de residuos — modelo final

![Distribución de residuos](<p align="center">
  <img src="assets/31.jpeg" width="550" alt="Distribucion de Residuos">
</p>)

La mayoría de errores se concentran cerca de cero. La cola derecha larga corresponde a viviendas de lujo (precio > 1 M€) que el modelo no puede predecir bien con las variables disponibles. La kurtosis de 14.74 confirma la presencia de estos errores extremos.

---

### Variables más influyentes

![Variables influyentes](<p align="center">
  <img src="assets/4variables.jpeg" width="550" alt="variables mas influyentes">
</p>)

Con variables estandarizadas, los coeficientes son directamente comparables. `postal_code` domina con amplia ventaja, lo que refleja el peso de la ubicación en el mercado francés.

---

## Hallazgos principales

La ubicación (`postal_code`) es el predictor más importante del precio, con una importancia casi tres veces superior a la segunda variable (`nb_rooms`). Esto es consistente con la dinámica del mercado inmobiliario francés, donde el precio por metro cuadrado varía drásticamente entre regiones.

El modelo falla principalmente con viviendas de lujo (precio > 1 M€), un segmento que representa menos del 2.5% del dataset pero concentra los errores más grandes. Para este segmento sería necesario un modelo especializado con variables adicionales como calidad de materiales, vistas o características exclusivas.

La estandarización de variables fue el paso metodológico con mayor impacto en los resultados: permitió que el HuberRegressor comparara correctamente el peso de cada variable y pasara de ser el peor modelo a ser el mejor.

---

## Tecnologías

- Python 3.12
- pandas, numpy
- matplotlib, seaborn
- scikit-learn
- scipy

---

## Autor

**Franklin Manuel Manjarres**
Proyecto desarrollado como parte del programa de formación **Machine Learning & AI for the Working Analyst**.
Aplicando técnicas de análisis de datos, ingeniería de características y modelos de aprendizaje automático para la predicción de precios inmobiliarios.                             
