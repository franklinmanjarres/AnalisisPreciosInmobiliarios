# Predicción de Precios de Vivienda — Francia

Proyecto de machine learning aplicado al mercado inmobiliario francés. El objetivo es predecir el precio de venta de una vivienda a partir de sus características físicas, energéticas y de ubicación, utilizando técnicas de regresión supervisada.

Desarrollado como parte del Módulo II del curso Machine Learning & AI for the Working Analyst, Colegio de Matemáticas Bourbaki.

---

## Descripción del problema

Dado un conjunto de 37,368 registros de viviendas en Francia con características como superficie, número de habitaciones, eficiencia energética y código postal, se busca construir un modelo que estime el precio de venta con el menor error posible.

Las métricas utilizadas son MAE (Error Absoluto Medio) y RMSE (Raíz del Error Cuadrático Medio). No se utiliza R² en ninguna parte del análisis.

---

## Dataset

- Fuente: dataset inmobiliario francés con variables numéricas y categóricas
- Registros: 37,368 viviendas
- Variables originales: 26
- Variables finales tras preprocesamiento: 47 (luego de codificación One-Hot)
- Archivo de características: `X.csv`
- Archivo de precios: `y.csv`

Variables categóricas incluidas:

- `property_type` — tipo de inmueble (appartement, maison, etc.)
- `energy_performance_category` — certificado energético escala A a G
- `ghg_category` — emisiones de gases efecto invernadero escala A a G
- `exposition` — orientación cardinal de la fachada

---

## Estructura del proyecto

```
AnálisisPredictivoInmobiliario.ipynb   notebook principal
X.csv                                  características de las viviendas
y.csv                                  variable objetivo (price)
README.md                              este archivo
```

---

## Metodología

### 1. Limpieza y preprocesamiento

Se eliminó la variable `city` por alta cardinalidad (8,643 categorías únicas). Los valores faltantes se imputaron con la mediana para variables numéricas y con la moda para variables categóricas. Once variables presentaban nulos, todas tratadas sin eliminar registros.

### 2. Selección de variables numéricas

Se calculó la correlación de Pearson de cada variable con `price`. Se descartaron 9 variables con correlación absoluta menor a 0.05 por no aportar información útil al modelo. Se construyó la matriz de correlación entre las variables restantes para detectar redundancia; ningún par superó el umbral de 0.85.

### 3. Árbol de Decisión como preprocesamiento

Se utilizó un `DecisionTreeClassifier` (max_depth=6) entrenado sobre una versión binaria de `price` (umbral = mediana de 255,250 €) para identificar las variables numéricas con mayor capacidad discriminativa. El árbol no es el modelo final; su función es reducir la dimensionalidad antes de aplicar la regresión. Se seleccionaron 6 variables con importancia mayor a 0.01:

- `postal_code`
- `nb_rooms`
- `nb_bedrooms`
- `approximate_longitude`
- `floor`
- `nb_terraces`

### 4. Codificación y partición

Las variables categóricas se transformaron con `OneHotEncoder` (min_frequency=0.001). El dataset final quedó con 47 columnas. La partición fue 75% entrenamiento (28,026 registros) y 25% validación (9,342 registros), con random_state=42.

### 5. Estandarización

Se aplicó `StandardScaler` a todas las variables antes de entrenar los modelos. Esto fue determinante para el HuberRegressor, ya que sin estandarización variables como `postal_code` (valores ~75,000) distorsionan el modelo frente a variables como `nb_rooms` (valores 1-10).

---

## Modelos evaluados

Todos los modelos fueron evaluados con validación cruzada de 10 folds (KFold, shuffle=True, random_state=42).

| Modelo | MAE validación | Desviación estándar |
|---|---|---|
| Línea base (predecir la media) | 207,801 € | — |
| Regresión Lineal estandarizada | 178,217 € | ± 3,781 € |
| HuberRegressor sin estandarizar | 198,268 € | ± 17,839 € |
| HuberRegressor estandarizado | 165,223 € | ± 3,943 € |

El modelo final seleccionado fue el **HuberRegressor estandarizado**, con los siguientes resultados sobre el conjunto de validación:

- MAE: 166,935 €
- RMSE: 289,054 €
- Mejora sobre línea base: 19.7%

La diferencia entre MAE y RMSE (aproximadamente 122,000 €) refleja el impacto de las viviendas de lujo (precios hasta 2.3 M€) que el modelo no logra predecir correctamente. Esto se confirma con una kurtosis de residuos de 14.74, muy superior a la distribución teórica normal.

---

## Variables más influyentes

La variable con mayor impacto en el modelo es `postal_code`, lo que confirma que la ubicación es el factor determinante del precio en el mercado inmobiliario francés. Le siguen en importancia `nb_rooms` y `nb_bedrooms`, que capturan el tamaño del inmueble.

Las categorías de eficiencia energética y el tipo de inmueble tienen influencia menor pero estadísticamente presente en el modelo.

---

## Tecnologías utilizadas

- Python 3.12
- pandas
- numpy
- matplotlib
- seaborn
- scikit-learn (LinearRegression, HuberRegressor, DecisionTreeClassifier, OneHotEncoder, StandardScaler, KFold, cross_validate)
- scipy (kurtosis)

---

## Cómo ejecutar

1. Clonar el repositorio
2. Subir `X.csv` e `y.csv` al mismo directorio que el notebook
3. Abrir `AnálisisPredictivoInmobiliario.ipynb` en Google Colab o Jupyter
4. Ejecutar Runtime > Run all

---

## Autor

Franklin — Universidad del Valle, Cali, Colombia  
Curso: Machine Learning & AI for the Working Analyst  
Colegio de Matemáticas Bourbaki
