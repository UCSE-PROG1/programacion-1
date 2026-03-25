# Unidad 5: Archivos y Serialización JSON

En esta unidad aprendemos a **persistir datos** en disco usando archivos de texto con formato JSON. Esto permite que la información sobreviva entre ejecuciones de nuestra aplicación.

---

## 1. ¿Por qué Persistir Datos?

Cuando una aplicación se ejecuta, todos los datos que crea y manipula viven en la **memoria RAM**. En el momento en que el programa se cierra, esa información se pierde para siempre.

**El problema:**
```
Ejecuto la app → cargo 10 productos → cierro la app → los 10 productos desaparecen
```

**La solución:** guardar los datos en un archivo en el disco rígido antes de cerrar, y leerlos al volver a iniciar.

**Casos de uso comunes:**
- Preferencias del usuario (nombre, configuración, idioma)
- Datos de la aplicación (lista de productos, clientes, ventas)
- Estado del juego (puntajes, progreso)
- Caché de resultados para no repetir cálculos costosos

---

## 2. ¿Qué es JSON?

**JSON** (JavaScript Object Notation) es un formato de texto liviano para representar datos estructurados. Aunque viene del mundo JavaScript, es independiente del lenguaje y se usa en prácticamente toda la industria del software.

**Características:**
- Es texto plano, legible por humanos
- Es fácil de parsear por las máquinas
- Es más liviano que XML
- Soporta los siguientes tipos de datos:
  - `number` → `30`, `3.14`
  - `string` → `"Juan"`
  - `boolean` → `true`, `false`
  - `null` → `null`
  - `array` → `[1, 2, 3]`
  - `object` → `{ "clave": "valor" }`

**Ejemplo: un objeto JSON que representa una Persona:**
```json
{
  "Nombre": "Juan",
  "Edad": 30,
  "Email": "juan@email.com"
}
```

**Ejemplo: un array JSON de Personas:**
```json
[
  { "Nombre": "Ana", "Edad": 25 },
  { "Nombre": "Luis", "Edad": 32 }
]
```

---

## 3. Serialización y Deserialización

Estos dos conceptos son el corazón de esta unidad:

| Concepto | Descripción | Dirección |
|---|---|---|
| **Serialización** | Convertir un objeto C# en texto JSON | Objeto → JSON |
| **Deserialización** | Convertir texto JSON en un objeto C# | JSON → Objeto |

**Diagrama del flujo completo:**
```
Objeto C#  ──[Serializar]──>  String JSON  ──[Escribir]──>  Archivo en disco
Objeto C#  <──[Deserializar]──  String JSON  <──[Leer]──  Archivo en disco
```

En la práctica:
1. Tenés un objeto o lista en memoria
2. Lo **serializás** a un string JSON
3. **Escribís** ese string a un archivo `.json`
4. Más tarde, **leés** el archivo y obtenés el string
5. Lo **deserializás** para recuperar el objeto o lista

---

## 4. Configuración del Proyecto

Para trabajar con JSON en C# usamos la biblioteca **Newtonsoft.Json**, también conocida como **Json.NET**. Es la librería de serialización JSON más popular del ecosistema .NET.

**Instalar vía terminal:**
```bash
dotnet add package Newtonsoft.Json
```

**O agregarlo manualmente al archivo `.csproj`:**
```xml
<ItemGroup>
  <PackageReference Include="Newtonsoft.Json" Version="13.0.3" />
</ItemGroup>
```

**Agregar el using en los archivos que lo necesiten:**
```csharp
using Newtonsoft.Json;
```

---

## 5. Serializar un Objeto a JSON

El método principal es `JsonConvert.SerializeObject()`. Recibe un objeto C# y devuelve un `string` con el JSON correspondiente.

