# Unidad 1 - Introducción a .NET Core, C# y Programación Orientada a Objetos

Esta unidad cubre los fundamentos del lenguaje C# y la plataforma .NET Core, desde la sintaxis básica hasta los pilares de la Programación Orientada a Objetos (POO).

---

## Tabla de contenidos

1. [¿Qué es .NET Core?](#1-qué-es-net-core)
2. [Instalación de .NET](#2-instalación-de-net)
3. [Crear y ejecutar un proyecto](#3-crear-y-ejecutar-un-proyecto)
4. [Características de C#](#4-características-de-c)
5. [Tipos de datos](#5-tipos-de-datos)
6. [Entrada y Salida por Consola](#6-entrada-y-salida-por-consola)
7. [Operadores y Expresiones](#7-operadores-y-expresiones)
8. [Estructuras de Control - Condicionales](#8-estructuras-de-control---condicionales)
9. [Estructuras de Control - Bucles](#9-estructuras-de-control---bucles)

---

## 1. ¿Qué es .NET Core?

**.NET Core** (actualmente llamado simplemente **.NET**) es una plataforma de desarrollo **gratuita, de código abierto y multiplataforma** creada por Microsoft. Permite crear aplicaciones que corren en Windows, Linux y macOS usando el mismo código.

### Diferencias con .NET Framework

| Característica | .NET Framework | .NET Core / .NET |
|---|---|---|
| Plataformas | Solo Windows | Windows, Linux, macOS |
| Código abierto | No | Sí |
| Rendimiento | Moderado | Alto |
| Estado | Legacy (mantenimiento) | Activo (versiones anuales) |

### Compilación JIT (Just-In-Time)

El código C# no se compila directamente a lenguaje de máquina. El proceso es:

1. El código fuente `.cs` se compila a **IL (Intermediate Language)**, un lenguaje intermedio.
2. En el momento de ejecución, el **JIT Compiler** convierte el IL a código de máquina nativo optimizado para el procesador donde corre la aplicación.

Esto permite que el mismo IL corra en distintas plataformas sin recompilar el código fuente.

```csharp
// El programa más simple en C#
Console.WriteLine("Hola, mundo!");
```

---

## 2. Instalación de .NET

### Descargar el SDK

Para desarrollar con .NET necesitás instalar el **SDK** (Software Development Kit), que incluye el compilador, el runtime y las herramientas de línea de comandos (`dotnet`).

1. Ir a [https://dotnet.microsoft.com/download](https://dotnet.microsoft.com/download)
2. Descargar el instalador del **SDK** para tu sistema operativo (Windows, Linux o macOS).
3. Ejecutar el instalador y seguir los pasos.

> Instalá siempre el **SDK**, no solo el Runtime. El Runtime solo permite *ejecutar* aplicaciones ya compiladas; el SDK permite además *desarrollarlas*.

### Verificar la instalación

Abrí una terminal (PowerShell, CMD o Bash) y ejecutá:

```bash
dotnet --version
```

Si la instalación fue exitosa, verás el número de versión instalada, por ejemplo: `8.0.100`.

Para ver todas las versiones del SDK instaladas:

```bash
dotnet --list-sdks
```

### Herramientas recomendadas

| Herramienta | Descripción |
|-------------|-------------|
| **Visual Studio 2022** | IDE completo de Microsoft, incluye el SDK de .NET. Recomendado para Windows. |
| **Visual Studio Code** | Editor liviano multiplataforma. Requiere instalar la extensión **C# Dev Kit**. |
| **JetBrains Rider** | IDE multiplataforma alternativo (de pago, con licencia gratuita para estudiantes). |

---

## 3. Crear y ejecutar un proyecto

### Crear un proyecto nuevo desde la terminal

El comando `dotnet new` genera la estructura base de un proyecto. Los tipos de proyecto más comunes son:

| Comando | Tipo de proyecto |
|---------|-----------------|
| `dotnet new console` | Aplicación de consola |
| `dotnet new classlib` | Biblioteca de clases (sin punto de entrada) |
| `dotnet new xunit` | Proyecto de tests con xUnit |
| `dotnet new webapi` | API web con ASP.NET Core |

Ejemplo: crear una aplicación de consola llamada `MiApp`:

```bash
dotnet new console -n MiApp
cd MiApp
```

Esto genera la siguiente estructura:

```
MiApp/
├── MiApp.csproj   ← archivo de proyecto (dependencias, versión de .NET)
└── Program.cs     ← punto de entrada de la aplicación
```

### Estructura del archivo `.csproj`

```xml
<Project Sdk="Microsoft.NET.Sdk">

  <PropertyGroup>
    <OutputType>Exe</OutputType>       <!-- Exe = ejecutable, Library = librería -->
    <TargetFramework>net8.0</TargetFramework>
    <Nullable>enable</Nullable>
    <ImplicitUsings>enable</ImplicitUsings>
  </PropertyGroup>

</Project>
```

### Ejecutar el proyecto

Desde la carpeta del proyecto:

```bash
dotnet run
```

Esto compila y ejecuta la aplicación en un solo paso. La salida por defecto de un proyecto de consola nuevo es:

```
Hello, World!
```

### Compilar sin ejecutar

```bash
dotnet build
```

Genera los archivos compilados en `bin/Debug/net8.0/`. Útil para verificar que no hay errores antes de ejecutar.

### Crear una solución con múltiples proyectos

En proyectos más grandes (como cuando se separa la lógica del proyecto de tests), se usa una **solución** (`.sln`) que agrupa varios proyectos:

```bash
# Crear la solución
dotnet new sln -n MiSolucion

# Crear los proyectos
dotnet new classlib -n MiApp.Dominio
dotnet new xunit -n MiApp.Tests

# Agregar los proyectos a la solución
dotnet sln add MiApp.Dominio/MiApp.Dominio.csproj
dotnet sln add MiApp.Tests/MiApp.Tests.csproj

# Referenciar el proyecto de dominio desde el proyecto de tests
dotnet add MiApp.Tests/MiApp.Tests.csproj reference MiApp.Dominio/MiApp.Dominio.csproj
```

Estructura resultante:

```
MiSolucion/
├── MiSolucion.sln
├── MiApp.Dominio/
│   ├── MiApp.Dominio.csproj
│   └── Class1.cs
└── MiApp.Tests/
    ├── MiApp.Tests.csproj
    └── UnitTest1.cs
```

### Ejecutar los tests

```bash
dotnet test
```

---

## 4. Características de C#

C# es un lenguaje:

- **Fuertemente tipado**: cada variable tiene un tipo definido y no puede cambiar. El compilador detecta errores de tipo antes de ejecutar el programa.
- **Orientado a objetos**: todo se organiza en clases y objetos.
- **Compilado**: el código se convierte a IL antes de ejecutarse, lo que mejora el rendimiento respecto a lenguajes interpretados.
- **Moderno**: soporta genéricos, LINQ, async/await, expresiones lambda, y más.

```csharp
// Declaración de variables con distintos tipos
int edad = 25;
double precio = 19.99;
bool estaActivo = true;
string nombre = "Ana";
char inicial = 'A';

// C# no permite usar una variable sin inicializarla
// int numero; // esto compilaría pero daría advertencia si se usa sin asignar
```

---

## 5. Tipos de datos

### Tipos numéricos

| Tipo | Descripción | Rango aproximado | Ejemplo |
|------|-------------|-----------------|---------|
| `int` | Entero de 32 bits | -2.1 mil millones a 2.1 mil millones | `int x = 10;` |
| `long` | Entero de 64 bits | Muy grande | `long x = 10000000000L;` |
| `float` | Decimal de 32 bits (poca precisión) | 7 dígitos decimales | `float x = 3.14f;` |
| `double` | Decimal de 64 bits (uso general) | 15-16 dígitos decimales | `double x = 3.14;` |
| `decimal` | Decimal de alta precisión (dinero) | 28-29 dígitos decimales | `decimal x = 9.99m;` |

### Otros tipos básicos

- `bool`: valores `true` o `false`
- `char`: un solo carácter Unicode, entre comillas simples: `'A'`
- `string`: cadena de texto, entre comillas dobles: `"Hola"`

### Conversiones

```csharp
// Conversión implícita (el compilador la hace solo, no hay pérdida de datos)
int entero = 10;
double doble = entero; // int -> double es seguro, no se pierde información

// Conversión explícita con cast (puede haber pérdida de datos)
double precio = 9.99;
int precioEntero = (int)precio; // resultado: 9 (se trunca el decimal)

// Conversión desde string usando Convert
string textoNumero = "42";
int numero1 = Convert.ToInt32(textoNumero); // devuelve 0 si el string es null
int numero2 = int.Parse(textoNumero);       // lanza excepción si el string es null o inválido

// int.TryParse: forma segura de convertir (no lanza excepción)
bool exito = int.TryParse("abc", out int resultado);
// exito = false, resultado = 0
```

> **Diferencia clave**: `Convert.ToInt32(null)` devuelve `0`, mientras que `int.Parse(null)` lanza una excepción. Para entradas del usuario, preferí `int.TryParse`.

---

## 6. Entrada y Salida por Consola

| Método | Descripción |
|--------|-------------|
| `Console.WriteLine(texto)` | Imprime texto y salta a la siguiente línea |
| `Console.Write(texto)` | Imprime texto sin saltar de línea |
| `Console.ReadLine()` | Lee una línea de texto ingresada por el usuario (devuelve `string`) |
| `Console.ReadKey()` | Espera que el usuario presione una tecla |

```csharp
// Programa que lee nombre y edad, luego imprime un saludo
Console.Write("Ingresá tu nombre: ");
string nombre = Console.ReadLine();

Console.Write("Ingresá tu edad: ");
int edad = int.Parse(Console.ReadLine());

Console.WriteLine($"Hola, {nombre}! Tenés {edad} años.");
Console.WriteLine($"En 10 años tendrás {edad + 10} años.");

Console.ReadKey(); // espera que el usuario presione una tecla antes de cerrar
```

> Los strings precedidos por `$` son **strings interpolados**: permiten insertar expresiones C# dentro de `{}` directamente en el texto.

---

## 7. Operadores y Expresiones

### Operadores aritméticos

| Operador | Descripción | Ejemplo |
|----------|-------------|---------|
| `+` | Suma | `5 + 3 = 8` |
| `-` | Resta | `5 - 3 = 2` |
| `*` | Multiplicación | `5 * 3 = 15` |
| `/` | División | `10 / 3 = 3` (entero) |
| `%` | Módulo (resto) | `10 % 3 = 1` |
| `++` | Incremento | `x++` equivale a `x = x + 1` |
| `--` | Decremento | `x--` equivale a `x = x - 1` |

### Operadores de comparación

`==`, `!=`, `>`, `<`, `>=`, `<=` → devuelven `bool`

### Operadores lógicos

| Operador | Descripción | Ejemplo |
|----------|-------------|---------|
| `&&` | AND (y) | `edad > 18 && tieneDNI` |
| `\|\|` | OR (o) | `esAdmin \|\| esModerador` |
| `!` | NOT (negación) | `!estaActivo` |

```csharp
// Calculadora simple
int a = 10;
int b = 3;

Console.WriteLine($"Suma:         {a} + {b} = {a + b}");
Console.WriteLine($"Resta:        {a} - {b} = {a - b}");
Console.WriteLine($"Multiplicación: {a} * {b} = {a * b}");
Console.WriteLine($"División entera: {a} / {b} = {a / b}");
Console.WriteLine($"Módulo (resto): {a} % {b} = {a % b}");

// División con decimales: al menos uno debe ser double
double resultadoDecimal = (double)a / b;
Console.WriteLine($"División decimal: {a} / {b} = {resultadoDecimal:F2}"); // F2 = 2 decimales
```

---

## 8. Estructuras de Control - Condicionales

### if / else if / else

Permiten ejecutar distintos bloques de código según una condición.

```csharp
int nota = 75;
string resultado;

if (nota >= 90)
{
    resultado = "Sobresaliente";
}
else if (nota >= 70)
{
    resultado = "Aprobado";
}
else if (nota >= 60)
{
    resultado = "Regular";
}
else
{
    resultado = "Desaprobado";
}

Console.WriteLine($"Nota: {nota} → {resultado}");
```

### switch

Útil cuando una variable puede tomar varios valores discretos.

```csharp
int nota = 85;
string calificacion;

switch (nota / 10) // divide para obtener la decena
{
    case 10:
    case 9:
        calificacion = "Sobresaliente";
        break;
    case 8:
    case 7:
        calificacion = "Aprobado";
        break;
    case 6:
        calificacion = "Regular";
        break;
    default:
        calificacion = "Desaprobado";
        break;
}

Console.WriteLine($"Nota {nota}: {calificacion}");
```

> Siempre incluí `break` al final de cada `case` para evitar que la ejecución "caiga" al siguiente caso.

---

## 9. Estructuras de Control - Bucles

### for

Se usa cuando se conoce de antemano cuántas veces se va a repetir.

```csharp
// Imprimir números pares del 1 al 10
for (int i = 1; i <= 10; i++)
{
    if (i % 2 == 0)
    {
        Console.WriteLine(i);
    }
}
// Imprime: 2, 4, 6, 8, 10
```

### while

Se repite mientras una condición sea verdadera. La condición se evalúa **antes** de ejecutar el cuerpo.

```csharp
int contador = 1;
while (contador <= 5)
{
    Console.WriteLine($"Iteración {contador}");
    contador++;
}
```

### do-while

Similar a `while`, pero la condición se evalúa **después** de ejecutar el cuerpo: garantiza al menos una ejecución.

```csharp
int numero;
do
{
    Console.Write("Ingresá un número positivo: ");
    numero = int.Parse(Console.ReadLine());
} while (numero <= 0); // se repite si el número no es positivo

Console.WriteLine($"Ingresaste: {numero}");
```

### foreach

Ideal para recorrer colecciones (arrays, listas, etc.) sin necesitar un índice.

```csharp
string[] frutas = { "manzana", "banana", "naranja", "uva" };

foreach (string fruta in frutas)
{
    Console.WriteLine(fruta);
}
```

---

## Recursos adicionales

Los ejercicios prácticos de esta unidad se encuentran en la carpeta [`001-ejercicios-net/`](./001-ejercicios-net/).

Allí encontrarás ejercicios ordenados por dificultad que te permiten practicar todos los conceptos vistos en esta unidad: tipos de datos, estructuras de control y fundamentos del lenguaje.

La continuación del curso (clases, objetos, POO, herencia, interfaces y más) está en la [Unidad 2 - Programación Orientada a Objetos](../002-poo-csharp/README.md).

---

> Unidad 1 - Programación 1 | UCSE

