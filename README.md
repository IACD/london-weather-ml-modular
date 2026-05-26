# 🌡️ Predicción de Temperatura - Londres (1979-2020)

Proyecto modular de Machine Learning para predicción de temperatura media diaria usando Random Forest.

---

## 📊 Resumen del Proyecto

### 🎯 Resultados Finales

**Modelo de Producción:** Random Forest v1 (Modularizado)  
**RMSE Test:** 2.850°C (IC 95%: 2.775-2.922°C)  
**MAE Test:** 2.264°C  
**R² Test:** 0.747

---

### 🏆 Comparación de Rendimiento de Modelos

| Modelo | Versión | RMSE (Test) | MAE (Test) | R² (Test) | Estado |
|--------|---------|-------------|------------|-----------|--------|
| **Seasonal Naive** | Baseline | 3.690°C | 2.921°C | 0.576 | Baseline |
| **Ridge Regression** | v1 (sin leakage) | 3.681°C | 2.733°C | 0.578 | Benchmark |
| **Random Forest** | v1 (sin leakage) | **2.850°C** | **2.264°C** | **0.747** | **PRODUCCIÓN** ✅ |
| **XGBoost** | v1 (sin leakage) | 2.950°C | 2.336°C | 0.729 | Benchmark |
| Ridge Regression | v2 (con leakage) | 0.922°C | 0.701°C | 0.974 | Upper Bound ⚠️ |
| Random Forest | v2 (con leakage) | 1.060°C | 0.786°C | 0.965 | Upper Bound ⚠️ |
| XGBoost | v2 (con leakage) | 0.886°C | 0.678°C | 0.976 | Upper Bound ⚠️ |

**Mejora sobre Baseline:** 22.8% ↓ RMSE

---

### 🔍 Hallazgos Clave

1. **Selección de Modelo:** Random Forest v1 supera a XGBoost y Ridge manteniendo cero filtración de datos
2. **Validación Temporal:** División cronológica estricta (60% train, 20% val, 20% test) previene contaminación con datos futuros
3. **Importancia de Features:** Variables meteorológicas (presión, radiación, nubosidad) + patrones estacionales impulsan las predicciones
4. **Rendimiento por Estación:** 
   - Mejor: Otoño (RMSE: 2.706°C)
   - Peor: Invierno (RMSE: 3.039°C)
5. **Limitación:** El modelo se degrada con temperaturas extremas (>25°C: RMSE: 6.727°C) debido a ejemplos limitados en entrenamiento
6. **Reproducibilidad:** Intervalos de confianza bootstrap confirman estabilidad del modelo (ancho IC: ±0.147°C)

---

### ⚖️ Trade-off Ético: Data Leakage vs Rendimiento

**Problema:** Incluir `max_temp` y `min_temp` como features crea **filtración temporal** porque:
- Estas variables se miden **el mismo día** que el objetivo (`mean_temp`)
- Tienen correlación >0.95 con el objetivo (≈ hacer trampa)
- El modelo fallaría en despliegue real (máx/mín futuros son desconocidos)

**Nuestra Decisión:**

| Enfoque | RMSE | Estado | Justificación |
|---------|------|--------|---------------|
| **v1 (sin leakage)** | **2.850°C** | ✅ **PRODUCCIÓN** | Reproducible, ético, desplegable |
| v2 (con leakage) | 1.060°C | ❌ Solo comparación | Cota superior teórica, no utilizable |

**Trade-off:** Aceptar **169% más error** para garantizar **cero filtración de datos** y **prácticas éticas de ML**.

Esto se alinea con principios de IA responsable: **transparencia > rendimiento**.

---

### 🛠️ Stack Técnico

**Lenguajes y Librerías:**
- Python 3.x con sklearn 1.8.0
- XGBoost, Pandas, NumPy, Matplotlib, Seaborn

**Arquitectura:**
- Paquete modular `src/` (6 módulos: __init__, data_processing, feature_engineering, models, evaluation, visualization)
- Validación cruzada temporal (sin mezcla aleatoria)
- Intervalos de confianza bootstrap (1000 iteraciones)

**Estrategia de Validación:**
- Baseline: Seasonal Naive (promedios estacionales históricos)
- 3 modelos ML con optimización de hiperparámetros
- Conjunto de validación separado para estimación imparcial del rendimiento

---

### 📁 Archivos Generados

**Modelos:**
- `models/random_forest_v1_modular.pkl` (modelo de producción, 200 árboles)

**Visualizaciones:**
- EDA: distribuciones, correlaciones, series temporales
- División temporal: visualización de splits train/val/test
- Comparación de modelos: predicciones v2, residuos v2, feature importance
- Análisis de errores: por estación, por rango de temperatura
- Bootstrap: intervalos de confianza
- Resumen final: comparación RMSE de todos los modelos

**Resultados (archivos CSV):**
- `results/final_analysis.csv` (tabla comparativa de modelos)
- `results/bootstrap_results.csv` (intervalos de confianza)
- `data/processed/` (divisiones train/val/test y features)

**Código:**
- Paquete `src/`: 5 módulos, ~1200 líneas de Python listo para producción
- Notebook Jupyter: 15 celdas, pipeline completamente reproducible
- `README.md`: Documentación profesional

---

### 🎓 Declaración de Reproducibilidad

Todos los resultados son **100% reproducibles** con:
- Semilla aleatoria fija (`random_state=42`)
- Divisiones de datos deterministas (temporal, sin aleatoriedad)
- Hiperparámetros documentados
- Código versionado

**Para reproducir:**

```bash
git clone https://github.com/IACD/london-weather-ml-modular.git
cd london-weather-ml-modular
pip install -r requirements.txt
jupyter notebook main_notebook.ipynb
# Ejecutar: Cell → Run All
```

---

### 🚀 Uso Rápido

#### Instalación

```bash
git clone https://github.com/IACD/london-weather-ml-modular.git
cd london-weather-ml-modular
pip install -r requirements.txt
```

#### Ejecución

```bash
jupyter notebook main_notebook.ipynb
# En el notebook: Cell → Run All
```

---

### 📚 Referencias

- **Dataset:** Datos Meteorológicos de Londres (1979-2020, 15,305 observaciones diarias)
- **Mejores Prácticas:** Documentación de Scikit-learn, principios MLOps

---

## 👤 Autor

**Ismael Alejandro Carreño Diaz**  
Bioinformático | Candidato a Científico de Datos  
Walmart / Stefanini México

---

## 📋 Información del Proyecto

**Estado del Proyecto:** ✅ **COMPLETO Y LISTO PARA PRODUCCIÓN**  
**Autor:** Ismael Alejandro Carreño Diaz  
**Fecha:** Mayo 2026  
**Contexto:** Walmart/Stefanini - Assessment de Científico de Datos  
**Versión:** 1.0.0  
**Última Actualización:** 25 de Mayo, 2026