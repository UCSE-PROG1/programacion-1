# Ejercicios POO con C# - Práctica 2

## Configuración inicial

Crear el proyecto .NET en una carpeta de tu computadora:

```bash
dotnet new console -n EjerciciosPOO2
```

Luego abrir la carpeta del proyecto en VS Code: `File → Open Folder`.

> Si no tenés instalado el SDK de .NET, descargarlo desde [dotnet.microsoft.com/download](https://dotnet.microsoft.com/download) seleccionando **.NET 8** (LTS).

## Estructura del proyecto

Cada ejercicio va en su propia carpeta dentro del proyecto:

```
EjerciciosPOO2/
├── Program.cs                              ← único punto de entrada
├── Ejercicio01Veterinaria/
│   ├── Animal.cs
│   └── Veterinaria.cs
├── Ejercicio02Gimnasio/
│   ├── Socio.cs
│   └── Gimnasio.cs
└── ...
```

> `Program.cs` es el único punto de entrada. Cada carpeta contiene solo las clases del ejercicio. Para probar un ejercicio, instanciar las clases desde `Program.cs`.

---

## Cómo leer los enunciados

A diferencia de la práctica anterior, los enunciados están escritos en forma de párrafos. Tu trabajo es **identificar las entidades, sus propiedades y sus comportamientos** a partir del texto, y luego modelarlos en clases C#.

Prestá atención a:
- Los **sustantivos** → suelen ser clases o propiedades.
- Los **verbos** → suelen ser métodos.
- Las **cantidades** ("varios", "una lista de", "muchos") → suelen indicar colecciones.
- Las **condiciones** ("solo si", "siempre que", "si hay suficiente") → suelen ser validaciones dentro de los métodos.

---

## Ejemplo de referencia

El siguiente enunciado muestra cómo leer el texto e identificar los elementos del modelo.

> Una biblioteca universitaria necesita gestionar el préstamo de libros a sus socios. Cada libro tiene un título, un autor y un estado que indica si está disponible para préstamo. Los socios tienen un nombre, un número de documento y pueden tener como máximo un libro prestado a la vez. La biblioteca debe poder registrar un préstamo (solo si el libro está disponible y el socio no tiene uno ya prestado), registrar la devolución de un libro, y consultar qué libro tiene prestado un socio en este momento.

De ese párrafo se identifican:

- **`Libro`**: `Titulo`, `Autor`, `Disponible`
- **`Socio`**: `Nombre`, `Documento`, `LibroPrestado`
- **`Biblioteca`**: gestiona el préstamo → métodos `PrestarLibro()`, `DevolverLibro()`, `ConsultarPrestamo()`

```csharp
// Libro.cs
namespace EjerciciosPOO2.EjercicioReferencia;

public class Libro
{
    public string Titulo { get; set; }
    public string Autor { get; set; }
    public bool Disponible { get; set; }

    public Libro(string titulo, string autor)
    {
        Titulo = titulo;
        Autor = autor;
        Disponible = true;
    }

    public override string ToString() => $"\"{Titulo}\" de {Autor}";
}
```

```csharp
// Socio.cs
namespace EjerciciosPOO2.EjercicioReferencia;

public class Socio
{
    public string Nombre { get; set; }
    public string Documento { get; set; }
    public Libro? LibroPrestado { get; set; }

    public Socio(string nombre, string documento)
    {
        Nombre = nombre;
        Documento = documento;
    }
}
```

```csharp
// Biblioteca.cs
namespace EjerciciosPOO2.EjercicioReferencia;

public class Biblioteca
{
    private List<Libro> libros = new List<Libro>();
    private List<Socio> socios = new List<Socio>();

    public void AgregarLibro(Libro libro) => libros.Add(libro);
    public void AgregarSocio(Socio socio) => socios.Add(socio);

    public bool PrestarLibro(Libro libro, Socio socio)
    {
        if (!libro.Disponible || socio.LibroPrestado != null)
            return false;

        libro.Disponible = false;
        socio.LibroPrestado = libro;
        return true;
    }

    public void DevolverLibro(Socio socio)
    {
        if (socio.LibroPrestado == null) return;

        socio.LibroPrestado.Disponible = true;
        socio.LibroPrestado = null;
    }

    public Libro? ConsultarPrestamo(Socio socio) => socio.LibroPrestado;
}
```

```csharp
// Program.cs
using EjerciciosPOO2.EjercicioReferencia;

Libro libro1 = new Libro("Clean Code", "Robert C. Martin");
Socio socio1 = new Socio("Ana García", "30111222");

Biblioteca bib = new Biblioteca();
bib.AgregarLibro(libro1);
bib.AgregarSocio(socio1);

bool exito = bib.PrestarLibro(libro1, socio1);
Console.WriteLine($"Préstamo realizado: {exito}");
Console.WriteLine($"Libro de {socio1.Nombre}: {bib.ConsultarPrestamo(socio1)}");

bib.DevolverLibro(socio1);
Console.WriteLine($"Después de la devolución: {bib.ConsultarPrestamo(socio1) ?? "sin libro"}");
```

---

## Ejercicios

### 1. Veterinaria

Una veterinaria de barrio necesita llevar un registro de los animales que atiende. Cada animal tiene un nombre, una especie (perro, gato, ave, etc.) y una edad en años. La veterinaria permite registrar nuevos pacientes, dar de baja a un animal (cuando el dueño ya no lo trae), buscar un animal por su nombre y listar todos los pacientes activos ordenados por nombre.

---

### 2. Gimnasio

Un gimnasio necesita gestionar sus socios. Cada socio tiene un nombre, un número de socio único y un estado que indica si su cuota está al día. El gimnasio tiene una capacidad máxima de socios activos. El sistema debe poder dar de alta un nuevo socio (solo si no se superó la capacidad), dar de baja a un socio existente, marcar la cuota de un socio como paga o vencida, y obtener la cantidad de socios con cuota al día.

---

### 3. Estacionamiento

Un estacionamiento privado necesita controlar los vehículos que ingresan y egresan. Cada vehículo tiene una patente y el nombre de su dueño. El estacionamiento tiene una cantidad máxima de lugares. El sistema debe registrar el ingreso de un vehículo (solo si hay lugar disponible), registrar su egreso, verificar si un vehículo con una determinada patente está estacionado en este momento, e informar cuántos lugares libres quedan.

---

### 4. Tienda de música

Una tienda de música vende instrumentos y lleva un registro de su stock. Cada instrumento tiene un nombre, una categoría (cuerda, viento, percusión) y un precio de venta. La tienda debe poder agregar nuevos instrumentos al stock, registrar la venta de un instrumento (eliminándolo del stock), buscar instrumentos por categoría y calcular el valor total del inventario disponible sumando los precios de todos los instrumentos en stock.

---

### 5. Sistema de encomiendas

Una empresa de logística necesita rastrear sus encomiendas. Cada encomienda tiene un código único, el nombre del remitente, el nombre del destinatario y un estado que puede ser: ingresada, en camino o entregada. El sistema debe permitir registrar una nueva encomienda (con estado inicial "ingresada"), avanzar el estado de una encomienda al siguiente paso del proceso, consultar el estado actual de una encomienda por su código, y listar todas las encomiendas que todavía no fueron entregadas.

---

### 6. Agenda de turnos

Una peluquería necesita organizar su agenda de turnos. Cada turno tiene una fecha (representada como texto, por ejemplo "2025-06-15"), un horario (por ejemplo "10:30"), el nombre del cliente y el servicio solicitado (corte, tintura, etc.). La peluquería puede tener varios peluqueros, cada uno con su propia agenda. El sistema debe permitir asignar un turno a un peluquero específico, cancelar un turno, listar todos los turnos de un peluquero para una fecha dada y verificar si un peluquero tiene disponibilidad en un horario determinado.

---

### 7. Plataforma de cursos online

Una plataforma de aprendizaje online ofrece cursos a sus usuarios. Cada curso tiene un título, una descripción, una duración en horas y un cupo máximo de estudiantes. Los usuarios tienen un nombre, un email y una lista de cursos en los que están inscriptos. La plataforma debe permitir inscribir a un usuario en un curso (verificando que no supere el cupo y que el usuario no esté ya inscripto), dar de baja la inscripción, calcular la cantidad total de horas de estudio de un usuario sumando la duración de sus cursos, y listar los usuarios inscriptos en un curso específico.

---

### 8. Sistema de préstamos financieros

Una entidad financiera necesita gestionar los préstamos que otorga a sus clientes. Cada cliente tiene un nombre, un CUIT y un historial de préstamos. Cada préstamo tiene un monto otorgado, una tasa de interés mensual, una cantidad de cuotas y un estado (activo o cancelado). El sistema debe poder registrar un nuevo préstamo para un cliente, calcular el valor de cada cuota (monto × (1 + tasa) / cuotas), cancelar un préstamo activo, calcular la deuda total vigente de un cliente sumando los montos de sus préstamos activos, y verificar si un cliente puede solicitar un nuevo préstamo (solo si no tiene más de dos préstamos activos).

---

### 9. Red social simplificada

Una red social básica permite a sus usuarios publicar contenido y seguir a otros usuarios. Cada usuario tiene un nombre de usuario, una biografía y dos listas: una con los usuarios que sigue y otra con sus publicaciones. Cada publicación tiene un texto, una fecha (como texto) y un contador de "me gusta". El sistema debe permitir que un usuario siga a otro (si todavía no lo sigue), deje de seguirlo, realice una nueva publicación, agregue un "me gusta" a cualquier publicación, y obtenga un feed con todas las publicaciones de los usuarios que sigue, ordenadas de más reciente a más antigua (usando la fecha como texto para ordenar).

---

### 10. Sistema de facturación

Una empresa necesita emitir facturas a sus clientes. Cada cliente tiene un nombre, un CUIT y una dirección. Cada producto tiene un nombre, un precio unitario y un porcentaje de IVA. Una factura tiene un número único, el cliente al que pertenece, una lista de líneas (cada línea tiene un producto y una cantidad), y un estado (borrador, emitida, anulada). El sistema debe permitir crear una factura en borrador, agregar y quitar líneas, calcular el subtotal (suma de precio × cantidad por cada línea), calcular el IVA total y el total final, emitir la factura (cambiando su estado) y anularla. Solo se pueden modificar las facturas en estado borrador.

---

### 11. Concesionaria de autos con herencia

Una concesionaria vende distintos tipos de vehículos, todos con características en común pero también con particularidades propias. Todo vehículo tiene una marca, un modelo, un año de fabricación y un precio base. Los autos (`Auto`) son vehículos de pasajeros con una cantidad de puertas y un tipo de combustible. Las camionetas (`Camioneta`) tienen una capacidad de carga en kilogramos y tracción (simple o doble). Los vehículos eléctricos (`VehiculoElectrico`) tienen una autonomía en kilómetros y el tiempo de carga en horas. Todos los tipos de vehículo deben poder describirse a sí mismos con un método que devuelva un texto detallando sus características. La concesionaria debe poder listar todos los vehículos disponibles, filtrar por tipo y calcular el precio promedio de su catálogo.

---

### 12. Sistema de empleados con jerarquía

Una empresa tiene distintos tipos de empleados y necesita calcular el sueldo de cada uno según su categoría. Todos los empleados comparten un nombre, un legajo y una fecha de ingreso (como texto). Los empleados de planta (`EmpleadoPlanta`) cobran un sueldo base fijo más un adicional del 10% por cada año de antigüedad. Los empleados contratados (`EmpleadoContrato`) cobran un valor hora multiplicado por las horas trabajadas en el mes. Los gerentes (`Gerente`) cobran un sueldo base más un porcentaje de los ingresos del área a su cargo. El sistema de recursos humanos debe poder registrar empleados de cualquier tipo, calcular el sueldo de cada uno, obtener el total de la nómina y listar los empleados ordenados de mayor a menor sueldo.

---

### 13. Aplicación de delivery con interfaces

Una aplicación de delivery conecta restaurantes, repartidores y clientes. Los restaurantes y los repartidores deben poder ser geolocalizados: ambos implementan una interfaz `IGeoLocalizable` que obliga a exponer una latitud, una longitud y un método que calcule la distancia en kilómetros a otro punto dado (podés usar una fórmula simplificada con diferencia de coordenadas). Los restaurantes también deben implementar `ICalificable`, que permite agregar puntuaciones (del 1 al 5) y obtener el promedio. Un pedido tiene un cliente, un restaurante, una lista de items (nombre y precio), un repartidor asignado y un estado (pendiente, en preparación, en camino, entregado). La plataforma debe poder crear pedidos, asignar el repartidor más cercano al restaurante de entre los disponibles, avanzar el estado del pedido y calcular el costo total del pedido incluyendo un cargo de envío fijo.

---

### 14. Gestión hospitalaria

Un hospital necesita un sistema para administrar pacientes, médicos e internaciones. Los médicos tienen un nombre, una matrícula y una especialidad. Los pacientes tienen un nombre, un número de historia clínica y un grupo sanguíneo. Una internación vincula a un paciente con un médico responsable, tiene una fecha de ingreso (como texto), una fecha de alta (como texto, puede ser nula si el paciente sigue internado) y un diagnóstico. El hospital tiene un número máximo de camas. El sistema debe permitir registrar una nueva internación (solo si hay camas disponibles), dar el alta médica a un paciente, buscar todas las internaciones activas de un médico determinado, calcular cuántos días lleva internado un paciente dado (en base a las fechas como texto en formato `yyyy-MM-dd`) y generar un reporte con la cantidad de internaciones activas por especialidad médica.

---

### 15. Torneo de fútbol completo

Una plataforma deportiva gestiona torneos de fútbol. Cada jugador tiene un nombre, un número de documento, una posición (arquero, defensor, mediocampista, delantero) y estadísticas de goles y tarjetas amarillas en el torneo. Cada equipo tiene un nombre, un director técnico y un plantel de jugadores (mínimo 11, máximo 25). El torneo tiene un nombre, una lista de equipos participantes y una tabla de posiciones. En la fase de grupos todos los equipos se enfrentan entre sí (partidos de ida y vuelta). Cada partido tiene dos equipos, una fecha (como texto) y puede estar pendiente o jugado; si está jugado, registra los goles de cada equipo y qué jugadores convirtieron. El sistema debe poder registrar partidos jugados (actualizando posiciones: 3 puntos por victoria, 1 por empate, 0 por derrota), calcular la tabla de posiciones ordenada (primero por puntos, luego por diferencia de goles, luego por nombre), identificar al goleador del torneo, listar los jugadores con más de dos tarjetas amarillas y determinar los equipos clasificados a la siguiente fase (los dos primeros de la tabla).

---

### 16. Biblioteca con reservas
Una biblioteca necesita gestionar su catálogo de libros y los préstamos a usuarios. Cada libro tiene un título, un autor, un ISBN único y una cantidad de ejemplares disponibles. Los usuarios tienen un nombre, un DNI y una lista de préstamos activos. El sistema debe permitir registrar nuevos libros (sumando ejemplares si el ISBN ya existe), prestar un libro a un usuario (solo si hay stock disponible y el usuario no lo tiene ya prestado), registrar la devolución de libros y permitir que los usuarios reserven libros cuando no hay disponibilidad. Cuando un libro es devuelto, debe asignarse automáticamente al primer usuario en la lista de reservas. Además, se debe poder listar los libros más prestados.

---

### 17. Sistema de cine con butacas
Un cine necesita gestionar sus salas y la venta de entradas. Cada sala tiene un número, una capacidad y un conjunto de butacas identificadas por fila y número. Cada función tiene una película, una fecha y hora, y una sala asignada. El sistema debe permitir vender entradas asignando una butaca específica si está disponible, cancelar entradas, mostrar el estado de las butacas (ocupadas o libres) para una función determinada, calcular la recaudación total de una función y evitar que una misma butaca sea vendida más de una vez.

---

### 18. Plataforma de streaming
Una plataforma de streaming ofrece distintos tipos de contenido, como películas y series. Cada contenido tiene un título, un género y una duración. Las series, además, están compuestas por temporadas y episodios. Los usuarios tienen un nombre, un email y un historial de contenido visto. El sistema debe permitir marcar contenido como visto, calificarlo con un puntaje del 1 al 5, obtener recomendaciones de contenido no visto ordenado por mejor calificación, calcular el tiempo total de visualización de un usuario y listar los contenidos más populares según las calificaciones recibidas.

---

### 19. Sistema de vuelos
Una aerolínea necesita gestionar sus vuelos y pasajeros. Cada vuelo tiene un código, un origen, un destino, una capacidad máxima y una lista de pasajeros con sus asientos asignados. Cada pasajero tiene un nombre y un DNI. El sistema debe permitir reservar asientos (asignando automáticamente uno disponible o permitiendo elegirlo), cancelar reservas, manejar una lista de espera cuando el vuelo está completo y asignar automáticamente un asiento cuando se libera un lugar. Además, se debe poder calcular el porcentaje de ocupación de cada vuelo.

---

### 20. Carrito de compras con descuentos
Una tienda online necesita gestionar carritos de compra. Cada producto tiene un nombre, un precio y una categoría. Un carrito contiene múltiples productos con sus respectivas cantidades. El sistema debe permitir agregar y quitar productos del carrito, aplicar descuentos (como un 10% si se supera un monto determinado o promociones del tipo 2x1 en ciertas categorías), calcular el total final de la compra y generar un resumen detallado con los productos, cantidades, descuentos aplicados y total a pagar.

---

### 21. Sistema de turnos médicos avanzado
Un centro médico necesita gestionar turnos con mayor complejidad. Cada médico tiene un nombre, una especialidad y una duración específica para sus turnos. Los pacientes tienen un nombre y un historial de turnos. El sistema debe permitir asignar turnos verificando que un paciente no tenga superposición horaria, cancelar y reprogramar turnos, calcular el tiempo de espera promedio por médico y dar prioridad a pacientes con carácter urgente, asegurando que puedan obtener turnos más rápidamente.

---

### 22. Videojuego RPG (POO con herencia)
Un videojuego de rol gestiona distintos tipos de personajes. Todos los personajes tienen un nombre, puntos de vida y nivel. Existen distintos tipos de personajes como guerreros, magos y arqueros, cada uno con características particulares. Los personajes pueden atacarse entre sí, subir de nivel mejorando sus estadísticas y gestionar un inventario con objetos como armas y pociones. El sistema debe permitir realizar combates por turnos, utilizar objetos del inventario para recuperar vida y aplicar reglas distintas según el tipo de personaje.

---

### 23. Sistema bancario con cuentas
Un banco necesita gestionar clientes y sus cuentas. Cada cliente tiene un nombre, un DNI y puede tener múltiples cuentas. Existen distintos tipos de cuentas, como caja de ahorro y cuenta corriente, esta última permitiendo un descubierto. El sistema debe permitir realizar depósitos, extracciones y transferencias entre cuentas, calcular intereses mensuales, y bloquear una cuenta si su saldo negativo supera un límite establecido.

---

### 24. Gestión de proyectos
Una aplicación de gestión de proyectos permite organizar tareas. Cada proyecto tiene un nombre y una lista de tareas. Cada tarea tiene un título, una descripción, un estado (pendiente, en progreso o terminada) y un usuario asignado. El sistema debe permitir crear tareas, cambiar su estado, reasignarlas a distintos usuarios, calcular el porcentaje de avance del proyecto en base a las tareas completadas y listar las tareas correspondientes a cada usuario.

---

### 25. Sistema de restaurante
Un restaurante necesita gestionar mesas, mozos y pedidos. Cada mesa puede estar ocupada por clientes y tener un pedido asociado, el cual contiene una lista de platos con sus precios. El sistema debe permitir asignar mesas a clientes, crear pedidos, agregar o quitar platos, calcular el total a pagar, dividir la cuenta entre los comensales y liberar la mesa una vez que el pago ha sido realizado.

---

### 26. Plataforma de alquiler de autos
Una empresa de alquiler de autos gestiona su flota y sus clientes. Cada auto tiene una marca, un modelo, un precio por día y un estado (disponible o alquilado). Los clientes pueden alquilar autos si están disponibles y luego devolverlos. El sistema debe calcular el costo total del alquiler según la cantidad de días, aplicar recargos en caso de devoluciones tardías y permitir listar todos los autos disponibles en un momento dado.

---

### 27. Sistema de encuestas
Una aplicación permite crear encuestas con múltiples preguntas y opciones de respuesta. Los usuarios pueden responder encuestas, pero solo una vez por encuesta. El sistema debe registrar las respuestas, calcular el porcentaje de votos por opción, mostrar los resultados de cada pregunta y determinar cuál fue la opción más elegida.

---

### 28. Gestión de stock con proveedores
Una empresa necesita controlar su stock de productos y sus proveedores. Cada producto tiene un nombre, una cantidad en stock y un nivel mínimo requerido. Los proveedores tienen un nombre y una lista de productos que pueden suministrar. El sistema debe permitir registrar compras y ventas de productos, actualizar el stock, detectar productos que están por debajo del mínimo, sugerir proveedores para reponerlos y calcular el valor total del inventario disponible.

---

### 29. Sistema de mensajería tipo WhatsApp
Una aplicación de mensajería permite a los usuarios comunicarse entre sí y en grupos. Cada usuario tiene un nombre y una lista de conversaciones. Los mensajes tienen un texto, una fecha y un estado (enviado, recibido o leído). El sistema debe permitir enviar mensajes individuales y grupales, marcar mensajes como leídos, mostrar el historial de conversaciones ordenado por fecha y contar la cantidad de mensajes no leídos.

---

### 30. Sistema de reservas de hotel
Un hotel necesita gestionar sus habitaciones y reservas. Cada habitación tiene un número, un tipo y un precio por noche. Las reservas incluyen un cliente, una habitación y un rango de fechas de ingreso y salida. El sistema debe verificar la disponibilidad de habitaciones para un rango de fechas determinado, registrar nuevas reservas, cancelarlas, calcular el costo total de la estadía y listar la ocupación del hotel en una fecha específica.

> Unidad 2 - Práctica 2 | Programación 1 | UCSE

---

## Corrección — Ejercicios POO 2 (2026)

> Total de ejercicios: **30**. Fecha de corrección: 2026-04-16.

| Alumno | Ejercicios entregados | Ejercicios completados | Ejercicios faltantes |
|---|---|---|---|
| Caudana | 1, 2, 3, 4, 5, 6 | 6 | 24 |
| Franco | 1, 2, 3, 4, 5 | 5 | 25 |
| Hartmann | 1, 2, 3 | 3 | 27 |
| Cassani | 1, 2 | 2 | 28 |
| Lagger | 1, 2 | 2 | 28 |
| Lubatti | 1, 2 | 2 | 28 |
| Heinzmann | 1 | 1 | 29 |
| REY | 1 | 1 | 29 |
| Solari | 1 | 1 | 29 |
| Castro | — | 0 | 30 |
| Ferrarese | — | 0 | 30 |
| Gandini | — | 0 | 30 |
| Tocchetti | — | 0 | 30 |
