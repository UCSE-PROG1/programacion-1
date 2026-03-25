# Unidad 7: Programación Visual - HTML, CSS y JavaScript

En esta unidad aprendemos a construir interfaces web. Veremos cómo funciona **HTML** para estructurar el contenido, **CSS** para darle estilo y **JavaScript** para agregar interactividad. Al final conectaremos el frontend con una API desarrollada en .NET Core.

---

## Sección 1: HTML - HyperText Markup Language

---

### 1. ¿Qué es HTML?

HTML (_HyperText Markup Language_) es el lenguaje estándar para crear páginas web. No es un lenguaje de programación: es un lenguaje de **marcado** que describe la estructura y el contenido de una página mediante **etiquetas** (_tags_).

Conceptos clave:
- **Elemento**: la unidad básica de HTML. Está compuesto por una etiqueta de apertura, contenido y una etiqueta de cierre.
- **Etiqueta**: la instrucción envuelta entre `<` y `>`. Por ejemplo: `<p>`, `<h1>`, `<div>`.
- **Atributo**: información adicional dentro de la etiqueta de apertura. Por ejemplo: `<img src="foto.jpg" alt="Mi foto">`.
- **Documento HTML**: un archivo de texto con extensión `.html` que el navegador interpreta y renderiza visualmente.

El navegador lee el archivo HTML de arriba hacia abajo y construye el **DOM** (Document Object Model), que es la representación interna del documento que luego puede ser manipulada con JavaScript.

```html
<!DOCTYPE html>
<html lang="es">
  <head>
    <meta charset="UTF-8" />
    <meta name="viewport" content="width=device-width, initial-scale=1.0" />
    <title>Mi Primera Página</title>
  </head>
  <body>
    <h1>Hola, mundo</h1>
    <p>Este es mi primer documento HTML.</p>
  </body>
</html>
```

---

### 2. Estructura Básica de HTML

Todo documento HTML válido sigue una estructura fija:

| Etiqueta / Elemento | Función |
|---|---|
| `<!DOCTYPE html>` | Declara que el documento usa HTML5. Siempre va al principio. |
| `<html lang="es">` | Elemento raíz. El atributo `lang` indica el idioma del contenido. |
| `<head>` | Contiene metadatos: charset, viewport, título, enlaces a CSS, etc. No se muestra en pantalla. |
| `<meta charset="UTF-8">` | Indica la codificación de caracteres. UTF-8 soporta tildes, ñ y otros caracteres especiales. |
| `<meta name="viewport" ...>` | Controla el comportamiento en dispositivos móviles. Esencial para diseño responsive. |
| `<title>` | Texto que aparece en la pestaña del navegador. |
| `<link rel="stylesheet" href="...">` | Enlaza un archivo CSS externo. |
| `<body>` | Contiene todo el contenido visible de la página. |

Este es el **template base** que usaremos a lo largo de toda la unidad:

```html
<!DOCTYPE html>
<html lang="es">
  <head>
    <meta charset="UTF-8" />
    <meta name="viewport" content="width=device-width, initial-scale=1.0" />
    <title>Programación Visual - UCSE</title>
    <link rel="stylesheet" href="estilos.css" />
  </head>
  <body>

    <!-- Aquí va el contenido visible -->

    <script src="app.js"></script>
  </body>
</html>
```

> El `<script>` se coloca al final del `<body>` para que el HTML cargue antes de que JavaScript intente manipularlo.

---

### 3. Encabezados y Texto

HTML ofrece seis niveles de encabezados (`<h1>` a `<h6>`) y varias etiquetas para formatear texto. `<h1>` es el más importante y debe usarse una sola vez por página (el título principal).

| Etiqueta | Descripción |
|---|---|
| `<h1>` a `<h6>` | Encabezados, de mayor a menor importancia |
| `<p>` | Párrafo |
| `<strong>` | Texto en **negrita** (semánticamente importante) |
| `<em>` | Texto en _cursiva_ (énfasis) |
| `<u>` | Texto subrayado |
| `<br>` | Salto de línea (etiqueta vacía, sin cierre) |
| `<blockquote>` | Cita larga en bloque, con sangría |
| `<q>` | Cita corta en línea |

```html
<!DOCTYPE html>
<html lang="es">
  <head>
    <meta charset="UTF-8" />
    <title>Encabezados y Texto</title>
  </head>
  <body>
    <h1>Encabezado nivel 1 - Título principal</h1>
    <h2>Encabezado nivel 2 - Sección</h2>
    <h3>Encabezado nivel 3 - Subsección</h3>
    <h4>Encabezado nivel 4</h4>
    <h5>Encabezado nivel 5</h5>
    <h6>Encabezado nivel 6 - El más pequeño</h6>

    <p>
      Este es un párrafo normal. Puede contener texto
      <strong>en negrita</strong>, texto <em>en cursiva</em>,
      texto <u>subrayado</u> y también<br />saltos de línea.
    </p>

    <blockquote>
      "La simplicidad es la máxima sofisticación." — Leonardo da Vinci
    </blockquote>

    <p>
      Como dijo Einstein: <q>La imaginación es más importante que el conocimiento.</q>
    </p>
  </body>
</html>
```

---

### 4. Listas

HTML soporta tres tipos de listas:

- **`<ul>`** (_unordered list_): lista desordenada, con viñetas.
- **`<ol>`** (_ordered list_): lista ordenada, con números o letras.
- **`<dl>`** (_definition list_): lista de definiciones, con término (`<dt>`) y descripción (`<dd>`).

Todos los ítems de `<ul>` y `<ol>` se marcan con `<li>`. Las listas pueden anidarse dentro de otras listas.

```html
<!DOCTYPE html>
<html lang="es">
  <head>
    <meta charset="UTF-8" />
    <title>Listas en HTML</title>
  </head>
  <body>

    <!-- Lista de compras (desordenada) -->
    <h2>Lista de compras</h2>
    <ul>
      <li>Frutas
        <ul>
          <li>Manzanas</li>
          <li>Bananas</li>
          <li>Naranjas</li>
        </ul>
      </li>
      <li>Lácteos
        <ul>
          <li>Leche</li>
          <li>Yogur</li>
        </ul>
      </li>
      <li>Pan</li>
      <li>Huevos</li>
    </ul>

    <!-- Receta paso a paso (ordenada) -->
    <h2>Receta: Tortilla de papas</h2>
    <ol>
      <li>Pelar y cortar las papas en rodajas finas.</li>
      <li>Calentar aceite en una sartén a fuego medio.</li>
      <li>Freír las papas hasta que estén tiernas (no doradas).</li>
      <li>Batir los huevos en un bowl y salpimentar.</li>
      <li>Mezclar las papas con los huevos batidos.</li>
      <li>Verter la mezcla en la sartén y cocinar a fuego bajo.</li>
      <li>Dar vuelta con un plato y cocinar el otro lado.</li>
      <li>Servir caliente o a temperatura ambiente.</li>
    </ol>

    <!-- Lista de definiciones -->
    <h2>Glosario web</h2>
    <dl>
      <dt>HTML</dt>
      <dd>Lenguaje de marcado para estructurar el contenido de una página web.</dd>
      <dt>CSS</dt>
      <dd>Lenguaje de estilos para dar formato y presentación a documentos HTML.</dd>
      <dt>JavaScript</dt>
      <dd>Lenguaje de programación que permite agregar interactividad a las páginas web.</dd>
    </dl>

  </body>
</html>
```

---

### 5. Imágenes y Multimedia

HTML permite incrustar distintos tipos de medios en una página.

| Elemento | Descripción |
|---|---|
| `<img>` | Muestra una imagen. Etiqueta vacía (sin cierre). |
| `<video>` | Reproduce un video. Admite múltiples `<source>`. |
| `<audio>` | Reproduce audio. Admite múltiples `<source>`. |
| `<iframe>` | Incrusta contenido externo (Google Maps, YouTube, etc.). |

El atributo `alt` en `<img>` es **obligatorio**: describe la imagen para lectores de pantalla y se muestra si la imagen no carga. Siempre hay que escribirlo con una descripción significativa.

