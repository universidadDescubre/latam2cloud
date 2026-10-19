
# Predicción de Rendimiento de Conectividad hacia Proveedores de Nube desde América Latina usando Aprendizaje Automático

**Autor:** Edwin Martin Salazar Vega
**Universidad:** Universidad Nacional de Ingeniería (UNI)
**Unidad:** Unidad de Posgrado — Facultad de Ingeniería Industrial e Ingeniería de Sistemas
**Grado:** Maestro en Inteligencia Artificial
**Asesor:** Glen Dario Rodriguez Rafael
**Lima, Perú · 2026**

---

## Para los asesores de tesis — público objetivo y hoja de ruta de este repositorio

Este repositorio está dirigido en primer lugar a mis asesores de tesis, para seguir el pipeline de investigación y acceder a los artefactos citables (notebooks, dataset, modelos, resultados) que respaldan el Anexo 5 de la tesis.

**Lo que hay hoy (alcance actual):**
- Los notebooks curados de las 12 etapas del pipeline (00→11), descritos en la tabla de abajo.
- Un único DOI en Zenodo (ver sección "Datos y Artefactos") que contiene el dataset completo, los pesos de modelo, los resultados de evaluación, **y una copia congelada de este mismo código**, todo bajo licencia CC-BY-4.0.

**Lo que viene después (hoja de ruta, aún no ejecutado):**
- **Documento de tesis completo** — se publicará aquí eventualmente, sujeto a la normativa de la Universidad Nacional de Ingeniería (UNI) sobre el depósito/difusión de tesis de maestría. Hasta que esa normativa quede confirmada, el documento permanece fuera de este repositorio.
- **Paper de revista (IEEE Access)** — se publicará aquí eventualmente, sujeto a que se resuelva la política de la revista sobre archivar el manuscrito públicamente antes/durante el proceso de revisión (EDW-123). Hasta entonces, el paper permanece fuera de este repositorio.

Ninguna de las dos exclusiones de arriba es definitiva — son temporales, a la espera de confirmar las reglas correspondientes antes de publicar.

---

## Descripción

Código y notebooks del proyecto de investigación de maestría cuyo objetivo es predecir degradaciones de conectividad entre nodos de América Latina y proveedores de nube (AWS, GCP, Azure) usando aprendizaje automático supervisado. Los modelos entrenados (Random Forest, XGBoost, LightGBM) operan sobre telemetría de red recolectada con ThousandEyes (OpenTelemetry v2) e integran el stack Splunk DSDL para su despliegue en producción.

**Dataset activo:** `te_features_v6.parquet` · 162 días (Apr 22 – Sep 30, 2026) · 7,628,801 ventanas · 1,035 rutas LATAM→Cloud — **no incluido en este repo, ver sección "Datos y Artefactos" abajo**.
**Candidato definitivo:** Ensemble D38 argmax — RF v5.1 × 0.80 + LGB v7 × 0.20 · QWK_val = 0.6529 ✅ · RecAlta_val = 0.6509 ✅ (LOG-25 · LOG-50)

---

## Datos y Artefactos

Este repositorio contiene el código (notebooks, scripts, documentación). El dataset completo, los pesos de modelo entrenados, los resultados/outputs de evaluación, **y una copia congelada de este mismo código**, están publicados juntos en un único depósito Zenodo (licencia CC-BY-4.0 para todo el depósito; el código en este repo mantiene su propia licencia MIT, ver `LICENSE`):

