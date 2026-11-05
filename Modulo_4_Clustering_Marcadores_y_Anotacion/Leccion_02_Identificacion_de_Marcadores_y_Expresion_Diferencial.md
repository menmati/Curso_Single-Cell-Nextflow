[⬅️ Anterior: Lección 10 (Louvain y Leiden)](Leccion_01_Algoritmos_de_Clustering_de_Grafos_Louvain_y_Leiden.md) | [🏠 Temario Principal](../README.md) | [Siguiente: Lección 12 (Anotación Manual vs. Automatizada) ➡️](Leccion_03_Estrategias_de_Anotacion_Manual_vs_Automatizada.md)

---

# Lección 02: Detección de Genes Marcadores y Expresión Diferencial

---

## 1. El Reto Estadístico del Dropout en Marcadores

En scRNA-seq, un gen con un valor de conteo de 0 en una célula puede deberse a dos causas:

1. **Verdad biológica:** El gen realmente no se transcribe en esa célula.
2. **Abandono técnico (*dropout*):** El ARNm sí estaba presente en la célula, pero no fue capturado durante la transcripción inversa en la microgota (la eficiencia de captura de ARNm en 10x ronda el 10-30%).

Por tanto, los tests estadísticos clásicos de bulk RNA-Seq (como t-Student o DESeq2 clásico) no son directamente aplicables a nivel de células individuales sin adaptaciones especiales.

---

## 2. Métodos de Contraste Estadístico

### A) Wilcoxon Rank-Sum Test (Estándar por defecto)

Es una prueba no paramétrica basada en rangos. No asume una distribución normal de la expresión génica y es extremadamente robusta frente a outliers:

```R
# Búsqueda de marcadores para todos los clusters mediante Wilcoxon
marcadores <- FindAllMarkers(
  obj,
  only.pos = TRUE,
  min.pct = 0.25,
  logfc.threshold = 0.25,
  test.use = "wilcox"
)
```

* `min.pct = 0.25`**:** Exige que el gen se detecte en al menos el 25% de las células del cluster evaluado (elimina genes ruidosos expresados en 2 o 3 células).
* `logfc.threshold = 0.25`**:** Exige una diferencia de expresión media de al menos log2(1.2) ≈ 0.25.
* `only.pos = TRUE`**:** Retiene únicamente marcadores sobreexpresados (positivos), que son los útiles para definir la identidad celular.

### B) Modelo Bi-fásico MAST (*Model-based Analysis of Single-cell Transcriptomics*)

Desarrollado por Finak et al. (*Genome Biology*, 2015), **MAST** es un modelo estadístico tipo **Hurdle (de dos partes)** diseñado específicamente para scRNA-seq:

1. **Parte Binaria:** Modela mediante regresión logística la probabilidad de que el gen se exprese (conteo > 0) o no (conteo = 0).
2. **Parte Continua:** Modela el nivel de expresión condicional en aquellas células donde el gen sí fue detectado, ajustando por la tasa global de detección celular (*cellular detection rate - CDR*).

```R
# Ejecución con MAST (muy recomendado para contrastes finos de estados celulares)
marcadores_mast <- FindMarkers(
  obj,
  ident.1 = "Cluster_0",
  ident.2 = "Cluster_1",
  test.use = "MAST",
  latent.vars = "nCount_RNA"
)
```

---

## 3. Visualización Diagnóstica de Marcadores

Para confirmar visualmente que un gen es un marcador genuino de un cluster y no un artefacto estadístico, se emplean cuatro gráficos fundamentales:

1. `FeaturePlot`**:** Proyecta la intensidad de expresión del gen directamente sobre el mapa UMAP 2D.
2. `VlnPlot`**:** Muestra la distribución de expresión del gen en cada cluster (visualizando tanto el porcentaje de ceros como la densidad de expresión positiva).
3. `DotPlot`**:** La figura más compacta y publicada en artículos científicos. Codifica simultáneamente dos dimensiones:
   * El tamaño del círculo representa el porcentaje de células que expresan el gen (`Pct.Exp`).
   * El color del círculo representa la intensidad media de expresión (`Average Expression`).
4. `DoHeatmap`**:** Mapa de calor de los mejores 10 genes marcadores por cluster sobre las células agrupadas.

```R
# DotPlot de los principales marcadores inmunes
genes_clave <- c("CD3D", "CD4", "CD8A", "MS4A1", "CD14", "FCGR3A", "NKG7", "EPCAM")
DotPlot(obj, features = genes_clave) + RotatedAxis()
```

---

[⬅️ Anterior: Lección 10 (Louvain y Leiden)](Leccion_01_Algoritmos_de_Clustering_de_Grafos_Louvain_y_Leiden.md) | [🏠 Temario Principal](../README.md) | [Siguiente: Lección 12 (Anotación Manual vs. Automatizada) ➡️](Leccion_03_Estrategias_de_Anotacion_Manual_vs_Automatizada.md)
