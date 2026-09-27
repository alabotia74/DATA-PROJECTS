# ETL — Data Warehouse de Seguros de Salud

Proceso ETL completo en PySpark que extrae datos transaccionales de un sistema de seguros de salud y los transforma en un **data warehouse dimensional (modelo estrella)**, aplicando manejo de dimensiones de cambio lento (SCD tipo 2).

## Qué hace

- **Extracción**: lectura de tablas fuente desde una base de datos transaccional MySQL vía JDBC (áreas de servicio, planes de beneficio, condiciones de pago, tipos de beneficio, geografía).
- **Transformación**:
  - Limpieza y normalización de texto (trim, colapso de espacios, estandarización de valores).
  - Deduplicación mediante funciones de ventana.
  - Validación de integridad referencial entre dimensiones (`left_semi join`) para conservar solo registros usados.
  - Generación de llaves sustitutas (*surrogate keys*) y construcción de una dimensión de fecha (`DimFecha`) con atributos derivados (trimestre, semestre, inicio de año).
  - Manejo de historicidad tipo **SCD 2**: control de vigencia de cada registro con `FechaDesde`, `FechaHasta` y `EsActual`, cerrando versiones anteriores cuando cambia algún atributo.
  - Validaciones de calidad de datos (unicidad, conteo de IDs repetidos, coherencia de rangos de fecha).
- **Carga**: escritura por lotes (`batch`) hacia el data warehouse en MySQL, tanto de las tablas de dimensión (`DimAreaServicio`, `DimGeografia`, `DimFecha`, `DimCondicionPago`, `DimTipoBeneficio`, `DimPlan`, `DimProveedor`) como de las tablas de hechos históricos y de hechos principales (`HechoPlanesTiposBeneficio`).

## Modelo de datos

El resultado es un esquema estrella típico de un data warehouse de seguros de salud, con una tabla de hechos central relacionada a dimensiones de plan, beneficio, condición de pago, área de servicio, proveedor y fecha — apto para análisis de indicadores de cobertura, vigencia de planes y seguimiento histórico de cambios normativos/operativos.

## Stack técnico

- **Python + PySpark** (procesamiento distribuido, `SparkSession`, funciones de ventana)
- **SQL / MySQL** (origen y destino, vía conector JDBC)
- Control de calidad de datos y trazabilidad histórica (auditoría de cambios)

## Nota

Algunos atributos de las dimensiones `Plan` y `Proveedor` se generaron de forma sintética para efectos del ejercicio académico, dado que no formaban parte del dataset fuente disponible.

> ⚠️ Antes de ejecutar este notebook, reemplaza las credenciales de conexión (`db_user`, `db_psswd`, connection strings) por variables de entorno o un archivo de configuración no versionado.