[![DOI](https://zenodo.org/badge/doi/10.5281/zenodo.23455883.svg)](https://doi.org/10.5281/zenodo.23455883)

> **DOI:** [`10.5281/zenodo.23455883`](https://doi.org/10.5281/zenodo.23455883)

Ese depósito contiene 54 archivos: 21 de dataset/modelos/resultados subidos individualmente (sin agrupar en `.zip` — no es la convención de Zenodo), más 33 de la copia del código. Detalle completo de cada archivo (tamaño, checksum MD5, notebook de origen) en `MANIFEST.md`, publicado dentro del propio depósito.

**Para ejecutar los notebooks con datos reales:** descarga los archivos que necesites del DOI de arriba y colócalos en una carpeta local `data/` (no trackeada en este repo — ver `.gitignore`) antes de correr cada notebook. Cada notebook documenta en su primera celda qué archivo(s) espera.

---

## Pipeline de Investigación — Estado Actual

| Etapa | Notebook | Descripción | Estado |
|---|---|---|---|
| 0 · Ingesta | `00 search load 01.ipynb` + `00 search load 52.ipynb` (muestra, ver nota ⬇) | 72 parquets · 3,306 MB · 525M filas · 162 días | ✅ D41-1 |
| 1 · EDA | `01 dataset analisis v10.ipynb` | 162 días · 1,035 rutas · BGP 1:45 · distribución de clases | ✅ D41-2 (2026-10-04) |
| 2 · Calibración | `02 calibracion v7.ipynb` | BD=7, k=(1.0,2.0,3.5), OR≥4/6 · congelado (LOG-33) | ✅ Congelado |
| 3 · Etiquetado | `03 etiquetado v4.ipynb` ⭐ FINAL + `v3.ipynb` (provenance, ver nota ⬇) | 7,628,801 ventanas · 1,035 rutas · splits 70/15/15 · NO re-ejecutar (LOG-38) | ✅ D41-3 (2026-10-05) |
| 4 · Features | `04 feature_engineering v6.ipynb` ⭐ FINAL + `v4/v5.ipynb` (provenance, ver nota ⬇) | 100 features · `te_features_v6.parquet` · NaN máx t-48=0.86% (LOG-41) | ✅ D41-4 (2026-10-05) |
| 5 · Modelado | `05 modelado v4/v5/v7/v8/v9/v10-v15/rf_retrain v1.ipynb` (12 notebooks, ver nota ⬇) | Candidato definitivo: Ensemble D38 (RF v5.1 × 0.80 + LGB v7 × 0.20) · LOG-25, LOG-50 | ✅ D41-5 (2026-10-06) |
| 6 · Eval. test | `06 evaluacion test set v7.ipynb` ⭐ + `v6.ipynb` (ver nota ⬇) | ONE-SHOT Sep 8–Sep 30, 2026 · QWK=0.6786 ✅ RecAlta=0.4966 ❌ · H2 VERIFICADA (Dim-1) · LOG-52/53 | ✅ D41-6 (2026-10-10) |
| 7 · Ablación BGP v2 | `07 ablacion_bgp v2.ipynb` ⭐ FINAL | Re-confirmación H1 con 162 días · RF ΔQWK=−0.0000 ✅ · LGB ΔQWK=−0.0113 ✅ · LOG-54 | ✅ D41-7 (2026-10-12) |
| 8 · ONNX export | `08 onnx_export v1.ipynb` | `lgb_v7.onnx` + `ensemble_d38_infer.py` · QWK D40=0.6786 · LOG-56 | ✅ Ejecutado 2026-10-12 |
| 9 · Inferencia DSDL | `09 dsdl_inference v1.ipynb` | Smoke test · QWK=0.6786 ✅ · SLA<100ms ✅ · LOG-57 | ✅ Ejecutado 2026-10-12 |
| 10 · Dashboard | `10 splunk_dashboard v1.ipynb` | MTTD_ML=8.7 min vs baseline 30.0 min (−71%) · H3 VERIFICADA · LOG-63 | ✅ Ejecutado 2026-10-12 |
| 11 · DSDL Live Apply | `11 splunk_dsdl_apply v1.ipynb` + `latam2cloud_monitor_dashboard_v4.xml` (ver nota ⬇) | Endpoint `latam2cloud_monitor` · SLA<100ms · dashboard v4 en producción | ✅ Ejecutado 2026-10-14 |

⚠️ Pendiente confirmar esta lista es la final exacta contra `tesis status.md` antes de copiar al repo definitivo (ver `latam2cloud-docs/zenodo/ZENODO_DOI_PLAN_EJECUCION.md` §3.4).

### Nota general sobre la limpieza de notebooks

Cada notebook se verifica contra el MD de análisis correspondiente en `latam2cloud-docs/pipeline-analysis/` (cifras de outputs reales, no solo nombre de archivo) y se confirma que no tenga celdas con output de tipo `error` antes de copiarlo aquí. Al copiarlo:

1. **Solo quedan celdas de código y su output.** Se eliminan todas las celdas markdown (encabezados, explicaciones) — la copia en esta carpeta es código + output, no el notebook narrado completo.
2. **Se eliminan todos los comentarios de código**, tanto líneas completas de comentario como comentarios al final de una línea de código (ej. `x = {}  # nota`). La eliminación usa el módulo `tokenize` de Python para distinguir un comentario real de un carácter `#` que forma parte de un string (ej. `print(f"Ruta #1")` no se toca).

El código ejecutable y los outputs de celda no se modifican. **El notebook original en `splunkLab` (rama `jupyter/notebooks`) nunca se modifica** — la limpieza se aplica solo a la copia que vive en esta carpeta.

⚠️ **Correcciones (2026-10-15):**
- La primera pasada de limpieza (etapas 0, 1 y 2) solo eliminaba líneas de comentario completas, dejando comentarios al final de línea sin eliminar. Se corrigió el método (ahora basado en `tokenize`) y se re-procesaron los notebooks de las etapas 1 y 2 (la etapa 0 no tenía comentarios al final de línea, quedó igual).
- La copia de la Etapa 1 (`01 dataset analisis v10.ipynb`) conservaba 18 celdas markdown (encabezados de sección). Se corrigió: ahora solo tiene las 19 celdas de código originales, sin markdown.

### Nota sobre la muestra de la Etapa 0 (Ingesta)

La etapa de ingesta generó 51 notebooks casi idénticos (`00 search load 01.ipynb` … `00 search load 52.ipynb`, numeración no consecutiva), uno por combinación de rango de fechas / cloud provider, todos ejecutando la misma lógica de carga. Copiar las 51 variantes no aporta valor al repo curado — solo serían ejecuciones repetidas del mismo código. Por eso aquí se incluyen únicamente **2 notebooks representativos**:

- **`00 search load 01.ipynb`** — primera ejecución de la etapa.
- **`00 search load 52.ipynb`** — última ejecución de la etapa.

Ambos fueron verificados fila por fila contra sus outputs reales (no solo por nombre de archivo): ninguno de los dos presenta outputs de tipo `error` — es decir, muestran el proceso de ingesta corriendo limpio. Se descartó deliberadamente mostrar como ejemplo algún notebook con un incidente de datos (por ejemplo, una ejecución con un parquet corrupto) para no sugerir que ese tipo de error es representativo del proceso general.

El conjunto completo de 51 notebooks permanece versionado en `splunkLab` (privado), rama `jupyter/notebooks`, carpeta `latam2cloud-notebooks/`.

`00 search load 01.ipynb` no tenía comentarios de código; `00 search load 52.ipynb` tenía 1 línea, eliminada (ver nota general de limpieza arriba).

### Nota sobre la Etapa 1 (EDA)

`01 dataset analisis v10.ipynb` es la única versión final de esta etapa (existen v1–v10 en `jupyter/notebooks`; solo v10 corresponde al análisis documentado). Verificado contra `latam2cloud-docs/pipeline-analysis/DSDL_DATASET_ANALYSIS.md`: las cifras de los outputs reales del notebook (525,019,966 filas, 162 días, 3,306 MB, 45 agentes, 27 ciudades, 24 destinos) coinciden con las del MD. El notebook original tiene 37 celdas (18 markdown + 19 código), 0 outputs de tipo `error`. La copia aquí conserva solo las 19 celdas de código y se eliminaron 266 comentarios de código (ver correcciones en nota general de limpieza arriba).

### Nota sobre la Etapa 2 (Calibración)

`02 calibracion v7.ipynb` es la versión final y **congelada** (LOG-33) de esta etapa — existen v0, v1, "v2 v3" (combinado), v4, v5 y v6 en `jupyter/notebooks`, todas superadas. Verificado contra `latam2cloud-docs/pipeline-analysis/CALIBRATION_ANALYSIS.md`: los outputs reales del notebook reproducen exactamente los parámetros congelados documentados — `bd=7, k=(1.0,2.0,3.5)` → `Normal=88.0% Baja=8.0% Media=1.9% Alta=2.1%` — y la cobertura de datos (39 parquets, 1,035 rutas: AWS=405, Azure=315, GCP=315). 6 celdas, 0 outputs de tipo `error`. Se eliminaron 45 comentarios de código en la copia — líneas completas y también comentarios al final de una línea de código (ver corrección en nota general de limpieza arriba).

Nota metodológica: esta calibración corre sobre el subconjunto de 83 días (39 parquets) disponible en julio 2026, no sobre los 162 días finales de la Etapa 0 — es intencional, ya que la regla se congela una sola vez y se aplica igual sobre el dataset completo en la Etapa 3 (Etiquetado).

### Nota sobre la Etapa 3 (Etiquetado)

`03 etiquetado v4.ipynb` es la versión final — existen `03 etiquetado.ipynb` (sin versión, la original), v2 y v3 en `jupyter/notebooks`. El propio notebook v4 está marcado "NO re-ejecutar" (LOG-38) porque fija el split 70/15/15 usado en el resto del pipeline (anti-HARKing, LOG-37). Verificado contra `latam2cloud-docs/pipeline-analysis/LABELING_ANALYSIS.md`: los outputs reales coinciden exactamente — 7,628,801 ventanas, 1,035 rutas (AWS=405, Azure=315, GCP=315), distribución Normal=86.3% Baja=9.5% Media=2.2% Alta=2.0%, cobertura test set 99.6% (1,138,360 ventanas). 8 celdas, 0 outputs de tipo `error`. Se eliminaron 117 comentarios de código en la copia (ver nota general de limpieza arriba).

**`03 etiquetado v3.ipynb` — incluido por provenance (2026-10-18).** No es un duplicado de v4: es la versión que corre sobre el dataset incompleto de 83 días (May–Jul 2026) con **los mismos parámetros de calibración congelados** (LOG-39: "parámetros idénticos pero dataset incompleto"). Se incluye porque su output (`te_baseline_stats_v3.parquet`, `te_labeled_ventanas_v3.parquet`) es el que consumen realmente `04 feature_engineering v4.ipynb` y, a través de él, `v5.ipynb` — confirmado leyendo el código fuente de ambos (no solo por fecha). Verificación cruzada: el test set de v3 reporta 993,449 ventanas · 20 días · 1,035 rutas · cobertura 100%; ese mismo número (993,449) reaparece exacto en el split de test que guarda `04 feature_engineering v5.ipynb`, confirmando la cadena. 8 celdas, 0 outputs de tipo `error`. 28 comentarios de código eliminados en la copia. `03 etiquetado v2.ipynb` queda excluido — no tiene ninguna mención en `DECISION_LOG.md`, `LABELING_ANALYSIS.md` ni `tesis status.md`, por lo que no hay nada contra qué verificarlo.

### Nota sobre la Etapa 4 (Feature Engineering)

`04 feature_engineering v6.ipynb` es la versión final — existen `04 feature_engineering.ipynb` (sin versión, la original), v2, v3, v4 y v5 en `jupyter/notebooks`; el espacio de features está congelado desde v5 (LOG-39, anti-HARKing), v6 es solo la re-ejecución sobre el dataset extendido de 162 días. Verificado contra `latam2cloud-docs/pipeline-analysis/FEATURE_ENGINEERING_ANALYSIS.md`: los outputs reales coinciden exactamente — 7,628,801 filas × 109 columnas (100 features de modelo), 1,035 rutas, y los NaN estructurales de los lags (`t-1`=3,915/0.05%, `t-2`=5,887/0.08%, `t-48`=65,640/0.86%). 11 celdas, todas código (sin markdown en el original), 0 outputs de tipo `error`. Se eliminaron 112 comentarios de código en la copia (ver nota general de limpieza arriba).

**`04 feature_engineering v4.ipynb` y `v5.ipynb` — incluidos por provenance (2026-10-18).** `FEATURE_ENGINEERING_ANALYSIS.md` §1.3 los marca como "históricas — no analizadas en este documento" (ya no tienen desglose celda por celda en los MD vivos), pero sí tienen decisión citada en `DECISION_LOG.md`: LOG-14 (FE v4, 2026-07-27, artefacto `te_features_v4.parquet` 3,763,917×122, 119 features ML — corrige el bug W<H de FE v3) y D21/D23 vía LOG-17 §17.3.A (FE v5, agrega features `y_lag1/2/3` tras confirmar que H2 no se verificó en test v4, LOG-19). Se incluyen porque **son el input real** de los notebooks de modelado ya seleccionados — confirmado leyendo el código fuente de cada uno, no por fecha: `te_features_v4.parquet` lo carga `05 modelado v4.ipynb`; `te_features_v5.parquet` lo cargan `05 modelado v5/v7/v8/v9.ipynb`, `rf_retrain v1.ipynb` y `06 evaluacion test set v5/v6/v7.ipynb`. Verificación cruzada: el shape guardado por v4 (122 columnas) coincide con LOG-14; el shape guardado por v5 (3,763,917 × 109, 100 features modelo, split test=993,449) coincide exactamente con el test set de `03 etiquetado v3.ipynb` (993,449 ventanas). 0 outputs de tipo `error` en ambos. 125 (v4) y 145 (v5) comentarios de código eliminados en las copias.

### Nota sobre la Etapa 5 (Modelado)

A diferencia de las etapas anteriores, aquí **no hay un único notebook final** — la etapa de modelado es la narrativa de búsqueda científica OE2 completa, y cada versión documentada prueba una hipótesis distinta citada individualmente en `MODELING_ANALYSIS.md` y `tesis status.md`. Se incluyen **12 notebooks**:

**Evolución v4→v9 (documentada en `MODELING_ANALYSIS.md` §1, dataset de 83 días):**

| Notebook | Qué prueba | Resultado |
|---|---|---|
| `v4` | Introduce GRU + ablación D12-REV (lag48 vs lag336) | Confirma eliminar lag336 |
| `v5` | Introduce LightGBM + primer ensemble soft-voting | — |
| `v7` | Corrige desbalance con `sample_weight=sqrt`, grid search de pesos del ensemble | Ensemble 0.80/0.05/0.15, QWK=0.6453 |
| `v8` | Threshold tuning post-training (D35/D36) | Sube QWK pero colapsa RecAlta — descartado |
| `v9` | RF `balanced_subsample` + nuevo grid search | **Candidato congelado (LOG-25)** |

**Búsqueda OE2 sobre dataset extendido 162 días (documentada en `tesis status.md`, filas 5h–5m):**

| Notebook | Qué prueba | Resultado |
|---|---|---|
| `v10` | Concept drift hipotética | Sin candidato |
| `v11` (P1) | Refuta la hipótesis de concept drift | Sin candidato |
| `v12` (P2) | XGBoost sobre 162 días | Sin candidato |
| `v13` (T1+T2) | Barrido de umbrales + tuning LGB | Sin candidato — búsqueda cerrada |
| `v14` (R5+R1) | SMOTE + threshold tuning | Candidato provisional (descartado en v15) |
| `v15` (R2+R3) | BBSE + warm-start | Sin candidato nuevo — confirma v9 como definitivo (LOG-50) |

**Reconstrucción operacional:**

| Notebook | Qué hace |
|---|---|
| `rf_retrain v1` | Reconstruye el modelo RF v5.1 bit-a-bit (el `.pkl` original de v9 se perdió) — es el modelo realmente usado en el one-shot test final (`06 evaluacion test set v7.ipynb`, Celda 5). max_diff=0.000000 vs las probabilidades congeladas en LOG-25 |

**Excluidos:**
- `v1`, `v2`, `v3` — el propio `MODELING_ANALYSIS.md` §1 los excluye explícitamente: su pipeline de preprocessing difiere sustancialmente (violación W<H en FE v3) y sus resultados fueron superados a partir de v4. Nunca documentados individualmente.
- `v6` (plano, sin "b") — superado por `v6b` el mismo día (31-jul-2026), nunca documentado por separado.
- **`v6b` — excluido por falta de verificación.** El archivo en `jupyter/notebooks` no tiene ningún output guardado (las 8 celdas de código tienen `execution_count=None`, 0 outputs cada una). No fue posible confirmar contra el notebook real las cifras que `MODELING_ANALYSIS.md` §1.3 le atribuye (XGBoost v6 QWK=0.6000/RecAlta=0.4655, LightGBM v6 QWK=0.6037/RecAlta=0.4819) — esas cifras existen solo en el MD, no en el archivo ejecutado. El hallazgo de v6b (más iteraciones con `mlogloss` agrava el desbalance) queda documentado únicamente por referencia textual al MD, sin copiar el notebook.

**Verificación aplicada a los 12 notebooks incluidos:** 0 celdas con output de tipo `error` en todos; cifras de outputs reales (QWK, RecAlta, pesos de ensemble, tamaños de split, etc.) cruzadas una por una contra `MODELING_ANALYSIS.md` y `tesis status.md` — coinciden exactamente en todos los casos. Comentarios de código eliminados: v4=45, v5=124, v7=37, v8=24, v9=27, v10=192, v11=203, v12=69, v13=41, v14=44, v15=91, rf_retrain v1=14 (ver nota general de limpieza arriba). Ninguno de los 12 originales tenía celdas markdown.

**Nota de provenance (2026-10-18):** `v4`, `v5`, `v7`, `v8`, `v9` y `rf_retrain v1` no usan `te_features_v6.parquet` (el único parquet de la Etapa 4 copiado originalmente) — usan `te_features_v4.parquet` y `te_features_v5.parquet`. Por eso se agregaron `04 feature_engineering v4/v5.ipynb` (y, por extensión, `03 etiquetado v3.ipynb`) a sus respectivas etapas — ver notas de las Etapas 3 y 4 arriba. Solo `v10`–`v15` usan `te_features_v6.parquet`.

### Nota sobre la Etapa 6 (Evaluación en Test Set)

`06 evaluacion test set v7.ipynb` es el **ONE-SHOT definitivo** — veredicto final de H2 (`TEST_SET_EVALUATION.md` §30). Verificado: QWK=0.6786, RecAlta=0.4966, MacroF1=0.6763, BalAcc=0.6647, split Sep 8–30 2026 (162 días, 1,136,290 ventanas) — coincide exactamente. Usa `te_features_v6.parquet`.

**`06 evaluacion test set v6.ipynb` — incluido.** Confirma el mismo resultado que `v5` (Δ=0 en todas las métricas — por eso `v5` queda excluido, es una repetición idéntica, no un hallazgo nuevo) **y genera los artefactos ONNX** (`feature_cols_v5.json`, `X_test_rf.npy`, `X_test_lgb.npy`, `y_test.npy`) requeridos por `08 onnx_export v1.ipynb`. Verificado contra `TEST_SET_EVALUATION.md` §3.2: QWK=0.6381, RecAlta=0.5954, MacroF1=0.6687, BalAcc=0.6668, split Jul 3–22 2026 (83 días, 991,379 ventanas) — coincide exactamente. Usa `te_features_v5.parquet` (de ahí la relevancia de haber agregado esa Etapa 4 adicional). 0 outputs de tipo `error` en ambos (solo una celda final vacía sin contenido en cada uno, no es un problema de verificación). 36 (v6) y 74 (v7) comentarios de código eliminados en las copias.

**Excluidos:** `v2`, `v3`, `v4` — evaluaciones de modelos descartados en la búsqueda OE2 (v2/v3 con bug de leakage W<H); su detalle completo vive en `MODELING_ANALYSIS.md`, no en `TEST_SET_EVALUATION.md`. `06 evaluacion test set.ipynb` (sin versión) y `06 parquet 2 julio.ipynb` — sin ninguna mención en los MD de análisis.

### Nota sobre la Etapa 7 (Ablación BGP)

`07 ablacion_bgp v2.ipynb` es el notebook que **cierra H1 definitivamente** — `tesis status.md` lo marca "OE1 CERRADO DEFINITIVAMENTE" (LOG-54), sobre el dataset extendido de 162 días. Verificado contra `ABLATION_BGP_ANALYSIS.md`: BGP MDI RF=1.29%, Config C QWK=0.0131 (RF) / 0.0040 (LGB) — coincide exactamente. 16 celdas (1 markdown eliminada en la copia), 0 outputs de tipo `error`. 36 comentarios de código eliminados.

**Excluidos — se mantiene solo `v2` para esta etapa (decisión de Edwin):**
- **`07 ablacion_bgp.ipynb` (v1) — excluido por redundancia, no por falta de verificación.** Se verificó igual de bien que `v2` (tabla completa Config A/B/C × RF/XGB/LSTM, outputs reales coherentes, conclusión consistente: BGP no ayuda). Pero `tesis status.md` lo marca "✅ Referencia" (no el cierre oficial) mientras que `v2` es "OE1 CERRADO DEFINITIVAMENTE" sobre el dataset completo.
- **`07 ablacion_multi_horizonte.ipynb` (L2-A) — excluido.** Es un experimento distinto (¿el horizonte T+30 vs T+60 cambia la conclusión sobre BGP?), documentado en `DECISION_LOG.md` con tabla propia y verificado cifra por cifra (RF Config A T+30 QWK=0.6537, etc. — coincide exacto). Se descarta igualmente para mantener la etapa 7 enfocada únicamente en `v2`, el cierre oficial de H1.
- **`07 ablacion_5min_pipeline.ipynb` (L2-C) — excluido por falta de verificación**, mismo criterio que `v6b` en la Etapa 5. `DECISION_LOG.md` cita para este notebook RF T+5 QWK=0.6478, T+15=0.6391, etc., pero el archivo real en `jupyter/notebooks` muestra RF T+5 QWK=0.3554, T+15=0.2454, T+30=0.1916 — valores completamente distintos y degenerados — y le faltan los resultados de LSTM en T+30/T+60 que el log sí reporta. El propio log menciona que la primera ejecución de este notebook tuvo "tres problemas estructurales" que requirieron corrección y una re-ejecución; todo indica que el archivo guardado es esa primera ejecución con bugs, no la versión corregida. El hallazgo L2-C queda documentado solo por referencia textual al `DECISION_LOG.md`, sin copiar el notebook.

### Nota sobre la Etapa 8 (ONNX Export)

`08 onnx_export v1.ipynb` es el **único archivo existente** para esta etapa. `DECISION_LOG.md` (D41-9) planificó un `08 onnx_export v2.ipynb` que nunca se creó como archivo separado — el notebook v1 se re-ejecutó sobre el candidato re-entrenado (igual patrón que `05 modelado` y `06 evaluacion test set`, donde el plan D41 usaba nombres distintos a los finalmente ejecutados). Verificado contra `MLOPS_PIPELINE.md` §7 y `tesis status.md`: SHA256 de `lgb_v7.onnx` (`9524180dad737af6`), epsilon máximo RF-vs-ONNX = 8.49e-08, QWK D40=0.6786 — coincide exactamente. 12 celdas markdown + 11 de código en el original; la copia conserva solo las 11 de código. 0 outputs de tipo `error`. Se eliminaron 54 comentarios de código en la copia.

**Caso especial de limpieza:** una de las celdas no contiene código ejecutado directamente, sino que **construye el wrapper de inferencia de producción `ensemble_d38_infer.py` como un string de Python** (`WRAPPER_CODE = '''...'''`) que luego se escribe a disco — es el artefacto real que usan los notebooks 09 y 11 en producción. Como ese string es código (no texto narrativo), se le aplicó la misma limpieza de comentarios al contenido interno del string (90 → 85 líneas), preservando el docstring del módulo. Se verificó que el script embebido sigue siendo Python válido después de la limpieza (`ast.parse` exitoso).

### Nota sobre la Etapa 9 (Inferencia DSDL)

`09 dsdl_inference v1.ipynb` es el **único archivo existente** para esta etapa — mismo patrón que la Etapa 8: `DECISION_LOG.md` (D41-9) planificó un `v2.ipynb` que nunca se creó como archivo separado. Verificado contra `MLOPS_PIPELINE.md` §7.3 y `tesis status.md`: QWK=0.6786, RecAlta=0.4966, 1,138,360 ventanas inferidas, `predictions_d38.parquet` 507.4 MB, MLflow run `4e0516d4` — coincide exactamente. 11 celdas markdown + 10 de código en el original; la copia conserva solo las 10 de código. 0 outputs de tipo `error`. Se eliminaron 56 comentarios de código en la copia.

**Mismo caso especial de limpieza que la Etapa 8:** una celda construye el script de integración Splunk AI Toolkit `dsdl_apply_d38.py` como string de Python (`DSDL_APPLY_CODE = '''...'''`), que se escribe a disco y es el artefacto real usado en producción (notebook 11). Se le aplicó la misma limpieza de comentarios al contenido interno (81 → 78 líneas), preservando el docstring del módulo, y se verificó que sigue siendo Python válido tras la limpieza.

### Nota sobre la Etapa 10 (Dashboard + MTTD + H3)

`10 splunk_dashboard v1.ipynb` es el **único archivo existente** para esta etapa. Verificado contra `MLOPS_PIPELINE.md` y `tesis status.md`: 22,716 eventos totales, 3,785 alertados, Wilcoxon W=3,933,850.5, p=1.3689×10⁻¹⁴⁸ (coincide exacto con el "1.37×10⁻¹⁴⁸" citado), MTTD_ML≈8.69 min, MTTD_baseline=30.0 min — todo coincide exactamente. 2 celdas markdown + 13 de código en el original; la copia conserva solo las 13 de código. 0 outputs de tipo `error`. Se eliminaron 68 comentarios de código en la copia; se preservaron intencionalmente 7 líneas con `#` que son códigos de color hexadecimales en strings (`'#2ecc71'`, paleta del dashboard), no comentarios.

**Nota de verificación:** el `run_id` de MLflow real en el notebook (`a020ca029f454be6b1e1e1e67a6be59c`) no coincide con el citado en la documentación (`6ca9c9d9`). Dado que el resto del contenido científico (todas las cifras anteriores) coincide exactamente, esto se interpreta como un run_id distinto de una re-ejecución posterior con resultados idénticos (el cálculo es determinista) — no como un problema de verificación como el de `07 ablacion_5min_pipeline.ipynb`.

### Nota sobre la Etapa 11 (DSDL Live Apply)

`11 splunk_dsdl_apply v1.ipynb` es el **único archivo existente** para esta etapa. A diferencia de las etapas anteriores, este notebook no verifica una hipótesis nueva — H3 ya quedó verificada en la Etapa 10 — sino que es evidencia de **despliegue/factibilidad operacional**: registra el modelo como `latam2cloud_monitor` en el contenedor Splunk MLTK y mide la latencia real de `apply()` contra el SLA (<100ms/ventana). Se incluye como evidencia de que la arquitectura en tiempo real (parte de H3) es desplegable en producción, no solo teórica.

**Verificado:** los 7 artefactos D40 (`ensemble_d38_infer.py`, `lgb_v7.onnx`, `rf_v51_model.pkl`, `scaler_v5.pkl`, `feature_cols_v5.json`, `ensemble_d38_manifest.json`, `dsdl_apply_d38.py`) con sus checksums — el checksum de `lgb_v7.onnx` (`9524180dad737af6`) coincide exactamente con el verificado en las Etapas 8 y 9. SLA<100ms cumplido en todos los batches del benchmark (1/10/100/500/1000 ventanas). 0 outputs de tipo `error`.

**Nota de verificación (variación esperable, no estructural):** las cifras exactas de latencia difieren levemente de las citadas en `tesis status.md` — el notebook muestra 11.957 ms/ventana para el `apply()` sobre 500 ventanas reales (vs "14.8 ms" citado) y 63.839 ms/ventana en el batch=1 del benchmark (vs "63.4 ms" citado). Son mediciones de wall-clock (latencia real de inferencia), inherentemente variables entre ejecuciones por carga del sistema — no deterministas como un QWK o un p-value. La conclusión (SLA cumplido) y los checksums de artefactos coinciden exactamente; solo la medición fina de tiempo varía, análogo a la discrepancia de `run_id` en la Etapa 10.

**Nota de reproducibilidad:** este notebook requiere una instancia Splunk DSDL real y credenciales interactivas (login vía `getpass` en el flujo de registro, no capturado en el output guardado por diseño de seguridad) — no es ejecutable por alguien que solo clona el repo curado, a diferencia de los notebooks 00–10. Se incluye como evidencia documentada del despliegue, no como artefacto reproducible de extremo a extremo.

Se eliminaron 57 comentarios de código en la copia; 3 celdas markdown removidas del original (quedan 9 de código).

**Dashboard — solo `latam2cloud_monitor_dashboard_v4.xml`** (copiado directo en `notebooks/`, sin subcarpeta `dashboard/`, aunque en `jupyter/notebooks` sí vive en `latam2cloud-notebooks/dashboard/`). Existen 4 versiones del dashboard (v1–v4) y un archivo `latam2cloud_monitor.py` (wrapper del algoritmo DSDL, LOG-65). Decisión de Edwin tras revisión directa:
- **`v1.xml`, `v2.xml`, `v3.xml` — excluidos.** `v4.xml` es superset de `v3.xml` (v3 = 2 filas RT puro; v4 = esas mismas 2 filas + 2 filas adicionales de historial = 4 filas totales) y, según confirma Edwin, **solo v4 está realmente implementado** en producción — v1/v2/v3 quedaron como pasos intermedios superados, no piezas que aporten algo que v4 no tenga.
- **`latam2cloud_monitor.py` — excluido.** Verificado que es un archivo standalone, no generado ni embebido por ningún notebook (el notebook 11 solo referencia el nombre `MODEL_NAME = 'latam2cloud_monitor'` como string y documenta su registro vía SPL, pero no contiene su código fuente). Decisión de Edwin: no es necesario para el repo curado — es infraestructura de registro en Splunk, no evidencia científica ni código reproducible fuera del lab.
- Sin hallazgos de seguridad en `v4.xml` (los "token" encontrados son variables de filtro de dashboard de Splunk Simple XML, no secretos). El XML se copia tal cual — no es código Python, no aplica la limpieza de comentarios/markdown.

---

## Estado de Hipótesis

| Hipótesis | OE | Estado | Resultado clave |
|---|---|---|---|
| **H1** | OE1 | ✅ VERIFICADA · RE-CONFIRMADA (D41-7, LOG-54) · OE1 CERRADO | 7,628,801 ventanas · BD=7, k=(1.0,2.0,3.5) · Ablación BGP v2: RF ΔQWK=−0.0000 ✅, LGB ΔQWK=−0.0113 ✅ |
| **H2** | OE2 | ✅ VERIFICADA (Dim-1 comparativa) · ⚠️ readiness operacional PARCIAL (Dim-2) | ONE-SHOT v7: QWK=0.6786 vs baseline 0.6149 (+10.4%); RecAlta=0.4966 bajo floor 0.55 por label drift |
| **H3** | OE3 | ✅ VERIFICADA (2026-10-12) · OE3-D COMPLETO (2026-10-14) | MTTD_ML=8.7 min vs baseline 30.0 min (−71%), Wilcoxon p=1.37×10⁻¹⁴⁸ |

---

## Cómo citar

```
Salazar Vega, E. M. (2026). Predicción de Rendimiento de Conectividad hacia
Proveedores de Nube desde América Latina usando Aprendizaje Automático
(Dataset, modelos y código) [Data set]. Zenodo.
https://doi.org/10.5281/zenodo.23455883
```

Metadatos de citación también disponibles en `CITATION.cff`. El código en este repositorio no tiene DOI propio — el DOI de arriba corresponde al depósito Zenodo, que incluye una copia congelada de este mismo código.

---

## Licencia

Código bajo licencia MIT — ver `LICENSE`. Este mismo código se incluye también, como copia, dentro del depósito Zenodo (dataset + artefactos + código), que se publica bajo CC-BY-4.0 en su conjunto — dual-licensing válido, Edwin es el único titular de derechos de autor del código. La tesis tiene su propia licencia, según normativa UNI (pendiente de confirmar, no cubierta por este repo ni por el depósito Zenodo).
