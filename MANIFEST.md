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

Este depósito incluye además una copia congelada del código curado (29 notebooks + `latam2cloud_monitor_dashboard_v4.xml` + `README.md` + `LICENSE` + `CITATION.cff` + `environment.yml`), idéntica al repositorio público [`latam2cloud`](https://github.com/universidadDescubre/latam2cloud) (ahí bajo licencia MIT propia; como parte de este depósito, bajo CC-BY-4.0).
