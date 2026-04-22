# Ejercicios POO con C# - Práctica 3

## Configuración inicial

Crear la solución con dos proyectos separados, ejecutando los comandos en este orden dentro de una carpeta vacía:

```bash
# 1. Crear la solución
dotnet new sln -n EjerciciosPOO3

# 2. Crear el proyecto de lógica (class library, sin ejecutable)
dotnet new classlib -n EjerciciosPOO3

# 3. Crear el proyecto de consola (ejecutable)
dotnet new console -n AppConsola

# 4. Agregar ambos proyectos a la solución
dotnet sln add EjerciciosPOO3/EjerciciosPOO3.csproj
dotnet sln add AppConsola/AppConsola.csproj

# 5. Agregar la referencia: AppConsola depende de EjerciciosPOO3
dotnet add AppConsola/AppConsola.csproj reference EjerciciosPOO3/EjerciciosPOO3.csproj
```

Luego abrir la carpeta raíz (donde está `EjerciciosPOO3.sln`) en VS Code: `File → Open Folder`.

> Si no tenés instalado el SDK de .NET, descargarlo desde [dotnet.microsoft.com/download](https://dotnet.microsoft.com/download) seleccionando **.NET 8** (LTS).

## Jerarquía de proyectos

```
EjerciciosPOO3.sln
├── AppConsola/                   ← punto de entrada (ejecutable)
│   ├── AppConsola.csproj         ← referencia a EjerciciosPOO3
│   └── Program.cs
└── EjerciciosPOO3/               ← lógica del negocio (class library)
    ├── EjerciciosPOO3.csproj
    ├── Ejercicio01Vehiculos/
    │   ├── Vehiculo.cs
    │   ├── Auto.cs
    │   └── Moto.cs
    ├── Ejercicio02Bebidas/
    │   ├── Bebida.cs
    │   ├── Cafe.cs
    │   ├── Jugo.cs
    │   └── Agua.cs
    └── ...
```

**Dependencia:** `AppConsola` → `EjerciciosPOO3`

> Las clases de cada ejercicio van en `EjerciciosPOO3/`. `Program.cs` es el único punto de entrada: instancia y prueba las clases desde `AppConsola/`.

---

## Cómo leer los enunciados

Esta práctica tiene dos formatos de ejercicios:

**Ejercicios estructurados**: el enunciado detalla las clases, propiedades, interfaces y métodos a implementar. Tu trabajo es escribir el código siguiendo esa especificación.

**Ejercicios en párrafo**: el enunciado describe un problema del mundo real. Tu trabajo es **identificar las entidades, sus propiedades y comportamientos** a partir del texto, y modelarlos en clases C# aplicando herencia, interfaces, polimorfismo y clases abstractas según corresponda.

En los ejercicios en párrafo, prestá atención a:
- Los **sustantivos** → suelen ser clases o propiedades.
- Los **verbos** → suelen ser métodos.
- Las **relaciones "es un"** → suelen indicar herencia o clase abstracta.
- Las **capacidades compartidas entre clases sin relación directa** → suelen indicar interfaces.
- Las **condiciones** ("solo si", "lanzar error si", "no puede ser negativo") → suelen ser validaciones con `throw`.
- Las **frases como "ordenado por", "filtrar", "calcular el total de"** → suelen resolverse con LINQ.

---

## Ejemplo de referencia

El siguiente ejemplo muestra cómo estructurar código con clase abstracta, herencia, interfaz, polimorfismo y excepciones. Tomalo como modelo antes de resolver los ejercicios.

> Una empresa de medios tiene distintos tipos de contenidos: artículos y podcasts. Todos tienen un título y un autor. Los artículos tienen una cantidad de palabras; los podcasts tienen una duración en minutos. Todos deben poder calcularse su tiempo de lectura/escucha. Los artículos usan 200 palabras por minuto; los podcasts usan la duración directamente. El contenido debe implementar la interfaz `IPublicable` que obliga a tener un método `Publicar()`. No se puede crear contenido con título vacío.

```csharp
// IPublicable.cs
namespace EjerciciosPOO3.EjercicioReferencia;

public interface IPublicable
{
    void Publicar();
}
```

```csharp
// Contenido.cs
namespace EjerciciosPOO3.EjercicioReferencia;

public abstract class Contenido : IPublicable
{
    public string Titulo { get; set; }
    public string Autor { get; set; }

    public Contenido(string titulo, string autor)
    {
        if (string.IsNullOrWhiteSpace(titulo))
            throw new ArgumentException("El título no puede estar vacío.");

        Titulo = titulo;
        Autor = autor;
    }

    public abstract int CalcularTiempoMinutos();

    public void Publicar()
    {
        Console.WriteLine($"Publicando: \"{Titulo}\" de {Autor} ({CalcularTiempoMinutos()} min)");
    }
}
```

```csharp
// Articulo.cs
namespace EjerciciosPOO3.EjercicioReferencia;

public class Articulo : Contenido
{
    public int CantidadPalabras { get; set; }

    public Articulo(string titulo, string autor, int cantidadPalabras) : base(titulo, autor)
    {
        CantidadPalabras = cantidadPalabras;
    }

    public override int CalcularTiempoMinutos()
    {
        return CantidadPalabras / 200;
    }
}
```

```csharp
// Podcast.cs
namespace EjerciciosPOO3.EjercicioReferencia;

public class Podcast : Contenido
{
    public int DuracionMinutos { get; set; }

    public Podcast(string titulo, string autor, int duracionMinutos) : base(titulo, autor)
    {
        DuracionMinutos = duracionMinutos;
    }

    public override int CalcularTiempoMinutos()
    {
        return DuracionMinutos;
    }
}
```

```csharp
// Program.cs
using EjerciciosPOO3.EjercicioReferencia;

List<Contenido> contenidos = new List<Contenido>
{
    new Articulo("Introducción a C#", "Ana García", 1000),
    new Podcast("POO en la práctica", "Luis Pérez", 45),
    new Articulo("Herencia y Polimorfismo", "Carlos López", 600)
};

// Polimorfismo: cada uno ejecuta SU versión de CalcularTiempoMinutos
foreach (Contenido c in contenidos)
{
    c.Publicar();
}

// LINQ: ordenar por tiempo de lectura/escucha descendente
List<Contenido> ordenados = contenidos.OrderByDescending(c => c.CalcularTiempoMinutos()).ToList();
Console.WriteLine("\nOrdenados por duración:");
foreach (Contenido c in ordenados)
{
    Console.WriteLine($"  {c.Titulo}: {c.CalcularTiempoMinutos()} min");
}

// Manejo de excepción
try
{
    Articulo invalido = new Articulo("", "Sin autor", 100);
}
catch (ArgumentException ex)
{
    Console.WriteLine($"\nError: {ex.Message}");
}
```

---

## Ejercicios

---

### 1. Vehículos (Estructurado — Fácil)

**Clases:**
- `Vehiculo` (base): `Marca: string`, `Modelo: string`, `Anio: int`
  - Constructor: lanzar `ArgumentException` si `Anio` es menor a 1900
  - Método `virtual string Describir()` → devuelve `"{Marca} {Modelo} ({Anio})"`
- `Auto : Vehiculo`: `CantidadPuertas: int`
  - `override string Describir()` → agrega `" - {CantidadPuertas} puertas"` al resultado del padre (usá `base.Describir()`)
- `Moto : Vehiculo`: `TieneSidecar: bool`
  - `override string Describir()` → agrega `" - con sidecar"` o `" - sin sidecar"` según corresponda

**En `Program.cs`:**
- Crear al menos dos `Auto` y dos `Moto`.
- Guardarlos en una `List<Vehiculo>` y recorrerla llamando `Describir()` sobre cada uno.
- Capturar la excepción si se intenta crear un vehículo con año inválido.

---

### 2. Bebidas (Estructurado — Fácil)

**Clases:**
- `abstract Bebida`: `Nombre: string`, `Precio: decimal`
  - Constructor: lanzar `ArgumentException` si `Precio` es menor o igual a cero
  - `abstract string Preparar()` → sin implementación en la base
  - `string MostrarPrecio()` → devuelve `"{Nombre}: ${Precio:F2}"`
- `Cafe : Bebida`: `TieneLeche: bool`
  - `override string Preparar()` → describe cómo se prepara el café (con o sin leche)
- `Jugo : Bebida`: `Fruta: string`
  - `override string Preparar()` → describe cómo se prepara el jugo con esa fruta
- `Agua : Bebida`: `EsConGas: bool`
  - `override string Preparar()` → indica si es con gas o sin gas

**En `Program.cs`:**
- Crear una `List<Bebida>` con al menos una instancia de cada tipo.
- Recorrer la lista y llamar `Preparar()` y `MostrarPrecio()` sobre cada una.
- Intentar crear una bebida con precio negativo y capturar la excepción.

---

### 3. Publicaciones de una librería (Párrafo — Fácil)

Una librería vende distintos tipos de publicaciones: libros y revistas. Todas tienen un título, un precio y un año de publicación. Los libros tienen además un autor y un número de páginas; las revistas tienen un número de edición y una periodicidad (mensual, semanal o quincenal, usando un enum). El sistema debe poder describir cualquier publicación con un método que devuelva un texto con sus datos, listar todas las publicaciones ordenadas por precio de menor a mayor usando LINQ, y listar solo los libros usando LINQ. Si se intenta crear una publicación con precio negativo, se debe lanzar una excepción.

---

### 4. Catálogo de productos y servicios (Estructurado — Fácil)

**Interfaz:**
- `IDescribible`:
  - `string ObtenerDescripcion()` → devuelve un texto descriptivo del elemento

**Clases:**
- `Producto` (implementa `IDescribible`): `Nombre: string`, `Precio: decimal`, `Categoria: string`
  - `ObtenerDescripcion()` → devuelve `"Producto: {Nombre} | Categoría: {Categoria} | Precio: ${Precio:F2}"`
- `Servicio` (implementa `IDescribible`): `Nombre: string`, `PrecioHora: decimal`, `DuracionHoras: int`
  - `ObtenerDescripcion()` → devuelve `"Servicio: {Nombre} | Duración: {DuracionHoras}h | Total: ${PrecioHora * DuracionHoras:F2}"`

**En `Program.cs`:**
- Crear una `List<IDescribible>` con al menos dos `Producto` y dos `Servicio`.
- Recorrer la lista e imprimir la descripción de cada elemento.
- Usar LINQ para filtrar y mostrar solo los elementos que tengan un costo total mayor a $500.

---

### 5. Flota de vehículos de transporte (Párrafo — Fácil)

Una empresa de transporte maneja distintos tipos de vehículos en su flota. Cada vehículo tiene una patente, un año de fabricación y un estado que indica si está activo o en mantenimiento (usar un enum con esos dos valores). Los camiones tienen además una capacidad de carga en toneladas; las furgonetas tienen un volumen de carga en metros cúbicos. El sistema debe poder registrar vehículos, cambiar el estado de un vehículo, listar usando LINQ todos los que están activos, y obtener usando LINQ el camión con mayor capacidad de carga. Si se intenta registrar un vehículo con una patente ya existente en la flota, se debe lanzar una excepción.

---

### 6. Empleados y sueldos (Estructurado — Medio)

**Enum:** `TipoEmpleado { Permanente, Contratado, Pasante }`

**Interfaz:** `ICalculableSueldo` con `decimal CalcularSueldo()`

**Clases:**
- `abstract Empleado` (implementa `ICalculableSueldo`): `Nombre: string`, `Legajo: string`, `TipoEmpleado Tipo { get; }`
  - `abstract decimal CalcularSueldo()`
- `EmpleadoPermanente : Empleado`: `SueldoBase: decimal`, `AñosAntiguedad: int`, tipo siempre `Permanente`
  - `CalcularSueldo()` → `SueldoBase + (SueldoBase * 0.05m * AñosAntiguedad)`
- `EmpleadoContratado : Empleado`: `ValorHora: decimal`, `HorasTrabajadas: int`, tipo siempre `Contratado`
  - `CalcularSueldo()` → `ValorHora * HorasTrabajadas`
- `EmpleadoPasante : Empleado`: `Beca: decimal`, tipo siempre `Pasante`
  - `CalcularSueldo()` → devuelve `Beca` directamente

**Clase de servicio `RecursosHumanos`:**
- `Agregar(Empleado e)` → agrega el empleado a la lista interna; lanzar `ArgumentException` si ya existe un empleado con el mismo legajo
- `decimal CalcularNomina()` → suma los sueldos de todos los empleados con LINQ
- `Empleado ObtenerMejorPago()` → devuelve el empleado con mayor sueldo usando LINQ
- `List<Empleado> ListarPorTipo(TipoEmpleado tipo)` → filtra con LINQ

**En `Program.cs`:** instanciar `RecursosHumanos`, agregar empleados de los tres tipos y probar todos los métodos.

---

### 7. Documentos archivables (Estructurado — Medio)

**Interfaces:**
- `IImprimible`: `string GenerarReporte()`
- `IArchivable`: `bool EstaArchivado { get; }`, `void Archivar()`

**Clases:**
- `Documento` (implementa `IImprimible` e `IArchivable`): `Titulo: string`, `Fecha: string`, `Contenido: string`
  - `GenerarReporte()` → devuelve texto con título, fecha y contenido
  - `Archivar()` → marca el documento como archivado (no se puede desarchivar)
  - `EstaArchivado` → propiedad de solo lectura
- `Contrato : Documento`: `MontoTotal: decimal`, `Vigencia: string`
  - `override GenerarReporte()` → extiende el reporte del padre agregando monto y vigencia
- `Acta : Documento`: `Participantes: List<string>`
  - `override GenerarReporte()` → extiende el reporte del padre listando los participantes

**En `Program.cs`:**
- Crear una `List<Documento>` con al menos un `Contrato` y un `Acta`.
- Archivar algunos documentos.
- Usar LINQ para listar los que todavía no están archivados.
- Imprimir el reporte generado de cada documento.

---

### 8. Academia de música (Párrafo — Medio)

Una academia de música tiene profesores y alumnos. Los profesores tienen un nombre, una matrícula y el instrumento que enseñan (usar un enum con al menos Guitarra, Piano, Violin y Bateria). Los alumnos tienen un nombre, un número de legajo y pueden estar inscriptos en varias clases. Cada clase tiene un profesor asignado, un día de la semana (usar el enum `DiaSemana` estándar de C# o definir uno propio con los siete días) y una duración en minutos. El sistema debe poder inscribir un alumno en una clase (lanzando una excepción si el alumno ya está inscripto en esa clase), cancelar la inscripción, listar las clases de un alumno ordenadas por día de la semana usando LINQ, y calcular el total de minutos de estudio semanal de un alumno sumando las duraciones de todas sus clases con LINQ.

---

### 9. Catálogo de publicaciones (Estructurado — Medio)

**Enum:** `Categoria { Ficcion, NoFiccion, Infantil, Tecnico }`

**Clases:**
- `abstract Publicacion`: `Titulo: string`, `Precio: decimal`, `Categoria Categoria { get; }`
  - Constructor: lanzar `ArgumentException` si `Titulo` está vacío o si `Precio` es negativo
  - `abstract string ObtenerResumen()`
- `Libro : Publicacion`: `Autor: string`, `Paginas: int`
  - `ObtenerResumen()` → texto con título, autor y páginas
- `Revista : Publicacion`: `NumeroEdicion: int`, `EsMensual: bool`
  - `ObtenerResumen()` → texto con título, número de edición y frecuencia
- `Ebook : Publicacion`: `FormatoArchivo: string`, `TamanioMB: double`
  - `ObtenerResumen()` → texto con título, formato y tamaño

**Clase de servicio `Catalogo`:**
- `Agregar(Publicacion p)` → agrega la publicación
- `List<Publicacion> BuscarPorCategoria(Categoria c)` → filtra con LINQ
- `List<Publicacion> ObtenerMasCaros(int n)` → devuelve los `n` más caros con LINQ
- `void MostrarTodo()` → imprime el resumen de cada publicación

**En `Program.cs`:** instanciar `Catalogo`, agregar publicaciones de los tres tipos y probar todos los métodos.

---

### 10. Plataforma de e-learning (Párrafo — Medio)

Una plataforma educativa ofrece distintos tipos de contenido: videos, documentos PDF y quizzes. Todos los contenidos tienen un título, una descripción y una duración estimada en minutos. Los videos tienen una URL y una resolución; los PDFs tienen un tamaño en MB y una cantidad de páginas; los quizzes tienen una cantidad de preguntas y un puntaje máximo. Todos los contenidos deben poder ser completados por un usuario, pero la forma varía según el tipo: un video se marca como visto, un PDF se marca como leído, y un quiz requiere ingresar el puntaje obtenido (que no puede superar el puntaje máximo, de lo contrario se lanza una excepción). El sistema debe poder registrar usuarios, completar contenidos, listar usando LINQ los contenidos completados de un usuario y calcular el tiempo total invertido sumando las duraciones de los contenidos completados.

---

### 11. Pólizas de seguro (Párrafo — Difícil)

Una empresa de seguros ofrece distintos tipos de pólizas. Todas tienen un número único, el nombre del asegurado, una fecha de inicio (como texto) y una prima mensual. Las pólizas de auto incluyen la patente y el modelo del vehículo asegurado; las de hogar incluyen la dirección y la superficie cubierta en metros cuadrados; las de vida incluyen el monto de cobertura y una lista de beneficiarios. Todas las pólizas deben poder calcular su costo anual (prima × 12), pero las pólizas de vida aplican un recargo del 15% si el monto de cobertura supera los $500.000. El estado de cada póliza se maneja con un enum (Activa, Suspendida, Vencida). El sistema debe permitir registrar pólizas (lanzando excepción si la prima es negativa o cero), cambiar el estado de una póliza, listar las pólizas activas de un asegurado usando LINQ, calcular el gasto anual total de un asegurado sumando los costos anuales de sus pólizas activas, y obtener el listado completo de pólizas ordenadas por prima de mayor a menor.

---

### 12. Gestor de tareas (Estructurado — Difícil)

**Enum:** `EstadoTarea { Pendiente, EnProgreso, Completada, Cancelada }`

**Interfaz:** `INotificable` con `void EnviarNotificacion(string mensaje)`

**Clases (dominio):**
- `abstract Tarea`: `Id: int`, `Titulo: string`, `Descripcion: string`, `EstadoTarea Estado`
  - `abstract string ObtenerTipo()`
  - `virtual void CambiarEstado(EstadoTarea nuevo)` → lanzar `InvalidOperationException` si la tarea ya está `Cancelada`
- `TareaSimple : Tarea`: `FechaLimite: string`
  - `ObtenerTipo()` → devuelve `"Simple"`
- `TareaConAprobacion : Tarea`, implementa `INotificable`: `Aprobador: string`
  - `ObtenerTipo()` → devuelve `"Con Aprobación"`
  - `EnviarNotificacion(string mensaje)` → imprime `"[Notificación a {Aprobador}]: {mensaje}"`
  - Al cambiar estado a `Completada`, enviar automáticamente una notificación al aprobador

**Clase de servicio `GestorTareas`:**
- `AgregarTarea(Tarea t)` → lanzar `ArgumentException` si ya existe una tarea con el mismo `Id`
- `CambiarEstadoTarea(int id, EstadoTarea nuevo)` → busca la tarea y llama `CambiarEstado()`; lanzar `InvalidOperationException` si el id no existe
- `List<Tarea> ObtenerPorEstado(EstadoTarea estado)` → filtra con LINQ
- `void MostrarResumen()` → imprime cada tarea con su tipo, título y estado, usando LINQ para ordenarlas por `Id`

**En `Program.cs`:** instanciar `GestorTareas`, agregar tareas de ambos tipos, cambiar estados y probar todos los métodos.

---

### 13. Cadena de gimnasios (Párrafo — Difícil)

Una cadena de gimnasios necesita gestionar sucursales, socios y rutinas. Cada sucursal tiene un nombre, una dirección y una lista de socios habilitados. Los socios tienen un nombre, un número de socio único, una fecha de vencimiento de cuota (como texto en formato `yyyy-MM-dd`) y un tipo de membresía (Basica, Premium o Vip, usando un enum). Las rutinas tienen un nombre, un tipo (Cardio, Musculacion o Funcional, usando un enum), una duración en minutos y un nivel de dificultad (Principiante, Intermedio o Avanzado, usando un enum). Cada tipo de membresía da acceso a distintas rutinas: los socios Básicos solo acceden a rutinas de tipo Cardio; los Premium acceden a Cardio y Musculacion; los Vip acceden a todas. El sistema debe registrar socios en una sucursal (lanzando excepción si ya existe un socio con ese número en esa sucursal), verificar si un socio puede acceder a una rutina según su membresía, renovar la cuota de un socio actualizando la fecha de vencimiento, listar usando LINQ las rutinas accesibles para un socio dado, y calcular el tiempo total de entrenamiento disponible para un socio sumando las duraciones de sus rutinas accesibles.

---

### 14. Empresa de logística (Párrafo — Difícil)

Una empresa de logística rastrea el envío de paquetes. Un paquete tiene un código único, un remitente, un destinatario, un peso en kilogramos y un estado de envío (usando un enum con los valores: Registrado, EnDeposito, EnTransito, Entregado, Devuelto). Los paquetes pueden ser estándar o express; los express tienen una fecha límite de entrega y un recargo fijo del 30% sobre el costo base. El costo base se calcula multiplicando el peso por una tarifa definida como constante (`const decimal TARIFA_POR_KG = 150m`). El sistema debe registrar paquetes, avanzar el estado siguiendo la secuencia lógica (Registrado → EnDeposito → EnTransito → Entregado), lanzando una excepción si se intenta un cambio de estado inválido (por ejemplo saltar pasos o modificar un paquete Devuelto). También debe poder calcular el costo de envío de cada paquete según su tipo, y listar usando LINQ todos los paquetes en tránsito ordenados por peso de mayor a menor.

---

### 15. Sistema de acceso con usuarios (Estructurado — Difícil)

**Enum:** `NivelAcceso { Empleado, Supervisor, Administrador }`

**Interfaces:**
- `IAutenticable`: `bool Autenticar(string clave)`, `NivelAcceso Nivel { get; }`
- `IAuditable`: `string UltimoAcceso { get; }`, `void RegistrarAcceso(string fecha)`

**Clases:**
- `abstract Usuario` (implementa `IAutenticable` e `IAuditable`): `Nombre: string`, `Email: string`
  - Almacena internamente la clave (campo privado)
  - `Autenticar(string clave)` → compara la clave y si es correcta llama `RegistrarAcceso()` con la fecha actual (podés usar `DateTime.Now.ToString("yyyy-MM-dd HH:mm")`)
  - `RegistrarAcceso(string fecha)` → actualiza `UltimoAcceso`
- `UsuarioEmpleado : Usuario`: `Departamento: string`, `Nivel` siempre `Empleado`
- `UsuarioSupervisor : Usuario`: `Area: string`, `Nivel` siempre `Supervisor`
- `UsuarioAdmin : Usuario`: `Nivel` siempre `Administrador`

**Clase de servicio `SistemaAcceso`:**
- `RegistrarUsuario(Usuario u)` → lanzar `ArgumentException` si ya existe un usuario con el mismo email
- `Usuario? Autenticar(string email, string clave)` → busca el usuario y llama `Autenticar()`; devuelve el usuario si la clave es correcta, `null` si no
- `List<Usuario> ObtenerPorNivel(NivelAcceso nivel)` → filtra con LINQ
- `List<Usuario> ObtenerUltimosAccesos(int n)` → devuelve los `n` usuarios con accesos más recientes, ordenados por `UltimoAcceso` descendente con LINQ (los que nunca accedieron van al final)

**En `Program.cs`:** instanciar `SistemaAcceso`, registrar usuarios de los tres tipos, autenticar algunos y probar todos los métodos.

---

### 16. Venta de entradas para eventos (Párrafo — Difícil)

Una plataforma de venta de entradas gestiona distintos tipos de eventos y sus localidades. Todos los eventos tienen un nombre, una fecha (como texto en formato `yyyy-MM-dd`), un lugar y una capacidad total. Los recitales tienen un artista principal y un género musical; los eventos deportivos tienen dos equipos o competidores y una disciplina; las obras de teatro tienen un elenco (lista de actores) y una duración en minutos. Cada evento tiene localidades de distintas categorías (usar un enum: Campo, Platea, Palco), cada categoría con un precio diferente y una cantidad de asientos disponibles. El sistema debe permitir vender entradas para una categoría específica de un evento (disminuyendo la disponibilidad, lanzando excepción si no quedan asientos en esa categoría o si la fecha del evento ya pasó), cancelar una entrada (devolviendo el asiento), calcular la recaudación total de un evento sumando los precios de las entradas vendidas usando LINQ, y listar todos los eventos con entradas disponibles en alguna categoría, ordenados por fecha.

---

### 17. Clínica veterinaria (Párrafo — Difícil)

Una clínica veterinaria gestiona fichas médicas de animales. Cada animal tiene un nombre, una especie (usar un enum: Perro, Gato, Ave, Reptil, Otro), un peso y una edad. Los propietarios tienen un nombre, un teléfono y una lista de animales registrados. Cada consulta veterinaria tiene una fecha (como texto), un motivo, un diagnóstico y el tratamiento indicado. Hay dos tipos de veterinarios: los generalistas pueden atender cualquier especie, y los especialistas solo atienden ciertas especies almacenadas en una lista. Todos los veterinarios tienen nombre y matrícula, y deben poder indicar si pueden atender a un animal de una especie dada. El sistema debe permitir registrar propietarios y sus animales (lanzando excepción si el propietario ya tiene un animal con el mismo nombre), registrar consultas asignando el veterinario más adecuado para la especie del animal, y listar usando LINQ el historial de consultas de un animal ordenado por fecha. También debe poder listar todos los animales de una especie específica registrados en la clínica.

---

### 18. Gestión académica universitaria (Párrafo — Difícil)

Un sistema académico universitario maneja materias, docentes y calificaciones de alumnos. Las materias tienen un código, un nombre y una cantidad de créditos. Los docentes pueden ser titulares o adjuntos: todos tienen nombre y legajo, pero los titulares tienen además una dedicación (Simple, Parcial o Exclusiva, usando un enum), y los adjuntos están vinculados al legajo de un docente titular. Cada alumno tiene un nombre, un legajo y un registro de calificaciones. Una calificación tiene nota numérica, fecha (como texto) y condición (Regular, Libre o Ausente, usando un enum). El sistema debe poder registrar calificaciones (lanzando una excepción si la nota no está entre 0 y 10, o si el alumno ya tiene una calificación registrada para esa materia en la misma fecha), calcular el promedio de un alumno considerando solo las notas con condición `Regular` usando LINQ, determinar si un alumno puede rendir el final (promedio de regulares mayor o igual a 4), y listar los alumnos de una materia ordenados por promedio descendente usando LINQ.

---

### 19. Gestión de proyectos de software (Párrafo — Difícil)

Una empresa de desarrollo de software gestiona proyectos, equipos y empleados. Los empleados tienen distintos roles: desarrolladores, testers o líderes técnicos. Todos comparten nombre, legajo y seniority (Junior, SemiSenior o Senior, usando un enum). Los desarrolladores tienen un stack tecnológico (lista de tecnologías como strings) y una especialización (Frontend, Backend o Fullstack, usando un enum); los testers tienen el tipo de testing que practican (Manual, Automatizado o Ambos, usando un enum); los líderes técnicos tienen una lista de empleados a su cargo. Un proyecto tiene un nombre, una fecha de inicio (como texto), un presupuesto y un estado (Planificado, Activo, Pausado o Finalizado, usando un enum). El costo mensual de cada empleado está definido como constantes según su seniority: Junior $100.000, SemiSenior $180.000, Senior $280.000. El sistema debe poder asignar empleados a proyectos (lanzando excepción si el empleado ya está en ese proyecto), calcular el costo mensual total del equipo de un proyecto sumando los costos de sus integrantes, listar usando LINQ los proyectos activos ordenados por fecha de inicio, y obtener el empleado con mayor cantidad de proyectos activos asignados.

---

### 20. Facturación con productos físicos y digitales (Párrafo — Difícil)

Una empresa retail necesita un sistema de facturación con separación en capas. Los productos del catálogo pueden ser físicos o digitales: los físicos tienen peso en kg, dimensiones y stock disponible; los digitales tienen un enlace de descarga y no tienen stock limitado. Ambos tipos tienen nombre, precio base y una categoría (usando un enum). El precio final de cada producto se calcula aplicando IVA: los físicos aplican 21% y los digitales aplican 10,5%. Una factura pertenece a un cliente (con nombre y CUIT), tiene un número único, una lista de líneas (cada línea tiene un producto y una cantidad), y un estado (Borrador, Emitida o Anulada, usando un enum). Las reglas son: solo se pueden agregar o quitar líneas en estado Borrador (lanzar excepción si no); al emitir, verificar que haya stock suficiente para todos los productos físicos (lanzar excepción indicando qué producto no tiene stock); al emitir, descontar el stock de los productos físicos. La capa de presentación (`Program.cs`) solo debe comunicarse con la capa de servicio; la capa de servicio contiene toda la lógica de negocio; la capa de dominio contiene las clases del modelo. El sistema debe calcular el subtotal, el IVA total y el total de cada factura usando LINQ, listar las facturas emitidas de un cliente, y obtener el producto más vendido considerando todas las facturas emitidas.

---

> Unidad 2 - Práctica 3 | Programación 1 | UCSE
