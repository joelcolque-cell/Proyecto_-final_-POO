# Acta de consultas – Sistema de Pedidos Sabor Valluno

**Estudiante:** ______________________  **Variante:** ____
**Cliente (docente):** ______________________

Toda duda sobre las reglas de negocio se consulta al cliente y se registra aquí con fecha, pregunta y respuesta. No se implementan reglas supuestas sin consulta.

> **Cómo usar esta acta:** las preguntas de abajo salen de las dudas reales que aparecieron al implementar el sistema. Antes de entregarla, haz cada consulta al docente y escribe la fecha y su respuesta textual. Si el docente responde algo distinto a lo que implementé, ajusta el código y anótalo en la columna "Cómo quedó implementado". Si una consulta no se hizo, bórrala del acta: no se registran consultas que no ocurrieron.

---

## Consulta 1 – Retraso en delivery (regla 9)

| Campo | Detalle |
| --- | --- |
| Fecha | ____/____/2026 |
| Pregunta | ¿Cuándo se considera que un pedido de delivery se entregó con retraso y cuál es la compensación para el cliente? |
| Respuesta del docente | |
| Cómo quedó implementado | Hay retraso si `_entregado_en − _registrado_en` supera **60 minutos**; la compensación es la **devolución del costo de envío** (`PedidoDelivery.calcular_compensacion()`). Si el envío ya era gratis, la compensación es Bs 0. *Confirmar que coincide con la respuesta.* |
| Pruebas | `pruebas.py`, sección 10 (90 min → compensa; 60 min exactos → no compensa) |

## Consulta 2 – Momento del cobro

| Campo | Detalle |
| --- | --- |
| Fecha | ____/____/2026 |
| Pregunta | ¿En qué estado se cobra un pedido? ¿Solo cuando está ENTREGADO, o también en LISTO? |
| Respuesta del docente | |
| Cómo quedó implementado | Solo se cobra en estado `ENTREGADO`; así el retraso del delivery ya se conoce al cobrar. Un pedido cancelado, sin entregar o ya pagado lanza `ErrorPago`. |

## Consulta 3 – Base de cálculo del pedido mínimo y del envío gratis

| Campo | Detalle |
| --- | --- |
| Fecha | ____/____/2026 |
| Pregunta | El pedido mínimo del delivery y el umbral de envío gratis, ¿se calculan sobre el subtotal antes de descuentos o después? |
| Respuesta del docente | |
| Cómo quedó implementado | Sobre el subtotal **antes** de descuentos (`calcular_subtotal()`). El mínimo se valida al pasar de REGISTRADO a EN_PREPARACION. |

## Consulta 4 – Variante F: "5 % más Bs 5 fijos" *(solo si te tocó la F)*

| Campo | Detalle |
| --- | --- |
| Fecha | ____/____/2026 |
| Pregunta | En el descuento del combo "5 % más Bs 5 fijos", ¿se descuenta el 5 % y además Bs 5 al precio del combo? ¿Se aplica también a los combos dentro de otros combos? |
| Respuesta del docente | |
| Cómo quedó implementado | `precio = suma × 0,95 − 5`, nunca menor a 0, en cada combo (también en los anidados). |

## Consulta 5 – Variante D: "3 productos o más" *(solo si te tocó la D)*

| Campo | Detalle |
| --- | --- |
| Fecha | ____/____/2026 |
| Pregunta | El descuento del 15 % exige 3 productos o más: ¿se cuentan los productos directos del combo o también los de los combos que contiene? |
| Respuesta del docente | |
| Cómo quedó implementado | Se cuentan los productos directos del combo (`len(_productos)`). |

## Consulta 6 – Reporte de productos más vendidos

| Campo | Detalle |
| --- | --- |
| Fecha | ____/____/2026 |
| Pregunta | En el top 3 de productos más vendidos, ¿un combo cuenta como un producto o se descompone en sus productos? |
| Respuesta del docente | |
| Cómo quedó implementado | Se cuentan las unidades de cada ítem del pedido: un combo cuenta como un producto. |

## Consulta 7 – Variante: regla adicional

| Campo | Detalle |
| --- | --- |
| Fecha | ____/____/2026 |
| Pregunta | *(Escribe aquí cualquier duda sobre tu regla adicional, por ejemplo: en B, ¿el 2×1 del martes aplica también a las jarras? En E, ¿el redondeo aplica antes o después de la compensación por retraso?)* |
| Respuesta del docente | |
| Cómo quedó implementado | |

---

## Firma

Declaro que este registro incluye todas las consultas realizadas al cliente y que las respuestas son las recibidas.

Estudiante: ______________________  Docente: ______________________
