[⬅️ Anterior: Lección 07 (SCTransform v2)](Leccion_01_Normalizacion_SCTransform_v2.md) | [🏠 Temario Principal](../README.md) | [Siguiente: Lección 09 (Integración Harmony) ➡️](Leccion_03_Integracion_Multimuestra_con_Harmony_en_Seurat_v5.md)

---

# Lección 02: Reducción Dimensional y Selección de PCs Informáticos

---

## 1. La Maldición de la Dimensionalidad en Single-Cell

En un experimento típico de single-cell medimos la expresión de más de **25.000 genes** en miles de células. Si representáramos cada célula como un punto en un espacio de 25.000 dimensiones:

1. **Ruido técnico abrumador:** La gran mayoría de los genes son constitutivos (*housekeeping*), están silenciados o varían puramente por estocasticidad de muestreo.
2. **Distancias euclidianas colapsadas:** En hiperespacios de muy alta dimensión, la distancia geométrica entre cualquier par de puntos tiende a ser constante, inutilizando los algoritmos de clustering.

Para resolver esto, se seleccionan los **genes altamente variables (HVGs)** (por defecto, 3.000 genes seleccionados automáticamente por el modelo `SCTransform` basándose en el exceso de varianza residual) y se proyectan mediante **Análisis de Componentes Principales (PCA)**.

---

## 2. PCA sobre Residuos de Pearson

El PCA transforma el espacio de 3.000 genes variables en un conjunto de **componentes ortogonales e independientes (PCs)** ordenados de forma decreciente según la cantidad de varianza biológica que explican:

* **PC1, PC2, PC3...:** Capturan los grandes programas transcripcionales celulares (ej. linaje hematopoyético vs. epitelial, estado de activación inmune o ciclo celular).
* **PCs tardías (ej. PC40 a PC50):** Capturan varianza residual dominada por ruido técnico aleatorio.

```R
# Cálculo de PCA sobre el ensayo SCT
obj <- RunPCA(obj, assay = "SCT", npcs = 50, verbose = FALSE)
```

---

## 3. ¿Cuántas Componentes Principales Debemos Retener?

Elegir cuántos PCs utilizar para el clustering y UMAP es una decisión analítica fundamental:

* Retener muy pocos PCs (ej. solo 5) omite subpoblaciones celulares minoritarias o estados sutiles de activación.
* Retener demasiados PCs (ej. 50) introduce ruido técnico aleatorio en el grafo de vecinos.

Existen dos estrategias complementarias:

### A) El Gráfico del Codo (*ElbowPlot*)

Representa la desviación estándar (o porcentaje de varianza explicada) frente a cada PC:

```R
ElbowPlot(obj, ndims = 50)
```

Se busca el "punto de inflexión" o codo a partir del cual la curva se aplana y cada componente adicional aporta una fracción despreciable de varianza adicional (típicamente entre 20 y 30 PCs en tejidos complejos).

### B) Procedimiento Cuantitativo Formal de JackStraw

El método `JackStraw` (Chung & Storey, 2015) permuta aleatoriamente subconjuntos de datos para construir una distribución nula de cargas de genes por componente, calculando un **p-valor formal de significancia estadística** para cada PC individual:

```R
# Test de significancia empírica por PC
obj <- JackStraw(obj, num.replicate = 100, dims = 30)
obj <- ScoreJackStraw(obj, dims = 1:30)
JackStrawPlot(obj, dims = 1:30)
```

Se seleccionan todos aquellos componentes cuyo p-valor sea significativamente inferior a 0.05.

---

[⬅️ Anterior: Lección 07 (SCTransform v2)](Leccion_01_Normalizacion_SCTransform_v2.md) | [🏠 Temario Principal](../README.md) | [Siguiente: Lección 09 (Integración Harmony) ➡️](Leccion_03_Integracion_Multimuestra_con_Harmony_en_Seurat_v5.md)
