# Modelado de Regresión Robusta y Machine Learning para la Predicción de Precios Inmobiliarios (PropTech)

![portada](assets/Portada_inmobiliario.png)

> Pipeline end-to-end en Python para optimizar la tasación automatizada de activos residenciales, mitigando el impacto de outliers mediante regresión robusta y manteniendo la interpretabilidad comercial de las variables.

---

## 1. Resumen ejecutivo

- Impacto cuantificado: `HuberRegressor` redujo el MAE un 7.3%, de 182,847 USD a 169,465 USD.
- Problema: precios extremos (penthouses, mansiones) distorsionan modelos clásicos.
- Solución: pipeline automatizado con limpieza, feature engineering (filtro Pareto 99%) y estandarización.
- Próximos pasos: modelos tree-based (RandomForest, XGBoost) y enriquecimiento con variables urbanas.

---

## 2. Problema de negocio

Estimaciones erróneas generan pérdidas para:
- Agencias: activos mal posicionados.
- Compradores: riesgo de sobrepago.
- Bancos: valoración inexacta de garantías hipotecarias.

Pregunta central: ¿Podemos predecir el valor real usando dimensiones físicas y ubicación? Sí, con robustez frente a outliers.

---

## 3. Metodología end-to-end y habilidades técnicas

Datos: 37,368 registros.

1. Limpieza: eliminación de columnas con alta tasa de nulos (exposition, floor, land_size, ghg_category, eficiencia energética).
2. Feature engineering:
   - Filtro Pareto 99% en property_type.
   - One-hot encoding con pd.get_dummies().
   - Estandarización con StandardScaler.
3. Validación: Train 75% / Test 25%.
4. Modelado: LinearRegression vs HuberRegressor. Análisis de residuos con SciPy.

Habilidades: ML supervisado, regresión robusta, tratamiento de outliers, feature engineering, estandarización, validación y evaluación.

---

## 4. Resultados y métricas

Métrica principal: MAE (USD).

| Modelo                | MAE (USD)   | Comportamiento frente a outliers |
|-----------------------|-------------|----------------------------------|
| LinearRegression      | 182,847     | Vulnerable                       |
| HuberRegressor        | 169,465     | Robusto (−7.3% error)            |

---

## 5. Análisis de coeficientes y recomendaciones estratégicas

- property_type_maison: −27,264 USD → casas valen 27k menos que apartamentos.  
  → Priorizar adquisición/remodelación de apartamentos en zonas urbanas.
- viager: −10,671 USD → aplicar descuento automático por restricciones legales.
- nb_boxes: −2,052 USD por garaje → en zonas densas, garajes no incrementan precio.
- nb_rooms, terrazas, balcones, aire acondicionado: positivos → invertir en estos para maximizar ROI.

---

## 6. Limitaciones y próximos pasos

Limitaciones:
- Modelo lineal no captura interacciones no lineales complejas.
- Umbral de Huber configurado manualmente.
- Alta nulidad en variables críticas (eficiencia energética).

Próximos pasos:
1. Modelos no lineales: RandomForestRegressor, XGBoost.
2. Optimización de hiperparámetros (GridSearchCV / RandomizedSearchCV).
3. Enriquecimiento con APIs de geolocalización (distancia a puntos de interés).

---

## Tecnologías

- Python 3.8+
- scikit-learn 1.3+
- pandas 2.0+
- numpy 1.24+
- matplotlib 3.7+
- seaborn 0.12+
- SciPy 1.10+

---

## Autor

Franklin Manjarres  
manjarresfranklin587@gmail.com  
LinkedIn: https://www.linkedin.com/in/franklinmanjarres/

Proyecto desarrollado con enfoque comercial en soluciones de ML para PropTech.
