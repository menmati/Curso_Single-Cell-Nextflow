[⬅️ Anterior: Test Módulo 02](../Modulo_2_Control_de_Calidad_y_Dobletes/Test_Autoevaluacion_M2.md) | [🏠 Temario Principal](../README.md) | [Siguiente: Lección 08 (PCA y Selección de PCs) ➡️](Leccion_02_Reduccion_de_Dimensionalidad_PCA_y_Seleccion_de_PCs.md)

---

# Lección 01: Modelado Estadístico y Normalización con SCTransform v2

---

## 1. El Fracaso del Método Clásico `LogNormalize`

El método tradicional de normalización en Seurat (`NormalizeData(normalization.method = "LogNormalize")`) divide el conteo de cada gen por el número total de UMIs de la célula, multiplica por un factor de escala arbitrario (10.000) y aplica una transformación logarítmica:

$$
y_{ij} = \log\left( \frac{x_{ij}}{\sum_k x_{kj}} \times 10.000 + 1 \right)
$$

Aunque este método es intuitivo y computacionalmente barato, adolece de **tres fallos matemáticos graves**:

1. **Asume erróneamente una relación lineal** entre la profundidad de secuenciación y el conteo de todos los genes.
2. **Pseudo-conteo distorsionador:** El término +1 altera artificialmente la varianza de los genes poco expresados frente a los altamente expresados.
3. **No estabiliza la varianza:** Provoca que genes muy expresados dominen las componentes principales simplemente por tener una varianza técnica no corregida superior.

---

## 2. El Modelo Estadístico de `SCTransform`

Desarrollado por Hafemeister y Satija (*Genome Biology*, 2019) y refinado en su versión v2 por Choudhary y Satija (*Genome Biology*, 2022), `SCTransform` reemplaza la división global por un **modelo lineal generalizado con distribución Binomial Negativa (GLM-NB)** independiente para cada gen i en cada célula j:

$$
\log(\mu_{ij}) = \beta_{0i} + \beta_{1i} \log_{10}(\text{total\_umi}_j) + \beta_{2i} \text{percent.mt}_j
$$

* *μij* representa la tasa de expresión esperada del gen en la célula.
* La pendiente *β*1*i* mide la dependencia técnica real de ese gen concreto con respecto a la profundidad de secuenciación.
* Se estima un parámetro de dispersión *θi* que modela la hiperdispersión intrínseca del conteo.

### Regularización Bayesiana mediante Kernel Smoothing:

Para evitar que los genes con pocos conteos tengan estimaciones inestables de *β* y *θ*, el algoritmo ajusta una curva de regresión no paramétrica (*kernel density regression*) a través de todos los genes, regularizando los parámetros individuales hacia la tendencia global.

---

## 3. Residuos de Pearson: Los Nuevos Datos para PCA

Una vez ajustado el modelo nulo, la expresión normalizada de un gen no es un número de conteos, sino su **residuo de Pearson** (*zij*):

$$
z_{ij} = \frac{x_{ij} - \mu_{ij}}{\sqrt{\mu_{ij} + \frac{\mu_{ij}^2}{\theta_i}}}
$$

* Si *zij* *= 0*, el gen se expresa exactamente según lo esperado por el ruido técnico y el tamaño celular.
* Si *zij* *> 0* de forma marcada, la célula expresa ese gen significativamente por encima del ruido de fondo (biología activa).
* **Varianza unitaria:** La varianza de los residuos de Pearson está perfectamente estabilizada (*zi ≈ 1* para genes no variables), permitiendo que el PCA capture exclusivamente heterogeneidad biológica real.

---

## 4. Implementación en Seurat v5 con `glmGamPoi`

Para optimizar el tiempo de cómputo en grandes cohortes, se utiliza el solver `glmGamPoi` (Ahlmann-Eltze & Huber, 2020), que acelera el cálculo del modelo GLM-NB hasta 10× mediante optimización cuasi-Newton:

```R
library(Seurat)
library(glmGamPoi)

# Ejecución de SCTransform v2 en Seurat v5
obj <- SCTransform(
  obj,
  assay = "RNA",
  new.assay.name = "SCT",
  vst.flavor = "v2",
  method = "glmGamPoi",
  vars.to.regress = "percent.mt",
  verbose = TRUE
)
```

---

[⬅️ Anterior: Test Módulo 02](../Modulo_2_Control_de_Calidad_y_Dobletes/Test_Autoevaluacion_M2.md) | [🏠 Temario Principal](../README.md) | [Siguiente: Lección 08 (PCA y Selección de PCs) ➡️](Leccion_02_Reduccion_de_Dimensionalidad_PCA_y_Seleccion_de_PCs.md)
