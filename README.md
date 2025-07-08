
## Detección de Fraude en Canastas de Compra

Este proyecto implementa modelos de aprendizaje automático para detectar fraude en transacciones de compra, utilizando técnicas como Árboles de Decisión y Random Forest. El análisis se basa en un conjunto de datos reales, logrando resultados con más del **88% de precisión** en la clasificación de operaciones fraudulentas.

---

###  Objetivo del Proyecto

Desarrollar un modelo capaz de identificar canastas de compra potencialmente fraudulentas, basándose en atributos transaccionales y características del comportamiento del cliente.

---

###  Modelos utilizados

- **Árboles de Decisión (Decision Tree Classifier)**: Modelo base que permite interpretar fácilmente las reglas de clasificación.
- **Random Forest**: Modelo de ensamble que mejora la precisión y generalización al combinar múltiples árboles.

---

###  Metodología

- Análisis exploratorio del conjunto de datos.
- Limpieza y codificación de variables categóricas.
- Balanceo de clases si es necesario.
- Entrenamiento y validación cruzada.
- Evaluación del desempeño con métricas como:
  - Precisión (Accuracy)
  - Matriz de confusión
  - Curva ROC y AUC

---

###  Resultados

- Precisión obtenida: **≈ 88%**
- Random Forest mostró mejor desempeño que el árbol de decisión simple, con menor sobreajuste y mejor capacidad predictiva.

---

### Tecnologías usadas

- Python
- Pandas
- Scikit-learn
- Matplotlib
- Jupyter Notebook





Predicción de precios con regresión robusta (Huber








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

###  Tecnologías usadas
