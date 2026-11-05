[⬅️ Anterior: Hito 02 (Benchmarking de Anotación)](Hito_02_Benchmarking_Anotacion_Fenotipica_Subpoblaciones.md) | [🏠 Temario Principal](../README.md) | [Siguiente: Lección 13 (Monocle3 y Pseudotiempo) ➡️](../Modulo_5_Biologia_de_Sistemas_Pseudotiempo_y_Comunicacion/Leccion_01_Inferencia_de_Trayectorias_y_Pseudotiempo_con_Monocle3.md)

---

# Test de Autoevaluación — Módulo 04

> **Instrucciones:**  
> Selecciona la respuesta correcta para cada una de las 5 preguntas y reflexiona sobre la justificación técnica de 2-3 líneas para cada opción elegida.

---

### Pregunta 1

**¿Cuál es la principal ventaja algorítmica del método de Leiden sobre el método de Louvain para la detección de comunidades en grafos celulares?**

* [A] Leiden no requiere construir un grafo de vecinos previo.
* [B] Leiden incluye una fase de refinamiento de partición que garantiza formalmente que todas las comunidades resultantes sean conexas, eliminando clusters desconectados y particiones inestables de Louvain.
* [C] Leiden solo funciona en células con menos de 500 genes expresados.
* [D] Leiden sustituye el cálculo de PCA por redes neuronales convolucionales.

---

### Pregunta 2

**Al utilizar la herramienta** `clustree`**, ¿qué patrón gráfico indica que estamos aplicando una resolución excesivamente alta (*over-clustering*)?**

* [A] Las ramas del árbol descienden de forma perfectamente vertical y lineal.
* [B] La aparición masiva de ramas entrecruzadas (*crossovers*) donde las células oscilan caóticamente entre múltiples clusters adyacentes de un nivel de resolución al siguiente.
* [C] El árbol se transforma automáticamente en un mapa de calor en tres dimensiones.
* [D] El número total de clusters se reduce a uno.

---

### Pregunta 3

**¿Cómo aborda el modelo estadístico MAST el fenómeno del dropout técnico al contrastar la expresión diferencial de genes marcadores?**

* [A] Asignando un valor constante de 10.000 a todas las células que tengan un cero.
* [B] Realizando un test de Chi-cuadrado sobre los códigos celulares.
* [C] Eliminando todos los genes que contengan ceros en más del 1% de las células.
* [D] Mediante un modelo Hurdle de dos partes que parametriza de forma separada la probabilidad de detección binaria (cero vs. no cero) y la media continua del nivel de expresión en las células positivas.

---

### Pregunta 4

**¿Qué combinación canónica de marcadores moleculares permite diferenciar de forma inequívoca a los Linfocitos T Reguladores (Treg) de otras células T CD4+ auxiliares?**

* [A] `CD14` positivo y `CD3D` negativo.
* [B] Co-expresión de `CD3E`, `CD4`, `FOXP3`, `IL2RA` (CD25) junto con niveles muy bajos o nulos de `IL7R` (CD127).
* [C] Presencia exclusiva de hemoglobina (`HBA1`, `HBB`).
* [D] Expresión masiva de citoqueratinas epiteliales (`KRT8`, `KRT18`).

---

### Pregunta 5

**En una clasificación automatizada con** `SingleR`**, ¿qué precaución metodológica debe tomar siempre un bioinformático antes de validar el resultado final?**

* [A] Auditar la idoneidad de la base de datos de referencia (ej. no usar un atlas de sangre periférica normal para predecir células de un glioma cerebral atípico) y validar que los clusters expresan sus marcadores canónicos clásicos.
* [B] Asumir que la predicción de la máquina es 100% infalible y no contrastar los genes.
* [C] Cambiar el sistema operativo a Windows 98.
* [D] Eliminar todas las células que tengan un p-valor menor a 0.05.

---

[⬅️ Anterior: Hito 02 (Benchmarking de Anotación)](Hito_02_Benchmarking_Anotacion_Fenotipica_Subpoblaciones.md) | [🏠 Temario Principal](../README.md) | [Siguiente: Lección 13 (Monocle3 y Pseudotiempo) ➡️](../Modulo_5_Biologia_de_Sistemas_Pseudotiempo_y_Comunicacion/Leccion_01_Inferencia_de_Trayectorias_y_Pseudotiempo_con_Monocle3.md)