```html
<!DOCTYPE html>
<html lang="es">
  <head>
    <meta charset="UTF-8" />
    <title>Imágenes y Multimedia</title>
  </head>
  <body>

    <!-- Imagen con todos los atributos recomendados -->
    <img
      src="imagenes/paisaje.jpg"
      alt="Fotografía de un lago rodeado de montañas al atardecer"
      width="800"
      height="450"
    />

    <!-- Video con controles -->
    <video width="640" height="360" controls>
      <source src="videos/intro.mp4" type="video/mp4" />
      <source src="videos/intro.webm" type="video/webm" />
      Tu navegador no soporta el elemento de video.
    </video>

    <!-- Audio con controles -->
    <audio controls>
      <source src="audios/musica.mp3" type="audio/mpeg" />
      Tu navegador no soporta el elemento de audio.
    </audio>

    <!-- Iframe para incrustar un mapa de Google -->
    <iframe
      src="https://www.google.com/maps/embed?pb=..."
      width="600"
      height="450"
      style="border:0;"
      allowfullscreen=""
      loading="lazy"
      title="Mapa de ubicación"
    ></iframe>

  </body>
</html>
```

---

### 6. Enlaces

El elemento `<a>` (_anchor_) crea hipervínculos. El atributo `href` (_Hypertext REFerence_) indica el destino.

| Tipo de enlace | Ejemplo de `href` |
|---|---|
| URL absoluta (externo) | `https://www.ejemplo.com` |
| URL relativa (interno) | `contacto.html` o `../index.html` |
| Ancla en la misma página | `#seccion-2` |
| Correo electrónico | `mailto:info@ejemplo.com` |
| Teléfono | `tel:+5491112345678` |

El atributo `target="_blank"` abre el enlace en una nueva pestaña. Siempre combinar con `rel="noopener noreferrer"` por seguridad.

```html
<!DOCTYPE html>
<html lang="es">
  <head>
    <meta charset="UTF-8" />
    <title>Enlaces en HTML</title>
  </head>
  <body>

    <!-- Menú de navegación interna -->
    <nav>
      <a href="#inicio">Inicio</a> |
      <a href="#nosotros">Nosotros</a> |
      <a href="#servicios">Servicios</a> |
      <a href="#contacto">Contacto</a>
    </nav>

    <!-- Enlace a sitio externo, se abre en nueva pestaña -->
    <p>
      Visita la
      <a href="https://developer.mozilla.org/es/" target="_blank" rel="noopener noreferrer">
        documentación de MDN
      </a>
      para más información.
    </p>

    <!-- Secciones con id para las anclas -->
    <section id="inicio">
      <h2>Inicio</h2>
      <p>Bienvenidos a nuestra página.</p>
    </section>

    <section id="nosotros">
      <h2>Nosotros</h2>
      <p>Somos un equipo de desarrolladores.</p>
    </section>

    <section id="servicios">
      <h2>Servicios</h2>
      <p>Desarrollo web, aplicaciones móviles y más.</p>
    </section>

    <section id="contacto">
      <h2>Contacto</h2>
      <p>
        Escribinos a
        <a href="mailto:info@empresa.com">info@empresa.com</a>
        o llamanos al
        <a href="tel:+5493415001000">+54 341 500-1000</a>.
      </p>
    </section>

  </body>
</html>
```

---

### 7. Tablas

Las tablas se usan para mostrar datos tabulares (filas y columnas). **No** deben usarse para maquetar la página (eso es tarea del CSS).

| Etiqueta | Descripción |
|---|---|
| `<table>` | Contenedor de la tabla |
| `<thead>` | Grupo de filas de encabezado |
| `<tbody>` | Grupo de filas de datos |
| `<tfoot>` | Grupo de filas de pie |
| `<tr>` | Fila (_table row_) |
| `<th>` | Celda de encabezado (_table header_), en negrita y centrada por defecto |
| `<td>` | Celda de datos (_table data_) |
| `colspan` | Une varias columnas en una sola celda |
| `rowspan` | Une varias filas en una sola celda |

```html
<!DOCTYPE html>
<html lang="es">
  <head>
    <meta charset="UTF-8" />
    <title>Tabla de Calificaciones</title>
    <style>
      table { border-collapse: collapse; width: 100%; }
      th, td { border: 1px solid #ccc; padding: 8px 12px; text-align: center; }
      thead { background-color: #2c3e50; color: white; }
      tfoot { background-color: #ecf0f1; font-weight: bold; }
      tr:nth-child(even) { background-color: #f9f9f9; }
    </style>
  </head>
  <body>

    <h2>Calificaciones - Programación I (2025)</h2>

    <table>
      <thead>
        <tr>
          <th rowspan="2">Alumno</th>
          <th colspan="3">Parciales</th>
          <th rowspan="2">Promedio</th>
        </tr>
        <tr>
          <th>Parcial 1</th>
          <th>Parcial 2</th>
          <th>Recuperatorio</th>
        </tr>
      </thead>
      <tbody>
        <tr>
          <td>García, Lucía</td>
          <td>8</td>
          <td>9</td>
          <td>-</td>
          <td>8.5</td>
        </tr>
        <tr>
          <td>Martínez, Juan</td>
          <td>6</td>
          <td>5</td>
          <td>7</td>
          <td>6.5</td>
        </tr>
        <tr>
          <td>Rodríguez, Ana</td>
          <td>10</td>
          <td>9</td>
          <td>-</td>
          <td>9.5</td>
        </tr>
        <tr>
          <td>López, Carlos</td>
          <td>4</td>
          <td>6</td>
          <td>8</td>
          <td>7</td>
        </tr>
      </tbody>
      <tfoot>
        <tr>
          <td>Promedio del curso</td>
          <td>7</td>
          <td>7.25</td>
          <td>-</td>
          <td>7.875</td>
        </tr>
      </tfoot>
    </table>

  </body>
</html>
```

---

### 8. Formularios

Los formularios permiten al usuario enviar datos al servidor. El elemento `<form>` agrupa todos los controles.

- `action`: URL a la que se envían los datos. Si se omite, se envía a la misma página.
- `method`: `GET` (datos en la URL) o `POST` (datos en el cuerpo, más seguro para contraseñas).

Siempre vincular cada `<label>` a su `<input>` con el atributo `for` (que debe coincidir con el `id` del input). Esto mejora la accesibilidad y la usabilidad.

```html
<!DOCTYPE html>
<html lang="es">
  <head>
    <meta charset="UTF-8" />
    <title>Formularios</title>
    <style>
      form { max-width: 400px; margin: 20px auto; }
      label { display: block; margin-top: 12px; font-weight: bold; }
      input, select, textarea { width: 100%; padding: 8px; margin-top: 4px; box-sizing: border-box; }
      button { margin-top: 16px; padding: 10px 24px; background: #2c3e50; color: white; border: none; cursor: pointer; }
    </style>
  </head>
  <body>

    <!-- Formulario de inicio de sesión -->
    <h2>Iniciar Sesión</h2>
    <form action="/api/login" method="POST">
      <label for="email">Correo electrónico:</label>
      <input type="email" id="email" name="email" placeholder="usuario@ejemplo.com" required />

      <label for="password">Contraseña:</label>
      <input type="password" id="password" name="password" placeholder="••••••••" required />

      <button type="submit">Ingresar</button>
    </form>

    <hr />

    <!-- Formulario de registro -->
    <h2>Registro de Usuario</h2>
    <form action="/api/registro" method="POST">
      <label for="nombre">Nombre completo:</label>
      <input type="text" id="nombre" name="nombre" placeholder="Juan Pérez" required />

      <label for="email-reg">Correo electrónico:</label>
      <input type="email" id="email-reg" name="email" required />

      <label for="edad">Edad:</label>
      <input type="number" id="edad" name="edad" min="18" max="100" />

      <label for="carrera">Carrera:</label>
      <select id="carrera" name="carrera">
        <option value="">-- Seleccionar --</option>
        <option value="sistemas">Ingeniería en Sistemas</option>
        <option value="informatica">Licenciatura en Informática</option>
        <option value="redes">Tecnicatura en Redes</option>
      </select>

      <label>Género:</label>
      <label><input type="radio" name="genero" value="M" /> Masculino</label>
      <label><input type="radio" name="genero" value="F" /> Femenino</label>
      <label><input type="radio" name="genero" value="otro" /> Otro / Prefiero no decir</label>

      <label for="comentarios">Comentarios adicionales:</label>
      <textarea id="comentarios" name="comentarios" rows="4" placeholder="Escribe aquí..."></textarea>

      <label>
        <input type="checkbox" name="terminos" required />
        Acepto los términos y condiciones
      </label>

      <button type="submit">Registrarse</button>
    </form>

  </body>
</html>
```

