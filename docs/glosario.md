# Glosario de Negocio

Definiciones de los términos clave usados en el pipeline de taxis NYC.
Escritas para que cualquier persona del negocio las entienda, 
sin necesidad de ser ingeniero de datos.

---

## Viaje válido

Un viaje válido es aquel que cumple todas las condiciones mínimas para
ser considerado real: tiene una distancia recorrida mayor a cero, una
tarifa positiva, y el tiempo de llegada es posterior al de salida.
Los viajes que no cumplen estas condiciones se descartan en la capa
Trusted y no se usan en ningún análisis.

---

## Hora pico

Franja horaria entre las 5:00 PM y las 7:59 PM de lunes a viernes,
donde se concentra la mayor demanda de taxis en NYC. Durante esta
franja los tiempos de espera aumentan y las tarifas pueden incluir
recargos adicionales. Es el período más importante para analizar
la capacidad y eficiencia del servicio.

---

## Borough

División administrativa de la ciudad de Nueva York. Existen cinco:
Manhattan, Brooklyn, Queens, Bronx y Staten Island. Cada zona de
taxi pertenece a un borough, lo que permite agrupar y comparar
el comportamiento de la demanda a nivel geográfico más amplio que
una zona individual.

---

## Ingreso por milla

Métrica que indica cuánto dinero genera un viaje por cada milla
recorrida. Se calcula dividiendo el total cobrado entre la distancia
del viaje. Es útil para comparar la rentabilidad entre zonas,
pero puede distorsionarse en viajes muy cortos con tarifa fija
como los viajes a aeropuertos.

---

## Tarifa base

Es el valor que registra el taxímetro por el servicio de transporte,
sin incluir propinas, peajes, recargos por congestión ni tarifas
de aeropuerto. Representa el ingreso mínimo garantizado del conductor
por cada viaje y es el campo central para cualquier análisis
financiero del negocio.

---

*Glosario construido como parte del pipeline ETL de taxis NYC*
