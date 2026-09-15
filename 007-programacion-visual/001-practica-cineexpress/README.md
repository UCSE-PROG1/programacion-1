# Práctica en clase: CineExpress — Catálogo de películas

## Objetivo

Aplicar, en un ejercicio nuevo y de forma individual, los mismos conceptos que se usaron para construir el catálogo de **TechStore** en la teoría de la unidad: HTML semántico, CSS libre, JavaScript (DOM, eventos, `localStorage`, Fetch API) y una **Web API mínima en .NET** que le sirve los datos al frontend.

**Duración estimada: 2 horas de clase.**

---

## Contexto del ejercicio

**CineExpress** es un videoclub que todavía renta películas en DVD. El dueño quiere una página donde sus clientes puedan ver el catálogo de películas disponibles, marcar cuáles quieren ver, y donde el encargado del local pueda sumar nuevos títulos.

No es el mismo ejercicio que TechStore: es otro dominio, con otros nombres de campos y de funciones. Podés mirar el checkpoint final de la teoría como referencia de **estructura y estilo de código**, pero el objetivo es que vos mismo/a escribas el HTML, el CSS y el JS de esta página, adaptando cada pieza al nuevo enunciado.

---

## Enunciado

Armá dos cosas: una página (`index.html` + `estilos.css` + `app.js`) que muestre el catálogo de películas de CineExpress, y una **Web API en .NET** que le devuelve los datos a esa página — igual que el ejemplo de TechStore, pero con un backend más simple: **un único proyecto**, sin capas de Service/Repository ni proyecto de tests. Un `Controller` que trabaje directo sobre una lista en memoria alcanza.

Cada película tiene: `id`, `titulo`, `genero` (uno de: Acción, Comedia, Terror, Drama, Ciencia Ficción), `duracion` (en minutos) y `copiasDisponibles` (cuántos DVD quedan en stock).

### 1. Estructura (HTML)

- `<header>` con el nombre del videoclub y un `<nav>` con anclas a las secciones de la página (catálogo, agregar película, nosotros).
- `<main>` con al menos dos zonas:
  - Una **sección de catálogo** (`id="catalogo"`) con un contenedor vacío (`id="grid-peliculas"`) donde JavaScript va a inyectar una tarjeta (`<article>`) por película.
  - Una **sección de formulario** (`id="agregar"`) para agregar una película nueva.
- Un `<aside>` con la lista de géneros disponibles (puede ser una lista fija en HTML, como la de categorías de TechStore).
- Un `<footer>` con algún dato de contacto.
- El formulario (`<form id="form-pelicula">`) debe pedir: título (texto, obligatorio, mínimo 2 caracteres), duración (número, obligatorio, mínimo 1), copias disponibles (número, obligatorio, mínimo 0) y género (`<select>` con las 5 opciones). Usá los atributos de validación HTML5 (`required`, `minlength`, `min`) — sección 8 de la teoría.
- Cada `<label>` vinculado a su `<input>` con `for`/`id`.

### 2. Estilo (CSS)

Acá no hay una lista de requisitos: dale el estilo que quieras a la página, con las herramientas de CSS que ya vimos (colores, Box Model, Flexbox, Grid, variables, `:hover`, media queries, etc.). Usá tu criterio — lo importante es que se note que la página tiene una hoja de estilos propia y que se ve prolija, no que cumpla una lista de casilleros.

### 3. Comportamiento (JavaScript)

- Al cargar la página (`DOMContentLoaded`), traer las películas con `fetch` desde la API (variable `API_URL` apuntando a `http://localhost:5000/api/peliculas` o el puerto que te asigne tu proyecto) y guardarlas en un array `peliculas` en memoria.
- Una función que renderice ese array como tarjetas dentro de `#grid-peliculas`, usando `map` y template literals (igual que `renderizarCatalogo` en TechStore). Cada tarjeta debe mostrar título, género, duración y copias disponibles.
- Cada tarjeta tiene un botón de favorito ("🎬 Quiero verla" / marcado como favorita). Los clics se manejan con **un solo listener por delegación de eventos** en el contenedor de la grilla (no un listener por tarjeta).
- Los favoritos se guardan en un array separado (`favoritos`, array de ids) y **persisten con `localStorage`** en el navegador — no hace falta que el backend sepa nada de favoritos, es una preferencia del cliente (igual que en TechStore).
- El formulario de "agregar película", al enviarse:
  - Frena el envío por defecto (`event.preventDefault()`).
  - Lee los valores de los inputs.
  - Hace un `fetch` con `method: "POST"` a la misma `API_URL`, mandando el objeto película en el `body` (como JSON).
  - Vuelve a pedir el catálogo completo a la API y re-renderiza la grilla.
  - Limpia el formulario (`event.target.reset()`).
