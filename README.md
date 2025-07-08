## Predictivo de Precios Inmobiliarios

Este proyecto aplica técnicas de aprendizaje automático para predecir el precio de propiedades inmobiliarias. Se utilizan regresión lineal y HuberRegressor, junto con selección de variables y tratamiento de valores atípicos.

---

##  Objetivo del Proyecto

Elegir y justificar un modelo de regresión adecuado al problema y a las evidencias analíticas para mejorar la precisión en la predicción del precio de inmuebles.

---

### Modelos utilizados

- **Regresión Lineal**: Sirve como modelo base.
- **HuberRegressor**: Modelo robusto frente a valores atípicos, que logró reducir el MAE (Error Absoluto Medio) de **182,847** a **169,465**, manteniendo interpretabilidad y rendimiento.

---

###  Preprocesamiento de Datos

- **Eliminación de columnas de alta cardinalidad** (e.g., `city`, `postal_code`) por problemas con one-hot encoding.
- **Limpieza de columnas con valores nulos o redundantes**.
- **Selección de variables relevantes** mediante análisis de correlación y significancia.
- **Transformación de variables categóricas**.

---

###  Evaluación del modelo

Se utilizó el **Mean Absolute Error (MAE)** como métrica principal para comparar modelos. El modelo HuberRegressor demostró mayor robustez frente a outliers sin perder interpretabilidad.

---

###  Tecnologías usada

- **Python**: Lenguaje principal de implementación.
- **Pandas**: Manipulación y limpieza de datos.
- **NumPy**: Operaciones numéricas eficientes.
- **Matplotlib** y **Seaborn**: Visualización de datos.
- **Scikit-learn**: Implementación de modelos de regresión, evaluación, preprocesamiento y selección de características.
- **Jupyter Notebook**: Desarrollo interactivo del proyecto.






