[⬅️ Anterior: Ejercicios Prácticos M5](Ejercicios_M5.md) | [🏠 Temario Principal](../README.md)

---

# Test de Autoevaluación — Módulo 05

> **Instrucciones:**  
> Selecciona la respuesta correcta para cada una de las 5 preguntas y reflexiona sobre la justificación técnica de 2-3 líneas para cada opción elegida.

---

### Pregunta 1

**¿Qué representa conceptualmente la métrica de pseudotiempo en un análisis de Monocle3?**

* [A] La hora exacta del día en que la muestra fue secuenciada en la máquina Illumina.
* [B] Una medida continua unidimensional que cuantifica el grado relativo de progresión de una célula individual a lo largo de un proceso biológico continuo de maduración, diferenciación o respuesta.
* [C] El tiempo transcurrido en minutos durante el ciclo de PCR.
* [D] La fecha de caducidad de los reactivos de transcripción inversa.

---

### Pregunta 2

**Al utilizar** `learn_graph()` **en Monocle3, ¿qué indica la aparición de una ramificación o bifurcación en el esqueleto de la trayectoria?**

* [A] Un error de desbordamiento de memoria RAM en el ordenador.
* [B] Un punto de decisión celular biológica donde una población progenitora diverge en dos linajes fenotípicos maduros diferentes con destinos transcripcionales distintos.
* [C] La contaminación de la muestra con ADN bacteriano exógeno.
* [D] Que las células se han fusionado formando un sincitio multinucleado.

---

### Pregunta 3

**¿Qué ventaja metodológica tiene el índice de autocorrelación espacial de Moran (I) en** `graph_test()` **sobre un test de correlación de Pearson simple contra el pseudotiempo?**

* [A] Permite detectar patrones complejos no lineales de activación génica evaluando la vecindad de las células en la topología completa del grafo principal, sin requerir una relación monótona simple.
* [B] Convierte los valores de expresión negativa en valores positivos absolutos.
* [C] Limita el cálculo a un máximo de 10 células por muestra.
* [D] Solo funciona en genomas de plantas.

---

### Pregunta 4

**¿Por qué CellChat v2 modela explícitamente complejos multiméricos de receptores y ligandos en lugar de pares proteicos individuales simples?**

* [A] Porque la mayoría de los factores solubles e inmunológicos (como interleucinas, receptores de TGF-beta o integrinas) requieren múltiples subunidades proteicas simultáneas para ser funcionalmente activas; evaluar monómeros simples generaría falsos positivos masivos.
* [B] Porque la base de datos de CellChat solo almacena secuencias de aminoácidos sintéticas.
* [C] Porque Nextflow DSL2 exige que todos los canales de entrada sean tuplas de tres elementos.
* [D] Para reducir el uso de disco duro en un 99%.

---

### Pregunta 5

**En un análisis de centralidad de red con CellChat en cáncer, una población de Fibroblastos Asociados a Tumor (CAFs) muestra una alta puntuación de salida (*outgoing signaling*) en vías como colágeno, fibronectina y VEGF, pero baja puntuación de entrada (*incoming signaling*). ¿Cuál es la interpretación biológica correcta?**

* [A] Los CAFs están muertos y no participan en el microambiente.
* [B] Los CAFs actúan dominantemente como una fuente emisora clave (*sender*) de señales paracrinas y remodelado de matriz extracelular hacia otras poblaciones receptoras como células endoteliales y epiteliales tumorales.
* [C] Los CAFs han mutado en linfocitos B de memoria.
* [D] El algoritmo CellChat ha fallado y se debe reiniciar el análisis.

---

[⬅️ Anterior: Ejercicios Prácticos M5](Ejercicios_M5.md) | [🏠 Temario Principal](../README.md)
