# Análisis Predictivo de Precios Inmobiliarios


Proyecto de regresión para predecir precios de mercado de propiedades inmobiliarias, con énfasis en **robustez ante valores atípicos** e **interpretabilidad de los resultados**.

---

## Objetivo

Construir un modelo predictivo capaz de estimar el precio de una propiedad a partir de sus características estructurales y tipológicas, eligiendo un enfoque que tolere los outliers inherentes al mercado inmobiliario (propiedades de lujo, errores de registro) sin sacrificar la interpretabilidad del modelo.

---

## Resultados Principales

| Modelo | MAE | Mejora |
|---|---|---|
| Regresión Lineal Clásica | 182,847 | — |
| **HuberRegressor** | **169,465** | **↓ 7.3%** |

> El HuberRegressor redujo el error absoluto medio en un **7.3%** frente a la regresión lineal estándar, siendo más robusto ante el 2.5% de propiedades que superan 1.275M en precio.

---

## Metodología

### Análisis Exploratorio y Limpieza de Datos
- Imputación de valores nulos con la **mediana** para variables numéricas (`size`, `nb_rooms`).
- Eliminación de variables con alta cardinalidad (`city`, `postal_code`) o exceso de nulos para evitar sobreajuste.
- Detección de outliers en precios: el 2.5% de propiedades superan 1.275M de unidades monetarias.

### Ingeniería de Características
- **Análisis de Pareto** sobre `property_type`: se conservaron 9 categorías que representan el 99% de los datos, reduciendo ruido categórico.
- *One-Hot Encoding* sobre las categorías resultantes.
- Conversión de variables binarias al formato booleano correcto.
- **Estandarización** con `StandardScaler` para mejorar convergencia del modelo.

### Selección de Variables
- `nb_rooms` mostró la mayor correlación con el precio (r = 0.30).
- Variables geográficas (`approximate_latitude`) y `size` descartadas por baja relevancia predictiva.

### Modelado y Evaluación
- Comparación entre `LinearRegression` y `HuberRegressor`.
- Evaluación del supuesto de normalidad en residuos mediante **curtosis** y visualizaciones empíricas.
- Elección justificada del HuberRegressor por su función de pérdida híbrida (cuadrática para errores pequeños, lineal para grandes).

---

## Conclusiones e Interpretación de Negocio

Los coeficientes del modelo permiten cuantificar el impacto de cada variable en el precio:

| Factor | Dirección | Interpretación |
|---|---|---|
| `nb_rooms` | Aumenta precio | Mayor número de habitaciones agrega valor |
| `property_type: propriété` | Aumenta precio | Propiedades exclusivas tienen alta valorización |
| `property_type: appartement` | Aumenta precio | Los apartamentos presentan precio superior a la categoría base |
| `property_type: viager` | Reduce precio | Derecho de uso vitalicio reduce significativamente el valor de mercado |
| `property_type: maison` | Reduce precio | Casas tienden a valor menor frente a la categoría base |
| `nb_boxes` | Reduce precio | Posiblemente asociado a ubicaciones menos urbanizadas |

---

## Tecnologías Utilizadas

- **Lenguaje:** Python 3.10+
- **Manipulación de datos:** Pandas, NumPy, SciPy
- **Machine Learning:** Scikit-Learn (`HuberRegressor`, `LinearRegression`, `StandardScaler`)
- **Visualización:** Matplotlib, Seaborn

---

## Datos

El dataset utilizado es de carácter académico/personal. Para replicar el análisis, pueden usarse datasets similares disponibles públicamente en [Kaggle](https://www.kaggle.com/) (ej. *France Real Estate*) o adaptar el pipeline a un conjunto de datos propio.

---

## Autor

**Franklin Manuel Manjarres**  
Matemático | Machine Learning Enthusiast  
Orientado a soluciones prácticas, robustas e interpretables para problemas de negocio.

Si tienes feedback o buscas talento con sólida base analítica en Python y Scikit-Learn, no dudes en contactarme.
