
# Model Card — Regresión de ingreso anual

## 1. Detalles del modelo
- **Nombre:** Regresión lineal con target Box-Cox.
- **Versión:** 1.0.
- **Modelo:** `LinearRegression` dentro de un pipeline de scikit-learn.
- **Target engineering:** Box-Cox mediante `TransformedTargetRegressor`.
- **Validación:** KFold de 5 folds con shuffle y semilla 42.

## 2. Propósito
Estimar `income` anual a partir de variables demográficas y laborales para fines académicos.

**No usar como único criterio para decisiones de crédito, contratación, salarios, seguros u otras decisiones de alto impacto.**

## 3. Entradas
`age`, `hours_per_week`, `education`, `employer_type`, `marital_status`, `sex`, `race`, `born_in_us`.

## 4. Salida
Ingreso anual estimado en las mismas unidades monetarias de `income`.

## 5. Prevención de leakage
- Imputación numérica por mediana.
- Estandarización con `StandardScaler`.
- Imputación categórica por moda.
- One-hot encoding.
- Todos los pasos se ajustan dentro de cada fold.
- Box-Cox se ajusta sobre la target de entrenamiento de cada fold.

## 6. Métricas globales
- **MAE CV target original:** $31,104.43
- **MAE CV target Box-Cox:** $27,950.03
- **Mejora relativa del MAE:** 10.14%
- **MAE OOF modelo final:** $27,950.03
- **R² OOF:** 0.247

## 7. Métricas por sexo
- **Male:** MAE = $33,649.33; MAE/ingreso medio = 51.7%; n = 5,136
- **Female:** MAE = $21,932.02; MAE/ingreso medio = 50.9%; n = 4,864

## 8. Métricas adicionales por raza
- **White:** MAE = $31,078.95; n = 6,797
- **Black:** MAE = $19,618.83; n = 2,341
- **Asian:** MAE = $32,760.12; n = 432
- **Other or multiple:** MAE = $19,015.42; n = 430

## 9. Limitaciones y riesgos
- La regresión lineal impone una estructura simple.
- La target tiene una cola extrema y persisten errores altos en dólares.
- `sex` y `race` son atributos sensibles incluidos como entradas.
- Las diferencias de MAE por grupo no prueban causalidad ni discriminación por sí solas.
- Los subgrupos raciales pequeños producen estimaciones menos estables.
- La base puede reflejar sesgos históricos o de muestreo.
- No se evaluó desempeño fuera de la población representada por este dataset.

## 10. Monitoreo recomendado
Monitorear MAE global, MAE por subgrupos, drift de variables, cambios en la distribución de `income` y estabilidad de la transformación.
