# MANIFEST — Dataset y Artefactos

Depósito Zenodo `10.5281/zenodo.23455882` (CC-BY-4.0). Checksums listados son **MD5**, calculados por Zenodo al recibir cada archivo. Ningún archivo se zipea — cada uno se sube individual.

## Raw ingestion / consolidado

| Archivo | Contenido | Tamaño | MD5 | Notebook de origen |
|---|---|---:|---|---|
| `te_all_raw_v4.parquet` | Consolidado de los 72 parquets de ingesta cruda (525,019,966 filas) | 3,305,936,291 B (3.1 GB) | `9957627251880a73c481910cd1399c2e` | `01 dataset analisis v10.ipynb` Celda 1 (consolida `00 search load 01-52.ipynb`) |

## Etiquetado

| Archivo | Contenido | Tamaño | MD5 | Notebook de origen |
|---|---|---:|---|---|
| `te_baseline_stats_v4.parquet` | P95/MAD por ruta-métrica, shape (3,105, 6) | 33,419 B | `6d2c0872d1301eb32dc036af434c089e` | `03 etiquetado v4.ipynb` |
| `te_labeled_ventanas_v4.parquet` | 7,628,801 ventanas etiquetadas | 13,564,534 B (13 MB) | `0e755b43ca8ff6f2ee2f73f5a0fdabcb` | `03 etiquetado v4.ipynb` |

## Feature engineering

| Archivo | Contenido | Tamaño | MD5 | Notebook de origen |
|---|---|---:|---|---|
| `te_features_v4.parquet` | Dataset de features v4 (119 features) | 1,859,731,002 B (1.8 GB) | `0a3a7fce060b78129f3fe780ea184f15` | `04 feature_engineering v4.ipynb` |
| `te_features_v5.parquet` | Dataset de features v5 (100 features) | 1,618,797,078 B (1.6 GB) | `2a28f64229ada0bcd64ff9a92bb014eb` | `04 feature_engineering v5.ipynb` |
| `te_features_v6.parquet` | Dataset final de features (100 features, 162 días) | 3,253,625,395 B (3.0 GB) | `6b9b2fd3a856e7c5208ad032692104e9` | `04 feature_engineering v6.ipynb` |

## Modelo

| Archivo | Contenido | Tamaño | MD5 | Notebook de origen |
|---|---|---:|---|---|
| `rf_v51_model.pkl` | Modelo RF v5.1 congelado (candidato D38) | 3,948,418,447 B (3.7 GB) | `3282daeb98663abf3e52f55821be61d8` | `05 modelado v9.ipynb` + `05 modelado rf_retrain v1.ipynb` |
| `lgb_v7_model.txt` | Modelo LightGBM v7 (formato nativo) | 55,889,723 B (53 MB) | `ccd3b16929c6cdd0058429a18f235075` | `05 modelado v7.ipynb` |
| `lgb_v7.onnx` | Modelo LightGBM v7 exportado a ONNX (opset 9) | 38,000,841 B (36 MB) | `2fac826e756df3e1f3ec9643f1a79b0f` | `08 onnx_export v1.ipynb` |
| `scaler_v5.pkl` | StandardScaler ajustado sobre train | 2,824 B | `42b427054064a42cbb47b13368171427` | `05 modelado v7.ipynb` |
| `feature_cols_v5.json` | Orden canónico de las 100 features | 1,748 B | `f9f9ae47fefce01e409c3f89015af76b` | `05 modelado v7.ipynb` |
| `ensemble_d38_manifest.json` | Manifest del ensemble D38 (pesos, checksums) | 3,682 B | `880a550a467b6b6650672084df200801` | `08 onnx_export v1.ipynb` |

## Outputs de evaluación / inferencia

| Archivo | Contenido | Tamaño | MD5 | Notebook de origen |
|---|---|---:|---|---|
| `X_test_rf_v7.npy` | Features de test, formato RF | 454,516,128 B (434 MB) | `22539577e7d9502777ba6d496e07248b` | `06 evaluacion test set v6.ipynb` |
| `X_test_lgb_v7.npy` | Features de test, formato LGB nativo | 454,516,128 B (434 MB) | `5623ece3508ad7f8592ac7770502a3a6` | `06 evaluacion test set v6.ipynb` |
| `y_test.npy` | Labels de test | 7,931,160 B (7.6 MB) | `588cca1bd61b59d2bb7b0b19f5e66b64` | `06 evaluacion test set v6.ipynb` |
| `predictions_d38.parquet` | Predicciones del ensemble D38 sobre test completo | 532,074,682 B (508 MB) | `54df3aa7914d882d1253b1f7357b256f` | `09 dsdl_inference v1.ipynb` |
| `alert_log_d38.parquet` | Log de alertas generadas | 15,211,679 B (15 MB) | `9f6e7c52771000182ca595d37143a230` | `10 splunk_dashboard v1.ipynb` |
| `mttd_results_d38.parquet` | Resultados de MTTD por evento | 106,735 B (104 KB) | `acab12f6e1ca90a5d4ef0b3b8f6084f1` | `10 splunk_dashboard v1.ipynb` |

