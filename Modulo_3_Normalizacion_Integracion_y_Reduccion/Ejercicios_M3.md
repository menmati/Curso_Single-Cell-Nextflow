[⬅️ Anterior: Lección 09 (Integración Harmony)](Leccion_03_Integracion_Multimuestra_con_Harmony_en_Seurat_v5.md) | [🏠 Temario Principal](../README.md) | [Siguiente: Test de Autoevaluación M3 ➡️](Test_Autoevaluacion_M3.md)

---

# Ejercicios Prácticos — Módulo 03: Normalización e Integración

> **Instrucciones:**  
> Resuelve los siguientes retos técnicos aplicando los conocimientos adquiridos en las lecciones previas sobre modelado estadístico, selección dimensional e integración multimuestra.

---

### 📝 Ejercicio 3.1: Comparación Empírica de Residuos de Pearson frente a LogNormalize
* Escribe un script en R que tome un gen altamente variable (ej. `MALAT1` o `GAPDH`) y un gen marcador específico con expresión moderada/baja (ej. `CD3D` o `CD14`).
* Genera dos gráficos de dispersión (Scatter plot):
  1. En el eje X el total de UMIs de la célula (`nCount_RNA`) y en el eje Y la expresión bajo `LogNormalize`.
  2. En el eje X el total de UMIs de la célula y en el eje Y los residuos de Pearson bajo `SCTransform`.
* Explica cómo se observa en los gráficos la independencia técnica lograda por `SCTransform`.

---

### 📝 Ejercicio 3.2: Diagnóstico Cuantitativo del Efecto de Lote con kBET / LISI
* Define conceptualmente qué mide el índice de mezcla local **LISI** (*Local Inverse Simpson’s Index*) o **kBET** al evaluar una integración de célula única.
* Si tras aplicar Harmony observas que un cluster celular de macrófagos contiene un 98% de células del Paciente 1 y prácticamente 0% de los Pacientes 2 y 3, ¿cómo distinguirías si se trata de un fallo de integración de Harmony o de una verdadera respuesta biológica específica de ese paciente?

---

### 📝 Ejercicio 3.3: Preparación de Capas para Análisis Diferencial Post-SCTransform
En Seurat v5, tras ejecutar `IntegrateLayers` sobre el ensayo `SCT`, el objeto mantiene las capas divididas por muestra para preservar la corrección de varianza local.
* Escribe el comando formal de Seurat v5 necesario (`PrepSCTFindMarkers` o `JoinLayers`) para preparar el objeto antes de buscar genes marcadores diferenciales entre clusters mediante `FindMarkers()` o `FindAllMarkers()`.
* Justifica por qué invocar directamente `FindAllMarkers()` sobre un objeto con capas divididas sin este paso preliminar arrojará un error de dimensiones o sesgos estadísticos.

---

[⬅️ Anterior: Lección 09 (Integración Harmony)](Leccion_03_Integracion_Multimuestra_con_Harmony_en_Seurat_v5.md) | [🏠 Temario Principal](../README.md) | [Siguiente: Test de Autoevaluación M3 ➡️](Test_Autoevaluacion_M3.md)
