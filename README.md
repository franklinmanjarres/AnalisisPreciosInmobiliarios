# Análisis Predictivo de Precios Inmobiliarios

![portada](assets/Portada_inmobiliario.png)

> Implementación de modelos de regresión robusta para predicción de precios inmobiliarios, diseñada para mitigar el impacto de valores atípicos (outliers) sin perder interpretabilidad.

---

## Resultado

| Modelo | MAE | Mejora |
|---|---|---|
| Regresión Lineal | 182,847 | — |
| HuberRegressor | 169,465 | 7.3% menor error |

El HuberRegressor superó a la regresión lineal clásica reduciendo el error promedio de predicción en un 7.3%.

---

## Tecnologías

| Herramienta | Uso |
|---|---|
| Python 3.x | Lenguaje principal |
| Scikit-learn | Modelos, preprocesamiento y métricas |
| Pandas / NumPy | Manipulación de datos |
| Matplotlib / Seaborn | Visualización |
| SciPy | Análisis estadístico |

---

## Tabla de Contenidos

- [1. Identificación del Problema](#1-identificación-del-problema)
- [2. Los Datos — Cómo se utilizan para resolver el problema](#2-los-datos--cómo-se-utilizan-para-resolver-el-problema)
- [3. Conclusiones Finales](#3-conclusiones-finales)

---

## 1. Identificación del Problema

###  Objetivo del Proyecto

Comprar o vender una propiedad sin información precisa sobre su valor real es un problema costoso. Para un comprador, significa pagar de más. Para una inmobiliaria, significa perder dinero o clientes. Para un banco que otorga hipotecas, significa asumir riesgos mal calculados.

La pregunta que resuelve este proyecto es directa:

**¿Podemos predecir el precio de una propiedad a partir de sus características físicas y de ubicación?**

### Desafíos Técnicos

Los precios inmobiliarios no se distribuyen de forma uniforme. Existen propiedades con precios extremadamente altos que distorsionan los modelos tradicionales — un penthouse de lujo en el mismo dataset que un apartamento estudio puede arruinar las predicciones si no se maneja correctamente.

Este fenómeno se llama **valores atípicos (outliers)** y es el principal reto técnico del proyecto.

### Solución Propuesta

Evaluar y comparar dos modelos de regresión:

- **Regresión Lineal** — modelo clásico, sensible a los outliers
- **HuberRegressor** — modelo robusto, diseñado para resistir el efecto de precios extremos

> **Ejemplo concreto:** Imagina 100 apartamentos y 2 mansiones de lujo en el mismo dataset. Un modelo común intenta ajustarse a todos incluyendo las mansiones, y falla en los apartamentos normales. El HuberRegressor le da menos peso a esas mansiones y predice mejor el resto.

---

## 2. Los Datos — Cómo se utilizan para resolver el problema

### Descripción del Dataset

| Archivo | Contenido |
|---|---|
| `X.csv` | Variables independientes — características de cada propiedad |
| `y.csv` | Variable objetivo — precio de cada propiedad |

### Variables principales

| Variable | Tipo | Descripción |
|---|---|---|
| `property_type` | Categórica | Tipo de propiedad (apartamento, casa, etc.) |
| `approximate_latitude / longitude` | Numérica | Ubicación geográfica |
| `has_a_garage` | Booleana | Si tiene garaje |
| `has_a_balcony` | Booleana | Si tiene balcón |
| `has_air_conditioning` | Booleana | Si tiene aire acondicionado |
| `price` | Numérica | Precio de la propiedad (variable a predecir) |

### Preprocesamiento de Datos

**1. Eliminación de columnas con muchos valores faltantes**
Columnas como `exposition` (75% nulos) o `energy_performance_category` (49% nulos) se eliminan porque no aportan información confiable.

**2. Reducción de cardinalidad con regla de Pareto (99%)**
La variable `property_type` tiene miles de categorías. Se conservan solo las que representan el 99% de los datos.

**3. Codificación One-Hot**
Las variables categóricas fueron transformadas mediante One-Hot Encoding.

> Se aplicó reducción de cardinalidad mediante regla de Pareto (99%) para disminuir sparsity y mejorar estabilidad del modelo.

**4. Estandarización**
Todas las variables numéricas se transforman para tener media 0 y desviación estándar 1, evitando que variables con valores grandes dominen el modelo.

### Flujo del proyecto (pipeline)

> **Pipeline** es la secuencia ordenada de pasos que siguen los datos desde que entran hasta que se obtiene el resultado final. Como una línea de producción: cada etapa transforma los datos y los pasa a la siguiente.

```
1. Carga de datos (X.csv + y.csv)
        ↓
2. Limpieza — eliminación de columnas con alta nulidad
        ↓
3. Reducción de cardinalidad (Pareto 99%)
        ↓
4. One-Hot Encoding de variables categóricas
        ↓
5. Estandarización (media 0, std 1)
        ↓
6. División entrenamiento / prueba (75% / 25%)
        ↓
7. Entrenamiento: Regresión Lineal vs HuberRegressor
        ↓
8. Evaluación con MAE (Error Absoluto Medio)
        ↓
9. Análisis de coeficientes — variables más influyentes
```

### Métrica de Evaluación

Se usa el **MAE (Mean Absolute Error)**: el promedio de cuánto se equivoca el modelo al predecir el precio.

$$\text{MAE} = \frac{1}{n} \sum_{i=1}^{n} \left| y_i - \hat{y}_i \right|$$

- Cuanto menor es el MAE, mayor es la precisión del modelo
- El modelo presenta un error absoluto promedio de 169,465 en la predicción del precio de las propiedades
---

## 3. Conclusiones Finales

### Comparación de Modelos
El **HuberRegressor redujo el error en un 7.3%** respecto a la regresión lineal, sin sacrificar interpretabilidad. Es más adecuado cuando los datos tienen precios extremos — como ocurre siempre en el mercado inmobiliario real.

### Variables Más Influyentes

- Las variables de **ubicación geográfica** tienen alta influencia
- El tipo de propiedad `viager` tiene el coeficiente más negativo — reduce significativamente el precio predicho
- Garaje, balcón y aire acondicionado tienen influencia positiva

### Limitaciones del Modelo

- El modelo no captura relaciones no lineales entre variables y precio
- El umbral de Huber fue fijado manualmente; optimización de hiperparámetros podría mejorar resultados
- Variables con alta nulidad debieron descartarse, reduciendo información disponible

### Trabajo Futuro

Probar modelos no lineales como Random Forest o XGBoost, aplicar optimización de hiperparámetros y enriquecer el dataset con variables de contexto como cercanía a servicios.


## Habilidades Demostradas

- Modelado estadístico
- Regresión robusta
- Machine Learning supervisado
- Feature Engineering
- Tratamiento de outliers
- Preprocesamiento de datos
- Estandarización y codificación
- Evaluación de modelos
- Interpretabilidad de modelos
- Python / Scikit-learn

---

## Autor

Proyecto desarrollado como parte de un curso de **Machine Learning & AI**.













# 🏠 Predicción de Precios Inmobiliarios con Regresión Robusta
### Reduciendo el Error de Estimación en un 7.3% mediante HuberRegressor y Selección Inteligente de Variables

<br>

![Python](https://img.shields.io/badge/Python-3.10+-3776AB?style=for-the-badge&logo=python&logoColor=white)
![Scikit-Learn](https://img.shields.io/badge/Scikit--Learn-1.3-F7931E?style=for-the-badge&logo=scikit-learn&logoColor=white)
![Pandas](https://img.shields.io/badge/Pandas-2.0-150458?style=for-the-badge&logo=pandas&logoColor=white)
![Status](https://img.shields.io/badge/Estado-Completado-28a745?style=for-the-badge)
![Type](https://img.shields.io/badge/Tipo-Regresión%20Predictiva-9b59b6?style=for-the-badge)

---

## 📌 Resumen Ejecutivo

> **El sector inmobiliario mueve billones de dólares al año. Sin embargo, la fijación de precios sigue siendo uno de sus mayores desafíos.**

En este proyecto construí un **modelo de predicción de precios de inmuebles** capaz de estimar el valor de una propiedad basándose en sus características físicas, tipo de inmueble y ubicación geográfica.

El reto no era solo entrenar un modelo, sino hacerlo **robusto frente a propiedades de lujo y valores atípicos** que distorsionan los modelos tradicionales.

| Métrica | Regresión Lineal | HuberRegressor | Mejora |
|:--------|:----------------:|:--------------:|:------:|
| **MAE** | 182,847 | **169,465** | ✅ **-7.3%** |
| Robustez ante outliers | Baja | Alta | ✅ |
| Interpretabilidad | Alta | Alta | ✅ |

*El modelo final reduce el error promedio de estimación en más de 13,000 unidades monetarias por propiedad.*

---

## 📋 Tabla de Contenidos

- [El Problema](#-el-problema)
- [Objetivo del Proyecto](#-objetivo-del-proyecto)
- [Los Datos](#-los-datos)
- [Metodología](#-metodología)
- [Modelado](#-modelado-y-técnicas-utilizadas)
- [Resultados](#-resultados-y-métricas)
- [Visualizaciones Clave](#-visualizaciones-clave)
- [Conclusiones](#-conclusiones-finales)
- [Impacto Real](#-impacto-del-proyecto)
- [Tecnologías](#-tecnologías-utilizadas)
- [Autor](#-autor)

---

## 🔍 El Problema

### ¿Por qué predecir precios de inmuebles es difícil?

El mercado inmobiliario tiene una característica que lo hace especialmente desafiante para modelos estadísticos clásicos: **los precios no siguen una distribución normal**.

Un apartamento promedio puede costar 200,000 €. Pero una mansión en la misma ciudad puede costar 5,000,000 €. Si entrenas un modelo de regresión lineal estándar con esos datos, **la mansión "contamina" los coeficientes** y el modelo se vuelve impreciso para el 97% de las propiedades.

```
Distribución de precios:
├── 97.5% de propiedades → precio ≤ 1,275,000
└──  2.5% de propiedades → precio > 1,275,000 (outliers)
```

> 💡 **El problema no era solo predecir precios. Era predecir bien sin que los casos extremos arruinaran el modelo.**

### Consecuencias de no resolverlo

- **Compradores**: Pagan de más o pierden oportunidades por estimaciones incorrectas
- **Vendedores**: Fijan precios fuera del mercado, prolongando el tiempo de venta
- **Inversores**: Toman decisiones de cartera con información imprecisa
- **Instituciones financieras**: Valoran garantías hipotecarias con mayor riesgo

---

## 🎯 Objetivo del Proyecto

### Objetivo Principal
Construir un modelo predictivo capaz de estimar el precio de un inmueble a partir de sus características, **minimizando el error promedio** y manteniéndose **robusto frente a propiedades atípicas**.

### Objetivos Secundarios
- Identificar las variables que más impactan en el precio de un inmueble
- Comparar el desempeño de regresión lineal clásica vs. regresión robusta
- Diseñar un pipeline de preprocesamiento reproducible y justificado

### Hipótesis del Análisis

| # | Hipótesis | Resultado |
|:--|:----------|:---------:|
| H1 | El número de habitaciones tiene correlación positiva con el precio | ✅ Confirmada |
| H2 | Los modelos estándar son sensibles a propiedades de lujo | ✅ Confirmada |
| H3 | La regresión robusta (Huber) superará a la lineal en MAE | ✅ Confirmada |
| H4 | La ubicación geográfica influye significativamente en el precio | ✅ Confirmada |

### Preguntas Clave
1. ¿Qué características de una propiedad predicen mejor su precio?
2. ¿Cómo afectan los outliers al rendimiento de los modelos?
3. ¿Cuánto mejora un modelo robusto frente a uno clásico?
4. ¿Qué tipo de propiedad tiende a valer más en este mercado?

---

## 📊 Los Datos

### Dataset
El análisis utilizó un conjunto de datos inmobiliarios con dos archivos separados:
- **`X.csv`** — Variables independientes (características de las propiedades)
- **`y.csv`** — Variable objetivo (precio del inmueble)

**Dimensiones iniciales:** ~37,368 registros × múltiples variables

### Variables Clave

| Variable | Tipo | Descripción |
|:---------|:----:|:------------|
| `size` | Numérica | Superficie del inmueble |
| `nb_rooms` | Numérica | Número total de habitaciones |
| `nb_bedrooms` | Numérica | Número de dormitorios |
| `nb_bathrooms` | Numérica | Número de baños |
| `property_type` | Categórica | Tipo de propiedad (apartamento, casa, etc.) |
| `approximate_latitude` | Numérica | Latitud geográfica |
| `approximate_longitude` | Numérica | Longitud geográfica |
| `has_a_garage` | Booleana | ¿Tiene garaje? |
| `has_a_balcony` | Booleana | ¿Tiene balcón? |
| `has_air_conditioning` | Booleana | ¿Tiene aire acondicionado? |
| `nb_photos` | Numérica | Número de fotos del anuncio |
| `price` | Numérica | **Variable objetivo** — Precio de venta |

### Calidad de los Datos

Las columnas con alta tasa de nulos o baja variabilidad fueron eliminadas:

```python
# Variables eliminadas y justificación
col_borrar = {
    'energy_performance_category': '48.97% valores nulos',
    'exposition':                  '75.66% valores nulos',
    'floor':                       '74% valores nulos',
    'land_size':                   '58% nulos + outliers extremos',
    'ghg_category':                '50% valores nulos',
    'city':                        'Alta cardinalidad (8,643 categorías únicas)',
    'postal_code':                  'Redundante con lat/long',
    'last_floor':                  'Variabilidad < 5% (0.38%)',
    'upper_floors':                'Variabilidad < 5% (0.024%)'
}
```

### Ingeniería de Variables (Feature Engineering)

**1. Análisis de Pareto para `property_type`**
Se aplicó la regla de Pareto al 99%: se conservaron solo las categorías que representan el 99% de los registros, reemplazando las minoritarias con `NaN`.

**2. Codificación One-Hot**
La variable categórica `property_type` fue transformada con `pd.get_dummies()`, generando columnas binarias por cada categoría relevante.

**3. Imputación con Mediana**
Variables: `size`, `nb_rooms`, `nb_bedrooms`, `nb_bathrooms` → valores faltantes reemplazados por la mediana (método robusto ante outliers).

**4. Estandarización**
Se aplicó `StandardScaler` para centrar y escalar todas las variables numéricas (media=0, std=1), necesario para comparar la importancia de coeficientes.

---

## ⚙️ Metodología

### Pipeline Completo

```
Datos Brutos
     │
     ▼
[1] Carga y exploración inicial (shape, dtypes, nulls)
     │
     ▼
[2] Eliminación de columnas con alta cardinalidad / alta tasa de nulos
     │
     ▼
[3] Análisis de Pareto → Reducción de cardinalidad en property_type
     │
     ▼
[4] One-Hot Encoding de variables categóricas
     │
     ▼
[5] Imputación de nulos con mediana (variables numéricas)
     │
     ▼
[6] Análisis Exploratorio (EDA): correlaciones, distribuciones, outliers
     │
     ▼
[7] División Train/Test (75% / 25%, random_state=8)
     │
     ▼
[8] Entrenamiento: LinearRegression → HuberRegressor
     │
     ▼
[9] Evaluación con MAE + análisis de errores (curtosis)
     │
     ▼
[10] Estandarización + reentrenamiento + análisis de coeficientes
     │
     ▼
Modelo Final + Conclusiones
```

### Análisis Exploratorio (EDA)

**Correlaciones con el precio:**

| Variable | Correlación con `price` |
|:---------|:-----------------------:|
| `nb_rooms` | **0.30** (más alta) |
| `nb_bedrooms` | 0.27 |
| `nb_bathrooms` | 0.22 |
| `size` | ~0.15 |
| `approximate_latitude` | ~0.05 |

> 💡 El número de habitaciones es el predictor numérico más fuerte del precio, lo que tiene sentido intuitivo: más habitaciones → mayor espacio → mayor valor.

**Distribución del precio:**
- Distribución sesgada a la derecha (cola larga en precios altos)
- 97.5% de propiedades: precio ≤ 1,275,000
- 2.5% de outliers (932 propiedades): precio > 1,275,000

---

## 🤖 Modelado y Técnicas Utilizadas

### Modelo 1: Regresión Lineal Ordinaria (OLS)

La regresión lineal clásica minimiza la **suma de errores al cuadrado**, lo que la hace muy sensible a valores extremos.

**Problema detectado:** Los errores presentaron **alta curtosis** (distribución leptocúrtica), confirmando que los outliers estaban distorsionando el modelo.

```python
lr = LinearRegression().fit(X_train, y_train)
MAE: 182,847
```

### Modelo 2: HuberRegressor (Regresión Robusta) ✅ Seleccionado

El HuberRegressor combina lo mejor de dos mundos:
- Para errores pequeños: se comporta como mínimos cuadrados (OLS)
- Para errores grandes: usa una función lineal que reduce el impacto de outliers

```python
hr = HuberRegressor(
    epsilon=1,      # Sensibilidad a outliers (más bajo = más robusto)
    max_iter=1000,
    alpha=0,        # Sin regularización
    fit_intercept=True
)
MAE: 169,465
```

### ¿Por qué HuberRegressor?

| Criterio | LinearRegression | HuberRegressor |
|:---------|:----------------:|:--------------:|
| Robustez ante outliers | ❌ Baja | ✅ Alta |
| Interpretabilidad | ✅ Alta | ✅ Alta |
| Velocidad de entrenamiento | ✅ Muy rápida | ✅ Rápida |
| MAE (error promedio) | 182,847 | **169,465** |
| Adecuación para datos inmobiliarios | ⚠️ Limitada | ✅ Excelente |

---

## 📈 Resultados y Métricas

### Métrica Principal: MAE (Error Absoluto Medio)

> El MAE mide cuánto se equivoca el modelo en promedio. Un MAE de 169,465 significa que las predicciones están a ~169,465 unidades del precio real.

| Modelo | MAE | Reducción |
|:-------|:---:|:---------:|
| Regresión Lineal (baseline) | 182,847 | — |
| **HuberRegressor (final)** | **169,465** | **↓ 7.3%** |

### Variables Más Influyentes (Coeficientes Estandarizados)

**Aumentan el precio:**
- `nb_rooms` → Más habitaciones = mayor valor
- `property_type_appartement` → Los apartamentos tienen mayor precio base
- `has_air_conditioning` → Comodidad valorada positivamente
- `nb_terraces` → Las terrazas añaden valor
- `nb_photos` → Más fotos correlacionan con propiedades premium

**Reducen el precio:**
- `property_type_viager` → Modalidad especial de venta (precio intrínsecamente menor)
- `property_type_maison` → Casas vs. apartamentos: ~27,264 unidades menos
- `property_type_divers` → Otras categorías: ~11,830 unidades menos

### Análisis de Errores

| Modelo | Curtosis de errores | Interpretación |
|:-------|:-------------------:|:---------------|
| Regresión Lineal | Alta | Distribución con colas pesadas → outliers afectan el modelo |
| **HuberRegressor** | **10.37** | Colas más controladas → mejor manejo de casos extremos |

> La curtosis de 10.37 indica que aún existen valores extremos en los errores, pero el modelo Huber los penaliza de forma menos agresiva que OLS.

---

## 📊 Visualizaciones Clave

### 1. Distribución de Precios (Histograma con Outliers)
```
Sugiere visualizar: sns.histplot con hue='aux'
Objetivo: Mostrar la separación entre precios normales y atípicos
Insight: El 97.5% de propiedades se concentra bajo 1,275,000
```

### 2. Mapa de Calor de Correlaciones
```
Sugiere visualizar: sns.heatmap con matriz de correlación
Objetivo: Identificar relaciones entre variables numéricas
Insight: nb_rooms tiene la correlación más alta con price (0.30)
```

### 3. Análisis de Pareto - property_type
```
Sugiere visualizar: Barras + línea de % acumulado
Objetivo: Justificar la reducción de cardinalidad
Insight: Pocas categorías representan el 99% de los datos
```

### 4. Comparación de Distribución de Errores
```
Sugiere visualizar: Dos histogramas lado a lado (LR vs Huber)
Objetivo: Mostrar cómo Huber reduce la asimetría de errores
Insight: Ambos centrados en 0, Huber con colas más controladas
```

### 5. Importancia de Variables (Coeficientes)
```
Sugiere visualizar: sns.barplot horizontal ordenado por coeficiente
Objetivo: Comunicar qué variables más impactan el precio
Insight: nb_rooms positivo, property_type_viager negativo
```

---

## 🏁 Conclusiones Finales

### Descubrimientos Principales

**1. Los outliers son el mayor enemigo de los modelos inmobiliarios**
El 2.5% de propiedades de lujo genera distorsiones significativas. La regresión robusta es la solución correcta para este tipo de datos.

**2. El número de habitaciones es el predictor más fuerte**
Con una correlación de 0.30, `nb_rooms` supera a variables como el tamaño en metros cuadrados, lo que sugiere que el mercado valora la funcionalidad sobre el espacio bruto.

**3. El tipo de propiedad importa más de lo esperado**
Los apartamentos (`appartement`) tienen precios base más altos que las casas (`maison`) en este dataset, reflejando posiblemente la preferencia por ubicaciones urbanas centrales.

**4. Las fotos del anuncio correlacionan con el precio**
`nb_photos` tiene coeficiente positivo, lo que puede indicar que propiedades de mayor valor reciben más inversión en marketing.

### Decisiones que Habilita este Análisis

| Actor | Decisión habilitada |
|:------|:--------------------|
| 🏦 Banco / Aseguradora | Valoración de garantías hipotecarias con menor error |
| 🏢 Agencia Inmobiliaria | Precio de lista recomendado para nuevos inmuebles |
| 🏗️ Desarrollador | Qué características añadir para maximizar valor de reventa |
| 💼 Inversor | Identificar propiedades subvaloradas (precio real < predicción) |

### Limitaciones

- El dataset es estático (snapshot en el tiempo); los precios inmobiliarios son dinámicos
- La variable geográfica (lat/long) captura ubicación pero no factores cualitativos del vecindario
- El MAE de ~169,465 sigue siendo alto en términos absolutos para propiedades de menor valor

### Mejoras Futuras

- [ ] Incorporar datos de vecindario (escuelas, transporte, criminalidad)
- [ ] Probar modelos de ensemble: Random Forest, Gradient Boosting, XGBoost
- [ ] Añadir variables temporales (fecha de publicación, tendencias del mercado)
- [ ] Implementar validación cruzada k-fold para estimaciones más robustas
- [ ] Explorar modelos espaciales (kriging, SAR) para capturar dependencia geográfica

---

## 💡 Impacto del Proyecto

```
┌─────────────────────────────────────────────────────────┐
│                    IMPACTO ESTIMADO                     │
├─────────────────────────────────────────────────────────┤
│  Reducción de error:  182,847 → 169,465 (-7.3% MAE)    │
│  Propiedades analizadas: ~37,368                        │
│  Outliers detectados:  932 propiedades (2.5%)           │
│  Variables eliminadas: 11 (con justificación técnica)   │
│  Categorías reducidas: property_type → top 99% Pareto  │
└─────────────────────────────────────────────────────────┘
```

**¿Por qué este proyecto es relevante profesionalmente?**

Este proyecto demuestra la capacidad de:
1. **Diagnosticar** por qué un modelo estándar falla (curtosis alta en errores)
2. **Elegir** la herramienta correcta para el problema (regresión robusta)
3. **Justificar** cada decisión de preprocesamiento con métricas y referencias
4. **Comunicar** los resultados en términos de impacto de negocio

No se trata solo de código que funciona. Se trata de **pensar como un Data Scientist** que resuelve problemas reales.

---

## 🛠️ Tecnologías Utilizadas

| Categoría | Herramienta | Uso |
|:----------|:------------|:----|
| Lenguaje | ![Python](https://img.shields.io/badge/-Python-3776AB?logo=python&logoColor=white) | Análisis y modelado |
| Manipulación de datos | ![Pandas](https://img.shields.io/badge/-Pandas-150458?logo=pandas&logoColor=white) | Limpieza, transformación, EDA |
| Operaciones numéricas | ![NumPy](https://img.shields.io/badge/-NumPy-013243?logo=numpy&logoColor=white) | Cálculos vectorizados |
| Machine Learning | ![Scikit-Learn](https://img.shields.io/badge/-Scikit--Learn-F7931E?logo=scikit-learn&logoColor=white) | LinearRegression, HuberRegressor, StandardScaler |
| Visualización | ![Matplotlib](https://img.shields.io/badge/-Matplotlib-11557c) | Gráficos base |
| Visualización | ![Seaborn](https://img.shields.io/badge/-Seaborn-4C72B0) | Heatmaps, distribuciones, PairGrid |
| Estadística | ![SciPy](https://img.shields.io/badge/-SciPy-8CAAE6?logo=scipy&logoColor=white) | Curtosis, asimetría |
| Entorno | ![Google Colab](https://img.shields.io/badge/-Google%20Colab-F9AB00?logo=googlecolab&logoColor=white) | Desarrollo y ejecución |

---

## 📁 Estructura del Repositorio

```
📦 prediccion-precios-inmobiliarios/
├── 📓 Análisis_Predictivo_de_Precios_Inmobiliario.ipynb   # Notebook principal
├── 📄 README.md                                            # Este archivo
├── 📂 data/
│   ├── X.csv                                              # Variables independientes
│   └── y.csv                                              # Variable objetivo (precios)
└── 📂 images/
    ├── distribucion_precios.png
    ├── heatmap_correlaciones.png
    ├── pareto_property_type.png
    ├── comparacion_errores.png
    └── importancia_variables.png
```

---

## 👤 Autor

**[Tu Nombre]**  
Data Scientist | Machine Learning | Análisis Predictivo

[![LinkedIn](https://img.shields.io/badge/LinkedIn-Conectar-0A66C2?style=for-the-badge&logo=linkedin&logoColor=white)](https://linkedin.com/in/tu-perfil)
[![GitHub](https://img.shields.io/badge/GitHub-Ver%20más%20proyectos-181717?style=for-the-badge&logo=github&logoColor=white)](https://github.com/tu-usuario)

---

<div align="center">

*Si este proyecto te resultó útil o interesante, considera darle una ⭐*

**[⬆ Volver al inicio](#)**

</div>
