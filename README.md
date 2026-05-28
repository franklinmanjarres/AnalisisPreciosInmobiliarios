# Modelado de Regresión Robusta y Machine Learning para Predicción de Precios Inmobiliarios (PropTech)

![portada](assets/Portada_inmobiliario.png)

> Pipeline end-to-end en Python para tasación automatizada de propiedades, mitigando outliers con regresión robusta y manteniendo interpretabilidad comercial.

---

## 1. Resumen Ejecutivo

![Comparación de MAE](assets/mae_comparison.png)

- **Resultado clave:** `HuberRegressor` redujo el MAE un **7.3%** (de 182,847 USD a **169,465 USD**).
- **Problema:** Outliers (penthouses, mansiones) distorsionan modelos lineales clásicos.
- **Solución:** Pipeline automatizado con limpieza, feature engineering (Pareto 99%) y estandarización.
- **Próximos pasos:** Modelos tree-based (XGBoost, RandomForest) + variables urbanas contextuales.

---

## 2. Problema de Negocio

Estimaciones erróneas generan pérdidas para:
- **Agencias:** activos estancados o margen reducido.
- **Compradores:** riesgo de sobrepago.
- **Bancos:** valoración inexacta de garantías hipotecarias.

**Pregunta central:** ¿Podemos predecir el valor real usando solo dimensiones físicas y ubicación? **Sí**, con robustez frente a outliers.

---

## 3. Metodología (37,368 registros)

1. **Limpieza:** Eliminación de columnas con >48% nulos (`exposition`, `floor`, `land_size`, `ghg_category`, eficiencia energética).
2. **Feature Engineering:**
   - Filtro Pareto 99% en `property_type` (redujo cardinalidad).
   - One-hot encoding + `StandardScaler`.
3. **Validación:** Train 75% / Test 25%.
4. **Modelado:** `LinearRegression` vs `HuberRegressor` + análisis de residuos (SciPy).

**Habilidades:** ML supervisado, regresión robusta, outliers, feature engineering, validación cruzada.

---

## 4. Resultados

| Modelo                | MAE (USD)   | Outliers       |
|-----------------------|-------------|----------------|
| LinearRegression      | 182,847     | Vulnerable     |
| **HuberRegressor**    | **169,465** | **Robusto**    |

---

## 5. Insights Comerciales (Coeficientes del Modelo)

| Variable              | Coeficiente  | Recomendación de Negocio                                      |
|-----------------------|--------------|---------------------------------------------------------------|
| `property_type_maison`| -27,264 USD  | Priorizar apartamentos urbanos; casas valen 27k menos.        |
| `viager`              | -10,671 USD  | Aplicar descuento automático por restricciones legales.       |
| `nb_boxes` (garaje)   | -2,052 USD   | Garajes no incrementan precio en zonas densas; no sobreevaluar.|
| `nb_rooms`, terrazas, aire acondicionado | + | Invertir en estos para maximizar ROI. |

---

## 6. Limitaciones y Próximos Pasos

**Limitaciones:**
- Modelo lineal no captura interacciones no lineales.
- Umbral de Huber configurado manualmente.
- Alta nulidad en variables críticas.

**Próximos pasos:**
1. Modelos no lineales: `RandomForestRegressor`, `XGBoost`.
2. Optimización de hiperparámetros (`GridSearchCV`).
3. Enricher datos con APIs de geolocalización (distancia a puntos de interés).

---

### Tecnologías
Python 3.x, scikit-learn, pandas, numpy, matplotlib, seaborn, SciPy.

### Autor
**Franklin Manjarres**  
manjarresfranklin587@gmail.com  
LinkedIn: https://www.linkedin.com/in/franklinmanjarres/

*Proyecto con enfoque comercial en ML para PropTech.*
