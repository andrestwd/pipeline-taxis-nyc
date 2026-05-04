# Diagrama de Lineaje — Pipeline Taxis NYC

Muestra el flujo completo de los datos desde las fuentes originales
hasta los indicadores de negocio.

## Fuentes → Raw → Trusted → Refined

```mermaid
flowchart TD
    %% fuentes originales
    F1["TLC New York\nyellow_tripdata_2023-01.parquet"]
    F2["TLC New York\ntaxi_zone_lookup.csv"]

    %% capa raw
    R1["raw.viajes_enero_2023\n3,066,766 registros"]
    R2["raw.zonas_taxi\n265 registros"]

    %% capa trusted
    T1["trusted.viajes_limpios\n2,995,263 registros\n71,503 descartados (2.33%)"]

    %% capa refined
    KPI1["refined.kpi_demanda_temporal\nviajes por franja horaria y dia"]
    KPI2["refined.kpi_top10_zonas_rentables\ntop 10 zonas por ingreso promedio"]
    KPI3["refined.kpi_calidad_datos\nimpacto economico de datos invalidos"]
    DQ["refined.reporte_calidad_datos\nregistros que pasan y fallan por regla"]

    %% flujo
    F1 --> R1
    F2 --> R2
    R1 --> T1
    R2 --> T1
    T1 --> KPI1
    T1 --> KPI2
    T1 --> KPI3
    R1 --> DQ

    %% estilos
    style F1 fill:#4a90d9,color:#fff
    style F2 fill:#4a90d9,color:#fff
    style R1 fill:#e8a838,color:#fff
    style R2 fill:#e8a838,color:#fff
    style T1 fill:#5ba85a,color:#fff
    style KPI1 fill:#8e5ea2,color:#fff
    style KPI2 fill:#8e5ea2,color:#fff
    style KPI3 fill:#8e5ea2,color:#fff
    style DQ fill:#8e5ea2,color:#fff
```

## Transformaciones aplicadas en cada capa

**Raw → sin transformaciones**
Los datos llegan tal como están en la fuente. Solo se persisten
en Delta para tener una fuente de verdad inmutable.

**Raw → Trusted**
- 5 reglas de validación aplicadas
- Join con tabla de zonas para enriquecer con barrio y zona de origen
- Renombramiento de columnas al español
- 71,503 registros descartados (2.33% del total)

**Trusted → Refined**
- Agregaciones por franja horaria y día de la semana (KPI 1)
- Ranking de zonas por ingreso promedio por viaje (KPI 2)
- Comparación de ingresos con y sin datos inválidos (KPI 3)
- Reporte formal de calidad con registros que pasan y fallan por regla
