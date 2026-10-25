[⬅️ Anterior: Lección 06 (DoubletFinder)](Leccion_03_Identificacion_y_Purga_de_Dobletes_Heterotipicos.md) | [🏠 Temario Principal](../README.md) | [Siguiente: Test de Autoevaluación M2 ➡️](Test_Autoevaluacion_M2.md)

---

# 🔥 Hito 01: Pipeline de QC Multivariante Adaptativo y Detección de Dobletes

---

## 🎯 Objetivo del Hito

En este primer hito del curso, implementarás un **script analítico en R (Seurat v5)** que automatice el control de calidad riguroso sobre un conjunto de datos de célula única, superando las limitaciones de los filtros estáticos y garantizando la eliminación de dobletes celulares antes de proceder a la integración y clustering.

---

## 📋 Especificaciones Técnicas a Implementar

Tu script `qc_and_doublets.R` debe recibir como entrada una matriz de conteo (o ruta MEX) y ejecutar los siguientes pasos de forma automatizada:

### 1. Ingesta y Cálculo de Métricas Basales

* Cargar los datos crudos con `Read10X()` y crear el objeto Seurat inicial.
* Calcular `percent.mt` (genes con patrón `^MT-` o `^mt-`) y `percent.rb` (genes ribosómicos con patrón `^RP[SL]` o `^Rp[sl]`).

### 2. Detección Adaptativa de Outliers mediante MAD

* Calcular los umbrales de exclusión estadísticos utilizando la Desviación Absoluta de la Mediana (± 3 × MAD):
  * Límite inferior para `nFeature_RNA` y `nCount_RNA` (en escala **\(\log_{10}\)**).
  * Límite superior para `percent.mt` (en escala lineal).
* Filtrar el objeto Seurat conservando las células que superen todos los criterios adaptativos.

### 3. Pipeline Intermedio de Reducción para DoubletFinder

`DoubletFinder` requiere que el objeto cuente previamente con normalización, identificación de genes variables y componentes principales calculados:

* Normalizar con `NormalizeData()`.
* Identificar 2.000 genes variables con `FindVariableFeatures()`.
* Escalar con `ScaleData()` y calcular PCA (`RunPCA(npcs = 30)`).
* Calcular UMAP preliminar (`RunUMAP(dims = 1:20)`).

### 4. Optimización de pK y Detección de Dobletes con `DoubletFinder`

* Ejecutar un barrido de parámetros con `paramSweep()` y `summarizeSweep()`.
* Identificar el valor óptimo de pK que maximice el coeficiente de bimodalidad media (`find.pK()`).
* Estimar el número de dobletes esperados en función del número de células recuperadas (asumiendo ~ 7.5% para 10.000 células).
* Ejecutar `doubletFinder()` clasificando cada código celular como `Singlet` o `Doublet`.

### 5. Exportación de Entregables Visuales y Datos

* **Figura 1 (`qc_violin_pre_post.pdf`):** Violin plots comparativos de `nFeature_RNA`, `nCount_RNA` y `percent.mt` antes y después del filtrado.
* **Figura 2 (`doublets_umap.pdf`):** Gráfico UMAP coloreando las células según su clasificación (`Singlet` en gris/azul, `Doublet` en rojo brillante).
* **Objeto filtrado (`seurat_qc_filtered.rds`):** Guardar el objeto Seurat limpio conteniendo únicamente los `Singlets` viables para el Módulo 03.

---

## 📤 Entrega y Verificación del Hito

Desarrolla el script completo y comentado `qc_and_doublets.R` y evalúa cómo interpretas los gráficos diagnósticos generados (`qc_violin_pre_post.pdf` y `doublets_umap.pdf`) para verificar la correcta purga de dobletes y células dañadas en tu entorno de trabajo.

---

[⬅️ Anterior: Lección 06 (DoubletFinder)](Leccion_03_Identificacion_y_Purga_de_Dobletes_Heterotipicos.md) | [🏠 Temario Principal](../README.md) | [Siguiente: Test de Autoevaluación M2 ➡️](Test_Autoevaluacion_M2.md)