```csharp
using Newtonsoft.Json;

public class Persona
{
    public string Nombre { get; set; }
    public int Edad { get; set; }
    public string Email { get; set; }
}

// --- Uso ---

var persona = new Persona
{
    Nombre = "Juan",
    Edad = 30,
    Email = "juan@email.com"
};

// Serialización compacta (una sola línea)
string jsonCompacto = JsonConvert.SerializeObject(persona);
Console.WriteLine(jsonCompacto);
// Salida: {"Nombre":"Juan","Edad":30,"Email":"juan@email.com"}

// Serialización con formato (más legible)
string jsonFormateado = JsonConvert.SerializeObject(persona, Formatting.Indented);
Console.WriteLine(jsonFormateado);
// Salida:
// {
//   "Nombre": "Juan",
//   "Edad": 30,
//   "Email": "juan@email.com"
// }
```

> Para guardar en un archivo se prefiere `Formatting.Indented` porque el archivo es más fácil de leer e inspeccionar manualmente.

---

## 6. Deserializar JSON a un Objeto

El método es `JsonConvert.DeserializeObject<T>()`, donde `T` es el tipo al que queremos convertir el JSON. Recibe el string JSON y devuelve un objeto del tipo indicado.

```csharp
using Newtonsoft.Json;

string jsonString = @"{
  ""Nombre"": ""Ana"",
  ""Edad"": 25,
  ""Email"": ""ana@email.com""
}";

// Deserializar: convierte el texto JSON en un objeto Persona
Persona persona = JsonConvert.DeserializeObject<Persona>(jsonString);

Console.WriteLine(persona.Nombre);  // Ana
Console.WriteLine(persona.Edad);    // 25
Console.WriteLine(persona.Email);   // ana@email.com
```

> Los nombres de las propiedades en el JSON deben coincidir con los nombres de las propiedades de la clase C# (por defecto es sensible a mayúsculas y minúsculas).

---

## 7. Escribir y Leer Archivos

.NET nos provee la clase estática `File` en el namespace `System.IO` para operaciones de lectura y escritura de archivos de texto.

**Métodos principales:**

| Método | Descripción |
|---|---|
| `File.WriteAllText(ruta, contenido)` | Crea (o sobreescribe) un archivo con el texto indicado |
| `File.ReadAllText(ruta)` | Lee todo el contenido de un archivo como string |
| `File.Exists(ruta)` | Devuelve `true` si el archivo existe, `false` si no |

```csharp
using System.IO;
using Newtonsoft.Json;

var persona = new Persona { Nombre = "Carlos", Edad = 28, Email = "carlos@email.com" };

// Definir la ruta del archivo
string ruta = "persona.json";

// --- GUARDAR ---
string json = JsonConvert.SerializeObject(persona, Formatting.Indented);
File.WriteAllText(ruta, json);
Console.WriteLine("Persona guardada correctamente.");

// --- CARGAR ---
if (File.Exists(ruta))
{
    string contenido = File.ReadAllText(ruta);
    Persona personaCargada = JsonConvert.DeserializeObject<Persona>(contenido);
    Console.WriteLine($"Persona cargada: {personaCargada.Nombre}, {personaCargada.Edad} años");
}
else
{
    Console.WriteLine("El archivo no existe todavía.");
}
```

> Las rutas relativas como `"persona.json"` crean el archivo en el directorio desde donde se ejecuta la aplicación. En proyectos .NET, eso suele ser `bin/Debug/net8.0/`.

---

## 8. Serializar Listas

El mismo enfoque funciona para `List<T>`. JSON representa las listas como arrays `[...]`.

```csharp
using System.Collections.Generic;
using System.IO;
using Newtonsoft.Json;

var personas = new List<Persona>
{
    new Persona { Nombre = "Ana",   Edad = 25, Email = "ana@email.com"   },
    new Persona { Nombre = "Luis",  Edad = 32, Email = "luis@email.com"  },
    new Persona { Nombre = "Maria", Edad = 29, Email = "maria@email.com" }
};

string ruta = "personas.json";

// --- GUARDAR LA LISTA ---
string json = JsonConvert.SerializeObject(personas, Formatting.Indented);
File.WriteAllText(ruta, json);
Console.WriteLine($"Se guardaron {personas.Count} personas.");

// --- CARGAR LA LISTA ---
string contenido = File.ReadAllText(ruta);
List<Persona> personasCargadas = JsonConvert.DeserializeObject<List<Persona>>(contenido);
Console.WriteLine($"Se cargaron {personasCargadas.Count} personas.");

foreach (var p in personasCargadas)
{
    Console.WriteLine($"  - {p.Nombre} ({p.Edad} años)");
}
```

