[⬅️ Anterior: Lección 05 (EmptyDrops)](Leccion_02_Deteccion_de_Gotas_Vacias_con_EmptyDrops.md) | [🏠 Temario Principal](../README.md) | [Siguiente: Hito 01 (Pipeline QC y Dobletes) ➡️](Hito_01_Pipeline_QC_Multivariante_Adaptativo.md)

---

# Lección 03: Identificación y Purga de Dobletes con DoubletFinder

---

## 1. Naturaleza y Tipología de los Dobletes

Un **doblete** (o multiplete) se origina cuando dos (o más) células independientes son encapsuladas por azar dentro de la misma microgota GEM y reciben el mismo código celular.

Existen dos tipos fundamentales:

* **Dobletes Homotípicos:** Co-encapsulación de dos células del mismo linaje (ej. dos linfocitos T CD4+). Tienen el doble de contenido de ARN, pero su perfil transcriptómico no se altera sustancialmente.
* **Dobletes Heterotípicos:** Co-encapsulación de dos células de linajes completamente distintos (ej. un linfocito T y un macrófago, o una célula epitelial tumoral y una célula endotelial).

### El Peligro de los Dobletes Heterotípicos en Biomedicina:

En el clustering downstream, los dobletes heterotípicos no se agrupan con sus poblaciones progenitoras, sino que forman **falsos clusters "intermedios" o quiméricos**. En oncología, esto ha llevado a publicar erróneamente el descubrimiento de "nuevos tipos celulares híbridos" o "fenotipos invasivos transdiferenciados" que en realidad eran artefactos técnicos de dobletes.

---

## 2. La Tasa de Dobletes en 10x Genomics

La frecuencia de dobletes sigue una relación lineal directa con el número de células cargadas en el chip de microfluídica:

| Células Recuperadas | Tasa Teórica de Dobletes (%) |
| :-----------------: | :--------------------------: |
| 1.000               | ~ 0.8                        |
| 5.000               | ~ 3.9                        |
| 10.000              | ~ 7.6                        |
| 20.000              | ~ 15.2                       |

---

## 3. Principio del Algoritmo `DoubletFinder`

Desarrollado por McGinnis et al. (*Cell Systems*, 2019), `DoubletFinder` opera bajo el siguiente principio algorítmico:

1. **Generación de dobletes sintéticos *in silico*:** A partir de los datos reales del experimento, promedia aleatoriamente pares de perfiles de expresión de células individuales para simular cómo se vería un doblete heterotípico perfecto.
2. **Co-proyección en espacio PCA:** Mezcla los datos reales con los dobletes sintéticos artificiales y calcula una reducción de componentes principales conjunta.
3. **Cálculo de la métrica pANN (Proportion of Artificial Nearest Neighbors):** Para cada célula real, busca sus K vecinos más cercanos en el espacio de PCA y cuantifica qué proporción de esos vecinos son dobletes sintéticos artificiales.
4. **Optimización de hiperparámetros:**
   * **pN:** Proporción de dobletes artificiales generados (generalmente 0.25).
   * **pK:** Tamaño del vecindario de vecinos más próximos. Debe optimizarse empíricamente mediante un barrido de varianza biparamétrica (*paramSweep*) para maximizar la métrica de bimodalidad media (*BCmvn*).
5. **Clasificación final:** Asigna la etiqueta `Doublet` o `Singlet` según la tasa esperada de dobletes del experimento.

---

[⬅️ Anterior: Lección 05 (EmptyDrops)](Leccion_02_Deteccion_de_Gotas_Vacias_con_EmptyDrops.md) | [🏠 Temario Principal](../README.md) | [Siguiente: Hito 01 (Pipeline QC y Dobletes) ➡️](Hito_01_Pipeline_QC_Multivariante_Adaptativo.md)
