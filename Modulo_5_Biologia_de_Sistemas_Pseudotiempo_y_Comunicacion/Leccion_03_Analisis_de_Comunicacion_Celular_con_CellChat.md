[⬅️ Anterior: Lección 14 (Dinámicas Génicas en Pseudotiempo)](Leccion_02_Modelado_de_Dinamicas_Genicas_a_lo_Largo_del_Pseudotiempo.md) | [🏠 Temario Principal](../README.md) | [Siguiente: Ejercicios Prácticos M5 ➡️](Ejercicios_M5.md)

---

# Lección 03: Redes de Comunicación Célula-Célula con CellChat v2

---

## 1. El Paradigma de la Señalización en el Microambiente

Las células dentro de un tejido sólido no actúan de forma aislada: interactúan dinámicamente mediante señales químicas paracrinas y yuxtacrinas. En biomedicina y oncología, comprender esta comunicación es vital:

* ¿Cómo reclutan las células tumorales a macrófagos inmunosupresores (vía *CCL2-CCR2*)?
* ¿Cómo inducen angiogénesis hacia las células endoteliales (vía *VEGFA-FLT1 / KDR*)?
* ¿Cómo apagan la respuesta de los linfocitos T citotóxicos (vía *CD274/PD-L1 - PDCD1/PD-1*)?

---

## 2. La Base de Datos Curada `CellChatDB`

Desarrollado por Jin et al. (*Nature Communications*, 2021) y expandido en `CellChat v2` (*Nature Methods*, 2023), **CellChat** modela la probabilidad de comunicación biológica integrando una base de datos curada exhaustiva (`CellChatDB`):

* **Complejos Multiméricos Reales:** A diferencia de herramientas tempranas que solo evaluaban pares monoméricos 1:1, CellChat modela formalmente receptores multiméricos (ej. receptores heterotrímeros de interleucinas formados por α, β y γ). Si una célula expresa la subunidad α pero carece de la subunidad β, el complejo no es funcional y la probabilidad de señalización decae a cero.
* **Cofactores Solubles y de Membrana:** Modela agonistas, antagonistas y correceptores estimuladores o inhibidores.

---

## 3. Ley de Acción de Masas y Probabilidad de Comunicación

CellChat calcula la probabilidad de comunicación intercelular (P{i,j}) entre la población emisora i y la población receptora j aplicando la **ley de acción de masas bioquímica**:

$$
P_{i,j} = \frac{L_i \times R_j}{K_d + L_i \times R_j}
$$

* L{i} es la expresión media regularizada del ligando en la población emisora.
* R{j} es la expresión media regularizada del receptor en la población diana.
* K{d} es la constante de disociación y afinidad de la interacción.
* La significancia estadística se evalúa mediante un **test de permutaciones aleatorias (p < 0.05)**.

```R
library(CellChat)

# 1. Crear el objeto CellChat a partir de Seurat v5
cellchat <- createCellChat(object = seurat_obj, group.by = "manual_annotation")

# 2. Cargar base de datos humana completa
CellChatDB <- CellChatDB.human
cellchat@DB <- CellChatDB

# 3. Preprocesamiento de expresión e inferencia de probabilidades
cellchat <- subsetData(cellchat)
cellchat <- identifyOverExpressedGenes(cellchat)
cellchat <- identifyOverExpressedInteractions(cellchat)

cellchat <- computeCommunProb(cellchat, raw.use = TRUE)
cellchat <- filterCommunication(cellchat, min.cells = 10)
cellchat <- computeCommunProbPathway(cellchat)
cellchat <- aggregateNet(cellchat)
```

---

## 4. Visualización de Redes de Señalización

CellChat ofrece diversas representaciones visuales de alto impacto:

1. **Gráfico Circular Global (*Circle Plot*):** Visualiza el número total de interacciones o la fuerza de señalización entre todos los tipos celulares.
2. **Diagramas de Cuerdas (*Chord Plots*):** Ilustran el flujo direccional de una vía específica (ej. señalización *CXCL*, *VEGF* o *TGFb*) desde las células emisoras hacia las receptoras.
3. **Mapas de Centralidad de Red (*Centrality Heatmap*):** Clasifica cada tipo celular según su rol topológico en la red:
   * **Emisor (*Sender*):** Produce activamente el ligando.
   * **Receptor (*Receiver*):** Posee alta densidad de receptores diana.
   * **Mediador (*Mediator*):** Conecta flujos entre múltiples poblaciones.
   * **Influenciador (*Influencer*):** Controla la topología global de la señalización.

```R
# Visualizar la vía de señalización VEGF mediante gráfico circular
pathways.show <- c("VEGF")
netVisual_aggregate(cellchat, signaling = pathways.show, layout = "circle")
```

---

[⬅️ Anterior: Lección 14 (Dinámicas Génicas en Pseudotiempo)](Leccion_02_Modelado_de_Dinamicas_Genicas_a_lo_Largo_del_Pseudotiempo.md) | [🏠 Temario Principal](../README.md) | [Siguiente: Ejercicios Prácticos M5 ➡️](Ejercicios_M5.md)
