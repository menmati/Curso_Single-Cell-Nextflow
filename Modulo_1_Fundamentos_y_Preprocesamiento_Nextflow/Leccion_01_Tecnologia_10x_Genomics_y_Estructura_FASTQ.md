[🏠 Temario Principal](../README.md) | [Siguiente: Lección 02 (STARsolo) ➡️](Leccion_02_STARsolo_y_Cuantificacion_Alineamiento.md)

---

# Lección 01: Tecnología 10x Genomics y Estructura de Lecturas FASTQ

---

## 1. El Principio de Microgotas (GEMs) en 10x Chromium

La tecnología de célula única basada en microfluídica encapsula células individuales dentro de gotas de emulsión acuosa en aceite, denominadas **GEMs** (*Gel Beads-in-Emulsion*). Cada gota contiene:
1. Una única célula lisada in situ.
2. Una microesfera de gel con millones de oligonucleótidos cebadores fijados covalentemente.
3. Reactivos enzimáticos para la transcripción inversa (RT).

```
   [ Célula Individual ]
            +             ==>  [ Gota GEM ] ==> [ Transcripción Inversa con Barcode ]
   [ Microesfera de Gel]
```

Cada oligonucleótido de la microesfera posee tres regiones funcionales críticas:
* **Secuencia adaptadora Illumina:** Necesaria para la hibridación en la celda de flujo (*flow cell*).
* **Cell Barcode (CB):** Una secuencia de 16 nucleótidos idéntica en todos los cebadores de una misma microesfera, pero distinta entre diferentes microesferas. Permite identificar a qué célula original perteneció cada transcrito.
* **Unique Molecular Identifier (UMI):** Una secuencia aleatoria corta (10 pb en química v2; 12 pb en química v3) única para cada molécula de cebador individual. Permite contar moléculas de ARNm reales y eliminar el sesgo de amplificación por PCR.
* **Cola Oligo-dT:** Hibrida con la cola poli(A) del extremo 3' de los transcritos de ARNm eucariotas.

---

## 2. Anatomía de las Lecturas FASTQ (Paired-End Asimétrico)

A diferencia del RNA-Seq convencional (*bulk*) donde ambas lecturas (R1 y R2) contienen fragmentos de cDNA, en 10x Genomics el diseño es **asimétrico**:

```
Read 1 (26 o 28 pb):
┌──────────────────────────┬──────────────────────┐
│  Cell Barcode (16 pb)    │     UMI (10 o 12 pb) │
└──────────────────────────┴──────────────────────┘

Read 2 (90 a 150 pb):
┌─────────────────────────────────────────────────┐
│     Inserto de cDNA biológico (genoma/gen)      │
└─────────────────────────────────────────────────┘
```

* **Read 1 (R1):** No se alinea contra el genoma. Se utiliza exclusivamente para extraer la dirección celular (CB) y la identidad molecular (UMI).
* **Read 2 (R2):** Contiene la secuencia del transcrito biológico y se alinea contra la referencia genómica para identificar el gen de origen.

---

## 3. Corrección de Errores de Barcode y la *Whitelist*

Durante la secuenciación o la síntesis de oligos, pueden ocurrir errores de sustitución en los 16 pb del código celular. Para evitar fragmentar una célula en múltiples perfiles falsos:
1. Se utiliza una **Whitelist** (lista blanca oficial de secuencias conocidas fabricadas por 10x Genomics).
2. Si un código celular observado no coincide exactamente con la lista pero difiere en solo 1 mismatch (distancia de Hamming = 1) y el valor de calidad Phred en la base mutada es bajo, el algoritmo lo corrige hacia el código celular de la lista blanca más probable con pseudoconteos bayesianos (`1MM_multi_Nbase_pseudocounts`).

---

## 4. Por qué el UMI es Esencial: Eliminación del Sesgo de PCR

En bulk RNA-Seq, un gen altamente expresado produce muchas lecturas; sin embargo, durante los ciclos de PCR, algunas secuencias se amplifican con mayor eficiencia que otras debido al contenido GC o longitud, generando lecturas duplicadas no proporcionales.

Con el **UMI**:
* Si observamos 50 lecturas con el mismo *Cell Barcode*, alineadas en el mismo exón y con **el mismo UMI**, sabemos que proceden de la amplificación de **una única molécula original de ARNm**. Por tanto, se desduplican y cuentan como **1 conteo**.
* Si observamos 50 lecturas en el mismo gen pero con **50 UMIs distintos**, sabemos que la célula transcribió realmente **50 moléculas independientes**.

---

[🏠 Temario Principal](../README.md) | [Siguiente: Lección 02 (STARsolo) ➡️](Leccion_02_STARsolo_y_Cuantificacion_Alineamiento.md)
