# MANIFEST — Dataset y Artefactos

Depósito Zenodo `10.5281/zenodo.23455882` (CC-BY-4.0). Ningún archivo se zipea — cada uno se sube individual. Cada tabla corresponde a una etapa del pipeline, en el orden en que se ejecuta.

## Etapa 0 — Ingesta

| Notebook | Descripción | Resultado | Descripción del resultado |
|---|---|---|---|
| `00 search load 01.ipynb` / `00 search load 52.ipynb` | Descarga la telemetría ThousandEyes desde Splunk, particionada por semana y proveedor de nube (52 notebooks en total; se incluyen 2 como muestra representativa, el primero y el último) | 72 parquets crudos | 525,019,966 filas, 162 días de medición |

## Etapa 1 — Análisis exploratorio

| Notebook | Descripción | Resultado | Descripción del resultado |
|---|---|---|---|
| `01 dataset analisis v10.ipynb` | Consolida los 72 parquets y analiza cobertura, rutas y correlación con eventos BGP | `te_all_raw_v4.parquet` | 525M filas, 162 días, 621 rutas, ratio BGP 1:45 |

## Etapa 2 — Calibración

| Notebook | Descripción | Resultado | Descripción del resultado |
|---|---|---|---|
| `02 calibracion v7.ipynb` | Barrido y validación de umbrales de severidad sobre las 1,035 rutas completas | Parámetros congelados | BD=7, k=(1.0, 2.0, 3.5), OR≥4/6 — no se re-ejecuta |

## Etapa 3 — Etiquetado

| Notebook | Descripción | Resultado | Descripción del resultado |
|---|---|---|---|
| `03 etiquetado v3.ipynb` | Primera ejecución, dataset de 83 días, mismos parámetros que v4 pero incompleto | Histórico | Superado por v4 |
| `03 etiquetado v4.ipynb` | Aplica los umbrales congelados sobre los 162 días completos | `te_baseline_stats_v4.parquet`, `te_labeled_ventanas_v4.parquet` | 7,628,801 ventanas etiquetadas, 1,035 rutas, splits 70/15/15, cobertura test 99.6% |

## Etapa 4 — Ingeniería de características

| Notebook | Descripción | Resultado | Descripción del resultado |
|---|---|---|---|
| `04 feature_engineering v4.ipynb` | Primera iteración, 83 días | `te_features_v4.parquet` | 119→113 características tras corrección — superado por v5/v6 |
| `04 feature_engineering v5.ipynb` | Segunda iteración, 83 días | `te_features_v5.parquet` | 100 características, arquitectura de referencia (la misma que v6) |
| `04 feature_engineering v6.ipynb` | Versión final, 162 días completos | `te_features_v6.parquet` | 100 características, 7,628,801 ventanas (train=5,349,594 / val=1,140,847 / test=1,138,360) |

## Etapa 5 — Modelado

| Notebook | Descripción | Resultado | Descripción del resultado |
|---|---|---|---|
| `05 modelado v4.ipynb` | Primera campaña (Random Forest, XGBoost, GRU) sobre características v4 | Sin candidato | Ningún modelo alcanza el umbral mínimo de desempeño (colapso en la clase Alta) |
| `05 modelado v5.ipynb` | Incorpora LightGBM y el primer ensamble, sobre características v5 | Primer ensamble | Base para las iteraciones siguientes |
| `05 modelado v7.ipynb` | LightGBM v7, versión final del componente LGB | `lgb_v7_model.txt`, `scaler_v5.pkl`, `feature_cols_v5.json` | Modelo candidato del ensamble final |
| `05 modelado v8.ipynb` | Experimento de ajuste de umbral de decisión | Rechazado | No mejora el desempeño frente al candidato vigente |
| `05 modelado v9.ipynb` + `05 modelado rf_retrain v1.ipynb` | Candidato congelado; el segundo notebook verifica su reproducibilidad | `rf_v51_model.pkl` | Ensamble D38 (Random Forest × 0.80 + LightGBM × 0.20); reproducibilidad confirmada de forma idéntica |
| `05 modelado v10.ipynb` | Reentrenamiento sobre la ventana completa de 162 días | Sin candidato | No mejora al candidato vigente |
| `05 modelado v11.ipynb` | Reentrenamiento sobre una ventana de 83 días, replicando el candidato | Sin candidato | Descarta la hipótesis de desplazamiento del concepto como causa de la brecha |
| `05 modelado v12.ipynb` | Prueba de XGBoost sobre características v6 | Sin candidato | Desempeño inferior al candidato vigente |
| `05 modelado v13.ipynb` | Escaneo de umbrales de decisión + reentrenamiento de LightGBM | Sin candidato | Cierra la búsqueda de un modelo superior sobre características v6 |
| `05 modelado v14.ipynb` | Escaneo de umbral sobre las probabilidades del candidato + reentrenamiento con balanceo de clases | Candidato provisional | Mejora el recall de la clase Alta sin perder desempeño general |
| `05 modelado v15.ipynb` | Corrección del desbalance de clases entre entrenamiento y prueba + reentrenamiento incremental | Sin candidato nuevo | Confirma como definitivo el Ensamble D38 de v9; cierra la búsqueda de modelado |

