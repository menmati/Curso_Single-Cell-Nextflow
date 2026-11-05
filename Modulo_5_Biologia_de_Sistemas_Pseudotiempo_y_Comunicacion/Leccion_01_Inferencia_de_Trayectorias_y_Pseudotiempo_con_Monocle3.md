[⬅️ Anterior: Test Módulo 04](../Modulo_4_Clustering_Marcadores_y_Anotacion/Test_Autoevaluacion_M4.md) | [🏠 Temario Principal](../README.md) | [Siguiente: Lección 14 (Dinámicas Génicas en Pseudotiempo) ➡️](Leccion_02_Modelado_de_Dinamicas_Genicas_a_lo_Largo_del_Pseudotiempo.md)

---

# Lección 01: Inferencia de Trayectorias y Pseudotiempo con Monocle3

---

## 1. El Concepto Biológico de Pseudotiempo

En un experimento de secuenciación de célula única extraemos simultáneamente miles de células en un momento temporal congelado (t0). Sin embargo, en un tejido biológico dinámico (como la médula ósea en hematopoyesis, el desarrollo embrionario o la transformación tumoral), diferentes células individuales se encuentran en **diferentes etapas de maduración o diferenciación**.

El **pseudotiempo (*pseudotime*)** es una métrica continua cuantitativa abstracta que mide el progreso de cada célula individual a lo largo de un proceso biológico continuo, ordenando las células a lo largo de una trayectoria unidimensional que refleja su evolución temporal real.

---

## 2. El Algoritmo de Grafo Principal en `Monocle3`

Desarrollado por Cao et al. (*Nature*, 2019), `Monocle3` supera las limitaciones de las herramientas lineales tempranas mediante el aprendizaje de **grafos principales reversibles (*SimplePPT - Simple Principal Tree*)**:

1. **Partición de Trayectorias en el espacio UMAP:** Divide las células en "super-grupos" o particiones disjuntas utilizando clustering de grafos para evitar forzar trayectorias falsas entre tipos celulares que no comparten un origen común.
2. **Ajuste del Grafo Principal:** Traza un esqueleto o árbol curvilíneo continuo que atraviesa el centro de mayor densidad celular de la población.
3. **Puntos de Ramificación (*Branching Points*):** Identifica bifurcaciones donde una célula madre progenitora toma una decisión de destino celular divergente (por ejemplo, una célula progenitora que se diferencia hacia linaje eritroide o hacia linaje mieloide).

```R
library(monocle3)
library(SeuratWrappers)

# Conversión directa desde Seurat v5 a objeto Monocle3 (cell_data_set - cds)
cds <- as.cell_data_set(seurat_obj)

# Transferir la reducción UMAP calculada
cds <- cluster_cells(cds, reduction_method = "UMAP")
cds <- learn_graph(cds, use_partition = TRUE)
```

---

## 3. Asignación del Estado Raíz (*Root*) y Medición Temporal

Para definir la dirección de la flecha del tiempo (qué punto del árbol es el "inicio" y cuáles son los "destinos finales"), se debe indicar el **nodo raíz (*root node*)**:

* Se selecciona mediante conocimiento biológico previo (las células con mayor expresión de marcadores de pluripotencia o células madre, como `CD34` o `PROM1/CD133`).

```R
# Función para identificar las células raíz con mayor expresión del marcador stem
obtener_celulas_raiz <- function(cds, marcador = "CD34") {
  celulas_stem <- which(exprs(cds)[marcador, ] > 0)
  return(names(celulas_stem))
}

# Ordenar las células a lo largo del árbol principal
cds <- order_cells(cds, root_cells = obtener_celulas_raiz(cds, "CD34"))

# Visualización del pseudotiempo continuo sobre UMAP
plot_cells(
  cds,
  color_cells_by = "pseudotime",
  label_cell_groups = FALSE,
  label_leaves = TRUE,
  label_branch_points = TRUE
)
```

El resultado asigna a cada célula un valor numérico de pseudotiempo desde 0.0 (célula progenitora indiferenciada) hasta valores crecientes en las ramas terminales maduras.

---

[⬅️ Anterior: Test Módulo 04](../Modulo_4_Clustering_Marcadores_y_Anotacion/Test_Autoevaluacion_M4.md) | [🏠 Temario Principal](../README.md) | [Siguiente: Lección 14 (Dinámicas Génicas en Pseudotiempo) ➡️](Leccion_02_Modelado_de_Dinamicas_Genicas_a_lo_Largo_del_Pseudotiempo.md)
