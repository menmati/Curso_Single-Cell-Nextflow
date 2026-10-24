[⬅️ Anterior: Lección 02](Leccion_02_STARsolo_y_Cuantificacion_Alineamiento.md) | [🏠 Temario Principal](../README.md) | [Siguiente: Ejercicios Prácticos M1 ➡️](Ejercicios_M1.md)

---

# Lección 03: Arquitectura del Módulo y Pipeline Nextflow DSL2

---

## 1. Diseño del Manifiesto de Entrada (`samplesheet.csv`)

En un pipeline de célula única, cada muestra experimental suele constar de pares de lecturas FASTQ y la especificación de la versión química utilizada (para cargar automáticamente la *whitelist* y longitudes de UMI adecuadas):

```csv
sample,fastq_1,fastq_2,chemistry
patient1_tumor,/data/p1_R1.fastq.gz,/data/p1_R2.fastq.gz,10xv3
patient2_tumor,/data/p2_R1.fastq.gz,/data/p2_R2.fastq.gz,10xv3
patient3_control,/data/p3_R1.fastq.gz,/data/p3_R2.fastq.gz,10xv3
```

---

## 2. El Proceso Modular `STARSOLO` en Nextflow DSL2

Siguiendo las mejores prácticas aprendidas en el diseño modular de Nextflow DSL2, creamos `modules/starsolo.nf`:

```groovy
/*
 * Módulo STARsolo para cuantificación de célula única (10x Genomics)
 */
process STARSOLO {
    tag "${meta.id}"
    label 'process_high'

    container 'quay.io/biocontainers/star:2.7.11b--h43eeafb_0'

    publishDir "${params.outdir}/starsolo/${meta.id}", mode: 'copy'

    input:
    tuple val(meta), path(reads)
    path  genome_index
    path  whitelist

    output:
    tuple val(meta), path("Solo.out/Gene/filtered"), emit: counts_filtered
    tuple val(meta), path("Solo.out/Gene/raw"),      emit: counts_raw
    tuple val(meta), path("*.sortedByCoord.out.bam"), emit: bam, optional: true
    path  "Solo.out/Gene/Summary.csv",               emit: summary
    path  "versions.yml",                            emit: versions

    script:
    def (r1, r2) = reads
    def umi_len  = meta.chemistry == '10xv2' ? 10 : 12
    """
    STAR \\
        --runThreadN ${task.cpus} \\
        --genomeDir ${genome_index} \\
        --readFilesIn ${r2} ${r1} \\
        --readFilesCommand zcat \\
        --soloType CB_UMI_Simple \\
        --soloCBwhitelist ${whitelist} \\
        --soloCBstart 1 --soloCBlen 16 \\
        --soloUMIstart 17 --soloUMIlen ${umi_len} \\
        --soloCBmatchWLtype 1MM_multi_Nbase_pseudocounts \\
        --soloUMIfiltering MultiGeneUMI_CR \\
        --soloUMIdedup 1MM_CR \\
        --soloCellFilter EmptyDrops_CR \\
        --outSAMtype BAM SortedByCoordinate \\
        --outSAMattributes NH HI AS nM CB UB \\
        --outFileNamePrefix ${meta.id}_

    mkdir -p Solo.out/Gene
    mv ${meta.id}_Solo.out/Gene/* Solo.out/Gene/

    cat <<-END_VERSIONS > versions.yml
    "${task.process}":
        star: \$(STAR --version | sed -e "s/STAR_//g")
    END_VERSIONS
    """
}
```

---

## 3. Orquestador Reactivo `main.nf`

El flujo de trabajo principal vincula el canal de entrada con el módulo y emite las matrices listas para el análisis con Seurat o Scanpy:

```groovy
#!/usr/bin/env nextflow
nextflow.enable.dsl = 2

include { STARSOLO } from './modules/starsolo'

params.samplesheet  = "${projectDir}/data/samplesheet.csv"
params.genome_index = "${projectDir}/reference/star_index"
params.whitelist_v3 = "${projectDir}/reference/3M-february-2018.txt.gz"
params.outdir       = "${projectDir}/results"

workflow {

    // 1. Ingesta del samplesheet
    Channel
        .fromPath(params.samplesheet)
        .splitCsv(header: true)
        .map { row ->
            def meta = [id: row.sample, chemistry: row.chemistry]
            [meta, [file(row.fastq_1), file(row.fastq_2)]]
        }
        .set { sc_reads_ch }

    // 2. Ejecución paralela de STARsolo por muestra
    STARSOLO(
        sc_reads_ch,
        file(params.genome_index),
        file(params.whitelist_v3)
    )

    // 3. Emisión de matrices hacia el canal downstream
    STARSOLO.out.counts_filtered.view { meta, path ->
        "Matriz generada para ${meta.id} en ${path}"
    }
}
```

---

[⬅️ Anterior: Lección 02](Leccion_02_STARsolo_y_Cuantificacion_Alineamiento.md) | [🏠 Temario Principal](../README.md) | [Siguiente: Ejercicios Prácticos M1 ➡️](Ejercicios_M1.md)
