# Unidad 7: Programación Visual - HTML, CSS y JavaScript

En esta unidad aprendemos a construir interfaces web. Veremos cómo funciona **HTML** para estructurar el contenido, **CSS** para darle estilo y **JavaScript** para agregar interactividad. Al final conectaremos el frontend con una API desarrollada en .NET Core.

## El proyecto de la unidad

A diferencia de otras unidades, acá no vamos a ver ejemplos sueltos y desconectados. Vamos a construir **un solo proyecto**, el catálogo de productos de una tienda de tecnología ficticia llamada **TechStore**, y en cada sección vamos a sumarle una pieza nueva. El proyecto vive en tres archivos:

```
catalogo/
├── index.html   ← estructura (Sección 1)
├── estilos.css  ← presentación (Sección 2)
└── app.js       ← comportamiento (Sección 3)
```

Cada vez que aparezca el recuadro **🧩 Sumamos esto al proyecto**, ese código se agrega a uno de estos tres archivos. Al final de cada sección grande vas a encontrar el **archivo completo** tal como debería quedar hasta ese punto, para que puedas comparar tu propio avance. Te recomendamos ir escribiendo (no copiando y pegando) el código en tu propio editor con Live Server abierto, para ver cada cambio reflejado en el navegador.

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

El navegador lee el archivo HTML de arriba hacia abajo y construye el **DOM** (Document Object Model), que es la representación interna del documento que luego puede ser manipulada con JavaScript. Volveremos sobre esto en la Sección 3.

```html
<!DOCTYPE html>
<html lang="es">
  <head>
    <meta charset="UTF-8" />
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

> El `<script>` se coloca al final del `<body>` para que el HTML cargue antes de que JavaScript intente manipularlo. Ya lo dejamos preparado abajo, aunque `app.js` todavía no exista.

Con esto arrancamos `catalogo/index.html`, el archivo que vamos a ir completando durante toda la unidad:

🧩 **Sumamos esto al proyecto — `index.html` (versión inicial):**

```html
<!DOCTYPE html>
<html lang="es">
  <head>
    <meta charset="UTF-8" />
    <meta name="viewport" content="width=device-width, initial-scale=1.0" />
    <title>TechStore - Catálogo de Productos</title>
    <link rel="stylesheet" href="estilos.css" />
  </head>
  <body>

    <!-- Acá va a crecer el catálogo de productos durante toda la unidad -->

    <script src="app.js"></script>
  </body>
</html>
```

---

### 3. Encabezados y Texto

HTML ofrece seis niveles de encabezados (`<h1>` a `<h6>`) y varias etiquetas para formatear texto. `<h1>` es el más importante y debe usarse una sola vez por página (el título principal).

| Etiqueta | Descripción |
|---|---|
| `<h1>` a `<h6>` | Encabezados, de mayor a menor importancia |
| `<p>` | Párrafo |
| `<strong>` | Texto en **negrita** (semánticamente importante) |
| `<em>` | Texto en _cursiva_ (énfasis) |
| `<br>` | Salto de línea (etiqueta vacía, sin cierre) |
| `<blockquote>` | Cita larga en bloque, con sangría |

```html
<blockquote>
  "La simplicidad es la máxima sofisticación." — Leonardo da Vinci
</blockquote>
```

🧩 **Sumamos esto al proyecto** (dentro de `<body>`, antes del comentario):

```html
<header>
  <h1>TechStore</h1>
  <p>Tu tienda de tecnología <strong>online</strong>, con <em>envíos a todo el país</em>.</p>
</header>
```

---

### 4. Listas

HTML soporta tres tipos de listas:

- **`<ul>`** (_unordered list_): lista desordenada, con viñetas.
- **`<ol>`** (_ordered list_): lista ordenada, con números o letras.
- **`<dl>`** (_definition list_): lista de definiciones, con término (`<dt>`) y descripción (`<dd>`).

Todos los ítems de `<ul>` y `<ol>` se marcan con `<li>`.

🧩 **Sumamos esto al proyecto** (después del `<header>`, va a ser nuestra barra lateral de categorías):

```html
<aside>
  <h3>Categorías</h3>
  <ul id="lista-categorias">
    <li>Periféricos</li>
    <li>Computadoras</li>
    <li>Accesorios</li>
  </ul>
</aside>
```

---

### 5. Imágenes y Multimedia

HTML permite incrustar distintos tipos de medios en una página.

| Elemento | Descripción |
|---|---|
| `<img>` | Muestra una imagen. Etiqueta vacía (sin cierre). |
| `<video>` | Reproduce un video. Admite múltiples `<source>`. |
| `<audio>` | Reproduce audio. Admite múltiples `<source>`. |
| `<iframe>` | Incrusta contenido externo (mapas, videos de YouTube, etc.). |

El atributo `alt` en `<img>` es **obligatorio**: describe la imagen para lectores de pantalla y se muestra si la imagen no carga.

```html
<!-- Referencia: video con controles -->
<video width="640" height="360" controls>
  <source src="videos/intro.mp4" type="video/mp4" />
  Tu navegador no soporta el elemento de video.
