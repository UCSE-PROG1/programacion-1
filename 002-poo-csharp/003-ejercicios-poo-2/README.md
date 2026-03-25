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

> Unidad 2 - Práctica 2 | Programación 1 | UCSE