## Resultados / veredictos

| Archivo | Contenido | Tamaño | MD5 | Notebook de origen |
|---|---|---:|---|---|
| `h3_verdict_d38.json` | Veredicto H3 (MTTD, Wilcoxon) | 537 B | `c692deb93d59feb2eb92ab9429e463b1` | `10 splunk_dashboard v1.ipynb` |
| `latency_report_d38.json` | Reporte de latencia/SLA | 1,118 B | `dce75de9aee50fcb0292f76c4b5d00d1` | `11 splunk_dsdl_apply v1.ipynb` |

## Código

Este depósito incluye además una copia congelada del código curado (28 notebooks + `latam2cloud_monitor_dashboard_v4.xml` + `README.md` + `LICENSE` + `CITATION.cff` + `environment.yml`), idéntica al repositorio público [`latam2cloud`](https://github.com/universidadDescubre/latam2cloud) (ahí bajo licencia MIT propia; como parte de este depósito, bajo CC-BY-4.0).

### Pipeline de notebooks

Recorrido progresivo de los 28 notebooks, en el orden en que se ejecutan. El detalle celda por celda de cada uno vive en su documento de referencia (`DSDL_DATASET_ANALYSIS.md`, `CALIBRATION_ANALYSIS.md`, `LABELING_ANALYSIS.md`, `FEATURE_ENGINEERING_ANALYSIS.md`, `MODELING_ANALYSIS.md`, `DECISION_LOG.md`, `TEST_SET_EVALUATION.md`, `ABLATION_BGP_ANALYSIS.md`, `MLOPS_PIPELINE.md`, `DSDL_APPLY_PROTOTYPE.md`), citado en cada entrada.

**Etapa 0 · Ingesta** — `00 search load 01.ipynb` / `00 search load 52.ipynb`: 2 de 52 notebooks de ingesta incluidos como muestra representativa (primero y último, mismo patrón repetido por semana/proveedor contra el índice `thousandeyes` de Splunk). Outcome conjunto de los 52: 72 parquets crudos, 525,019,966 filas (`DSDL_DATASET_ANALYSIS.md`).

**Etapa 1 · EDA** — `01 dataset analisis v10.ipynb`: consolida los 72 parquets y ejecuta el análisis exploratorio final. Outcome: `te_all_raw_v4.parquet` — 525M filas, 162 días, 621 rutas, ratio BGP 1:45 (`DSDL_DATASET_ANALYSIS.md`).

**Etapa 2 · Calibración** — `02 calibracion v7.ipynb`: barrido y validación de umbrales de severidad sobre las 1,035 rutas completas. Outcome: parámetros congelados BD=7, k=(1.0, 2.0, 3.5), OR≥4/6 — no se re-ejecuta (`CALIBRATION_ANALYSIS.md`).

**Etapa 3 · Etiquetado**
- `03 etiquetado v3.ipynb` — primera ejecución, dataset de 83 días (may–jul 2026), mismos parámetros que v4 pero incompleto. Outcome: histórico, superado por v4 (`DECISION_LOG.md`).
- `03 etiquetado v4.ipynb` — versión final sobre los 162 días completos. Outcome: 7,628,801 ventanas etiquetadas, 1,035 rutas, splits 70/15/15, cobertura test 99.6% (`LABELING_ANALYSIS.md`).

**Etapa 4 · Feature Engineering**
- `04 feature_engineering v4.ipynb` — primera iteración, 83 días. Outcome: 119→113 features tras corrección D12-REV; superada por v5/v6.
- `04 feature_engineering v5.ipynb` — segunda iteración, 83 días. Outcome: 100 features, arquitectura de referencia (la misma que v6).
- `04 feature_engineering v6.ipynb` — versión final, 162 días completos. Outcome: `te_features_v6.parquet` — 100 features ML, 7,628,801 ventanas, train=5,349,594/val=1,140,847/test=1,138,360 (`FEATURE_ENGINEERING_ANALYSIS.md §12`).

**Etapa 5 · Modelado** — búsqueda del candidato final, campaña v4→v15 (`MODELING_ANALYSIS.md`):
- `05 modelado v4.ipynb` — primera campaña (RF, XGBoost, GRU) sobre FE v4. Outcome: RF v4 Full QWK=0.6257, XGBoost v4 QWK=0.6142, GRU (QWK Loss) QWK=0.4996 con colapso de la clase Alta — ninguno alcanza el floor QWK≥0.65 (§1.1).
- `05 modelado v5.ipynb` — incorpora LightGBM y el primer ensemble (D27), sobre FE v5. Outcome: primer ensemble multi-modelo, base para las iteraciones siguientes (§1.2).
- `05 modelado v7.ipynb` — LightGBM v7, versión final del componente LGB. Outcome: modelo candidato del Ensemble D38.
- `05 modelado v8.ipynb` — experimento de threshold tuning (D35/D36). Outcome: `ensemble_v8_threshold` rechazado (§7).
- `05 modelado v9.ipynb` — candidato congelado. Outcome: Ensemble D38 (RF v5.1×0.80 + LGB v7×0.20), QWK=0.6529, RecAlta=0.6509 en validación (LOG-25).
- `05 modelado rf_retrain v1.ipynb` — sanity check de reproducibilidad del RF v5.1 (D41-5a→5c). Outcome: modelo re-entrenado bit-a-bit idéntico al original, max diff=0.000000 (`DECISION_LOG.md`).
- `05 modelado v10.ipynb` (P0) — ventana completa de 162 días. Outcome: Ensemble RF×0.85+LGB×0.15 QWK=0.6227, RecAlta=0.5237 — sin candidato, hipótesis de concept drift (§14).
- `05 modelado v11.ipynb` (P1) — ventana de 83 días, replica el setup de v9. Outcome: QWK=0.6231, RecAlta=0.5254 — sin candidato; refuta la hipótesis de concept drift (Δ=+0.0004 vs v10) (§15).
- `05 modelado v12.ipynb` (P2) — XGBoost sobre FE v6. Outcome: XGBoost solo QWK=0.3684; Ensemble XGB×0.5+RF×0.5 QWK=0.5410 — sin candidato, XGBoost sesgado hacia la clase Alta (§16).
- `05 modelado v13.ipynb` (T1+T2) — escaneo de 46 umbrales + LightGBM v13. Outcome: máximo QWK=0.6231 con RecAlta≥0.55 — sin candidato; búsqueda OE2 declarada cerrada sobre FE v6 (§17).
- `05 modelado v14.ipynb` (R5+R1) — threshold scan sobre probabilidades de v9 (val) + LGB con SMOTE. Outcome: T=0.38 → QWK=0.6504, RecAlta=0.6697 — candidato provisional (§18–§19).
- `05 modelado v15.ipynb` (R2+R3) — corrección de label shift (BBSE) + LGB warm-start. Outcome: R2 BBSE colapsa (QWK=0.0000), R3 warmstart QWK=0.5827 (ΔQWK=−0.0119) — sin candidato nuevo; se confirma como candidato definitivo el Ensemble D38 argmax de v9 (QWK=0.6529, RecAlta=0.6509); búsqueda OE2 cerrada (§20).

**Etapa 6 · Evaluación en test** (`TEST_SET_EVALUATION.md`):
- `06 evaluacion test set v6.ipynb` — confirmación one-shot (idéntica a v5) + guardado de artefactos para exportación ONNX. Outcome: `X_test_rf_v7.npy`, `X_test_lgb_v7.npy`, `y_test.npy` (§29).
- `06 evaluacion test set v7.ipynb` — ONE-SHOT DEFINITIVO sobre el hold-out completo (8–30 sep 2026). Outcome: QWK=0.6786, RecAlta=0.4966 — H2 verificada, dimensión científica comparativa (§30).

**Etapa 7 · Ablación BGP** — `07 ablacion_bgp v2.ipynb`: re-confirmación de H1 con el dataset extendido de 162 días. Outcome: RF ΔQWK=−0.0000, LGB ΔQWK=−0.0113 — H1 re-confirmada (`ABLATION_BGP_ANALYSIS.md §16`).

**Etapa 8 · Exportación ONNX** — `08 onnx_export v1.ipynb`: exporta el componente LightGBM del ensemble a ONNX. Outcome: `lgb_v7.onnx` (opset 9), epsilon máximo 8.49e-08 frente al modelo original, QWK D40=0.6786 confirmado post-exportación (`MLOPS_PIPELINE.md §7`).

**Etapa 9 · Inferencia DSDL** — `09 dsdl_inference v1.ipynb`: aplica el Ensemble D38 vía el pipeline DSDL. Outcome: smoke test QWK=0.6786, SLA<100ms, 1,138,360 ventanas inferidas, `predictions_d38.parquet` (`MLOPS_PIPELINE.md §7.3`).

**Etapa 10 · Dashboard y MTTD** — `10 splunk_dashboard v1.ipynb`: genera alertas y mide el MTTD. Outcome: MTTD_ML=8.7 min vs. baseline=30.0 min (−71%), Wilcoxon p=1.37×10⁻¹⁴⁸ — H3 verificada (`MLOPS_PIPELINE.md`).

**Etapa 11 · DSDL Live Apply** — `11 splunk_dsdl_apply v1.ipynb`: prototipo de scoring en vivo contra Splunk. Outcome: 7 artefactos D40 verificados SHA-256, latencia 63.4 ms/ventana (batch=1), SLA<100ms en todos los batches (`DSDL_APPLY_PROTOTYPE.md`).