---

## 9. Patrón Completo: Guardar y Cargar

En proyectos bien organizados, encapsulamos la lógica de lectura y escritura en una clase llamada **Repositorio**. Esta clase es la única responsable de saber cómo y dónde se guardan los datos.

```csharp
using System.Collections.Generic;
using System.IO;
using Newtonsoft.Json;

public class PersonaRepositorio
{
    private string _rutaArchivo = "personas.json";

    /// <summary>
    /// Lee el archivo JSON y devuelve la lista de personas.
    /// Si el archivo no existe, devuelve una lista vacía.
    /// </summary>
    public List<Persona> ObtenerTodos()
    {
        if (!File.Exists(_rutaArchivo))
            return new List<Persona>();

        string json = File.ReadAllText(_rutaArchivo);
        return JsonConvert.DeserializeObject<List<Persona>>(json) ?? new List<Persona>();
    }

    /// <summary>
    /// Serializa la lista completa y sobreescribe el archivo.
    /// </summary>
    public void Guardar(List<Persona> lista)
    {
        string json = JsonConvert.SerializeObject(lista, Formatting.Indented);
        File.WriteAllText(_rutaArchivo, json);
    }
}

// --- Uso del repositorio ---

var repositorio = new PersonaRepositorio();

// Leer los datos existentes
List<Persona> personas = repositorio.ObtenerTodos();

// Agregar una nueva persona
personas.Add(new Persona { Nombre = "Sofia", Edad = 22, Email = "sofia@email.com" });

// Guardar la lista actualizada
repositorio.Guardar(personas);

Console.WriteLine($"Total de personas guardadas: {personas.Count}");
```

> **Principio importante:** siempre leer los datos existentes antes de agregar uno nuevo y guardar. Si no lo hacés, sobreescribís el archivo con solo el nuevo elemento, perdiendo los anteriores.

---

## 10. Arquitectura con Capa de Datos

A medida que los proyectos crecen, separamos el código en **capas** con responsabilidades bien definidas. Cada capa solo se comunica con la inmediatamente adyacente.

```
┌─────────────────────────────────┐
│      PRESENTACIÓN               │  ← Program.cs / Consola
│  Muestra datos, recibe input    │
└──────────────┬──────────────────┘
               │
┌──────────────▼──────────────────┐
│      SERVICIO                   │  ← PersonaService.cs
│  Lógica de negocio              │
└──────────────┬──────────────────┘
               │
┌──────────────▼──────────────────┐
│      DATOS                      │  ← PersonaRepositorio.cs
│  Lectura/escritura de archivos  │
└──────────────┬──────────────────┘
               │
┌──────────────▼──────────────────┐
│      ENTIDADES                  │  ← Persona.cs
│  Clases modelo (POCO)           │
└─────────────────────────────────┘
```

**Estructura de archivos del proyecto:**
```
MiProyecto/
├── Entidades/
│   └── Persona.cs
├── Datos/
│   └── PersonaRepositorio.cs
├── Servicios/
│   └── PersonaService.cs
└── Program.cs
```

**Ejemplo de la capa Servicio usando el Repositorio:**
```csharp
public class PersonaService
{
    private PersonaRepositorio _repositorio = new PersonaRepositorio();

    public void AgregarPersona(string nombre, int edad, string email)
    {
        var personas = _repositorio.ObtenerTodos();
        personas.Add(new Persona { Nombre = nombre, Edad = edad, Email = email });
        _repositorio.Guardar(personas);
    }

    public List<Persona> ObtenerPersonas()
    {
        return _repositorio.ObtenerTodos();
    }
}
```

---

## 11. Buenas Prácticas

Seguir estas prácticas evita errores comunes y hace el código más robusto:

