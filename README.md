# Reto #07 


## Punto 1 (Gestión de Pedidos con una Cola FIFO):


- En esta parte del codigo implementa una estructura de datos de tipo cola FIFO **(First-In, First-Out)** para gestionar múltiples pedidos de manera eficiente. La cola FIFO asegura que el primer pedido que entra es el primero en ser procesado, lo cual es útil en situaciones donde el orden de llegada es importante.

### Clase `FIFOQueue`

La clase `FIFOQueue` tiene los siguientes métodos:

- **`__init__(self)`**: Inicializa una cola vacía.
- **`agregar_elemento(self, element)`**: Agrega un elemento al final de la cola.
- **`eliminar_elemento(self)`**: Elimina y retorna el primer elemento de la cola. Si la cola está vacía, retorna `None`.
- **`esta_vacia(self)`**: Retorna `True` si la cola está vacía, de lo contrario retorna `False`.

### Código de la Clase `FIFOQueue`

```python
class FIFOQueue:
    def __init__(self):
        self.items = []

    def agregar_elemento(self, element):
        self.items.append(element)

    def eliminar_elemento(self):
        if not self.esta_vacia():
            return self.items.pop(0)

    def esta_vacia(self):
        return len(self.items) == 0
```
#
## Punto 2 (# Definición de un Menú usando `namedtuple`)

- En esta parte se utiliza la estructura de datos `namedtuple` de Python para definir un conjunto de elementos en un menú. Un `namedtuple` es una subclase de tupla que permite acceder a los campos por nombre.

### Estructura del Menú

El menú está compuesto por diferentes categorías de productos, como bebidas, platos principales, postres, ensaladas, sopas y desayunos. Cada producto se define utilizando un `namedtuple` llamado `Producto`, que contiene los siguientes campos:

- **`nombre`**: El nombre del producto.
- **`precio`**: El precio del producto en pesos colombianos (COP).
- **`tipo_caracteristicas`**: El tipo de característica del producto (por ejemplo, "ml" para mililitros, "g" para gramos, "ingredientes" para listas de ingredientes, etc.).
- **`caracteristicas`**: Las características específicas del producto (por ejemplo, la cantidad en mililitros, gramos, o una lista de ingredientes).

### Código de la Definición del Menú

```python
from collections import namedtuple

# Definición del namedtuple "Producto"
Producto = namedtuple("Producto", ["nombre", "precio", "tipo_caracteristicas", "caracteristicas"])

# Bebidas
cafe = Producto("Café", 2000, "ml", 200)  # 200 ml
jugo_lulo = Producto("Jugo de Lulo", 2500, "ml", 250)  # 250 ml
agua_panela = Producto("Agua de panela", 1500, "ml", 300)  # 300 ml

# Platos principales
arepas = Producto("Arepa con queso", 8000, "ingredientes", ["arepa", "queso"])
bandeja_paisa = Producto("Bandeja Paisa", 25000, "ingredientes", ["arroz", "frijoles", "huevo", "carne", "chicharrón", "aguacate"])
sancocho = Producto("Sancocho de gallina", 15000, "ingredientes", ["gallina", "yuca", "papas", "plátano"])
ajiaco = Producto("Ajiaco", 18000, "ingredientes", ["pollo", "papas", "mazorca", "guasca"])
empanadas = Producto("Empanadas", 3500, "ingredientes", ["harina de maíz", "carne", "papas"])
bandeja_pescado = Producto("Bandeja De Pescado", 22000, "ingredientes", ["arroz", "pescado frito", "patacones", "ensalada", "aguacate"])
tamal = Producto("Tamales", 7000, "ingredientes", ["masa de maíz", "carne", "pollo", "papa", "zanahoria"])

# Postres
arroz_con_leche = Producto("Arroz con Leche", 3500, "g", 200)  # 200 g
tres_leches = Producto("Pastel Tres Leches", 5000, "g", 180)  # 180 g
torta_de_guanabana = Producto("Torta de Guanábana", 4500, "g", 150)  # 150 g

# Ensaladas
ensalada_mixta = Producto("Ensalada Mixta", 4000, "tazón", 1)  # 1 tazón
ensalada_de_pasta = Producto("Ensalada de Pasta", 5000, "plato", 1)  # 1 plato

# Sopas
sopa_de_lentejas = Producto("Sopa de Lentejas", 6000, "temperatura", 1)  # Caliente
sopa_de_carne = Producto("Sopa de Carne", 8000, "temperatura", 1)  # Caliente
sopa_de_vegetales = Producto("Sopa de Vegetales", 7000, "temperatura", 1)  # Caliente

# Desayunos
arepas_con_huevo = Producto("Arepas con Huevo", 5000, "unidades", 2)  # 2 unidades
calentado = Producto("Calentado", 7000, "porción", 1)  # 1 porción
empanadas_con_aji = Producto("Empanadas con Aji", 4500, "unidades", 3)  # 3 unidades
```

