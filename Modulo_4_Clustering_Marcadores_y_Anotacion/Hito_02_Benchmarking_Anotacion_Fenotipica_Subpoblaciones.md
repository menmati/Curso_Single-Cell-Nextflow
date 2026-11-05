[⬅️ Anterior: Lección 12 (Anotación Manual vs. Automatizada)](Leccion_03_Estrategias_de_Anotacion_Manual_vs_Automatizada.md) | [🏠 Temario Principal](../README.md) | [Siguiente: Test de Autoevaluación M4 ➡️](Test_Autoevaluacion_M4.md)

---

# 🔥 Hito 02: Benchmarking de Anotación Fenotípica y Resolución de Subpoblaciones Inmunes Raras

---

## 🎯 Objetivo del Hito

En este segundo hito del curso, resolverás uno de los mayores desafíos en oncología e inmunología computacional: **diferenciar subpoblaciones inmunes altamente emparentadas en el microambiente tumoral**, contrastando sistemáticamente la anotación predictiva automatizada con la curación manual basada en paneles canónicos.

---

## 📋 Especificaciones Técnicas a Desarrollar

A partir de un dataset infiltrante de tumor integrado y reducido (Módulo 03):

### 1. Detección de Comunidades con Algoritmo de Leiden
* Construir el grafo SNN con `FindNeighbors(reduction = "harmony", dims = 1:30)`.
* Probar tres resoluciones distintas (`0.2`, `0.6` y `1.2`) mediante `FindClusters(algorithm = 4)`.
* Generar un árbol de clusters con `clustree()` y seleccionar de forma justificada la resolución óptima que resuelva la compartimentación de linfocitos T y mieloides sin sobreajustar.

### 2. Anotación Automatizada Independiente con `SingleR`
* Ejecutar `SingleR` utilizando tanto las etiquetas generales (`label.main`) como las etiquetas detalladas de subpoblación (`label.fine`) de `celldex::MonacoImmuneData()`.
* Generar un mapa de calor diagnósticos de puntuaciones de asignación (`plotScoreHeatmap()`) para auditar la ambigüedad en células con asignaciones de baja confianza.

### 3. Curación Manual Experta con Panel Canónico
* Identificar los 10 genes marcadores más significativos de cada cluster mediante `FindAllMarkers(only.pos = TRUE, test.use = "wilcox")`.
* Diseñar un `DotPlot` evaluando específicamente los siguientes pares discriminantes:
  1. **Linfocitos T CD4+ Reguladores (Treg)** frente a **T CD4+ Convencionales**: expresión co-localizada de `FOXP3`, `IL2RA` y ausencia de `IL7R`.
  2. **Linfocitos T CD8+ Exhaustos** frente a **T CD8+ Citotóxicos Efectores**: expresión de puntos de control inmunitario (`PDCD1`, `HAVCR2`, `LAG3`) frente a moléculas de lisis (`GZMB`, `PRF1`).
  3. **Macrófagos TAM Inmunosupresores (M2)** frente a **Monocitos Proinflamatorios**: expresión de receptores carroñeros (`CD163`, `MRC1`, `MSR1`) frente a alarminas (`S100A8`, `S100A9`).
* Asignar manualmente la etiqueta definitiva a cada cluster en el metadato `manual_annotation`.

### 4. Matriz de Confusión y Benchmarking Cuantitativo
* Construir una **matriz de confusión** cruzando las etiquetas de `SingleR` frente a `manual_annotation`.
* Calcular el grado de concordancia global (Índice de Rand Ajustado - ARI) y documentar los casos de discordancia biológica donde `SingleR` se equivocó al clasificar estados de activación celular como linajes erróneos.

### 5. Entregables Visuales
* **Figura 1 (`clustree_resolution.pdf`):** Árbol de estabilidad de resolución.
* **Figura 2 (`markers_dotplot.pdf`):** DotPlot ordenado de los marcadores discriminantes clave.
* **Figura 3 (`umap_annotated_comparison.pdf`):** UMAP lado a lado mostrando la anotación de `SingleR` vs. `Anotación Manual Experta`.

---

## 📤 Entrega y Verificación del Hito

Desarrolla el script completo y estructurado en R (`annotation_benchmarking.R`), computa la matriz de confusión y audita las figuras diagnósticas generadas (`clustree_resolution.pdf`, `markers_dotplot.pdf` y `umap_annotated_comparison.pdf`) para contrastar la concordancia entre la inferencia automatizada y la curación biológica experta.

---

[⬅️ Anterior: Lección 12 (Anotación Manual vs. Automatizada)](Leccion_03_Estrategias_de_Anotacion_Manual_vs_Automatizada.md) | [🏠 Temario Principal](../README.md) | [Siguiente: Test de Autoevaluación M4 ➡️](Test_Autoevaluacion_M4.md)
