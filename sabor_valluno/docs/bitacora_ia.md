# Bitácora de uso de IA – Sistema de Pedidos Sabor Valluno

**Estudiante:** ______________________  **Variante:** ____

**Herramienta usada:** Claude (Anthropic), chat web/app.

Declarar el uso de IA no baja la nota; ocultarlo sí. Cada entrada completa los campos del modelo de registro personal (sección 8 del enunciado). Los campos marcados con ✏️ los completa el estudiante con lo que realmente hizo.

## 1. Uso de IA por fase

| Semana | Fase | Uso de IA según el enunciado | Uso real declarado |
| --- | --- | --- | --- |
| 1 | Modelado en aula | No permitido | ✏️ Ninguno / ______ |
| 2 | Productos y encapsulación (VS Code) | No permitido | ✏️ Ninguno / ______ |
| 3 | Pedidos, polimorfismo y pagos | Permitido y declarado | Sí. Ver entradas 1 a 3 |
| 4 | Integración y defensa | Permitido y declarado; prohibido en la defensa | Sí en la integración (entradas 4 a 6). **Ninguno durante la defensa.** |

> **Importante:** en esta consulta Claude también generó `modelo/productos.py` (jerarquía `Producto` con `Combo`) y `modelo/clientes.py`, que corresponden al alcance de la **semana 2**, donde la IA no está permitida. Si entregaste en la semana 2 tu propio código de esos archivos, indícalo arriba y en la entrada 1 (qué parte es tuya y qué parte tomaste de la IA). Si en cambio usaste el código generado para esa fase, debes declararlo así: ocultarlo anula el criterio de proceso.

## 2. Registro de usos

### Entrada 1 – Generación del código del modelo

| Campo | Detalle |
| --- | --- |
| Fecha | 23/11/2026 |
| Semana | ✏️ ____ |
| Herramienta | Claude |
| Tipo de uso | Generación de código |
| Consulta textual | "Con base en este diagrama v2 y en las reglas de negocio del documento que te pasé anteriormente, necesito que hagas lo siguiente: 1. Implementación en Python: Escribe el código completo respetando estrictamente los nombres exactos de clases, atributos protegidos, tipos de datos y firmas de métodos del diagrama…" (se adjuntó el diagrama v2 y el enunciado) |
| Qué acepté | Estructura de clases y jerarquías (`Producto`, `Pedido`, `MetodoPago`), propiedades con validación, estados del pedido, cargos, pagos, compensación por retraso, `Restaurante`. |
| Qué modifiqué y por qué | ✏️ ______________________________ |
| Archivo o clase | `modelo/productos.py`, `modelo/pedidos.py`, `modelo/pagos.py`, `modelo/clientes.py`, `modelo/variante.py`, `modelo/utilidades.py`, `restaurante.py` |
| ¿Lo explico sin ayuda? (Sí / Parcial / No) | ✏️ ____ |

Notas verificables: la IA agregó métodos que no estaban en el diagrama (`contar_productos()`, `contiene()`, `unidades_gratis()`, `calcular_descuentos()`, `calcular_compensacion()`, `calcular_total_a_pagar()`, `ocupa_mesa()`, `permitido_en_delivery()`); deben figurar en el diagrama v2. Los parámetros de las seis variantes quedaron en `modelo/variante.py`; la variante activa por defecto es la A hasta que se configure la propia.

### Entrada 2 – Pruebas y menú de consola

| Campo | Detalle |
| --- | --- |
| Fecha | 23/11/2026 |
| Semana | ✏️ ____ |
| Herramienta | Claude |
| Tipo de uso | Generación de código |
| Consulta textual | "Crea un bloque de pruebas (o archivo main.py) que ejecute paso a paso todos los casos de uso y escenarios de error que están en el documento, imprimiendo en consola los resultados, totales, comprobantes y el reporte final…" |
| Qué acepté | `pruebas.py` con verificaciones `assert` (caso de referencia, combo dentro de combo, errores de validación, estados, pagos, retraso, variantes) y `main.py` con el menú de consola. |
| Qué modifiqué y por qué | ✏️ ______________________________ |
| Archivo o clase | `pruebas.py`, `main.py` |
| ¿Lo explico sin ayuda? | ✏️ ____ |

### Entrada 3 – Revisión de resultados

