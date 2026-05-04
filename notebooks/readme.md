# Notebooks del pipeline

# Pipeline de Taxis NYC — Arquitectura Medallion

Pipeline ETL construido sobre Databricks con arquitectura Medallion
(Raw → Trusted → Refined), usando el dataset público de taxis amarillos
de Nueva York (enero 2023). Desarrollado como prueba técnica para el rol de 
Ingeniero de Datos.

---

## Función del pipeline

Toma los datos crudos de viajes en taxi de NYC, los limpia y los valida,
los enriquece con información geográfica de zonas y genera tres
indicadores de negocio:

- **KPI 1** — Patrón de demanda: cuándo y cuánto se viaja por franja
  horaria y día de la semana
- **KPI 2** — Eficiencia económica: qué zonas generan más ingreso
  por viaje para el conductor
- **KPI 3** — Impacto de calidad: cuánto dinero representan los
  registros con datos inválidos

---

## Stack utilizado

- Azure Databricks (Free Edition — Serverless)
- PySpark + Delta Lake
- Unity Catalog
- Python 3
- Git/Github

---

## Estructura del proyecto
```
pipeline-taxis-nyc/
├── notebooks/
│   ├── 00_setup_catalogo.py     # crea el catálogo y schemas en Unity Catalog
│   ├── 01_ingesta_raw.py        # descarga y persiste los datos en capa Raw
│   ├── 02_limpieza_trusted.py   # limpieza, validación y enriquecimiento
│   ├── 03_kpis_refined.py       # generación de los tres KPIs de negocio
│   ├── 04_calidad_datos.py      # reporte formal de calidad de datos
│   └── 05_reporte_ejecucion.py  # reporte JSON del pipeline completo
├── docs/
│   ├── cdes.md                  # elementos críticos de datos documentados
│   └── glosario.md              # glosario de términos de negocio
├── lineaje/
│   └── diagrama_lineaje.md      # diagrama Mermaid del flujo de datos
├── reportes/
│   └── reporte_ejecucion.json   # reporte de ejecución del pipeline
└── README.md
```


---

## Cómo ejecutarlo paso a paso

### Requisitos previos
- Databricks
- Unity Catalog habilitado en el workspace

### Paso 1 — Setup inicial
Abre y corre el notebook `00_setup_catalogo`. Este paso solo se
hace una vez — crea el catálogo `nyc_taxi_andres` con los tres
schemas (raw, trusted, refined) y el volumen para los archivos fuente.

### Paso 2 — Ingesta Raw
Corre el notebook `01_ingesta_raw`. Descarga los archivos desde
la fuente oficial del TLC de Nueva York y los persiste en Delta
sin ninguna transformación.

- Fuente viajes: `https://d37ci6vzurychx.cloudfront.net/trip-data/yellow_tripdata_2023-01.parquet`
- Fuente zonas: `https://d37ci6vzurychx.cloudfront.net/misc/taxi_zone_lookup.csv`

### Paso 3 — Limpieza Trusted
Corre el notebook `02_limpieza_trusted`. Aplica las reglas de
validación, enriquece los viajes con información de zonas y
estandariza los nombres de columnas al español.

### Paso 4 — KPIs Refined
Corre el notebook `03_kpis_refined`. Genera los tres KPIs de
negocio y los persiste en la capa Refined del catálogo.

### Paso 5 — Calidad de datos
Corre el notebook `04_calidad_datos`. Genera el reporte formal
de calidad con registros que pasan y fallan por cada regla.

### Paso 6 — Reporte de ejecución
Corre el notebook `05_reporte_ejecucion`. Genera el resumen
completo del pipeline en formato JSON.

---

## Decisiones técnicas clave

**¿Por qué se descartan solo el 2.33% de los registros?**
El dataset del TLC no está tan sucio. La mayoría de
los descartados corresponden a viajes con distancia cero, que
pueden ser cancelaciones o errores del medidor al arrancar.

**¿Por qué el ranking de zonas usa ingreso promedio y no ingreso por milla?**
El ingreso por milla se distorsiona en viajes con tarifa fija
como los de aeropuerto — distancia corta registrada pero tarifa
alta. El ingreso promedio por viaje es más estable y tiene mejor representacion
del negocio real.

**¿Por qué se filtra duración menor a 1 minuto solo en el KPI de velocidad?**
Esos viajes son transacciones reales que generaron cobro, por eso
no se descartan en Trusted. Pero sí distorsionan el cálculo de
velocidad, así que los excluimos solo de ese cálculo específico.

---

## Resultados principales

| Métrica | Valor |
|---|---|
| Viajes procesados en Raw | 3,066,766 |
| Viajes válidos en Trusted | 2,995,263 |
| Registros descartados | 71,503 (2.33%) |
| Ingresos con datos limpios | $81,866,688 |
| Ingresos en datos crudos | $82,865,192 |
| Impacto de datos inválidos | $998,503 (1.2%) |

---

## Algunas limitaciones conocidas

- El pipeline procesa solo enero 2023. Para múltiples meses
  se necesitaría parametrizar la fecha y usar AutoLoader para
  ingesta incremental.
- Unity Catalog en Free Edition tiene limitaciones de permisos
  comparado con una implementación completa en Azure.
- El linaje automático de Unity Catalog no está disponible en
  esta versión, por eso se documenta manualmente con Mermaid.

## Cómo evolucionaría esto en producción

- **Orquestación** — Databricks Workflows o Apache Airflow para
  automatizar la ejecución de cada capa
- **Ingesta incremental** — AutoLoader para procesar solo los
  archivos nuevos en lugar de cargar todo cada vez
- **Calidad de datos** — Great Expectations integrado en el pipeline
- **Serving** — SQL Warehouse conectado a Power BI para consumo
  de los KPIs por el negocio

---

*Desarrollado por Andres Agudelo — abril 2026*
