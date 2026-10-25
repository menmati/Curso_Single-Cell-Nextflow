[⬅️ Anterior: Test Módulo 01](../Modulo_1_Fundamentos_y_Preprocesamiento_Nextflow/Test_Autoevaluacion_M1.md) | [🏠 Temario Principal](../README.md) | [Siguiente: Lección 05 (EmptyDrops) ➡️](Leccion_02_Deteccion_de_Gotas_Vacias_con_EmptyDrops.md)

---

# Lección 01: Métricas de QC Celular y Umbrales Adaptativos (MAD)

---

## 1. Las Tres Métricas Cardinales del QC en Célula Única

Al construir un objeto Seurat a partir de una matriz de conteo, se evalúan tres variables esenciales para cada código celular:

1. `nFeature_RNA` **(Número de genes únicos detectados):**
   * *Valores muy bajos:* Gotas vacías, células degradadas o fragmentos de ARN de baja complejidad.
   * *Valores anómalamente altos:* Células gigantes, núcleos poliploides o, frecuentemente, **dobletes celulares**.
2. `nCount_RNA` **(Número total de moléculas UMI detectadas):**
   * Cuantifica la profundidad de secuenciación efectiva alcanzada en esa célula. Correlaciona fuertemente con `nFeature_RNA`.
3. `percent.mt` **(Porcentaje de lecturas asignadas al genoma mitocondrial):**
   * Se calcula mediante la expresión regular `pattern = "^MT-"` (humanos) o `pattern = "^mt-"` (ratón):
   * *Significado biológico:* Cuando una célula sufre estrés mecánico o lisis de membrana durante la disociación tisular, el ARNm citoplasmático se escapa rápidamente a través de los poros de la membrana plasmática. Sin embargo, las mitocondrias (al tener doble membrana y mayor tamaño) quedan retenidas dentro de la célula residual. Por ello, una fracción mitocondrial elevada es un biomarcador característico de **células apoptóticas, dañadas o moribundas**.

---

## 2. El Error de los Umbrales Rígidos Universales

En la literatura temprana era común aplicar reglas rígidas como:  
*"Filtrar todas las células con percent.mt > 5% y nFeature < 200"*.

Aplicar esta regla a ciegas en biología computacional moderna es un grave error metodológico:

* **Células metabólicamente hiperactivas:** Cardiomiocitos, túbulos proximales renales, células musculares y subpoblaciones tumorales oxidativas tienen una masa mitocondrial fisiológica enorme, con porcentajes basales normales de 15-25% de ARNm mitocondrial. Un umbral rígido del 5% eliminaría al 100% de estas células funcionales.
* **Células quiescentes:** Los linfocitos vírgenes maduros son transcripcionalmente muy pequeños, con apenas 300–600 genes expresados.

---

## 3. Umbrales Adaptativos basados en la Desviación Absoluta de la Mediana (MAD)

Para evitar sesgos antropocéntricos, el estándar metodológico (recomendado por las guías *OSCA / Bioconductor*) es utilizar **umbrales adaptativos basados en distribuciones estadísticas robustas frente a outliers**.

Dado que las distribuciones de conteos no son gaussianas, la media y la desviación estándar son distorsionadas por valores extremos. En su lugar, se utiliza la **Mediana** y la **MAD** (*Median Absolute Deviation*):

$$
\text{MAD} = \text{mediana}\left(|X_i - \text{mediana}(X)|\right)
$$

Se define como célula atípica (*outlier*) aquella que se desvíe más de 3 o 5 veces la MAD de la distribución poblacional (en escala logarítmica para cuentas y lineal para porcentajes):

```R
library(scater) # o implementación nativa en R

# Umbral adaptativo superior para porcentaje mitocondrial (3 MADs por encima de la mediana)
med_mt <- median(obj$percent.mt)
mad_mt <- mad(obj$percent.mt)
corte_superior_mt <- med_mt + (3 * mad_mt)

# Filtrado conservando células viables
celulas_validas <- obj$percent.mt <= corte_superior_mt
```

---

[⬅️ Anterior: Test Módulo 01](../Modulo_1_Fundamentos_y_Preprocesamiento_Nextflow/Test_Autoevaluacion_M1.md) | [🏠 Temario Principal](../README.md) | [Siguiente: Lección 05 (EmptyDrops) ➡️](Leccion_02_Deteccion_de_Gotas_Vacias_con_EmptyDrops.md)