| Campo | Detalle |
| --- | --- |
| Fecha | 23/11/2026 |
| Semana | ✏️ ____ |
| Herramienta | Claude |
| Tipo de uso | Error / revisión |
| Consulta textual | (Claude ejecutó las pruebas y corrigió por su cuenta los fallos que aparecieron, por ejemplo un caso de prueba de retraso a los 60 minutos exactos que dependía del reloj.) |
| Qué acepté | Las correcciones a las pruebas y a las etiquetas del detalle del pedido. |
| Qué modifiqué y por qué | ✏️ ______________________________ |
| Archivo o clase | `pruebas.py`, `modelo/pedidos.py` |
| ¿Lo explico sin ayuda? | ✏️ ____ |

### Entrada 4 – README

| Campo | Detalle |
| --- | --- |
| Fecha | 23/11/2026 |
| Semana | ✏️ 4 |
| Herramienta | Claude |
| Tipo de uso | Generación de documentación |
| Consulta textual | "puedes entregarme el README" y luego "hazme el README tomando en cuenta esto" (con una imagen de estructura recomendada de README) |
| Qué acepté | Estructura de 10 secciones, tabla de pilares, supuestos de diseño, estructura del proyecto. |
| Qué modifiqué y por qué | ✏️ Datos personales, URL del repositorio, licencia, notas del diagrama v2. |
| Archivo o clase | `README.md` |
| ¿Lo explico sin ayuda? | ✏️ ____ |

### Entrada 5 – Acta de consultas

| Campo | Detalle |
| --- | --- |
| Fecha | 23/11/2026 |
| Semana | ✏️ 4 |
| Herramienta | Claude |
| Tipo de uso | Generación de documentación |
| Consulta textual | "deme un acta de colsulta del proyecto" |
| Qué acepté | La plantilla con siete preguntas surgidas de las dudas de implementación. Las fechas y respuestas del docente se dejaron en blanco; la IA no las inventó. |
| Qué modifiqué y por qué | ✏️ Fechas y respuestas reales del docente; consultas borradas si no se hicieron. |
| Archivo o clase | `docs/acta_consultas.md` |
| ¿Lo explico sin ayuda? | ✏️ ____ |

### Entrada 6 – Bitácora de IA

| Campo | Detalle |
| --- | --- |
| Fecha | 23/11/2026 |
| Semana | ✏️ 4 |
| Herramienta | Claude |
| Tipo de uso | Generación de documentación |
| Consulta textual | "dame una bitacora de ia, que solo lo usamos la ia como indica ahi" (con una imagen de la tabla de entregas por semana) |
| Qué acepté | Esta plantilla. |
| Qué modifiqué y por qué | ✏️ Completar con los usos reales, incluidos los que no figuren aquí. |
| Archivo o clase | `docs/bitacora_ia.md` |
| ¿Lo explico sin ayuda? | ✏️ ____ |

### Entrada 7 – Guion de exposición y guía de estudio

| Campo | Detalle |
| --- | --- |
| Fecha | 23/11/2026 |
| Semana | ✏️ 4 |
| Herramienta | Claude |
| Tipo de uso | Concepto / generación de documentación |
| Consulta textual | "dame un texto uno para exponer todo el proyecto bien como si fueras un humano estudiante de sistemas de segundo semestre y dame otro texto para entender el proyecto de inicio a fin su trazabilidad todo" |
| Qué acepté | Un guion de exposición y una guía de estudio con la trazabilidad regla → código → prueba. |
| Qué modifiqué y por qué | ✏️ Adapté el guion a mis palabras, mi variante y mi historial real. |
| Archivo o clase | `docs/guion_exposicion.md`, `docs/guia_estudio.md` |
| ¿Lo explico sin ayuda? | ✏️ ____ |

### Entradas adicionales

Agrega aquí cada nueva consulta a la IA, aunque sea breve, con el mismo formato.

## 3. Declaración

Marca solo lo que sea verdad:

- [ ] No usé IA en la semana 1 (modelado en aula, diagrama v1).
- [ ] No usé IA en la semana 2 (jerarquía `Producto` y `Combo`, escrita en VS Code).
- [ ] Usé IA en las semanas 3 y 4 solo como se permite, y lo registré en esta bitácora.
- [ ] No usaré IA durante la defensa.
- [ ] Este registro incluye todos mis usos de IA en el proyecto y puedo explicar el código que entrego.

Firma: ______________________  Fecha: ____/____/2026
