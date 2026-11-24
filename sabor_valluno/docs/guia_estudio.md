# Guía de estudio – Sistema de Pedidos Sabor Valluno (de inicio a fin)

Esta guía es para **entender** el proyecto, no para recitarlo. Si puedes explicar cada sección con tus palabras y rehacer los ejemplos a mano, estás listo para la defensa. Los ejemplos usan la variante A; adapta los números a la tuya (`modelo/variante.py`).

---

## 1. Qué problema resuelve y cómo se resolvió

El restaurante tomaba pedidos en una libreta y fallaba en tres cosas: cobrar combos, cobrar envases y cobrar el envío por zona. La solución es un modelo orientado a objetos donde **cada regla vive en el objeto al que pertenece**:

- cuánto cuesta algo lo sabe el producto,
- cuánto se cobra de más lo sabe el tipo de pedido,
- cómo se paga lo sabe el método de pago.

Esa idea (cada objeto responde por sí mismo) es el polimorfismo, y es lo que más se evalúa.

## 2. Línea de tiempo del proyecto

| Semana | Qué se hizo | IA | Evidencia |
| --- | --- | --- | --- |
| 1 | Diagrama de clases v1 en aula, firmado; acta de consultas iniciada | No | `docs/diagrama_v1.png`, `docs/acta_consultas.md` |
| 2 | Jerarquía `Producto` con `Combo`, propiedades y validaciones, en VS Code | No | `modelo/productos.py`, commits |
| 3 | Jerarquías `Pedido` y `MetodoPago`, estados, bitácora | Sí, declarada | `modelo/pedidos.py`, `modelo/pagos.py`, `docs/bitacora_ia.md` |
| 4 | Integración, menú, pruebas, diagrama v2, README, defensa | Sí, declarada; no en la defensa | `main.py`, `pruebas.py`, `README.md`, `docs/diagrama_v2.png` |

## 3. El modelo, clase por clase

### Productos (`modelo/productos.py`)

- **Producto** (abstracta): `_codigo`, `_nombre`, `_precio`. Métodos abstractos: `calcular_precio()`, `esta_disponible(dia)`, `contar_platos()`, `contar_productos()`. Concretos: `contiene(producto)` y `unidades_gratis(dia, cantidad)`.
- **Plato**: agrega `_dias_venta`. Vale lo que dice su precio. Cuenta como 1 plato. Está disponible solo los días de su lista (en la variante A, el chicharrón solo los domingos).
- **Bebida** (abstracta): agrega `_tamano` (`vaso` o `jarra`). La jarra cuesta 3 veces el vaso. Cuenta como 0 platos. Siempre está disponible.
- **RefrescoNatural** y **Gaseosas**: implementan `calcular_precio()` usando el precio según tamaño. El refresco además sobrescribe `unidades_gratis()` para el 2×1 de la variante B.
- **Combo**: guarda `_productos`, una lista de `Producto`. Su precio es la suma de lo que cuestan sus productos menos el descuento de la variante. Está disponible si **todos** sus productos lo están. Cuenta los platos sumando los de sus productos. No puede contenerse a sí mismo: `agregar_producto` rechaza un producto que sea el mismo combo o que lo contenga a cualquier nivel (para eso existe `contiene()`).

**Composite en una frase:** `Combo` es un `Producto` que contiene `Producto`s, así que el código que trata un producto cualquiera funciona igual para un plato, una bebida o un combo con otros combos adentro.

### Pedidos (`modelo/pedidos.py`)

- **ItemPedido**: un producto con su cantidad (1 a 20) y su subtotal.
- **Pedido** (abstracta): `_numero`, `_cliente`, `_dia`, `_items`, `_estado`, `_registrado_en`, `_entregado_en`. Calcula subtotal, descuentos y total recorriendo los ítems. Métodos abstractos: `calcular_cargos()` y `nombre_tipo()`.
- **PedidoLocal**: agrega `_mesa` (1 a 15). Sin cargos.
- **PedidoParaLlevar**: cargos = cantidad de envases × valor del envase de la variante. Los envases se cuentan con `contar_platos()`, así que cuentan los platos dentro de combos.
- **PedidoDelivery**: agrega `_zona`. Cargos = envío de la zona, o 0 si cumple el envío gratis. Valida el pedido mínimo, rechaza la tarjeta y calcula la compensación por retraso.

