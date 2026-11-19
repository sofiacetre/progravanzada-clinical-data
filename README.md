# Ingeniería de Datos Clínicos y Predicción de Apendicitis

Pipeline que adquiere definiciones clínicas de MedlinePlus mediante Web Scraping, las almacena en Google BigQuery, procesa un dataset de pacientes con Polars y entrena tres modelos de Regresión Logística para predecir apendicitis, comparando el impacto de la escala de Alvarado y los hallazgos de ultrasonido.

**Proyecto Final — Tópicos Selectos de Programación**

---

## Descripción del proyecto

El objetivo es construir un flujo reproducible de ingeniería y análisis de datos clínicos
en cuatro etapas:

1. **Adquisición:** Web Scraping de MedlinePlus para extraer la descripción de cada variable
   clínica (WBC, neutrófilos, PCR) y construir un diccionario de datos trazable.
2. **Almacenamiento:** Carga de las tablas a Google BigQuery (`clinical_dictionary`,
   `appendicitis_raw`, `model_evaluation`) y lectura en memoria vía Apache Arrow.
3. **Análisis:** Análisis exploratorio y validación de calidad con Polars, pruebas no
   paramétricas U de Mann-Whitney con corrección de Holm-Bonferroni y tamaño del efecto.
4. **Modelado:** Comparación de tres escenarios de variables predictoras (Modelos A, B y C),
   evaluados con partición estratificada 80/20 y validación cruzada estratificada de 5 folds.

## Resultados principales

- Se documentaron cuatro variables clínicas provenientes de tres páginas de MedlinePlus.
- Las cuatro variables analizadas (leucocitos, neutrófilos, PCR y temperatura) presentaron
  diferencias estadísticamente significativas entre pacientes con y sin apendicitis.
- Se entrenaron y compararon tres modelos:

| Modelo | Variables | Propósito |
|---|---|---|
| **A** | Clínicas + laboratorio + Alvarado + ultrasonido | Línea base completa |
| **B** | Clínicas + laboratorio + ultrasonido (sin Alvarado) | Eliminar colinealidad con los síntomas |
| **C** | Clínicas + laboratorio (sin Alvarado ni ultrasonido) | Capacidad predictiva sin imagenología |

## Herramientas utilizadas

| Herramienta | Uso en el proyecto |
|---|---|
| Python 3.13 | Lenguaje base |
| requests + BeautifulSoup4 | Web Scraping de MedlinePlus |
| Pandas | Estructuración inicial del diccionario clínico |
| Google Cloud BigQuery | Almacenamiento y consulta de las tablas en la nube |
| Apache Arrow + PyArrow | Transferencia de datos BigQuery → Polars sin copia de memoria |
| Polars | Análisis exploratorio y transformación columnar de alta velocidad |
| SciPy | Prueba U de Mann-Whitney y corrección de Holm |
| Scikit-Learn | Pipelines, preprocesamiento y Regresión Logística |
| Matplotlib + Seaborn | Visualizaciones clínicas y de desempeño |
| Google Colab | Entorno de desarrollo y autenticación |
