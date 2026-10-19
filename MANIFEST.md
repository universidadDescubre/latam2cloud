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

### Notebooks incluidos

| Notebook | Etapa | Rol | Referencia |
|---|---|---|---|
| `00 search load 01.ipynb` / `00 search load 52.ipynb` | 0 · Ingesta | 2 de 52 notebooks de ingesta incluidos como muestra representativa (primero y último, mismo patrón); generan los 72 parquets crudos consolidados en `te_all_raw_v4.parquet` | `DSDL_DATASET_ANALYSIS.md` |
| `01 dataset analisis v10.ipynb` | 1 · EDA | Consolidación + análisis exploratorio final — 525M filas, 621 rutas | `DSDL_DATASET_ANALYSIS.md` |
| `02 calibracion v7.ipynb` | 2 · Calibración | Parámetros congelados: BD=7, k=(1.0,2.0,3.5), OR≥4/6 | `CALIBRATION_ANALYSIS.md` |
| `03 etiquetado v3.ipynb` | 3 · Etiquetado | Histórico, superado por v4 (dataset 83 días, incompleto) | `DECISION_LOG.md` |
| `03 etiquetado v4.ipynb` | 3 · Etiquetado | Versión final — 7,628,801 ventanas etiquetadas | `LABELING_ANALYSIS.md` |
| `04 feature_engineering v4.ipynb` | 4 · Features | Iteración temprana — 113 features, 83 días | `FEATURE_ENGINEERING_ANALYSIS.md` |
| `04 feature_engineering v5.ipynb` | 4 · Features | Iteración intermedia — 100 features, arquitectura de referencia | `FEATURE_ENGINEERING_ANALYSIS.md` |
| `04 feature_engineering v6.ipynb` | 4 · Features | Versión final — 100 features, 162 días | `FEATURE_ENGINEERING_ANALYSIS.md §12` |
| `05 modelado v4.ipynb` | 5 · Modelado | Iteración temprana (RF/XGB/GRU v4, FE v4) | `MODELING_ANALYSIS.md §1.1` |
| `05 modelado v5.ipynb` | 5 · Modelado | Iteración temprana (RF/XGB/LGB v5, primer ensemble) | `MODELING_ANALYSIS.md §1.2` |
| `05 modelado v7.ipynb` | 5 · Modelado | LightGBM v7 final — candidato del Ensemble D38 | `MODELING_ANALYSIS.md` |
| `05 modelado v8.ipynb` | 5 · Modelado | Threshold tuning D35/D36 — rechazado | `MODELING_ANALYSIS.md §7` |
| `05 modelado v9.ipynb` | 5 · Modelado | Candidato congelado — Ensemble D38 (RF v5.1×0.80 + LGB v7×0.20) | `MODELING_ANALYSIS.md` |
| `05 modelado rf_retrain v1.ipynb` | 5 · Modelado | Sanity check de reproducibilidad de RF v5.1 (bit-a-bit idéntico) | `DECISION_LOG.md` |
| `05 modelado v10.ipynb` | 5 · Modelado (búsqueda OE2) | P0: ventana 162 días completa — sin candidato | `MODELING_ANALYSIS.md §14` |
| `05 modelado v11.ipynb` | 5 · Modelado (búsqueda OE2) | P1: ventana 83 días — sin candidato, hipótesis concept drift refutada | `MODELING_ANALYSIS.md §15` |
| `05 modelado v12.ipynb` | 5 · Modelado (búsqueda OE2) | P2: XGBoost sobre v6 — sin candidato | `MODELING_ANALYSIS.md §16` |
| `05 modelado v13.ipynb` | 5 · Modelado (búsqueda OE2) | T1+T2: threshold scan + LGB v13 — sin candidato | `MODELING_ANALYSIS.md §17` |
| `05 modelado v14.ipynb` | 5 · Modelado (búsqueda OE2) | R5+R1: candidato provisional T=0.38 | `MODELING_ANALYSIS.md §18–§19` |
| `05 modelado v15.ipynb` | 5 · Modelado (búsqueda OE2) | R2 BBSE + R3 warmstart — sin candidato nuevo, búsqueda OE2 cerrada | `MODELING_ANALYSIS.md §20` |
| `06 evaluacion test set v6.ipynb` | 6 · Evaluación | Confirmación one-shot + guarda artefactos de test (`X_test_*.npy`, `y_test.npy`) | `TEST_SET_EVALUATION.md §29` |
| `06 evaluacion test set v7.ipynb` | 6 · Evaluación | ONE-SHOT DEFINITIVO — QWK=0.6786, RecAlta=0.4966, veredicto H2 | `TEST_SET_EVALUATION.md §30` |
| `07 ablacion_bgp v2.ipynb` | 7 · Ablación BGP | Re-confirmación H1 con 162 días — H1 verificada | `ABLATION_BGP_ANALYSIS.md §16` |
| `08 onnx_export v1.ipynb` | 8 · Export ONNX | `lgb_v7.onnx` + manifest del ensemble D40 | `MLOPS_PIPELINE.md §7` |
| `09 dsdl_inference v1.ipynb` | 9 · Inferencia DSDL | Pipeline D38 `apply()`, predicciones sobre test completo | `MLOPS_PIPELINE.md §7.3` |
| `10 splunk_dashboard v1.ipynb` | 10 · Dashboard + MTTD | Alertas, MTTD_ML vs. baseline, veredicto H3 | `MLOPS_PIPELINE.md` |
| `11 splunk_dsdl_apply v1.ipynb` | 11 · DSDL Live Apply | Prototipo de scoring en vivo, benchmark de latencia | `DSDL_APPLY_PROTOTYPE.md` |