**Estados:** REGISTRADO → EN_PREPARACION → LISTO → ENTREGADO. CANCELADO solo desde REGISTRADO. Las transiciones válidas están en el diccionario `TRANSICIONES`. Cualquier otra lanza `ErrorEstadoPedido`.

### Pagos (`modelo/pagos.py`)

- **MetodoPago** (abstracta): `procesar_pago(total)`, `comprobante()`, `permitido_en_delivery()`.
- **Efectivo**: valida que el monto recibido alcance y calcula el cambio.
- **PagoQR**: valida un código de exactamente 10 dígitos.
- **Tarjeta**: suma 3 % de comisión. Su `permitido_en_delivery()` devuelve `False`, y así el delivery la rechaza **sin preguntar el tipo**.

### Resto

- **Cliente**: nombre y teléfono; el teléfono se valida (8 dígitos, empieza en 6 o 7).
- **Restaurante**: guarda menú, clientes y pedidos; crea pedidos, cobra, arma combos y genera el reporte.
- **variante.py**: todos los números que cambian por estudiante. Por eso un cambio de variante no toca las clases.
- **utilidades.py**: las excepciones (`ErrorValidacion`, `ErrorDisponibilidad`, `ErrorEstadoPedido`, `ErrorPago`) y la normalización de los días.

## 4. Los cuatro pilares: dónde están y qué decir

| Pilar | Dónde mirar | Qué explicar |
| --- | --- | --- |
| Encapsulación | `@property` y setters en `Producto`, `ItemPedido`, `Cliente`, `PedidoLocal` | Los atributos son protegidos y los setters validan. `items` devuelve una tupla, así nadie altera la lista desde fuera. |
| Herencia | `class Plato(Producto)`, `super().__init__(...)` | Los atributos comunes están en la base; la subclase solo agrega los suyos. |
| Polimorfismo | `Pedido.calcular_subtotal`, `Combo.calcular_precio`, `PedidoParaLlevar.calcular_cargos` | Misma llamada, comportamiento distinto según el objeto, sin `isinstance` ni `if` por tipo. |
| Abstracción | `class Producto(ABC)` con `@abstractmethod` | Una clase abstracta define un contrato y no se puede instanciar. |

## 5. Cómo se calcula un pedido (traza paso a paso)

**Ejemplo:** pedido para llevar, 1 combo (silpancho + mocochinchi) y 2 piques macho (Bs 55 c/u).

1. Combo: (35 + 6) × 0,90 = 36,90.
2. Subtotal: 36,90 + 2 × 55 = 146,90.
3. Platos: 1 (dentro del combo) + 2 = 3. Envases: 3 × 2 = 6,00.
4. Total: 146,90 + 6,00 = **152,90**.

**Ejemplo de referencia:** 2 combos por delivery a Quillacollo: 2 × 36,90 = 73,80 + 15 = **88,80**.

**Combo dentro de combo:** combo interno 36,90; se une con una limonada de Bs 5: (36,90 + 5) × 0,90 = **37,71**.

**Retraso:** pedido de delivery a Sacaba con 110 de productos y 15 de envío. Si se entrega a los 90 minutos, retraso → compensación 15 → paga 110. Si se entrega a los 60 minutos exactos, no hay retraso (se necesita **más de** 60).

## 6. Trazabilidad: de la regla del enunciado al código, a la prueba