---

### 9. Div y Semántica

El elemento `<div>` es un contenedor genérico sin significado semántico. Es útil para agrupar elementos con fines de estilo o scripting.

HTML5 introdujo **elementos semánticos** que reemplazan el uso de múltiples `<div>` cuando el contenido tiene un rol específico:

| Elemento | Descripción |
|---|---|
| `<header>` | Encabezado de la página o de una sección |
| `<nav>` | Bloque de navegación (menús, links) |
| `<main>` | Contenido principal único de la página |
| `<section>` | Sección temática del contenido |
| `<article>` | Contenido independiente y autocontenido (un post, una noticia) |
| `<aside>` | Contenido relacionado pero secundario (barra lateral) |
| `<footer>` | Pie de página o de sección |

Usar semántica correcta mejora la **accesibilidad** (lectores de pantalla), el **SEO** (motores de búsqueda entienden mejor el contenido) y la **mantenibilidad** del código.

```html
<!DOCTYPE html>
<html lang="es">
  <head>
    <meta charset="UTF-8" />
    <title>Layout Semántico</title>
  </head>
  <body>

    <header>
      <h1>Blog de Tecnología</h1>
      <nav>
        <a href="/">Inicio</a>
        <a href="/articulos">Artículos</a>
        <a href="/acerca">Acerca de</a>
      </nav>
    </header>

    <main>
      <section>
        <h2>Artículos recientes</h2>

        <article>
          <h3>Introducción a HTML5</h3>
          <p>Publicado el 20 de marzo de 2025</p>
          <p>HTML5 trajo nuevas etiquetas semánticas que facilitan...</p>
        </article>

        <article>
          <h3>CSS Grid vs Flexbox</h3>
          <p>Publicado el 15 de marzo de 2025</p>
          <p>Ambas técnicas de layout tienen sus casos de uso...</p>
        </article>
      </section>

      <aside>
        <h3>Etiquetas populares</h3>
        <ul>
          <li>HTML</li>
          <li>CSS</li>
          <li>JavaScript</li>
          <li>.NET</li>
        </ul>
      </aside>
    </main>

    <footer>
      <p>&copy; 2025 Blog de Tecnología - UCSE</p>
    </footer>

  </body>
</html>
```

---

## Sección 2: CSS - Cascading Style Sheets

---

### 10. ¿Qué es CSS?

CSS (_Cascading Style Sheets_) es el lenguaje que controla la presentación visual de los documentos HTML: colores, tipografías, tamaños, márgenes, disposición de los elementos, animaciones, etc.

Hay tres formas de agregar CSS a un documento HTML:

**1. Inline** (en línea): directamente en el atributo `style` del elemento.
```html
<p style="color: red; font-size: 18px;">Texto rojo</p>
```
Desventaja: mezcla estructura y presentación, difícil de mantener.

**2. Internal** (interno): dentro de una etiqueta `<style>` en el `<head>`.
```html
<style>
  p { color: red; font-size: 18px; }
</style>
```
Útil para páginas de una sola página, pero no reutilizable.

**3. External** (externo): en un archivo `.css` separado, enlazado desde el HTML.
```html
<link rel="stylesheet" href="estilos.css" />
```
Es el método **recomendado**: separa responsabilidades, permite reutilizar estilos en múltiples páginas y es más fácil de mantener.

La sintaxis básica de una regla CSS:

```css
/* selector { propiedad: valor; } */

p {
  color: #333333;
  font-size: 16px;
  line-height: 1.6;
}
```

---

### 11. Selectores

Los selectores determinan a qué elementos HTML se aplican los estilos.

```css
/* ── Selector de elemento ── aplica a todos los <p> */
p {
  color: #333;
}

/* ── Selector de clase ── aplica a cualquier elemento con class="destacado" */
.destacado {
  background-color: yellow;
  font-weight: bold;
}

/* ── Selector de ID ── aplica al elemento con id="titulo-principal" (único por página) */
#titulo-principal {
  font-size: 2rem;
  color: #2c3e50;
}

/* ── Selector descendiente ── aplica a <p> que estén dentro de un <div> */
div p {
  margin-left: 16px;
}

/* ── Selector hijo directo ── aplica a <p> que sean hijos directos de <div> */
div > p {
  border-left: 3px solid #3498db;
}

/* ── Selectores múltiples ── aplica el mismo estilo a h1, h2 y h3 */
h1, h2, h3 {
  font-family: 'Arial', sans-serif;
  color: #2c3e50;
}

/* ── Selector de atributo ── aplica a inputs de tipo text */
input[type="text"] {
  border: 1px solid #ccc;
  border-radius: 4px;
}
```

Aplicación en HTML:

```html
<h1 id="titulo-principal">Título principal</h1>

<div>
  <p class="destacado">Este párrafo tiene clase "destacado".</p>
  <p>Este párrafo es hijo directo del div.</p>
  <section>
    <p>Este párrafo es descendiente del div pero no hijo directo.</p>
  </section>
</div>
```

---

### 12. Colores y Tipografía

CSS ofrece varias formas de especificar colores y numerosas propiedades para controlar la tipografía.

**Formas de definir colores:**
- Nombre: `red`, `blue`, `white`, `black`, etc.
- Hexadecimal: `#FF5733` (rojo, verde, azul en base 16).
- RGB: `rgb(255, 87, 51)`.
- RGBA: `rgba(255, 87, 51, 0.8)` (el cuarto valor es la opacidad, de 0 a 1).

```css
/* archivo: estilos.css */

body {
  font-family: 'Segoe UI', Arial, sans-serif; /* fuente con fallbacks */
  font-size: 16px;                            /* tamaño base */
  color: #333333;                             /* color del texto */
  background-color: #f5f5f5;                  /* fondo de la página */
  line-height: 1.6;                           /* altura de línea (interlineado) */
}

h2 {
  color: #2c3e50;
  font-size: 1.8rem;
  font-weight: 700;          /* bold */
  text-align: center;
  text-decoration: underline;
}

.parrafo-ejemplo {
  color: rgb(50, 50, 50);
  background-color: rgba(52, 152, 219, 0.1); /* azul muy transparente */
  font-size: 1rem;
  font-style: italic;        /* cursiva */
  font-weight: 400;          /* normal */
  text-align: justify;
  text-decoration: none;
  line-height: 1.8;
}
```

```html
<h2>Tipografía en CSS</h2>
<p class="parrafo-ejemplo">
  Este párrafo demuestra el uso de colores y propiedades tipográficas en CSS.
  Podemos controlar la fuente, el tamaño, el color, el estilo y mucho más.
</p>
```

---

### 13. El Modelo de Caja (Box Model)

En CSS, **todo elemento es una caja**. El modelo de caja describe cómo se calcula el tamaño total de un elemento:

```
┌─────────────────────────────────────────┐
│               MARGIN                    │  ← espacio exterior (transparente)
│  ┌───────────────────────────────────┐  │
│  │             BORDER                │  │  ← borde (tiene grosor y color)
│  │  ┌─────────────────────────────┐  │  │
│  │  │           PADDING           │  │  │  ← espacio interior (toma el color de fondo)
│  │  │  ┌───────────────────────┐  │  │  │
│  │  │  │       CONTENT         │  │  │  │  ← texto, imagen, etc.
│  │  │  └───────────────────────┘  │  │  │
│  │  └─────────────────────────────┘  │  │
│  └───────────────────────────────────┘  │
└─────────────────────────────────────────┘
```

