[⬅️ Anterior: Test Módulo 03](../Modulo_3_Normalizacion_Integracion_y_Reduccion/Test_Autoevaluacion_M3.md) | [🏠 Temario Principal](../README.md) | [Siguiente: Lección 11 (Detección de Marcadores y MAST) ➡️](Leccion_02_Identificacion_de_Marcadores_y_Expresion_Diferencial.md)

---

# Lección 01: Algoritmos de Grafos (Louvain y Leiden) y Resolución Multiescala

---

## 1. De Coordenadas Continuas a Grafos de Vecindad (SNN)

En scRNA-seq, agrupar células mediante algoritmos de distancia espacial directa (como K-means) es ineficaz debido a la geometría no esférica y altamente curvada de las variedades celulares biológicas. En su lugar, se utilizan **grafos de comunidades**:

1. **Grafo de K-Vecinos más Cercanos (k-NN):** En el espacio reducido de componentes principales integradas (ej. 30 dimensiones de Harmony), se identifican para cada célula sus k células más cercanas (típicamente k=20).
2. **Grafo de Vecinos Compartidos (SNN - Shared Nearest Neighbor):** Dos células no se conectan solo por estar cerca, sino si comparten vecinos en común. La fuerza de la arista (*peso de Jaccard*) refleja el grado de solapamiento de sus vecindarios locales:

$$
\text{Jaccard}(u, v) = \frac{|N(u) \cap N(v)|}{|N(u) \cup N(v)|}
$$

```R
# Construcción del grafo SNN en Seurat v5
obj <- FindNeighbors(obj, reduction = "harmony", dims = 1:30, k.param = 20)
```

---

## 2. Detección de Comunidades: Louvain vs. Leiden

Una vez construido el grafo, el objetivo es particionar las células en comunidades densamente conectadas mediante la **optimización de la función de modularidad**.

### El Algoritmo de Louvain (Blondel et al., 2008):

Fue el estándar durante años. Opera fusionando nodos y optimizando la modularidad localmente. Sin embargo, sufre de un **defecto matemático crítico demostrado por Traag et al. (2019)**:

* Puede atrapar comunidades mal conectadas o **clusters desconectados** (grupos donde algunas células no tienen ningún camino en el grafo para comunicarse con otras células del mismo cluster).

### El Algoritmo de Leiden (Traag et al., 2019):

Es el algoritmo moderno recomendado:

1. Divide las comunidades en sub-particiones locales.
2. Aplica una fase de **refinamiento** que garantiza que todos los clusters sean formalmente conexos y estables.
3. Converge más rápido y reproduce resultados deterministas.

```R
# En Seurat v5 se activa Leiden especificando algorithm = 4
# Requiere tener instalado el paquete python leidenalg
obj <- FindClusters(obj, resolution = 0.5, algorithm = 4)
```

---

## 3. El Parámetro de Resolución y el Análisis con `clustree`

El parámetro `resolution` modula la granularidad de las comunidades detectadas:

* **Resoluciones bajas (0.1 - 0.3):** Agrupan células en grandes linajes macroscópicos (Linfocitos, Mieloides, Células Estromales, Epiteliales).
* **Resoluciones altas (0.8 - 1.5):** Separan subpoblaciones sutiles, estados de activación o fases de diferenciación (ej. Linfocitos T CD8+ *naive*, efectores, de memoria y exhaustos).

Para evitar elegir una resolución a ciegas, se realiza un barrido sistemático y se visualiza la estabilidad de los clusters mediante un **árbol de clustering (**`clustree`**)**:

```R
library(clustree)

# Barrido de resoluciones de 0.1 a 1.0
resoluciones <- seq(0.1, 1.0, by = 0.1)
obj <- FindClusters(obj, resolution = resoluciones, algorithm = 4)

# Visualización del árbol de linajes
clustree(obj, prefix = "SCT_snn_res.")
```

* Un cluster es **estable** si las ramas del árbol descienden de forma vertical y rectilínea sin fragmentarse caóticamente.
* Si a una resolución mayor las células se intercambian aleatoriamente entre ramas adyacentes (*crossover* masivo), indica que esa resolución está provocando **sobreajuste (over-clustering)** artificial sobre ruido técnico.

---

[⬅️ Anterior: Test Módulo 03](../Modulo_3_Normalizacion_Integracion_y_Reduccion/Test_Autoevaluacion_M3.md) | [🏠 Temario Principal](../README.md) | [Siguiente: Lección 11 (Detección de Marcadores y MAST) ➡️](Leccion_02_Identificacion_de_Marcadores_y_Expresion_Diferencial.md)