## Punto 3 (Gestión de Menús con Almacenamiento en JSON)

- Se implementa una clase `Menu` que permite gestionar múltiples menús, incluyendo la creación de nuevos menús, la adición, actualización y eliminación de elementos. Todos los menús se almacenan en archivos JSON, lo que facilita su persistencia y recuperación.


### Clase `Menu`

La clase `Menu` tiene los siguientes métodos:

- **`__init__(self, menu_file="menu.json")`**: Inicializa la clase cargando el menú desde un archivo JSON. Si el archivo no existe, se crea un menú vacío.
- **`load_menu(self)`**: Carga el menú desde el archivo JSON especificado.
- **`guardar_menu(self)`**: Guarda el menú actual en el archivo JSON.
- **`crear_menu(self, nombre_menu)`**: Crea un nuevo menú con el nombre especificado.
- **`agregar_elemento(self, nombre_menu, producto, categoria="MainDishes")`**: Agrega un elemento al menú especificado.
- **`actualizar_menu(self, nombre_menu, nombre_elemento, nuevo_precio)`**: Actualiza el precio de un elemento en el menú especificado.
- **`eliminar_elemento(self, nombre_menu, nombre_elemento)`**: Elimina un elemento del menú especificado.
- **`ver_menu(self, nombre_menu)`**: Muestra los elementos del menú especificado.

### Código de la Clase `Menu`

```python
import json
from collections import namedtuple

# Definición del namedtuple "Producto"
Producto = namedtuple("Producto", ["nombre", "precio", "tipo_caracteristicas", "caracteristicas"])

class Menu:
    def __init__(self, menu_file="menu.json"):
        self.menu_file = menu_file
        self.menu = self.load_menu()

    def load_menu(self):
        try:
            with open(self.menu_file, "r") as file:
                data = json.load(file)
                # Si el archivo es una lista, asumimos que es un menú único
                if isinstance(data, list):
                    return {"Menú Principal": [MenuItem.from_dict(item) for item in data]}
                # Si el archivo es un diccionario, lo cargamos como está
                elif isinstance(data, dict):
                    return {nombre_menu: [MenuItem.from_dict(item) for item in items] for nombre_menu, items in data.items()}
                else:
                    print("Formato de archivo no válido.")
                    return {}
        except (FileNotFoundError, json.JSONDecodeError):
            print("Error al cargar el archivo. Creando un menú vacío.")
            return {}

    def guardar_menu(self):
        if not self.menu:
            print("El menú está vacío, creando estructura inicial...")
            self.menu = {}

        with open(self.menu_file, "w") as file:
            data = {nombre_menu: [item.to_dict() for item in items] for nombre_menu, items in self.menu.items()}
            json.dump(data, file, indent=4)

    def crear_menu(self, nombre_menu):
        if nombre_menu in self.menu:
            print(f"El menú '{nombre_menu}' ya existe.")
        else:
            self.menu[nombre_menu] = []
            self.guardar_menu()
            print(f"Menú '{nombre_menu}' creado.")

    def agregar_elemento(self, nombre_menu: str, producto: Producto, categoria="MainDishes"):
        if nombre_menu not in self.menu:
            print(f"El menú '{nombre_menu}' no existe.")
            return None

        categorias = {"Drink": Drink, "MainDishes": MainDishes, "Dessert": Dessert, "Salads": Salads, "Soups": Soups, "Breakfast": Breakfast}
        item_class = categorias.get(categoria, MainDishes)
        self.menu[nombre_menu].append(item_class(producto))
        self.guardar_menu()
        print(f"Elemento '{producto.nombre}' agregado al menú '{nombre_menu}'.")

    def actualizar_menu(self, nombre_menu, nombre_elemento, nuevo_precio):
        if nombre_menu not in self.menu:
            print(f"El menú '{nombre_menu}' no existe.")
            return None

        for item in self.menu[nombre_menu]:
            if item._name == nombre_elemento:
                item._price = nuevo_precio
                self.guardar_menu()
                print(f"Elemento '{nombre_elemento}' actualizado en el menú '{nombre_menu}'.")
                return None
        print(f"Elemento '{nombre_elemento}' no existe en el menú '{nombre_menu}'.")

    def eliminar_elemento(self, nombre_menu, nombre_elemento):
        if nombre_menu not in self.menu:
            print(f"El menú '{nombre_menu}' no existe.")
            return None

        for item in self.menu[nombre_menu]:
            if item._name == nombre_elemento:
                self.menu[nombre_menu].remove(item)
                self.guardar_menu()
                print(f"Elemento '{nombre_elemento}' eliminado del menú '{nombre_menu}'.")
                return None
        print(f"Elemento '{nombre_elemento}' no existe en el menú '{nombre_menu}'.")

    def ver_menu(self, nombre_menu):
        if nombre_menu not in self.menu:
            print(f"El menú '{nombre_menu}' no existe.")
        else:
            print(f"Menú '{nombre_menu}':")
            for item in self.menu[nombre_menu]:
                print(f"{item._name} - ${item._price:,}")
        return None

```