Por defecto, `width` y `height` solo afectan al **content**. Con `box-sizing: border-box`, el `width` incluye padding y border, lo que es mucho más intuitivo. Se recomienda aplicarlo a todos los elementos:

```css
/* Buena práctica: aplicar box-sizing globalmente */
*, *::before, *::after {
  box-sizing: border-box;
}

/* Caja con mucho margen exterior */
.caja-margen {
  width: 300px;
  height: 100px;
  background-color: #3498db;
  color: white;
  margin: 40px;     /* 40px en los 4 lados */
  padding: 10px;
  border: 2px solid #2980b9;
}

/* Caja con mucho padding interior */
.caja-padding {
  width: 300px;
  background-color: #2ecc71;
  color: white;
  margin: 10px;
  padding: 40px;    /* 40px de espacio interior en los 4 lados */
  border: 2px solid #27ae60;
}

/* Margen individual por lado */
.caja-asimetrica {
  margin-top: 20px;
  margin-right: 10px;
  margin-bottom: 20px;
  margin-left: 10px;
  /* shorthand equivalente: margin: 20px 10px; */

  padding-top: 15px;
  padding-right: 30px;
  padding-bottom: 15px;
  padding-left: 30px;
  /* shorthand equivalente: padding: 15px 30px; */
}
```

```html
<div class="caja-margen">Caja con margen grande</div>
<div class="caja-padding">Caja con padding grande</div>
```

---

### 14. Display y Posicionamiento

La propiedad `display` controla cómo se comporta un elemento en el flujo del documento.

| Valor | Comportamiento |
|---|---|
| `block` | Ocupa todo el ancho disponible. Genera salto de línea antes y después. (`<div>`, `<p>`, `<h1>`) |
| `inline` | Solo ocupa lo necesario. No genera salto. No acepta `width`/`height`. (`<span>`, `<a>`, `<strong>`) |
| `inline-block` | Como inline pero acepta `width`, `height`, `margin` y `padding` verticales. |
| `flex` | Activa Flexbox para el elemento y sus hijos directos. |
| `none` | Oculta el elemento (no ocupa espacio). |

**Flexbox** es el sistema de layout más usado para distribuir elementos en una dimensión (fila o columna):

```css
/* Navbar con Flexbox */
.navbar {
  display: flex;
  flex-direction: row;        /* los hijos se disponen en fila (por defecto) */
  justify-content: space-between; /* distribución horizontal */
  align-items: center;        /* alineación vertical */
  background-color: #2c3e50;
  padding: 0 24px;
  height: 60px;
}

.navbar .logo {
  color: white;
  font-size: 1.4rem;
  font-weight: bold;
  text-decoration: none;
}

.navbar .menu {
  display: flex;
  gap: 24px;                  /* espacio entre hijos */
  list-style: none;
  margin: 0;
  padding: 0;
}

.navbar .menu a {
  color: white;
  text-decoration: none;
  font-size: 0.95rem;
}
```

```html
<nav class="navbar">
  <a href="/" class="logo">MiSitio</a>
  <ul class="menu">
    <li><a href="/inicio">Inicio</a></li>
    <li><a href="/cursos">Cursos</a></li>
    <li><a href="/contacto">Contacto</a></li>
  </ul>
</nav>
```

---

### 15. Unidades de Medida

CSS ofrece unidades absolutas y relativas:

| Unidad | Tipo | Relativa a... | Uso recomendado |
|---|---|---|---|
| `px` | Absoluta | - | Bordes, sombras, valores fijos pequeños |
| `%` | Relativa | El contenedor padre | Anchos fluidos |
| `em` | Relativa | El `font-size` del elemento actual | Espaciado relacionado a la fuente |
| `rem` | Relativa | El `font-size` del elemento raíz (`html`) | Tipografía y espaciado consistente |
| `vh` | Relativa | La altura del viewport (ventana) | Secciones de pantalla completa |
| `vw` | Relativa | El ancho del viewport | Elementos que ocupan % de la pantalla |

La ventaja de `rem` es que si el usuario cambia el tamaño de fuente en su navegador, toda la tipografía escala proporcionalmente.

```css
/* Tamaño base: 1rem = 16px (por defecto del navegador) */
html {
  font-size: 16px;
}

h1 { font-size: 2.5rem; }    /* 40px */
h2 { font-size: 2rem; }      /* 32px */
h3 { font-size: 1.5rem; }    /* 24px */
p  { font-size: 1rem; }      /* 16px */
small { font-size: 0.875rem; } /* 14px */

/* Sección que ocupa toda la pantalla */
.hero {
  height: 100vh;
  width: 100%;
  display: flex;
  align-items: center;
  justify-content: center;
}

/* Contenedor con ancho máximo y márgenes automáticos para centrar */
.contenedor {
  max-width: 1200px;
  width: 90%;          /* 90% del viewport, nunca supera 1200px */
  margin: 0 auto;
  padding: 0 1rem;
}
```

---

### 16. Pseudoclases y Pseudoelementos

Las **pseudoclases** aplican estilos según el estado del elemento (`:hover`, `:focus`) o su posición en el DOM (`:first-child`, `:nth-child`).

Los **pseudoelementos** permiten estilizar partes específicas de un elemento (`::before`, `::after`, `::first-line`).

```css
/* ── Pseudoclases de estado ── */
a:hover {
  color: #3498db;
  text-decoration: underline;
}

input:focus {
  outline: 2px solid #3498db;
  border-color: #3498db;
}

button:active {
  transform: scale(0.97); /* se "hunde" al hacer clic */
}

/* ── Pseudoclases estructurales ── */
li:first-child { font-weight: bold; }
li:last-child  { color: gray; }
tr:nth-child(even) { background-color: #f2f2f2; } /* filas pares de tabla */

/* ── Botón con efecto hover ── */
.btn {
  display: inline-block;
  padding: 10px 24px;
  background-color: #2c3e50;
  color: white;
  border: none;
  border-radius: 4px;
  cursor: pointer;
  font-size: 1rem;
  transition: background-color 0.2s ease, transform 0.1s ease; /* animación suave */
}

.btn:hover {
  background-color: #3498db;
  transform: translateY(-2px); /* sube 2px al pasar el mouse */
}

.btn:active {
  transform: translateY(0);    /* vuelve a su posición al hacer clic */
}

/* ── Pseudoelementos ── */
/* Agrega comillas decorativas antes y después de una cita */
blockquote::before {
  content: '"';
  font-size: 3rem;
  color: #ccc;
  line-height: 0;
  vertical-align: -0.6em;
}

/* Punto decorativo antes de cada ítem de una lista personalizada */
.lista-custom li::before {
  content: '▸ ';
  color: #3498db;
}
```

```html
<button class="btn">Hacer clic aquí</button>

<ul class="lista-custom">
  <li>Primer elemento</li>
  <li>Segundo elemento</li>
  <li>Tercer elemento</li>
</ul>
```

---

### 17. Responsive Design

El diseño responsive permite que una misma página se vea bien en pantallas de distintos tamaños: computadoras de escritorio, tablets y celulares.

Las **media queries** aplican estilos solo cuando se cumplen ciertas condiciones (ancho de pantalla, orientación, etc.).

Estrategia recomendada: **Mobile First**. Escribir primero los estilos para pantallas pequeñas y luego agregar overrides para pantallas más grandes con `min-width`.

```css
/* ── Estilos base (mobile first: para pantallas pequeñas) ── */
.tarjetas {
  display: flex;
  flex-direction: column; /* apiladas verticalmente en móvil */
  gap: 16px;
  padding: 16px;
}

.tarjeta {
  background: white;
  border-radius: 8px;
  padding: 20px;
  box-shadow: 0 2px 8px rgba(0,0,0,0.1);
  width: 100%; /* ocupa todo el ancho en móvil */
}

.tarjeta h3 {
  font-size: 1.2rem;
}

/* ── Tablet: pantallas desde 768px ── */
@media (min-width: 768px) {
  .tarjetas {
    flex-direction: row;  /* en fila a partir de 768px */
    flex-wrap: wrap;
  }

  .tarjeta {
    width: calc(50% - 8px); /* dos columnas */
  }
}

/* ── Desktop: pantallas desde 1024px ── */
@media (min-width: 1024px) {
  .tarjetas {
    max-width: 1200px;
    margin: 0 auto;
  }

  .tarjeta {
    width: calc(33.333% - 12px); /* tres columnas */
  }

  .tarjeta h3 {
    font-size: 1.4rem;
  }
}
```

