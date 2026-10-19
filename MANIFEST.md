# MANIFEST — Dataset y Artefactos

Depósito Zenodo `10.5281/zenodo.23455882` (CC-BY-4.0). Checksums listados son **MD5**, calculados por Zenodo al recibir cada archivo. Ningún archivo se zipea — cada uno se sube individual. La tabla sigue el orden de ejecución del pipeline (etapas 0 a 11); el detalle celda por celda de cada notebook vive en su documento de referencia (`DSDL_DATASET_ANALYSIS.md`, `CALIBRATION_ANALYSIS.md`, `LABELING_ANALYSIS.md`, `FEATURE_ENGINEERING_ANALYSIS.md`, `MODELING_ANALYSIS.md`, `DECISION_LOG.md`, `TEST_SET_EVALUATION.md`, `ABLATION_BGP_ANALYSIS.md`, `MLOPS_PIPELINE.md`, `DSDL_APPLY_PROTOTYPE.md`).

## Pipeline, notebooks y archivos generados

| Notebook | Etapa | Archivo | Contenido / Resultado | Tamaño | MD5 |
|---|---|---|---|---:|---|
| `00 search load 01.ipynb` / `00 search load 52.ipynb` | 0 · Ingesta | — | 2 de 52 notebooks de ingesta incluidos como muestra representativa (primero y último, mismo patrón repetido por semana/proveedor); generan los 72 parquets crudos consolidados en `te_all_raw_v4.parquet` | — | — |
| `01 dataset analisis v10.ipynb` | 1 · EDA | `te_all_raw_v4.parquet` | Celda 1 — consolidado de los 72 parquets de ingesta cruda (525,019,966 filas), 162 días, 621 rutas | 3,305,936,291 B (3.1 GB) | `9957627251880a73c481910cd1399c2e` |
| `02 calibracion v7.ipynb` | 2 · Calibración | — | Parámetros congelados: BD=7, k=(1.0, 2.0, 3.5), OR≥4/6 — no se re-ejecuta | — | — |
| `03 etiquetado v3.ipynb` | 3 · Etiquetado | — | Histórico, superado por v4 (dataset 83 días, incompleto) | — | — |
| `03 etiquetado v4.ipynb` | 3 · Etiquetado | `te_baseline_stats_v4.parquet` | P95/MAD por ruta-métrica, shape (3,105, 6) | 33,419 B | `6d2c0872d1301eb32dc036af434c089e` |
| `03 etiquetado v4.ipynb` | 3 · Etiquetado | `te_labeled_ventanas_v4.parquet` | 7,628,801 ventanas etiquetadas, 1,035 rutas, splits 70/15/15, cobertura test 99.6% | 13,564,534 B (13 MB) | `0e755b43ca8ff6f2ee2f73f5a0fdabcb` |
| `04 feature_engineering v4.ipynb` | 4 · Features | `te_features_v4.parquet` | 119→113 features (tras corrección D12-REV), 83 días — superado por v5/v6 | 1,859,731,002 B (1.8 GB) | `0a3a7fce060b78129f3fe780ea184f15` |
| `04 feature_engineering v5.ipynb` | 4 · Features | `te_features_v5.parquet` | 100 features, arquitectura de referencia (la misma que v6) | 1,618,797,078 B (1.6 GB) | `2a28f64229ada0bcd64ff9a92bb014eb` |
| `04 feature_engineering v6.ipynb` | 4 · Features | `te_features_v6.parquet` | Dataset final: 100 features, 162 días, 7,628,801 ventanas (train=5,349,594/val=1,140,847/test=1,138,360) | 3,253,625,395 B (3.0 GB) | `6b9b2fd3a856e7c5208ad032692104e9` |
| `05 modelado v4.ipynb` | 5 · Modelado | — | RF v4 Full QWK=0.6257, XGBoost v4 QWK=0.6142, GRU (QWK Loss) QWK=0.4996 (colapso clase Alta) — ninguno alcanza floor QWK≥0.65 | — | — |
| `05 modelado v5.ipynb` | 5 · Modelado | — | Incorpora LightGBM y el primer ensemble (D27), sobre FE v5 | — | — |
| `05 modelado v7.ipynb` | 5 · Modelado | `lgb_v7_model.txt` | LightGBM v7, formato nativo — candidato del Ensemble D38 | 55,889,723 B (53 MB) | `ccd3b16929c6cdd0058429a18f235075` |
| `05 modelado v7.ipynb` | 5 · Modelado | `scaler_v5.pkl` | StandardScaler ajustado sobre train | 2,824 B | `42b427054064a42cbb47b13368171427` |
| `05 modelado v7.ipynb` | 5 · Modelado | `feature_cols_v5.json` | Orden canónico de las 100 features | 1,748 B | `f9f9ae47fefce01e409c3f89015af76b` |
| `05 modelado v8.ipynb` | 5 · Modelado | — | Experimento de threshold tuning (D35/D36) — `ensemble_v8_threshold` rechazado | — | — |
| `05 modelado v9.ipynb` + `05 modelado rf_retrain v1.ipynb` | 5 · Modelado | `rf_v51_model.pkl` | RF v5.1 congelado (candidato D38) — Ensemble D38 (RF×0.80+LGB×0.20) QWK=0.6529, RecAlta=0.6509 en validación (LOG-25); rf_retrain = sanity check de reproducibilidad, bit-a-bit idéntico | 3,948,418,447 B (3.7 GB) | `3282daeb98663abf3e52f55821be61d8` |
| `05 modelado v10.ipynb` | 5 · Modelado (OE2) | — | P0: ventana 162 días completa — Ensemble QWK=0.6227, RecAlta=0.5237 — sin candidato | — | — |
| `05 modelado v11.ipynb` | 5 · Modelado (OE2) | — | P1: ventana 83 días — QWK=0.6231 — sin candidato; refuta hipótesis de concept drift | — | — |
| `05 modelado v12.ipynb` | 5 · Modelado (OE2) | — | P2: XGBoost sobre FE v6 — Ensemble XGB+RF QWK=0.5410 — sin candidato | — | — |
| `05 modelado v13.ipynb` | 5 · Modelado (OE2) | — | T1+T2: escaneo de 46 umbrales + LGB v13 — QWK máx=0.6231 — sin candidato; búsqueda OE2 cerrada sobre FE v6 | — | — |
| `05 modelado v14.ipynb` | 5 · Modelado (OE2) | — | R5+R1: threshold scan + LGB con SMOTE — T=0.38 → QWK=0.6504, RecAlta=0.6697 — candidato provisional | — | — |
| `05 modelado v15.ipynb` | 5 · Modelado (OE2) | — | R2 BBSE colapsa (QWK=0.0000), R3 warmstart QWK=0.5827 — sin candidato nuevo; confirma como definitivo el Ensemble D38 argmax de v9; búsqueda OE2 cerrada | — | — |
| `06 evaluacion test set v6.ipynb` | 6 · Evaluación | `X_test_rf_v7.npy` | Features de test, formato RF | 454,516,128 B (434 MB) | `22539577e7d9502777ba6d496e07248b` |
| `06 evaluacion test set v6.ipynb` | 6 · Evaluación | `X_test_lgb_v7.npy` | Features de test, formato LGB nativo | 454,516,128 B (434 MB) | `5623ece3508ad7f8592ac7770502a3a6` |
| `06 evaluacion test set v6.ipynb` | 6 · Evaluación | `y_test.npy` | Labels de test | 7,931,160 B (7.6 MB) | `588cca1bd61b59d2bb7b0b19f5e66b64` |
| `06 evaluacion test set v7.ipynb` | 6 · Evaluación | — | ONE-SHOT DEFINITIVO sobre el hold-out completo (8–30 sep 2026): QWK=0.6786, RecAlta=0.4966 — H2 verificada | — | — |
| `07 ablacion_bgp v2.ipynb` | 7 · Ablación BGP | — | Re-confirmación de H1 con 162 días: RF ΔQWK=−0.0000, LGB ΔQWK=−0.0113 — H1 re-confirmada | — | — |
| `08 onnx_export v1.ipynb` | 8 · Export ONNX | `lgb_v7.onnx` | LightGBM v7 exportado a ONNX (opset 9); epsilon máx. 8.49e-08 vs. modelo original | 38,000,841 B (36 MB) | `2fac826e756df3e1f3ec9643f1a79b0f` |
| `08 onnx_export v1.ipynb` | 8 · Export ONNX | `ensemble_d38_manifest.json` | Manifest del ensemble D38 (pesos, checksums) | 3,682 B | `880a550a467b6b6650672084df200801` |
| `09 dsdl_inference v1.ipynb` | 9 · Inferencia DSDL | `predictions_d38.parquet` | Predicciones del ensemble D38 sobre test completo — smoke test QWK=0.6786, SLA<100ms, 1,138,360 ventanas | 532,074,682 B (508 MB) | `54df3aa7914d882d1253b1f7357b256f` |
| `10 splunk_dashboard v1.ipynb` | 10 · Dashboard + MTTD | `alert_log_d38.parquet` | Log de alertas generadas | 15,211,679 B (15 MB) | `9f6e7c52771000182ca595d37143a230` |
| `10 splunk_dashboard v1.ipynb` | 10 · Dashboard + MTTD | `mttd_results_d38.parquet` | Resultados de MTTD por evento — MTTD_ML=8.7 min vs. baseline=30.0 min (−71%), Wilcoxon p=1.37×10⁻¹⁴⁸ | 106,735 B (104 KB) | `acab12f6e1ca90a5d4ef0b3b8f6084f1` |
| `10 splunk_dashboard v1.ipynb` | 10 · Dashboard + MTTD | `h3_verdict_d38.json` | Veredicto H3 (MTTD, Wilcoxon) — H3 verificada | 537 B | `c692deb93d59feb2eb92ab9429e463b1` |
| `11 splunk_dsdl_apply v1.ipynb` | 11 · DSDL Live Apply | `latency_report_d38.json` | Reporte de latencia/SLA — 7 artefactos D40 verificados SHA-256, latencia 63.4 ms/ventana (batch=1), SLA<100ms | 1,118 B | `dce75de9aee50fcb0292f76c4b5d00d1` |

## Código

Este depósito incluye además una copia congelada del código curado (28 notebooks — detallados en la tabla de arriba — + `latam2cloud_monitor_dashboard_v4.xml` + `README.md` + `LICENSE` + `CITATION.cff` + `environment.yml`), idéntica al repositorio público [`latam2cloud`](https://github.com/universidadDescubre/latam2cloud) (ahí bajo licencia MIT propia; como parte de este depósito, bajo CC-BY-4.0).
