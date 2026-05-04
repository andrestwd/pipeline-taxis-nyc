# Elementos Críticos de Datos (CDEs)

Los CDEs son los campos que consideramos más importantes para el negocio.
Si alguno de estos falla o tiene datos incorrectos, los análisis pierden
confiabilidad. Los identificamos basándonos en las reglas de calidad
aplicadas en la capa Trusted.

---

## CDE 1 — fare_amount (tarifa base)

| Atributo | Detalle |
|---|---|
| **Nombre técnico** | `fare_amount` |
| **Tabla** | `nyc_taxi_andres.raw.viajes_enero_2023` |
| **Columna en Trusted** | `nyc_taxi_andres.trusted.viajes_limpios.tarifa_base` |
| **Definición de negocio** | Es el valor que el taxímetro registra por el viaje, sin incluir propinas, peajes ni recargos. Es la base del ingreso del conductor y la métrica central para cualquier análisis financiero. |
| **Regla de calidad** | Debe ser mayor a cero. Una tarifa negativa o en cero indica un error en el sistema de cobro o un viaje no completado. |
| **Impacto si falla** | Los KPIs de ingreso promedio y eficiencia económica por zona quedan distorsionados. En enero 2023, se descartaron 22,522 registros por esta regla, representando parte de los $998,503 en ingresos cuestionables. |

---

## CDE 2 — tpep_pickup_datetime (fecha y hora de inicio)

| Atributo | Detalle |
|---|---|
| **Nombre técnico** | `tpep_pickup_datetime` |
| **Tabla** | `nyc_taxi_andres.raw.viajes_enero_2023` |
| **Columna en Trusted** | `nyc_taxi_andres.trusted.viajes_limpios.fecha_hora_inicio` |
| **Definición de negocio** | Momento exacto en que el pasajero aborda el taxi. Es el ancla temporal de todo el análisis — sin este campo no es posible calcular duración del viaje, franjas horarias ni patrones de demanda. |
| **Regla de calidad** | No puede ser nulo y debe ser estrictamente menor que la fecha de fin del viaje. Si pickup >= dropoff, el registro es inválido. |
| **Impacto si falla** | Sin este campo no se pueden construir los KPIs de demanda temporal. Se descartaron 1,121 registros donde el tiempo de inicio era mayor o igual al de fin. |

---

## CDE 3 — PULocationID (zona de origen)

| Atributo | Detalle |
|---|---|
| **Nombre técnico** | `PULocationID` |
| **Tabla** | `nyc_taxi_andres.raw.viajes_enero_2023` |
| **Columna en Trusted** | `nyc_taxi_andres.trusted.viajes_limpios.id_zona_origen` |
| **Definición de negocio** | Identificador numérico de la zona donde el pasajero abordó el taxi. Permite cruzar cada viaje con su zona y barrio de origen, habilitando el análisis geográfico de demanda y rentabilidad. |
| **Regla de calidad** | No puede ser nulo y debe existir en la tabla de zonas (taxi_zone_lookup). Valores como 264 (Unknown) o 265 (Outside of NYC) se consideran zonas sin información útil para el análisis. |
| **Impacto si falla** | Sin este campo no es posible construir el KPI de eficiencia económica por zona ni identificar los patrones geográficos de demanda. En enero 2023 no se encontraron nulos en esta columna. |

---

*Documentado como parte del pipeline ETL de taxis NYC *