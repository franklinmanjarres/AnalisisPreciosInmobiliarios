# Predicción de Precios de Vivienda — Mercado Inmobiliario Francés

**Sector:** Real Estate  
**Técnica:** Regresión supervisada con Machine Learning  
**Datos:** 37,368 registros de viviendas en Francia  
**Resultado:** MAE de 166,935 € — mejora del 19.7% sobre la línea base

---

## El problema

Las plataformas inmobiliarias necesitan estimar el precio de venta de una propiedad antes de publicarla. Una estimación imprecisa genera pérdidas para el vendedor o alarga el tiempo de venta. Este proyecto construye un modelo que predice el precio a partir de características físicas, energéticas y de ubicación del inmueble.

---

## Resultados

| Modelo | MAE validación |
|---|---|
| Línea base (predecir la media) | 207,801 € |
| Regresión Lineal estandarizada | 178,217 € |
| HuberRegressor sin estandarizar | 198,268 € |
| HuberRegressor estandarizado | 165,223 € |

**Modelo final — HuberRegressor estandarizado**

- MAE: 166,935 €
- RMSE: 289,054 €
- Mejora sobre línea base: 19.7%

> MAE es el error promedio por vivienda. RMSE penaliza más los errores grandes — la diferencia entre ambos (122,000 €) refleja el impacto de las viviendas de lujo que el modelo no puede predecir bien.

---

## Metodología

**1. Limpieza** — Se eliminó `city` (8,643 categorías únicas). Nulos imputados con mediana y moda.

**2. Selección numérica** — Correlación de Pearson con `price`. Se descartaron 9 variables con |r| < 0.05.

**3. Árbol de Decisión como preprocesamiento** — Se binarizó `price` (umbral = mediana 255,250 €) y se entrenó un `DecisionTreeClassifier` para identificar las 6 variables numéricas con mayor poder discriminativo: `postal_code`, `nb_rooms`, `nb_bedrooms`, `approximate_longitude`, `floor`, `nb_terraces`.

**4. Codificación** — `OneHotEncoder` sobre 4 variables categóricas → 47 columnas finales.

**5. Estandarización** — `StandardScaler` aplicado antes del modelado. Este fue el paso con mayor impacto: el HuberRegressor pasó de 198,268 € a 165,223 € de MAE solo por escalar correctamente las variables.

**6. Validación** — KFold de 10 particiones para todos los modelos. Partición 75% entrenamiento / 25% validación.

---

## Visualizaciones

### Correlación de variables con el precio
![Correlación con price](imagenes/correlacion_precio.png)

### Matriz de correlación entre predictoras
![Matriz de correlación](imagenes/matriz_correlacion.png)

### Selección por Árbol de Decisión
![Importancia de variables](imagenes/importancia_arbol.png)

### Comparación de modelos
![Comparación de modelos](imagenes/comparacion_modelos.png)

### Distribución de residuos
![Residuos](imagenes/residuos.png)

### Variables más influyentes
![Variables influyentes](imagenes/variables_influyentes.png)

---

## Conclusiones

La ubicación (`postal_code`) es el factor más determinante del precio, con una importancia tres veces superior a la segunda variable. El modelo predice bien viviendas de precio normal pero falla con el segmento de lujo (precio > 1 M€), que concentra los errores más grandes. Para ese segmento se necesitarían variables adicionales que el dataset no incluye.

---

## Tecnologías

Python 3.12, pandas, numpy, matplotlib, seaborn, scikit-learn, scipy

---

## Publicación LinkedIn

> Construí un modelo para predecir el precio de viviendas en Francia con 37,368 registros y lo que más me sorprendió fue lo que pasó con la estandarización.
>
> El HuberRegressor sin escalar las variables: MAE de 198,268 €.
> El mismo modelo con StandardScaler: 165,223 €.
> Una diferencia de 33,000 € solo por poner las variables en la misma escala.
>
> El pipeline incluyó limpieza, selección por correlación, un Árbol de Decisión como preprocesamiento (no como modelo final), codificación One-Hot y validación cruzada de 10 folds.
>
> Resultado final: MAE de 166,935 € — mejora del 19.7% sobre la línea base.
>
> Proyecto documentado en GitHub con metodología, visualizaciones y resultados.
> [enlace al repositorio]

**Imagen de portada recomendada:** `comparacion_modelos.png` — muestra números concretos y criterio técnico en una sola imagen.

---

## Cómo exportar las imágenes del notebook

Agregar antes de cada `plt.show()`:
```python
plt.savefig('imagenes/nombre.png', dpi=150, bbox_inches='tight')
```

Imágenes a exportar:
- `correlacion_precio.png` — sección 4.1
- `matriz_correlacion.png` — sección 4.2
- `importancia_arbol.png` — sección 6
- `comparacion_modelos.png` — sección 10.6
- `residuos.png` — sección 11
- `variables_influyentes.png` — sección 12

---

## Estructura del repositorio

```
Proyecto_Precios_Vivienda_Francia/
├── README.md
└── imagenes/
    ├── correlacion_precio.png
    ├── matriz_correlacion.png
    ├── importancia_arbol.png
    ├── comparacion_modelos.png
    ├── residuos.png
    └── variables_influyentes.png
```

---

## Autor

Franklin — Universidad del Valle, Cali, Colombia  
Curso: Machine Learning & AI for the Working Analyst — Colegio de Matemáticas Bourbaki
