# Unidad 6: Web Services y REST APIs con .NET Core

En esta unidad aprendemos a construir y consumir **APIs Web** usando ASP.NET Core. Esto nos permite exponer la lógica de nuestra aplicación a través de la red, habilitando que cualquier cliente (una app web, móvil, o consola) pueda interactuar con nuestros datos.

---

## Tabla de contenido

1. [¿Qué es un Web Service?](#1-qué-es-un-web-service)
2. [SOAP vs REST](#2-soap-vs-rest)
3. [¿Qué es REST?](#3-qué-es-rest)
4. [HTTP: Verbos y Códigos de Estado](#4-http-verbos-y-códigos-de-estado)
5. [Crear un Proyecto Web API con .NET Core](#5-crear-un-proyecto-web-api-con-net-core)
6. [Controllers y Endpoints](#6-controllers-y-endpoints)
7. [Instalar y Usar Postman para Probar la API](#7-instalar-y-usar-postman-para-probar-la-api)
8. [Filtrar Resultados con Query Strings](#8-filtrar-resultados-con-query-strings)
9. [Modelo y DTOs](#9-modelo-y-dtos)
10. [Validación de Modelos con Data Annotations](#10-validación-de-modelos-con-data-annotations)
11. [Patrón Servicio en Web API](#11-patrón-servicio-en-web-api)
12. [Manejo de Errores](#12-manejo-de-errores)
13. [Inyección de Dependencias](#13-inyección-de-dependencias)
14. [Logging con ILogger](#14-logging-con-ilogger)
15. [Configuración de CORS](#15-configuración-de-cors)
16. [Consumir una API con HttpClient (Cliente)](#16-consumir-una-api-con-httpclient-cliente)
17. [Probar la API](#17-probar-la-api)
18. [Arquitectura de la Aplicación Completa](#18-arquitectura-de-la-aplicación-completa)
19. [Resumen](#resumen)

---

## 1. ¿Qué es un Web Service?

Un **Web Service** es un componente de software accesible a través de una red (generalmente Internet o una red local) mediante protocolos estándar como HTTP. Permite que dos sistemas distintos se comuniquen e intercambien datos sin importar el lenguaje de programación ni el sistema operativo que usen.

**Beneficios principales:**
- **Interoperabilidad:** una app Android puede hablar con un servidor escrito en C#
- **Reusabilidad:** varios clientes distintos consumen el mismo servicio
- **Escalabilidad:** el servidor puede crecer de forma independiente al cliente
- **Separación de responsabilidades:** el frontend no necesita saber cómo funciona el backend

**Modelo cliente-servidor:**
```
Cliente (app web, móvil, consola)
        │
        │  Petición HTTP (GET /api/productos)
        ▼
Servidor (Web Service / API)
        │
        │  Respuesta HTTP (200 OK + JSON)
        ▼
Cliente recibe y procesa los datos
```

---

## 2. SOAP vs REST

Existen dos grandes enfoques para diseñar Web Services:

| Característica | SOAP | REST |
|---|---|---|
| Tipo | Protocolo estricto | Estilo arquitectónico |
| Formato de datos | XML obligatorio | JSON, XML u otros |
| Contrato | WSDL (archivo de definición) | Sin contrato formal (OpenAPI es opcional) |
| Complejidad | Alta | Baja |
| Estado | Puede ser con estado | Sin estado (stateless) |
| Uso típico | Sistemas bancarios, SAP, entornos enterprise | APIs modernas, microservicios, móvil |
| Verbo HTTP | Solo POST | GET, POST, PUT, DELETE, PATCH |

**Ejemplo de una petición SOAP (XML):**
```xml
<soap:Envelope xmlns:soap="http://schemas.xmlsoap.org/soap/envelope/">
  <soap:Body>
    <ObtenerProducto>
      <Id>42</Id>
    </ObtenerProducto>
  </soap:Body>
</soap:Envelope>
```

**Ejemplo equivalente en REST:**
```
GET /api/productos/42
```

**¿Por qué REST es preferido hoy?**
- Mucho más simple de implementar y consumir
- Usa JSON, que es más liviano que XML
- Compatible con cualquier cliente HTTP
- Fácil de probar desde el navegador o con herramientas como Postman o curl

---

## 3. ¿Qué es REST?

**REST** (REpresentational State Transfer) es un estilo arquitectónico para diseñar servicios web. No es un protocolo ni una librería, es un conjunto de principios y restricciones.

**Principios clave:**
- **Stateless (sin estado):** cada petición del cliente debe contener toda la información necesaria. El servidor no recuerda peticiones anteriores.
- **Interfaz uniforme:** los recursos se identifican mediante URLs consistentes.
- **Recursos identificados por URLs:** cada "cosa" que manejamos es un recurso con su propia URL.

**Recursos y sus URLs:**
```
GET    /api/productos          → obtener todos los productos
GET    /api/productos/5        → obtener el producto con id 5
POST   /api/productos          → crear un nuevo producto
PUT    /api/productos/5        → actualizar completamente el producto 5
DELETE /api/productos/5        → eliminar el producto 5
PATCH  /api/productos/5        → actualizar parcialmente el producto 5
```

**Verbos HTTP y su uso:**

| Verbo | Uso | Idempotente |
|---|---|---|
| `GET` | Obtener datos | Sí |
| `POST` | Crear un recurso nuevo | No |
| `PUT` | Reemplazar un recurso completo | Sí |
| `DELETE` | Eliminar un recurso | Sí |
| `PATCH` | Actualizar parcialmente un recurso | No necesariamente |

> **Idempotente** significa que ejecutar la operación múltiples veces produce el mismo resultado que ejecutarla una sola vez.

---

## 4. HTTP: Verbos y Códigos de Estado

Cuando un cliente hace una petición, el servidor responde con un **código de estado HTTP** que indica si todo salió bien o si hubo algún problema.

**Verbos y qué incluyen:**

| Verbo | ¿Lleva body en el request? | Ejemplo |
|---|---|---|
| GET | No | `GET /api/productos` |
| POST | Sí (el nuevo recurso en JSON) | `POST /api/productos` + JSON en el body |
| PUT | Sí (el recurso completo actualizado) | `PUT /api/productos/5` + JSON |
| DELETE | No | `DELETE /api/productos/5` |
| PATCH | Sí (solo los campos a modificar) | `PATCH /api/productos/5` + JSON parcial |

**Códigos de estado más importantes:**

| Código | Nombre | Cuándo se usa |
|---|---|---|
| `200 OK` | Éxito | GET o PUT exitoso |
| `201 Created` | Creado | POST exitoso, recurso creado |
| `204 No Content` | Sin contenido | DELETE exitoso (no hay nada que devolver) |
| `400 Bad Request` | Petición inválida | El cliente envió datos incorrectos |
| `404 Not Found` | No encontrado | El recurso no existe |
| `500 Internal Server Error` | Error interno | Fallo inesperado en el servidor |

---

## 5. Crear un Proyecto Web API con .NET Core

**.NET Core** incluye una plantilla para crear APIs Web listas para usar.

**Crear el proyecto:**
```bash
dotnet new webapi -n MiApi
cd MiApi
```

**Estructura del proyecto generado:**
```
MiApi/
├── Controllers/
│   └── WeatherForecastController.cs   ← controlador de ejemplo
├── Properties/
│   └── launchSettings.json
├── appsettings.json                   ← configuración de la app
├── appsettings.Development.json
├── MiApi.csproj
├── Program.cs                         ← punto de entrada y configuración
└── WeatherForecast.cs                 ← modelo de ejemplo
```

**Ejecutar el proyecto:**
```bash
dotnet run
```

La API queda disponible en `https://localhost:7xxx` (el puerto exacto se muestra en la consola).

**El controlador de ejemplo que viene con la plantilla:**
```csharp
using Microsoft.AspNetCore.Mvc;

[ApiController]
[Route("[controller]")]
public class WeatherForecastController : ControllerBase
{
    private static readonly string[] Summaries = new[]
    {
        "Freezing", "Bracing", "Chilly", "Cool", "Mild",
        "Warm", "Balmy", "Hot", "Sweltering", "Scorching"
    };

    [HttpGet(Name = "GetWeatherForecast")]
    public IEnumerable<WeatherForecast> Get()
    {
        return Enumerable.Range(1, 5).Select(index => new WeatherForecast
        {
            Date      = DateOnly.FromDateTime(DateTime.Now.AddDays(index)),
            TemperatureC = Random.Shared.Next(-20, 55),
            Summary   = Summaries[Random.Shared.Next(Summaries.Length)]
        }).ToArray();
    }
}
```

> **Nota — Minimal APIs:** .NET también permite definir endpoints sin controllers, directamente en `Program.cs` (`app.MapGet("/api/productos", ...)`). Se llaman **Minimal APIs** y son útiles para servicios muy chicos. En este curso usamos **Controllers**, porque organizan mejor el código a medida que la aplicación crece y combinan naturalmente con el patrón de capas (Controller → Servicio → Repositorio) que venimos usando.

---

## 6. Controllers y Endpoints

Un **Controller** es una clase que agrupa todos los endpoints relacionados a un recurso. Cada método público dentro del controller se convierte en un endpoint de la API.

**Atributos principales:**

| Atributo | Descripción |
|---|---|
| `[ApiController]` | Indica que la clase es un controlador de API (habilita validaciones automáticas) |
| `[Route("api/[controller]")]` | Define la URL base. `[controller]` se reemplaza por el nombre de la clase sin "Controller" |
| `[HttpGet]` | El método responde a peticiones GET |
| `[HttpPost]` | El método responde a peticiones POST |
| `[HttpPut("{id}")]` | El método responde a PUT con un parámetro en la URL |
| `[HttpDelete("{id}")]` | El método responde a DELETE con un parámetro en la URL |

**Ejemplo: un controlador completo para Productos:**
```csharp
using Microsoft.AspNetCore.Mvc;
using System.Collections.Generic;

[ApiController]
[Route("api/[controller]")]
public class ProductoController : ControllerBase
{
    private readonly ProductoService _servicio;

    public ProductoController(ProductoService servicio)
    {
        _servicio = servicio;
    }

    // GET api/producto
    [HttpGet]
    public IActionResult ObtenerTodos()
    {
        var productos = _servicio.ObtenerTodos();
        return Ok(productos);  // 200 OK + lista en JSON
    }

    // GET api/producto/5
    [HttpGet("{id}")]
    public IActionResult ObtenerPorId(int id)
    {
        var producto = _servicio.ObtenerPorId(id);
        if (producto == null)
            return NotFound();  // 404 Not Found

        return Ok(producto);    // 200 OK + objeto en JSON
    }

    // POST api/producto
    [HttpPost]
    public IActionResult Crear([FromBody] Producto producto)
    {
        _servicio.Agregar(producto);
        return CreatedAtAction(nameof(ObtenerPorId), new { id = producto.Id }, producto);
        // 201 Created
    }

    // PUT api/producto/5
    [HttpPut("{id}")]
    public IActionResult Actualizar(int id, [FromBody] Producto producto)
    {
        bool actualizado = _servicio.Actualizar(id, producto);
        if (!actualizado)
            return NotFound();  // 404 Not Found

        return Ok(producto);    // 200 OK + objeto actualizado
    }

    // DELETE api/producto/5
    [HttpDelete("{id}")]
    public IActionResult Eliminar(int id)
    {
        bool eliminado = _servicio.Eliminar(id);
        if (!eliminado)
            return NotFound();  // 404 Not Found

        return NoContent();     // 204 No Content
    }
}
```

> `IActionResult` es una interfaz que permite devolver distintos tipos de respuesta HTTP (`Ok`, `NotFound`, `CreatedAtAction`, `NoContent`, `BadRequest`, etc.) desde el mismo método.

---

## 7. Instalar y Usar Postman para Probar la API

Escribir cada `curl` a mano funciona, pero para probar una API con comodidad (guardar peticiones, armar el body en JSON, ver la respuesta formateada) lo más práctico es usar **Postman**, un cliente HTTP con interfaz gráfica.

### ¿Qué es Postman?

Postman es una aplicación que permite armar peticiones HTTP (GET, POST, PUT, DELETE, etc.), enviarlas a una API y visualizar la respuesta (status code, headers, body en JSON) sin escribir código ni comandos.

### Instalación

**Opción 1 — Descargar el instalador (recomendado):**
1. Ir a [https://www.postman.com/downloads/](https://www.postman.com/downloads/)
2. Descargar la versión para Windows
3. Ejecutar el instalador y seguir los pasos

**Opción 2 — Con winget (desde una terminal de Windows):**
```bash
winget install Postman.Postman
```

### Crear una petición

Con la API corriendo (`dotnet run`), en Postman:

1. Click en **New → HTTP Request** (o el botón `+` para una pestaña nueva)
2. Elegir el **verbo** en el dropdown (GET, POST, PUT, DELETE, ...)
3. Escribir la **URL** del endpoint
4. Si la petición lleva body (POST/PUT), ir a la pestaña **Body**, elegir **raw** y el formato **JSON**
5. Click en **Send**
6. La respuesta aparece abajo: **status code**, **tiempo**, y el **body** en JSON formateado

### Ejemplos con el `ProductoController` de la sección 6

**GET — obtener todos los productos**
```
Verbo: GET
URL:   https://localhost:7001/api/producto
```

**GET — obtener un producto por id**
```
Verbo: GET
URL:   https://localhost:7001/api/producto/5
```

**POST — crear un producto**
```
Verbo: POST
URL:   https://localhost:7001/api/producto
Body (raw, JSON):
{
    "nombre": "Laptop",
    "precio": 1500.00,
    "categoria": "Electrónica"
}
```

**PUT — actualizar un producto**
```
Verbo: PUT
URL:   https://localhost:7001/api/producto/5
Body (raw, JSON):
{
    "nombre": "Laptop Pro",
    "precio": 1800.00,
    "categoria": "Electrónica"
}
```

**DELETE — eliminar un producto**
```
Verbo: DELETE
URL:   https://localhost:7001/api/producto/5
```

> Al crear un `POST`, Postman debería mostrar `201 Created`; en un `DELETE` exitoso, `204 No Content` (sin body). Esto es una forma rápida de confirmar que el controller está devolviendo los códigos de estado correctos.

### Collections: guardar peticiones para reutilizarlas

En vez de reescribir cada petición, Postman permite agruparlas en una **Collection**:
1. Click en **New → Collection**, ponerle un nombre (por ejemplo, "MiApi")
2. Dentro de la collection, **Add request** para cada endpoint (GET todos, GET por id, POST, PUT, DELETE)
3. Guardar cada petición con **Save**

Así, la próxima vez que se quiera probar la API, ya están todas las peticiones armadas y solo hace falta apretar **Send**.

---

## 8. Filtrar Resultados con Query Strings

Además del `id` en la ruta, un endpoint `GET` puede recibir parámetros opcionales por **query string** (lo que va después del `?` en la URL) para filtrar resultados. Se capturan con el atributo `[FromQuery]`.

```
GET /api/producto?categoria=Electronica
```

**Ejemplo: filtrar productos por categoría**
```csharp
// GET api/producto?categoria=Electronica
[HttpGet]
public IActionResult ObtenerTodos([FromQuery] string? categoria)
{
    var productos = _servicio.ObtenerTodos();

    if (!string.IsNullOrEmpty(categoria))
        productos = productos.Where(p => p.Categoria == categoria).ToList();

    return Ok(productos);
}
```

> Si el parámetro no se envía (`GET /api/producto`), `categoria` llega en `null` y el filtro simplemente no se aplica. Esto permite que el mismo endpoint sirva tanto para "traer todo" como para "traer filtrado", sin duplicar código.

---

## 9. Modelo y DTOs

El **modelo** (también llamado entidad) es la clase que representa un objeto del dominio de negocio. Es la misma clase que usaríamos en nuestra capa de entidades.

```csharp
public class Producto
{
    public int Id { get; set; }
    public string Nombre { get; set; }
    public decimal Precio { get; set; }
    public string Categoria { get; set; }
    public bool Activo { get; set; }
}
```

**¿Qué es un DTO (Data Transfer Object)?**
Un DTO es un objeto simplificado que usamos para transferir datos entre capas o entre cliente y servidor, cuando no queremos exponer el modelo completo. **Request** y **Response** son los dos DTOs que vamos a usar en esta materia: un **Request** es el DTO que recibimos del cliente (el body de un `POST`/`PUT`), y un **Response** es el DTO que le devolvemos (el body de la respuesta). Los dos son DTOs — la única diferencia es la dirección en la que viaja el dato.

**¿Cuándo usar DTOs?**
- Para ocultar campos sensibles (contraseñas, claves internas)
- Para simplificar la respuesta cuando el cliente no necesita todos los datos
- Para recibir datos de creación/actualización que no incluyen el `Id` (lo genera el servidor) ni campos internos (como `Activo`)

### ¿Dónde van los DTOs?

Como venimos trabajando en la materia (Unidad 4), la solución no es un único proyecto: son varios proyectos separados dentro de la misma `.sln` — **proyecto de Servicio**, **proyecto de Test** y, a partir de esta unidad, **proyecto de Web API**. El proyecto de Web API es el que referencia al proyecto de Servicio (no al revés), y es ahí — dentro del proyecto de Web API — donde van los DTOs, junto a los Controllers: son un detalle de cómo la API expone los datos por HTTP, no parte de la lógica de negocio.

```
MiSolucion/
│
├── MiSolucion.sln
│
├── MiServicio/                      ← Proyecto Class Library (lógica de negocio)
│   ├── MiServicio.csproj
│   ├── Entidades/
│   │   └── Producto.cs
│   ├── Servicios/
│   │   └── ProductoService.cs
│   └── Datos/
│       └── ProductoRepositorio.cs
│
├── MiServicioTest/                  ← Proyecto NUnit Test (tests unitarios)
│   ├── MiServicioTest.csproj        ← Referencia a MiServicio
│   └── ProductoServiceTest.cs
│
└── MiApi/                           ← Proyecto Web API
    ├── MiApi.csproj                 ← Referencia a MiServicio
    ├── Controllers/
    │   └── ProductoController.cs
    ├── DTOs/
    │   ├── ProductoRequest.cs
    │   ├── ProductoResponse.cs
    │   └── ProductoMapper.cs
    └── Program.cs
```

Igual que se hizo con `MiServicioTest` en la Unidad 4, `MiApi` necesita una referencia al proyecto de Servicio para poder usar `Producto`, `ProductoService`, etc.:

```bash
dotnet sln MiSolucion.sln add MiApi/MiApi.csproj
dotnet add MiApi/MiApi.csproj reference MiServicio/MiServicio.csproj
```

> Los DTOs **no van en `MiServicio`**: ese proyecto es la lógica de negocio y no debería depender de cómo la expone la Web API (mañana podría exponerse por una app de consola, y no necesitaría los DTOs). Por eso viven del lado de `MiApi`, junto con los Controllers que los usan.

**Los DTOs que vamos a usar de acá en adelante:**

```csharp
// DTOs/ProductoResponse.cs
// Response: para exponer un producto al cliente (respuestas de GET, POST, PUT)
public class ProductoResponse
{
    public int Id { get; set; }
    public string Nombre { get; set; }
    public decimal Precio { get; set; }
    public string Categoria { get; set; }
}
```

```csharp
// DTOs/ProductoRequest.cs
// Request: para recibir datos al crear o actualizar (sin Id: en POST lo
// genera el servidor, en PUT ya viene en la URL)
public class ProductoRequest
{
    public string Nombre { get; set; }
    public decimal Precio { get; set; }
    public string Categoria { get; set; }
}
```

### Convertir entre DTO y Modelo (mapeo)

Cuando se usan DTOs, hace falta convertir en las dos direcciones:
- **Modelo → Response:** al responder al cliente (ocultamos u ordenamos los datos que se muestran)
- **Request → Modelo:** al recibir datos del cliente (armamos o actualizamos la entidad que se guarda)

A esta conversión se la llama **mapeo**. Es código repetitivo (copiar propiedad por propiedad), así que conviene concentrarlo en un solo lugar (`ProductoMapper`) en vez de repetirlo en cada método del servicio.

**Mapeo manual con métodos de extensión**

Una forma prolija de mapear es escribir métodos de extensión, para poder llamarlos como si fueran parte de la clase original (`producto.AProductoResponse()`):

```csharp
// DTOs/ProductoMapper.cs
public static class ProductoMapper
{
    // Modelo → Response
    public static ProductoResponse AProductoResponse(this Producto producto)
    {
        return new ProductoResponse
        {
            Id        = producto.Id,
            Nombre    = producto.Nombre,
            Precio    = producto.Precio,
            Categoria = producto.Categoria
        };
    }

    // Request → Modelo nuevo (para crear)
    public static Producto AProducto(this ProductoRequest request)
    {
        return new Producto
        {
            Nombre    = request.Nombre,
            Precio    = request.Precio,
            Categoria = request.Categoria,
            Activo    = true
        };
    }

}
```

> Notar que el mapper solo tiene los dos métodos: **Modelo → Response** y **Request → Modelo**. No hace falta un método para "aplicar un Request sobre un Producto existente" — ya vamos a ver por qué al hablar de `Actualizar`.

**¿Dónde se usa el mapeo? En el Controller, no en el Servicio**

Podría parecer natural que el Servicio reciba el `Request` y devuelva el `Response` (así lo planteamos en un principio), pero eso **no compila**: el proyecto `MiServicio` no puede conocer `ProductoRequest` ni `ProductoResponse`, porque esas clases viven en `MiApi`. Y `MiApi` ya referencia a `MiServicio` (para usar `Producto` y `ProductoService`). Si `MiServicio` tuviera que referenciar a `MiApi` para usar los DTOs, tendríamos una **referencia circular** (`MiApi → MiServicio → MiApi`), y una solución de .NET no puede compilar con dos proyectos que se referencian mutuamente.

La solución es que el **Servicio no sepa que existen los DTOs**: recibe y devuelve `Producto` únicamente, igual que en la sección 11. Es el **Controller** — que ya referencia a los dos (`MiServicio` para el modelo, y sus propios DTOs) — el que llama a los métodos de mapeo antes de pasarle datos al Servicio, y después de recibirlos:

```csharp
// GET api/producto/5
[HttpGet("{id}")]
public IActionResult ObtenerPorId(int id)
{
    var producto = _servicio.ObtenerPorId(id);   // el Servicio devuelve Producto
    if (producto == null)
        return NotFound();

    return Ok(producto.AProductoResponse());     // el Controller mapea Modelo → Response
}

// POST api/producto
[HttpPost]
public IActionResult Crear([FromBody] ProductoRequest request)
{
    var producto = request.AProducto();          // el Controller mapea Request → Modelo
    _servicio.Agregar(producto);                 // el Servicio recibe y guarda un Producto

    // 201 Created + solo el ProductoResponse (sin Location ni nada más)
    return StatusCode(201, producto.AProductoResponse());
}

// PUT api/producto/5
[HttpPut("{id}")]
public IActionResult Actualizar(int id, [FromBody] ProductoRequest request)
{
    var producto = request.AProducto();          // el Controller mapea Request → Modelo
    producto.Id = id;                            // el Id viene de la URL, no del Request

    bool actualizado = _servicio.Actualizar(id, producto);
    if (!actualizado)
        return NotFound();

    return Ok(producto.AProductoResponse());     // 200 OK + el ProductoResponse actualizado
}
```

> Tanto el `POST` como el `PUT` devuelven **solo la entidad** (el `ProductoResponse`) y nada más: `StatusCode(201, response)` para el `POST` (sin `Location` ni depender de `nameof(ObtenerPorId)`) y `Ok(response)` para el `PUT` (en vez de `NoContent`), para que el cliente reciba siempre el estado final del recurso.

Para `Actualizar` (`PUT`) esto también simplifica las cosas: en vez de "aplicar" el Request sobre el `Producto` existente (lo que sí necesitaría un tercer método de mapeo mezclando ambos tipos), el Controller arma un `Producto` transitorio con `request.AProducto()` y se lo pasa al Servicio, que ya sabe copiar esos campos sobre el existente — igual que hacía antes de introducir DTOs (sección 11).

> **Librerías de mapeo automático:** en proyectos reales, con muchos DTOs, escribir todos los métodos de mapeo a mano se vuelve repetitivo. Librerías como **AutoMapper** generan el mapeo automáticamente a partir de la coincidencia de nombres de propiedades. Está fuera del alcance de este curso, pero es útil saber que existe.

**De acá en adelante, todos los ejemplos de este apunte van a usar estos DTOs:** el Controller recibe un `ProductoRequest` y devuelve un `ProductoResponse`, mapeando con `ProductoMapper` en los dos sentidos. El Servicio (sección 11) sigue trabajando pura y exclusivamente con `Producto` — no sabe que los DTOs existen.

---

## 10. Validación de Modelos con Data Annotations

Hasta ahora, si un cliente manda un `POST` con un `ProductoRequest` incompleto o con datos inválidos (por ejemplo, precio negativo), el controller lo acepta igual. Para evitar eso, usamos **Data Annotations**: atributos que se agregan a las propiedades para declarar reglas de validación.

> Las Data Annotations van en el **Request** (`ProductoRequest`), no en el modelo (`Producto`). El Request es lo que efectivamente manda el cliente por HTTP, así que es ahí donde tiene sentido validar; el modelo queda libre de atributos de validación.

```csharp
// DTOs/ProductoRequest.cs
using System.ComponentModel.DataAnnotations;

public class ProductoRequest
{
    [Required(ErrorMessage = "El nombre es obligatorio")]
    [StringLength(100, MinimumLength = 2)]
    public string Nombre { get; set; }

    [Range(0.01, 1000000, ErrorMessage = "El precio debe ser mayor a 0")]
    public decimal Precio { get; set; }

    public string Categoria { get; set; }
}
```

**Atributos de validación más comunes:**

| Atributo | Qué valida |
|---|---|
| `[Required]` | El valor no puede ser nulo/vacío |
| `[Range(min, max)]` | El valor numérico debe estar en un rango |
| `[StringLength(max, MinimumLength = min)]` | Longitud de un texto |
| `[EmailAddress]` | El texto tiene formato de email |
| `[RegularExpression("...")]` | El texto cumple una expresión regular |

**¿Cómo se aplica automáticamente?**
Gracias al atributo `[ApiController]` (que ya usamos en todos nuestros controllers), ASP.NET Core valida el Request **antes** de ejecutar el método. Si algo no cumple las reglas, devuelve automáticamente un `400 Bad Request` con el detalle de los errores, sin que tengamos que escribir ese chequeo a mano.

```csharp
// POST api/producto
[HttpPost]
public IActionResult Crear([FromBody] ProductoRequest request)
{
    // Si "request" no cumple las Data Annotations,
    // esta línea nunca se ejecuta: [ApiController] ya respondió 400 Bad Request
    var producto = request.AProducto();
    _servicio.Agregar(producto);
    return StatusCode(201, producto.AProductoResponse());
}
```

> Si quisiéramos controlar la validación manualmente (por ejemplo, sin `[ApiController]`), se hace consultando `if (!ModelState.IsValid) return BadRequest(ModelState);` al inicio del método.

---

## 11. Patrón Servicio en Web API

El controlador no debe contener lógica de negocio. Su única responsabilidad es:
1. Recibir la petición HTTP
2. Llamar al servicio correspondiente
3. Devolver la respuesta HTTP adecuada

Toda la lógica vive en la capa **Servicio**.

> **El Servicio no conoce los DTOs.** Vive en el proyecto `MiServicio`, que no referencia a `MiApi` (sección 9) — así que solo puede trabajar con `Producto`. Es el Controller el que mapea `Request`/`Response` antes y después de llamar al Servicio.

**Ejemplo: ProductoService**
```csharp
using System.Collections.Generic;
using System.Linq;

public class ProductoService
{
    private ProductoRepositorio _repositorio = new ProductoRepositorio();

    public List<Producto> ObtenerTodos()
    {
        return _repositorio.ObtenerTodos();
    }

    public Producto? ObtenerPorId(int id)
    {
        var productos = _repositorio.ObtenerTodos();
        return productos.FirstOrDefault(p => p.Id == id);
    }

    public void Agregar(Producto producto)
    {
        var productos = _repositorio.ObtenerTodos();
        // Generar nuevo Id automáticamente
        producto.Id = productos.Count > 0 ? productos.Max(p => p.Id) + 1 : 1;
        productos.Add(producto);
        _repositorio.Guardar(productos);
    }

    public bool Actualizar(int id, Producto productoActualizado)
    {
        var productos = _repositorio.ObtenerTodos();
        var existente = productos.FirstOrDefault(p => p.Id == id);
        if (existente == null) return false;

        existente.Nombre    = productoActualizado.Nombre;
        existente.Precio    = productoActualizado.Precio;
        existente.Categoria = productoActualizado.Categoria;
        _repositorio.Guardar(productos);
        return true;
    }

    public bool Eliminar(int id)
    {
        var productos = _repositorio.ObtenerTodos();
        var existente = productos.FirstOrDefault(p => p.Id == id);
        if (existente == null) return false;

        productos.Remove(existente);
        _repositorio.Guardar(productos);
        return true;
    }
}
```

**El Controller correspondiente (con el mapeo Request/Response):**
```csharp
[ApiController]
[Route("api/[controller]")]
public class ProductoController : ControllerBase
{
    private readonly ProductoService _servicio;

    public ProductoController(ProductoService servicio)
    {
        _servicio = servicio;
    }

    [HttpGet]
    public IActionResult ObtenerTodos()
    {
        var responses = _servicio.ObtenerTodos().Select(p => p.AProductoResponse());
        return Ok(responses);
    }

    [HttpGet("{id}")]
    public IActionResult ObtenerPorId(int id)
    {
        var producto = _servicio.ObtenerPorId(id);
        if (producto == null)
            return NotFound();

        return Ok(producto.AProductoResponse());
    }

    [HttpPost]
    public IActionResult Crear([FromBody] ProductoRequest request)
    {
        var producto = request.AProducto();
        _servicio.Agregar(producto);
        return StatusCode(201, producto.AProductoResponse());
    }

    [HttpPut("{id}")]
    public IActionResult Actualizar(int id, [FromBody] ProductoRequest request)
    {
        var producto = request.AProducto();
        producto.Id = id;    // el Id viene de la URL, no del Request

        bool actualizado = _servicio.Actualizar(id, producto);
        if (!actualizado)
            return NotFound();

        return Ok(producto.AProductoResponse());
    }

    [HttpDelete("{id}")]
    public IActionResult Eliminar(int id)
    {
        bool eliminado = _servicio.Eliminar(id);
        if (!eliminado)
            return NotFound();

        return NoContent();
    }
}
```

> Notar que ni el Repositorio ni el Servicio se enteran de que existen los DTOs: los dos trabajan pura y exclusivamente con `Producto`. Los DTOs y su mapeo son un detalle exclusivo de la capa de Controllers (`MiApi`), que es la única con visibilidad sobre ambos tipos.

---

## 12. Manejo de Errores

Las Data Annotations cubren datos inválidos, pero no cubren **fallos inesperados**: el archivo JSON no existe, está corrupto, o hay un error de programación. Si eso pasa y no lo manejamos, el cliente recibe un `500 Internal Server Error` sin ninguna explicación útil.

**Opción 1: `try/catch` en el controller**
```csharp
[HttpGet("{id}")]
public IActionResult ObtenerPorId(int id)
{
    try
    {
        var producto = _servicio.ObtenerPorId(id);
        if (producto == null)
            return NotFound();

        return Ok(producto.AProductoResponse());
    }
    catch (Exception ex)
    {
        return StatusCode(500, new { mensaje = "Ocurrió un error inesperado", detalle = ex.Message });
    }
}
```

**Opción 2: manejo centralizado (evita repetir `try/catch` en cada endpoint)**
```csharp
var app = builder.Build();

if (!app.Environment.IsDevelopment())
{
    // En producción, redirige cualquier excepción no controlada a /error
    app.UseExceptionHandler("/error");
}

app.MapControllers();
app.Run();
```

> En desarrollo, ASP.NET Core ya muestra una página detallada de errores por defecto. La opción centralizada se usa para mostrar un mensaje genérico y seguro en producción, sin exponer detalles internos del servidor.

---

## 13. Inyección de Dependencias

La **Inyección de Dependencias** (DI) es un patrón de diseño donde una clase no crea sus propias dependencias, sino que las recibe desde afuera. .NET Core tiene un sistema de DI integrado.

**¿Por qué usarla?**
- Las clases son más fáciles de testear (podemos inyectar versiones simuladas)
- Reduce el acoplamiento entre clases
- El ciclo de vida de los objetos está centralizado

**Registrar servicios en `Program.cs`:**
```csharp
var builder = WebApplication.CreateBuilder(args);

// Agregar servicios al contenedor de DI
builder.Services.AddControllers();

// Registrar nuestros propios servicios
builder.Services.AddSingleton<ProductoService>();
// AddSingleton  → una sola instancia para toda la app
// AddScoped     → una instancia por petición HTTP
// AddTransient  → una instancia nueva cada vez que se solicita

var app = builder.Build();
app.MapControllers();
app.Run();
```

**Inyección en el constructor del Controller:**
```csharp
[ApiController]
[Route("api/[controller]")]
public class ProductoController : ControllerBase
{
    // .NET inyecta automáticamente el ProductoService registrado en Program.cs
    private readonly ProductoService _servicio;

    public ProductoController(ProductoService servicio)
    {
        _servicio = servicio;
    }

    [HttpGet]
    public IActionResult ObtenerTodos()
    {
        var responses = _servicio.ObtenerTodos().Select(p => p.AProductoResponse());
        return Ok(responses);
    }
}
```

> El framework detecta que `ProductoController` necesita un `ProductoService` en su constructor, lo busca en el registro de DI, y lo provee automáticamente al crear el controlador.

---

## 14. Logging con ILogger

`ILogger<T>` es un servicio que .NET Core registra automáticamente en el contenedor de DI, sin que tengamos que hacer nada en `Program.cs`. Es el ejemplo más directo de inyección de dependencias "gratis" que ofrece el framework, y sirve para dejar rastro de lo que hace la API mientras corre.

```csharp
public class ProductoController : ControllerBase
{
    private readonly ProductoService _servicio;
    private readonly ILogger<ProductoController> _logger;

    public ProductoController(ProductoService servicio, ILogger<ProductoController> logger)
    {
        _servicio = servicio;
        _logger = logger;
    }

    [HttpGet("{id}")]
    public IActionResult ObtenerPorId(int id)
    {
        _logger.LogInformation("Buscando producto con id {Id}", id);

        var producto = _servicio.ObtenerPorId(id);
        if (producto == null)
        {
            _logger.LogWarning("Producto con id {Id} no encontrado", id);
            return NotFound();
        }

        return Ok(producto.AProductoResponse());
    }
}
```

**Niveles de log más usados:**

| Método | Cuándo usarlo |
|---|---|
| `LogInformation` | Eventos normales (una petición se procesó) |
| `LogWarning` | Algo raro pero no fatal (recurso no encontrado) |
| `LogError` | Una excepción o fallo real |
| `LogDebug` | Detalle solo útil mientras se desarrolla |

Estos logs aparecen en la consola mientras corremos `dotnet run`, y son la primera herramienta para diagnosticar qué está pasando cuando algo no funciona como se espera.

---

## 15. Configuración de CORS

**CORS** (Cross-Origin Resource Sharing) es un mecanismo de seguridad del navegador que bloquea las peticiones a un dominio diferente al de la página actual. Por ejemplo, si tu frontend corre en `http://localhost:3000` y tu API en `http://localhost:5000`, el navegador bloqueará las peticiones por defecto.

**¿Por qué existe?** Para proteger a los usuarios de ataques donde una página maliciosa intenta hacer peticiones a otras APIs en nombre del usuario.

**Habilitar CORS en `Program.cs`:**
```csharp
var builder = WebApplication.CreateBuilder(args);

builder.Services.AddControllers();

// Definir una política de CORS
builder.Services.AddCors(options =>
{
    options.AddPolicy("PermitirTodo", policy =>
    {
        policy.AllowAnyOrigin()   // Permite cualquier origen
              .AllowAnyMethod()   // Permite GET, POST, PUT, DELETE, etc.
              .AllowAnyHeader();  // Permite cualquier header
    });
});

var app = builder.Build();

// Aplicar la política de CORS (debe ir antes de MapControllers)
app.UseCors("PermitirTodo");
app.MapControllers();
app.Run();
```

> En producción, reemplazá `AllowAnyOrigin()` por `.WithOrigins("https://mi-frontend.com")` para restringir el acceso solo a los orígenes conocidos.

---

## 16. Consumir una API con HttpClient (Cliente)

Para que nuestra aplicación consuma una API externa (o la propia API), usamos `HttpClient`. Esta clase permite enviar peticiones HTTP y recibir respuestas.

**Ejemplo: aplicación de consola que consulta una API pública**
```csharp
using System;
using System.Net.Http;
using System.Threading.Tasks;
using Newtonsoft.Json;
using System.Collections.Generic;

// Modelo que coincide con la respuesta de la API
public class Post
{
    public int Id { get; set; }
    public int UserId { get; set; }
    public string Title { get; set; }
    public string Body { get; set; }
}

class Program
{
    static async Task Main(string[] args)
    {
        using var cliente = new HttpClient();

        // GET: obtener todos los posts
        string url = "https://jsonplaceholder.typicode.com/posts";
        HttpResponseMessage respuesta = await cliente.GetAsync(url);

        if (respuesta.IsSuccessStatusCode)
        {
            string json = await respuesta.Content.ReadAsStringAsync();
            List<Post> posts = JsonConvert.DeserializeObject<List<Post>>(json);

            Console.WriteLine($"Se recibieron {posts.Count} posts.");
            foreach (var post in posts.Take(3))
            {
                Console.WriteLine($"[{post.Id}] {post.Title}");
            }
        }
        else
        {
            Console.WriteLine($"Error: {respuesta.StatusCode}");
        }
    }
}
```

**Ejemplo: POST para crear un recurso**
```csharp
using System.Text;

var nuevoPost = new Post { UserId = 1, Title = "Mi nuevo post", Body = "Contenido..." };
string jsonBody = JsonConvert.SerializeObject(nuevoPost);
var content = new StringContent(jsonBody, Encoding.UTF8, "application/json");

HttpResponseMessage respuesta = await cliente.PostAsync(url, content);
Console.WriteLine($"Código de respuesta: {(int)respuesta.StatusCode} {respuesta.StatusCode}");
```

---

## 17. Probar la API

Existen varias formas de probar los endpoints de una API mientras desarrollamos:

> Para probar con **Postman**, ver la sección 7.

### Desde el navegador (solo GET)
Para peticiones GET simples, podés escribir la URL directamente en el navegador:
```
https://localhost:7001/api/producto
https://localhost:7001/api/producto/5
```

### Usando curl (todos los verbos)
`curl` es una herramienta de línea de comandos para hacer peticiones HTTP:

```bash
# GET: obtener todos los productos
curl https://localhost:7001/api/producto

# GET: obtener producto por id
curl https://localhost:7001/api/producto/5

# POST: crear un nuevo producto
curl -X POST https://localhost:7001/api/producto \
     -H "Content-Type: application/json" \
     -d '{"nombre": "Laptop", "precio": 1500.00, "categoria": "Electrónica"}'

# PUT: actualizar un producto
curl -X PUT https://localhost:7001/api/producto/5 \
     -H "Content-Type: application/json" \
     -d '{"nombre": "Laptop Pro", "precio": 1800.00, "categoria": "Electrónica"}'

# DELETE: eliminar un producto
curl -X DELETE https://localhost:7001/api/producto/5
```

### Swagger UI (incluido en .NET Web API)
Cuando creás un proyecto con `dotnet new webapi`, Swagger viene configurado por defecto. Al correr la app y navegar a:
```
https://localhost:7001/swagger
```
Vas a ver una interfaz gráfica que lista todos los endpoints, permite ejecutarlos directamente desde el navegador, y muestra la estructura esperada de los datos.

**Cómo está configurado en `Program.cs` (ya viene por defecto):**
```csharp
var builder = WebApplication.CreateBuilder(args);

builder.Services.AddControllers();
builder.Services.AddEndpointsApiExplorer();
builder.Services.AddSwaggerGen();  // Registra Swagger

var app = builder.Build();

if (app.Environment.IsDevelopment())
{
    app.UseSwagger();
    app.UseSwaggerUI();  // Habilita la interfaz gráfica en /swagger
}

app.UseHttpsRedirection();
app.MapControllers();
app.Run();
```

---

## 18. Arquitectura de la Aplicación Completa

Cuando combinamos todo lo visto en el curso, la arquitectura completa de una aplicación con Web API queda así:

```
┌─────────────────────────────────────────────────┐
│              CLIENTE (Frontend)                 │
│    HTML + JavaScript / App móvil / Consola      │
│   Hace peticiones HTTP, muestra los datos       │
└──────────────────┬──────────────────────────────┘
                   │  HTTP (GET, POST, PUT, DELETE)
                   │  DTOs en JSON (ProductoRequest, ProductoResponse)
┌──────────────────▼──────────────────────────────┐
│            CONTROLADORES (API Layer)            │
│    ProductoController, ClienteController, ...   │
│    Reciben/devuelven DTOs, validan el Request,  │
│    mapean DTO ↔ Modelo (DTOs/*Mapper.cs) y      │
│    llaman al servicio con/desde el Modelo       │
└──────────────────┬──────────────────────────────┘
                   │  Modelo (Entidad) — nunca DTOs
┌──────────────────▼──────────────────────────────┐
│               SERVICIOS (Business Logic)        │
│    ProductoService, ClienteService, ...         │
│    No conocen los DTOs: solo trabajan con el    │
│    Modelo y contienen las reglas de negocio     │
└──────────────────┬──────────────────────────────┘
                   │  Modelo (Entidad)
┌──────────────────▼──────────────────────────────┐
│               REPOSITORIOS (Data Layer)         │
│    ProductoRepositorio, ClienteRepositorio, ... │
│    Leen y escriben archivos JSON                │
└──────────────────┬──────────────────────────────┘
                   │
┌──────────────────▼──────────────────────────────┐
│              ARCHIVOS JSON (Persistencia)       │
│    productos.json, clientes.json, ...           │
└─────────────────────────────────────────────────┘
```

**Estructura de archivos de la solución completa** (los 3 proyectos de la sección 9):
```
MiSolucion/
│
├── MiSolucion.sln
│
├── MiServicio/                     ← lógica de negocio (no conoce los DTOs)
│   ├── MiServicio.csproj
│   ├── Entidades/
│   │   ├── Producto.cs
│   │   └── Cliente.cs
│   ├── Servicios/
│   │   ├── ProductoService.cs
│   │   └── ClienteService.cs
│   └── Datos/
│       ├── ProductoRepositorio.cs
│       ├── ClienteRepositorio.cs
│       ├── productos.json        ← generado en tiempo de ejecución
│       └── clientes.json         ← generado en tiempo de ejecución
│
├── MiServicioTest/                 ← Referencia a MiServicio
│   ├── MiServicioTest.csproj
│   ├── ProductoServiceTest.cs
│   └── ClienteServiceTest.cs
│
└── MiApi/                          ← Referencia a MiServicio
    ├── MiApi.csproj
    ├── Controllers/
    │   ├── ProductoController.cs
    │   └── ClienteController.cs
    ├── DTOs/
    │   ├── ProductoRequest.cs
    │   ├── ProductoResponse.cs
    │   ├── ProductoMapper.cs
    │   ├── ClienteRequest.cs
    │   ├── ClienteResponse.cs
    │   └── ...
    ├── appsettings.json
    └── Program.cs
```

**Flujo de ejemplo para `GET /api/producto/5`:**
```
1. El cliente envía: GET https://localhost:7001/api/producto/5
2. ProductoController.ObtenerPorId(5) recibe la petición
3. Llama a _servicio.ObtenerPorId(5)
4. El servicio llama a _repositorio.ObtenerTodos()
5. El repositorio lee "productos.json" y deserializa la lista de Producto
6. El servicio busca el Producto con Id == 5 en la lista y lo devuelve tal cual
7. El controlador recibe el Producto y lo mapea a ProductoResponse (producto.AProductoResponse())
8. El controlador devuelve Ok(productoResponse)
9. .NET serializa el DTO a JSON y lo envía al cliente con código 200
```

---

## Resumen

| Concepto | Herramienta / Clase |
|---|---|
| Crear proyecto API | `dotnet new webapi -n MiApi` |
| Definir controlador | `[ApiController]`, `[Route("api/[controller]")]` |
| Definir endpoints | `[HttpGet]`, `[HttpPost]`, `[HttpPut]`, `[HttpDelete]` |
| Filtrar por query string | `[FromQuery]` |
| Mapear DTO (Request/Response) ↔ Modelo, en el Controller | Métodos de extensión (`AProductoResponse()`, `AProducto()`) |
| Devolver respuesta | `Ok()`, `NotFound()`, `StatusCode(201, ...)`, `NoContent()` |
| Validar el DTO de entrada | Data Annotations (`[Required]`, `[Range]`, ...) + `[ApiController]` |
| Manejar errores | `try/catch`, `app.UseExceptionHandler(...)` |
| Inyectar dependencias | `builder.Services.AddSingleton<MiServicio>()` |
| Registrar actividad | `ILogger<T>` (`LogInformation`, `LogWarning`, `LogError`) |
| Habilitar CORS | `builder.Services.AddCors(...)` + `app.UseCors(...)` |
| Consumir API | `HttpClient` + `GetAsync` / `PostAsync` |
| Probar endpoints | Swagger (`/swagger`), curl, navegador |
