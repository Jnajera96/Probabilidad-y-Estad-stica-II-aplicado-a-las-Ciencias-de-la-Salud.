# 📚 GUÍA DE ESTUDIO COMPLETA Y DEFENSA TÉCNICA

## Optimización Asintótica en la Predicción del Mercado de Memoria RAM

> **Proyecto académico** · Universidad de Guadalajara · Mayo 2026
> **Repositorio:** https://github.com/Jnajera96/ram-pricing-2026
> **Materias evaluadas:** Asymptotic Notation · Bayesian Inference · Machine Learning · Bases de Datos · Programación
> **Equipo:** José Carmen Najera Ortiz · Bernardo Maciel · Juan Pablo Cruz · Diego De Jesús

---

## 🎯 Cómo usar esta guía

Este documento es la **biblia de estudio** del proyecto. Está diseñado para que **cualquier integrante** del equipo pueda defender el proyecto completo frente a los 4 evaluadores con dominio real.

**Estrategia de estudio recomendada:**

1. **Lectura 1 (comprensión):** lee de principio a fin sin memorizar. Entiende el "porqué" de cada decisión.
2. **Lectura 2 (tu sección):** profundiza en la parte que te toca presentar.
3. **Lectura 3 (Q&A):** practica las preguntas anticipadas en voz alta.
4. **Repaso final:** memoriza el "cheat sheet de números" del final.

**Regla de oro de la defensa:** No memorices frases textuales. Entiende los conceptos tan bien que puedas explicarlos con tus propias palabras a un niño de 12 años. Si puedes hacer eso, puedes responder cualquier pregunta del jurado.

---

## 📑 ÍNDICE

