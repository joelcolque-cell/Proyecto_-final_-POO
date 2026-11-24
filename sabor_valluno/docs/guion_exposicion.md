# Guion de exposición – Sistema de Pedidos Sabor Valluno

Duración aproximada: 10 minutos. Está pensado para decirse con tus propias palabras: léelo varias veces, cambia lo que no suene a ti y no lo recites de memoria. Los números usan la **variante A**; si la tuya es otra, cámbialos (están en `modelo/variante.py`). Lo marcado con ✏️ lo completas tú.

---

## 1. Presentación (1 min)

Buenos días, profe, buenos días compañeros. Mi nombre es ✏️______ y les voy a presentar mi proyecto integrador de Programación Orientada a Objetos: el sistema de pedidos del restaurante Sabor Valluno.

El problema es este: el restaurante toma los pedidos en una libreta, entonces cobra mal los combos, se olvida del costo de los envases para llevar y no sabe cuánto cobrar de envío según la zona. Mi trabajo fue hacer un sistema de consola en Python que registre el menú, los clientes, los pedidos y los pagos, aplicando los cuatro pilares de la POO. Usé solo la biblioteca estándar, nada externo.

Mi variante es la ✏️__. Eso significa que mis reglas no son las base: por ejemplo, el descuento del combo es de ✏️10 % y el envase para llevar cuesta ✏️Bs 2 por plato.

## 2. El diagrama de clases (2 min)

Empecé en la semana 1 con el diagrama v1, que hice en clase, a mano y sin IA, y el docente lo firmó. Ahí ya estaban las tres jerarquías principales:

- **Producto**, que es abstracta, con `Plato`, `Bebida` y `Combo`. La bebida se divide en `RefrescoNatural` y `Gaseosas`.
- **Pedido**, también abstracta, con `PedidoLocal`, `PedidoParaLlevar` y `PedidoDelivery`.
- **MetodoPago**, abstracta, con `Efectivo`, `PagoQR` y `Tarjeta`.

Además están `Cliente`, `ItemPedido` y la clase gestora `Restaurante`, que es la que junta todo.

La relación más importante es la de **Combo con Producto**. Un combo *es un* producto, pero también *tiene* productos, y esos productos pueden ser otros combos. Eso es el patrón Composite. Por eso en el diagrama el Combo hereda de Producto y además tiene una composición hacia Producto con multiplicidad 2 o más.

En el diagrama v2 ajusté lo que descubrí al programar. ✏️[Cuenta aquí los cambios de tus notas del v2.] Por ejemplo, agregué métodos como `contar_productos()` y `calcular_compensacion()`, porque los necesité para las reglas de mi variante y para el retraso del delivery.

## 3. Los cuatro pilares en mi código (3 min)

**Encapsulación.** Todos los atributos son protegidos, con guion bajo, y se acceden con `@property`. Los setters validan: el precio tiene que ser mayor que cero, la cantidad de un producto va de 1 a 20, el teléfono tiene 8 dígitos y empieza en 6 o 7, la mesa va del 1 al 15. Algo importante: la lista de ítems del pedido no se puede modificar desde afuera. La propiedad `items` devuelve una tupla, así que solo se agrega o quita con `agregar_producto` y `quitar_producto`, y esos métodos revisan que el pedido siga en estado REGISTRADO.

**Herencia.** Cada jerarquía tiene su clase base con los atributos comunes y las subclases llaman a `super().__init__()` y agregan solo lo suyo. Por ejemplo, `PedidoDelivery` agrega la zona y `Tarjeta` agrega los últimos cuatro dígitos. En total son 11 subclases, y ningún atributo común está repetido.

**Polimorfismo.** Acá está lo que más me costó y de lo que estoy más tranquilo. El pedido calcula su subtotal recorriendo los ítems y llamando a `calcular_precio()`, sin preguntar nunca qué tipo de producto es: no hay `isinstance`, ni `type()`, ni `if` por tipo. El `Combo` calcula su precio sumando el `calcular_precio()` de sus productos y aplicando el descuento. Como un combo puede contener otro combo, la llamada es recursiva y funciona sola. Lo mismo pasa con los envases: `contar_platos()` en un plato devuelve 1, en una bebida 0, y en un combo suma los de sus productos. Por eso un plato dentro de un combo también paga envase.

