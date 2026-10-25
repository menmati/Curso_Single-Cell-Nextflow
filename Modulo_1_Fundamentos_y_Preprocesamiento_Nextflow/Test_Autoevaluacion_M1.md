[⬅️ Anterior: Ejercicios Prácticos M1](Ejercicios_M1.md) | [🏠 Temario Principal](../README.md) | [Siguiente: Lección 04 (QC Celular y MAD) ➡️](../Modulo_2_Control_de_Calidad_y_Dobletes/Leccion_01_Metricas_QC_y_Filtrado_Adaptativo.md)

---

# Test de Autoevaluación — Módulo 01

> **Instrucciones:**  
> Selecciona la respuesta correcta para cada una de las 5 preguntas y anota tus elecciones junto con una breve justificación técnica de 2-3 líneas para cada opción elegida.

---

### Pregunta 1
**¿Cuál es la función biológica y computacional primordial del Unique Molecular Identifier (UMI) en la secuenciación de célula única basada en microgotas?**
* [A] Identificar a qué tipo celular pertenece cada lectura secuenciada en el tejido.
* [B] Distinguir entre moléculas originales de ARNm celular y duplicados generados por los ciclos de amplificación por PCR.
* [C] Alinear la lectura contra el transcriptoma de referencia con mayor sensibilidad de empalme.
* [D] Facilitar la unión del oligonucleótido a la cola poli(A) del ARN ribosomal.

---

### Pregunta 2
**Al invocar STARsolo con `--readFilesIn`, ¿cuál es el orden estricto en que deben suministrarse los archivos de lectura FASTQ emparejados?**
* [A] Primero `R1` (Cell Barcode + UMI) y segundo `R2` (cDNA insert).
* [B] Es indistinto, ya que STARsolo detecta automáticamente el contenido mediante inspección de calidad Phred.
* [C] Primero `R2` (cDNA insert biológico) y segundo `R1` (Cell Barcode + UMI).
* [D] Ambos archivos deben fusionarse previamente en un único archivo FASTQ intercalado (*interleaved*).

---

### Pregunta 3
**¿Qué estrategia implementa el parámetro `--soloCBmatchWLtype 1MM_multi_Nbase_pseudocounts` en STARsolo?**
* [A] Descarta inmediatamente cualquier lectura cuyo código celular difiera en al menos un nucleótido de la lista blanca.
* [B] Permite hasta 1 mismatch frente a la whitelist, resolviendo casos ambiguos mediante distribución de pseudoconteos bayesianos para asignar la lectura al barcode más probable.
* [C] Exige coincidencia exacta (0 mismatches) en las primeras 12 bases y tolera inserciones en las 4 restantes.
* [D] Modifica químicamente la secuencia FASTQ en disco antes de proceder con el alineamiento.

---

### Pregunta 4
**¿Por qué las matrices de recuento generadas por STARsolo se exportan en formato de matriz dispersa (Market Exchange Format - `.mtx`) en lugar de un archivo tabular denso (CSV / TSV)?**
* [A] Porque el formato `.mtx` comprime las secuencias genómicas en formato binario ilegible.
* [B] Porque más del 90% de los elementos de una matriz de célula única son ceros, por lo que almacenar únicamente las tripletas `(gen, célula, conteo)` optimiza drásticamente el uso de memoria RAM y disco.
* [C] Porque los sistemas operativos Linux impiden la creación de archivos CSV con más de 1.000 columnas.
* [D] Porque Seurat v5 y Scanpy son incompatibles con el cálculo matricial en coma flotante.

---

### Pregunta 5
**En una corrida de secuenciación 10x Genomics Chromium v3, un investigador observa que la carpeta `Solo.out/Gene/raw/` contiene 700.000 códigos celulares, mientras que `Solo.out/Gene/filtered/` contiene únicamente 8.500. ¿A qué se debe esta enorme discrepancia?**
* [A] A un fallo catastrófico en el proceso de secuenciación que destruyó el 98% de las células.
* [B] A que `raw/` incluye todas las microgotas capturadas (la inmensa mayoría gotas vacías con ARN ambiental o ruido de fondo), mientras que `filtered/` contiene solo los códigos celulares llamados como células vivas por el algoritmo estadístico `EmptyDrops`.
* [C] A que STARsolo descartó por error todas las células que expresaban genes mitocondriales.
* [D] A que la *whitelist* de 10x v3 tiene un límite estricto fijado en 8.500 células por muestra.

---

[⬅️ Anterior: Ejercicios Prácticos M1](Ejercicios_M1.md) | [🏠 Temario Principal](../README.md) | [Siguiente: Lección 04 (QC Celular y MAD) ➡️](../Modulo_2_Control_de_Calidad_y_Dobletes/Leccion_01_Metricas_QC_y_Filtrado_Adaptativo.md)
