# Unidad 6: Web Services y REST APIs con .NET Core

En esta unidad aprendemos a construir y consumir **APIs Web** usando ASP.NET Core. Esto nos permite exponer la lógica de nuestra aplicación a través de la red, habilitando que cualquier cliente (una app web, móvil, o consola) pueda interactuar con nuestros datos.

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

## 7. Modelo y DTOs

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
Un DTO es un objeto simplificado que usamos para transferir datos entre capas o entre cliente y servidor, cuando no queremos exponer el modelo completo.

**Ejemplo de uso:** si un `Producto` tiene 10 propiedades pero el cliente solo necesita `Nombre` y `Precio`, creamos un DTO:
```csharp
public class ProductoResumenDto
{
    public string Nombre { get; set; }
    public decimal Precio { get; set; }
}
```

**¿Cuándo usar DTOs?**
- Para ocultar campos sensibles (contraseñas, claves internas)
- Para simplificar la respuesta cuando el cliente no necesita todos los datos
- Para recibir datos de creación que no incluyen el `Id` (que lo genera el servidor)

En proyectos simples de este curso, podemos trabajar directamente con los modelos sin DTOs.

---

## 8. Patrón Servicio en Web API

El controlador no debe contener lógica de negocio. Su única responsabilidad es:
1. Recibir la petición HTTP
2. Llamar al servicio correspondiente
3. Devolver la respuesta HTTP adecuada

Toda la lógica vive en la capa **Servicio**.

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

    public Producto ObtenerPorId(int id)
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

---

## 9. Inyección de Dependencias

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
        return Ok(_servicio.ObtenerTodos());
    }
}
```

> El framework detecta que `ProductoController` necesita un `ProductoService` en su constructor, lo busca en el registro de DI, y lo provee automáticamente al crear el controlador.

---

## 10. Configuración de CORS

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

## 11. Consumir una API con HttpClient (Cliente)

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

## 12. Probar la API

Existen varias formas de probar los endpoints de una API mientras desarrollamos:

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

## 13. Arquitectura de la Aplicación Completa

Cuando combinamos todo lo visto en el curso, la arquitectura completa de una aplicación con Web API queda así:

```
┌─────────────────────────────────────────────────┐
│              CLIENTE (Frontend)                 │
│    HTML + JavaScript / App móvil / Consola      │
│   Hace peticiones HTTP, muestra los datos       │
└──────────────────┬──────────────────────────────┘
                   │  HTTP (GET, POST, PUT, DELETE)
                   │  Datos en JSON
┌──────────────────▼──────────────────────────────┐
│            CONTROLADORES (API Layer)            │
│    ProductoController, ClienteController, ...   │
│    Reciben la petición, llaman al servicio,     │
│    devuelven la respuesta HTTP adecuada         │
└──────────────────┬──────────────────────────────┘
                   │
┌──────────────────▼──────────────────────────────┐
│               SERVICIOS (Business Logic)        │
│    ProductoService, ClienteService, ...         │
│    Contienen las reglas de negocio              │
└──────────────────┬──────────────────────────────┘
                   │
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

**Estructura de archivos del proyecto completo:**
```
MiApi/
├── Controllers/
│   ├── ProductoController.cs
│   └── ClienteController.cs
├── Servicios/
│   ├── ProductoService.cs
│   └── ClienteService.cs
├── Datos/
│   ├── ProductoRepositorio.cs
│   └── ClienteRepositorio.cs
├── Entidades/
│   ├── Producto.cs
│   └── Cliente.cs
├── datos/
│   ├── productos.json        ← generado en tiempo de ejecución
│   └── clientes.json         ← generado en tiempo de ejecución
├── appsettings.json
├── Program.cs
└── MiApi.csproj
```

**Flujo de ejemplo para `GET /api/producto/5`:**
```
1. El cliente envía: GET https://localhost:7001/api/producto/5
2. ProductoController.ObtenerPorId(5) recibe la petición
3. Llama a _servicio.ObtenerPorId(5)
4. El servicio llama a _repositorio.ObtenerTodos()
5. El repositorio lee "datos/productos.json" y deserializa la lista
6. El servicio busca el producto con Id == 5 en la lista
7. El controlador recibe el producto y devuelve Ok(producto)
8. .NET serializa el objeto a JSON y lo envía al cliente con código 200
```

---

## Resumen

| Concepto | Herramienta / Clase |
|---|---|
| Crear proyecto API | `dotnet new webapi -n MiApi` |
| Definir controlador | `[ApiController]`, `[Route("api/[controller]")]` |
| Definir endpoints | `[HttpGet]`, `[HttpPost]`, `[HttpPut]`, `[HttpDelete]` |
| Devolver respuesta | `Ok()`, `NotFound()`, `CreatedAtAction()`, `NoContent()` |
| Inyectar dependencias | `builder.Services.AddSingleton<MiServicio>()` |
| Habilitar CORS | `builder.Services.AddCors(...)` + `app.UseCors(...)` |
| Consumir API | `HttpClient` + `GetAsync` / `PostAsync` |
| Probar endpoints | Swagger (`/swagger`), curl, navegador |