```html
<div class="tarjetas">
  <div class="tarjeta">
    <h3>HTML</h3>
    <p>Estructura el contenido de la página web.</p>
  </div>
  <div class="tarjeta">
    <h3>CSS</h3>
    <p>Controla la presentación visual del contenido.</p>
  </div>
  <div class="tarjeta">
    <h3>JavaScript</h3>
    <p>Agrega interactividad y lógica al frontend.</p>
  </div>
</div>
```

---

## Sección 3: JavaScript

---

### 18. ¿Qué es JavaScript?

JavaScript (JS) es el lenguaje de programación de la web. Se ejecuta directamente en el **navegador** del usuario (lado del cliente), lo que permite crear páginas dinámicas e interactivas sin necesidad de comunicarse con el servidor para cada acción.

Usos principales:
- Responder a eventos del usuario (clics, teclado, formularios).
- Modificar el contenido y estilo de la página dinámicamente.
- Realizar peticiones a APIs sin recargar la página (AJAX, Fetch).
- Validar formularios antes de enviarlos.

Formas de agregar JavaScript a una página HTML:

**Inline** (no recomendado):
```html
<button onclick="alert('Hola!')">Clic</button>
```

**Bloque `<script>` interno** (útil para pruebas rápidas):
```html
<script>
  console.log("Script interno cargado");
</script>
```

**Archivo externo** (recomendado):
```html
<script src="app.js"></script>
```

```javascript
// app.js

// Muestra un cuadro de diálogo en el navegador
alert("¡Hola, mundo!");

// Imprime en la consola del navegador (F12 → Consola)
console.log("Hola desde la consola");
console.log("La suma de 2 + 3 es:", 2 + 3);

// También se puede usar para depuración:
console.error("Esto es un error");
console.warn("Esto es una advertencia");
```

---

### 19. Variables y Tipos de Datos

JavaScript es un lenguaje de **tipado dinámico**: una variable puede cambiar de tipo durante la ejecución.

- `var`: forma antigua de declarar variables. Tiene problemas de alcance (_scope_). **Evitar su uso.**
- `let`: declara una variable que puede cambiar de valor.
- `const`: declara una constante. No puede reasignarse (pero los objetos y arrays declarados con `const` sí pueden modificarse internamente).

```javascript
// ── Números ──
let edad = 25;
const PI = 3.14159;
let precio = 99.90;

// ── Cadenas de texto (string) ──
let nombre = "María";
let saludo = 'Hola, mundo';
let mensaje = `Hola, ${nombre}! Tenés ${edad} años.`; // template literal

// ── Booleanos ──
let estaActivo = true;
let esMayorDeEdad = false;

// ── Nulo e indefinido ──
let valorNulo = null;        // asignado explícitamente: "sin valor"
let valorIndefinido;         // declarada pero no asignada → undefined

// ── Arrays (listas) ──
let frutas = ["manzana", "banana", "naranja"];
let numeros = [1, 2, 3, 4, 5];

// ── Objetos ──
let persona = {
  nombre: "Carlos",
  edad: 30,
  activo: true
};

// ── Operador typeof ──
console.log(typeof edad);           // "number"
console.log(typeof nombre);         // "string"
console.log(typeof estaActivo);     // "boolean"
console.log(typeof valorNulo);      // "object" (comportamiento histórico de JS)
console.log(typeof valorIndefinido); // "undefined"
console.log(typeof frutas);         // "object"
console.log(typeof persona);        // "object"
```

---

### 20. Operadores

```javascript
// ── Operadores aritméticos ──
let a = 10, b = 3;

console.log(a + b);   // 13 (suma)
console.log(a - b);   // 7  (resta)
console.log(a * b);   // 30 (multiplicación)
console.log(a / b);   // 3.333... (división)
console.log(a % b);   // 1  (módulo: resto de la división entera)
console.log(a ** b);  // 1000 (potenciación: 10³)

// ── Operadores de comparación ──
console.log(5 == "5");   // true  → == compara VALOR (con conversión de tipo)
console.log(5 === "5");  // false → === compara VALOR Y TIPO (sin conversión)
console.log(5 != "5");   // false
console.log(5 !== "5");  // true
console.log(10 > 3);     // true
console.log(10 < 3);     // false
console.log(10 >= 10);   // true
console.log(10 <= 9);    // false

// ── Por qué === es mejor que == ──
console.log(0 == false);    // true  (¡sorpresa! == convierte tipos)
console.log(0 === false);   // false (correcto: 0 es number, false es boolean)
console.log("" == false);   // true  (otro caso problemático)
console.log("" === false);  // false (correcto)
console.log(null == undefined);  // true  (con ==)
console.log(null === undefined); // false (con ===)

// Regla práctica: SIEMPRE usar === y !== en lugar de == y !=

// ── Operadores lógicos ──
let x = true, y = false;

console.log(x && y);  // false (AND: ambos deben ser true)
console.log(x || y);  // true  (OR:  al menos uno debe ser true)
console.log(!x);      // false (NOT: invierte el booleano)
console.log(!y);      // true
```

---

### 21. Condicionales

```javascript
// ── if / else if / else ──
function clasificarNota(nota) {
  if (nota >= 9) {
    return "Sobresaliente";
  } else if (nota >= 7) {
    return "Aprobado";
  } else if (nota >= 4) {
    return "Aprobado por recuperatorio";
  } else {
    return "Reprobado";
  }
}

console.log(clasificarNota(10));  // "Sobresaliente"
console.log(clasificarNota(7.5)); // "Aprobado"
console.log(clasificarNota(5));   // "Aprobado por recuperatorio"
console.log(clasificarNota(3));   // "Reprobado"

// ── Operador ternario ──
// condicion ? valorSiVerdadero : valorSiFalso
let edad = 20;
let estado = edad >= 18 ? "Mayor de edad" : "Menor de edad";
console.log(estado); // "Mayor de edad"

// ── switch ──
let dia = new Date().getDay(); // 0=Domingo, 1=Lunes, ..., 6=Sábado

switch (dia) {
  case 0:
  case 6:
    console.log("Es fin de semana");
    break;
  case 1:
    console.log("Es lunes");
    break;
  case 5:
    console.log("Es viernes");
    break;
  default:
    console.log("Es un día de semana");
}

// ── FizzBuzz clásico ──
for (let i = 1; i <= 20; i++) {
  if (i % 15 === 0) {
    console.log("FizzBuzz");
  } else if (i % 3 === 0) {
    console.log("Fizz");
  } else if (i % 5 === 0) {
    console.log("Buzz");
  } else {
    console.log(i);
  }
}
```

---

### 22. Bucles

```javascript
const nombres = ["Ana", "Carlos", "María", "Juan", "Lucía"];

// ── for clásico ──
console.log("--- for clásico ---");
for (let i = 0; i < nombres.length; i++) {
  console.log(`${i + 1}. ${nombres[i]}`);
}

// ── for...of (recorre los valores del array) ──
console.log("--- for...of ---");
for (const nombre of nombres) {
  console.log(`Hola, ${nombre}!`);
}

// ── forEach (método del array, recibe una función) ──
console.log("--- forEach ---");
nombres.forEach((nombre, indice) => {
  console.log(`Posición ${indice}: ${nombre}`);
});

// ── while ──
console.log("--- while ---");
let contador = 0;
while (contador < 3) {
  console.log(`Iteración ${contador + 1}`);
  contador++;
}

// ── do...while (ejecuta al menos una vez) ──
console.log("--- do...while ---");
let intentos = 0;
do {
  console.log(`Intento número ${intentos + 1}`);
  intentos++;
} while (intentos < 3);

// ── break y continue ──
console.log("--- break y continue ---");
for (let i = 0; i < nombres.length; i++) {
  if (nombres[i] === "María") continue; // saltar a María
  if (nombres[i] === "Juan")  break;    // detener en Juan
  console.log(nombres[i]);
}
// Imprime: Ana, Carlos
```

