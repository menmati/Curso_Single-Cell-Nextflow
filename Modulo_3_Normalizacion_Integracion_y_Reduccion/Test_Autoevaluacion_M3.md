[⬅️ Anterior: Ejercicios Prácticos M3](Ejercicios_M3.md) | [🏠 Temario Principal](../README.md) | [Siguiente: Lección 10 (Louvain y Leiden) ➡️](../Modulo_4_Clustering_Marcadores_y_Anotacion/Leccion_01_Algoritmos_de_Clustering_de_Grafos_Louvain_y_Leiden.md)

---

# Test de Autoevaluación — Módulo 03

> **Instrucciones:**  
> Selecciona la respuesta correcta para cada una de las 5 preguntas y reflexiona sobre la justificación técnica de 2-3 líneas para cada opción elegida.

---

### Pregunta 1

**¿Cuál es la principal ventaja matemática de los residuos de Pearson calculados por** `SCTransform v2` **frente a los valores log-transformados de** `LogNormalize`**?**

* [A] Los residuos de Pearson convierten todas las cuentas a números enteros positivos mayores que 1.000.
* [B] Estabilizan la varianza génica en un valor unitario constante (Var ≈ 1), desacoplando la expresión génica de la profundidad de secuenciación técnica y el tamaño celular.
* [C] Multiplican por diez el número total de lecturas secuenciadas por la celda de flujo.
* [D] Permiten que las células se alineen automáticamente contra el genoma de ratón y humano simultáneamente.

---

### Pregunta 2

**Al examinar un gráfico de** `ElbowPlot(obj, ndims = 50)`**, ¿qué criterio cuantitativo orienta la selección del número de componentes principales a retener para clustering?**

* [A] Identificar el punto de inflexión ("codo") donde la curva se aplana y la varianza adicional explicada por las siguientes componentes pasa a ser marginal y dominada por ruido técnico.
* [B] Seleccionar siempre los 50 componentes sin importar la forma de la curva.
* [C] Elegir exactamente el número de muestras incluidas en el experimento.
* [D] Conservar únicamente el PC1 y descartar todos los demás.

---

### Pregunta 3

**¿Cómo opera la arquitectura de Capas (**`Layers`**) introducida en Seurat v5 al trabajar con múltiples lotes o muestras?**

* [A] Obliga a guardar cada muestra en un archivo `.txt` separado en el disco duro durante el cálculo.
* [B] Desactiva la memoria RAM del ordenador y utiliza almacenamiento virtual en la nube.
* [C] Convierte el objeto Seurat en una tabla de SQL relacional.
* [D] Mantiene matrices de conteo desacopladas por muestra dentro del mismo objeto Seurat, permitiendo que `SCTransform` modele la varianza específica de cada lote antes de la integración unificada.

---

### Pregunta 4

**¿En qué espacio matemático opera el algoritmo Harmony para corregir los efectos de lote?**

* [A] Sobre las imágenes PNG de los gráficos UMAP bidimensionales.
* [B] Directamente sobre las secuencias de nucleótidos de los archivos FASTQ.
* [C] En el espacio de Componentes Principales (PCA), agrupando células mediante clustering difuso y aplicando factores de corrección específicos por lote para alinear sus centroides.
* [D] Exclusivamente en matrices binarias de mutaciones somáticas.

---

### Pregunta 5

**¿Qué diferencia metodológica fundamental existe entre la proyección UMAP y el espacio integrado de Harmony?**

* [A] UMAP es un algoritmo de reducción dimensional no lineal diseñado para visualización e interpretación humana en 2D, mientras que el espacio de Harmony es un espacio de baja dimensionalidad lineal (ej. 30 dimensiones) optimizado para el cálculo de distancias y clustering robusto.
* [B] Harmony solo funciona en datos de bulk RNA-Seq, mientras que UMAP solo funciona en microarrays.
* [C] UMAP calcula p-valores de Kaplan-Meier para cada célula individual.
* [D] No existe ninguna diferencia; son exactamente la misma función matemática con diferente nombre en R.

---

[⬅️ Anterior: Ejercicios Prácticos M3](Ejercicios_M3.md) | [🏠 Temario Principal](../README.md) | [Siguiente: Lección 10 (Louvain y Leiden) ➡️](../Modulo_4_Clustering_Marcadores_y_Anotacion/Leccion_01_Algoritmos_de_Clustering_de_Grafos_Louvain_y_Leiden.md)
