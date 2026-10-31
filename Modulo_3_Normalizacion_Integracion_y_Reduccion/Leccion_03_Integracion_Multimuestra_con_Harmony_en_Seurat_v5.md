[⬅️ Anterior: Lección 08 (PCA y Selección de PCs)](Leccion_02_Reduccion_de_Dimensionalidad_PCA_y_Seleccion_de_PCs.md) | [🏠 Temario Principal](../README.md) | [Siguiente: Ejercicios Prácticos M3 ➡️](Ejercicios_M3.md)

---

# Lección 03: Corrección de Efectos de Lote con Harmony en Seurat v5

---

## 1. El Problema del Efecto de Lote (*Batch Effect*)

Cuando se analiza un experimento con múltiples donantes, pacientes, réplicas o tecnologías (por ejemplo, biopsias de 10 pacientes tumorales):
* Las células del Paciente 1 se agrupan por separado de las del Paciente 2 en el espacio de PCA y UMAP, **incluso si ambas son linfocitos T biológicamente idénticos**.
* Este artefacto se debe a pequeñas diferencias en las condiciones de disociación, temperatura, lote de reactivos de microfluídica o corrida de secuenciación.

**Objetivo de la integración:** Alinear y solapar las poblaciones celulares biológicamente equivalentes entre lotes, pero **preservando las diferencias biológicas reales** (por ejemplo, diferencias entre pacientes respondedores y no respondedores a inmunoterapia).

---

## 2. La Arquitectura de Capas (*Layers*) en Seurat v5

Tradicionalmente, en Seurat v4, integrar múltiples muestras requería dividir el objeto en una lista de objetos individuales (`SplitObject`), normalizar cada uno por separado y ejecutar `FindIntegrationAnchors()`.

En **Seurat v5**, se introduce la arquitectura de **Capas (`Layers`)**, donde un único objeto Seurat contiene matrices de conteo desacopladas por muestra dentro del mismo ensayo:

```R
# Si partimos de un objeto Seurat con múltiples muestras en 'orig.ident':
obj[["RNA"]] <- split(obj[["RNA"]], f = obj$orig.ident)

# Al ejecutar SCTransform, Seurat v5 procesa cada capa de muestra de forma independiente
obj <- SCTransform(obj, assay = "RNA", vst.flavor = "v2", verbose = FALSE)

# Se ejecuta PCA sobre el espacio SCT conjunto
obj <- RunPCA(obj, assay = "SCT", npcs = 50, verbose = FALSE)
```

---

## 3. El Algoritmo Harmony y su Ejecución con `IntegrateLayers`

Desarrollado por Korsunsky et al. (*Nature Methods*, 2019), **Harmony** es uno de los algoritmos de integración más rápidos y eficientes de la bioinformática de célula única:
1. Proyecta las células en el espacio de PCA.
2. Utiliza un algoritmo de **clustering difuso (*fuzzy clustering*)** para identificar qué clusters celulares están presentes en múltiples lotes.
3. Para cada cluster, calcula un factor de corrección específico por lote y ajusta linealmente las coordenadas en el espacio PCA, empujando suavemente a las células hacia el centroide común del tipo celular sin distorsionar su heterogeneidad interna.

En Seurat v5, Harmony se ejecuta de manera elegante a través de la función envoltorio `IntegrateLayers`:

```R
library(harmony)

# Integración con Harmony en Seurat v5
obj <- IntegrateLayers(
  object = obj,
  method = HarmonyIntegration,
  normalization.method = "SCT",
  orig.reduction = "pca",
  new.reduction = "harmony",
  verbose = TRUE
)
```

El resultado es una nueva reducción dimensional llamada `harmony`, idéntica en dimensiones a PCA pero con los efectos de lote completamente corregidos.

---

## 4. Visualización No Lineal: UMAP (*Uniform Manifold Approximation and Projection*)

Para visualizar e interpretar el espacio corregido de 30 dimensiones de Harmony en un plano bidimensional (2D) interpretable para el ojo humano, utilizamos **UMAP**:

```R
# Cálculo de UMAP basado en el espacio integrado por Harmony
obj <- RunUMAP(
  obj,
  reduction = "harmony",
  dims = 1:30,
  reduction.name = "umap.harmony"
)

# Visualización comparativa
p1 <- DimPlot(obj, reduction = "umap.harmony", group.by = "orig.ident") + 
  ggtitle("Integración por Muestra (Buen solapamiento)")

p2 <- DimPlot(obj, reduction = "umap.harmony", group.by = "seurat_clusters") + 
  ggtitle("Poblaciones Celulares Identificadas")

p1 | p2
```

Si la integración ha tenido éxito:
* Las células de distintos pacientes o muestras se encuentran **homogéneamente entremezcladas** dentro de los mismos clusters.
* Cada cluster representa un fenotipo o linaje celular biológicamente coherente.

---

[⬅️ Anterior: Lección 08 (PCA y Selección de PCs)](Leccion_02_Reduccion_de_Dimensionalidad_PCA_y_Seleccion_de_PCs.md) | [🏠 Temario Principal](../README.md) | [Siguiente: Ejercicios Prácticos M3 ➡️](Ejercicios_M3.md)