---

### 23. Funciones

```javascript
// ── Declaración de función (function declaration) ──
// Puede llamarse antes de su definición (hoisting)
function saludar(nombre) {
  return `Hola, ${nombre}!`;
}
console.log(saludar("Ana")); // "Hola, Ana!"

// ── Expresión de función (function expression) ──
// No tiene hoisting
const despedir = function(nombre) {
  return `Hasta luego, ${nombre}.`;
};
console.log(despedir("Carlos")); // "Hasta luego, Carlos."

// ── Arrow function (función flecha) ──
// Sintaxis más concisa, muy usada en callbacks y métodos de arrays
const sumar = (a, b) => a + b;
console.log(sumar(3, 5)); // 8

// Si solo tiene un parámetro, los paréntesis son opcionales:
const duplicar = n => n * 2;
console.log(duplicar(7)); // 14

// Si el cuerpo tiene más de una línea, necesita llaves y return explícito:
const calcularIMC = (peso, altura) => {
  const imc = peso / (altura * altura);
  return imc.toFixed(2); // redondea a 2 decimales
};

// ── Función para calcular el área de un círculo (como arrow function) ──
const areaCirculo = (radio) => {
  const area = Math.PI * radio ** 2;
  return area;
};

console.log(areaCirculo(5));   // 78.53981633974483
console.log(areaCirculo(10));  // 314.1592653589793

// Versión compacta en una línea:
const areaCirculoCompacta = (radio) => Math.PI * radio ** 2;
```

---

### 24. Arrays en JavaScript

```javascript
// ── Declaración y acceso ──
let tareas = ["Comprar leche", "Estudiar CSS", "Llamar al médico"];

console.log(tareas[0]);          // "Comprar leche"
console.log(tareas.length);      // 3
console.log(tareas[tareas.length - 1]); // "Llamar al médico" (último elemento)

// ── Agregar y quitar elementos ──
tareas.push("Ir al gimnasio");          // agrega al final
tareas.unshift("Revisar emails");       // agrega al principio
console.log(tareas);
// ["Revisar emails", "Comprar leche", "Estudiar CSS", "Llamar al médico", "Ir al gimnasio"]

tareas.pop();                           // quita el último elemento
tareas.shift();                         // quita el primer elemento
console.log(tareas);
// ["Comprar leche", "Estudiar CSS", "Llamar al médico"]

// ── Gestionar lista de TODO ──
let todoList = [
  { id: 1, texto: "Aprender HTML", completada: true },
  { id: 2, texto: "Aprender CSS",  completada: true },
  { id: 3, texto: "Aprender JS",   completada: false },
  { id: 4, texto: "Hacer proyecto",completada: false },
];

// Agregar nueva tarea
todoList.push({ id: 5, texto: "Subir al repositorio", completada: false });

// Eliminar una tarea por id
todoList = todoList.filter(tarea => tarea.id !== 2);

// Obtener solo las tareas pendientes
const pendientes = todoList.filter(tarea => !tarea.completada);
console.log("Pendientes:", pendientes.map(t => t.texto));
// ["Aprender JS", "Hacer proyecto", "Subir al repositorio"]

// Obtener solo los textos de todas las tareas
const textos = todoList.map(tarea => tarea.texto);
console.log("Todas:", textos);

// Encontrar una tarea específica
const tarea3 = todoList.find(tarea => tarea.id === 3);
console.log("Tarea 3:", tarea3);

// Contar tareas completadas con reduce
const completadas = todoList.reduce((acum, tarea) => acum + (tarea.completada ? 1 : 0), 0);
console.log(`Completadas: ${completadas} de ${todoList.length}`);
```

---

### 25. Objetos en JavaScript

```javascript
// ── Sintaxis literal de objeto ──
const persona = {
  nombre: "Laura",
  apellido: "González",
  edad: 28,
  activa: true,
  hobbies: ["lectura", "programación", "ciclismo"],

  // Método: una función como propiedad del objeto
  saludar: function() {
    return `Hola! Soy ${this.nombre} ${this.apellido} y tengo ${this.edad} años.`;
  },

  // También se puede escribir como arrow function, pero "this" se comporta diferente
  // Para métodos de objetos, se recomienda la sintaxis de función normal o shorthand:
  presentarse() {
    return `Me llamo ${this.nombre} y me gusta ${this.hobbies[0]}.`;
  }
};

// ── Acceso a propiedades ──
console.log(persona.nombre);          // "Laura"  → notación de punto
console.log(persona["apellido"]);     // "González" → notación de corchetes
console.log(persona.hobbies[1]);      // "programación"

// ── Llamar métodos ──
console.log(persona.saludar());
// "Hola! Soy Laura González y tengo 28 años."
console.log(persona.presentarse());
// "Me llamo Laura y me gusta lectura."

// ── Agregar y modificar propiedades ──
persona.email = "laura@ejemplo.com";  // agregar nueva propiedad
persona.edad = 29;                    // modificar propiedad existente

// ── Desestructuración (forma moderna de extraer propiedades) ──
const { nombre, edad, hobbies } = persona;
console.log(nombre, edad);            // "Laura" 29

// ── Spread operator: clonar y combinar objetos ──
const personaActualizada = { ...persona, ciudad: "Rosario" };
console.log(personaActualizada.ciudad); // "Rosario"
```

---

### 26. El DOM - Document Object Model

El DOM es la representación en memoria del documento HTML. JavaScript puede acceder a él y modificarlo para cambiar el contenido, los estilos y la estructura de la página en tiempo real.

```html
<!-- index.html -->
<!DOCTYPE html>
<html lang="es">
  <head>
    <meta charset="UTF-8" />
    <title>Manipulación del DOM</title>
  </head>
  <body>
    <h2 id="titulo">Texto original</h2>
    <p id="parrafo" class="texto">Este es un párrafo de ejemplo.</p>
    <button id="btn-cambiar">Cambiar texto y color</button>

    <script src="dom.js"></script>
  </body>
</html>
```

```javascript
// dom.js

// ── Seleccionar elementos ──
const titulo   = document.getElementById("titulo");
const parrafo  = document.querySelector("#parrafo");         // selector CSS
const botones  = document.querySelectorAll("button");        // devuelve NodeList

// ── Leer y modificar contenido ──
console.log(parrafo.textContent);  // texto sin HTML
console.log(parrafo.innerHTML);    // contenido con etiquetas HTML

// ── Modificar estilos directamente ──
titulo.style.color = "#2c3e50";
titulo.style.fontSize = "2rem";
titulo.style.textAlign = "center";

// ── Trabajar con clases CSS (recomendado) ──
parrafo.classList.add("destacado");     // agrega clase
parrafo.classList.remove("texto");      // quita clase
parrafo.classList.toggle("oculto");     // agrega si no está, quita si está
console.log(parrafo.classList.contains("destacado")); // true/false

// ── Cambiar texto y color al hacer clic en el botón ──
const btnCambiar = document.getElementById("btn-cambiar");

btnCambiar.addEventListener("click", () => {
  titulo.textContent = "¡Texto cambiado con JavaScript!";
  titulo.style.color = "#e74c3c";

  parrafo.innerHTML = "El párrafo fue <strong>modificado</strong> dinámicamente.";
  parrafo.style.backgroundColor = "#f0f8ff";
  parrafo.style.padding = "12px";
  parrafo.style.borderRadius = "4px";
});
```

---

### 27. Eventos

Los eventos permiten ejecutar código en respuesta a acciones del usuario o del navegador.