</video>
```

🧩 **Sumamos esto al proyecto** (en una nueva sección "Sobre nosotros", que va después de la lista de categorías):

```html
<section id="nosotros">
  <h2>Sobre nosotros</h2>
  <img
    src="imagenes/equipo.jpg"
    alt="Equipo de TechStore trabajando en el local"
    width="800"
    height="400"
  />
  <p>Somos una tienda de tecnología con más de 10 años en el mercado, especializada en computadoras y periféricos.</p>
</section>
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

El atributo `target="_blank"` abre el enlace en una nueva pestaña. Siempre combinar con `rel="noopener noreferrer"` por seguridad.

🧩 **Sumamos esto al proyecto** — un menú de navegación por anclas dentro del `<header>`, y un pie de página con el mail de contacto:

```html
<header>
  <h1>TechStore</h1>
  <p>Tu tienda de tecnología <strong>online</strong>, con <em>envíos a todo el país</em>.</p>
  <nav>
    <a href="#catalogo">Catálogo</a>
    <a href="#comparativa">Comparativa</a>
    <a href="#agregar">Agregar producto</a>
    <a href="#nosotros">Nosotros</a>
  </nav>
</header>
```

```html
<footer>
  <p>&copy; 2026 TechStore - UCSE</p>
  <p>Contacto: <a href="mailto:info@techstore.com">info@techstore.com</a></p>
</footer>
```

(El `<footer>` va al final del `<body>`, justo antes del `<script>`.)

---

### 7. Tablas

Las tablas se usan para mostrar datos tabulares (filas y columnas). **No** deben usarse para maquetar la página (eso es tarea del CSS).

| Etiqueta | Descripción |
|---|---|
| `<table>` | Contenedor de la tabla |
| `<thead>` | Grupo de filas de encabezado |
| `<tbody>` | Grupo de filas de datos |
| `<tr>` | Fila (_table row_) |
| `<th>` | Celda de encabezado (_table header_) |
| `<td>` | Celda de datos (_table data_) |

En nuestro catálogo, el listado principal de productos lo vamos a mostrar como **tarjetas** (con CSS Grid, más adelante), pero una tabla sigue siendo la forma correcta de mostrar una comparativa de datos tabulares. Vamos a agregar una sección de comparativa rápida, cuyas filas va a completar JavaScript en la Sección 3.

🧩 **Sumamos esto al proyecto** (nueva sección, después de "Categorías"):

```html
<section id="comparativa">
  <h2>Comparativa de destacados</h2>
  <table>
    <thead>
      <tr>
        <th>Producto</th>
        <th>Precio</th>
        <th>Stock</th>
        <th>Categoría</th>
      </tr>
    </thead>
    <tbody id="tabla-comparativa">
      <!-- Las filas se generan dinámicamente con JavaScript (Sección 3) -->
    </tbody>
  </table>
</section>
```

---

### 8. Formularios

Los formularios permiten al usuario enviar datos. El elemento `<form>` agrupa todos los controles.

Siempre vincular cada `<label>` a su `<input>` con el atributo `for` (que debe coincidir con el `id` del input). Esto mejora la accesibilidad.

HTML5 trae **validación nativa** sin necesidad de JavaScript, a través de atributos:

| Atributo | Efecto |
|---|---|
| `required` | El campo no puede quedar vacío |
| `minlength` / `maxlength` | Longitud mínima/máxima de texto |
| `min` / `max` | Valor mínimo/máximo (números, fechas) |
| `pattern` | Expresión regular que el valor debe cumplir |
| `type="email"`, `type="number"` | El navegador valida el formato automáticamente |

Si un campo no cumple sus reglas, el navegador **bloquea el envío** del formulario y muestra un mensaje, sin escribir una sola línea de JavaScript. En la Sección 3 vamos a combinar esto con validación propia en JS para casos más específicos.

🧩 **Sumamos esto al proyecto** — el formulario para agregar productos al catálogo (nueva sección):

```html
<section id="agregar">
  <h2>Agregar producto</h2>
  <form id="form-producto">
    <label for="input-nombre">Nombre:</label>
    <input type="text" id="input-nombre" required minlength="2" />

    <label for="input-precio">Precio:</label>
    <input type="number" id="input-precio" min="0" step="0.01" required />

    <label for="input-stock">Stock:</label>
    <input type="number" id="input-stock" min="0" required />

    <label for="input-categoria">Categoría:</label>
    <select id="input-categoria">
      <option value="perifericos">Periféricos</option>
      <option value="computadoras">Computadoras</option>
      <option value="accesorios">Accesorios</option>
    </select>

    <button type="submit">Agregar al catálogo</button>
  </form>
</section>
```

---

### 9. Div, Semántica y Checkpoint del HTML

El elemento `<div>` es un contenedor genérico sin significado semántico. HTML5 introdujo **elementos semánticos** que reemplazan el uso de múltiples `<div>` cuando el contenido tiene un rol específico:

| Elemento | Descripción |
|---|---|
| `<header>` | Encabezado de la página o de una sección |
| `<nav>` | Bloque de navegación (menús, links) |
| `<main>` | Contenido principal único de la página |
| `<section>` | Sección temática del contenido |
| `<article>` | Contenido independiente y autocontenido (una tarjeta de producto, por ejemplo) |
| `<aside>` | Contenido relacionado pero secundario (barra lateral) |
| `<footer>` | Pie de página o de sección |

Usar semántica correcta mejora la **accesibilidad**, el **SEO** y la **mantenibilidad** del código. Cerramos la parte de HTML envolviendo todo lo que construimos en un `<main>`, y agregando el contenedor donde va a vivir la grilla de productos (todavía vacío: lo va a llenar JavaScript).

Un `<div>` sigue siendo perfectamente válido cuando **no** hay una etiqueta semántica que describa mejor el contenido — como el contenedor de la grilla de tarjetas, que es solo una caja de layout.

🧩 **Sumamos esto al proyecto** — el catálogo (grilla de productos) va como primera sección dentro de `<main>`, antes de "Comparativa":

```html
<section id="catalogo">
  <h2>Nuestros productos</h2>
  <p>Encontrá los mejores accesorios de tecnología a un clic.</p>

  <div id="estado-carga">Cargando productos...</div>
  <div class="grid-productos" id="grid-productos">
    <!-- Las tarjetas de producto se generan dinámicamente con JavaScript -->
  </div>
</section>
```

#### ✅ Checkpoint — `index.html` completo

```html
<!DOCTYPE html>
<html lang="es">
  <head>
    <meta charset="UTF-8" />
    <meta name="viewport" content="width=device-width, initial-scale=1.0" />
    <title>TechStore - Catálogo de Productos</title>
    <link rel="stylesheet" href="estilos.css" />
  </head>
  <body>

    <header>
      <h1>TechStore</h1>
      <p>Tu tienda de tecnología <strong>online</strong>, con <em>envíos a todo el país</em>.</p>
      <nav>
        <a href="#catalogo">Catálogo</a>
        <a href="#comparativa">Comparativa</a>
        <a href="#agregar">Agregar producto</a>
        <a href="#nosotros">Nosotros</a>
      </nav>
    </header>

    <main>
      <div class="contenido">
        <section id="catalogo">
          <h2>Nuestros productos</h2>
          <p>Encontrá los mejores accesorios de tecnología a un clic.</p>

          <div id="estado-carga">Cargando productos...</div>
          <div class="grid-productos" id="grid-productos">
            <!-- Las tarjetas de producto se generan dinámicamente con JavaScript -->
          </div>
        </section>

        <section id="comparativa">
          <h2>Comparativa de destacados</h2>
          <table>
            <thead>
              <tr>
                <th>Producto</th>
                <th>Precio</th>
                <th>Stock</th>
                <th>Categoría</th>
              </tr>
            </thead>
            <tbody id="tabla-comparativa">
              <!-- Las filas se generan dinámicamente con JavaScript -->
            </tbody>
          </table>
        </section>

        <section id="agregar">
          <h2>Agregar producto</h2>
          <form id="form-producto">
            <label for="input-nombre">Nombre:</label>
            <input type="text" id="input-nombre" required minlength="2" />

            <label for="input-precio">Precio:</label>
            <input type="number" id="input-precio" min="0" step="0.01" required />

            <label for="input-stock">Stock:</label>
            <input type="number" id="input-stock" min="0" required />

            <label for="input-categoria">Categoría:</label>
            <select id="input-categoria">
              <option value="perifericos">Periféricos</option>
              <option value="computadoras">Computadoras</option>
              <option value="accesorios">Accesorios</option>
            </select>

            <button type="submit">Agregar al catálogo</button>
          </form>
        </section>

        <section id="nosotros">
          <h2>Sobre nosotros</h2>
          <img
            src="imagenes/equipo.jpg"
            alt="Equipo de TechStore trabajando en el local"
            width="800"
            height="400"
          />
          <p>Somos una tienda de tecnología con más de 10 años en el mercado, especializada en computadoras y periféricos.</p>
        </section>
      </div>

      <aside>
        <h3>Categorías</h3>
        <ul id="lista-categorias">
          <li>Periféricos</li>
          <li>Computadoras</li>
          <li>Accesorios</li>
        </ul>
      </aside>
    </main>

    <footer>
      <p>&copy; 2026 TechStore - UCSE</p>
      <p>Contacto: <a href="mailto:info@techstore.com">info@techstore.com</a></p>
    </footer>

    <script src="app.js"></script>
  </body>
</html>
```

> Nota: agregamos un `<div class="contenido">` envolviendo las cuatro secciones principales, como hermano de `<aside>`. Es un `<div>` puramente de layout (no representa nada semántico), y nos va a servir en la Sección 2 para armar un diseño de dos columnas con Flexbox. Si abrís este archivo con Live Server ahora, vas a ver todo el contenido sin ningún estilo — eso es exactamente lo que ataca la Sección 2.

---