| Regla del enunciado | Clase / método | Prueba (`pruebas.py`) |
| --- | --- | --- |
| Precio mayor que cero | `Producto.precio` (setter) | Sec. 3: precio 0 y negativo |
| Cantidad de 1 a 20 | `ItemPedido.validar_cantidad` | Sec. 3: cantidades 0, 21, 15 + 6 |
| Teléfono de 8 dígitos que empieza en 6 o 7 | `Cliente.validar_telefono` | Sec. 3: teléfonos inválidos y válidos |
| Jarra = 3 × vaso | `Bebida._precio_por_tamano` | Sec. 5: total del pedido local |
| Combo = suma − descuento | `Combo.calcular_precio` | Sec. 1: 36,90 |
| Combo con otro combo | `Combo.calcular_precio` (recursivo) | Sec. 2: 37,71 |
| Combo no se contiene a sí mismo | `Combo.agregar_producto`, `contiene()` | Sec. 2: ciclo directo e indirecto |
| Disponibilidad por día | `Plato.esta_disponible`, `Combo.esta_disponible` | Sec. 4 |
| Local: mesa 1–15, una mesa un pedido abierto | `PedidoLocal.mesa`, `Restaurante.crear_pedido`, `ocupa_mesa()` | Sec. 5 |
| Llevar: envase por plato, incluso en combos | `PedidoParaLlevar.calcular_cargos`, `contar_platos()` | Sec. 6: 4 platos = Bs 8 |
| Delivery: envío por zona | `PedidoDelivery.calcular_cargos`, `ZONAS_DELIVERY` | Sec. 7 |
| Envío gratis (según variante) | `PedidoDelivery.calcular_cargos` | Sec. 7: Cercado sí, Quillacollo no |
| Pedido mínimo del delivery | `PedidoDelivery._validar_confirmacion` | Sec. 7 |
| Estados y cancelación | `Pedido.cambiar_estado`, `TRANSICIONES` | Sec. 8 |
| Solo modificar ítems en REGISTRADO | `Pedido._exigir_registrado` | Sec. 8 |
| Pago: efectivo, QR, tarjeta | `pagos.py` | Sec. 9 |
| Delivery solo efectivo o QR | `permitido_en_delivery()`, `PedidoDelivery._validar_metodo` | Sec. 7 |
| Retraso en delivery (regla 9) | `PedidoDelivery.calcular_compensacion` | Sec. 10 |
| Reporte por tipo y top 3 | `Restaurante.generar_reporte` | Sec. 12 |
| Clases abstractas | `ABC`, `@abstractmethod` | Sec. 11 |
| Reglas de la variante | `modelo/variante.py` | Sec. 13 |

**Trazabilidad de documentos:** diagrama v1 (semana 1) → diagrama v2 (cambios explicados) → código → pruebas. La regla 9 viene del acta de consultas. Los usos de IA están en la bitácora.

## 7. Preguntas que pueden hacerte y cómo pensarlas

- **¿Por qué `Pedido` no usa `isinstance`?** Porque cada producto sabe calcular su precio; el pedido solo le pide `calcular_precio()`. Si agrego un producto nuevo, el pedido no cambia.
- **¿Cómo evitas que un combo se contenga a sí mismo?** `agregar_producto` revisa `producto is self` o `producto.contiene(self)`. `contiene()` es recursivo, así que detecta ciclos indirectos.
- **¿Por qué `items` devuelve una tupla?** Para que nadie modifique la lista desde afuera; solo se cambia con los métodos que validan el estado.
- **¿Por qué la tarjeta se rechaza en delivery sin un `if` por tipo?** Cada método de pago responde `permitido_en_delivery()`: la tarjeta devuelve `False`.
- **¿Cómo se cuentan los envases dentro de un combo?** `contar_platos()` del combo suma los de sus productos, y los combos internos hacen lo mismo.
- **¿De dónde sale este descuento?** De mi variante, en `modelo/variante.py`.
- **¿Por qué se captura esta excepción?** Contesta con el caso concreto: qué línea la lanza y cuándo ocurre.
- **¿Qué pasa si el pago en efectivo es menor al total?** `Efectivo.procesar_pago` lanza `ErrorPago` y el pedido **no** queda marcado como pagado.

## 8. Cambios en vivo típicos

- **Zona nueva:** agregar una línea en `ZONAS_DELIVERY` (`modelo/pedidos.py`). Es un dato, no una clase.
- **Postre** (precio + Bs 3 con crema, no cuenta como plato): una clase que hereda de `Producto`, implementa los cuatro métodos abstractos y devuelve 0 en `contar_platos()`. El código completo está al final de `docs/guion_exposicion.md`.
- **Descuento o envase distinto:** se cambia el valor en `variante.py` y se vuelve a correr `pruebas.py`.

## 9. Cómo correr y verificar

```bash
python3 pruebas.py   # todas las pruebas con assert, imprime cada escenario
python3 main.py      # menú de consola
```

Antes de la defensa: recalcula a mano un pedido con tu variante y compara con el sistema. Si coincide, entiendes la regla.

## 10. Lista de verificación antes de defender

- [ ] Sé explicar cada archivo de `modelo/` con mis palabras.
- [ ] Puedo rehacer a mano el caso de referencia con mi variante.
- [ ] Puedo agregar el `Postre` y la zona Tiquipaya sin mirar.
- [ ] Mi diagrama v2 coincide con los nombres del código (clases, atributos, relaciones).
- [ ] El acta de consultas tiene fechas y respuestas reales del docente.
- [ ] La bitácora de IA incluye todos mis usos, también los de semana 2 si los hubo.
- [ ] Mi historial de commits está distribuido en las semanas.
