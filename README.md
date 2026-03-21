[![LinkedIn](https://img.shields.io/badge/LinkedIn-Connect-blue?logo=linkedin)](https://www.linkedin.com/in/yonathanmontenegro/)
[![GitHub](https://img.shields.io/badge/GitHub-Repo-181717?logo=github&logoColor=white)](https://github.com/yomod290/End_to_End_CRM)
[![Notion Project](https://img.shields.io/badge/Notion-Project-black?logo=notion&logoColor=white)](https://www.notion.so/yonathan-montenegro/End-to-End-para-An-lisis-de-Ventas-CRM-318c42650553800db6f4f16ec875386f)
[![Notion Portafolio](https://img.shields.io/badge/Notion-Portafolio-black?logo=notion&logoColor=white)](https://yonathan-montenegro.notion.site/portafolio-data-engineer)
[![Azure](https://img.shields.io/badge/Azure-Cloud-0078D4?logo=microsoftazure&logoColor=white)](https://azure.microsoft.com/)
[![Synapse SQL](https://img.shields.io/badge/Synapse-SQL%20Pool-0078D4?logo=microsoftazure&logoColor=white)](https://learn.microsoft.com/en-us/azure/synapse-analytics/sql/overview-architecture)
[![Databricks](https://img.shields.io/badge/Databricks-Lakehouse-EF3D2C?logo=databricks&logoColor=white)](https://www.databricks.com/)
[![PySpark](https://img.shields.io/badge/PySpark-Data%20Processing-E25A1C?logo=apachespark&logoColor=white)](https://spark.apache.org/docs/latest/api/python/)

# 📊 Pipeline End-to-End de Ingeniería de Datos para Analítica de Ventas CRM (Azure)

## 📌 Descripción del Proyecto

Este proyecto implementa un **pipeline End-to-End de Ingeniería de Datos en Microsoft Azure** para ingerir, procesar y analizar datos de ventas provenientes de un sistema CRM utilizando una arquitectura moderna **Lakehouse**.

La solución integra varios servicios de Azure para construir una plataforma de datos escalable que soporta:

- ingestión de datos
- transformación de datos
- almacenamiento en Data Lake
- monitoreo de pipelines
- exposición de datos para analítica

El pipeline procesa datos CRM almacenados en archivos **CSV** y los transforma en datasets analíticos estructurados que pueden ser consultados mediante **Azure Synapse Serverless SQL** y visualizados en **Power BI**.

Adicionalmente, la arquitectura incorpora **monitoreo y alertas mediante Azure Logic Apps**, lo que permite mejorar la observabilidad y confiabilidad del sistema.

---

# 🎯 Objetivos del Proyecto

## Objetivo General

Diseñar e implementar un **pipeline de datos escalable en la nube** que permita ingerir, transformar y exponer datos de ventas CRM para análisis de negocio.

## Objetivos Específicos

- Construir un pipeline de ingestión de datos utilizando **Azure Data Factory**
- Almacenar datos en bruto en **Azure Data Lake Storage Gen2**
- Implementar una **arquitectura Medallion (Bronze, Silver, Gold)**
- Realizar transformaciones de datos usando **Azure Databricks con PySpark**
- Almacenar datasets procesados en formato **Delta Lake**
- Exponer datasets analíticos mediante **Azure Synapse Serverless SQL**
- Crear visualizaciones de negocio utilizando **Power BI**
- Implementar monitoreo y alertas con **Azure Logic Apps y Azure Monitor**

---

# 🧠 Problema de Negocio

Muchas organizaciones almacenan datos de CRM en diferentes sistemas y formatos, lo que dificulta realizar análisis centralizados.

Este proyecto simula un escenario real donde los datos de ventas deben:

- ser ingeridos desde archivos en bruto
- ser limpiados y estandarizados
- transformarse en datasets analíticos
- ser consumidos por herramientas de inteligencia de negocio

El objetivo final es permitir **toma de decisiones basada en datos para analizar el rendimiento de ventas y oportunidades comerciales**.

---

# 🏗️ Arquitectura del Proyecto

La solución sigue el patrón moderno **Lakehouse Architecture**.

```
Archivos Fuente (CSV)
        │
        ▼
Azure Data Factory
(Ingestión de datos)
        │
        ▼
Azure Data Lake Storage Gen2
(Almacenamiento Raw)
        │
        ▼
Azure Databricks
(Transformaciones PySpark)
        │
        ▼
Delta Lake Tables
(Bronze → Silver → Gold)
        │
        ▼
Azure Synapse Serverless SQL
        │
        ▼
Power BI
(Dashboard Analítico)
```

El monitoreo y alertas funcionan de la siguiente manera:

```
Azure Monitor
      │
      ▼
Azure Logic Apps
      │
      ▼
Notificaciones por Email
```

---

# 🧰 Tecnologías Utilizadas

| Tecnología | Uso |
|-------------|-----|
| Azure Data Factory | Orquestación e ingestión de datos |
| Azure Data Lake Storage Gen2 | Almacenamiento del Data Lake |
| Azure Databricks | Procesamiento distribuido |
| PySpark | Transformaciones de datos |
| Delta Lake | Formato optimizado de almacenamiento |
| Azure Synapse Serverless SQL | Consulta de datos en el Data Lake |
| Power BI | Visualización y dashboards |
| Azure Logic Apps | Automatización de alertas |
| Azure Monitor | Monitoreo del pipeline |

---

# 📂 Arquitectura de Datos

El proyecto utiliza **Medallion Architecture**, que divide el procesamiento de datos en tres capas.

## Bronze Layer

Capa de ingestión de datos en bruto.

Características:

- almacena los datos tal como llegan
- sin transformaciones complejas
- funciona como fuente de verdad

Ejemplo de estructura:

```
/raw/data/*.csv
```

---

## Silver Layer

Capa de datos limpios y estandarizados.

Transformaciones aplicadas:

- corrección de tipos de datos
- limpieza de registros nulos
- validaciones básicas de calidad
- eliminación de duplicados

---

## Gold Layer

Capa analítica preparada para consumo de negocio.

Ejemplos de datasets generados:

- ventas por agente
- ventas por producto
- ventas por sector
- KPIs del pipeline de ventas
- tendencias mensuales de ventas

Estos datasets se almacenan como **tablas Delta** y se exponen mediante **vistas en Synapse SQL**.

---

# 📊 Datasets Analíticos

El pipeline genera varios datasets orientados al análisis de negocio.

| Dataset | Descripción |
|--------|-------------|
| sales_detail | Detalle completo de oportunidades CRM |
| kpi_by_stage | KPIs por etapa del pipeline |
| sales_by_agent | Rendimiento por agente de ventas |
| sales_by_product | Ventas por producto |
| sales_by_sector | Distribución de ventas por sector |
| monthly_won_trend | Tendencia mensual de oportunidades ganadas |

---

# 🔍 Vistas en Synapse SQL

Los datasets procesados se exponen utilizando **Azure Synapse Serverless SQL Views**.

Ejemplo:

```sql
CREATE OR ALTER VIEW dbo.vw_sales_detail
AS
SELECT *
FROM OPENROWSET(
 BULK 'https://adsldevcrm.dfs.core.windows.net/raw/gold/sales_detail',
 FORMAT = 'DELTA'
) AS rows;
```

Esto permite:

- consultas SQL directamente sobre el Data Lake
- integración con herramientas de BI
- análisis escalable sin mover datos a un Data Warehouse

---

# ⚙️ Monitoreo y Alertas

El proyecto incorpora monitoreo para detectar errores en el pipeline.

Componentes utilizados:

- Azure Monitor
- monitoreo de pipelines
- triggers en Logic Apps
- envío automático de alertas por email

Esto permite detectar fallas rápidamente y mejorar la confiabilidad del sistema.

---

# 📈 Preguntas de Negocio que se Pueden Responder

Los datasets generados permiten responder preguntas como:

- ¿Qué agentes de ventas generan más ingresos?
- ¿Qué productos tienen mayor rendimiento?
- ¿Cómo evoluciona el pipeline de ventas en cada etapa?
- ¿Cuál es la tendencia mensual de oportunidades ganadas?

---

# 🚀 Posibles Mejoras Futuras

Mejoras que podrían implementarse:

- implementar **ingestión incremental de datos**
- agregar **validaciones avanzadas de calidad de datos**
- implementar **CI/CD para despliegue automático**
- aplicar **gobernanza de datos con Unity Catalog**
- incorporar **ingestión en tiempo real con Event Hub**

---

# 👨‍💻 Autor

**Yonathan Montenegro**

Data Engineer  
Azure | Databricks | Synapse | Spark | Data Lake
