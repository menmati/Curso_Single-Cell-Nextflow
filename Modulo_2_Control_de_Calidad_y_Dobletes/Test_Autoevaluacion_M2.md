[⬅️ Anterior: Hito 01 (Pipeline QC)](Hito_01_Pipeline_QC_Multivariante_Adaptativo.md) | [🏠 Temario Principal](../README.md)

---

# Test de Autoevaluación — Módulo 02

> **Instrucciones:**  
> Selecciona la respuesta correcta para cada una de las 5 preguntas y reflexiona sobre la justificación técnica de 2-3 líneas para cada opción elegida.

---

### Pregunta 1

**¿Cuál es la razón biológica fundamental por la cual una fracción elevada de lecturas mitocondriales (`percent.mt`) es indicativa de daño celular o apoptosis en scRNA-seq?**

* [A] Porque las células viables degradan sus mitocondrias antes de entrar en fase S del ciclo celular.
* [B] Porque la rotura mecánica de la membrana celular permite el escape rápido del ARNm citoplasmático, mientras que las mitocondrias intactas quedan retenidas en la célula dañada.
* [C] Porque el genoma mitocondrial no posee regiones codificantes de ARNm.
* [D] Porque los transcritos mitocondriales carecen de cola de poliadenilación y no hibridan con los cebadores.

---

### Pregunta 2

**¿Por qué el uso de la Desviación Absoluta de la Mediana (MAD) es metodológicamente superior a la desviación estándar ($\sigma$) para definir umbrales de filtrado en scRNA-seq?**

* [A] Porque la MAD solo se puede calcular en matrices de números enteros.
* [B] Porque Seurat v5 no permite operaciones matriciales basadas en varianza muestral.
* [C] Porque la desviación estándar exige que el número de células sea estrictamente superior a un millón.
* [D] Porque la media y la desviación estándar son extremadamente sensibles a los valores atípicos (outliers) extremos, sesgando los umbrales de corte en distribuciones no gaussianas.

---

### Pregunta 3

**¿Cómo distingue el algoritmo `EmptyDrops` entre una célula biológica real de pequeño tamaño y una gota vacía que contiene ARN ambiental libre (*soup*)?**

* [A] Mediante un modelo Dirichlet-multinomial que evalúa si el perfil de frecuencias de expresión génica de la gota difiere significativamente del perfil del fondo ambiental estimado a partir de gotas con muy bajos conteos.
* [B] Excluyendo cualquier gota que contenga menos de 10.000 lecturas.
* [C] Calculando el contenido en GC de los adaptadores Illumina de la lectura 1.
* [D] Buscando la presencia obligatoria de inmunoglobulinas o receptores de células T.

---

### Pregunta 4

**¿Qué consecuencia analítica crítica tienen los dobletes heterotípicos si no se eliminan antes del clustering?**

* [A] Provocan que el comando `RunPCA()` falle con error de división por cero.
* [B] Convierten todas las lecturas de la muestra en duplicados idénticos de PCR.
* [C] Generan clusters intermedios artificiales o artefactos quiméricos que pueden confundirse erróneamente con tipos celulares de transición o nuevos linajes celulares.
* [D] Reducen la tasa de alineamiento contra el genoma de referencia a cero.

---

### Pregunta 5

**En el flujo de trabajo de `DoubletFinder`, ¿cuál es el propósito de realizar un barrido de parámetros (*paramSweep*) para seleccionar el hiperparámetro pK?**

* [A] Determinar el número máximo de hilos de CPU que debe utilizar la computadora.
* [B] Forzar a que exactamente el 50% de las células sean clasificadas como dobletes.
* [C] Identificar el tamaño del vecindario en el espacio PCA que maximiza la métrica de bimodalidad de la distribución de vecinos artificiales (*BCmvn*), optimizando la precisión de detección.
* [D] Calcular la correlación de Pearson entre la longitud de los telómeros y el genoma mitocondrial.

---

[⬅️ Anterior: Hito 01 (Pipeline QC)](Hito_01_Pipeline_QC_Multivariante_Adaptativo.md) | [🏠 Temario Principal](../README.md)