```html
<!-- formulario.html -->
<!DOCTYPE html>
<html lang="es">
  <head>
    <meta charset="UTF-8" />
    <title>Validación de Formulario</title>
    <style>
      .error { color: #e74c3c; font-size: 0.85rem; margin-top: 4px; }
      .campo-invalido { border-color: #e74c3c; }
    </style>
  </head>
  <body>
    <form id="form-registro">
      <div>
        <label for="nombre">Nombre:</label>
        <input type="text" id="nombre" name="nombre" />
        <span class="error" id="error-nombre"></span>
      </div>
      <div>
        <label for="email">Email:</label>
        <input type="email" id="email-input" name="email" />
        <span class="error" id="error-email"></span>
      </div>
      <div>
        <label for="password">Contraseña (mínimo 6 caracteres):</label>
        <input type="password" id="password-input" name="password" />
        <span class="error" id="error-password"></span>
      </div>
      <button type="submit">Registrarse</button>
    </form>

    <script src="validacion.js"></script>
  </body>
</html>
```

```javascript
// validacion.js

// Esperar a que el DOM esté completamente cargado
document.addEventListener("DOMContentLoaded", () => {

  const form = document.getElementById("form-registro");

  // ── Evento change: detectar cambio en un campo ──
  document.getElementById("email-input").addEventListener("change", (event) => {
    console.log("Email ingresado:", event.target.value);
  });

  // ── Evento keyup: detectar escritura en tiempo real ──
  document.getElementById("nombre").addEventListener("keyup", (event) => {
    const valor = event.target.value.trim();
    const errorSpan = document.getElementById("error-nombre");
    if (valor.length > 0 && valor.length < 2) {
      errorSpan.textContent = "El nombre debe tener al menos 2 caracteres.";
    } else {
      errorSpan.textContent = "";
    }
  });

  // ── Evento submit: validar antes de enviar el formulario ──
  form.addEventListener("submit", (event) => {
    event.preventDefault(); // detiene el envío por defecto

    const nombre   = document.getElementById("nombre").value.trim();
    const email    = document.getElementById("email-input").value.trim();
    const password = document.getElementById("password-input").value;
    let hayErrores = false;

    // Validar nombre
    if (!nombre) {
      document.getElementById("error-nombre").textContent = "El nombre es obligatorio.";
      document.getElementById("nombre").classList.add("campo-invalido");
      hayErrores = true;
    } else {
      document.getElementById("error-nombre").textContent = "";
      document.getElementById("nombre").classList.remove("campo-invalido");
    }

    // Validar email (expresión regular básica)
    const regexEmail = /^[^\s@]+@[^\s@]+\.[^\s@]+$/;
    if (!email || !regexEmail.test(email)) {
      document.getElementById("error-email").textContent = "Ingresá un email válido.";
      document.getElementById("email-input").classList.add("campo-invalido");
      hayErrores = true;
    } else {
      document.getElementById("error-email").textContent = "";
      document.getElementById("email-input").classList.remove("campo-invalido");
    }

    // Validar contraseña
    if (password.length < 6) {
      document.getElementById("error-password").textContent = "La contraseña debe tener al menos 6 caracteres.";
      document.getElementById("password-input").classList.add("campo-invalido");
      hayErrores = true;
    } else {
      document.getElementById("error-password").textContent = "";
      document.getElementById("password-input").classList.remove("campo-invalido");
    }

    if (!hayErrores) {
      console.log("Formulario válido. Enviando datos...");
      // Aquí iría el fetch para enviar al servidor
    }
  });

});
```

---

### 28. Fetch API y llamadas al servidor

La **Fetch API** permite hacer peticiones HTTP asíncronas desde el navegador sin recargar la página. Reemplaza al antiguo `XMLHttpRequest`.

```javascript
// ── Versión con Promesas (.then().then().catch()) ──
fetch("https://api.miservicio.com/productos")
  .then(response => {
    if (!response.ok) {
      throw new Error(`Error HTTP: ${response.status}`);
    }
    return response.json(); // parsea el cuerpo como JSON
  })
  .then(datos => {
    console.log("Productos recibidos:", datos);
    mostrarProductos(datos);
  })
  .catch(error => {
    console.error("Error al obtener productos:", error);
  });

// ── Versión con async/await (más legible, recomendada) ──
async function obtenerProductos() {
  try {
    const response = await fetch("https://api.miservicio.com/productos");

    if (!response.ok) {
      throw new Error(`Error HTTP: ${response.status}`);
    }

    const productos = await response.json();
    console.log("Productos:", productos);
    mostrarProductos(productos);
  } catch (error) {
    console.error("No se pudieron cargar los productos:", error);
  }
}

// ── Enviar datos con POST ──
async function crearProducto(nuevoProducto) {
  try {
    const response = await fetch("https://api.miservicio.com/productos", {
      method: "POST",
      headers: {
        "Content-Type": "application/json"
      },
      body: JSON.stringify(nuevoProducto) // convierte objeto JS a string JSON
    });

    const productoCreado = await response.json();
    console.log("Producto creado:", productoCreado);
    return productoCreado;
  } catch (error) {
    console.error("Error al crear producto:", error);
  }
}

// ── Mostrar datos en una tabla del DOM ──
function mostrarProductos(productos) {
  const tabla = document.getElementById("tabla-productos");
  tabla.innerHTML = ""; // limpiar tabla antes de repoblar

  const encabezado = `
    <thead>
      <tr>
        <th>ID</th>
        <th>Nombre</th>
        <th>Precio</th>
        <th>Stock</th>
      </tr>
    </thead>
  `;

  const filas = productos
    .map(p => `
      <tr>
        <td>${p.id}</td>
        <td>${p.nombre}</td>
        <td>$${p.precio.toFixed(2)}</td>
        <td>${p.stock}</td>
      </tr>
    `)
    .join("");

  tabla.innerHTML = encabezado + `<tbody>${filas}</tbody>`;
}

// Llamar la función cuando cargue la página
document.addEventListener("DOMContentLoaded", obtenerProductos);
```

```html
<!-- En el HTML -->
<table id="tabla-productos"></table>
```

---

### 29. CORS

**CORS** (_Cross-Origin Resource Sharing_) es un mecanismo de seguridad del navegador que **bloquea por defecto** las peticiones HTTP entre diferentes orígenes.

Un **origen** está compuesto por: protocolo + dominio + puerto. Por ejemplo:
- `http://localhost:5173` (frontend en Vite)
- `https://api.miservicio.com:443` (backend)

Si el frontend y el backend están en orígenes distintos, el navegador bloqueará la petición a menos que el servidor incluya los encabezados de CORS apropiados en su respuesta.

> CORS es una restricción del **navegador**, no del servidor. Una herramienta como Postman o curl puede hacer la misma petición sin problemas porque no tiene esta restricción.

Para habilitar CORS en una API de **.NET Core** hay que configurarlo en `Program.cs`:

```csharp
// Program.cs

var builder = WebApplication.CreateBuilder(args);

builder.Services.AddControllers();

// ── Configurar política de CORS ──
builder.Services.AddCors(options =>
{
    options.AddPolicy("PermitirFrontend", policy =>
    {
        policy
            .WithOrigins(
                "http://localhost:5500",   // Live Server de VS Code
                "http://127.0.0.1:5500",  // variante de Live Server
                "http://localhost:5173",  // Vite (React, Vue, etc.)
                "https://mi-frontend.com" // dominio de producción
            )
            .AllowAnyHeader()
            .AllowAnyMethod();
    });
});

var app = builder.Build();

// ── Aplicar el middleware de CORS ──
// IMPORTANTE: debe ir antes de UseAuthorization y MapControllers
app.UseCors("PermitirFrontend");

app.UseAuthorization();
app.MapControllers();
app.Run();
```

Para desarrollo local, se puede usar una política más permisiva:

```csharp
// Solo para desarrollo - NO usar en producción
builder.Services.AddCors(options =>
{
    options.AddPolicy("DesarrolloLocal", policy =>
    {
        policy.AllowAnyOrigin()
              .AllowAnyHeader()
              .AllowAnyMethod();
    });
});
```

---

### 30. Proyecto Completo: Frontend + Backend

Este ejemplo integra todo lo aprendido: una página HTML con CSS y JavaScript que consume una API REST construida en .NET Core.

**Flujo de datos:**
```
Usuario → HTML page → fetch() → .NET API → Base de datos
                    ←          ← JSON response ←
        ← DOM actualizado ←
```