- Mostrá algún mensaje mientras carga (o si falla la conexión con la API), como el `#estado-carga` de TechStore.

### 4. Backend (Web API)

- Crear un **único proyecto** de Web API en .NET (`dotnet new webapi -n CineExpressApi`, sin controladores de ejemplo).
- Una clase `Pelicula` con las propiedades del enunciado (`Id`, `Titulo`, `Genero`, `Duracion`, `CopiasDisponibles`).
- Un `PeliculasController` con:
  - `GET api/peliculas`: devuelve la lista completa (una lista `static` en memoria dentro del controller alcanza, con al menos 5 películas ya cargadas).
  - `POST api/peliculas`: recibe una película nueva por el body, le asigna un id (por ejemplo, el máximo id actual + 1) y la agrega a la lista.
- Configurar **CORS** en `Program.cs` para permitir el origen de Live Server (`http://127.0.0.1:5500` / `http://localhost:5500`), igual que en el checkpoint final de la teoría (sección 32).
- No hace falta base de datos, ni persistencia en archivo, ni tests: alcanza con la lista en memoria — se reinicia cada vez que se corre `dotnet run`, y está bien que sea así para esta práctica.

---

## Extra 

Para aprobar la práctica **no son obligatorios** — alcanza con el enunciado principal (listar y agregar películas). Pero para mantener la condición de **promoción** hay que entregar los dos:

1. **Filtro por género**: un `<select>` o botones en el `<aside>` que, al cambiar, filtren la grilla con `array.filter(...)` y vuelvan a renderizar solo las películas de ese género.
2. **Buscador por título**: un `<input type="search">` que, con el evento `input`, filtre las tarjetas a medida que se escribe (sin recargar ni enviar ningún formulario).

---

## Notas

Conceptos de la unidad que se ponen en juego (repasá la sección correspondiente del `README.md` de la unidad si te trabás):

- HTML semántico: `header`, `nav`, `main`, `section`, `aside`, `article`, `footer` (sección 9)
- Formularios y validación HTML5 (sección 8)
- CSS: usá lo que te resulte más cómodo de las secciones 10 a 19 (colores, Box Model, Flexbox, Grid, variables, `:hover`, media queries)
- Arrays de objetos, `map`/`filter`/`find` (secciones 26 y 27)
- DOM, `innerHTML`, delegación de eventos (secciones 28 y 29)
- `localStorage` (sección 30)
- Fetch API y `async`/`await` (sección 31)
- CORS (sección 32)
- Web API mínima en .NET: `[ApiController]`, `[HttpGet]`/`[HttpPost]`, `[FromBody]` (sección 33)

> No hace falta guardar nada en disco (ni archivo, ni base de datos): la lista en memoria del controller alcanza. Se reinicia cada vez que se corre `dotnet run`, y para esta práctica está bien que sea así.

### Fecha de entrega y condición

- **Fecha límite: 18/09.**
- Esta práctica **tiene nota** y es de presentación obligatoria para mantener la condición de **alumno regular**.
- Quienes quieran quedar en condición de **promoción** deben entregar también los **2 puntos extra** (filtro por género y buscador por título).
- **Criterio de aprobación**: el sitio tiene que funcionar de punta a punta — se listan las películas, se puede registrar una nueva y esa película se suma al listado en la página. Quienes vayan por promoción, además, tienen que tener funcionando las búsquedas (filtro por género y buscador por título).

---

## Entrega

- Crear un repositorio privado en GitHub con el nombre `Prog1-Practica-CineExpress-TuApellido` (reemplazá `TuApellido` por tu apellido real).
- Agregar a los profesores como colaboradores.
- Organizar el repo en dos carpetas, una para cada proyecto:

```
cineexpress/
├── backend/
│   └── CineExpressApi/    ← proyecto de Web API (único, sin capas)
└── frontend/
    ├── index.html
    ├── estilos.css
    └── app.js
```

- Incluir un `.gitignore` para proyectos .NET (para no subir `bin/` ni `obj/`).
- Para probarlo: `dotnet run` en `CineExpressApi`, y abrir `frontend/index.html` con Live Server. Con ambos corriendo al mismo tiempo, la página tiene que cargar el catálogo desde la API.
- Como mínimo dos commits (por ejemplo: uno con el backend, y otro con el frontend) que reflejen el avance real durante la clase.

---

*Práctica en clase | Unidad 7: Programación Visual | Programación 1 | UCSE*
