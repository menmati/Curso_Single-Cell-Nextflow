[⬅️ Anterior: Lección 01](Leccion_01_Tecnologia_10x_Genomics_y_Estructura_FASTQ.md) | [🏠 Temario Principal](../README.md) | [Siguiente: Lección 03 (Pipeline Nextflow DSL2) ➡️](Leccion_03_Pipeline_Nextflow_DSL2_para_scRNAseq.md)

---

# Lección 02: STARsolo y Algoritmos de Cuantificación

---

## 1. ¿Por qué STARsolo frente a Cell Ranger?

Históricamente, el software estándar para procesar datos de 10x Chromium ha sido **Cell Ranger**, una suite propietaria de 10x Genomics basada internamente en el alineador STAR. No obstante, Cell Ranger presenta limitaciones en entornos de investigación intensivos:
* Requiere gran cantidad de tiempo de CPU y memoria.
* Es una "caja negra" monolítica difícil de modularizar en flujos de trabajo personalizados.

**STARsolo** es una extensión nativa integrada directamente dentro del alineador **STAR** (a partir de la versión 2.7+). Ejecuta simultáneamente el alineamiento de lecturas, el análisis del código celular, la desduplicación de UMIs y la cuantificación a nivel de gen, ofreciendo:
* **Velocidad $\sim 10\times$ superior a Cell Ranger.**
* Paridad de resultados prácticamente idéntica ($> 99.8\%$ de correlación) cuando se configuran los parámetros de emulación correspondientes.
* Salida nativa estándar compatible con Seurat y Scanpy.

---

## 2. Parámetros Críticos de Configuración de STARsolo

Para asegurar paridad metodológica rigurosa con Cell Ranger v3/v4/v5+, el comando de STARsolo requiere los siguientes argumentos específicos:

```bash
STAR \
    --runThreadN 8 \
    --genomeDir /referencias/star_index \
    --readFilesIn sample_R2.fastq.gz sample_R1.fastq.gz \
    --readFilesCommand zcat \
    --soloType CB_UMI_Simple \
    --soloCBwhitelist /referencias/737K-august-2016.txt \
    --soloCBstart 1 --soloCBlen 16 \
    --soloUMIstart 17 --soloUMIlen 12 \
    --soloCBmatchWLtype 1MM_multi_Nbase_pseudocounts \
    --soloUMIfiltering MultiGeneUMI_CR \
    --soloUMIdedup 1MM_CR \
    --soloCellFilter EmptyDrops_CR \
    --outSAMtype BAM SortedByCoordinate \
    --outSAMattributes NH HI AS nM CB UB \
    --outFileNamePrefix ./results/
```

### Explicación de los Flags:
* **`--readFilesIn R2 R1`:** ⚠️ **Orden crucial:** STARsolo requiere primero el archivo con el cDNA (`R2`) y en segundo lugar el archivo con los códigos celulares y UMIs (`R1`).
* **`--soloType CB_UMI_Simple`:** Define el diseño de lectura donde el Barcode y el UMI se encuentran en una sola lectura contigua.
* **`--soloCBlen 16` y `--soloUMIlen 12`:** Para química 10x Chromium v3 (16 pb de Barcode y 12 pb de UMI). En química v2, la longitud del UMI es 10 pb.
* **`--soloCBmatchWLtype 1MM_multi_Nbase_pseudocounts`:** Aplica la estrategia de corrección de Cell Ranger con 1 mismatch y pseudoconteos ponderados.
* **`--soloUMIdedup 1MM_CR`:** Aplica el algoritmo de Cell Ranger para colapsar UMIs con hasta 1 error de secuenciación.
* **`--soloCellFilter EmptyDrops_CR`:** Ejecuta el algoritmo estadístico `EmptyDrops` para distinguir entre gotas con células reales y gotas vacías con ARN ambiental (*ambient RNA*).

---

## 3. Estructura de Salida: Las Matrices Dispersas (*Sparse Matrices*)

STARsolo genera dos carpetas dentro de `Solo.out/Gene/`:
1. `raw/`: Contiene todos los barcodes detectados (incluyendo gotas vacías con apenas 1 o 2 lecturas).
2. `filtered/`: Contiene únicamente los barcodes llamados como células viables por el filtro `EmptyDrops`.

Dentro de cada carpeta se encuentran tres archivos estándar que componen el formato **Market Exchange (MEX)**:

```
filtered/
├── barcodes.tsv.gz   # Lista de identificadores de células (filas de células)
├── features.tsv.gz   # Lista de genes (ID Ensembl, nombre del símbolo génico, tipo)
└── matrix.mtx.gz     # Matriz dispersa de coordenadas (gen, célula, conteo)
```

### ¿Por qué una Matriz Dispersa?
Una matriz densa típica de $30.000\text{ genes} \times 10.000\text{ células}$ ocuparía varios gigabytes en memoria. Dado que $> 90\%$ de los valores son ceros (un gen típico no se expresa simultáneamente en todas las células), el formato `.mtx` almacena únicamente las tripletas `(fila, columna, valor)` donde el conteo es estrictamente $> 0$, reduciendo drásticamente la huella de memoria.

---

[⬅️ Anterior: Lección 01](Leccion_01_Tecnologia_10x_Genomics_y_Estructura_FASTQ.md) | [🏠 Temario Principal](../README.md) | [Siguiente: Lección 03 (Pipeline Nextflow DSL2) ➡️](Leccion_03_Pipeline_Nextflow_DSL2_para_scRNAseq.md)