## Etapa 6 — Evaluación

| Notebook | Descripción | Resultado | Descripción del resultado |
|---|---|---|---|
| `06 evaluacion test set v6.ipynb` | Confirmación y guardado de los artefactos de prueba para las etapas siguientes | `X_test_rf_v7.npy`, `X_test_lgb_v7.npy`, `y_test.npy` | Artefactos de prueba congelados |
| `06 evaluacion test set v7.ipynb` | Evaluación definitiva y única (sin reentrenamiento posterior) sobre el conjunto de prueba completo | Hipótesis 2 verificada | Desempeño del modelo significativamente superior al método de referencia |

## Etapa 7 — Ablación

| Notebook | Descripción | Resultado | Descripción del resultado |
|---|---|---|---|
| `07 ablacion_bgp v2.ipynb` | Re-confirma la Hipótesis 1 removiendo las variables de eventos BGP, sobre el dataset extendido de 162 días | Hipótesis 1 re-confirmada | La pérdida de desempeño al remover BGP es mínima en ambos modelos |

## Etapa 8 — Exportación

| Notebook | Descripción | Resultado | Descripción del resultado |
|---|---|---|---|
| `08 onnx_export v1.ipynb` | Exporta el componente LightGBM del ensamble a un formato portable para producción | `lgb_v7.onnx`, `ensemble_d38_manifest.json` | Exportación verificada, sin pérdida de desempeño respecto al modelo original |

## Etapa 9 — Inferencia

| Notebook | Descripción | Resultado | Descripción del resultado |
|---|---|---|---|
| `09 dsdl_inference v1.ipynb` | Aplica el ensamble sobre el conjunto de prueba completo a través de la plataforma de inferencia | `predictions_d38.parquet` | Desempeño confirmado, tiempo de respuesta dentro del límite de servicio acordado |

## Etapa 10 — Monitoreo

| Notebook | Descripción | Resultado | Descripción del resultado |
|---|---|---|---|
| `10 splunk_dashboard v1.ipynb` | Genera alertas y mide el tiempo de detección de degradaciones | `alert_log_d38.parquet`, `mttd_results_d38.parquet`, `h3_verdict_d38.json` | Hipótesis 3 verificada — el tiempo de detección se reduce frente al método de referencia |

## Etapa 11 — Aplicación en producción

| Notebook | Descripción | Resultado | Descripción del resultado |
|---|---|---|---|
| `11 splunk_dsdl_apply v1.ipynb` | Prototipo de evaluación en vivo contra el sistema en producción | `latency_report_d38.json` | Tiempo de respuesta dentro del límite de servicio acordado |

## Código

Este depósito incluye además una copia congelada del código curado (28 notebooks — detallados en las tablas de arriba — + `latam2cloud_monitor_dashboard_v4.xml` + `README.md` + `LICENSE` + `CITATION.cff` + `environment.yml`), idéntica al repositorio público [`latam2cloud`](https://github.com/universidadDescubre/latam2cloud) (ahí bajo licencia MIT propia; como parte de este depósito, bajo CC-BY-4.0).