# Código Completo con Casos de Uso

```python
from collections import namedtuple
import random
import time
import json

# Definición de la clase Producto
Producto = namedtuple("Producto", ["nombre", "precio", "tipo_caracteristicas", "caracteristicas"])

class MenuItem:
    """Representa un artículo del menú con un nombre y un precio."""

    def __init__(self, data: Producto) -> None:
        """Inicializa el artículo del menú con nombre y precio."""
        self._name = data.nombre
        self._price = data.precio
        self.caracteristicas = [data.tipo_caracteristicas, data.caracteristicas]

    def to_dict(self) -> dict:
        """Devuelve un diccionario con los datos del artículo."""
        return {
            "nombre": self._name,
            "precio": self._price,
            "caracteristicas": self.caracteristicas
        }

    @classmethod
    def from_dict(cls, data) -> 'MenuItem': 
        producto = Producto(data["nombre"], data["precio"], data["caracteristicas"][0], data["caracteristicas"][1])
        return cls(producto)

    def calculate_total(self, order: 'Order') -> float:
        """Calcula el total de una orden aplicando descuento."""

        def compound_order(order) -> None:
            """Mediante de que se compone el pedido, genera un descuento."""
            # Descuento del 10% si hay un MainDish y un Drink
            if any(isinstance(item, MainDishes) for item in order._items.keys()):
                for key, item in order._items.items():
                    if isinstance(key, Drink):
                        order._items[key] = item * 0.9  # 10% de descuento en Drink
                        print("Descuento del 10% en Drink")
                        break

            # Descuento del 5% si hay un Breakfast y un Dessert
            if any(isinstance(item, Breakfast) for item in order._items.keys()):
                for key, item in order._items.items():
                    if isinstance(key, Dessert):
                        order._items[key] = item * 0.95  # 5% de descuento en Dessert
                        print("Descuento del 5% en Dessert")
                        break

        compound_order(order)
        total = sum(order._items.values())
        total -= total * order._discount_percentage
        return total

    def print_order(self, order: 'Order') -> None:
        """Imprime los detalles de la orden y el total con descuento."""
        total = self.calculate_total(order)
        for item, price in order._items.items():
            print(f"{item._name} - {price}")
        print(f"Descuento: {order._discount_percentage * 100}%")
        print(f"Total: ${total:,} pesos")

    def pagar(self, order):
        order._type_payment.pagar(order._total)


class Menu:
    def __init__(self, menu_file="menu.json") -> None:
        self.menu_file = menu_file
        self.menu = self.load_menu()

    def load_menu(self) -> dict:
        try:
            with open(self.menu_file, "r") as file:
                data = json.load(file)
                # Si el archivo es una lista, asumimos que es un menú único
                if isinstance(data, list):
                    return {"Menú Principal": [MenuItem.from_dict(item) for item in data]}
                # Si el archivo es un diccionario, lo cargamos como está
                elif isinstance(data, dict):
                    return {nombre_menu: [MenuItem.from_dict(item) for item in items] for nombre_menu, items in data.items()}
                else:
                    print("Formato de archivo no válido.")
                    return {}
        except (FileNotFoundError, json.JSONDecodeError):
            print("Error al cargar el archivo. Creando un menú vacío.")
            return {}

    def guardar_menu(self) -> None:
        if not self.menu:
            print("El menú está vacío, creando estructura inicial...")
            self.menu = {}

        with open(self.menu_file, "w") as file:
            data = {nombre_menu: [item.to_dict() for item in items] for nombre_menu, items in self.menu.items()}
            json.dump(data, file, indent=4)

    def crear_menu(self, nombre_menu) -> None:
        if nombre_menu in self.menu:
            print(f"El menú '{nombre_menu}' ya existe.")
        else:
            self.menu[nombre_menu] = []
            self.guardar_menu()
            print(f"Menú '{nombre_menu}' creado.")

    def agregar_elemento(self, nombre_menu: str, producto: Producto, categoria="MainDishes") -> None:
        if nombre_menu not in self.menu:
            print(f"El menú '{nombre_menu}' no existe.")
            return None

        categorias = {"Drink": Drink, "MainDishes": MainDishes, "Dessert": Dessert, "Salads": Salads, "Soups": Soups, "Breakfast": Breakfast}
        item_class = categorias.get(categoria, MainDishes)
        self.menu[nombre_menu].append(item_class(producto))
        self.guardar_menu()
        print(f"Elemento '{producto.nombre}' agregado al menú '{nombre_menu}'.")

    def actualizar_menu(self, nombre_menu, nombre_elemento, nuevo_precio) -> None:
        if nombre_menu not in self.menu:
            print(f"El menú '{nombre_menu}' no existe.")
            return None

        for item in self.menu[nombre_menu]:
            if item._name == nombre_elemento:
                item._price = nuevo_precio
                self.guardar_menu()
                print(f"Elemento '{nombre_elemento}' actualizado en el menú '{nombre_menu}'.")
                return None
        print(f"Elemento '{nombre_elemento}' no existe en el menú '{nombre_menu}'.")

    def eliminar_elemento(self, nombre_menu, nombre_elemento) -> None:
        if nombre_menu not in self.menu:
            print(f"El menú '{nombre_menu}' no existe.")
            return None

        for item in self.menu[nombre_menu]:
            if item._name == nombre_elemento:
                self.menu[nombre_menu].remove(item)
                self.guardar_menu()
                print(f"Elemento '{nombre_elemento}' eliminado del menú '{nombre_menu}'.")
                return None
        print(f"Elemento '{nombre_elemento}' no existe en el menú '{nombre_menu}'.")

    def ver_menu(self, nombre_menu) -> None:
        if nombre_menu not in self.menu:
            print(f"El menú '{nombre_menu}' no existe.")
        else:
            print(f"Menú '{nombre_menu}':")
            for item in self.menu[nombre_menu]:
                print(f"{item._name} - ${item._price:,}")
        return None


class Order:
    """Representa una orden realizada por un cliente."""

    def __init__(self, type_payment, student: bool = False) -> None:
        """Inicializa la orden con un descuento y una lista de artículos."""
        self._items: dict = {}
        self._discount_percentage: float = 0
        self._student = student
        self._type_payment = type_payment

    def add_item(self, item: MenuItem, quantity: int) -> None:
        """Añade un artículo a la orden multiplicado por la cantidad."""
        self._items[item] = quantity * item._price

    def calculate_total(self) -> float:
        """Calcula el total de la orden aplicando el descuento."""

        def compound_order(self) -> None:
            """Mediante de que se compone el pedido, genera un descuento."""
            if any(isinstance(item, MainDishes) for item in self._items.keys()):
                for key, item in self._items.items():
                    if isinstance(key, Drink):
                        self._items[key] = item * 0.9
                        print("Descuento del 10% en Drink")
                        break

            if any(isinstance(item, Breakfast) for item in self._items.keys()):
                for key, item in self._items.items():
                    if isinstance(key, Dessert):
                        self._items[key] = item * 0.95
                        print("Descuento del 5% en Dessert")
                        break

        compound_order(self)
        total = sum(self._items.values())
        total -= total * self._discount_percentage
        return total

    def promos(self) -> None:
        """Aplica promociones y descuentos según reglas predefinidas."""
        self._discount_percentage = 0

        if len(self._items) >= 6:
            self._discount_percentage += 0.1

        if self._student:
            self._discount_percentage += 0.2

        if random.random() < 0.1:
            self._discount_percentage = 1

    def print_order(self) -> None:
        """Imprime la orden y el total con descuento aplicado."""
        self._total = self.calculate_total()
        for item, price in self._items.items():
            print(f"{item._name} - {price}")
        print(f"Descuento: {self._discount_percentage * 100}%")
        print(f"Total: ${self._total:,} pesos")

    def pagar(self):
        self._type_payment.pagar(self._total)


class FIFOQueue:
    def __init__(self) -> None:
        self.items = []

    def agregar_elemento(self, element) -> None:
        self.items.append(element)

    def eliminar_elemento(self) -> None:
        if not self.esta_vacia():
            return self.items.pop(0)

    def esta_vacia(self) -> bool:
        return len(self.items) == 0


class Drink(MenuItem):
    def __init__(self, data: Producto) -> None:
        super().__init__(data)


class MainDishes(MenuItem):
    def __init__(self, data: Producto) -> None:
        super().__init__(data)


class Dessert(MenuItem):
    def __init__(self, data: Producto) -> None:
        super().__init__(data)


class Salads(MenuItem):
    def __init__(self, data: Producto) -> None:
        super().__init__(data)


class Soups(MenuItem):
    def __init__(self, data: Producto) -> None:
        super().__init__(data)


class Breakfast(MenuItem):
    def __init__(self, data: Producto) -> None:
        super().__init__(data)


class MedioPago:
    def __init__(self):
        pass

    def pagar(self, monto):
        raise NotImplementedError("Subclases deben implementar pagar()")


class Tarjeta(MedioPago):
    def __init__(self, numero, cvv):
        super().__init__()
        self._numero = numero
        self._cvv = cvv

    def pagar(self, monto):
        print(f"Pagando $ {monto:,} con tarjeta ({self._numero[-4:]})")
        print("Realizando transacción...")
        time.sleep(2)
        print("Transacción exitosa.")


class Efectivo(MedioPago):
    def __init__(self, monto_entregado):
        super().__init__()
        self._monto_entregado = monto_entregado

    def pagar(self, monto):
        print("Contando dinero...")
        time.sleep(2)
        if self._monto_entregado >= monto:
            print(f"Pago realizado en efectivo. Cambio: ${self._monto_entregado - monto:,}")
        else:
            print(f"Fondos insuficientes. Faltan ${monto - self._monto_entregado:,} para completar el pago.")


# Ejemplo de uso

# Bebidas
cafe = Producto("Café", 2000, "ml", 200)  # 200 ml
jugo_lulo = Producto("Jugo de Lulo", 2500, "ml", 250)  # 250 ml
agua_panela = Producto("Agua de panela", 1500, "ml", 300)  # 300 ml
# Platos principales
arepas = Producto("Arepa con queso", 8000, "ingredientes", ["arepa", "queso"])
bandeja_paisa = Producto("Bandeja Paisa", 25000, "ingredientes", ["arroz", "frijoles", "huevo", "carne", "chicharrón", "aguacate"])
sancocho = Producto("Sancocho de gallina", 15000, "ingredientes", ["gallina", "yuca", "papas", "plátano"])
ajiaco = Producto("Ajiaco", 18000, "ingredientes", ["pollo", "papas", "mazorca", "guasca"])
empanadas = Producto("Empanadas", 3500, "ingredientes", ["harina de maíz", "carne", "papas"])
bandeja_pescado = Producto("Bandeja De Pescado", 22000, "ingredientes", ["arroz", "pescado frito", "patacones", "ensalada", "aguacate"])
tamal = Producto("Tamales", 7000, "ingredientes", ["masa de maíz", "carne", "pollo", "papa", "zanahoria"])
# Postres
arroz_con_leche = Producto("Arroz con Leche", 3500, "g", 200)  # 200 g
tres_leches = Producto("Pastel Tres Leches", 5000, "g", 180)  # 180 g
torta_de_guanabana = Producto("Torta de Guanábana", 4500, "g", 150)  # 150 g

# Ensaladas
ensalada_mixta = Producto("Ensalada Mixta", 4000, "tazón", 1)  # 1 tazón
ensalada_de_pasta = Producto("Ensalada de Pasta", 5000, "plato", 1)  # 1 plato

# Sopas
sopa_de_lentejas = Producto("Sopa de Lentejas", 6000, "temperatura", 1)  # Caliente
sopa_de_carne = Producto("Sopa de Carne", 8000, "temperatura", 1)  # Caliente
sopa_de_vegetales = Producto("Sopa de Vegetales", 7000, "temperatura", 1)  # Caliente

# Desayunos
arepas_con_huevo = Producto("Arepas con Huevo", 5000, "unidades", 2)  # 2 unidades
calentado = Producto("Calentado", 7000, "porción", 1)  # 1 porción
empanadas_con_aji = Producto("Empanadas con Aji", 4500, "unidades", 3)  # 3 unidades

# Crear el menú completo y agregar todos los elementos
menu_completo = Menu()
menu_completo.crear_menu("Menú Completo")

# Agregar bebidas
menu_completo.agregar_elemento("Menú Completo", cafe, "Drink")
menu_completo.agregar_elemento("Menú Completo", jugo_lulo, "Drink")
menu_completo.agregar_elemento("Menú Completo", agua_panela, "Drink")

# Agregar platos principales
menu_completo.agregar_elemento("Menú Completo", arepas, "MainDishes")
menu_completo.agregar_elemento("Menú Completo", bandeja_paisa, "MainDishes")
menu_completo.agregar_elemento("Menú Completo", sancocho, "MainDishes")
menu_completo.agregar_elemento("Menú Completo", ajiaco, "MainDishes")
menu_completo.agregar_elemento("Menú Completo", empanadas, "MainDishes")
menu_completo.agregar_elemento("Menú Completo", bandeja_pescado, "MainDishes")
menu_completo.agregar_elemento("Menú Completo", tamal, "MainDishes")

# Agregar postres
menu_completo.agregar_elemento("Menú Completo", arroz_con_leche, "Dessert")
menu_completo.agregar_elemento("Menú Completo", tres_leches, "Dessert")
menu_completo.agregar_elemento("Menú Completo", torta_de_guanabana, "Dessert")

# Agregar ensaladas
menu_completo.agregar_elemento("Menú Completo", ensalada_mixta, "Salads")
menu_completo.agregar_elemento("Menú Completo", ensalada_de_pasta, "Salads")

# Agregar sopas
menu_completo.agregar_elemento("Menú Completo", sopa_de_lentejas, "Soups")
menu_completo.agregar_elemento("Menú Completo", sopa_de_carne, "Soups")
menu_completo.agregar_elemento("Menú Completo", sopa_de_vegetales, "Soups")

# Agregar desayunos
menu_completo.agregar_elemento("Menú Completo", arepas_con_huevo, "Breakfast")
menu_completo.agregar_elemento("Menú Completo", calentado, "Breakfast")
menu_completo.agregar_elemento("Menú Completo", empanadas_con_aji, "Breakfast")

# Ver el menú completo
menu_completo.ver_menu("Menú Completo")


# ### Casos de uso ###

tarjeta = Tarjeta("1234567890123456", 453)
cliente = Order(tarjeta, student=True)

cliente.add_item(Drink(cafe), 1)
cliente.add_item(Drink(jugo_lulo), 1)
cliente.add_item(Dessert(arroz_con_leche), 1)

efectivo = Efectivo(1000)
cliente2 = Order(efectivo)
cliente2.add_item(MainDishes(bandeja_paisa), 1)
cliente2.add_item(MainDishes(ajiaco), 1)
cliente2.add_item(Drink(agua_panela), 1)
cliente2.add_item(Dessert(tres_leches), 1)
cliente2.add_item(Breakfast(arepas_con_huevo), 5)
cliente2.add_item(MainDishes(empanadas), 2)
cliente2.add_item(Dessert(torta_de_guanabana), 1)
cliente2.add_item(Drink(agua_panela), 1)
cliente2.add_item(Breakfast(arepas_con_huevo), 1)

fifo_clientes = FIFOQueue()
fifo_clientes.agregar_elemento(cliente) # Orden 1
fifo_clientes.agregar_elemento(cliente2) # Orden 2

print("\n\n---Atendiendo a los clientes---")
indice = 1
while not fifo_clientes.esta_vacia():
    print(f"\n\n---Orden {indice}---")
    cliente = fifo_clientes.eliminar_elemento()
    cliente.promos()
    cliente.print_order()
    cliente.pagar()
    indice += 1

```
