[⬅️ Anterior: Lección 04 (QC Celular)](Leccion_01_Metricas_QC_y_Filtrado_Adaptativo.md) | [🏠 Temario Principal](../README.md) | [Siguiente: Lección 06 (DoubletFinder) ➡️](Leccion_03_Identificacion_y_Purga_de_Dobletes_Heterotipicos.md)

---

# Lección 02: Detección de Gotas Vacías con EmptyDrops

---

## 1. El Fenómeno del ARN Ambiental Libre (*Ambient RNA / Soup*)

Durante el proceso de disgregación enzimática y mecánica de un tejido sólido (por ejemplo, una biopsia de tumor), un porcentaje de las células se lisa involuntariamente antes de entrar en el cartucho de microfluídica.

El contenido celular de estas células rotas se vierte en el medio de suspensión, creando una "sopa" o **ARN ambiental libre**. Cuando las microgotas se forman, prácticamente todas capturan moléculas de esta sopa, incluso si **no contienen ninguna célula intacta**.

---

## 2. Limitaciones del Filtrado Simple por Corte de Conteos

Tradicionalmente se utilizaba el gráfico de rodilla (*barcode rank plot* o *knee plot*) ordenando los códigos celulares por número total de UMIs de mayor a menor y aplicando un corte horizontal arbitrario (por ejemplo, < 500 UMIs = gota vacía).

```
Log10(UMIs)
  ▲
  │   ═══════════ (Células reales con alta masa de ARN)
  │              \
  │               \ (Punto de Inflexión)
  │                \
  │                 ═════════════════════════ (Fondo ambiental: gotas vacías)
  └────────────────────────────────────────────► Códigos celulares ordenados
```

**El problema:** Células biológicas reales de tamaño diminuto (como linfocitos B/T en reposo o plaquetas) pueden tener apenas 300 UMIs, mientras que una gota vacía en una muestra con mucha lisis celular puede atrapar 600 UMIs procedentes exclusivamente de la sopa ambiental. Un corte arbitrario de conteos eliminaría las células pequeñas reales e incluiría gotas vacías contaminantes.

---

## 3. El Algoritmo Estadístico `EmptyDrops`

Desarrollado por Lun et al. (*Genome Biology*, 2019) e implementado en `DropletUtils::emptyDrops`:

1. **Estimación del perfil ambiental (Pool de fondo):** Se asume que todos los barcodes con muy bajos conteos (por defecto, ≤ 100 UMIs) corresponden con total certeza a gotas vacías. Con ellos se construye un perfil multinomial que representa las frecuencias relativas de cada gen en la "sopa".
2. **Contraste de Hipótesis Dirichlet-Multinomial:** Para cada código celular candidato (con conteos superiores al umbral de fondo, típicamente > 100 UMIs), se evalúa la probabilidad de que su distribución de transcritos provenga simplemente de un muestreo aleatorio del ARN ambiental.
3. **Cálculo de p-valores mediante simulaciones de Monte Carlo:** Si el perfil de expresión de la gota difiere significativamente del perfil de la sopa ambiental (p < 0.001, corregido por FDR), se concluye con rigor estadístico que la gota contiene una **célula biológica genuina**, sin importar que su número total de UMIs sea bajo.

```R
library(DropletUtils)

# Ejecución sobre matriz cruda (raw matrix)
set.seed(42)
e.out <- emptyDrops(raw_counts_matrix, lower = 100, niters = 10000)

# Células reales significativas (FDR < 0.001)
is.cell <- e.out$FDR <= 0.001 & !is.na(e.out$FDR)
celulas_filtradas <- raw_counts_matrix[, is.cell]
```

---

[⬅️ Anterior: Lección 04 (QC Celular)](Leccion_01_Metricas_QC_y_Filtrado_Adaptativo.md) | [🏠 Temario Principal](../README.md) | [Siguiente: Lección 06 (DoubletFinder) ➡️](Leccion_03_Identificacion_y_Purga_de_Dobletes_Heterotipicos.md)
