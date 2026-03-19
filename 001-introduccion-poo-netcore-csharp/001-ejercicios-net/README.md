# Ejercicios .NET Core - Programación 1

## Configuración inicial

Abrir Visual Studio Code y crear el proyecto .NET en una carpeta de tu computadora:
```bash
dotnet new console -n EjerciciosNet
```
Luego abrir la carpeta del proyecto en VS Code: `File → Open Folder`.

> Si no tenés instalado el SDK de .NET, descargarlo desde [dotnet.microsoft.com/download](https://dotnet.microsoft.com/download) seleccionando **.NET 8** (LTS).

## Estructura del proyecto

Cada ejercicio va en su propia carpeta dentro del proyecto:

```
EjerciciosNet/
├── Program.cs                           ← único punto de entrada, acá se prueban todos los ejercicios
├── Ejercicio01Pedidos/
│   ├── Producto.cs
│   ├── Pedido.cs
│   └── GestionPedidos.cs
├── Ejercicio02Cursos/
│   ├── Curso.cs
│   ├── Estudiante.cs
│   └── AdministracionCursos.cs
└── ...
```

> `Program.cs` es el único punto de entrada del programa. Cada carpeta contiene solo las clases del ejercicio. Para probar un ejercicio, instanciar las clases desde `Program.cs`.

---

## Ejemplo de referencia

Antes de arrancar con los ejercicios, tomá este ejemplo como modelo de cómo estructurar el código y las pruebas.

```csharp
// Libro.cs
namespace EjerciciosNet.Ejercicio00Biblioteca;

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
}
```

```csharp
// Usuario.cs
namespace EjerciciosNet.Ejercicio00Biblioteca;

public class Usuario
{
    public string Nombre { get; set; }
    public string Dni { get; set; }
    public Libro? LibroActual { get; set; }

    public Usuario(string nombre, string dni)
    {
        Nombre = nombre;
        Dni = dni;
    }
}
```

```csharp
// Ejercicio00Biblioteca/Biblioteca.cs
namespace EjerciciosNet.Ejercicio00Biblioteca;

public class Biblioteca
{
    public List<Libro> Libros { get; set; } = new List<Libro>();
    public List<Usuario> Usuarios { get; set; } = new List<Usuario>();

    public bool PrestarLibro(Libro libro, Usuario usuario)
    {
        if (libro.Disponible)
        {
            libro.Disponible = false;
            usuario.LibroActual = libro;
            return true;
        }
        return false;
    }

    public Libro DevolverLibro(Libro libro, Usuario usuario)
    {
        libro.Disponible = true;
        usuario.LibroActual = null;
        return libro;
    }

    public bool ConsultarDisponibilidad(Libro libro)
    {
        return libro.Disponible;
    }
}
```

```csharp
// Program.cs
using EjerciciosNet.Ejercicio00Biblioteca;

EjercicioBiblioteca();
EjercicioPedidos();
EjercicioCursos();

static void EjercicioBiblioteca()
{
    // Pedir datos por pantalla con Console.ReadLine() (titulo, autor, nombre de usuario, dni)
    // Crear objetos Libro y Usuario con esos datos
    // Crear instancia de Biblioteca y agregar los objetos a sus listas
    // Invocar PrestarLibro(), ConsultarDisponibilidad(), DevolverLibro()
    // Imprimir los resultados retornados por cada método con Console.WriteLine()
}

static void EjercicioPedidos()
{
    // Pedir datos por pantalla (nombre y precio de productos, cantidad)
    // Crear objetos Producto y un Pedido vacío
    // Invocar AgregarProducto() por cada producto ingresado
    // Invocar CalcularTotal() e imprimir el resultado
    // Pedir qué producto quitar, invocar QuitarProducto() y mostrar el nuevo total
}

static void EjercicioCursos()
{
    // Pedir datos del curso (nombre, cupo) y de varios estudiantes (nombre, legajo)
    // Crear instancia de Curso y lista de Estudiante
    // Invocar InscribirEstudiante() para cada uno e imprimir el resultado
    // Invocar VerificarCupo() e imprimir si hay lugares disponibles
    // Invocar DarBajaEstudiante() para alguno y mostrar el resultado
}
```

---

## Ejercicios

### 1. Gestión de Pedidos

**Clases:**
- `Producto`: `Nombre: string`, `Precio: double`
- `Pedido`: `Productos: List<Producto>`, `Total: double`

**Métodos a implementar:**
- `AgregarProducto(Pedido pedido, Producto producto)` → agrega el producto a la lista y suma su precio al total
- `QuitarProducto(Pedido pedido, Producto producto)` → elimina el producto de la lista y resta su precio del total
- `CalcularTotal(Pedido pedido)` → recalcula y retorna el total sumando los precios de todos los productos

---

### 2. Administración de Cursos

**Clases:**
- `Curso`: `Nombre: string`, `Cupo: int`, `Inscriptos: List<Estudiante>`
- `Estudiante`: `Nombre: string`, `Legajo: string`

**Métodos a implementar:**
- `InscribirEstudiante(Curso curso, Estudiante estudiante)` → agrega al estudiante si la cantidad de inscriptos es menor al cupo
- `DarBajaEstudiante(Curso curso, Estudiante estudiante)` → elimina al estudiante de la lista
- `VerificarCupo(Curso curso)` → retorna `true` si `Inscriptos.Count < Cupo`

---

### 3. Gestión de Cuentas Bancarias

**Clases:**
- `Cuenta`: `Numero: string`, `Saldo: double`
- `Cliente`: `Nombre: string`, `Dni: string`

**Métodos a implementar:**
- `Depositar(Cuenta cuenta, double monto)` → suma el monto al saldo
- `Retirar(Cuenta cuenta, double monto)` → resta el monto si el saldo es suficiente; si no, imprimir error con `Console.WriteLine()`
- `ConsultarSaldo(Cuenta cuenta)` → retorna el saldo actual

---

### 4. Sistema de Reservas de Hotel

**Clases:**
- `Habitacion`: `Numero: int`, `Disponible: bool`
- `Huesped`: `Nombre: string`, `Dni: string`

**Métodos a implementar:**
- `ReservarHabitacion(Habitacion habitacion, Huesped huesped)` → asigna la habitación al huésped si está disponible y la marca como ocupada
- `CancelarReserva(Habitacion habitacion, Huesped huesped)` → desvincula al huésped y la marca como disponible
- `VerificarDisponibilidad(Habitacion habitacion)` → retorna `true` si está libre

---

### 5. Gestión de Inventario

**Clases:**
- `Producto`: `Nombre: string`, `Stock: int`
- `Proveedor`: `Nombre: string`, `Cuit: string`

**Métodos a implementar:**
- `ReponerStock(Producto producto, int cantidad)` → suma la cantidad al stock
- `DescontarStock(Producto producto, int cantidad)` → resta la cantidad si hay stock suficiente; si no, imprimir error con `Console.WriteLine()`
- `VerificarStock(Producto producto)` → retorna el stock actual

---

### 6. Gestión de Empleados

**Clases:**
- `Empleado`: `Nombre: string`, `Salario: double`
- `Departamento`: `Nombre: string`, `Empleados: List<Empleado>`

**Métodos a implementar:**
- `AgregarEmpleado(Departamento departamento, Empleado empleado)` → agrega el empleado a la lista
- `RemoverEmpleado(Departamento departamento, Empleado empleado)` → elimina el empleado de la lista
- `CalcularNomina(Departamento departamento)` → retorna la suma de todos los salarios del departamento

---

### 7. Sistema de Ventas

**Clases:**
- `Cliente`: `Nombre: string`, `Dni: string`, `Historial: List<Compra>`
- `Compra`: `Productos: List<Producto>`, `Total: double`

**Métodos a implementar:**
- `RealizarCompra(Cliente cliente, Compra compra)` → agrega la compra al historial del cliente
- `DevolverCompra(Cliente cliente, Compra compra)` → elimina la compra del historial
- `ConsultarHistorial(Cliente cliente)` → retorna la lista de compras del cliente

---

### 8. Gestión de Alquiler de Autos

**Clases:**
- `Auto`: `Patente: string`, `Disponible: bool`
- `Cliente`: `Nombre: string`, `Licencia: string`

**Métodos a implementar:**
- `AlquilarAuto(Auto auto, Cliente cliente)` → asigna el auto al cliente si está disponible y lo marca como no disponible
- `DevolverAuto(Auto auto, Cliente cliente)` → desvincula al cliente y marca el auto como disponible
- `VerificarDisponibilidad(Auto auto)` → retorna `true` si está libre

---

### 9. Sistema de Consultas Médicas

**Clases:**
- `Paciente`: `Nombre: string`, `Dni: string`, `Consultas: List<Consulta>`
- `Consulta`: `Fecha: string`, `Especialidad: string`

**Métodos a implementar:**
- `AgendarConsulta(Paciente paciente, Consulta consulta)` → agrega la consulta a la lista del paciente
- `CancelarConsulta(Paciente paciente, Consulta consulta)` → elimina la consulta de la lista
- `ListarConsultas(Paciente paciente)` → retorna la lista de consultas programadas

---

### 10. Gestión de Proyectos

**Clases:**
- `Proyecto`: `Nombre: string`, `Presupuesto: double`, `Empleados: List<Empleado>`
- `Empleado`: `Nombre: string`, `Puesto: string`, `Salario: double`

**Métodos a implementar:**
- `AsignarEmpleado(Proyecto proyecto, Empleado empleado)` → agrega al empleado al proyecto
- `RemoverEmpleado(Proyecto proyecto, Empleado empleado)` → elimina al empleado del proyecto
- `CalcularGasto(Proyecto proyecto)` → retorna la suma de salarios de todos los empleados asignados

---

### 11. Sistema de Vuelos

**Clases:**
- `Vuelo`: `Codigo: string`, `Capacidad: int`, `Pasajeros: List<Pasajero>`
- `Pasajero`: `Nombre: string`, `Pasaporte: string`

**Métodos a implementar:**
- `ReservarAsiento(Vuelo vuelo, Pasajero pasajero)` → agrega al pasajero si hay capacidad disponible (`Pasajeros.Count < Capacidad`)
- `CancelarReserva(Vuelo vuelo, Pasajero pasajero)` → elimina al pasajero del vuelo
- `VerificarCapacidad(Vuelo vuelo)` → retorna la cantidad de asientos disponibles (`Capacidad - Pasajeros.Count`)

---

### 12. Gestión de Alquiler de Equipos

**Clases:**
- `Equipo`: `Nombre: string`, `Disponible: bool`
- `Usuario`: `Nombre: string`, `Id: string`

**Métodos a implementar:**
- `AlquilarEquipo(Equipo equipo, Usuario usuario)` → asigna el equipo al usuario si está disponible y lo marca como no disponible
- `DevolverEquipo(Equipo equipo, Usuario usuario)` → desvincula al usuario y marca el equipo como disponible
- `VerificarDisponibilidad(Equipo equipo)` → retorna `true` si el equipo está libre

---

### 13. Sistema de Notas Escolares

**Clases:**
- `Estudiante`: `Nombre: string`, `Legajo: string`, `Notas: Dictionary<string, double>` (clave: nombre de la materia)
- `Materia`: `Nombre: string`

**Métodos a implementar:**
- `AsignarNota(Estudiante estudiante, Materia materia, double nota)` → registra la nota para esa materia en el diccionario del estudiante
- `ModificarNota(Estudiante estudiante, Materia materia, double nota)` → reemplaza la nota existente de esa materia
- `CalcularPromedio(Estudiante estudiante)` → retorna el promedio de todos los valores del diccionario de notas

---

### 14. Sistema de Turnos Médicos

**Clases:**
- `Medico`: `Nombre: string`, `Especialidad: string`, `Turnos: List<Turno>`
- `Turno`: `Fecha: string`, `Paciente: string`

**Métodos a implementar:**
- `AsignarTurno(Medico medico, Turno turno)` → agrega el turno a la agenda del médico
- `CancelarTurno(Medico medico, Turno turno)` → elimina el turno de la agenda
- `ConsultarAgenda(Medico medico)` → retorna la lista de turnos del médico

---

### 15. Gestión de Torneo Deportivo

**Clases:**
- `Equipo`: `Nombre: string`, `Puntos: int`
- `Partido`: `EquipoLocal: Equipo`, `EquipoVisitante: Equipo`, `Jugado: bool`

**Métodos a implementar:**
- `RegistrarResultado(Partido partido, int golesLocal, int golesVisitante)` → suma 3 puntos al ganador o 1 a cada equipo si hay empate, y marca el partido como jugado
- `ObtenerPuntos(Equipo equipo)` → retorna los puntos actuales del equipo
- `PartidoJugado(Partido partido)` → retorna `true` si el partido ya fue jugado
