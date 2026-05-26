# Pre-Parcial — Programación I | UCSE

## Instrucciones generales

- El preparcial debe hacerse en **2 horas**. Si se entrega luego de las 22hs, no se realizaran correcciones ni observaciones.
- Se debe entregar por GitHub en un repositorio llamado **`Prog1-2026-preparcial1-Apellido`**.
- El enunciado sirve para medir tiempos de respuesta del alumno o del enunciado. Sumar al repositorio a gonzaperez2312@gmail.com

---

## MovilYa — Sistema de gestión de flota vehicular

---

*MovilYa* es una empresa de movilidad urbana que alquila vehículos a particulares y empresas. Su flota está compuesta por autos, motos y bicicletas. Hasta ahora todo el registro se lleva en papel, y la empresa necesita informatizar la gestión de su flota y sus reservas. Se les pide construir ese sistema.

---

### La flota

Todos los vehículos de la empresa tienen en común: una patente (identificador único que no cambia una vez asignado), una marca, un modelo, un año de fabricación y un precio base diario expresado en pesos.

Las motos tienen una característica adicional: la empresa las clasifica en tres categorías según su uso —urbanas, deportivas y todoterreno—, y esa categoría influye directamente en cómo se cotizan.

Las bicicletas registran únicamente la cantidad de cambios de velocidad que poseen, además de los datos comunes.

---

### Cotización de alquileres

Cada tipo de vehículo calcula su precio de alquiler de manera diferente:

**Autos.** El precio se obtiene multiplicando el precio base diario por la cantidad de días. Si el alquiler dura más de una semana, la empresa aplica un descuento del 15% sobre el total. Además, en el momento de la cotización el cliente puede optar por agregar cobertura de seguro o no: si la agrega, el precio resultante —ya con descuento aplicado si corresponde— se incrementa un 10%. Si no la agrega, el precio queda como estaba.

**Motos.** Las motos urbanas y todoterreno se cotizan al 60% del precio base diario por los días solicitados. Las deportivas tienen una tarifa más alta y se cotizan al 80%.

**Bicicletas.** Independientemente del precio base que figure en el registro, todas las bicicletas se alquilan a tarifa fija de $200 por día.

---

### Registro de reservas

Cuando un cliente confirma un alquiler, el sistema genera una reserva. Cada reserva tiene un número identificador único que se asigna automáticamente y no puede modificarse, el nombre del cliente, el vehículo que se lleva y la cantidad de días. El costo total de la reserva surge del vehículo reservado y los días pactados.

---

### Búsqueda de vehículos

El personal de la empresa necesita poder buscar vehículos de dos maneras: por patente exacta cuando ya la conocen, y por tipo de vehículo cuando quieren ver qué hay disponible de cada categoría. Si una búsqueda no encuentra ningún resultado, el sistema debe informarlo de forma clara.

---

### Reglas del negocio

La empresa establece que ningún dato ingresado al sistema puede ser incoherente. Un vehículo no puede tener un año de fabricación anterior a 1900 ni posterior al año en curso. La patente no puede estar vacía. Una reserva no puede tener cero días ni una cantidad negativa. Cualquier intento de registrar datos fuera de estas reglas debe ser rechazado por el sistema con una explicación del motivo.

---

### Calidad del software

El sistema deberá estar respaldado por pruebas automatizadas que verifiquen que cada regla de negocio funciona correctamente. Estas pruebas deben poder ejecutarse de forma independiente entre sí, sin importar el orden en que se corran, y cada una debe enfocarse en un único comportamiento. El nombre de cada prueba debe ser suficientemente descriptivo como para entender qué se está verificando sin necesidad de leer el cuerpo del método.

---

### Arquitectura y organización del código

El sistema debe organizarse en una solución con al menos dos proyectos: uno con la lógica de negocio (sin ninguna interacción con la consola ni con el usuario) y otro con las pruebas automatizadas. Si se incluye un programa de consola para demostrar el funcionamiento, debe residir en un tercer proyecto separado. Los nombres de clases, propiedades y métodos deben seguir las convenciones de C# vistas en la materia.

---

### Entrega

El trabajo debe entregarse en un repositorio privado de GitHub con acceso habilitado al docente. El historial de commits debe reflejar el avance progresivo del desarrollo: se esperan al menos 5 commits con mensajes que describan claramente qué agrega o corrige cada uno. El repositorio debe ignorar los archivos generados por la compilación.

---

> Pre-Parcial — Programación I | UCSE
