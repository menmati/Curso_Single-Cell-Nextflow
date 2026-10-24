[⬅️ Anterior: Lección 03](Leccion_03_Pipeline_Nextflow_DSL2_para_scRNAseq.md) | [🏠 Temario Principal](../README.md) | [Siguiente: Test de Autoevaluación M1 ➡️](Test_Autoevaluacion_M1.md)

---

# Ejercicios Prácticos — Módulo 01: Upstream Nextflow DSL2

> **Instrucciones:**  
> Resuelve los siguientes retos técnicos redactando tus bloques de código y respuestas explicativas en tu entorno de trabajo.

---

### 📝 Ejercicio 1.1: Adaptación Dinámica de la Química 10x Genomics en Nextflow
En un consorcio multicéntrico se han recibido muestras secuenciadas tanto con química 10x **Chromium v2** (UMI de 10 pb; whitelist `737K-august-2016.txt`) como con **Chromium v3** (UMI de 12 pb; whitelist `3M-february-2018.txt.gz`).
* Escribe una expresión en Groovy dentro del proceso Nextflow o en el workflow que asigne automáticamente la ruta correcta del archivo de whitelist en función del valor `meta.chemistry` de cada muestra.
* Explica qué ocurriría con la cuantificación de UMIs si procesaras una muestra v3 configurando una longitud de UMI de 10 pb en STARsolo.

---

### 📝 Ejercicio 1.2: Optimización de Almacenamiento en BAMs de Single-Cell
En secuenciación de célula única, generar archivos BAM completos ordenados por coordenadas para millones de lecturas puede saturar cientos de gigabytes de disco innecesariamente si el objetivo principal es únicamente obtener la matriz de expresión génica (`matrix.mtx`).
* Modifica el comando STARsolo de la Lección 02 para **desactivar completamente la generación del archivo BAM** sin afectar la generación de las matrices de recuento (`matrix.mtx.gz`, `barcodes.tsv.gz`, `features.tsv.gz`).
* Identifica qué parámetro específico de STAR controla esta salida y justifica el impacto en tiempo de CPU y espacio en disco.

---

### 📝 Ejercicio 1.3: Construcción de un Canal para Carga Automatizada en R (Seurat)
Diseña un operador de canal en Nextflow DSL2 que tome la salida `STARSOLO.out.counts_filtered` y genere un archivo de metadatos o tabla TSV lista para ser leída por un script de R, emparejando el ID de cada muestra (`meta.id`) con la ruta absoluta donde residen los tres archivos MEX (`matrix.mtx.gz`, `barcodes.tsv.gz`, `features.tsv.gz`).

---

[⬅️ Anterior: Lección 03](Leccion_03_Pipeline_Nextflow_DSL2_para_scRNAseq.md) | [🏠 Temario Principal](../README.md) | [Siguiente: Test de Autoevaluación M1 ➡️](Test_Autoevaluacion_M1.md)