1. [Visión integral del proyecto](#1-visión-integral-del-proyecto)
2. [Arquitectura del repositorio y pipeline de datos](#2-arquitectura-del-repositorio-y-pipeline-de-datos)
3. [Stack tecnológico explicado](#3-stack-tecnológico-explicado)
4. [Métricas de evaluación (MAPE, MAE, MSE, RMSE, R²)](#4-métricas-de-evaluación)
5. [Los 5 modelos de Machine Learning](#5-los-5-modelos-de-machine-learning)
6. [Complejidad computacional y análisis asintótico](#6-complejidad-computacional-y-análisis-asintótico)
7. [Bases de datos: SQLite a fondo](#7-bases-de-datos-sqlite-a-fondo)
8. [Estadística inferencial: ANOVA, p-value, t-test](#8-estadística-inferencial)
9. [Análisis Exploratorio de Datos (EDA)](#9-análisis-exploratorio-de-datos-eda)
10. [Programación y arquitectura de software](#10-programación-y-arquitectura-de-software)
11. [Preguntas y respuestas anticipadas del jurado](#11-preguntas-y-respuestas-anticipadas)
12. [Errores, malas prácticas y oportunidades de mejora](#12-errores-y-oportunidades-de-mejora)
13. [Resumen ejecutivo y conclusiones](#13-resumen-ejecutivo-y-conclusiones)
14. [Cheat sheet de números clave](#14-cheat-sheet-de-números-clave)

---

# 1. VISIÓN INTEGRAL DEL PROYECTO

## 1.1 El problema que resuelve

El mercado de memoria RAM es **opaco para el comprador promedio**. Un módulo de 32 GB puede costar $80 o $400 dependiendo de variables técnicas que el consumidor no sabe ponderar: generación DDR, frecuencia en MHz, latencia CAS, número de módulos, presencia de RGB, y marca. No existe una herramienta accesible que responda con rigor: **"¿cuánto *debería* costar una RAM con estas especificaciones?"** y **"¿qué variable influye más en el precio?"**

El proyecto resuelve esto construyendo un **pipeline completo de Ciencia de Datos** que:
- Extrae datos reales del mercado (web scraping de Newegg.com).
- Los limpia, valida y estructura en una base de datos.
- Aplica estadística inferencial para identificar qué factores son significativos.
- Entrena 5 modelos predictivos comparativos para estimar precios.
- Mide empíricamente la complejidad computacional de cada etapa del pipeline.

## 1.2 La pregunta de investigación

> **¿Qué variables técnicas de un módulo de RAM son los predictores más fuertes del precio de mercado, y cómo se comporta la complejidad computacional del pipeline a medida que el volumen de datos escala?**

Esta pregunta tiene dos mitades que corresponden a las dos materias núcleo:
- La primera mitad (**¿qué predice el precio?**) es **Machine Learning + Inferencia Bayesiana/estadística**.
- La segunda mitad (**¿cómo escala?**) es **Asymptotic Notation** (notación asintótica / complejidad computacional).

## 1.3 Objetivo general

Desarrollar y validar un sistema integral de análisis predictivo del precio de memorias RAM que integre seis paradigmas de Ciencia de Datos (web scraping, bases de datos con análisis de complejidad, estadística inferencial, regresión paramétrica, clustering no supervisado y ensembles de árboles), demostrando empíricamente tanto el poder predictivo de cada modelo como su comportamiento asintótico.

## 1.4 Objetivos específicos

1. **Extraer** un dataset representativo del mercado real de RAM mediante web scraping robusto y reproducible.
2. **Construir** un pipeline ETL (Extract–Transform–Load) que limpie, valide e impute datos faltantes con criterio estadístico.
3. **Diseñar** una base de datos SQLite con índices B-Tree y medir empíricamente el speedup que producen.
4. **Aplicar** estadística inferencial (t-test, ANOVA, Tukey HSD) para cuantificar qué factores explican el precio.
5. **Entrenar y comparar** cinco modelos de Machine Learning bajo condiciones idénticas (mismo split, mismas features, mismas métricas).
6. **Medir empíricamente** la complejidad temporal de cada modelo escalando el dataset y comparar con la teoría asintótica.
7. **Comunicar** los hallazgos mediante un repositorio profesional, un póster académico y una defensa oral.

## 1.5 Contexto del mercado de memoria RAM

La RAM (Random Access Memory) es la memoria de trabajo de cualquier computadora. Las características que definen su valor:

- **Generación DDR (Double Data Rate):** DDR3 (legado, 2007), DDR4 (estándar maduro, 2014), DDR5 (actual de gama alta, 2020). Cada generación duplica aproximadamente el ancho de banda. **Es el factor más caro.**
- **Capacidad (GB):** cuánta memoria almacena. Va de 8 GB a 128 GB en kits de consumo.
- **Frecuencia (MHz):** velocidad de transferencia. DDR5 va de 4800 a 8000+ MHz.
- **Latencia CAS (Column Address Strobe):** ciclos de reloj que tarda en responder. Menor es mejor. Es un número técnico que viene embebido en el SKU del producto.
- **Número de módulos (sticks):** un kit puede ser 1×32 GB o 2×16 GB. Afecta el rendimiento (dual-channel).
- **RGB:** iluminación. Es puramente estético pero algunas marcas lo cobran.
- **Marca:** CORSAIR, G.SKILL, Kingston, Crucial, Team, y "Other".

**Dato de contexto para la defensa:** en 2025–2026 el mercado vive una transición de DDR4 a DDR5. Nuestro dataset captura ese momento: 65.4% DDR5, 31.4% DDR4, 3.2% DDR3.

## 1.6 Importancia del análisis predictivo

Un modelo que predice el precio con un error porcentual del 8% tiene aplicaciones reales:

- **Para el consumidor:** detectar si una oferta está sobrevalorada o es un buen precio.
- **Para un retailer:** fijar precios competitivos basados en datos, no en intuición.
- **Para un analista:** entender la estructura del mercado (qué se paga y por qué).

Pero el valor académico va más allá del modelo: el proyecto demuestra **metodología**. Cómo se evalúa un modelo con rigor, cómo se comparan paradigmas distintos de forma justa, y cómo se conecta la teoría (complejidad asintótica) con la práctica (mediciones reales).

## 1.7 Justificación tecnológica y científica

**¿Por qué este stack y no otro?**

- **Python** porque es el lenguaje estándar de Ciencia de Datos: ecosistema maduro (pandas, scikit-learn, scipy) y legible.
- **SQLite** porque el dataset es de tamaño medio (cientos a miles de filas), no necesita un servidor, y permite demostrar índices B-Tree sin infraestructura.
- **5 modelos en lugar de 1** porque la **triangulación metodológica** —que paradigmas independientes lleguen a la misma conclusión— es la validación más robusta posible. Si OLS, Ridge, K-Means, Random Forest y Gradient Boosting **todos** señalan a `capacity_gb` como predictor #1, esa conclusión es casi incontestable.
- **Medición empírica de complejidad** porque la materia de Asymptotic Notation exige demostrar que entiendes la diferencia entre la teoría 𝒪(...) y el comportamiento real de un algoritmo en hardware real.

**El argumento científico central:** el proyecto no se conforma con "entrenar un modelo que funcione". Se conforma con **entender por qué funciona, cuál funciona mejor para qué objetivo, y cómo se comporta cuando los datos crecen.**

---

# 2. ARQUITECTURA DEL REPOSITORIO Y PIPELINE DE DATOS

## 2.1 Filosofía de la estructura

El repositorio está organizado en **carpetas numeradas que reflejan el flujo del pipeline**. Esto no es decorativo: cualquier persona que abre el repo puede leer las carpetas en orden y entender el proyecto sin documentación adicional. La estructura *es* la narrativa.

```
ram-pricing-2026/
├── README.md              ← Portada con badges, diagrama Mermaid, resultados
├── ROADMAP.md             ← Planificación de los 12 días de sprint
├── CHANGELOG.md           ← Historial de cambios (en español)
├── LICENSE                ← MIT License
├── requirements.txt       ← Dependencias de Python
├── .gitignore             ← Exclusiones de Git
├── migrate.py             ← Script de migración del repo (con --dry-run, --revert)
│
├── src/
│   ├── 01_scraping/       ← Extracción de datos
│   │   ├── scraper.py             ← Scraper principal de Newegg
│   │   ├── recalcular_cas.py      ← Re-aplica el parser de CAS sin re-scrapear
│   │   └── web_scraping.py        ← Script inicial de exploración
│   │
│   ├── 02_data_processing/    ← Limpieza y EDA
│   │   ├── clean.py               ← Pipeline completo de limpieza
│   │   └── eda.py                 ← Análisis exploratorio + figuras
│   │
│   ├── 03_database/           ← Base de datos y complejidad SQL
│   │   ├── create_db.py           ← Construye ram_market.db
│   │   ├── create_indices.py      ← Crea 3 índices B-Tree
│   │   ├── benchmark_pre.py       ← Benchmark SIN índices
│   │   ├── benchmark_post.py      ← Benchmark CON índices
│   │   └── benchmark_scaling.py   ← Test de escalado n ∈ [350, 50000]
│   │
│   ├── 04_inference/          ← Estadística inferencial
│   │   ├── normality_tests.py     ← Shapiro-Wilk, validación de supuestos
│   │   ├── ttest_ddr.py           ← Test t: DDR4 vs DDR5
│   │   ├── anova_ddr.py           ← ANOVA + Tukey por generación DDR
│   │   ├── anova_brand.py         ← ANOVA + Tukey por marca
│   │   └── dashboard.py           ← Dashboard inferencial consolidado
│   │
│   ├── 05_models/             ← Los 5 modelos predictivos
│   │   ├── ols.py                 ← Modelo 1: Regresión Lineal (OLS)
│   │   ├── kmeans.py              ← Modelo 2: K-Means clustering
│   │   ├── ridge.py               ← Modelo 3: Ridge Regression
│   │   ├── random_forest.py       ← Modelo 4: Random Forest
│   │   └── gradient_boosting.py   ← Modelo 5: Gradient Boosting
│   │
│   └── 06_analysis/           ← Análisis transversal
│       ├── ml_complexity.py       ← Complejidad empírica de los 5 modelos
│       └── final_dashboard.py     ← Dashboard maestro del proyecto
│
├── docs/                  ← Documentación técnica
│   ├── methodology.md         ← Metodología detallada
│   ├── architecture.md        ← Arquitectura del sistema
│   ├── results.md             ← Resultados completos
│   ├── models_comparison.md   ← Comparativa de modelos
│   └── complexity_analysis.md ← Análisis de complejidad
│
├── tests/                 ← Tests unitarios (parsers, paginación)
├── archive/               ← Scripts diagnósticos históricos
├── data/                  ← CSVs y base de datos SQLite
├── figures/               ← 26 visualizaciones a 300 DPI
└── scripts/               ← Carpeta reservada
```

## 2.2 El pipeline ETL explicado

**ETL** significa **Extract, Transform, Load**. Es el patrón estándar de procesamiento de datos. Así se mapea a nuestro proyecto:

### EXTRACT (Extracción) — `src/01_scraping/`

El scraper consulta páginas de listado de Newegg.com, descarga el HTML, y extrae con parsers de expresiones regulares los campos de cada producto: nombre, precio, capacidad, DDR, frecuencia, latencia CAS, etc.

**Salida:** `data/ram_raw.csv` — 359 productos crudos.

### TRANSFORM (Transformación) — `src/02_data_processing/`

El pipeline de limpieza:
- Elimina duplicados y filas corruptas.
- Convierte tipos de datos (precio de texto a `float`, etc.).
- Aplica **feature engineering**: crea variables derivadas como `log_price`, `ddr_group`, dummies de marca.
- **Imputa** los valores faltantes de latencia CAS usando la mediana condicional por generación DDR (un prior bayesiano informativo).
- Aplica la transformación logarítmica al precio para normalizar su distribución.

**Salida:** `data/ram_clean.csv` — 350 productos limpios, 17 columnas.

### LOAD (Carga) — `src/03_database/`

Los datos limpios se cargan en una base de datos SQLite (`ram_market.db`). Se crean índices B-Tree sobre las columnas más consultadas. A partir de aquí, los análisis consultan la base de datos, no el CSV.

**Salida:** `data/ram_market.db` — base de datos consultable.

### Análisis posteriores

Una vez los datos están en la base de datos:
- **`04_inference/`** corre los tests estadísticos.
- **`05_models/`** entrena los 5 modelos.
- **`06_analysis/`** mide la complejidad y genera el dashboard final.

## 2.3 Diagrama de flujo del proyecto completo

```
   NEWEGG.COM
       │
       │  scraper.py  (requests + BeautifulSoup + regex)
       ▼
   ram_raw.csv  (359 productos crudos)
       │
       │  clean.py  (limpieza + feature engineering + imputación)
       ▼
   ram_clean.csv  (350 productos, 17 features)
       │
       │  create_db.py + create_indices.py
       ▼
   ram_market.db  ◄──────────────────────────┐
       │                                      │
       ├──► benchmark_*.py  → complejidad SQL  │
       │                                      │
       ├──► 04_inference/  → t-test, ANOVA ────┤  Todos consultan
       │                                      │  la misma BD
       ├──► 05_models/  → 5 modelos ML ────────┤
       │                                      │
       └──► 06_analysis/  → complejidad ML ────┘
                │
                ▼
       figures/  +  docs/  +  README
                │
                ▼
       PÓSTER + DEFENSA ORAL
```

## 2.4 Integración entre componentes

El punto clave de la arquitectura es que **cada etapa produce un artefacto que la siguiente consume**, sin acoplamiento fuerte:

- El scraper no sabe nada de modelos. Solo produce un CSV.
- Los modelos no saben nada del scraper. Solo consumen la base de datos.
- Esto significa que puedes **re-ejecutar cualquier etapa de forma independiente** sin tocar las demás.

Esta es la propiedad de **separación de responsabilidades** (separation of concerns), un principio fundamental de ingeniería de software. Cada módulo tiene una única razón para cambiar.

**Frase para defensa:** *"El pipeline está desacoplado por artefactos: cada etapa lee un archivo y escribe otro. Esto permite reproducibilidad parcial —puedo re-entrenar los modelos sin volver a scrapear— y hace el sistema escalable y mantenible."*

---

# 3. STACK TECNOLÓGICO EXPLICADO

Para cada tecnología: qué es, por qué se usó, y cómo defenderla.

## 3.1 Python

**Qué es:** lenguaje de programación interpretado, de alto nivel, multiparadigma. Es el lenguaje dominante en Ciencia de Datos.

**Por qué se usó:** ecosistema científico maduro e integrado. Una sola sintaxis cubre scraping, álgebra lineal, estadística, ML y visualización. Es legible, lo que facilita el trabajo en equipo y la auditoría del código.

**Versión del proyecto:** Python 3.14.

**Cómo defenderlo:** *"Python no es el más rápido, pero su ecosistema (pandas, scikit-learn, scipy) está tan integrado que el costo de desarrollo es mínimo. Para un proyecto de análisis donde el cuello de botella es el pensamiento humano, no el CPU, Python es la elección correcta."*

## 3.2 requests + BeautifulSoup4 + lxml

**Qué son:** la pila de web scraping.
- `requests`: hace peticiones HTTP (descarga el HTML de una página).
- `BeautifulSoup4`: parsea el HTML en un árbol navegable.
- `lxml`: el motor de parseo subyacente, rápido y tolerante a HTML mal formado.

**Por qué se usaron:** son el estándar para scraping estático. Newegg sirve HTML renderizado en el servidor, así que no se necesitó un navegador headless (Selenium/Playwright), lo que mantiene el scraper ligero y rápido.

**Anécdota técnica defendible (el bug de Brotli):** `requests` no descomprime el formato de compresión Brotli sin un paquete adicional. Newegg respondía con un blob corrupto. La solución fue forzar `gzip` removiendo `br` del header `Accept-Encoding`. Esto demuestra debugging real de red.

## 3.3 pandas

**Qué es:** librería de manipulación de datos tabulares. Su estructura central es el `DataFrame`, una tabla en memoria con operaciones vectorizadas.

**Por qué se usó:** todo el pipeline de limpieza, feature engineering y preparación de datos para los modelos se hace en pandas. Es el "Excel programable" de Python.

**Operaciones clave usadas:** `read_csv`, `get_dummies` (codificación one-hot de variables categóricas), `apply` (transformaciones por fila), filtrado booleano, `merge`, `pivot`.

## 3.4 NumPy

**Qué es:** librería de cómputo numérico. Provee el array N-dimensional y operaciones de álgebra lineal vectorizadas en C.

**Por qué se usó:** es la base sobre la que se construyen pandas y scikit-learn. En el proyecto se usó directamente para:
- El OLS matricial manual (`np.linalg.pinv` — pseudoinversa de Moore-Penrose).
- Cálculo de métricas de error.
- La transformación logarítmica (`np.log`, `np.exp`).
- El ajuste log-log de las pendientes de complejidad (`np.polyfit`).

## 3.5 scikit-learn

**Qué es:** la librería de Machine Learning de referencia en Python. API uniforme: todo modelo tiene `.fit()` y `.predict()`.

**Por qué se usó:** provee 4 de los 5 modelos (Ridge, K-Means, Random Forest, Gradient Boosting), más las herramientas de evaluación: `train_test_split`, `GridSearchCV`, `cross_val_score`, métricas (`r2_score`, `mean_squared_error`, etc.), y preprocesamiento (`StandardScaler`, `PCA`).

**Componentes clave defendibles:**
- `GridSearchCV`: búsqueda exhaustiva de hiperparámetros con validación cruzada.
- `StandardScaler`: estandarización (media 0, desviación 1), obligatoria para K-Means y Ridge.
- `random_state=42`: semilla fija en todo el proyecto para reproducibilidad.

## 3.6 SciPy (scipy.stats)

**Qué es:** librería de cómputo científico. El submódulo `scipy.stats` provee toda la estadística inferencial.

**Por qué se usó:** todos los tests del día de inferencia: Shapiro-Wilk (normalidad), Levene (homocedasticidad), `ttest_ind` (test t de Welch), `f_oneway` (ANOVA), Tukey HSD (post-hoc), Mann-Whitney U y Kruskal-Wallis (alternativas no paramétricas).

## 3.7 Matplotlib + Seaborn

**Qué son:** las librerías de visualización.
- `matplotlib`: control de bajo nivel sobre cada elemento del gráfico.
- `seaborn`: capa de alto nivel sobre matplotlib, con estilos estadísticos predefinidos.

**Por qué se usaron:** generaron las 26 figuras del proyecto a 300 DPI (calidad de impresión para el póster). Gráficos de dispersión, boxplots, heatmaps de correlación, QQ-plots, gráficas log-log, tablas formateadas.

## 3.8 SQLite

**Qué es:** motor de base de datos relacional **embebido** (sin servidor). La base de datos completa es un solo archivo.

**Por qué se usó:** se cubre en detalle en la sección 7. En resumen: tamaño de dataset apropiado, cero infraestructura, y permite demostrar índices B-Tree de forma autocontenida.

## 3.9 SQL

**Qué es:** Structured Query Language. El lenguaje declarativo para consultar bases de datos relacionales.

**Por qué se usó:** las queries analíticas (agregaciones por marca, por DDR, filtros de rango de precio) y, crucialmente, el **benchmark de complejidad** que compara el tiempo de ejecución de queries con y sin índices.

## 3.10 Git + GitHub

**Qué son:**
- `Git`: sistema de control de versiones distribuido.
- `GitHub`: plataforma de hospedaje de repositorios Git.

**Por qué se usaron:** versionado de todo el código, historial de cambios con commits narrativos, y el repositorio como **portafolio profesional**. El proyecto tiene 12+ commits con mensajes multipárrafo que cuentan la historia técnica de cada día.

## 3.11 Sobre Jupyter Notebook

**Nota honesta para la defensa:** el proyecto **no usó Jupyter Notebook** como herramienta principal. Se optó por **scripts `.py` modulares y ejecutables**. Esta es una decisión de diseño defendible:

*"Elegimos scripts sobre notebooks porque los scripts son reproducibles de forma determinista (se ejecutan de arriba a abajo sin estado oculto), versionan limpio en Git (un notebook es un JSON difícil de diferenciar), y son la forma en que el código corre en producción. Los notebooks son excelentes para exploración; los scripts son mejores para un pipeline de ingeniería."*

---

# 4. MÉTRICAS DE EVALUACIÓN

Esta sección es **crítica**. El jurado va a preguntar por las métricas. Para cada una: definición, fórmula, interpretación, ventajas, desventajas, cuándo usarla, y ejemplo del proyecto.

## 4.1 Concepto previo: ¿qué es un "error" en regresión?

En un problema de regresión, el modelo predice un valor numérico (el precio) y lo comparamos con el valor real. El **residuo** o **error** de una predicción es:

```
error_i = valor_real_i − valor_predicho_i
```

Todas las métricas son **formas distintas de resumir el conjunto de errores** en un solo número. La pregunta es: ¿cómo penalizamos los errores?

## 4.2 MAE — Error Absoluto Medio (Mean Absolute Error)

**Qué es:** el promedio del valor absoluto de los errores. "En promedio, ¿por cuántos dólares me equivoco?"

**Fórmula:**
```
MAE = (1/n) · Σ |yᵢ − ŷᵢ|
```
donde `yᵢ` es el valor real, `ŷᵢ` el predicho, y `n` el número de observaciones.

**Interpretación:** está en las **unidades originales** (dólares). Un MAE de $52 significa que, en promedio, la predicción se desvía $52 del precio real.

**Ventajas:**
- Interpretación directa e intuitiva (mismas unidades que el target).
- **Robusto a outliers**: un error grande pesa lo mismo proporcionalmente que uno pequeño.

**Desventajas:**
- No penaliza extra los errores grandes (a veces sí quieres penalizarlos).
- No es diferenciable en cero (problema menor para optimización).

**Cuándo usarla:** cuando todos los errores importan por igual y quieres una métrica fácil de comunicar a no técnicos.

**Ejemplo del proyecto:** Gradient Boosting alcanzó **MAE = $52**, frente a los **$107** de OLS. Es decir, el mejor modelo se equivoca, en promedio, en solo $52 sobre el precio de una RAM.

## 4.3 MSE — Error Cuadrático Medio (Mean Squared Error)

**Qué es:** el promedio de los errores **al cuadrado**.

**Fórmula:**
```
MSE = (1/n) · Σ (yᵢ − ŷᵢ)²
```

**Interpretación:** está en **unidades al cuadrado** (dólares²), lo que lo hace difícil de interpretar directamente. Su valor se usa más como cantidad a minimizar que como métrica a reportar.

**Ventajas:**
- **Penaliza fuertemente los errores grandes** (un error de 10 pesa 100; uno de 2 pesa 4).
- Es diferenciable en todo punto → ideal como **función de pérdida** para optimización (OLS de hecho minimiza MSE).

**Desventajas:**
- **Muy sensible a outliers**: un solo error enorme puede dominar la métrica.
- Unidades no interpretables (dólares al cuadrado no significa nada intuitivo).

**Cuándo usarla:** como función objetivo durante el entrenamiento, o cuando los errores grandes son especialmente costosos.

**Ejemplo del proyecto:** el MSE es la base del RMSE (su raíz). OLS minimiza exactamente el MSE en escala logarítmica — esa es la definición matemática del método de mínimos cuadrados ordinarios.

## 4.4 RMSE — Raíz del Error Cuadrático Medio (Root Mean Squared Error)

**Qué es:** la raíz cuadrada del MSE. Devuelve la métrica a las unidades originales.

**Fórmula:**
```
RMSE = √[ (1/n) · Σ (yᵢ − ŷᵢ)² ]
```

**Interpretación:** está en unidades originales (dólares), como el MAE, pero **penaliza más los errores grandes**. Si RMSE >> MAE, hay outliers o errores muy dispares.

**Ventajas:**
- Unidades interpretables.
- Penaliza errores grandes (hereda esa propiedad del MSE).
- Es la métrica más reportada en la literatura de regresión.

**Desventajas:**
- Sigue siendo sensible a outliers.
- Menos intuitiva que el MAE para un público no técnico.

**Cuándo usarla:** cuando quieres una métrica en unidades originales pero te importa castigar los errores grandes.

**Ejemplo del proyecto:** Gradient Boosting tuvo **RMSE = $125** vs **$203** de OLS. La diferencia entre RMSE y MAE en cada modelo revela cuánto pesan los outliers — en OLS, RMSE ($203) casi duplica al MAE ($107), señal de que hay productos premium difíciles de predecir.

## 4.5 MAPE — Error Porcentual Absoluto Medio (Mean Absolute Percentage Error)

**Qué es:** el promedio del error **en porcentaje** relativo al valor real.

**Fórmula:**
```
MAPE = (100/n) · Σ |（yᵢ − ŷᵢ) / yᵢ|
```

**Interpretación:** "en promedio, ¿en qué porcentaje me equivoco?" Un MAPE de 8.32% significa que la predicción típica se desvía un 8.32% del precio real.

**Ventajas:**
- **Independiente de la escala**: permite comparar el error entre productos baratos y caros de forma justa.
- Muy intuitiva para comunicar ("el modelo se equivoca un 8%").

**Desventajas:**
- **Indefinida si algún valor real es 0** (división por cero).
- **Asimétrica**: penaliza más las sobrepredicciones que las subpredicciones.
- Se infla con valores reales muy pequeños (un error de $5 sobre un producto de $10 es 50%).

**Cuándo usarla:** cuando los valores objetivo varían mucho en escala (como los precios de RAM, de $30 a $1750) y quieres una métrica comparable y comunicable.

**Ejemplo del proyecto:** el MAPE fue la **métrica estrella** para comunicar resultados. Gradient Boosting bajó el MAPE del **17.36%** (OLS) al **8.32%** — una mejora del 52%, "el error se redujo a la mitad". Esta frase es de las más memorables de la defensa.

## 4.6 R² — Coeficiente de Determinación

**Qué es:** la proporción de la varianza del precio que el modelo logra explicar. Es una métrica **relativa**, no absoluta.

**Fórmula:**
```
R² = 1 − (SS_res / SS_tot)

donde:
  SS_res = Σ (yᵢ − ŷᵢ)²        ← suma de cuadrados de los residuos (lo que el modelo NO explica)
  SS_tot = Σ (yᵢ − ȳ)²         ← suma de cuadrados total (la varianza total del target)
  ȳ = media de los valores reales
```

**Interpretación:**
- **R² = 1**: el modelo explica el 100% de la varianza (predicción perfecta).
- **R² = 0**: el modelo no es mejor que predecir siempre la media.
- **R² < 0**: el modelo es *peor* que predecir la media (sí puede pasar en el test set).
- **R² = 0.962**: el modelo explica el 96.2% de la variabilidad del precio.

**¿Qué significa un R² alto o bajo?**
- **R² alto** (> 0.9): el modelo captura casi toda la estructura del fenómeno. Las variables elegidas son buenos predictores.
- **R² bajo** (< 0.5): o faltan variables predictoras importantes, o la relación no es la que el modelo asume, o hay mucho ruido irreducible.
- **Cuidado:** un R² alto en *train* pero bajo en *test* es señal de **overfitting**. Por eso siempre se reporta el R² de test.

**Ventajas:**
- Adimensional: permite comparar modelos sobre datasets distintos.
- Interpretación intuitiva como "porcentaje de varianza explicada".

**Desventajas:**
- **Siempre aumenta (o se queda igual) al añadir variables**, aunque sean ruido → por eso existe el R² ajustado.
- No dice nada sobre si las predicciones están sesgadas.
- Un R² alto no garantiza un buen modelo (puede haber overfitting o un test set fácil).

**Cuándo usarla:** como métrica de resumen global y para comparar modelos. Siempre acompañada de una métrica de error absoluto (MAE/RMSE).

**Ejemplo del proyecto:** la progresión de R² test fue OLS 0.876 → Ridge 0.869 → Random Forest 0.902 → Gradient Boosting 0.962. Todos los modelos explican más del 86% de la varianza del precio.

## 4.7 Diferencia entre error absoluto y error porcentual

Esta es una pregunta clásica de jurado. La respuesta:

- **Error absoluto** (MAE, RMSE): mide la desviación en **unidades fijas** (dólares). Un error de $50 es $50, sin importar si el producto cuesta $80 o $800.
- **Error porcentual** (MAPE): mide la desviación **relativa al valor**. Un error de $50 es 62% sobre un producto de $80, pero solo 6% sobre uno de $800.

**Cuál usar:** si te importa el impacto monetario absoluto, usa MAE/RMSE. Si te importa la *precisión relativa* y tu rango de precios es amplio, usa MAPE. **El proyecto reporta ambos** precisamente para no esconder nada: MAPE para comunicar, MAE/RMSE para rigor.

## 4.8 Cómo afectan los outliers a cada métrica

Tabla resumen — **memoriza esto**:

| Métrica | Sensibilidad a outliers | Por qué |
|---|---|---|
| **MAE** | Baja (robusto) | El error crece linealmente |
| **MSE** | Muy alta | El error crece al cuadrado |
| **RMSE** | Alta | Hereda el cuadrado del MSE |
| **MAPE** | Media-alta | Se infla con valores reales pequeños |
| **R²** | Alta | Usa sumas de cuadrados (SS_res, SS_tot) |

**Ejemplo concreto del proyecto:** hay un producto de ~$1750 (un kit de RAM de gama altísima) que **todos** los modelos sub-predicen. Este outlier infla el RMSE de todos los modelos. Por eso, en el día de Ridge, observamos la "aparente contradicción": Ridge tenía peor R² pero mejor RMSE que OLS — porque Ridge era más conservador con ese outlier extremo, "disparándose" menos.

**Frase para defensa:** *"Reportamos cinco métricas porque cada una cuenta una parte de la historia. El MAE dice el error típico, el RMSE revela el peso de los outliers, el MAPE permite comparación justa entre rangos de precio, y el R² resume la varianza explicada. Un solo número nunca es suficiente para evaluar un modelo con rigor."*

---

# 5. LOS 5 MODELOS DE MACHINE LEARNING

El proyecto entrenó **5 modelos** bajo condiciones idénticas: el mismo split 80/20, `random_state=42`, las mismas 17 features, y métricas comparables. Esto es lo que hace que la comparación sea **justa y defendible**.

Para cada modelo: qué tipo es, cómo funciona internamente, qué predice, cómo aprende, parámetros clave, ventajas/desventajas, complejidad computacional, riesgos de overfitting/underfitting, y por qué se eligió.

## 5.0 Conceptos previos transversales

**Variable objetivo (target):** `log_price` — el logaritmo natural del precio en dólares. Se usa el logaritmo, no el precio directo, porque la distribución del precio está sesgada a la derecha (muchos productos baratos, pocos carísimos). El log la normaliza.

**Features (variables predictoras):** capacity_gb, speed_mhz, cas_latency, num_sticks, has_rgb, cas_was_imputed, generación DDR (dummy), y 5 dummies de marca. Total: 17 columnas.

**Train/test split 80/20:** 80% de los datos (≈240 productos) para entrenar, 20% (≈60 productos) para evaluar. El modelo nunca ve el test set durante el entrenamiento. Esto mide la capacidad de **generalización**.

**Validación cruzada (CV) de 5 folds:** se divide el train en 5 partes; se entrena 5 veces, cada vez dejando una parte distinta como validación. El promedio de los 5 resultados es más robusto que un solo split.

**GridSearchCV:** prueba sistemáticamente todas las combinaciones de hiperparámetros, evaluando cada una con CV de 5 folds, y se queda con la mejor.

## 5.1 Modelo 1 — Regresión Lineal Multivariada (OLS)

**Tipo:** modelo lineal paramétrico supervisado. OLS = Ordinary Least Squares (Mínimos Cuadrados Ordinarios).

**Cómo funciona internamente:** asume que el precio (en log) es una **combinación lineal** de las features:
```
log_price = β₀ + β₁·capacity + β₂·speed + β₃·cas_latency + ... + βₚ·marca_p + ε
```
Cada `βᵢ` es un coeficiente que cuantifica cuánto cambia el log-precio cuando esa feature aumenta en una unidad, manteniendo las demás constantes.

**Qué intenta predecir:** el log-precio de un módulo de RAM dadas sus especificaciones.

**Cómo aprende:** encuentra los coeficientes `β` que **minimizan la suma de los errores al cuadrado** (SS_res). Tiene **solución cerrada** (no necesita iteración): `β = (XᵀX)⁻¹ Xᵀy`. En el proyecto se implementó manualmente con la pseudoinversa de Moore-Penrose (`np.linalg.pinv`), demostrando entendimiento del álgebra lineal subyacente.

**Parámetros importantes:** OLS no tiene hiperparámetros que ajustar — los coeficientes `β` se determinan analíticamente. Lo que sí importa es la **especificación del modelo**: qué variables incluir.

**Ventajas:**
- **Máxima interpretabilidad**: cada coeficiente tiene un significado directo.
- Provee **inferencia estadística**: p-values, intervalos de confianza para cada coeficiente.
- Rapidísimo (solución cerrada).
- Es la base teórica de la estadística clásica.

**Desventajas:**
- Asume **linealidad**: no captura interacciones ni relaciones curvas.
- Sensible a outliers y a multicolinealidad.
- Requiere que se cumplan los supuestos de Gauss-Markov.

**Complejidad computacional:**
- **Entrenamiento:** 𝒪(n·p² + p³) — el término p³ es la inversión de la matriz `XᵀX`. Con p pequeño (17 features), domina el término n·p², es decir, prácticamente lineal en n.
- **Inferencia (predicción):** 𝒪(p) — solo un producto punto por predicción.
- **Espacial:** 𝒪(n·p) para almacenar la matriz de diseño.

**Riesgos:**
- **Underfitting** si la relación real es no lineal (OLS solo ve líneas/planos).
- **Overfitting** es poco probable con un modelo tan simple, salvo con muchísimas features.

**Validación de supuestos (Gauss-Markov):** el proyecto validó los 5 supuestos:
- Multicolinealidad: todos los VIF < 5 ✓
- Normalidad de residuos: QQ-plot con r=0.94 ✓
- Sin autocorrelación: Durbin-Watson = 2.02 ✓
- Homocedasticidad: scale-location plot aproximadamente plano ✓
- Linealidad: leve curvatura cuadrática detectada ⚠ (limitación honesta)

**Por qué se eligió:** es el modelo de **inferencia** por excelencia. Aunque no es el más preciso, es el único que responde "¿*por qué* la RAM cuesta esto?" con rigor estadístico. Hallazgo clave: todas las marcas tienen coeficiente negativo respecto a CORSAIR → **CORSAIR comanda un premium real del 10-35%** controlando por features técnicos.

**Resultado:** R² = 0.876, MAPE = 17.36%, MAE = $107, RMSE = $203.

## 5.2 Modelo 2 — K-Means Clustering

**Tipo:** modelo de aprendizaje **no supervisado** (no usa la variable objetivo). Es de **clustering** (agrupamiento).

**Cómo funciona internamente:** divide los datos en `k` grupos minimizando la distancia de cada punto al **centroide** (centro) de su grupo. Algoritmo iterativo de Lloyd:
1. Inicializa k centroides al azar.
2. Asigna cada punto al centroide más cercano.
3. Recalcula cada centroide como la media de sus puntos.
4. Repite 2-3 hasta que los centroides se estabilicen.

**Qué intenta predecir:** no predice el precio. Responde una pregunta distinta: **"¿en qué grupos naturales se segmenta el mercado?"**

**Cómo aprende:** minimizando la **inercia** (suma de distancias al cuadrado intra-cluster). No hay "respuesta correcta" que aprender — descubre estructura.

**Parámetros importantes:**
- `k` (número de clusters): el más crítico. El proyecto usó una **estrategia híbrida** k=2 (principal, por parsimonia) y k=8 (exploratorio, para nichos).
- `n_init=20`: se ejecuta 20 veces con inicializaciones distintas y se queda con la mejor (evita mínimos locales).
- Requiere **estandarización obligatoria** (`StandardScaler`): si no, las features con escalas grandes (speed_mhz) dominarían la distancia.

**Ventajas:**
- Descubre estructura sin etiquetas.
- Centroides interpretables (un cluster "es" su centroide).
- Rápido y escalable.

**Desventajas:**
- Hay que elegir `k` de antemano (se usó el método del codo + silhouette).
- Asume clusters esféricos y de tamaño similar.
- Sensible a la inicialización (mitigado con `n_init=20`).

**Complejidad computacional:**
- **Entrenamiento:** 𝒪(n·k·i·d) — n puntos, k clusters, i iteraciones, d dimensiones. Lineal en n.
- **Inferencia:** 𝒪(k·d) — calcular distancia a k centroides.
- **Espacial:** 𝒪(n·d + k·d).

**Riesgos:** no aplica overfitting/underfitting en el sentido supervisado. El riesgo es elegir mal `k`.

**Por qué se eligió:** para **validar con un método independiente** el hallazgo del ANOVA. Y funcionó: K-Means con k=2 descubrió un cluster "Premium" (219 productos, 100% DDR5, mediana $490) y uno "Económico" (81 productos, 93.8% DDR4, mediana $166). **Las marcas están mezcladas en ambos clusters** → el mercado se segmenta por generación DDR, NO por marca. Esto confirma cuantitativamente que η²_DDR (60.8%) >> η²_marca (22.4%).

**Métrica de calidad:** Silhouette score = 0.420 para k=2. PCA captura el 69.3% de la varianza en 2D para visualización.

## 5.3 Modelo 3 — Ridge Regression (Regresión con regularización L2)

**Tipo:** modelo lineal paramétrico supervisado **con regularización**. Es OLS + una penalización.

**Cómo funciona internamente:** es OLS, pero la función a minimizar añade un término de penalización sobre el tamaño de los coeficientes:
```
minimizar:  Σ(yᵢ − ŷᵢ)²  +  α · Σβⱼ²
            └─ error OLS ─┘   └─ penalización L2 ─┘
```
El hiperparámetro `α` controla cuánto se penalizan los coeficientes grandes. Esto **"encoge" (shrinkage)** los coeficientes hacia cero, especialmente los de variables con señal débil.

**Qué intenta predecir:** lo mismo que OLS, el log-precio.

**Cómo aprende:** tiene solución cerrada igual que OLS, pero con la penalización: `β = (XᵀX + αI)⁻¹ Xᵀy`. El término `αI` (matriz identidad por α) es el que estabiliza la inversión.

**Parámetros importantes:**
- `α` (alpha): la fuerza de regularización. Se buscó con GridSearchCV en el rango [10⁻⁴, 10⁴], 50 valores en escala logarítmica. El óptimo fue **α = 7.91**.

**Ventajas:**
- Combate la **multicolinealidad** y el overfitting.
- Más estable que OLS cuando las features están correlacionadas.
- Funciona como **detector implícito de relevancia**: encoge más las variables irrelevantes.

**Desventajas:**
- Introduce **sesgo** a cambio de reducir varianza (trade-off sesgo-varianza).
- Los coeficientes ya no son "insesgados" → la inferencia es menos limpia que OLS.
- Hay que ajustar `α`.

**Complejidad computacional:** idéntica a OLS — 𝒪(n·p² + p³) en entrenamiento, 𝒪(p) en inferencia.

**Riesgos:**
- Si `α` es muy grande → **underfitting** (todos los coeficientes encogidos a casi cero).
- Si `α` es muy pequeño → se comporta como OLS (sin beneficio).

**Por qué se eligió:** para **probar si OLS necesitaba regularización**. El resultado fue revelador: empate técnico con OLS (R² 0.869 vs 0.876). La curva de R² es **plana** en el rango α ∈ [10⁻⁴, 10] → esto **confirma que el OLS original ya estaba bien especificado** y no sufría de multicolinealidad (consistente con los VIF < 5).

**Hallazgo nivel máster:** Ridge encoge más fuerte los coeficientes de marca (Kingston −73%, G.SKILL −31%) que los de features técnicas (capacity −4%, speed −11%). Esto **demuestra que las marcas individuales tienen señal estadística más débil** que las características técnicas — coherente con η²_marca < η²_DDR.

**Resultado:** R² = 0.869, MAPE = 17.45%, MAE = $106, RMSE = $198.

## 5.4 Modelo 4 — Random Forest (Bosque Aleatorio)

**Tipo:** modelo de **ensemble** supervisado. Específicamente, **bagging** (bootstrap aggregating) de árboles de decisión.

**Cómo funciona internamente:** entrena **muchos árboles de decisión independientes** y promedia sus predicciones. Cada árbol:
- Se entrena sobre una **muestra bootstrap** (muestreo con reemplazo) de los datos.
- En cada división (split), solo considera un **subconjunto aleatorio de features** (`max_features='sqrt'`).
Esto **descorrelaciona** los árboles. Como sus errores son distintos, al promediar se cancelan → el ensemble es más preciso y estable que un solo árbol.

**Concepto previo — árbol de decisión:** un árbol parte el espacio de features con preguntas binarias ("¿capacity > 32 GB?"). Cada hoja predice el promedio de los puntos que caen ahí. Un solo árbol sobreajusta fácil; el bosque lo arregla.

**Qué intenta predecir:** el log-precio, capturando relaciones **no lineales** e **interacciones** entre features que OLS no puede.

**Cómo aprende:** cada árbol crece dividiendo recursivamente para reducir la varianza intra-nodo. No hay solución cerrada — es un algoritmo voraz (greedy).

**Parámetros importantes (óptimos hallados por GridSearchCV sobre 36 combinaciones):**
- `n_estimators=300`: número de árboles.
- `max_depth=20`: profundidad máxima de cada árbol.
- `min_samples_split=2`: mínimo de muestras para dividir un nodo.
- `max_features='sqrt'`: features consideradas por split (la raíz cuadrada del total).

**Ventajas:**
- Captura no-linealidades e interacciones automáticamente.
- Robusto a outliers y a features irrelevantes.
- **No requiere estandarización** (los árboles son insensibles a la escala).
- Provee **feature importances**.

**Desventajas:**
- **Interpretabilidad reducida**: no hay coeficientes, solo importancias.
- Más lento de entrenar que los modelos lineales.
- Puede sobreajustar si los árboles son muy profundos.

**Complejidad computacional:**
- **Entrenamiento:** 𝒪(m · n·log(n) · √p · d) — m árboles, cada uno 𝒪(n·log n) para ordenar en los splits.
- **Inferencia:** 𝒪(m · d) — recorrer m árboles de profundidad d.
- **Espacial:** 𝒪(m · nodos) para almacenar todos los árboles.

**Riesgos:**
- **Overfitting moderado**: en el proyecto, el gap train-test fue 0.090 (R² train 0.992 vs test 0.902) — justo en el límite aceptable. El CV de 5 folds (0.939 ± 0.018) confirmó que el modelo es robusto.

**Por qué se eligió:** para introducir **no-linealidad** y ver cuánto mejora sobre los modelos lineales. Resultado: R² subió a 0.902 (+2.6 pp sobre OLS) pero el verdadero impacto fue el **MAPE: bajó de 17.4% a 10.4%** (−40% relativo). Las feature importances **confirmaron** OLS: capacity_gb domina (46.8%), seguido de speed_mhz (16.3%).

**Resultado:** R² = 0.902, MAPE = 10.42%, MAE = $71, RMSE = $159.

## 5.5 Modelo 5 — Gradient Boosting (el modelo ganador)

**Tipo:** modelo de **ensemble** supervisado. Específicamente, **boosting** secuencial de árboles de decisión.

**Cómo funciona internamente:** a diferencia de Random Forest (árboles independientes en paralelo), Gradient Boosting entrena árboles **secuencialmente**, donde **cada árbol nuevo corrige los errores residuales del conjunto anterior**:
1. El primer árbol hace una predicción tosca.
2. Se calculan los residuos (lo que el árbol 1 erró).
3. El árbol 2 se entrena para predecir esos residuos.
4. Se suma una fracción (`learning_rate`) del árbol 2 a la predicción.
5. Se repite: cada árbol ataca el error que queda.

**Analogía:** Random Forest es como pedir opinión a 300 expertos independientes y promediar. Gradient Boosting es como 300 expertos en cadena, donde cada uno se especializa en corregir los errores que dejó el anterior.

**Qué intenta predecir:** el log-precio, con el mínimo error posible.

**Cómo aprende:** descenso de gradiente en el espacio de funciones. Cada árbol apunta en la dirección que más reduce la pérdida (el "gradiente" de boosting).

**Parámetros importantes (óptimos hallados por GridSearchCV sobre 54 combinaciones):**
- `n_estimators=200`: número de árboles secuenciales.
- `learning_rate=0.05`: cuánto aporta cada árbol nuevo (bajo = aprendizaje lento pero estable).
- `max_depth=3`: árboles **superficiales** (weak learners).
- `subsample=1.0`: fracción de datos por árbol.

**Ventajas:**
- **El más preciso** de los 5 modelos.
- Captura relaciones complejas.
- Con `learning_rate` bajo + árboles superficiales, **regulariza implícitamente** y generaliza muy bien.

**Desventajas:**
- **Black-box**: la interpretabilidad es la más baja.
- Entrenamiento **secuencial** → no se paraleliza como Random Forest.
- Sensible a los hiperparámetros (requiere tuning cuidadoso).

**Complejidad computacional:**
- **Entrenamiento:** 𝒪(m · n·log(n) · d) — secuencial, no paralelizable. Este es el costo del boosting.
- **Inferencia:** 𝒪(m · d).
- **Espacial:** 𝒪(m · nodos).

**Riesgos:**
- **Overfitting** si `learning_rate` es alto o los árboles son profundos. **Pero en el proyecto NO sobreajustó**: gap train-test de solo **0.030** (R² train 0.992 vs test 0.962) — tres veces menor que Random Forest. Esto es contraintuitivo y defendible: árboles superficiales + learning rate conservador = regularización implícita.

**Por qué se eligió:** es el estado del arte para datos tabulares. Resultado: **el ganador absoluto** en todas las métricas. R² = 0.962, MAPE = 8.32%. Feature importance **extrema** en capacity_gb (85.6%) — GB explota la señal dominante de forma más concentrada que RF (46.8%).

**Resultado:** R² = 0.962, MAPE = 8.32%, MAE = $52, RMSE = $125.

## 5.6 Tabla comparativa final de los 5 modelos

| # | Modelo | Tipo | R² test | MAPE | MAE | RMSE | Interpretable |
|---|---|---|---|---|---|---|---|
| 1 | OLS | Lineal | 0.876 | 17.36% | $107 | $203 | ✅ Alta |
| 2 | K-Means | Clustering | N/A | N/A | N/A | N/A | ✅ Alta (centroides) |
| 3 | Ridge | Lineal regularizado | 0.869 | 17.45% | $106 | $198 | ✅ Alta |
| 4 | Random Forest | Ensemble bagging | 0.902 | 10.42% | $71 | $159 | ⚠️ Parcial |
| 5 | **Gradient Boosting** 🏆 | Ensemble boosting | **0.962** | **8.32%** | **$52** | **$125** | ⚠️ Parcial |

## 5.7 ¿Cuál fue el mejor modelo y por qué?

**Respuesta corta:** Gradient Boosting, en todas las métricas de error.

**Respuesta matizada y defendible — la decisión académica clave del proyecto:**

> *"No existe 'el mejor modelo' en abstracto. Existe el mejor modelo **para un objetivo dado**. El proyecto reporta dos modelos complementarios:*
> - ***OLS para INFERENCIA***: *cuando la pregunta es '¿por qué cuesta esto?', necesitas coeficientes interpretables con p-values e intervalos de confianza. OLS lo da; Gradient Boosting no.*
> - ***Gradient Boosting para PREDICCIÓN***: *cuando la pregunta es '¿cuánto costará una RAM con estas specs?', necesitas el mínimo error. GB logra MAPE 8.32%, la mitad del error de OLS.*
>
> *Esta dualidad no es una indecisión — es la respuesta metodológicamente correcta. Reportar un solo modelo escondería la mitad de la historia."*

## 5.8 Convergencia metodológica — el hallazgo más fuerte

Los 6 paradigmas (ANOVA + OLS + Ridge + K-Means + RF + GB) **convergen** en la misma conclusión:

| Paradigma | Evidencia sobre capacity_gb |
|---|---|
| OLS | coeficiente β = +0.49 (el mayor) |
| Ridge | β = +0.47 (estable bajo regularización) |
| Random Forest | importance = 46.8% (la mayor) |
| Gradient Boosting | importance = 85.6% (dominante) |
| ANOVA | la generación DDR explica η² = 60.8% |
| K-Means | segmenta el mercado por DDR, no por marca |

**Frase de oro para defensa:** *"Cuando seis paradigmas estadísticos independientes —lineal, regularizado, no supervisado, bagging, boosting e inferencial— convergen en la misma conclusión, esa conclusión deja de ser una opinión del modelo y se convierte en una propiedad del mercado. Esta es validación cruzada de nivel publicación."*

## 5.9 Sobre los modelos NO usados (XGBoost, redes neuronales, KNN, SVM)

El jurado puede preguntar "¿por qué no usaron XGBoost / redes neuronales?". Respuesta honesta y defendible:

- **XGBoost / LightGBM:** son implementaciones optimizadas de gradient boosting. Conceptualmente, nuestro `GradientBoostingRegressor` de scikit-learn cubre el mismo paradigma. No añadir XGBoost fue una decisión de alcance — habría dado un resultado similar con más complejidad de dependencias.
- **Redes neuronales:** con solo 300 observaciones y 17 features, una red neuronal **sobreajustaría** casi con seguridad. Las redes brillan con cientos de miles de ejemplos. Para datos tabulares pequeños, los ensembles de árboles son el estado del arte — y eso está documentado en la literatura.
- **KNN (k-vecinos):** sufre la "maldición de la dimensionalidad" con 17 features y es sensible a la escala. Habría sido un modelo débil aquí.
- **SVM:** funciona, pero su interpretabilidad es baja y el tuning del kernel es costoso. No aportaba sobre lo que ya cubrían los 5 modelos elegidos.

**Frase para defensa:** *"Los 5 modelos elegidos cubren los 4 grandes paradigmas de regresión supervisada (lineal, regularizado, bagging, boosting) más uno no supervisado. Añadir más modelos del mismo paradigma habría sido redundante; añadir redes neuronales habría sido metodológicamente incorrecto para un dataset de este tamaño."*

---

# 6. COMPLEJIDAD COMPUTACIONAL Y ANÁLISIS ASINTÓTICO

Esta sección corresponde directamente a la materia de **Asymptotic Notation**. Es donde el proyecto se diferencia de un proyecto de Ciencia de Datos "normal".

## 6.1 ¿Qué es Big O (notación asintótica)?

**Big O** describe **cómo crece el costo de un algoritmo a medida que crece el tamaño de la entrada (n)**, ignorando constantes y términos de menor orden. No mide el tiempo en segundos — mide la **forma del crecimiento**.

**Por qué ignora constantes:** porque para `n` suficientemente grande, la *forma* del crecimiento domina sobre cualquier constante. Un algoritmo 𝒪(n²) con una constante pequeña eventualmente será más lento que un 𝒪(n) con una constante grande.

**Las clases de complejidad más comunes (de mejor a peor):**

| Notación | Nombre | Ejemplo | Si n se duplica... |
|---|---|---|---|
| 𝒪(1) | Constante | Acceder a un elemento de un array | el tiempo no cambia |
| 𝒪(log n) | Logarítmica | Búsqueda binaria, índice B-Tree | el tiempo crece poquísimo |
| 𝒪(n) | Lineal | Recorrer una lista | el tiempo se duplica |
| 𝒪(n log n) | Cuasi-lineal | Mergesort, quicksort | el tiempo poco más que se duplica |
| 𝒪(n²) | Cuadrática | Comparar todos los pares | el tiempo se cuadruplica |
| 𝒪(2ⁿ) | Exponencial | Fuerza bruta de subconjuntos | inviable rápido |

## 6.2 ¿Cómo se mide la complejidad algorítmica?

Hay dos formas:

**1. Análisis teórico (a priori):** se cuenta el número de operaciones elementales como función de `n` leyendo el algoritmo. Ejemplo: un bucle dentro de otro bucle, ambos hasta `n`, es 𝒪(n²).

**2. Análisis empírico (a posteriori):** se **mide el tiempo real** ejecutando el algoritmo con entradas de tamaños crecientes, y se ajusta una curva. El proyecto hizo **ambos**, y los comparó — eso es lo valioso.

**Truco del proyecto — la gráfica log-log:** si `tiempo = c · nᵅ`, entonces aplicando logaritmo a ambos lados: `log(tiempo) = log(c) + α·log(n)`. Es decir, en una **gráfica log-log**, la relación se vuelve una **línea recta cuya pendiente es α**, el exponente de la complejidad. Ajustando una recta con `np.polyfit` se estima α directamente.

## 6.3 Complejidad temporal vs espacial

- **Complejidad temporal:** cómo crece el **tiempo de ejecución** con `n`.
- **Complejidad espacial:** cómo crece la **memoria usada** con `n`.

A veces hay un **trade-off**: puedes gastar más memoria para ir más rápido (ej: un índice B-Tree usa espacio extra para acelerar las búsquedas). El proyecto lo demuestra: los índices ocupan disco adicional pero aceleran las queries.

## 6.4 Complejidad de las operaciones del proyecto

### Búsqueda en base de datos
- **Sin índice (full scan):** 𝒪(n) — hay que revisar cada fila.
- **Con índice B-Tree:** 𝒪(log n) — el árbol balanceado descarta la mitad de las opciones en cada nivel.

### Ordenamiento
- SQLite usa algoritmos 𝒪(n log n) para `ORDER BY`. Si la columna tiene índice, el orden puede ser "gratis" porque el índice ya está ordenado.

### Entrenamiento de modelos
- OLS / Ridge: 𝒪(n·p² + p³) — con p fijo y pequeño, prácticamente 𝒪(n).
- K-Means: 𝒪(n·k·i·d) — lineal en n.
- Random Forest: 𝒪(m·n·log n·√p·d).
- Gradient Boosting: 𝒪(m·n·log n·d) — secuencial.

### Inferencia (predicción)
- Todos los modelos, una vez entrenados, predicen en tiempo que **no depende de n** (el tamaño del dataset de entrenamiento). Predecir un caso nuevo es 𝒪(p) para los lineales, 𝒪(m·d) para los ensembles. En la práctica, **inferencia ≈ 𝒪(1)** respecto al volumen de datos históricos.

## 6.5 El análisis asintótico del pipeline completo

El proyecto documenta cómo la complejidad cambia en cada etapa:

```
ETAPA              COMPLEJIDAD          NOTA
─────────────────────────────────────────────────────────────
Extracción         𝒪(n · t_red)        scraping dominado por I/O de red
Limpieza           𝒪(n)                regex amortizado 𝒪(1) por fila
SQL sin índice     𝒪(n)                búsqueda lineal (full scan)
SQL con B-Tree     𝒪(log n)            búsqueda en árbol balanceado ✓ validado
OLS / Ridge        𝒪(p³)               solución cerrada vía álgebra matricial
K-Means            𝒪(n·k·i·d)          clustering iterativo
Random Forest      𝒪(n·log n·m)        m árboles, paralelizable
Gradient Boosting  𝒪(n·log n·d)        secuencial, no paralelizable
Inferencia         𝒪(1)                modelo entrenado → predicción constante
```

## 6.6 Validación empírica #1 — complejidad de las queries SQL

Se midió el speedup (aceleración) de las queries con índice vs sin índice, escalando el dataset de n=350 a n=50,000:

| n | Q4 speedup (covering index) |
|---|---|
| 350 | 1.61× |
| 1,000 | 2.66× |
| 5,000 | 8.65× |
| 10,000 | 8.14× |
| 50,000 | **13.79×** |

**Hallazgo:** el speedup **crece** con n. Esto es exactamente lo que predice la teoría: la diferencia entre 𝒪(n) y 𝒪(log n) se hace más dramática conforme n crece. A n=350 el índice apenas ayuda (1.6×); a n=50,000 acelera casi 14×.

**Frase para defensa:** *"El speedup creciente valida empíricamente la transición de 𝒪(n) a 𝒪(log n). Para datasets pequeños el índice es casi irrelevante; su valor emerge con la escala. Esto demuestra que la notación asintótica no es teoría abstracta — describe comportamiento real medible."*

## 6.7 La "paradoja del índice" — un hallazgo honesto

No todas las queries mejoraron con índices. **Las queries de baja selectividad** (las que devuelven más del 30% de las filas) **empeoraron** con índice. ¿Por qué?

Cuando una query va a devolver casi todo el dataset, usar el índice obliga a saltar entre el índice y la tabla repetidamente (acceso aleatorio a disco), mientras que un full scan lee la tabla secuencialmente (acceso secuencial, más rápido en disco). El optimizador de SQLite a veces **ignora el índice** precisamente por esto.

**Por qué esto es valioso en la defensa:** demuestra entendimiento profundo. *"Descubrimos que los índices no son universalmente buenos. Para queries de baja selectividad, el full scan secuencial vence al índice por la naturaleza del acceso a disco. Esto es un trade-off real de bases de datos que solo se ve cuando mides empíricamente."*

## 6.8 Validación empírica #2 — complejidad de los modelos ML

Se midió el tiempo de entrenamiento de los 5 modelos escalando el dataset hasta n=100,000, con 75 mediciones totales (5 modelos × 5 tamaños × 3 repeticiones). Se calculó la pendiente empírica α (tiempo ∝ nᵅ).

**Resultado y su interpretación honesta:**

- **Gradient Boosting: α ≈ 1.03** → valida **exactamente 𝒪(n)**. Es el modelo donde la teoría se manifiesta limpiamente, porque es **secuencial** y no puede esconder su crecimiento con paralelismo.
- **Modelos lineales (OLS, Ridge): α ≈ 0.4** → aparente sub-linealidad. **No es que violen la teoría** — es que con tiempos de milisegundos, el **overhead del runtime de Python** (crear objetos, gestión de memoria) domina sobre el costo algorítmico real. Los exponentes asintóticos solo emergen cuando `n` es lo bastante grande para que el algoritmo domine sobre el overhead.
- **Random Forest: α ≈ 0.72** → su crecimiento real está **parcialmente oculto por el paralelismo** (`n_jobs=-1` usa todos los núcleos del CPU). El tiempo wall-clock no escala linealmente porque más trabajo se reparte entre más núcleos.

**Esta es la lección académica más sofisticada del proyecto. Frase de oro:**

> *"El experimento reveló un principio fundamental del análisis empírico de complejidad: los exponentes asintóticos requieren un régimen de n suficientemente grande para emerger por encima del overhead del runtime. Gradient Boosting, al ser inherentemente secuencial, mostró α=1.03 validando 𝒪(n) limpiamente. Los modelos lineales mostraron α≈0.4 porque su costo en milisegundos está dominado por el overhead de Python, no por el algoritmo. Random Forest ocultó su crecimiento real por paralelización. La conclusión: la teoría asintótica describe el límite cuando n→∞, pero efectos de sistema —overhead, paralelismo, caché— gobiernan el comportamiento en datasets reales. Reconocer esto es entender la notación asintótica de verdad, no solo memorizarla."*

## 6.9 ¿Qué algoritmos del proyecto son más costosos?

De más a menos costoso en entrenamiento:
1. **Gradient Boosting** — secuencial, no paralelizable, el cuello de botella.
2. **Random Forest** — costoso pero paralelizable.
3. **K-Means** — lineal en n, pero con `n_init=20` se ejecuta 20 veces.
4. **Ridge / OLS** — los más baratos, solución cerrada.

En **inferencia**, todos son baratísimos (𝒪(1) respecto al volumen de datos).

## 6.10 Optimizaciones implementadas y posibles

**Implementadas:**
- Índices B-Tree en SQLite (𝒪(n) → 𝒪(log n) en búsquedas).
- `n_jobs=-1` en Random Forest (paralelización).
- `recalcular_cas.py`: re-aplica el parser sin re-scrapear (evita repetir la operación cara de red).
- Mediana de múltiples corridas en benchmarks (reduce ruido de medición).

**Posibles mejoras (honestas, para la defensa):**
- Usar `LightGBM` o `XGBoost` (implementaciones de boosting con optimizaciones de bajo nivel).
- Caché de resultados intermedios del pipeline.
- Paralelizar el scraping (actualmente es secuencial).
- Índices compuestos en SQLite para queries multi-columna.

## 6.11 Trade-offs entre precisión y rendimiento

Esta es una pregunta clásica. El proyecto lo ilustra perfectamente:

```
            PRECISIÓN          COSTO COMPUTACIONAL      INTERPRETABILIDAD
OLS         ███               ▌                        ████████
Ridge       ███               ▌                        ████████
Random F.   ██████            ██████                   ███
Grad. Boost ████████          ████████                 ██
```

**Frase para defensa:** *"La progresión OLS → Gradient Boosting muestra un trade-off claro: ganar 9 puntos de MAPE cuesta dos órdenes de magnitud más tiempo de entrenamiento y casi toda la interpretabilidad. La decisión de qué modelo usar no es 'cuál es más preciso' sino 'qué priorizo: explicar o predecir, y cuánto cómputo tengo'."*

---

# 7. BASES DE DATOS: SQLite A FONDO

## 7.1 ¿Qué es SQLite?

SQLite es un **motor de base de datos relacional embebido**. "Embebido" significa que **no hay un servidor** — la base de datos completa es **un solo archivo** en disco (`ram_market.db`), y la librería de SQLite se ejecuta dentro del propio proceso de tu programa.

## 7.2 ¿Cómo funciona? Arquitectura

```
   Tu programa Python
        │
        │  import sqlite3
        ▼
   Librería SQLite (embebida en el proceso)
        │
        │  traduce SQL → operaciones sobre el archivo
        ▼
   ram_market.db  (un solo archivo en disco)
        │
        ├── Páginas de datos (las filas de las tablas)
        ├── Páginas de índices (los árboles B-Tree)
        └── Esquema (definición de tablas)
```

Internamente, SQLite organiza todo en **páginas** de tamaño fijo. Las tablas y los índices se almacenan como **árboles B-Tree** (de ahí que las búsquedas indexadas sean 𝒪(log n)).

## 7.3 Ventajas de SQLite

- **Cero configuración:** no hay servidor que instalar, configurar ni mantener.
- **Portátil:** la base de datos es un archivo; lo copias y ya está.
- **Rápido para lecturas:** para cargas de trabajo dominadas por lectura (como un análisis), es muy eficiente.
- **Transaccional (ACID):** garantiza atomicidad, consistencia, aislamiento y durabilidad.
- **Embebido en Python:** el módulo `sqlite3` es parte de la librería estándar — cero dependencias externas.
- **Ideal para reproducibilidad:** cualquiera clona el repo y tiene la BD exacta.

## 7.4 Desventajas de SQLite

- **Concurrencia de escritura limitada:** solo un escritor a la vez (bloquea toda la BD para escribir).
- **No escala a múltiples servidores:** es de un solo archivo, un solo host.
- **Sin control de usuarios/permisos:** no hay roles ni autenticación.
- **No apto para aplicaciones web de alto tráfico** con muchas escrituras concurrentes.

## 7.5 ¿Por qué SQLite fue la mejor opción para ESTE proyecto?

**Esta es la pregunta del jurado de Bases de Datos. La respuesta:**

> *"SQLite fue la elección correcta por tres razones. Primera: el tamaño. Nuestro dataset es de cientos a decenas de miles de filas — SQLite maneja eso sin esfuerzo, y un servidor como PostgreSQL sería sobre-ingeniería. Segunda: la carga de trabajo. Un proyecto de análisis es 99% lectura, 1% escritura — exactamente donde SQLite brilla y donde su limitación de concurrencia de escritura es irrelevante. Tercera: la reproducibilidad. La base de datos es un archivo versionable; cualquier evaluador clona el repo y tiene exactamente nuestra BD. Y crucialmente, SQLite implementa índices B-Tree reales — pudimos demostrar la transición 𝒪(n)→𝒪(log n) sin montar infraestructura. Para un proyecto académico de análisis, SQLite no es la opción 'simple', es la opción correcta."*

## 7.6 Diferencias contra otros motores

| Motor | Tipo | Concurrencia | Cuándo usarlo |
|---|---|---|---|
| **SQLite** | Embebido, archivo | 1 escritor | Apps locales, análisis, prototipos, móviles |
| **MySQL** | Cliente-servidor | Alta | Aplicaciones web, lecturas masivas |
| **PostgreSQL** | Cliente-servidor | Alta, avanzada | Apps complejas, integridad estricta, datos geoespaciales/JSON |
| **SQL Server** | Cliente-servidor | Alta | Entornos empresariales Microsoft |
| **MongoDB** | NoSQL documental | Alta, distribuida | Datos sin esquema fijo, escalado horizontal |

**El punto clave:** MySQL, PostgreSQL y SQL Server son **cliente-servidor** — necesitan un proceso servidor corriendo. MongoDB es **NoSQL** — no usa SQL ni tablas relacionales, guarda documentos tipo JSON. Para nuestro caso (datos tabulares estructurados, análisis local, reproducibilidad), SQLite gana.

## 7.7 Diseño de la base de datos

**Tabla principal: `ram_products`**

El proyecto usa un diseño de **tabla única desnormalizada**. Esto es deliberado y defendible: para análisis (no para una aplicación transaccional), una tabla plana es óptima — evita JOINs costosos y los datos se leen tal cual los necesitan los modelos.

**Tipos de datos usados:**
- `INTEGER`: capacity_gb, speed_mhz, cas_latency, num_sticks, has_rgb (booleano como 0/1).
- `REAL`: price_usd, log_price.
- `TEXT`: nombre del producto, ddr_type, brand_normalized.

## 7.8 Índices

Se crearon **3 índices B-Tree** sobre las columnas más consultadas:
- Índice sobre `price_usd` — acelera filtros y ordenamientos por precio.
- Índice sobre `ddr_type` — acelera agrupaciones por generación.
- Índice sobre `brand_normalized` — acelera agrupaciones por marca.

Un **covering index** (índice que cubre todas las columnas que una query necesita) permite responder la query **leyendo solo el índice, sin tocar la tabla** — esa es la query Q4 que alcanzó 13.79× de speedup.

## 7.9 Consultas utilizadas

El proyecto usó queries de:
- **Agregación:** `GROUP BY` para precio promedio/mediano por marca y por DDR.
- **Filtrado de rango:** `WHERE price BETWEEN x AND y`.
- **Ordenamiento:** `ORDER BY price`.
- **Conteo:** `COUNT(*)` por categoría.

Y la herramienta clave de diagnóstico: **`EXPLAIN QUERY PLAN`** — le pregunta a SQLite *cómo* va a ejecutar una query (si va a usar índice o full scan). Así se detectó la "paradoja del índice".

## 7.10 Rendimiento y escalabilidad

- **Rendimiento:** validado empíricamente — los índices producen speedups de hasta 13.79× a n=50,000.
- **Escalabilidad:** SQLite escala bien hasta el orden de gigabytes y millones de filas en un solo host. Más allá de eso, o con escritura concurrente intensiva, habría que migrar a PostgreSQL. Para este proyecto, está holgadamente dentro del rango óptimo de SQLite.

**Frase para defensa:** *"Medimos el rendimiento, no lo asumimos. El benchmark de escalado a 50,000 filas muestra que SQLite con índices B-Tree responde queries de alta selectividad casi 14 veces más rápido. Y documentamos honestamente dónde el índice NO ayuda: las queries de baja selectividad. Eso es entender una base de datos, no solo usarla."*

---

# 8. ESTADÍSTICA INFERENCIAL

Esta sección corresponde a la materia de **Bayesian Inference** y a la estadística clásica. Es la parte de Bernardo en la defensa, pero todo el equipo debe entenderla.

## 8.1 ¿Qué es la estadística inferencial?

Es la rama de la estadística que **saca conclusiones sobre una población a partir de una muestra**. En el proyecto: tenemos una *muestra* de 350 productos de RAM y queremos inferir propiedades del *mercado completo* — por ejemplo, "¿es cierto que en general DDR5 es más caro que DDR4, o lo que vemos es casualidad de nuestra muestra?".

La herramienta central es el **contraste de hipótesis**.

## 8.2 ANOVA — Análisis de la Varianza

### ¿Qué es?

ANOVA (ANalysis Of VAriance) es una prueba estadística que **compara las medias de tres o más grupos** simultáneamente para determinar si al menos uno difiere significativamente de los demás.

### ¿Para qué sirve?

En el proyecto se usó dos veces:
1. **ANOVA por generación DDR:** ¿el precio promedio difiere entre DDR3, DDR4 y DDR5?
2. **ANOVA por marca:** ¿el precio promedio difiere entre CORSAIR, G.SKILL, Kingston, etc.?

### ¿Cómo se compone? La idea central

ANOVA descompone la **varianza total** del precio en dos partes:
- **Varianza ENTRE grupos** (between): cuánto difieren las medias de los grupos entre sí.
- **Varianza DENTRO de grupos** (within): cuánta dispersión hay *dentro* de cada grupo.

La intuición: si la varianza *entre* grupos es mucho mayor que la varianza *dentro* de grupos, los grupos son genuinamente distintos. Si son similares, las diferencias podrían ser ruido.

### Hipótesis nula y alternativa

- **H₀ (hipótesis nula):** todas las medias de los grupos son iguales. `μ_DDR3 = μ_DDR4 = μ_DDR5`. "La generación no afecta el precio."
- **H₁ (hipótesis alternativa):** al menos una media difiere. "La generación sí afecta el precio."

### La tabla ANOVA y sus componentes

| Fuente | Suma de cuadrados (SS) | Grados de libertad (df) | Media cuadrática (MS) | F |
|---|---|---|---|---|
| Entre grupos | SS_between | k − 1 | MS_between = SS_between/(k−1) | MS_between / MS_within |
| Dentro de grupos | SS_within | N − k | MS_within = SS_within/(N−k) | |
| Total | SS_total | N − 1 | | |

donde `k` = número de grupos, `N` = número total de observaciones.

**Interpretación de cada componente:**
- **Suma de cuadrados (SS):** mide variabilidad. SS_between = variabilidad explicada por los grupos; SS_within = variabilidad no explicada (ruido).
- **Grados de libertad (df):** el número de valores que pueden variar libremente. Para `k` grupos, hay `k−1` df entre grupos.
- **Media cuadrática (MS):** la SS dividida por sus df. Es una "varianza promedio".
- **F-statistic:** el cociente `MS_between / MS_within`. Si F es grande, la variabilidad entre grupos supera al ruido → los grupos difieren.
- **P-value:** la probabilidad de observar un F tan grande (o mayor) si H₀ fuera verdadera.

### Resultados del proyecto

**ANOVA por DDR:**
- F = 269.24 (¡enorme!)
- p < 0.000001
- **η² (eta cuadrado) = 0.608** → la generación DDR explica el **60.8%** de la varianza del precio.

**ANOVA por marca:**
- F = 19.84
- p < 0.000001
- **η² = 0.224** → la marca explica el **22.4%** de la varianza.

### ¿Qué es η² (eta cuadrado)?

Es el **tamaño del efecto** para ANOVA: qué proporción de la varianza total es explicada por el factor. F te dice *si* hay efecto; η² te dice *qué tan grande* es. Un η² de 0.608 es un efecto **muy grande**.

**Conclusión del proyecto:** la generación DDR explica **2.7 veces más varianza** que la marca (60.8% vs 22.4%). El factor dominante del precio es la tecnología, no el branding.

### Tukey HSD — el test post-hoc

ANOVA solo dice "al menos un grupo difiere", pero no *cuáles*. El test **Tukey HSD** (Honestly Significant Difference) compara todos los pares de grupos controlando el error acumulado (FWER, Family-Wise Error Rate). En el proyecto: para DDR, los 3 pares resultaron significativos (3/3); para marca, 6 de 15 pares (40%).

## 8.3 P-Value

### ¿Qué representa realmente?

El **p-value** es la probabilidad de observar un resultado **tan extremo o más** que el observado, **asumiendo que la hipótesis nula es verdadera**.

**Lo que NO es** (errores comunes que el jurado puede testear):
- ❌ NO es la probabilidad de que H₀ sea verdadera.
- ❌ NO es la probabilidad de que H₁ sea falsa.
- ❌ NO mide el tamaño del efecto (para eso están η², Cohen's d).

**Lo que SÍ es:** una medida de **incompatibilidad entre los datos y la hipótesis nula**. Un p-value pequeño significa "estos datos serían muy raros si H₀ fuera cierta → dudo de H₀".

### Interpretación por umbrales

- **p < 0.05:** resultado estadísticamente significativo. Se rechaza H₀. (Umbral convencional, no sagrado.)
- **p > 0.05:** no hay evidencia suficiente para rechazar H₀. **Ojo:** esto NO prueba que H₀ sea verdadera — solo que no hay evidencia contra ella.
- **p ≈ 0** (como p < 0.000001 en el proyecto): evidencia abrumadora contra H₀. La diferencia observada es casi imposible de explicar por azar.

### Relación con la significancia estadística

"Estadísticamente significativo" significa simplemente "p < umbral elegido". **Pero significancia estadística ≠ relevancia práctica.** Con una muestra enorme, hasta una diferencia trivial puede ser "significativa". Por eso el proyecto **siempre acompaña el p-value con el tamaño del efecto** (η², Cohen's d) — una práctica de rigor estadístico moderno.

## 8.4 Prueba T vs Prueba Z

### Diferencias fundamentales

| Aspecto | Prueba Z | Prueba T |
|---|---|---|
| **Varianza poblacional** | Conocida | Desconocida (se estima de la muestra) |
| **Tamaño de muestra** | Grande (n > 30 típicamente) | Cualquiera, especialmente pequeña |
| **Distribución** | Normal estándar | t de Student (colas más anchas) |
| **Uso real** | Raro (rara vez conoces σ poblacional) | Común (es el caso realista) |

### ¿Cuándo usar cada una?

- **Prueba Z:** cuando conoces la **varianza poblacional real** (σ²). Esto casi nunca ocurre en la práctica — normalmente solo tienes la varianza de tu muestra.
- **Prueba T:** cuando **estimas la varianza desde la muestra** (lo habitual). La distribución t tiene colas más anchas que la normal para compensar la incertidumbre extra de estimar σ. A medida que n crece, la t converge a la normal.

### Supuestos

Ambas asumen:
- Las observaciones son independientes.
- La variable (o las medias muestrales) siguen una distribución aproximadamente normal.

La prueba T además tiene una variante (**Welch's t-test**) que **no asume varianzas iguales** entre los dos grupos — más robusta.

### Ejemplo aplicado al proyecto

El proyecto comparó **DDR4 vs DDR5** (dos grupos) con la **prueba t de Welch**:
- **t = 17.83** (estadístico enorme)
- **p < 0.000001**
- **Cohen's d = 2.20** (tamaño de efecto "muy grande")

Se usó **t de Welch** (no Z) porque no conocemos la varianza poblacional del mercado de RAM — solo tenemos la varianza de nuestra muestra. Y se usó la variante de Welch porque las varianzas de precio de DDR4 y DDR5 no son iguales.

**Triangulación de robustez:** el proyecto no se conformó con el t-test. Corrió también el **Mann-Whitney U** (test **no paramétrico**, no asume normalidad): U = 23,657, p < 0.000001. Ambos coinciden → la conclusión es robusta sea cual sea el supuesto de distribución.

### ¿Qué es Cohen's d?

Es el **tamaño del efecto** para comparación de dos medias: cuántas desviaciones estándar separan a los dos grupos. d = 2.20 significa que las medias de DDR4 y DDR5 están separadas por 2.2 desviaciones estándar — una diferencia gigante. (d=0.2 es pequeño, d=0.5 medio, d=0.8 grande; 2.20 es enorme.)

## 8.5 Validación de supuestos previa

Antes de aplicar tests paramétricos, el proyecto validó:
- **Normalidad:** test de **Shapiro-Wilk** y QQ-plots.
- **Homocedasticidad** (igualdad de varianzas): test de **Levene**.

Cuando los supuestos eran dudosos, se usaron alternativas **no paramétricas** (Mann-Whitney, Kruskal-Wallis). Esta **triangulación paramétrico + no paramétrico + tamaño de efecto** es la marca de rigor del análisis inferencial del proyecto.

## 8.6 La conexión bayesiana

La materia se llama "Bayesian Inference". ¿Dónde está lo bayesiano?

En la **imputación de datos faltantes**. 51 productos no tenían latencia CAS detectable. En lugar de borrarlos o poner un valor arbitrario, se imputó la **mediana condicional por generación DDR**. Esto es un **prior informativo**: usamos conocimiento previo ("la latencia CAS típica depende de la generación DDR") para informar la estimación del valor faltante. Es razonamiento bayesiano aplicado: combinar conocimiento previo con los datos disponibles.

---

# 9. ANÁLISIS EXPLORATORIO DE DATOS (EDA)

## 9.1 ¿Por qué es importante el EDA?

El EDA (Exploratory Data Analysis) es la fase donde **conoces tus datos antes de modelarlos**. Saltárselo es la causa #1 de modelos malos. El EDA responde: ¿qué forma tienen los datos? ¿hay errores? ¿qué variables se relacionan? ¿qué transformaciones necesito?

**Frase para defensa:** *"El EDA no es opcional. Es donde descubrimos el bug de paginación que multiplicó el dataset por 30, donde detectamos el sesgo del precio que justificó la transformación logarítmica, y donde validamos que las correlaciones tenían sentido físico antes de confiar en cualquier modelo."*

## 9.2 Cómo se detectaron los problemas

### Outliers
Se detectaron con **boxplots** y análisis de la distribución. El producto de ~$1750 (kit de gama altísima) se identificó como outlier legítimo — no es un error, es un producto real raro. Se mantuvo en el dataset pero se documentó que todos los modelos lo sub-predicen.

### Datos faltantes
51 productos sin latencia CAS detectable. Se detectó contando nulos por columna. Se resolvió con **imputación condicional bayesiana** (mediana por DDR), no con borrado — borrar 51 de 350 habría perdido el 15% del dataset.

### Correlaciones
Se calculó la **matriz de correlación de Pearson** y se visualizó como heatmap. Reveló qué features se relacionan con el precio (capacity, speed) y entre sí (chequeo previo de multicolinealidad).

### Sesgos (skewness)
La distribución del precio estaba **sesgada a la derecha** (cola larga de productos caros). Se detectó con histogramas y el coeficiente de asimetría de Pearson. Solución: **transformación logarítmica** — `log_price` tiene una distribución mucho más simétrica, lo que cumple mejor el supuesto de normalidad de OLS.

### Distribuciones
Se examinó la distribución de cada variable: la composición DDR (65% DDR5, 31% DDR4, 4% DDR3), las marcas (CORSAIR 103, Other 100, G.SKILL 59, etc.), los rangos de capacidad y frecuencia.

## 9.3 Selección de variables y feature engineering

### Variables creadas (feature engineering)
- `log_price`: logaritmo del precio (normaliza la distribución).
- `ddr_group`: agrupa DDR3+DDR4 como "DDR_legacy" vs "DDR5". Se hizo porque DDR3 tenía solo n=11 — muy pocos casos para tratarlo como grupo propio sin penalizar la potencia estadística.
- Dummies de marca: codificación one-hot de la variable categórica `brand_normalized`.
- `cas_was_imputed`: variable binaria que marca si la latencia CAS de esa fila fue imputada (transparencia — el modelo "sabe" qué datos son imputados).

### Variables más relevantes (confirmado por los 5 modelos)
1. **capacity_gb** — predictor #1 universal.
2. **speed_mhz** — segundo más importante.
3. **cas_latency** — relevante.
4. **generación DDR** — relevante.
5. **marca** — secundaria pero con efecto real (premium CORSAIR).

### Variables descartadas o de bajo impacto
- `has_rgb` resultó tener impacto bajo — el RGB casi no mueve el precio controlando por lo demás.
- Variables de texto libre (nombres de producto) no se usaron como predictores directos — se usaron solo para extraer features estructurados vía regex.

## 9.4 Impacto del preprocesamiento en el modelo final

El preprocesamiento **no es un trámite** — es lo que hace posible el resultado:
- Sin la **transformación logarítmica**, OLS violaría el supuesto de normalidad y sus p-values serían poco confiables.
- Sin la **imputación bayesiana**, se habría perdido el 15% del dataset.
- Sin el **arreglo del bug de paginación**, el dataset tendría 12 productos en lugar de 350 — estadísticamente inservible.
- Sin la **estandarización**, K-Means y Ridge habrían estado dominados por la feature de mayor escala (speed_mhz).

**Frase para defensa:** *"El 80% del trabajo de Ciencia de Datos es preparar los datos. El modelo es la punta del iceberg. Nuestro MAPE de 8.3% no viene solo de Gradient Boosting — viene de un dataset limpio, bien transformado, con los faltantes imputados con criterio. El mejor algoritmo sobre datos sucios da basura."*

---

# 10. PROGRAMACIÓN Y ARQUITECTURA DE SOFTWARE

## 10.1 Paradigmas de programación usados

El proyecto combina varios paradigmas:
- **Programación procedural/estructurada:** los scripts se organizan en funciones con responsabilidades claras (`main()`, funciones auxiliares).
- **Programación orientada a objetos (POO):** se usan `dataclass` para modelar los productos de RAM con tipado estricto desde el origen, en lugar de diccionarios sueltos.
- **Programación vectorizada/funcional:** pandas y NumPy operan sobre arrays completos en lugar de bucles explícitos (más rápido y legible).

## 10.2 Modularidad

El proyecto está dividido en **módulos independientes** organizados en las 6 carpetas numeradas de `src/`. Cada script:
- Tiene una **única responsabilidad** (scraping, o limpieza, o un modelo específico).
- Es **ejecutable de forma independiente** (`python ols.py`).
- Lee sus entradas de archivos y escribe sus salidas a archivos.

## 10.3 Buenas prácticas implementadas

- **Reproducibilidad:** `random_state=42` en todo el proyecto. Cualquiera que ejecute el código obtiene exactamente los mismos resultados.
- **Rutas robustas:** `Path(__file__).parent` en lugar de rutas absolutas hardcodeadas — el código funciona sin importar desde dónde se ejecute.
- **Medición rigurosa:** `time.perf_counter()` (resolución de nanosegundos, monotónico) en lugar de `time.time()`; mediana de múltiples corridas en lugar de una sola medición ruidosa.
- **Comparabilidad estricta:** todos los modelos usan el mismo split, las mismas features, las mismas métricas.
- **Documentación:** cada script tiene un docstring que explica qué hace, qué pregunta responde, y su pipeline interno.
- **Control de versiones:** commits narrativos multipárrafo que cuentan la historia técnica.

## 10.4 Manejo de errores

- **`try/except` por ítem en el scraping:** si un producto falla al parsearse, se registra y se continúa — un producto roto no tumba todo el scraping. Esta es la **granularidad de fallo correcta**.
- **Validación de rangos:** la latencia CAS se valida en el rango [10, 60] — valores fuera de ahí se rechazan como errores de parseo.
- **`log(1+x)` en lugar de `log(x)`:** robusto ante valores potenciales de cero (log(0) es −infinito).

## 10.5 Reutilización de código

- `recalcular_cas.py` reutiliza el parser de latencia CAS sin re-ejecutar el scraping completo — separa la lógica de parseo de la lógica de extracción.
- Las funciones de carga y preprocesamiento siguen el mismo patrón en los 5 modelos, garantizando que todos parten de datos idénticos.

## 10.6 Estructura escalable y separación de responsabilidades

Cada capa del pipeline tiene **una sola razón para cambiar**:
- Si Newegg cambia su HTML → solo se toca `01_scraping/`.
- Si se quiere un modelo nuevo → solo se añade un archivo a `05_models/`.
- Si se quiere cambiar la base de datos → solo se toca `03_database/`.

Esto es **bajo acoplamiento, alta cohesión** — el principio de diseño que hace el software mantenible.

## 10.7 Flujo de ejecución del proyecto completo

```
1. python src/01_scraping/scraper.py           → ram_raw.csv
2. python src/02_data_processing/clean.py      → ram_clean.csv
3. python src/02_data_processing/eda.py        → figuras EDA
4. python src/03_database/create_db.py         → ram_market.db
5. python src/03_database/create_indices.py    → índices B-Tree
6. python src/03_database/benchmark_*.py       → benchmarks SQL
7. python src/04_inference/*.py                → tests estadísticos
8. python src/05_models/ols.py ... gradient_boosting.py  → 5 modelos
9. python src/06_analysis/ml_complexity.py     → complejidad ML
10. python src/06_analysis/final_dashboard.py  → dashboard maestro
```

Tiempo total de reproducción: ~25 minutos (el cuello de botella es el benchmark de escalado SQL).

---

# 11. PREGUNTAS Y RESPUESTAS ANTICIPADAS DEL JURADO

Organizadas por área. Practica responderlas **en voz alta**.

## 11.1 Preguntas de Machine Learning

**P: ¿Por qué Gradient Boosting es mejor que Random Forest si ambos son ensembles de árboles?**
R: Random Forest entrena árboles independientes en paralelo y los promedia — reduce varianza. Gradient Boosting entrena árboles secuencialmente, donde cada uno corrige los errores residuales del anterior — reduce sesgo *y* varianza. En nuestro dataset, GB con learning_rate bajo (0.05) y árboles superficiales (depth 3) logró R²=0.962 vs 0.902 de RF, y crucialmente un gap train-test de solo 0.030 vs 0.090 — mejor generalización.

**P: ¿No sobreajustó Gradient Boosting con 200 árboles sobre solo 240 datos de entrenamiento?**
R: No, y eso es lo interesante. El gap train-test fue de solo 0.030. La razón: usamos árboles superficiales (max_depth=3, "weak learners") y learning_rate conservador (0.05). Esa combinación es una regularización implícita. El CV de 5 folds lo confirmó: 0.960 ± 0.018, consistente con el test.

**P: ¿Por qué usaron el logaritmo del precio en lugar del precio directo?**
R: La distribución del precio está sesgada a la derecha — muchos productos baratos, pocos carísimos. OLS asume residuos normales; con el precio crudo ese supuesto se viola. El log-precio tiene una distribución mucho más simétrica. Además, modela el precio de forma multiplicativa, que es más realista — un feature "multiplica" el precio en lugar de "sumarle" una constante.

**P: ¿Cómo garantizan que la comparación entre los 5 modelos es justa?**
R: Mismo split 80/20, mismo random_state=42, las mismas 17 features, las mismas métricas calculadas igual. La única variable que cambia entre experimentos es el modelo. Es un diseño experimental controlado.

**P: ¿Por qué no usaron una red neuronal?**
R: Con 300 observaciones y 17 features, una red neuronal sobreajustaría casi con certeza. Las redes necesitan decenas de miles de ejemplos. Para datos tabulares pequeños, los ensembles de árboles son el estado del arte documentado — y nuestros resultados lo confirman.

## 11.2 Preguntas de Estadística / Matemáticas

**P: ¿Qué es exactamente el p-value y qué NO es?**
R: Es la probabilidad de observar un resultado tan extremo o más, asumiendo que la hipótesis nula es verdadera. NO es la probabilidad de que la hipótesis nula sea verdadera — ese es el error de interpretación más común. Un p-value pequeño significa que los datos serían muy raros bajo H₀, así que dudamos de H₀.

**P: ¿Por qué usaron t de Welch y no una prueba Z?**
R: Porque no conocemos la varianza poblacional del mercado de RAM — solo la varianza de nuestra muestra. La prueba Z requiere varianza poblacional conocida, lo cual casi nunca ocurre en la práctica. Y usamos la variante de Welch porque las varianzas de precio de DDR4 y DDR5 no son iguales.

**P: F=269 en el ANOVA es enorme. ¿Eso no es sospechoso?**
R: No, es coherente. F es el cociente entre varianza explicada y varianza residual. Un F de 269 significa que la diferencia de precio entre generaciones DDR es 269 veces mayor que el ruido interno de cada grupo. Y el η²=0.608 lo respalda: la generación DDR explica el 60.8% de toda la varianza. Es un efecto genuinamente grande, no un artefacto.

**P: ¿Por qué reportan η² y Cohen's d además del p-value?**
R: Porque el p-value solo dice *si* hay un efecto, no *qué tan grande* es. Con muestras grandes, hasta diferencias triviales son "significativas". El tamaño del efecto (η², Cohen's d) cuantifica la magnitud práctica. Reportar ambos es rigor estadístico moderno.

## 11.3 Preguntas de Bases de Datos

**P: ¿Por qué SQLite y no PostgreSQL o MySQL?**
R: Tres razones: el tamaño del dataset (cientos a decenas de miles de filas — SQLite lo maneja sin esfuerzo), la carga de trabajo (99% lectura, donde la limitación de concurrencia de escritura de SQLite es irrelevante), y la reproducibilidad (la BD es un archivo versionable). Además, SQLite implementa índices B-Tree reales, así que pudimos demostrar la transición 𝒪(n)→𝒪(log n) sin montar infraestructura.

**P: ¿Qué es la "paradoja del índice" que mencionan?**
R: Descubrimos que las queries de baja selectividad — las que devuelven más del 30% de las filas — empeoran con índice. Cuando vas a leer casi toda la tabla, el índice te obliga a saltar entre el índice y la tabla (acceso aleatorio a disco), mientras que un full scan lee secuencialmente. El optimizador de SQLite a veces ignora el índice por esto. Es un trade-off real que solo se ve midiendo.

**P: ¿Qué es un covering index?**
R: Un índice que contiene todas las columnas que una query necesita. Permite responder la query leyendo solo el índice, sin tocar la tabla. Esa fue nuestra query Q4, que alcanzó 13.79× de speedup a n=50,000.

## 11.4 Preguntas de Complejidad / Optimización

**P: Midieron complejidad y los modelos lineales dieron α≈0.4, no α≈1. ¿No deberían ser 𝒪(n)?**
R: Teóricamente sí son 𝒪(n). El α≈0.4 no es una violación de la teoría — es el efecto del overhead del runtime. Con tiempos de milisegundos, el costo de crear objetos de Python y gestionar memoria domina sobre el costo algorítmico. Los exponentes asintóticos solo emergen cuando n es lo bastante grande para que el algoritmo domine. Gradient Boosting, que es secuencial y más costoso, sí mostró α=1.03 — la teoría se manifiesta limpiamente cuando el algoritmo domina sobre el overhead.

**P: ¿Por qué Random Forest no mostró su crecimiento teórico?**
R: Porque lo paralelizamos con n_jobs=-1. El tiempo wall-clock no escala linealmente cuando más trabajo se reparte entre más núcleos del CPU. Su crecimiento real está ahí, pero el paralelismo lo enmascara.

**P: ¿Cuál es el algoritmo más costoso del proyecto?**
R: Gradient Boosting en entrenamiento, porque es secuencial — cada árbol depende del anterior, no se puede paralelizar. En inferencia todos son baratos: una vez entrenado el modelo, predecir es 𝒪(1) respecto al volumen de datos históricos.

## 11.5 Preguntas de Programación / Arquitectura

**P: ¿Por qué scripts y no Jupyter Notebooks?**
R: Los scripts son reproducibles de forma determinista — se ejecutan de arriba a abajo sin estado oculto. Versionan limpio en Git. Y son la forma en que el código corre en producción. Los notebooks son geniales para explorar; los scripts son mejores para un pipeline de ingeniería.

**P: ¿Cómo manejan que Newegg cambie su HTML y rompa el scraper?**
R: La arquitectura está desacoplada por capas. Si Newegg cambia su HTML, solo se toca la carpeta 01_scraping — el resto del pipeline no se entera porque consume el CSV, no la web. Esa es la ventaja de la separación de responsabilidades.

**P: ¿Qué pasa si un producto falla al parsearse durante el scraping?**
R: Tenemos try/except por ítem. Un producto roto se registra y se salta, pero no tumba el scraping completo. Esa es la granularidad de fallo correcta para una operación de extracción masiva.

## 11.6 Preguntas comodín (las difíciles)

**P: ¿Cuál es la limitación más grande de su proyecto?**
R: El tamaño del dataset — 300 productos válidos. Es suficiente para los modelos lineales y para los tests inferenciales (los efectos son enormes), pero para los ensembles más datos siempre ayudaría. Lo documentamos honestamente: el gap train-test de Random Forest (0.090) refleja esa limitación. La validación cruzada de 5 folds es nuestra defensa contra ese riesgo.

**P: Si tuvieran un mes más, ¿qué harían?**
R: Tres cosas: ampliar el dataset a varios miles de productos de múltiples retailers (no solo Newegg), añadir validación temporal (los precios de RAM cambian con el tiempo), y desplegar el mejor modelo como una API web para que sea usable de verdad.

**P: ¿Por qué debería confiar en su conclusión de que capacity_gb es el predictor #1?**
R: Porque no es la conclusión de *un* modelo — es la conclusión de *seis* paradigmas independientes. OLS le da el mayor coeficiente, Ridge lo confirma bajo regularización, Random Forest le da 46.8% de importancia, Gradient Boosting un 85.6%, el ANOVA muestra que la generación DDR explica el 60.8% de la varianza, y K-Means segmenta el mercado por DDR. Cuando seis métodos que no comparten supuestos convergen, la conclusión es una propiedad del mercado, no un artefacto del modelo.

**P: ¿Qué aprendieron que no esperaban?**
R: Que la teoría asintótica tiene límites prácticos. Esperábamos que todos los modelos mostraran sus exponentes teóricos limpiamente. En cambio descubrimos que el overhead del runtime y el paralelismo enmascaran la complejidad teórica para datasets pequeños. Entender *por qué* la teoría no se manifestó fue más educativo que si los números hubieran "cuadrado" perfecto.

---

# 12. ERRORES Y OPORTUNIDADES DE MEJORA

Reconocer las limitaciones **fortalece** la defensa — demuestra pensamiento crítico.

## 12.1 Limitaciones reconocidas honestamente

1. **Tamaño del dataset:** 300 productos válidos. Suficiente para inferencia (efectos enormes) y modelos lineales, pero los ensembles se beneficiarían de más datos.
2. **Una sola fuente:** todos los datos vienen de Newegg.com. Otros retailers podrían tener estructura de precios distinta.
3. **Sin dimensión temporal:** es una foto del mercado en un momento. Los precios de RAM fluctúan; un modelo temporal sería más robusto.
4. **Leve no-linealidad en OLS:** el supuesto de linealidad mostró una leve curvatura cuadrática en los residuos. OLS sigue siendo válido para inferencia, pero es una imperfección documentada.
5. **El outlier de $1750:** todos los modelos lo sub-predicen. Es un producto real raro, aislado en el espacio de features.

## 12.2 Decisiones de diseño que son defendibles (no errores)

- **Tabla única desnormalizada:** correcto para análisis, evita JOINs.
- **DDR3+DDR4 agrupados como "legacy":** DDR3 tenía solo n=11 — agruparlo preserva potencia estadística.
- **Scripts sobre notebooks:** reproducibilidad y versionado.

## 12.3 Oportunidades de mejora futuras

- Ampliar a múltiples retailers y a miles de productos.
- Añadir validación temporal / series de tiempo.
- Probar XGBoost/LightGBM (boosting optimizado).
- Índices compuestos en SQLite para queries multi-columna.
- Desplegar el modelo ganador como API web.
- Paralelizar el scraping.

## 12.4 La filosofía del proyecto sobre los errores

**Frase para defensa:** *"Documentamos nuestros errores como aprendizajes: el bug de Brotli, el bug de paginación que casi nos deja con 12 productos, la paradoja del índice, la emergencia asintótica que no apareció como esperábamos. Un proyecto sin errores documentados es un proyecto que esconde algo. Los resultados imperfectos defendidos con rigor valen más que resultados perfectos sin entender."*

---

# 13. RESUMEN EJECUTIVO Y CONCLUSIONES

## 13.1 Resumen ejecutivo

Este proyecto integra **seis paradigmas de Ciencia de Datos** aplicados al mercado de memorias RAM: web scraping, SQL con análisis empírico de complejidad, estadística inferencial, y cinco modelos predictivos comparativos. Sobre 350 productos extraídos de Newegg.com, se validó estadísticamente que la generación DDR es el factor dominante del precio (η²=0.608, F=269, p<10⁻⁶), seguido por un premium de marca real: CORSAIR cobra 10-35% más que sus competidores controlando por features técnicos. Gradient Boosting alcanza la mejor predicción (MAPE=8.32%, R²=0.962) y valida empíricamente la complejidad teórica 𝒪(n) con pendiente α=1.03 sobre 75 mediciones (n hasta 100,000). El proyecto reporta dos modelos complementarios: OLS para inferencia interpretable y Gradient Boosting para predicción de mínimo error.

## 13.2 Conclusiones técnicas

1. **Sobre los predictores del precio:** la capacidad (capacity_gb) es el predictor #1 universal, confirmado por los 6 paradigmas. La generación DDR es el segundo factor (explica 60.8% de la varianza). La marca es secundaria (22.4%) pero con un premium CORSAIR real del 10-35%.

2. **Sobre los modelos:** existe un trade-off claro precisión-interpretabilidad. Gradient Boosting gana en predicción (MAPE 8.32%), OLS gana en inferencia (coeficientes con p-values). La decisión correcta no es elegir uno, sino reportar ambos según el objetivo.

3. **Sobre la complejidad:** la teoría asintótica se valida empíricamente cuando el algoritmo domina sobre el overhead — Gradient Boosting mostró α=1.03 (𝒪(n) exacto). Para modelos rápidos, el overhead del runtime y el paralelismo enmascaran la complejidad teórica. Los índices B-Tree producen speedups crecientes con n (hasta 13.79×), validando la transición 𝒪(n)→𝒪(log n).

4. **Sobre la metodología:** la triangulación —que paradigmas independientes converjan— es la validación más robusta posible. Seis métodos coincidiendo en capacity_gb hace esa conclusión casi incontestable.

## 13.3 Conclusiones académicas

El proyecto demuestra que la verdadera contribución no es "entrenar un modelo que funcione", sino **el método de evaluación**: cinco modelos sobre el mismo split, las mismas features, métricas comparables, decisiones justificadas, y limitaciones documentadas honestamente. Conecta tres dominios — matemáticas (álgebra lineal del OLS, estadística inferencial), programación (pipeline modular reproducible) y machine learning (los 5 modelos) — bajo el marco unificador del análisis asintótico.

## 13.4 La frase de cierre del proyecto

> *"Programar bien no es tener todo perfecto al primer intento — es saber diagnosticar, ajustar y documentar el aprendizaje. La verdadera contribución académica es el método: cinco modelos, el mismo split, métricas comparables, decisión justificada. Y a veces, los resultados negativos —la paradoja del índice, la emergencia asintótica que no apareció— son los más educativos."*

---

# 14. CHEAT SHEET DE NÚMEROS CLAVE

**Memoriza esto. Son los números que el jurado puede preguntar en cualquier momento.**

## Dataset
- **350** productos limpios (**300** válidos tras filtros de modelos)
- **17** features · **3** generaciones DDR · **5** marcas + "Other"
- Composición: DDR5 **65.4%** · DDR4 **31.4%** · DDR3 **3.2%**

## Los 5 modelos (R² test / MAPE)
- OLS: **0.876** / **17.36%** · MAE $107 · RMSE $203
- K-Means: clustering (k=2, silhouette **0.420**)
- Ridge: **0.869** / **17.45%** (α óptimo = **7.91**)
- Random Forest: **0.902** / **10.42%** · MAE $71
- **Gradient Boosting** 🏆: **0.962** / **8.32%** · MAE $52 · RMSE $125

## Hiperparámetros óptimos
- RF: n_estimators=**300**, max_depth=**20**, max_features='sqrt'
- GB: n_estimators=**200**, learning_rate=**0.05**, max_depth=**3**

## Estadística inferencial
- Welch's t (DDR4 vs DDR5): t=**17.83**, p<**0.000001**, Cohen's d=**2.20**
- ANOVA DDR: F=**269.24**, η²=**0.608** (60.8% de la varianza)
- ANOVA marca: F=**19.84**, η²=**0.224** (22.4% de la varianza)
- DDR explica **2.7×** más varianza que la marca

## Complejidad
- Q4 SQL speedup: **1.61×** (n=350) → **13.79×** (n=50,000)
- Complejidad ML: **75 mediciones** (5 modelos × 5 tamaños × 3 reps)
- Gradient Boosting: α empírica = **1.03** → valida 𝒪(n) exacto
- Modelos lineales: α≈**0.4** (overhead domina)
- Random Forest: α≈**0.72** (paralelismo enmascara)

## Feature importance — capacity_gb es #1 en TODOS
- OLS β=**+0.49** · Ridge β=+0.47 · RF=**46.8%** · GB=**85.6%**

## Hallazgos memorables
- "CORSAIR comanda premium real **10-35%** controlando por features"
- "**6 paradigmas** independientes convergen en capacity_gb como predictor #1"
- "MAPE bajó del **17.4%** al **8.32%** — el error se redujo a la mitad"
- "OLS para INFERENCIA, Gradient Boosting para PREDICCIÓN"
- "α empírica = **1.03** valida exactamente la complejidad teórica 𝒪(n)"

## Sprint
- **12 días** · deadline **19 mayo 2026** · defensa oral **10 min** · **4 evaluadores**
- **26** figuras a 300 DPI · **12+** commits · repo profesional reestructurado

---

*Fin de la Guía de Estudio. Última actualización: 14 de mayo de 2026.*
*Universidad de Guadalajara · Proyecto "Optimización Asintótica en la Predicción del Mercado de Memoria RAM"*
