[⬅️ Anterior: Lección 11 (Detección de Marcadores y MAST)](Leccion_02_Identificacion_de_Marcadores_y_Expresion_Diferencial.md) | [🏠 Temario Principal](../README.md) | [Siguiente: Hito 02 (Benchmarking de Anotación) ➡️](Hito_02_Benchmarking_Anotacion_Fenotipica_Subpoblaciones.md)

---

# Lección 03: Estrategias de Anotación: Manual vs. Automatizada con IA

---

## 1. El Doble Enfoque en la Asignación de Identidad Celular

Traducir un conjunto de números de cluster (ej. "Cluster 0", "Cluster 1") en identidades biológicas reales (ej. "Linfocitos T CD8+ de Memoria", "Macrófagos M2") se puede realizar mediante dos metodologías complementarias:

```
[ Matriz de Expresión y Clusters ]
              │
      ┌───────┴───────┐
      ▼               ▼
[ Anotación Manual ]   [ Anotación Automatizada ]
(Paneles Canónicos)    (SingleR / CellTypist)
      │               │
      └───────┬───────┘
              ▼
    [ Curación Experta Final ]
```

---

## 2. Estrategia 1: Anotación Manual basada en Paneles Canónicos

Consiste en contrastar los genes más significativos de cada cluster contra la literatura científica validada experimentalmente por citometría de flujo e inmunohistoquímica:

| Linaje Celular Mayor | Subtipo / Estado | Marcadores Canónicos Principales |
| :--- | :--- | :--- |
| **Linfocitos T** | T Pan / Linaje | `CD3D`, `CD3E`, `CD3G`, `TRAC` |
| | T Helper (CD4+) | `CD4`, `IL7R` (CD127) |
| | T Citotóxicos (CD8+) | `CD8A`, `CD8B` |
| | T Reguladores (Treg) | `FOXP3`, `IL2RA` (CD25), `IKZF2` |
| | T Exhaustos | `PDCD1` (PD-1), `HAVCR2` (TIM-3), `LAG3`, `TIGIT` |
| **Células NK** | NK Citotóxicas | `NCAM1` (CD56), `NKG7`, `GNLY`, `KLRD1`, `PRF1` (sin CD3) |
| **Linfocitos B / Plasmáticas** | Células B maduras | `MS4A1` (CD20), `CD19`, `CD79A` |
| | Células Plasmáticas | `SDC1` (CD138), `MZB1`, `IGHG1` |
| **Línea Mieloide** | Monocitos Clásicos | `CD14`, `S100A8`, `S100A9` |
| | Monocitos No Clásicos | `FCGR3A` (CD16), `MS4A7` |
| | Macrófagos TAM (M2) | `CD68`, `CD163`, `MSR1` (CD204), `MRC1` (CD206) |
| | Células Dendríticas (cDC) | `HLA-DRA`, `ITGAX` (CD11c), `CLEC9A`, `CD1C` |
| **Estroma / Microambiente** | Células Endoteliales | `PECAM1` (CD31), `VWF`, `CDH5` |
| | Fibroblastos (CAFs) | `COL1A1`, `COL3A1`, `ACTA2` (alfa-SMA), `FAP` |
| | Epitelio Tumoral | `EPCAM`, `KRT8`, `KRT18`, `KRT19` |

---

## 3. Estrategia 2: Anotación Automatizada por Referencia con `SingleR`

`SingleR` (Aran et al., *Nature Immunology*, 2019) compara el perfil transcriptómico completo de cada célula individual contra atlas de referencia curados (como el *Human Primary Cell Atlas* o *Monaco Immune Data* del paquete `celldex`):

```R
library(SingleR)
library(celldex)

# Cargar base de datos curada de células inmunes humanas
ref_immune <- MonacoImmuneData()

# Convertir matriz Seurat a formato SingleCellExperiment
sce <- as.SingleCellExperiment(obj)

# Clasificación célula a célula
prediccion <- SingleR(
  test = sce,
  ref = ref_immune,
  labels = ref_immune$label.main,
  assay.type.test = "logcounts"
)

# Añadir predicciones al metadato de Seurat
obj$SingleR_label <- prediccion$labels
```

---

## 4. Estrategia 3: Machine Learning con `CellTypist`

Desarrollado por Domínguez Conde et al. (*Science*, 2022), **CellTypist** utiliza modelos de regresión logística entrenados con más de 20 millones de células para predecir tipos y subtipos celulares finos, asignando una probabilidad bayesiana de confianza y detectando estados de activación transicionales.

### La Regla de Oro:
Ninguna herramienta automatizada reemplaza el juicio bioinformático. El estándar de oro consiste en **correr la anotación automatizada como hipótesis inicial** y validarla rigurosamente comprobando que los clusters expresan de forma mutuamente excluyente los marcadores canónicos de linaje.

---

[⬅️ Anterior: Lección 11 (Detección de Marcadores y MAST)](Leccion_02_Identificacion_de_Marcadores_y_Expresion_Diferencial.md) | [🏠 Temario Principal](../README.md) | [Siguiente: Hito 02 (Benchmarking de Anotación) ➡️](Hito_02_Benchmarking_Anotacion_Fenotipica_Subpoblaciones.md)
