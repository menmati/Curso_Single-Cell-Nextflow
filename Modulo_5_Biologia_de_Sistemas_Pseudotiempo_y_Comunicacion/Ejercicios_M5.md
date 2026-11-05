[⬅️ Anterior: Lección 15 (Comunicación Celular con CellChat)](Leccion_03_Analisis_de_Comunicacion_Celular_con_CellChat.md) | [🏠 Temario Principal](../README.md) | [Siguiente: Test de Autoevaluación M5 ➡️](Test_Autoevaluacion_M5.md)

---

# Ejercicios Prácticos — Módulo 05: Biología de Sistemas

> **Instrucciones:**  
> Resuelve los siguientes retos técnicos aplicando los conocimientos adquiridos en las lecciones previas sobre inferencia de trayectorias, análisis espacial de dinámicas génicas y modelado de comunicación celular.

---

### 📝 Ejercicio 5.1: Inferencia de Pseudotiempo en una Línea de Diferenciación

* A partir de un objeto Seurat con células precursoras hematopoyéticas y células maduras diferenciadas, escribe el pipeline completo en R para transferir el objeto a `Monocle3` (`cds`), calcular el grafo principal con `learn_graph()` y ordenar las células asignando automáticamente la raíz en la población con mayor expresión del gen progenitor `CD34`.
* Genera el código para graficar las células coloreadas por su valor de pseudotiempo sobre la reducción UMAP.

---

### 📝 Ejercicio 5.2: Identificación de Factores de Transcripción con Dinámica Temporal Significativa

* Utilizando la función `graph_test()` de `Monocle3`, filtra aquellos genes que cumplan simultáneamente:
  1. q-valor < 0.001.
  2. Índice de Moran I > 0.35.
* Escribe la función para ajustar y trazar la curva cinética spline de los dos factores de transcripción con mayor valor de autocorrelación espacial a lo largo del pseudotiempo.

---

### 📝 Ejercicio 5.3: Cuantificación del Eje Inmunosupresor PD-L1 / PD-1 con CellChat

* Escribe el bloque de código en R para aislar e inspeccionar específicamente la vía de señalización del punto de control inmunitario `PD-L1` (*CD274 - PDCD1*) dentro de un objeto `CellChat` precalculado.
* Genera un gráfico de cuerdas (*netVisual_chord_gene*) o gráfico de burbujas (*netVisual_bubble*) mostrando qué población actúa dominantemente como emisora del ligando PD-L1 y qué población linfocitaria actúa como receptora con PD-1.

---

[⬅️ Anterior: Lección 15 (Comunicación Celular con CellChat)](Leccion_03_Analisis_de_Comunicacion_Celular_con_CellChat.md) | [🏠 Temario Principal](../README.md) | [Siguiente: Test de Autoevaluación M5 ➡️](Test_Autoevaluacion_M5.md)
