# Predicción de Rendimiento de Conectividad hacia Proveedores de Nube desde América Latina usando Aprendizaje Automático

**Autor:** Edwin Martin Salazar Vega
**Universidad:** Universidad Nacional de Ingeniería
**Unidad:** Unidad de Posgrado — Facultad de Ingeniería Industrial e Ingeniería de Sistemas
**Grado:** Maestro en Inteligencia Artificial
**Asesor:** Glen Dario Rodriguez Rafael
**Lima, Perú · 2026**

---

## Descripción

Código y notebooks del proyecto de investigación de maestría cuyo objetivo es predecir degradaciones de conectividad entre nodos de América Latina y proveedores de nube (AWS, GCP, Azure) usando aprendizaje automático supervisado. Los modelos entrenados (Random Forest, LightGBM) operan sobre telemetría de red recolectada con ThousandEyes (OpenTelemetry v2) e integran el stack Splunk DSDL para su despliegue.

---

## Datos y Artefactos

[![DOI](https://zenodo.org/badge/doi/10.5281%2Fzenodo.23455882.svg)](https://doi.org/10.5281/zenodo.23455882)

El dataset completo, los pesos de modelo, los resultados de evaluación y una copia de este código están publicados en un único depósito Zenodo (CC-BY-4.0): [`10.5281/zenodo.23455882`](https://doi.org/10.5281/zenodo.23455882) (concept DOI, siempre resuelve a la última versión publicada) — detalle de cada archivo en `MANIFEST.md` dentro del depósito.

Para ejecutar los notebooks con datos reales: descarga los archivos que necesites del DOI y colócalos en una carpeta local. Cada notebook documenta en su primera celda qué archivo(s) espera.

---

## Pipeline de Investigación

| Etapa | Notebook | Resultado | Estado |
|---|---|---|---|
| 0 · Ingesta | `00 search load 01/52.ipynb` | 72 parquets · 525M filas · 162 días | ✅ |
| 1 · EDA | `01 dataset analisis v10.ipynb` | 1,035 rutas · distribución de clases | ✅ |
| 2 · Calibración | `02 calibracion v7.ipynb` | BD=7, k=(1.0,2.0,3.5) — congelado | ✅ |
| 3 · Etiquetado | `03 etiquetado v3/v4.ipynb` | 7,628,801 ventanas · splits 70/15/15 | ✅ |
| 4 · Features | `04 feature_engineering v4/v5/v6.ipynb` | 100 features · `te_features_v6.parquet` | ✅ |
| 5 · Modelado | `05 modelado v4-v15 + rf_retrain v1.ipynb` | Ensemble D38 (RF v5.1 × 0.80 + LGB v7 × 0.20) | ✅ |
| 6 · Eval. test | `06 evaluacion test set v6/v7.ipynb` | ONE-SHOT: QWK=0.6786 · RecAlta=0.4966 | ✅ |
| 7 · Ablación BGP | `07 ablacion_bgp v2.ipynb` | H1 re-confirmada, 162 días | ✅ |
| 8 · ONNX export | `08 onnx_export v1.ipynb` | `lgb_v7.onnx` + script de inferencia | ✅ |
| 9 · Inferencia DSDL | `09 dsdl_inference v1.ipynb` | QWK=0.6786 · SLA<100ms | ✅ |
| 10 · Dashboard | `10 splunk_dashboard v1.ipynb` | MTTD_ML=8.7 min vs baseline 30.0 min (−71%) · H3 | ✅ |
| 11 · DSDL Live Apply | `11 splunk_dsdl_apply v1.ipynb` + `latam2cloud_monitor_dashboard_v4.xml` | Dashboard v4 en producción | ✅ |

---

## Estado de Hipótesis

| Hipótesis | Estado | Resultado clave |
|---|---|---|
| **H1** | ✅ VERIFICADA | Ablación BGP v2: RF ΔQWK=−0.0000, LGB ΔQWK=−0.0113 |
| **H2** | ✅ VERIFICADA | QWK=0.6786 vs baseline 0.6149 (+10.4%); RecAlta=0.4966 bajo floor 0.55 |
| **H3** | ✅ VERIFICADA | MTTD_ML=8.7 min vs baseline 30.0 min (−71%), Wilcoxon p=1.37×10⁻¹⁴⁸ |

---

## Cómo citar

```
Salazar Vega, E. M. (2026). Predicción de Rendimiento de Conectividad hacia
Proveedores de Nube desde América Latina usando Aprendizaje Automático
(Dataset, modelos y código) [Data set]. Zenodo.
https://doi.org/10.5281/zenodo.23455882
```

Metadatos también disponibles en `CITATION.cff`.

---

## Licencia

Código bajo licencia MIT — ver `LICENSE`. El mismo código se incluye también dentro del depósito Zenodo (dataset + artefactos + código), publicado como conjunto bajo CC-BY-4.0.