**1. Siempre leer antes de guardar**
```csharp
// CORRECTO: leer primero, agregar, luego guardar
var lista = repositorio.ObtenerTodos();
lista.Add(nuevoElemento);
repositorio.Guardar(lista);

// INCORRECTO: sobreescribe con una lista de un solo elemento
repositorio.Guardar(new List<Persona> { nuevoElemento });
```

**2. Usar rutas configurables, no hardcodeadas profundamente**
```csharp
public class PersonaRepositorio
{
    // La ruta se puede configurar desde afuera si es necesario
    private string _rutaArchivo;

    public PersonaRepositorio(string rutaArchivo = "datos/personas.json")
    {
        _rutaArchivo = rutaArchivo;
    }
}
```

**3. Manejar el caso de primera ejecución**
```csharp
public List<Persona> ObtenerTodos()
{
    // Si el archivo no existe aún (primera vez que se corre la app)
    if (!File.Exists(_rutaArchivo))
        return new List<Persona>();

    string json = File.ReadAllText(_rutaArchivo);

    // Si el archivo existe pero está vacío o el JSON es inválido
    if (string.IsNullOrWhiteSpace(json))
        return new List<Persona>();

    // El ?? evita un null si DeserializeObject devuelve null
    return JsonConvert.DeserializeObject<List<Persona>>(json) ?? new List<Persona>();
}
```

**4. Verificar null después de deserializar**
```csharp
var persona = JsonConvert.DeserializeObject<Persona>(json);
if (persona == null)
{
    Console.WriteLine("Error: no se pudo deserializar el objeto.");
    return;
}
```

---

## 12. Manejo de Errores en Archivos

Las operaciones de archivo pueden fallar por muchos motivos: el archivo fue eliminado, hay un error de permisos, el JSON está corrupto, etc. Usamos `try-catch` para manejar estos casos.

**Excepciones comunes:**

| Excepción | Cuándo ocurre |
|---|---|
| `FileNotFoundException` | El archivo no existe en la ruta indicada |
| `UnauthorizedAccessException` | No hay permisos para leer o escribir el archivo |
| `JsonException` | El texto no es un JSON válido |
| `IOException` | Error general de entrada/salida |

**Ejemplo: método de lectura robusto con manejo de errores:**
```csharp
using System;
using System.IO;
using Newtonsoft.Json;

public List<Persona> ObtenerTodos()
{
    try
    {
        if (!File.Exists(_rutaArchivo))
        {
            Console.WriteLine("Archivo no encontrado. Se iniciará con lista vacía.");
            return new List<Persona>();
        }

        string json = File.ReadAllText(_rutaArchivo);
        return JsonConvert.DeserializeObject<List<Persona>>(json) ?? new List<Persona>();
    }
    catch (JsonException ex)
    {
        Console.WriteLine($"Error: el archivo contiene JSON inválido. Detalle: {ex.Message}");
        return new List<Persona>();
    }
    catch (IOException ex)
    {
        Console.WriteLine($"Error al leer el archivo. Detalle: {ex.Message}");
        return new List<Persona>();
    }
}

public void Guardar(List<Persona> lista)
{
    try
    {
        string json = JsonConvert.SerializeObject(lista, Formatting.Indented);
        File.WriteAllText(_rutaArchivo, json);
    }
    catch (IOException ex)
    {
        Console.WriteLine($"Error al guardar el archivo. Detalle: {ex.Message}");
    }
}
```

> En aplicaciones de producción, en lugar de `Console.WriteLine` se usaría un sistema de logging. En el contexto de este curso, mostrar el error en consola es suficiente.

---

## Resumen

| Concepto | Herramienta / Método |
|---|---|
| Serializar objeto a JSON | `JsonConvert.SerializeObject(objeto)` |
| Serializar con formato | `JsonConvert.SerializeObject(objeto, Formatting.Indented)` |
| Deserializar JSON a objeto | `JsonConvert.DeserializeObject<Tipo>(json)` |
| Escribir archivo | `File.WriteAllText(ruta, contenido)` |
| Leer archivo | `File.ReadAllText(ruta)` |
| Verificar existencia | `File.Exists(ruta)` |
| Clase que encapsula I/O | `Repositorio` (patrón de diseño) |