**Abstracción.** `Producto`, `Pedido` y `MetodoPago` heredan de `ABC` y tienen métodos con `@abstractmethod`, por ejemplo `calcular_precio()`, `calcular_cargos()` y `procesar_pago()`. Si intento crear un `Pedido()` directamente, Python lanza `TypeError`. Eso lo compruebo en las pruebas.

## 4. Cómo funciona un pedido y cómo lo probé (2 min)

Les muestro el caso de referencia. Un combo con un silpancho de Bs 35 y un vaso de mocochinchi de Bs 6 suma 41. Con mi descuento de ✏️10 % cuesta **Bs 36,90**. Si pido dos de esos combos por delivery a Quillacollo, son 73,80 de productos más 15 de envío: **Bs 88,80**.

El pedido pasa por estados: REGISTRADO, EN_PREPARACION, LISTO y ENTREGADO. Solo se puede cancelar desde REGISTRADO, y si intento una transición inválida el sistema lanza una excepción. En delivery, si la entrega pasa de 60 minutos desde el registro, el cliente recibe la devolución del costo del envío. Esa regla la consulté con el docente y quedó anotada en el acta de consultas.

Los pagos: en efectivo el sistema valida el monto y calcula el cambio, con QR pide un código de 10 dígitos y con tarjeta suma el 3 % de comisión. El delivery no acepta tarjeta.

Para verificar todo escribí un archivo `pruebas.py` con más de 90 verificaciones con `assert`: el caso de referencia, un combo dentro de otro combo, los errores de validación, los estados, los pagos, el retraso y las reglas de cada variante. Todas pasan.

## 5. Proceso, uso de IA y cierre (1 min)

Trabajé con Git y GitHub desde la semana 1, con commits distribuidos en las semanas ✏️[ajusta a tu historial real]. El diagrama v1 y la jerarquía de productos de la semana 2 ✏️[los hice sin IA / declara lo que corresponda]. En las semanas 3 y 4 usé IA como permite el enunciado, y todo está declarado en mi bitácora: qué le pregunté, qué acepté y qué modifiqué.

Para terminar: lo que más aprendí es que el polimorfismo bien hecho evita condicionales. Cuando quise agregar algo nuevo, como un postre o una zona de delivery, no tuve que tocar `Pedido` ni `Combo`. Muchas gracias, quedo atento a sus preguntas.

---

## Si te piden el cambio en vivo

**Zona nueva (Tiquipaya, Bs 18).** Solo es un dato: en `modelo/pedidos.py`, en el diccionario `ZONAS_DELIVERY`, agrega `"Tiquipaya": 18.0`. No hay que tocar ninguna clase.

**Postre** (precio base más Bs 3 si lleva crema; no cuenta como plato). Una clase nueva en `modelo/productos.py`:

```python
class Postre(Producto):
    def __init__(self, codigo, nombre, precio, lleva_crema=False):
        super().__init__(codigo, nombre, precio)
        self._lleva_crema = lleva_crema

    def calcular_precio(self):
        return round(self._precio + (3 if self._lleva_crema else 0), 2)

    def esta_disponible(self, dia):
        normalizar_dia(dia)
        return True

    def contar_platos(self):      # no cuenta como plato: sin envase
        return 0

    def contar_productos(self):
        return 1
```

Lo que dices: "Heredo de Producto, implemento los cuatro métodos abstractos y listo. `Pedido` y `Combo` no cambian porque solo llaman a `calcular_precio()` y `contar_platos()`."

**Pregunta de traza** (combo dentro de combo, variante A): combo interno = (35 + 6) × 0,90 = 36,90. Combo externo = (36,90 + 5) × 0,90 = 37,71. Rehazlo a mano con los números de tu variante.
