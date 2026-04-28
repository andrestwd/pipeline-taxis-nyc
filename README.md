# Pipeline de Taxis NYC — Arquitectura Medallion

Pipeline ETL construido sobre Azure Databricks con arquitectura Medallion 
(Raw → Trusted → Refined), usando el dataset público de taxis amarillos de 
Nueva York (enero 2023).

## ¿Qué hace este pipeline?

Toma los datos crudos de viajes en taxi de NYC, los limpia, los enriquece 
con información de zonas geográficas y genera indicadores de negocio sobre 
patrones de demanda y eficiencia económica por zona.

## Stack utilizado

- Azure Databricks (Community Edition)
- PySpark + Delta Lake
- Unity Catalog
- Python 3

## Estructura del proyecto
pipeline-taxis-nyc/
├── notebooks/       # notebooks de cada capa del pipeline
├── docs/            # documentación de gobierno de datos
├── reportes/        # reporte de ejecución en JSON
└── lineaje/         # diagrama de linaje del pipeline

## Cómo ejecutarlo

*(se completa al finalizar el desarrollo)*

## Decisiones técnicas

*(se documenta durante el desarrollo)*
