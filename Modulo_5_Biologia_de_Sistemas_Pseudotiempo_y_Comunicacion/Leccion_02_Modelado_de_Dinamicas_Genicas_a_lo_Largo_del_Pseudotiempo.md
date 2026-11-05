[⬅️ Anterior: Lección 13 (Monocle3 y Pseudotiempo)](Leccion_01_Inferencia_de_Trayectorias_y_Pseudotiempo_con_Monocle3.md) | [🏠 Temario Principal](../README.md) | [Siguiente: Lección 15 (Comunicación Celular con CellChat) ➡️](Leccion_03_Analisis_de_Comunicacion_Celular_con_CellChat.md)

---

# Lección 02: Dinámica de Expresión Génica a lo Largo del Pseudotiempo

---

## 1. Detección de Genes Dependientes del Pseudotiempo

Una vez ordenadas las células en el pseudotiempo, la pregunta biológica clave es:  
*¿Qué factores de transcripción y programas genéticos se encienden, se apagan o experimentan picos transitorios a medida que la célula avanza en su diferenciación?*

En `Monocle3`, esta evaluación se realiza mediante el contraste espacial `graph_test()`, utilizando el estadístico de **Autocorrelación Espacial de Moran (I)**:

* Evalúa si las células con niveles similares de expresión de un gen se encuentran contiguas en el grafo de la trayectoria celular o si están distribuidas al azar.

```R
# Identificar genes cuya expresión varía significativamente a lo largo del pseudotiempo
genes_dinamicos <- graph_test(cds, neighbor_graph = "principal_graph", cores = 4)

# Filtrar por significancia estadística estricta y alta autocorrelación
genes_top <- subset(genes_dinamicos, q_value < 0.01 & morans_I > 0.25)
genes_top <- genes_top[order(genes_top$morans_I, decreasing = TRUE), ]
```

---

## 2. Agrupamiento en Módulos Génicos Co-regulados

Analizar miles de genes individuales uno a uno resulta inmanejable. Mediante la función `find_gene_modules()`, `Monocle3` aplica clustering de comunidades en el grafo de correlación génica para agrupar los genes que siguen **perfiles de activación sincronizados**:

```R
# Agrupar genes dinámicos en módulos funcionales
modulos_genes <- find_gene_modules(cds[rownames(genes_top), ], resolution = 1e-2)

# Visualizar el mapa de calor de los módulos a lo largo de las trayectorias
plot_cells(
  cds,
  genes = modulos_genes,
  show_trajectory_graph = FALSE,
  label_cell_groups = FALSE
)
```

Cada módulo representa un programa biológico coordinado:

* **Módulo Temprano:** Factores de transcripción de mantenimiento del estado proliferativo indiferenciado (se apagan rápidamente).
* **Módulo Intermedio:** Reguladores de transición metabólica y cese de proliferación.
* **Módulo Terminal:** Proteínas efectoras de la función especializada de la célula madura (ej. receptores de citocinas o inmunoglobulinas).

---

## 3. Cinética de Curvas Spline

Para validar genes clave (ej. factores pioneros o receptores diana), se ajustan curvas no lineales (*splines cúbicos*) que muestran la tasa de expresión media y sus intervalos de confianza frente al pseudotiempo continuo:

```R
# Gráfico cinético temporal de genes clave individuales
genes_de_interes <- c("CD34", "GATA1", "SPI1", "MPO")
plot_genes_in_pseudotime(
  cds[genes_de_interes, ],
  color_cells_by = "pseudotime",
  min_expr = 0.5
)
```

Esto permite identificar el momento temporal exacto (*timing molecular*) en el que se activan los puntos de control del destino celular.

---

[⬅️ Anterior: Lección 13 (Monocle3 y Pseudotiempo)](Leccion_01_Inferencia_de_Trayectorias_y_Pseudotiempo_con_Monocle3.md) | [🏠 Temario Principal](../README.md) | [Siguiente: Lección 15 (Comunicación Celular con CellChat) ➡️](Leccion_03_Analisis_de_Comunicacion_Celular_con_CellChat.md)
