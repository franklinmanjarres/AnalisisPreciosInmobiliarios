# Modelado de Regresión Robusta y Machine Learning para la Predicción de Precios Inmobiliarios (PropTech)

![portada](assets/Portada_inmobiliario.png)

> Pipeline end-to-end en Python para optimizar la tasación automatizada de activos residenciales, mitigando el impacto de outliers mediante regresión robusta y manteniendo la interpretabilidad comercial de las variables.

![Python version](https://img.shields.io/badge/Python-3.8%2B-blue)
![License](https://img.shields.io/badge/License-MIT-green)
![scikit-learn](https://img.shields.io/badge/scikit--learn-1.3%2B-orange)
![pandas](https://img.shields.io/badge/pandas-2.0%2B-lightgrey)

---

## 📌 Tabla de contenidos

1. [Resumen ejecutivo](#1-resumen-ejecutivo)
2. [Problema de negocio](#2-problema-de-negocio)
3. [Metodología end-to-end y habilidades técnicas](#3-metodología-end-to-end-y-habilidades-técnicas)
4. [Resultados y métricas](#4-resultados-y-métricas)
5. [Análisis de coeficientes y recomendaciones estratégicas](#5-análisis-de-coeficientes-y-recomendaciones-estratégicas)
6. [Limitaciones y próximos pasos](#6-limitaciones-y-próximos-pasos)
7. [Instalación y uso](#7-instalación-y-uso)
8. [Estructura del proyecto](#8-estructura-del-proyecto)
9. [Tecnologías](#9-tecnologías)
10. [Autor](#10-autor)

---

## 1. Resumen ejecutivo

- **Impacto cuantificado:** `HuberRegressor` redujo el MAE un **7.3%**, de 182,847 USD a **169,465 USD**.
- **Problema:** precios extremos (penthouses, mansiones) distorsionan modelos clásicos.
- **Solución:** pipeline automatizado con limpieza, feature engineering (filtro Pareto 99%) y estandarización.
- **Próximos pasos:** modelos tree-based (RandomForest, XGBoost) y enriquecimiento con variables urbanas.

---

## 2. Problema de negocio

Estimaciones erróneas generan pérdidas para:
- **Agencias:** activos mal posicionados.
- **Compradores:** riesgo de sobrepago.
- **Bancos:** valoración inexacta de garantías hipotecarias.

Pregunta central: ¿Podemos predecir el valor real usando dimensiones físicas y ubicación? Sí, con robustez frente a outliers.

---

## 3. Metodología end-to-end y habilidades técnicas

Datos: 37,368 registros.

1. **Limpieza:** eliminación de columnas con alta tasa de nulos (`exposition`, `floor`, `land_size`, `ghg_category`, eficiencia energética).
2. **Feature engineering:**
   - Filtro Pareto 99% en `property_type`.
   - One-hot encoding con `pd.get_dummies()`.
   - Estandarización con `StandardScaler`.
3. **Validación:** Train 75% / Test 25%.
4. **Modelado:** `LinearRegression` vs `HuberRegressor`. Análisis de residuos con SciPy.

**Habilidades:** ML supervisado, regresión robusta, tratamiento de outliers, feature engineering, estandarización, validación y evaluación.

---

## 4. Resultados y métricas

Métrica principal: **MAE** (USD).

| Modelo                | MAE (USD)   | Comportamiento frente a outliers |
|-----------------------|-------------|----------------------------------|
| LinearRegression      | 182,847     | Vulnerable                       |
| **HuberRegressor**    | **169,465** | **Robusto** (−7.3% error)        |

---

## 5. Análisis de coeficientes y recomendaciones estratégicas

- `property_type_maison`: **−27,264 USD** → casas valen 27k menos que apartamentos.  
  → Priorizar adquisición/remodelación de apartamentos en zonas urbanas.
- `viager`: **−10,671 USD** → aplicar descuento automático por restricciones legales.
- `nb_boxes`: **−2,052 USD** por garaje → en zonas densas, garajes no incrementan precio.
- `nb_rooms`, terrazas, balcones, aire acondicionado: **positivos** → invertir en estos para maximizar ROI.

---

## 6. Limitaciones y próximos pasos

**Limitaciones:**
- Modelo lineal no captura interacciones no lineales complejas.
- Umbral de Huber configurado manualmente.
- Alta nulidad en variables críticas (eficiencia energética).

**Próximos pasos:**
1. Modelos no lineales: `RandomForestRegressor`, `XGBoost`.
2. Optimización de hiperparámetros (`GridSearchCV` / `RandomizedSearchCV`).
3. Enriquecimiento con APIs de geolocalización (distancia a puntos de interés).

---

## 7. Instalación y uso

### Requisitos previos

- Python 3.8+
- pip o conda

### Instalación

```bash
# Clonar repositorio
git clone https://github.com/tu-usuario/proyecto-propext.git
cd proyecto-propext

# Crear entorno virtual (opcional pero recomendado)
python -m venv venv
source venv/bin/activate  # En Windows: venv\Scripts\activate

# Instalar dependencias
pip install -r requirements.txt
```

### Ejecución del pipeline

```bash
# Entrenar modelo completo
python src/train.py --config config/train.yaml

# Evaluar modelo en test
python src/evaluate.py --model-path models/huber_model.pkl --test-path data/test.csv

# Generar informes
python src/report.py --output reports/
```

### Notebook mínimo reproducible

```bash
jupyter notebook notebooks/01_minimal_reproducible_example.ipynb
```

---

## 8. Estructura del proyecto

```text
proyecto-propext/
├── data/
│   ├── raw/
│   └── processed/
├── src/
│   ├── train.py
│   ├── evaluate.py
│   └── report.py
├── notebooks/
│   └── 01_minimal_reproducible_example.ipynb
├── models/
│   └── huber_model.pkl
├── assets/
│   └── Portada_inmobiliario.png
├── config/
│   └── train.yaml
├── reports/
│   └── results/
├── requirements.txt
├── LICENSE
└── README.md
```

---

## 9. Tecnologías

- Python 3.8+
- scikit-learn 1.3+
- pandas 2.0+
- numpy 1.24+
- matplotlib 3.7+
- seaborn 0.12+
- SciPy 1.10+

---

## 10. Autor

**Franklin Manjarres**  
✉️ [manjarresfranklin587@gmail.com](mailto:manjarresfranklin587@gmail.com)  
💼 [LinkedIn](https://www.linkedin.com/in/franklinmanjarres/)

*Proyecto desarrollado con enfoque comercial en soluciones de ML para PropTech.*

---

## 📄 Licencia

Este proyecto está bajo la licencia MIT. Ver [LICENSE](LICENSE) para más detalles.
