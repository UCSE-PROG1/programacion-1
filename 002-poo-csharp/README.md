# Unidad 2 - Programación Orientada a Objetos con C#

Esta unidad cubre los conceptos fundamentales de la Programación Orientada a Objetos aplicados en C#, desde la definición de clases y manejo de errores hasta los pilares de la POO, herencia, interfaces, polimorfismo y arquitectura por capas.

---

## Tabla de contenidos

1. [Namespaces y using](#1-namespaces-y-using)
2. [Clases y Objetos](#2-clases-y-objetos)
3. [Constructores](#3-constructores)
4. [Propiedades (get/set)](#4-propiedades-getset)
5. [Arrays](#5-arrays)
6. [Listas (List\<T\>)](#6-listas-listt)
7. [Excepciones](#7-excepciones)
8. [Constantes y Enumeradores](#8-constantes-y-enumeradores)
9. [Null y NullReferenceException](#9-null-y-nullreferenceexception)
10. [LINQ y Expresiones Lambda](#10-linq-y-expresiones-lambda)
11. [Programación Orientada a Objetos - Los 4 Pilares](#11-programación-orientada-a-objetos---los-4-pilares)
12. [Herencia](#12-herencia)
13. [Interfaces](#13-interfaces)
14. [Polimorfismo](#14-polimorfismo)
15. [Clases Abstractas](#15-clases-abstractas)
16. [Operadores is y as](#16-operadores-is-y-as)
17. [Arquitectura por Capas](#17-arquitectura-por-capas)
18. [Estándares de Programación](#18-estándares-de-programación)

---

## 1. Namespaces y using

Un **namespace** (espacio de nombres) es una forma de organizar el código agrupando clases relacionadas bajo un nombre común. Evita conflictos cuando dos clases tienen el mismo nombre en distintas partes de un proyecto. En .NET el namespace suele coincidir con la estructura de carpetas del proyecto.

### Declarar un namespace

```csharp
namespace MiApp.Dominio
{
    public class Persona
    {
        public string Nombre { get; set; }
    }
}

namespace MiApp.Servicios
{
    public class PersonaService
    {
        // PersonaService y Persona están en namespaces distintos
    }
}
```

En .NET 6+ se puede usar la sintaxis de **namespace con ámbito de archivo**, más compacta y sin llaves adicionales:

```csharp
namespace MiApp.Dominio; // aplica a todo el archivo

public class Persona
{
    public string Nombre { get; set; }
}
```

### Usar clases de otros namespaces con `using`

Para usar una clase de otro namespace sin escribir la ruta completa en cada lugar, se agrega una directiva `using` al inicio del archivo.

```csharp
// Sin using: hay que escribir el nombre completo cada vez
MiApp.Dominio.Persona p = new MiApp.Dominio.Persona();

// Con using: se importa el namespace y ya no hace falta el prefijo
using MiApp.Dominio;

Persona p = new Persona();
```

### Namespaces del framework que ya usamos

En .NET 6+ los namespaces más comunes se importan automáticamente mediante **global usings**, por eso podés usar `Console`, `List<T>` o `Math` sin declarar nada al inicio del archivo.

| Namespace | Qué contiene |
|-----------|-------------|
| `System` | `Console`, `Math`, `Convert`, `Exception` |
| `System.Collections.Generic` | `List<T>`, `Dictionary<K,V>`, `HashSet<T>` |
| `System.Linq` | Métodos de extensión LINQ (`Where`, `Select`, etc.) |
| `System.IO` | Lectura y escritura de archivos |

---

## 2. Clases y Objetos

Una **clase** es una plantilla o molde que define cómo será un objeto: qué datos (propiedades) tiene y qué acciones (métodos) puede realizar.

Un **objeto** es una instancia concreta de esa clase.

### Modificadores de acceso

| Modificador | Accesible desde |
|-------------|-----------------|
| `public` | Desde cualquier lugar |
| `private` | Solo dentro de la misma clase |
| `protected` | Dentro de la clase y sus clases derivadas |
| `internal` | Dentro del mismo proyecto (ensamblado) |

```csharp
// Definición de la clase Persona
public class Persona
{
    // Propiedades
    public string Nombre { get; set; }
    public int Edad { get; set; }

    // Constructor
    public Persona(string nombre, int edad)
    {
        Nombre = nombre;
        Edad = edad;
    }

    // Método
    public string Saludar()
    {
        return $"Hola, mi nombre es {Nombre} y tengo {Edad} años.";
    }
}

// Uso de la clase (creación de objetos)
Persona persona1 = new Persona("Carlos", 30);
Persona persona2 = new Persona("Laura", 25);

Console.WriteLine(persona1.Saludar()); // Hola, mi nombre es Carlos y tengo 30 años.
Console.WriteLine(persona2.Saludar()); // Hola, mi nombre es Laura y tengo 25 años.

// Modificar una propiedad
persona1.Edad = 31;
Console.WriteLine($"{persona1.Nombre} ahora tiene {persona1.Edad} años.");
```

### Miembros estáticos (`static`)

Un miembro **estático** pertenece a la clase en sí, no a una instancia. Se accede a través del nombre de la clase, no de un objeto. Todos los objetos comparten el mismo valor del miembro estático.

```csharp
public class Contador
{
    // Campo estático: compartido por todas las instancias
    private static int totalCreados = 0;

    public string Nombre { get; set; }

    public Contador(string nombre)
    {
        Nombre = nombre;
        totalCreados++; // se incrementa cada vez que se crea un objeto
    }

    // Método estático: se llama desde la clase, no desde un objeto
    public static int ObtenerTotal()
    {
        return totalCreados;
    }
}

// Uso
Contador c1 = new Contador("Primero");
Contador c2 = new Contador("Segundo");
Contador c3 = new Contador("Tercero");

Console.WriteLine(Contador.ObtenerTotal()); // 3 — se llama desde la clase, no desde c1
```

Las clases utilitarias del framework como `Math` y `Console` son **clases estáticas**: no se pueden instanciar, solo tienen miembros estáticos.

```csharp
double raiz = Math.Sqrt(16);       // Math es una clase estática
Console.WriteLine("Hola");         // Console también
```

> **Cuándo usar `static`**: para datos o comportamientos que son propios de la clase entera, no de cada objeto (contadores globales, métodos utilitarios, constantes de configuración). Evitá abusar de `static`; si todo es estático, estás escribiendo código procedural, no orientado a objetos.

### Sobreescribir `ToString()`

Por defecto, `Console.WriteLine(objeto)` imprime el nombre del tipo (`MiApp.Persona`), lo cual no es útil. Sobreescribiendo `ToString()` podés controlar cómo se representa un objeto como texto.

```csharp
public class Producto
{
    public string Nombre { get; set; }
    public decimal Precio { get; set; }
    public string Categoria { get; set; }

    public Producto(string nombre, decimal precio, string categoria)
    {
        Nombre = nombre;
        Precio = precio;
        Categoria = categoria;
    }

    // Sobreescribir ToString() para obtener una representación útil
    public override string ToString()
    {
        return $"{Nombre} (${Precio:F2}) - {Categoria}";
    }
}

// Uso
Producto p = new Producto("Laptop", 1200m, "Electrónica");

Console.WriteLine(p);              // Laptop ($1200.00) - Electrónica
Console.WriteLine(p.ToString());   // Laptop ($1200.00) - Electrónica

// También se usa automáticamente al interpolar en strings
Console.WriteLine($"Producto: {p}"); // Producto: Laptop ($1200.00) - Electrónica
```

---

## 3. Constructores

El **constructor** es un método especial que se ejecuta automáticamente cuando se crea un objeto con `new`. Su función es inicializar el objeto con valores válidos.

- Tiene el **mismo nombre que la clase**.
- **No tiene tipo de retorno** (ni siquiera `void`).
- Puede haber múltiples constructores con distintos parámetros (sobrecarga).

### Constructor por defecto

Si no definís ningún constructor, C# provee uno vacío automáticamente. Pero si definís al menos uno, el constructor vacío automático desaparece (debés declararlo vos si lo necesitás).

```csharp
public class Auto
{
    public string Marca { get; set; }
    public string Modelo { get; set; }
    public int Anio { get; set; }

    // Constructor parametrizado
    public Auto(string marca, string modelo, int anio)
    {
        Marca = marca;
        Modelo = modelo;
        Anio = anio;
    }

    // Constructor por defecto (definido manualmente)
    public Auto()
    {
        Marca = "Desconocida";
        Modelo = "Desconocido";
        Anio = 2024;
    }

    public string ObtenerInfo()
    {
        return $"{Marca} {Modelo} ({Anio})";
    }
}

// Con el constructor parametrizado
Auto auto1 = new Auto("Toyota", "Corolla", 2022);
Console.WriteLine(auto1.ObtenerInfo()); // Toyota Corolla (2022)

// Con el constructor por defecto
Auto auto2 = new Auto();
Console.WriteLine(auto2.ObtenerInfo()); // Desconocida Desconocido (2024)
```

### Sobrecarga de constructores (y métodos)

Tener dos constructores con distinta firma es un ejemplo de **sobrecarga**: mismo nombre, distintos parámetros. C# elige cuál ejecutar en **tiempo de compilación** según los argumentos que se pasan.

La sobrecarga no es exclusiva de constructores: cualquier método puede sobrecargarse de la misma manera.

```csharp
public class Saludo
{
    // Sobrecarga: mismo nombre, distintas firmas
    public string Saludar()
    {
        return "¡Hola!";
    }

    public string Saludar(string nombre)
    {
        return $"¡Hola, {nombre}!";
    }

    public string Saludar(string nombre, string idioma)
    {
        return idioma == "en" ? $"Hello, {nombre}!" : $"¡Hola, {nombre}!";
    }
}

Saludo s = new Saludo();
Console.WriteLine(s.Saludar());                  // ¡Hola!
Console.WriteLine(s.Saludar("Ana"));             // ¡Hola, Ana!
Console.WriteLine(s.Saludar("Ana", "en"));       // Hello, Ana!
```

### Encadenamiento de constructores (`this(...)`)

Cuando tenés múltiples constructores sobrecargados, es común que repitas la lógica de inicialización en cada uno. El encadenamiento con `this(...)` permite que un constructor llame a otro de la misma clase, evitando duplicación.

```csharp
public class Empleado
{
    public string Nombre { get; set; }
    public string Puesto { get; set; }
    public decimal Sueldo { get; set; }

    // Constructor completo: es el que tiene toda la lógica
    public Empleado(string nombre, string puesto, decimal sueldo)
    {
        Nombre = nombre;
        Puesto = puesto;
        Sueldo = sueldo;
    }

    // Llama al constructor completo con un valor por defecto para sueldo
    public Empleado(string nombre, string puesto) : this(nombre, puesto, 0m) { }

    // Llama al anterior con un valor por defecto para puesto
    public Empleado(string nombre) : this(nombre, "Sin asignar") { }
}

// Los tres son válidos
Empleado e1 = new Empleado("Ana", "Desarrolladora", 150000m);
Empleado e2 = new Empleado("Luis", "QA");       // sueldo = 0
Empleado e3 = new Empleado("Marta");            // puesto = "Sin asignar", sueldo = 0
```

> La lógica de validación e inicialización se escribe **una sola vez** en el constructor más completo. Los demás simplemente delegan con valores por defecto.

### Inicializadores de objetos

C# permite inicializar propiedades directamente al crear un objeto con la sintaxis `{ Propiedad = valor }`, sin necesidad de un constructor parametrizado para cada combinación posible.

```csharp
public class Direccion
{
    public string Calle { get; set; }
    public int Numero { get; set; }
    public string Ciudad { get; set; }
    public string Pais { get; set; }
}

// Con inicializador de objetos: no hace falta un constructor con 4 parámetros
Direccion d = new Direccion
{
    Calle = "Av. Corrientes",
    Numero = 1234,
    Ciudad = "Buenos Aires",
    Pais = "Argentina"
};

// También se puede combinar con un constructor y luego inicializar el resto
public class Persona
{
    public string Nombre { get; set; }
    public int Edad { get; set; }
    public Direccion Domicilio { get; set; }

    public Persona(string nombre)
    {
        Nombre = nombre;
    }
}

Persona p = new Persona("Carlos")
{
    Edad = 30,
    Domicilio = new Direccion { Calle = "San Martín", Numero = 500 }
};
```

> Usá inicializadores para **propiedades opcionales**. Usá constructores para **datos obligatorios** que el objeto necesita para ser válido desde el momento en que se crea.

---

## 4. Propiedades (get/set)

Las propiedades en C# son miembros de una clase que permiten controlar el acceso a los datos internos. Son preferibles a los campos públicos porque permiten agregar lógica de validación.

### Propiedad con getter y setter

```csharp
public class CuentaBancaria
{
    // Campo privado (backing field)
    private decimal saldo;

    // Propiedad con validación en el setter
    public decimal Saldo
    {
        get { return saldo; }
        private set
        {
            if (value < 0)
                throw new ArgumentException("El saldo no puede ser negativo.");
            saldo = value;
        }
    }

    // Propiedad auto-implementada (sin lógica adicional)
    public string Titular { get; set; }

    // Propiedad de solo lectura (solo get)
    public string NumeroCuenta { get; }

    public CuentaBancaria(string titular, string numeroCuenta, decimal saldoInicial)
    {
        Titular = titular;
        NumeroCuenta = numeroCuenta;
        Saldo = saldoInicial;
    }

    public void Depositar(decimal monto)
    {
        if (monto <= 0)
            throw new ArgumentException("El monto a depositar debe ser mayor a cero.");

        Saldo += monto;
        Console.WriteLine($"Depósito de ${monto:F2} realizado. Saldo actual: ${Saldo:F2}");
    }

    public void Extraer(decimal monto)
    {
        if (monto <= 0)
            throw new ArgumentException("El monto a extraer debe ser mayor a cero.");
        if (monto > Saldo)
            throw new InvalidOperationException("Saldo insuficiente.");

        Saldo -= monto;
        Console.WriteLine($"Extracción de ${monto:F2} realizada. Saldo actual: ${Saldo:F2}");
    }
}

// Uso
CuentaBancaria cuenta = new CuentaBancaria("Ana García", "001-234567", 1000m);
cuenta.Depositar(500m);   // Depósito de $500.00 realizado. Saldo actual: $1500.00
cuenta.Extraer(200m);     // Extracción de $200.00 realizada. Saldo actual: $1300.00

Console.WriteLine(cuenta.NumeroCuenta); // 001-234567 (solo lectura, no se puede asignar desde afuera)
```

---

## 5. Arrays

Un **array** (arreglo) es una estructura de datos de **tamaño fijo** que almacena elementos del mismo tipo en posiciones contiguas de memoria. Los índices empiezan en `0`.

```csharp
// Declaración e inicialización
int[] notas = new int[5]; // array de 5 enteros, todos inicializados en 0
notas[0] = 85;
notas[1] = 72;
notas[2] = 90;
notas[3] = 68;
notas[4] = 77;

// Inicialización directa con valores
int[] notasDirectas = { 85, 72, 90, 68, 77 };

// Acceder a un elemento
Console.WriteLine($"Primera nota: {notas[0]}"); // 85
Console.WriteLine($"Última nota: {notas[notas.Length - 1]}"); // 77

// Calcular el promedio
int suma = 0;
for (int i = 0; i < notas.Length; i++)
{
    suma += notas[i];
}
double promedio = (double)suma / notas.Length;
Console.WriteLine($"Promedio: {promedio:F2}"); // Promedio: 78.40

// Recorrer con foreach (más simple cuando no necesitás el índice)
Console.WriteLine("Todas las notas:");
foreach (int nota in notas)
{
    Console.Write($"{nota} ");
}
```

> **Limitación**: el tamaño de un array no puede cambiar después de crearlo. Si necesitás una colección de tamaño dinámico, usá `List<T>`.

---

## 6. Listas (List\<T\>)

Una **`List<T>`** es una colección de tamaño **dinámico** que puede crecer o reducirse en tiempo de ejecución. Es la estructura más utilizada en C# para manejar colecciones de objetos.

| Método / Propiedad | Descripción |
|--------------------|-------------|
| `Add(elemento)` | Agrega un elemento al final |
| `Remove(elemento)` | Elimina la primera ocurrencia del elemento |
| `RemoveAt(indice)` | Elimina el elemento en el índice indicado |
| `Contains(elemento)` | Devuelve `true` si el elemento existe |
| `IndexOf(elemento)` | Devuelve el índice del elemento (-1 si no existe) |
| `Count` | Cantidad de elementos en la lista |
| `Clear()` | Elimina todos los elementos |

```csharp
// Crear y gestionar una lista de alumnos
List<string> alumnos = new List<string>();

// Agregar alumnos
alumnos.Add("Carlos Pérez");
alumnos.Add("Ana García");
alumnos.Add("Luis Martínez");
alumnos.Add("María López");

Console.WriteLine($"Total de alumnos: {alumnos.Count}"); // 4

// Verificar si existe
bool existe = alumnos.Contains("Ana García");
Console.WriteLine($"¿Existe Ana García? {existe}"); // True

// Obtener índice
int indice = alumnos.IndexOf("Luis Martínez");
Console.WriteLine($"Índice de Luis Martínez: {indice}"); // 2

// Eliminar por nombre
alumnos.Remove("Luis Martínez");
Console.WriteLine($"Alumnos después de eliminar: {alumnos.Count}"); // 3

// Eliminar por índice
alumnos.RemoveAt(0); // elimina "Carlos Pérez"
Console.WriteLine($"Alumnos después de eliminar por índice: {alumnos.Count}"); // 2

// Recorrer la lista
Console.WriteLine("Lista de alumnos:");
foreach (string alumno in alumnos)
{
    Console.WriteLine($"  - {alumno}");
}

// Limpiar la lista
alumnos.Clear();
Console.WriteLine($"Alumnos después de limpiar: {alumnos.Count}"); // 0
```

---

## 7. Excepciones

Una **excepción** es un error que ocurre durante la ejecución del programa e interrumpe el flujo normal. C# usa un sistema estructurado de manejo de excepciones con los bloques `try`, `catch` y `finally`.

Cuando un error ocurre dentro de un bloque `try`, C# lanza una excepción que puede ser **capturada** por un bloque `catch`. El bloque `finally` se ejecuta siempre, independientemente de si hubo error o no.

### Estructura básica

```csharp
try
{
    // Código que puede fallar
    int resultado = 10 / 0; // DivideByZeroException
}
catch (DivideByZeroException ex)
{
    // Se ejecuta si ocurre ese tipo específico de excepción
    Console.WriteLine($"Error: no se puede dividir por cero. {ex.Message}");
}
finally
{
    // Se ejecuta siempre, haya o no excepción
    Console.WriteLine("Este bloque siempre se ejecuta.");
}
```

### Múltiples bloques catch

Podés capturar distintos tipos de excepciones con bloques `catch` separados. C# evalúa los bloques en orden y ejecuta el primero que coincida.

```csharp
try
{
    Console.Write("Ingresá un número: ");
    int numero = int.Parse(Console.ReadLine()); // puede lanzar FormatException
    int resultado = 100 / numero;               // puede lanzar DivideByZeroException
    Console.WriteLine($"100 / {numero} = {resultado}");
}
catch (FormatException)
{
    Console.WriteLine("Error: el valor ingresado no es un número válido.");
}
catch (DivideByZeroException)
{
    Console.WriteLine("Error: no se puede dividir por cero.");
}
catch (Exception ex)
{
    // Captura cualquier otra excepción no prevista
    Console.WriteLine($"Error inesperado: {ex.Message}");
}
finally
{
    Console.WriteLine("Operación finalizada.");
}
```

> El bloque `catch (Exception ex)` es el más general. Siempre colocalo **al final** si lo usás, porque si va primero captura todas las excepciones y los bloques siguientes nunca se ejecutan.

### Excepciones comunes en C#

| Excepción | Cuándo ocurre |
|-----------|---------------|
| `ArgumentException` | Un argumento pasado a un método no es válido |
| `ArgumentNullException` | Se pasa `null` donde no se esperaba |
| `InvalidOperationException` | La operación no es válida en el estado actual del objeto |
| `NullReferenceException` | Se intenta acceder a un miembro de una variable `null` |
| `IndexOutOfRangeException` | Se accede a un índice fuera del rango de un array |
| `FormatException` | Un string no tiene el formato esperado (ej: `int.Parse("abc")`) |
| `DivideByZeroException` | División entera por cero |
| `FileNotFoundException` | El archivo especificado no existe |
| `OverflowException` | Un valor aritmético supera el rango del tipo |

### Lanzar excepciones con `throw`

Los métodos pueden lanzar excepciones cuando reciben datos inválidos. Esto es la forma correcta de comunicar errores en la capa de lógica.

```csharp
public class Estudiante
{
    private string nombre;
    private int edad;

    public string Nombre
    {
        get => nombre;
        set
        {
            if (string.IsNullOrWhiteSpace(value))
                throw new ArgumentException("El nombre no puede estar vacío.");
            nombre = value;
        }
    }

    public int Edad
    {
        get => edad;
        set
        {
            if (value < 0 || value > 120)
                throw new ArgumentOutOfRangeException(nameof(value), "La edad debe estar entre 0 y 120.");
            edad = value;
        }
    }

    public Estudiante(string nombre, int edad)
    {
        Nombre = nombre; // pasa por el setter con validación
        Edad = edad;
    }
}
```

### Excepciones personalizadas

Podés crear tus propias clases de excepción heredando de `Exception`. Esto es útil para representar errores específicos del dominio de tu aplicación.

```csharp
// Excepción personalizada
public class SaldoInsuficienteException : Exception
{
    public decimal SaldoActual { get; }
    public decimal MontoSolicitado { get; }

    public SaldoInsuficienteException(decimal saldoActual, decimal montoSolicitado)
        : base($"Saldo insuficiente. Disponible: ${saldoActual:F2}, Solicitado: ${montoSolicitado:F2}")
    {
        SaldoActual = saldoActual;
        MontoSolicitado = montoSolicitado;
    }
}

// Uso
public void Extraer(decimal monto)
{
    if (monto > Saldo)
        throw new SaldoInsuficienteException(Saldo, monto);

    Saldo -= monto;
}

// Captura
try
{
    cuenta.Extraer(5000m);
}
catch (SaldoInsuficienteException ex)
{
    Console.WriteLine(ex.Message);
    Console.WriteLine($"Diferencia: ${ex.MontoSolicitado - ex.SaldoActual:F2}");
}
```

> **Buena práctica**: usá `throw` sin argumentos dentro de un `catch` para relanzar la excepción original sin perder el stack trace. Usá `throw ex` solo si querés relanzar con un nuevo punto de origen.

---

## 8. Constantes y Enumeradores

### Constantes (`const`)

Una constante es un valor que **no puede cambiar** durante la ejecución del programa. Se define con la palabra clave `const` y debe inicializarse en la declaración.

```csharp
public class Configuracion
{
    public const double IVA = 0.21;
    public const int MAX_INTENTOS = 3;
    public const string VERSION_APP = "1.0.0";
}

// Uso
double precio = 100.0;
double precioConIva = precio * (1 + Configuracion.IVA);
Console.WriteLine($"Precio con IVA: ${precioConIva}"); // $121.00
```

### Enumeradores (`enum`)

Un `enum` define un tipo con un conjunto fijo de valores con nombre. Mejora la legibilidad del código al reemplazar números mágicos por nombres descriptivos.

```csharp
// Enum básico (los valores internos empiezan en 0 por defecto)
public enum DiaSemana
{
    Lunes,    // 0
    Martes,   // 1
    Miercoles,// 2
    Jueves,   // 3
    Viernes,  // 4
    Sabado,   // 5
    Domingo   // 6
}

// Enum con valores explícitos
public enum EstadoPedido
{
    Pendiente  = 1,
    EnProceso  = 2,
    Entregado  = 3,
    Cancelado  = 4
}

// Uso
DiaSemana hoy = DiaSemana.Miercoles;
Console.WriteLine($"Hoy es: {hoy}"); // Hoy es: Miercoles

EstadoPedido estado = EstadoPedido.EnProceso;
Console.WriteLine($"Estado del pedido: {estado} (valor: {(int)estado})"); // EnProceso (valor: 2)

// Uso en switch
switch (estado)
{
    case EstadoPedido.Pendiente:
        Console.WriteLine("El pedido está pendiente de confirmación.");
        break;
    case EstadoPedido.EnProceso:
        Console.WriteLine("El pedido está siendo preparado.");
        break;
    case EstadoPedido.Entregado:
        Console.WriteLine("El pedido fue entregado.");
        break;
    case EstadoPedido.Cancelado:
        Console.WriteLine("El pedido fue cancelado.");
        break;
}
```

---

## 9. Null y NullReferenceException

**`null`** representa la ausencia de un objeto. Cuando una variable de tipo referencia (como `string`, clases, listas) no apunta a ningún objeto, su valor es `null`.

Si intentás llamar a un método o acceder a una propiedad de una variable que es `null`, obtenés una **`NullReferenceException`** en tiempo de ejecución.

### Cómo verificar null

```csharp
// Verificación tradicional con if
string nombre = ObtenerNombre(); // puede devolver null

if (nombre != null)
{
    Console.WriteLine(nombre.ToUpper());
}
else
{
    Console.WriteLine("El nombre no está disponible.");
}

// Operador null-condicional (?.) - forma moderna y segura
// Si nombre es null, no lanza excepción, simplemente devuelve null
string nombreMayusculas = nombre?.ToUpper();
Console.WriteLine(nombreMayusculas ?? "Sin nombre"); // ?? = operador null-coalescing

// Tipos nullable: permiten que tipos de valor (int, bool, etc.) sean null
int? edad = null; // sin el ?, int no puede ser null
if (edad.HasValue)
{
    Console.WriteLine($"Edad: {edad.Value}");
}
else
{
    Console.WriteLine("Edad no especificada.");
}

// Valor por defecto si es null
int edadReal = edad ?? 0; // si edad es null, usa 0
```

> **Buena práctica**: siempre verificá si una variable puede ser `null` antes de usarla, especialmente cuando viene de entrada del usuario o de métodos externos.

---

## 10. LINQ y Expresiones Lambda

**LINQ** (Language Integrated Query) es una característica de C# que permite consultar y manipular colecciones usando una sintaxis similar a SQL, directamente en el código C#.

Las **expresiones lambda** son funciones anónimas compactas que se usan frecuentemente con LINQ. Su sintaxis es: `parametro => expresion`.

### Métodos LINQ más comunes

| Método | Descripción |
|--------|-------------|
| `Where(condicion)` | Filtra elementos que cumplen la condición |
| `Select(transformacion)` | Proyecta cada elemento en otro valor |
| `OrderBy(campo)` | Ordena ascendentemente |
| `OrderByDescending(campo)` | Ordena descendentemente |
| `First()` | Devuelve el primer elemento (lanza excepción si está vacío) |
| `FirstOrDefault()` | Devuelve el primer elemento o el valor por defecto si está vacío |
| `Count()` | Cuenta los elementos |
| `Distinct()` | Elimina duplicados |

```csharp
// Clase de ejemplo
public class Producto
{
    public string Nombre { get; set; }
    public decimal Precio { get; set; }
    public string Categoria { get; set; }
}

// Lista de productos
List<Producto> productos = new List<Producto>
{
    new Producto { Nombre = "Laptop",    Precio = 1200m, Categoria = "Electrónica" },
    new Producto { Nombre = "Mouse",     Precio = 25m,   Categoria = "Electrónica" },
    new Producto { Nombre = "Teclado",   Precio = 80m,   Categoria = "Electrónica" },
    new Producto { Nombre = "Escritorio",Precio = 350m,  Categoria = "Muebles" },
    new Producto { Nombre = "Silla",     Precio = 150m,  Categoria = "Muebles" },
    new Producto { Nombre = "Monitor",   Precio = 450m,  Categoria = "Electrónica" }
};

// Filtrar productos con precio mayor a $100
List<Producto> caros = productos.Where(p => p.Precio > 100).ToList();
Console.WriteLine($"Productos más de $100: {caros.Count}");

// Ordenar por precio ascendente
List<Producto> ordenados = productos.OrderBy(p => p.Precio).ToList();
Console.WriteLine("Ordenados por precio:");
foreach (var p in ordenados)
    Console.WriteLine($"  {p.Nombre}: ${p.Precio}");

// Seleccionar solo los nombres de productos caros, ordenados por precio descendente
List<string> nombresCaros = productos
    .Where(p => p.Precio > 100)
    .OrderByDescending(p => p.Precio)
    .Select(p => p.Nombre)
    .ToList();

Console.WriteLine("Nombres (caros, mayor a menor precio):");
foreach (string nombre in nombresCaros)
    Console.WriteLine($"  - {nombre}");

// Obtener el producto más barato
Producto masBarato = productos.OrderBy(p => p.Precio).First();
Console.WriteLine($"Más barato: {masBarato.Nombre} (${masBarato.Precio})");

// Contar productos de electrónica
int cantElectronica = productos.Count(p => p.Categoria == "Electrónica");
Console.WriteLine($"Productos de electrónica: {cantElectronica}");

// Categorías únicas
List<string> categorias = productos.Select(p => p.Categoria).Distinct().ToList();
Console.WriteLine("Categorías: " + string.Join(", ", categorias));
```

---

## 11. Programación Orientada a Objetos - Los 4 Pilares

La POO se basa en cuatro principios fundamentales:

| Pilar | Descripción |
|-------|-------------|
| **Encapsulamiento** | Ocultar los datos internos de un objeto y exponer solo lo necesario mediante propiedades y métodos públicos |
| **Herencia** | Una clase puede heredar propiedades y métodos de otra clase, reutilizando código |
| **Polimorfismo** | Un mismo método puede comportarse de manera diferente según el tipo del objeto que lo ejecuta |
| **Abstracción** | Modelar entidades del mundo real enfocándose en las características relevantes, ignorando los detalles innecesarios |

```csharp
// Concepto general - los pilares se ven en detalle en las secciones siguientes

// Encapsulamiento: la clase Persona expone Nombre y Edad
// pero protege la lógica interna (edad no puede ser negativa)

// Herencia: Empleado : Persona → Empleado hereda de Persona

// Polimorfismo: Animal tiene HacerSonido(), Perro lo redefine como "Guau"
// y Gato lo redefine como "Miau"

// Abstracción: Figura define el contrato CalcularArea()
// sin importar si es un Círculo, Triángulo o Rectángulo
```

---

## 12. Herencia

La **herencia** permite crear una clase nueva (clase derivada o hija) basada en una clase existente (clase base o padre). La clase hija hereda todas las propiedades y métodos públicos/protegidos de la clase padre y puede agregar los suyos propios.

Sintaxis: `public class ClaseHija : ClaseBase`

La palabra clave `base` permite acceder a miembros de la clase padre desde la clase hija.

```csharp
// Clase base
public class Animal
{
    public string Nombre { get; set; }

    public Animal(string nombre)
    {
        Nombre = nombre;
    }

    public void Comer()
    {
        Console.WriteLine($"{Nombre} está comiendo.");
    }

    public void Dormir()
    {
        Console.WriteLine($"{Nombre} está durmiendo.");
    }
}

// Clase derivada Perro
public class Perro : Animal
{
    public string Raza { get; set; }

    public Perro(string nombre, string raza) : base(nombre) // llama al constructor del padre
    {
        Raza = raza;
    }

    public void HacerSonido()
    {
        Console.WriteLine($"{Nombre} dice: ¡Guau!");
    }

    public void Jugar()
    {
        Console.WriteLine($"{Nombre} está jugando.");
    }
}

// Clase derivada Gato
public class Gato : Animal
{
    public bool EsDomestico { get; set; }

    public Gato(string nombre, bool esDomestico) : base(nombre)
    {
        EsDomestico = esDomestico;
    }

    public void HacerSonido()
    {
        Console.WriteLine($"{Nombre} dice: ¡Miau!");
    }
}

// Uso
Perro perro = new Perro("Rex", "Labrador");
perro.Comer();        // heredado de Animal: Rex está comiendo.
perro.HacerSonido();  // propio de Perro: Rex dice: ¡Guau!
perro.Jugar();        // propio de Perro: Rex está jugando.

Gato gato = new Gato("Michi", true);
gato.Comer();         // heredado de Animal: Michi está comiendo.
gato.HacerSonido();   // propio de Gato: Michi dice: ¡Miau!
```

### Composición vs Herencia

La herencia modela una relación **"es un"** (`Perro` *es un* `Animal`). Pero no toda relación entre clases es de ese tipo. Cuando una clase **"tiene un"** objeto de otra clase, se usa **composición**: una clase contiene una instancia de otra como propiedad.

```csharp
// HERENCIA ("es un"): Auto es un Vehículo → tiene sentido
public class Vehiculo
{
    public int Velocidad { get; set; }
    public void Acelerar() => Velocidad += 10;
}

public class Auto : Vehiculo
{
    public string Marca { get; set; }
}

// COMPOSICIÓN ("tiene un"): Auto tiene un Motor → mejor que heredar de Motor
public class Motor
{
    public int Cilindros { get; set; }
    public void Encender() => Console.WriteLine("Motor encendido.");
}

public class AutoConComposicion
{
    public string Marca { get; set; }
    public Motor Motor { get; set; } // composición: Auto "tiene un" Motor

    public AutoConComposicion(string marca, int cilindros)
    {
        Marca = marca;
        Motor = new Motor { Cilindros = cilindros };
    }
}

AutoConComposicion auto = new AutoConComposicion("Ford", 4);
auto.Motor.Encender(); // Motor encendido.
```

**¿Cuándo usar cada una?**

| Situación | Usá |
|-----------|-----|
| La relación es claramente "es un" (Perro es un Animal) | Herencia |
| La relación es "tiene un" (Auto tiene un Motor) | Composición |
| Necesitás reutilizar comportamiento de múltiples fuentes | Composición |
| La jerarquía tiene más de 2 niveles de profundidad | Revisar si es composición |

> **Regla práctica**: ante la duda, preferí composición. La herencia crea un acoplamiento fuerte entre clases que puede volverse difícil de modificar con el tiempo.

---

## 13. Interfaces

Una **interfaz** define un **contrato**: un conjunto de métodos y propiedades que una clase se compromete a implementar. A diferencia de las clases abstractas, la interfaz no puede tener implementación propia (salvo implementaciones por defecto en C# 8+), ni estado interno.

En C# las interfaces se declaran con la palabra clave `interface` y, por convención, su nombre empieza con `I` mayúscula: `IAnimal`, `IRepositorio`, `IServicio`.

### Declarar e implementar una interfaz

```csharp
// Definición de la interfaz
public interface IAnimal
{
    string Nombre { get; set; }
    void HacerSonido();
    void Moverse();
}

// Clase que implementa la interfaz (con ":")
public class Perro : IAnimal
{
    public string Nombre { get; set; }

    public Perro(string nombre)
    {
        Nombre = nombre;
    }

    // Obligatorio implementar todos los miembros de la interfaz
    public void HacerSonido()
    {
        Console.WriteLine($"{Nombre} dice: ¡Guau!");
    }

    public void Moverse()
    {
        Console.WriteLine($"{Nombre} corre.");
    }
}

public class Pajaro : IAnimal
{
    public string Nombre { get; set; }

    public Pajaro(string nombre)
    {
        Nombre = nombre;
    }

    public void HacerSonido()
    {
        Console.WriteLine($"{Nombre} dice: ¡Pío!");
    }

    public void Moverse()
    {
        Console.WriteLine($"{Nombre} vuela.");
    }
}
```

### Programar contra la interfaz

La ventaja clave de las interfaces es que permiten tratar objetos de distintas clases de forma uniforme, sin importar su tipo concreto.

```csharp
// Podemos usar IAnimal como tipo, aunque los objetos sean Perro o Pajaro
List<IAnimal> animales = new List<IAnimal>
{
    new Perro("Rex"),
    new Pajaro("Piolín"),
    new Perro("Firulais")
};

foreach (IAnimal animal in animales)
{
    animal.HacerSonido(); // cada uno ejecuta SU implementación
    animal.Moverse();
}
// Rex dice: ¡Guau!    Rex corre.
// Piolín dice: ¡Pío!  Piolín vuela.
// Firulais dice: ¡Guau! Firulais corre.
```

### Una clase puede implementar múltiples interfaces

A diferencia de la herencia (que en C# solo permite una clase base), una clase puede implementar **varias interfaces** a la vez.

```csharp
public interface IGuardable
{
    void Guardar();
}

public interface IImprimible
{
    void Imprimir();
}

// Implementa dos interfaces y también hereda de una clase base
public class Factura : Documento, IGuardable, IImprimible
{
    public decimal Total { get; set; }

    public void Guardar()
    {
        Console.WriteLine("Factura guardada en disco.");
    }

    public void Imprimir()
    {
        Console.WriteLine($"Imprimiendo factura. Total: ${Total:F2}");
    }
}
```

### Interfaz vs Clase Abstracta

| Característica | Interfaz | Clase Abstracta |
|----------------|----------|-----------------|
| Puede tener implementación | No (salvo default en C# 8+) | Sí |
| Puede tener estado (campos) | No | Sí |
| Una clase puede heredar de... | Múltiples interfaces | Solo una clase base |
| Se usa cuando... | Definís un contrato de comportamiento | Compartís lógica común entre clases hijas |

> **Regla práctica**: usá una **interfaz** cuando querés definir *qué puede hacer* un objeto (comportamiento). Usá una **clase abstracta** cuando querés compartir *código común* entre varias clases relacionadas.

---

## 14. Polimorfismo

El **polimorfismo** permite que objetos de distintas clases sean tratados como objetos de la clase base, y que cada uno responda a un mismo mensaje de forma diferente.

Para lograrlo en C# se usan las palabras clave:
- `virtual`: en la clase base, indica que el método **puede** ser redefinido por las clases hijas.
- `override`: en la clase hija, indica que el método **reemplaza** la implementación del padre.

```csharp
public class Animal
{
    public string Nombre { get; set; }

    public Animal(string nombre)
    {
        Nombre = nombre;
    }

    // Método virtual: tiene implementación por defecto pero puede ser sobreescrito
    public virtual void HacerSonido()
    {
        Console.WriteLine($"{Nombre} hace un sonido genérico.");
    }

    public void Comer()
    {
        Console.WriteLine($"{Nombre} está comiendo.");
    }
}

public class Perro : Animal
{
    public Perro(string nombre) : base(nombre) { }

    // override reemplaza la implementación del padre
    public override void HacerSonido()
    {
        Console.WriteLine($"{Nombre} dice: ¡Guau!");
    }
}

public class Gato : Animal
{
    public Gato(string nombre) : base(nombre) { }

    public override void HacerSonido()
    {
        Console.WriteLine($"{Nombre} dice: ¡Miau!");
    }
}

public class Vaca : Animal
{
    public Vaca(string nombre) : base(nombre) { }

    public override void HacerSonido()
    {
        Console.WriteLine($"{Nombre} dice: ¡Muuu!");
    }
}

// Polimorfismo en acción: todos son Animal, pero cada uno responde diferente
List<Animal> animales = new List<Animal>
{
    new Perro("Rex"),
    new Gato("Michi"),
    new Vaca("Lola"),
    new Animal("Criatura") // usa la implementación por defecto
};

foreach (Animal animal in animales)
{
    animal.HacerSonido(); // cada uno ejecuta SU versión del método
}
// Rex dice: ¡Guau!
// Michi dice: ¡Miau!
// Lola dice: ¡Muuu!
// Criatura hace un sonido genérico.
```

---

## 15. Clases Abstractas

Una **clase abstracta** es una clase que **no puede instanciarse directamente** (no se puede hacer `new ClaseAbstracta()`). Su propósito es servir de base para otras clases, definiendo un contrato que las clases hijas **deben** implementar.

- Los **métodos abstractos** no tienen implementación en la clase base: las clases hijas están obligadas a implementarlos con `override`.
- Una clase abstracta puede tener métodos concretos (con implementación) además de abstractos.

```csharp
// Clase abstracta: no se puede instanciar directamente
public abstract class Figura
{
    public string Color { get; set; }

    public Figura(string color)
    {
        Color = color;
    }

    // Método abstracto: sin implementación, las clases hijas DEBEN implementarlo
    public abstract double CalcularArea();

    // Método abstracto
    public abstract double CalcularPerimetro();

    // Método concreto: tiene implementación, las hijas lo heredan
    public void MostrarInfo()
    {
        Console.WriteLine($"Figura: {GetType().Name}");
        Console.WriteLine($"Color: {Color}");
        Console.WriteLine($"Área: {CalcularArea():F2}");
        Console.WriteLine($"Perímetro: {CalcularPerimetro():F2}");
    }
}

public class Circulo : Figura
{
    public double Radio { get; set; }

    public Circulo(string color, double radio) : base(color)
    {
        Radio = radio;
    }

    public override double CalcularArea()
    {
        return Math.PI * Radio * Radio;
    }

    public override double CalcularPerimetro()
    {
        return 2 * Math.PI * Radio;
    }
}

public class Rectangulo : Figura
{
    public double Ancho { get; set; }
    public double Alto { get; set; }

    public Rectangulo(string color, double ancho, double alto) : base(color)
    {
        Ancho = ancho;
        Alto = alto;
    }

    public override double CalcularArea()
    {
        return Ancho * Alto;
    }

    public override double CalcularPerimetro()
    {
        return 2 * (Ancho + Alto);
    }
}

// Uso
// Figura figura = new Figura("rojo"); // ERROR: no se puede instanciar una clase abstracta

Circulo circulo = new Circulo("rojo", 5.0);
circulo.MostrarInfo();
// Figura: Circulo | Color: rojo | Área: 78.54 | Perímetro: 31.42

Rectangulo rect = new Rectangulo("azul", 4.0, 6.0);
rect.MostrarInfo();
// Figura: Rectangulo | Color: azul | Área: 24.00 | Perímetro: 20.00

// Polimorfismo con clase abstracta
List<Figura> figuras = new List<Figura> { circulo, rect };
foreach (Figura f in figuras)
{
    Console.WriteLine($"{f.GetType().Name} - Área: {f.CalcularArea():F2}");
}
```

---

## 16. Operadores is y as

### Operador `is`

Verifica si un objeto es de un tipo específico. Devuelve `bool`. No lanza excepción si el objeto es `null`.

### Operador `as`

Intenta convertir un objeto a un tipo específico. Si la conversión no es posible, devuelve `null` (no lanza excepción). Solo funciona con tipos de referencia.

```csharp
// Usando la jerarquía Animal/Perro/Gato del ejemplo anterior

List<Animal> animales = new List<Animal>
{
    new Perro("Rex"),
    new Gato("Michi"),
    new Animal("Criatura"),
    new Perro("Firulais")
};

foreach (Animal animal in animales)
{
    // is: verificar el tipo
    if (animal is Perro)
    {
        Console.WriteLine($"{animal.Nombre} es un Perro");
    }
    else if (animal is Gato)
    {
        Console.WriteLine($"{animal.Nombre} es un Gato");
    }
    else
    {
        Console.WriteLine($"{animal.Nombre} es un Animal genérico");
    }
}

Console.WriteLine("---");

// as: castear de forma segura para acceder a métodos específicos
foreach (Animal animal in animales)
{
    // Intentar convertir a Perro; si no es Perro, perro = null
    Perro perro = animal as Perro;
    if (perro != null)
    {
        Console.WriteLine($"{perro.Nombre} tiene raza: {perro.Raza ?? "sin especificar"}");
    }
}

// Forma moderna: is con declaración de variable (C# 7+)
foreach (Animal animal in animales)
{
    if (animal is Perro p)
    {
        Console.WriteLine($"Perro encontrado: {p.Nombre}");
    }
}
```

---

## 17. Arquitectura por Capas

La **arquitectura por capas** es un patrón de diseño que separa las responsabilidades de una aplicación en capas distintas. En las aplicaciones de consola de Programación 1 trabajamos principalmente con dos capas:

| Capa | Responsabilidad |
|------|-----------------|
| **Capa de Presentación** | Interactúa con el usuario: muestra información y captura entradas (Console.WriteLine, Console.ReadLine) |
| **Capa de Lógica de Negocio** | Contiene las reglas del negocio, procesa datos, no sabe nada de la interfaz |

### ¿Por qué separar?

- **Mantenibilidad**: si cambia la interfaz de usuario, no tocás la lógica.
- **Testabilidad**: podés probar la lógica sin necesitar la consola.
- **Reutilización**: la misma lógica puede usarse desde una app de consola, web o móvil.

```csharp
// --- Capa de Lógica de Negocio ---

public class ProductoService
{
    private List<Producto> productos = new List<Producto>();

    public void Agregar(Producto producto)
    {
        if (producto == null)
            throw new ArgumentNullException(nameof(producto));

        productos.Add(producto);
    }

    public List<Producto> ObtenerTodos()
    {
        return productos;
    }

    public List<Producto> BuscarPorCategoria(string categoria)
    {
        return productos.Where(p => p.Categoria == categoria).ToList();
    }

    public decimal ObtenerPrecioPromedio()
    {
        if (!productos.Any()) return 0;
        return productos.Average(p => p.Precio);
    }
}

// --- Capa de Presentación (Program.cs) ---

// La presentación usa el servicio pero no conoce sus detalles internos
ProductoService servicio = new ProductoService();

servicio.Agregar(new Producto { Nombre = "Laptop", Precio = 1200m, Categoria = "Electrónica" });
servicio.Agregar(new Producto { Nombre = "Silla",  Precio = 150m,  Categoria = "Muebles" });

// Mostrar todos los productos
Console.WriteLine("=== Todos los productos ===");
foreach (var p in servicio.ObtenerTodos())
{
    Console.WriteLine($"  {p.Nombre} - ${p.Precio}");
}

// Mostrar precio promedio
Console.WriteLine($"\nPrecio promedio: ${servicio.ObtenerPrecioPromedio():F2}");
```

---

## 18. Estándares de Programación

Seguir convenciones de nomenclatura hace que el código sea más legible y mantenible. En C# se usan los siguientes estándares:

### Convenciones de nombres

| Elemento | Convención | Ejemplo |
|----------|-----------|---------|
| Clases | PascalCase | `ProductoService`, `CuentaBancaria` |
| Métodos | PascalCase | `CalcularTotal()`, `ObtenerPorId()` |
| Propiedades | PascalCase | `NombreCompleto`, `FechaCreacion` |
| Parámetros | camelCase | `nombreCompleto`, `fechaCreacion` |
| Variables locales | camelCase | `totalVentas`, `listaProductos` |
| Constantes | PascalCase o UPPER_CASE | `MaxIntentos` o `MAX_INTENTOS` |
| Interfaces | Prefijo `I` + PascalCase | `IProductoService`, `IRepositorio` |
| Enums | PascalCase | `EstadoPedido`, `DiaSemana` |

### Otras buenas prácticas

- **Un archivo por clase**: el archivo se llama igual que la clase. `Persona.cs` contiene la clase `Persona`.
- **Las listas no necesitan el prefijo "Lista"**: usar `productos` en lugar de `listaProductos`.
- **Nombres descriptivos**: preferí `cantidadAlumnos` sobre `n` o `x`.
- **Métodos cortos**: un método debe hacer una sola cosa.
- **Comentarios útiles**: comentar el "por qué", no el "qué" (el código ya dice qué hace).

```csharp
// MAL: nombres poco descriptivos, sin separación de responsabilidades
public class c1
{
    public int x;
    public void m(int a, int b) { x = a + b; }
}

// BIEN: nombres claros, convenciones aplicadas
public class Calculadora
{
    public int Resultado { get; private set; }

    public void Sumar(int primerNumero, int segundoNumero)
    {
        Resultado = primerNumero + segundoNumero;
    }
}
```

---

## Recursos adicionales

Los ejercicios prácticos de esta unidad se encuentran en la carpeta de ejercicios de la Unidad 1 [`../001-introduccion-poo-netcore-csharp/001-ejercicios-net/`](../001-introduccion-poo-netcore-csharp/001-ejercicios-net/).

---

> Unidad 2 - Programación 1 | UCSE