**Estructura de carpetas recomendada:**
```
mi-proyecto/
├── backend/
│   └── MiApi/
│       ├── Controllers/
│       │   └── ProductosController.cs
│       ├── Models/
│       │   └── Producto.cs
│       └── Program.cs
└── frontend/
    ├── index.html
    ├── estilos.css
    └── app.js
```

```csharp
// backend/MiApi/Models/Producto.cs
public class Producto
{
    public int Id { get; set; }
    public string Nombre { get; set; } = string.Empty;
    public decimal Precio { get; set; }
    public int Stock { get; set; }
}
```

```csharp
// backend/MiApi/Controllers/ProductosController.cs
using Microsoft.AspNetCore.Mvc;

[ApiController]
[Route("api/[controller]")]
public class ProductosController : ControllerBase
{
    // Lista simulada (en un proyecto real vendría de la base de datos)
    private static List<Producto> _productos = new()
    {
        new Producto { Id = 1, Nombre = "Notebook",  Precio = 800000, Stock = 10 },
        new Producto { Id = 2, Nombre = "Mouse",     Precio = 12000,  Stock = 50 },
        new Producto { Id = 3, Nombre = "Teclado",   Precio = 25000,  Stock = 30 },
    };

    [HttpGet]
    public ActionResult<IEnumerable<Producto>> GetTodos()
    {
        return Ok(_productos);
    }

    [HttpGet("{id}")]
    public ActionResult<Producto> GetPorId(int id)
    {
        var producto = _productos.FirstOrDefault(p => p.Id == id);
        if (producto == null) return NotFound();
        return Ok(producto);
    }

    [HttpPost]
    public ActionResult<Producto> Crear([FromBody] Producto nuevo)
    {
        nuevo.Id = _productos.Max(p => p.Id) + 1;
        _productos.Add(nuevo);
        return CreatedAtAction(nameof(GetPorId), new { id = nuevo.Id }, nuevo);
    }
}
```

```html
<!-- frontend/index.html -->
<!DOCTYPE html>
<html lang="es">
  <head>
    <meta charset="UTF-8" />
    <meta name="viewport" content="width=device-width, initial-scale=1.0" />
    <title>Gestión de Productos</title>
    <link rel="stylesheet" href="estilos.css" />
  </head>
  <body>
    <header>
      <h1>Gestión de Productos</h1>
    </header>

    <main>
      <section>
        <h2>Agregar Producto</h2>
        <form id="form-producto">
          <label for="input-nombre">Nombre:</label>
          <input type="text" id="input-nombre" required />

          <label for="input-precio">Precio:</label>
          <input type="number" id="input-precio" min="0" step="0.01" required />

          <label for="input-stock">Stock:</label>
          <input type="number" id="input-stock" min="0" required />

          <button type="submit">Agregar</button>
        </form>
      </section>

      <section>
        <h2>Lista de Productos</h2>
        <div id="estado-carga">Cargando...</div>
        <table id="tabla-productos"></table>
      </section>
    </main>

    <script src="app.js"></script>
  </body>
</html>
```

```javascript
// frontend/app.js

const API_URL = "http://localhost:5000/api/productos";

// ── Cargar y mostrar productos ──
async function cargarProductos() {
  const estadoDiv = document.getElementById("estado-carga");
  estadoDiv.textContent = "Cargando...";

  try {
    const response = await fetch(API_URL);

    if (!response.ok) throw new Error(`Error ${response.status}`);

    const productos = await response.json();

    estadoDiv.textContent = "";
    renderizarTabla(productos);

  } catch (error) {
    estadoDiv.textContent = "Error al cargar los productos. Verificá que la API esté corriendo.";
    console.error(error);
  }
}

// ── Renderizar la tabla en el DOM ──
function renderizarTabla(productos) {
  const tabla = document.getElementById("tabla-productos");

  if (productos.length === 0) {
    tabla.innerHTML = "<p>No hay productos cargados.</p>";
    return;
  }

  const filas = productos
    .map(p => `
      <tr>
        <td>${p.id}</td>
        <td>${p.nombre}</td>
        <td>$${p.precio.toLocaleString("es-AR")}</td>
        <td>${p.stock}</td>
      </tr>
    `)
    .join("");

  tabla.innerHTML = `
    <thead>
      <tr><th>ID</th><th>Nombre</th><th>Precio</th><th>Stock</th></tr>
    </thead>
    <tbody>${filas}</tbody>
  `;
}

// ── Manejar el formulario para agregar productos ──
document.addEventListener("DOMContentLoaded", () => {
  cargarProductos();

  const form = document.getElementById("form-producto");

  form.addEventListener("submit", async (event) => {
    event.preventDefault();

    const nuevoProducto = {
      nombre: document.getElementById("input-nombre").value.trim(),
      precio: parseFloat(document.getElementById("input-precio").value),
      stock:  parseInt(document.getElementById("input-stock").value),
    };

    try {
      const response = await fetch(API_URL, {
        method: "POST",
        headers: { "Content-Type": "application/json" },
        body: JSON.stringify(nuevoProducto)
      });

      if (!response.ok) throw new Error(`Error ${response.status}`);

      form.reset();
      await cargarProductos(); // recargar la tabla con el nuevo producto
      console.log("Producto agregado correctamente");

    } catch (error) {
      console.error("Error al agregar el producto:", error);
    }
  });
});
```

---

## Herramientas Recomendadas

### Visual Studio Code + Live Server

**VS Code** es el editor de código recomendado para desarrollo web. Es gratuito, liviano y tiene una gran cantidad de extensiones.

Extensión indispensable para esta unidad: **Live Server** (de Ritwick Dey).

- Instalar desde el panel de extensiones de VS Code (Ctrl+Shift+X) buscando "Live Server".
- Una vez instalada, hacer clic derecho sobre el archivo `index.html` y seleccionar **"Open with Live Server"**.
- El navegador se abre automáticamente en `http://127.0.0.1:5500`.
- Cada vez que se guarda un archivo (`.html`, `.css`, `.js`), la página se **recarga automáticamente**.

Otras extensiones útiles:
- **Prettier**: formatea el código automáticamente al guardar.
- **Auto Rename Tag**: renombra la etiqueta de cierre cuando se modifica la de apertura.
- **CSS Peek**: permite ver los estilos CSS de un elemento directamente desde el HTML.

### Browser DevTools (F12)

Todos los navegadores modernos incluyen herramientas de desarrollo accesibles con **F12** (o clic derecho → "Inspeccionar").

Las pestañas más importantes:

| Pestaña | Para qué sirve |
|---|---|
| **Elements** | Ver y editar el HTML y CSS en tiempo real. Muy útil para experimentar con estilos. |
| **Console** | Ver mensajes de `console.log()`, errores y advertencias. También permite ejecutar código JS directamente. |
| **Network** | Ver todas las peticiones HTTP que hace la página (fetch, carga de imágenes, etc.) con sus respuestas. |
| **Sources** | Ver y depurar el código JavaScript con breakpoints. |
| **Application** | Ver localStorage, sessionStorage y cookies. |

### Cómo usar la consola del navegador

La consola es tu mejor herramienta para depurar JavaScript:

```javascript
// Imprimir valores simples
console.log("Valor de x:", x);

// Imprimir objetos (se pueden expandir en la consola)
console.log("Usuario:", { nombre: "Ana", edad: 25 });

// Imprimir arrays
console.log("Lista:", [1, 2, 3]);

// Mensajes de distintos niveles
console.info("Información");
console.warn("Advertencia: esto podría ser un problema");
console.error("Error: algo salió mal");

// Medir el tiempo de ejecución
console.time("miOperacion");
// ... código a medir ...
console.timeEnd("miOperacion"); // imprime el tiempo transcurrido

// Mostrar datos en formato tabla
console.table([
  { nombre: "Ana", edad: 25 },
  { nombre: "Carlos", edad: 30 }
]);
```

En la consola también se puede escribir código JavaScript directamente y ejecutarlo con Enter. Esto es muy útil para probar expresiones rápidamente sin modificar el código fuente.

---

*Programación I - UCSE | Unidad 7: Programación Visual*
