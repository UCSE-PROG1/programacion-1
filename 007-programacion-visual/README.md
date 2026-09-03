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

## Sección 2: CSS - Cascading Style Sheets

---

### 10. ¿Qué es CSS?

CSS (_Cascading Style Sheets_) es el lenguaje que controla la presentación visual de los documentos HTML: colores, tipografías, tamaños, márgenes, disposición de los elementos, animaciones, etc.

Hay tres formas de agregar CSS a un documento HTML:

**1. Inline** (en línea): directamente en el atributo `style` del elemento. Mezcla estructura y presentación, difícil de mantener.
```html
<p style="color: red; font-size: 18px;">Texto rojo</p>
```

**2. Internal** (interno): dentro de una etiqueta `<style>` en el `<head>`. Útil para pruebas rápidas, pero no reutilizable.

**3. External** (externo): en un archivo `.css` separado, enlazado desde el HTML con `<link>`. Es el método **recomendado** y el que ya dejamos preparado en `index.html` con `<link rel="stylesheet" href="estilos.css" />`.

La sintaxis básica de una regla CSS:

```css
/* selector { propiedad: valor; } */

p {
  color: #333333;
  font-size: 16px;
  line-height: 1.6;
}
```

Vamos a ir armando `catalogo/estilos.css` en el mismo orden en que aparecen los temas.

---

### 11. Selectores

Los selectores determinan a qué elementos HTML se aplican los estilos.

```css
/* ── Selector de elemento ── aplica a todos los <p> */
p { color: #333; }

/* ── Selector de clase ── aplica a cualquier elemento con class="destacado" */
.destacado { background-color: yellow; }

/* ── Selector de ID ── aplica al elemento con ese id (único por página) */
#titulo-principal { font-size: 2rem; }

/* ── Selector descendiente ── <a> dentro de <nav> */
nav a { color: white; }

/* ── Selectores múltiples ── mismo estilo para varios selectores ── */
h1, h2, h3 { font-family: 'Segoe UI', Arial, sans-serif; }

/* ── Selector de atributo ── inputs de tipo number ── */
input[type="number"] { text-align: right; }
```

🧩 **Sumamos esto al proyecto** — arrancamos `estilos.css` con el reset de `box-sizing` (lo justificamos en la sección 14) y la tipografía base:

```css
/* estilos.css */

*, *::before, *::after {
  box-sizing: border-box;
}

body {
  margin: 0;
  font-family: 'Segoe UI', Arial, sans-serif;
  line-height: 1.6;
}
```

---

### 12. Colores y Tipografía

**Formas de definir colores:**
- Nombre: `red`, `white`, `black`, etc.
- Hexadecimal: `#FF5733`.
- RGB / RGBA: `rgb(255, 87, 51)` / `rgba(255, 87, 51, 0.8)` (el cuarto valor es la opacidad, de 0 a 1).

```css
h2 {
  color: #2c3e50;
  font-weight: 700;          /* bold */
  text-align: center;
}
```

🧩 **Sumamos esto al proyecto:**

```css
body {
  margin: 0;
  font-family: 'Segoe UI', Arial, sans-serif;
  line-height: 1.6;
  color: #333333;
  background-color: #f5f5f5;
}

h1, h2, h3 {
  color: #2c3e50;
}
```

---

### 13. Variables CSS (Custom Properties)

Repetir el mismo color o medida en decenas de reglas es difícil de mantener: si querés cambiar el color principal de la marca, hay que buscarlo en todo el archivo. Las **variables CSS** (_custom properties_) resuelven esto: se definen una vez y se reutilizan con `var()`.

Se definen dentro de un selector — casi siempre `:root`, que representa al elemento `<html>` y hace que la variable esté disponible en **todo** el documento:

```css
:root {
  --color-primario: #2c3e50;
  --color-secundario: #3498db;
  --espaciado: 1rem;
}

.boton {
  background-color: var(--color-primario);
  padding: var(--espaciado);
}

.boton:hover {
  background-color: var(--color-secundario);
}
```

A diferencia de un preprocesador como Sass, las variables CSS son **nativas del navegador**, se pueden leer y modificar con JavaScript (`elemento.style.setProperty(...)`), y respetan la cascada: se puede redefinir el valor de una variable dentro de una clase o de un `@media` para cambiar un tema completo con una sola línea.

🧩 **Sumamos esto al proyecto** — definimos la paleta y las medidas que vamos a usar en el resto de la hoja de estilos:

```css
:root {
  --color-primario: #2c3e50;
  --color-secundario: #3498db;
  --color-fondo: #f5f5f5;
  --color-texto: #333333;
  --color-error: #e74c3c;
  --color-exito: #2ecc71;
  --radio-borde: 8px;
  --espaciado: 1rem;
}

body {
  margin: 0;
  font-family: 'Segoe UI', Arial, sans-serif;
  line-height: 1.6;
  color: var(--color-texto);
  background-color: var(--color-fondo);
}

h1, h2, h3 {
  color: var(--color-primario);
}
```

(Esto reemplaza los colores "a mano" que habíamos puesto en el paso anterior.)

---

### 14. El Modelo de Caja (Box Model)

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

Por defecto, `width` y `height` solo afectan al **content**. Con `box-sizing: border-box` (que ya aplicamos globalmente en la sección 11), el `width` incluye padding y border, lo que es mucho más intuitivo y evita sorpresas al calcular anchos.

```css
.caja {
  width: 300px;
  padding: 20px;
  border: 2px solid #2980b9;
  /* con border-box, el ancho TOTAL sigue siendo 300px */
}
```

🧩 **Sumamos esto al proyecto** — la tarjeta de producto (todavía no se ve porque JS aún no genera ninguna, pero ya dejamos la clase lista):

```css
.tarjeta-producto {
  background: white;
  border-radius: var(--radio-borde);
  padding: 1.25rem;
  box-shadow: 0 2px 8px rgba(0, 0, 0, 0.1);
}

.tarjeta-producto h3 {
  margin-top: 0;
}
```

---

### 15. Display y Flexbox

La propiedad `display` controla cómo se comporta un elemento en el flujo del documento.

| Valor | Comportamiento |
|---|---|
| `block` | Ocupa todo el ancho disponible, con salto de línea antes y después. (`<div>`, `<p>`, `<h1>`) |
| `inline` | Solo ocupa lo necesario, sin salto de línea. No acepta `width`/`height`. (`<span>`, `<a>`) |
| `flex` | Activa **Flexbox** para el elemento y sus hijos directos. |
| `none` | Oculta el elemento (no ocupa espacio). |

**Flexbox** es el sistema de layout pensado para distribuir elementos en **una** dimensión (fila o columna):

```css
.navbar {
  display: flex;
  justify-content: space-between; /* distribución horizontal */
  align-items: center;            /* alineación vertical */
  gap: 24px;                      /* espacio entre hijos */
}
```

🧩 **Sumamos esto al proyecto** — el header con Flexbox, y el layout de dos columnas (`contenido` + `aside`) que dejamos preparado en el HTML:

```css
header {
  display: flex;
  flex-direction: column;
  gap: 0.5rem;
  background-color: var(--color-primario);
  color: white;
  padding: var(--espaciado);
}

header h1 { color: white; margin: 0; }

header nav {
  display: flex;
  gap: 1.5rem;
}

header nav a {
  color: white;
  text-decoration: none;
}

main {
  max-width: 1200px;
  width: 90%;
  margin: 0 auto;
  padding: var(--espaciado) 0;
  display: flex;
  flex-direction: column;
  gap: 2rem;
}

.contenido { flex: 3; }
aside { flex: 1; }

aside ul {
  list-style: none;
  padding: 0;
}

footer {
  background: var(--color-primario);
  color: white;
  text-align: center;
  padding: var(--espaciado);
}

footer a { color: white; }
```

Por ahora `main` se muestra en columna (mobile-first): el catálogo, la comparativa, el formulario y "nosotros" arriba, y la barra de categorías abajo. En la sección 19 (Responsive Design) vamos a hacer que `main` pase a `flex-direction: row` en pantallas grandes, para que `aside` quede como una columna lateral.

---

### 16. CSS Grid

Mientras Flexbox distribuye elementos en una sola dimensión, **CSS Grid** está pensado para layouts de **dos dimensiones**: filas y columnas a la vez. Es la herramienta ideal para una grilla de tarjetas, como la de nuestro catálogo.

```css
.grid {
  display: grid;
  grid-template-columns: repeat(3, 1fr); /* 3 columnas de igual ancho */
  gap: 16px;
}
```

El patrón más usado para grillas de tarjetas **responsive sin media queries** es `repeat(auto-fill, minmax(...))`: el navegador calcula solo cuántas columnas entran, y cada una mide como mínimo el primer valor y como máximo el segundo.

```css
.grid-productos {
  display: grid;
  grid-template-columns: repeat(auto-fill, minmax(220px, 1fr));
  gap: 16px;
}
```

Con esto, si la pantalla es angosta entra una sola columna de tarjetas; si es ancha, entran varias, todas del mismo tamaño, **sin escribir ni una media query**.

🧩 **Sumamos esto al proyecto** — la grilla del catálogo:

```css
.grid-productos {
  display: grid;
  grid-template-columns: repeat(auto-fill, minmax(220px, 1fr));
  gap: var(--espaciado);
  margin-top: 1rem;
}
```

> `display: flex` y `display: grid` no son excluyentes: en esta misma hoja de estilos usamos Flexbox para el layout general de la página (`header`, `main`) y Grid para la grilla de tarjetas. Cada uno se elige según si el problema es de una dimensión o de dos.

---

### 17. Unidades de Medida y `clamp()`

CSS ofrece unidades absolutas y relativas:

| Unidad | Tipo | Relativa a... | Uso recomendado |
|---|---|---|---|
| `px` | Absoluta | - | Bordes, sombras, valores fijos pequeños |
| `%` | Relativa | El contenedor padre | Anchos fluidos |
| `rem` | Relativa | El `font-size` del elemento raíz (`html`) | Tipografía y espaciado consistente |
| `vh` / `vw` | Relativa | Alto / ancho del viewport | Secciones de pantalla completa |

La ventaja de `rem` es que si el usuario cambia el tamaño de fuente en su navegador, toda la tipografía escala proporcionalmente. Por eso preferimos `rem` a `px` para fuentes y espaciados.

La función **`clamp(mínimo, preferido, máximo)`** permite que un valor escale de forma fluida entre un piso y un techo, sin necesidad de una media query para cada tamaño de pantalla:

```css
h1 {
  /* nunca más chico que 1.75rem, nunca más grande que 2.5rem,
     y en el medio escala con el ancho del viewport (4vw) */
  font-size: clamp(1.75rem, 4vw, 2.5rem);
}
```

🧩 **Sumamos esto al proyecto:**

```css
html {
  font-size: 16px;
}

h1 {
  font-size: clamp(1.75rem, 4vw, 2.5rem);
}

h2 {
  font-size: clamp(1.4rem, 3vw, 2rem);
}
```

---

### 18. Pseudoclases y Pseudoelementos

Las **pseudoclases** aplican estilos según el estado del elemento (`:hover`, `:focus`) o su posición en el DOM (`:first-child`, `:nth-child`). Los **pseudoelementos** permiten estilizar partes específicas de un elemento (`::before`, `::after`).

```css
a:hover { text-decoration: underline; }

input:focus {
  outline: 2px solid var(--color-secundario);
}

tr:nth-child(even) { background-color: #f2f2f2; }
```

Dos pseudoclases muy útiles junto con la validación HTML5 que vimos en la sección 8: **`:valid`** y **`:invalid`** aplican estilos según si el valor actual del campo cumple sus reglas (`required`, `min`, `type="email"`, etc.), sin una sola línea de JavaScript.

🧩 **Sumamos esto al proyecto** — hover en la tarjeta y en el botón, y feedback visual de validación en el formulario:

```css
.tarjeta-producto {
  transition: transform 0.15s ease, box-shadow 0.15s ease;
}

.tarjeta-producto:hover {
  transform: translateY(-4px);
  box-shadow: 0 6px 16px rgba(0, 0, 0, 0.15);
}

.tarjeta-producto .precio {
  font-weight: 700;
  color: var(--color-secundario);
  font-size: 1.2rem;
}

.btn-favorito {
  border: none;
  background: none;
  font-size: 1.4rem;
  cursor: pointer;
}

.btn-favorito.activo { color: var(--color-error); }

form { max-width: 400px; }
label { display: block; margin-top: 12px; font-weight: bold; }
input, select { width: 100%; padding: 8px; margin-top: 4px; }

/* Solo se marcan en rojo/verde los campos que el usuario ya tocó */
input:not(:placeholder-shown):invalid { border-color: var(--color-error); }
input:not(:placeholder-shown):valid { border-color: var(--color-exito); }

button[type="submit"] {
  margin-top: 16px;
  padding: 10px 24px;
  background: var(--color-primario);
  color: white;
  border: none;
  border-radius: 4px;
  cursor: pointer;
}

button[type="submit"]:hover {
  background: var(--color-secundario);
}

table { border-collapse: collapse; width: 100%; }
th, td { border: 1px solid #ccc; padding: 8px 12px; text-align: center; }
thead { background-color: var(--color-primario); color: white; }
```

---

### 19. Responsive Design y Checkpoint del CSS

El diseño responsive permite que una misma página se vea bien en pantallas de distintos tamaños. Las **media queries** aplican estilos solo cuando se cumplen ciertas condiciones (ancho de pantalla, orientación, etc.).

Estrategia recomendada: **Mobile First**. Escribir primero los estilos para pantallas pequeñas (como ya venimos haciendo) y luego agregar overrides para pantallas más grandes con `min-width`.

```css
/* Base: mobile first */
.tarjetas { flex-direction: column; }

/* Desktop: pantallas desde 1024px */
@media (min-width: 1024px) {
  .tarjetas { flex-direction: row; }
}
```

🧩 **Sumamos esto al proyecto** — a partir de tablet, `main` pasa a fila para que `aside` quede como columna lateral:

```css
@media (min-width: 768px) {
  main {
    flex-direction: row;
    align-items: flex-start;
  }
}
```

Gracias a `grid-template-columns: repeat(auto-fill, minmax(...))` (sección 16) y a `clamp()` (sección 17), la grilla de productos y la tipografía **ya son responsive sin necesidad de una media query propia** — esa es justamente la ventaja de esas dos herramientas modernas frente al enfoque de escribir un breakpoint para cada tamaño.

#### ✅ Checkpoint — `estilos.css` completo

```css
/* estilos.css */

*, *::before, *::after {
  box-sizing: border-box;
}

:root {
  --color-primario: #2c3e50;
  --color-secundario: #3498db;
  --color-fondo: #f5f5f5;
  --color-texto: #333333;
  --color-error: #e74c3c;
  --color-exito: #2ecc71;
  --radio-borde: 8px;
  --espaciado: 1rem;
}

html {
  font-size: 16px;
}

body {
  margin: 0;
  font-family: 'Segoe UI', Arial, sans-serif;
  line-height: 1.6;
  color: var(--color-texto);
  background-color: var(--color-fondo);
}

h1, h2, h3 {
  color: var(--color-primario);
}

h1 { font-size: clamp(1.75rem, 4vw, 2.5rem); }
h2 { font-size: clamp(1.4rem, 3vw, 2rem); }

header {
  display: flex;
  flex-direction: column;
  gap: 0.5rem;
  background-color: var(--color-primario);
  color: white;
  padding: var(--espaciado);
}

header h1 { color: white; margin: 0; }

header nav {
  display: flex;
  gap: 1.5rem;
}

header nav a {
  color: white;
  text-decoration: none;
}

header nav a:hover {
  text-decoration: underline;
}

main {
  max-width: 1200px;
  width: 90%;
  margin: 0 auto;
  padding: var(--espaciado) 0;
  display: flex;
  flex-direction: column;
  gap: 2rem;
}

.contenido { flex: 3; }
aside { flex: 1; }

aside ul {
  list-style: none;
  padding: 0;
}

section { margin-bottom: 2.5rem; }

.grid-productos {
  display: grid;
  grid-template-columns: repeat(auto-fill, minmax(220px, 1fr));
  gap: var(--espaciado);
  margin-top: 1rem;
}

.tarjeta-producto {
  background: white;
  border-radius: var(--radio-borde);
  padding: 1.25rem;
  box-shadow: 0 2px 8px rgba(0, 0, 0, 0.1);
  transition: transform 0.15s ease, box-shadow 0.15s ease;
}

.tarjeta-producto:hover {
  transform: translateY(-4px);
  box-shadow: 0 6px 16px rgba(0, 0, 0, 0.15);
}

.tarjeta-producto h3 { margin-top: 0; }

.tarjeta-producto .precio {
  font-weight: 700;
  color: var(--color-secundario);
  font-size: 1.2rem;
}

.btn-favorito {
  border: none;
  background: none;
  font-size: 1.4rem;
  cursor: pointer;
}

.btn-favorito.activo { color: var(--color-error); }

table { border-collapse: collapse; width: 100%; }
th, td { border: 1px solid #ccc; padding: 8px 12px; text-align: center; }
thead { background-color: var(--color-primario); color: white; }

form { max-width: 400px; }
label { display: block; margin-top: 12px; font-weight: bold; }
input, select { width: 100%; padding: 8px; margin-top: 4px; }

input:not(:placeholder-shown):invalid { border-color: var(--color-error); }
input:not(:placeholder-shown):valid { border-color: var(--color-exito); }

button[type="submit"] {
  margin-top: 16px;
  padding: 10px 24px;
  background: var(--color-primario);
  color: white;
  border: none;
  border-radius: 4px;
  cursor: pointer;
}

button[type="submit"]:hover {
  background: var(--color-secundario);
}

footer {
  background: var(--color-primario);
  color: white;
  text-align: center;
  padding: var(--espaciado);
}

footer a { color: white; }

@media (min-width: 768px) {
  main {
    flex-direction: row;
    align-items: flex-start;
  }
}
```

Si abrís `index.html` con Live Server ahora, la página ya se ve como un sitio real: header oscuro, tipografía escalada, tarjeta y formulario con estilo. Lo único que falta es que la grilla y la tabla tengan contenido — de eso se encarga JavaScript.

---

## Sección 3: JavaScript

---

### 20. ¿Qué es JavaScript?

JavaScript (JS) es el lenguaje de programación de la web. Se ejecuta directamente en el **navegador** del usuario (lado del cliente), lo que permite crear páginas dinámicas e interactivas sin necesidad de comunicarse con el servidor para cada acción.

Usos principales:
- Responder a eventos del usuario (clics, teclado, formularios).
- Modificar el contenido y estilo de la página dinámicamente.
- Realizar peticiones a APIs sin recargar la página (Fetch).
- Complementar la validación nativa de formularios con reglas propias.

Ya dejamos `<script src="app.js"></script>` al final del `<body>` desde la sección 2. A partir de ahora, todo lo que escribamos va a ese archivo.

```javascript
// app.js

console.log("app.js cargado correctamente");
```

---

### 21. Variables y Tipos de Datos

JavaScript es un lenguaje de **tipado dinámico**: una variable puede cambiar de tipo durante la ejecución.

- `let`: declara una variable que puede cambiar de valor.
- `const`: declara una constante. No puede reasignarse (pero los objetos y arrays declarados con `const` sí pueden modificarse internamente).
- `var`: forma antigua, con problemas de alcance (_scope_). **Evitar su uso.**

```javascript
// ── Números ──
let edad = 25;
const PI = 3.14159;

// ── Cadenas de texto (string) ──
let nombre = "María";
let mensaje = `Hola, ${nombre}! Tenés ${edad} años.`; // template literal

// ── Booleanos ──
let estaActivo = true;

// ── Arrays y objetos ──
let frutas = ["manzana", "banana", "naranja"];
let persona = { nombre: "Carlos", edad: 30 };

console.log(typeof edad);      // "number"
console.log(typeof nombre);    // "string"
console.log(typeof persona);   // "object"
```

Con estos conceptos ya podemos modelar los datos del catálogo. Vamos a construir la lógica en piezas hasta llegar a una versión completa y funcional al final de la sección 30.

---

### 22. Operadores

```javascript
let a = 10, b = 3;

console.log(a + b);   // 13
console.log(a % b);   // 1  (módulo: resto de la división entera)
console.log(a ** b);  // 1000 (potenciación)

// ── Comparación: SIEMPRE usar === y !== (compara valor Y tipo) ──
console.log(5 === "5");    // false
console.log(5 == "5");     // true  → == convierte tipos, evitar
console.log(0 === false);  // false (correcto: distinto tipo)

// ── Operadores lógicos ──
let hayStock = true, estaEnOferta = false;
console.log(hayStock && estaEnOferta); // false
console.log(hayStock || estaEnOferta); // true
console.log(!hayStock);                // false
```

---

### 23. Condicionales

```javascript
function clasificarStock(cantidad) {
  if (cantidad === 0) {
    return "Sin stock";
  } else if (cantidad < 5) {
    return "Últimas unidades";
  } else {
    return "Disponible";
  }
}

console.log(clasificarStock(0));  // "Sin stock"
console.log(clasificarStock(3));  // "Últimas unidades"
console.log(clasificarStock(20)); // "Disponible"

// ── Operador ternario: condicion ? valorSiVerdadero : valorSiFalso ──
let stock = 2;
let etiqueta = stock > 0 ? "Comprar" : "Agotado";
```

Este patrón (`condicion ? valorA : valorB`) lo vamos a usar todo el tiempo en la sección 28 para decidir, por ejemplo, si un producto ya es favorito o no.

---

### 24. Bucles

```javascript
const categorias = ["Periféricos", "Computadoras", "Accesorios"];

// ── for...of (recorre los valores de un array) ──
for (const categoria of categorias) {
  console.log(categoria);
}

// ── forEach (método del array, recibe una función) ──
categorias.forEach((categoria, indice) => {
  console.log(`${indice + 1}. ${categoria}`);
});
```

En la práctica, para transformar listas de datos (como nuestro array de productos) vamos a usar sobre todo los métodos `map`, `filter`, `find` y `reduce`, que vemos en la sección 26 — son la forma moderna de recorrer arrays sin escribir un `for` manual.

---

### 25. Funciones

```javascript
// ── Declaración de función ──
function formatearPrecio(valor) {
  return `$${valor.toLocaleString("es-AR")}`;
}
console.log(formatearPrecio(800000)); // "$800.000"

// ── Arrow function: sintaxis concisa, muy usada en callbacks ──
const esFavorito = (id, favoritos) => favoritos.includes(id);

// Si el cuerpo tiene más de una línea, necesita llaves y return explícito:
const calcularDescuento = (precio, porcentaje) => {
  const descuento = precio * (porcentaje / 100);
  return precio - descuento;
};
```

---

### 26. Arrays en JavaScript

```javascript
let tareas = ["Definir HTML", "Definir CSS", "Conectar JS"];
tareas.push("Conectar con la API"); // agrega al final
console.log(tareas.length);         // 4

// ── Métodos funcionales para transformar listas ──
const productos = [
  { id: 1, nombre: "Notebook", precio: 800000, stock: 10, categoria: "computadoras" },
  { id: 2, nombre: "Mouse",    precio: 12000,  stock: 50, categoria: "perifericos" },
  { id: 3, nombre: "Teclado",  precio: 25000,  stock: 30, categoria: "perifericos" },
];

// filter: devuelve un nuevo array con los elementos que cumplen la condición
const perifericos = productos.filter(p => p.categoria === "perifericos");

// map: transforma cada elemento en otra cosa
const nombres = productos.map(p => p.nombre);

// find: devuelve el primer elemento que cumple la condición (o undefined)
const mouse = productos.find(p => p.nombre === "Mouse");

// reduce: acumula un único valor a partir de todo el array
const valorTotal = productos.reduce((acumulado, p) => acumulado + p.precio * p.stock, 0);

console.log(perifericos.length); // 2
console.log(nombres);            // ["Notebook", "Mouse", "Teclado"]
console.log(valorTotal);         // 9110000
```

Este array `productos` es exactamente el que vamos a usar como dato inicial del catálogo.

---

### 27. Objetos en JavaScript

```javascript
const producto = {
  id: 1,
  nombre: "Notebook",
  precio: 800000,
  stock: 10,
  categoria: "computadoras",

  // método: una función como propiedad del objeto
  descripcion() {
    return `${this.nombre} - $${this.precio.toLocaleString("es-AR")}`;
  }
};

console.log(producto.nombre);        // notación de punto
console.log(producto["precio"]);     // notación de corchetes
console.log(producto.descripcion()); // "Notebook - $800.000"

// ── Desestructuración: extraer propiedades directamente ──
const { nombre, precio } = producto;

// ── Spread operator: clonar y combinar objetos (por ejemplo, para agregar un id nuevo) ──
const productoConId = { ...producto, id: 4 };
```

🧩 **Sumamos esto al proyecto** — arrancamos `app.js` con los datos del catálogo (versión local, todavía sin conectar a ninguna API):

```javascript
// app.js

// ── Datos iniciales (versión local; en la sección 31 los reemplazamos por fetch) ──
let productos = [
  { id: 1, nombre: "Notebook", precio: 800000, stock: 10, categoria: "computadoras" },
  { id: 2, nombre: "Mouse",    precio: 12000,  stock: 50, categoria: "perifericos" },
  { id: 3, nombre: "Teclado",  precio: 25000,  stock: 30, categoria: "perifericos" },
];

let favoritos = []; // ids de los productos marcados como favoritos
```

---

### 28. El DOM

El DOM es la representación en memoria del documento HTML. JavaScript puede acceder a él y modificarlo para cambiar el contenido, los estilos y la estructura de la página en tiempo real.

```javascript
// ── Seleccionar elementos ──
const titulo  = document.getElementById("titulo");
const boton   = document.querySelector("#btn-cambiar"); // selector CSS

// ── Modificar contenido ──
titulo.textContent = "Nuevo texto";  // texto plano
titulo.innerHTML = "Texto <strong>con HTML</strong>"; // permite etiquetas

// ── Trabajar con clases CSS (preferible a modificar .style directamente) ──
titulo.classList.add("destacado");
titulo.classList.toggle("oculto");
```

Ahora escribimos las dos funciones que **generan el HTML de la grilla y de la tabla a partir del array `productos`**. Usamos template literals (sección 21) y `map` (sección 26) para transformar cada objeto producto en un fragmento de HTML, y `classList`/atributos `data-*` para vincular cada tarjeta con su id.

🧩 **Sumamos esto al proyecto:**

```javascript
// ── Renderizar catálogo (grilla de tarjetas) ──
function renderizarCatalogo(lista) {
  const grid = document.getElementById("grid-productos");
  grid.innerHTML = lista.map(p => `
    <article class="tarjeta-producto" data-id="${p.id}">
      <h3>${p.nombre}</h3>
      <p class="precio">$${p.precio.toLocaleString("es-AR")}</p>
      <p>Stock: ${p.stock}</p>
      <button class="btn-favorito ${favoritos.includes(p.id) ? "activo" : ""}" data-id="${p.id}">
        ${favoritos.includes(p.id) ? "♥" : "♡"}
      </button>
    </article>
  `).join("");
}

// ── Renderizar tabla comparativa ──
function renderizarComparativa(lista) {
  const tbody = document.getElementById("tabla-comparativa");
  tbody.innerHTML = lista.map(p => `
    <tr>
      <td>${p.nombre}</td>
      <td>$${p.precio.toLocaleString("es-AR")}</td>
      <td>${p.stock}</td>
      <td>${p.categoria}</td>
    </tr>
  `).join("");
}

function renderizarTodo() {
  renderizarCatalogo(productos);
  renderizarComparativa(productos);
}

// Primer render, apenas carga la página
document.addEventListener("DOMContentLoaded", () => {
  document.getElementById("estado-carga").textContent = "";
  renderizarTodo();
});
```

Si guardás y recargás con Live Server, ya deberías ver las tres tarjetas y las tres filas de la comparativa generadas dinámicamente, con el estilo que armamos en la Sección 2.

---

### 29. Eventos

Los eventos permiten ejecutar código en respuesta a acciones del usuario. Usamos `addEventListener` para "escuchar" un evento sobre un elemento.

```javascript
boton.addEventListener("click", () => {
  console.log("¡Click!");
});
```

Cuando muchos elementos generados dinámicamente (como nuestras tarjetas) necesitan el mismo comportamiento, en vez de agregar un listener a cada botón conviene usar **delegación de eventos**: escuchar el clic en el contenedor padre (que sí existe desde el principio) y revisar, con `event.target`, sobre qué elemento hijo se hizo clic.

También conectamos acá la validación HTML5 de la sección 8 con lógica propia: `event.preventDefault()` frena el envío del formulario para que podamos leer los valores, construir el objeto `producto` y actualizar el array antes de decidir qué hacer.

🧩 **Sumamos esto al proyecto:**

```javascript
document.addEventListener("DOMContentLoaded", () => {
  document.getElementById("estado-carga").textContent = "";
  renderizarTodo();

  // ── Delegación: un solo listener para todos los botones de favorito ──
  document.getElementById("grid-productos").addEventListener("click", (event) => {
    if (event.target.classList.contains("btn-favorito")) {
      const id = Number(event.target.dataset.id);
      alternarFavorito(id);
    }
  });

  // ── Envío del formulario ──
  document.getElementById("form-producto").addEventListener("submit", (event) => {
    event.preventDefault(); // el navegador ya validó required/min/minlength

    const nuevoProducto = {
      id: productos.length ? Math.max(...productos.map(p => p.id)) + 1 : 1,
      nombre: document.getElementById("input-nombre").value.trim(),
      precio: parseFloat(document.getElementById("input-precio").value),
      stock: parseInt(document.getElementById("input-stock").value),
      categoria: document.getElementById("input-categoria").value,
    };

    productos.push(nuevoProducto);
    renderizarTodo();
    event.target.reset();
  });
});

function alternarFavorito(id) {
  if (favoritos.includes(id)) {
    favoritos = favoritos.filter(favId => favId !== id); // ya estaba: lo sacamos
  } else {
    favoritos.push(id); // no estaba: lo agregamos
  }
  renderizarCatalogo(productos);
}
```

Probá agregar un producto y marcar/desmarcar favoritos: la grilla y la tabla se actualizan solas. Lo único es que si recargás la página, los favoritos se pierden — de eso se encarga la próxima sección.

---

### 30. Persistencia con `localStorage`

Hasta ahora, todo lo que hace JavaScript vive solo en la memoria de la pestaña: si recargamos la página, se pierde. El **Web Storage API** permite guardar datos en el navegador del usuario, disponibles incluso después de cerrar y volver a abrir la pestaña.

- `localStorage`: no tiene fecha de expiración, persiste hasta que se borre explícitamente.
- `sessionStorage`: misma API, pero se borra al cerrar la pestaña.

Solo puede guardar **strings**, por eso los objetos y arrays se convierten con `JSON.stringify()` al guardar y se reconstruyen con `JSON.parse()` al leer.

```javascript
localStorage.setItem("clave", "valor");        // guardar un string
localStorage.setItem("datos", JSON.stringify({ a: 1 })); // guardar un objeto

const valor = localStorage.getItem("clave");    // leer (siempre string o null)
const datos = JSON.parse(localStorage.getItem("datos") ?? "{}");

localStorage.removeItem("clave"); // borrar una clave puntual
```

Vamos a usarlo para que los **favoritos** sobrevivan a un refresh de la página.

🧩 **Sumamos esto al proyecto:**

```javascript
function cargarFavoritos() {
  const guardados = localStorage.getItem("favoritos");
  favoritos = guardados ? JSON.parse(guardados) : [];
}

function guardarFavoritos() {
  localStorage.setItem("favoritos", JSON.stringify(favoritos));
}

function alternarFavorito(id) {
  if (favoritos.includes(id)) {
    favoritos = favoritos.filter(favId => favId !== id);
  } else {
    favoritos.push(id);
  }
  guardarFavoritos(); // persistimos el cambio
  renderizarCatalogo(productos);
}
```

Y llamamos a `cargarFavoritos()` antes del primer render:

```javascript
document.addEventListener("DOMContentLoaded", () => {
  cargarFavoritos();
  document.getElementById("estado-carga").textContent = "";
  renderizarTodo();
  // ... el resto de los listeners queda igual ...
});
```

#### ✅ Checkpoint — `app.js` completo (versión local, con datos hardcodeados)

```javascript
// app.js

// ── Datos iniciales (versión local; en la sección 31 los reemplazamos por fetch) ──
let productos = [
  { id: 1, nombre: "Notebook", precio: 800000, stock: 10, categoria: "computadoras" },
  { id: 2, nombre: "Mouse",    precio: 12000,  stock: 50, categoria: "perifericos" },
  { id: 3, nombre: "Teclado",  precio: 25000,  stock: 30, categoria: "perifericos" },
];

let favoritos = []; // ids de los productos marcados como favoritos

// ── Renderizar catálogo (grilla de tarjetas) ──
function renderizarCatalogo(lista) {
  const grid = document.getElementById("grid-productos");
  grid.innerHTML = lista.map(p => `
    <article class="tarjeta-producto" data-id="${p.id}">
      <h3>${p.nombre}</h3>
      <p class="precio">$${p.precio.toLocaleString("es-AR")}</p>
      <p>Stock: ${p.stock}</p>
      <button class="btn-favorito ${favoritos.includes(p.id) ? "activo" : ""}" data-id="${p.id}">
        ${favoritos.includes(p.id) ? "♥" : "♡"}
      </button>
    </article>
  `).join("");
}

// ── Renderizar tabla comparativa ──
function renderizarComparativa(lista) {
  const tbody = document.getElementById("tabla-comparativa");
  tbody.innerHTML = lista.map(p => `
    <tr>
      <td>${p.nombre}</td>
      <td>$${p.precio.toLocaleString("es-AR")}</td>
      <td>${p.stock}</td>
      <td>${p.categoria}</td>
    </tr>
  `).join("");
}

function renderizarTodo() {
  renderizarCatalogo(productos);
  renderizarComparativa(productos);
}

// ── Favoritos persistidos con localStorage ──
function cargarFavoritos() {
  const guardados = localStorage.getItem("favoritos");
  favoritos = guardados ? JSON.parse(guardados) : [];
}

function guardarFavoritos() {
  localStorage.setItem("favoritos", JSON.stringify(favoritos));
}

function alternarFavorito(id) {
  if (favoritos.includes(id)) {
    favoritos = favoritos.filter(favId => favId !== id);
  } else {
    favoritos.push(id);
  }
  guardarFavoritos();
  renderizarCatalogo(productos);
}

// ── Eventos ──
document.addEventListener("DOMContentLoaded", () => {
  cargarFavoritos();
  document.getElementById("estado-carga").textContent = "";
  renderizarTodo();

  document.getElementById("grid-productos").addEventListener("click", (event) => {
    if (event.target.classList.contains("btn-favorito")) {
      const id = Number(event.target.dataset.id);
      alternarFavorito(id);
    }
  });

  document.getElementById("form-producto").addEventListener("submit", (event) => {
    event.preventDefault();

    const nuevoProducto = {
      id: productos.length ? Math.max(...productos.map(p => p.id)) + 1 : 1,
      nombre: document.getElementById("input-nombre").value.trim(),
      precio: parseFloat(document.getElementById("input-precio").value),
      stock: parseInt(document.getElementById("input-stock").value),
      categoria: document.getElementById("input-categoria").value,
    };

    productos.push(nuevoProducto);
    renderizarTodo();
    event.target.reset();
  });
});
```

En este punto el catálogo ya es una aplicación completa y funcional, sin depender de ningún servidor: HTML semántico, CSS moderno (variables, Grid, `clamp()`, responsive) y JavaScript con DOM, eventos y persistencia local. Lo que falta — y es el objetivo de lo que sigue — es reemplazar el array hardcodeado por datos reales de una API.

---

### 31. Fetch API y llamadas al servidor

La **Fetch API** permite hacer peticiones HTTP asíncronas desde el navegador sin recargar la página.

```javascript
// ── async/await (más legible que encadenar .then()) ──
async function obtenerDatos() {
  try {
    const response = await fetch("https://api.ejemplo.com/datos");

    if (!response.ok) {
      throw new Error(`Error HTTP: ${response.status}`);
    }

    const datos = await response.json();
    console.log(datos);
  } catch (error) {
    console.error("No se pudieron obtener los datos:", error);
  }
}
```

Vamos a reemplazar el array `productos` hardcodeado por una carga real desde una API. Guardamos la URL en una constante, y la función `cargarProductos` reemplaza al bloque de datos fijos.

🧩 **Sumamos esto al proyecto** (reemplaza la sección de "Datos iniciales" del checkpoint anterior):

```javascript
const API_URL = "http://localhost:5000/api/productos";

let productos = [];
let favoritos = [];

async function cargarProductos() {
  const estadoDiv = document.getElementById("estado-carga");
  estadoDiv.textContent = "Cargando productos...";

  try {
    const response = await fetch(API_URL);
    if (!response.ok) throw new Error(`Error HTTP: ${response.status}`);

    productos = await response.json();
    estadoDiv.textContent = "";
    renderizarTodo();
  } catch (error) {
    estadoDiv.textContent = "No se pudieron cargar los productos. Verificá que la API esté corriendo.";
    console.error(error);
  }
}

async function crearProducto(nuevo) {
  const response = await fetch(API_URL, {
    method: "POST",
    headers: { "Content-Type": "application/json" },
    body: JSON.stringify(nuevo),
  });

  if (!response.ok) throw new Error(`Error HTTP: ${response.status}`);
  return response.json();
}
```

Y el listener de `DOMContentLoaded` pasa a llamar `cargarProductos()` en vez de renderizar datos fijos, mientras que el `submit` del formulario ahora es `async` y llama a la API en vez de hacer `productos.push(...)` directamente:

```javascript
document.addEventListener("DOMContentLoaded", () => {
  cargarFavoritos();
  cargarProductos(); // reemplaza al renderizarTodo() con datos hardcodeados

  document.getElementById("grid-productos").addEventListener("click", (event) => {
    if (event.target.classList.contains("btn-favorito")) {
      const id = Number(event.target.dataset.id);
      alternarFavorito(id);
    }
  });

  document.getElementById("form-producto").addEventListener("submit", async (event) => {
    event.preventDefault();

    const nuevoProducto = {
      nombre: document.getElementById("input-nombre").value.trim(),
      precio: parseFloat(document.getElementById("input-precio").value),
      stock: parseInt(document.getElementById("input-stock").value),
      categoria: document.getElementById("input-categoria").value,
    };

    try {
      await crearProducto(nuevoProducto);
      event.target.reset();
      await cargarProductos(); // recargamos la lista desde el servidor
    } catch (error) {
      console.error("Error al agregar el producto:", error);
    }
  });
});
```

Notá que ya no necesitamos calcular el `id` a mano (`Math.max(...) + 1`): ahora lo asigna el servidor, que es su responsabilidad.

---

### 32. CORS

**CORS** (_Cross-Origin Resource Sharing_) es un mecanismo de seguridad del navegador que **bloquea por defecto** las peticiones HTTP entre diferentes orígenes.

Un **origen** está compuesto por: protocolo + dominio + puerto. Por ejemplo:
- `http://127.0.0.1:5500` (frontend con Live Server)
- `http://localhost:5000` (backend en .NET)

Si el frontend y el backend están en orígenes distintos, el navegador bloqueará la petición a menos que el servidor incluya los encabezados de CORS apropiados en su respuesta.

> CORS es una restricción del **navegador**, no del servidor. Una herramienta como Postman o curl puede hacer la misma petición sin problemas porque no tiene esta restricción.

Para habilitar CORS en una API de **.NET Core** hay que configurarlo en `Program.cs`:

```csharp
var builder = WebApplication.CreateBuilder(args);

builder.Services.AddControllers();

builder.Services.AddCors(options =>
{
    options.AddPolicy("PermitirFrontend", policy =>
    {
        policy
            .WithOrigins(
                "http://localhost:5500",  // Live Server de VS Code
                "http://127.0.0.1:5500"   // variante de Live Server
            )
            .AllowAnyHeader()
            .AllowAnyMethod();
    });
});

var app = builder.Build();

// IMPORTANTE: debe ir antes de UseAuthorization y MapControllers
app.UseCors("PermitirFrontend");

app.UseAuthorization();
app.MapControllers();
app.Run();
```

---

### 33. Proyecto Completo: Frontend + Backend con .NET

Cerramos la unidad conectando el catálogo con una API REST real, construida en .NET Core (Unidad 6). Este es el checkpoint final: los tres archivos del frontend, más el backend mínimo que los sirve.

**Flujo de datos:**
```
Usuario → index.html → fetch() → API .NET → lista en memoria
                     ←           ← JSON     ←
      ← DOM actualizado (grilla + tabla) ←
```

**Estructura de carpetas:**
```
techstore/
├── backend/
│   └── TechStoreApi/
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
// backend/TechStoreApi/Models/Producto.cs
public class Producto
{
    public int Id { get; set; }
    public string Nombre { get; set; } = string.Empty;
    public decimal Precio { get; set; }
    public int Stock { get; set; }
    public string Categoria { get; set; } = string.Empty;
}
```

```csharp
// backend/TechStoreApi/Controllers/ProductosController.cs
using Microsoft.AspNetCore.Mvc;

[ApiController]
[Route("api/[controller]")]
public class ProductosController : ControllerBase
{
    // Lista en memoria (en un proyecto real vendría de una base de datos, Unidad 5/6)
    private static List<Producto> _productos = new()
    {
        new Producto { Id = 1, Nombre = "Notebook", Precio = 800000, Stock = 10, Categoria = "computadoras" },
        new Producto { Id = 2, Nombre = "Mouse",     Precio = 12000,  Stock = 50, Categoria = "perifericos" },
        new Producto { Id = 3, Nombre = "Teclado",   Precio = 25000,  Stock = 30, Categoria = "perifericos" },
    };

    [HttpGet]
    public ActionResult<IEnumerable<Producto>> GetTodos()
    {
        return Ok(_productos);
    }

    [HttpPost]
    public ActionResult<Producto> Crear([FromBody] Producto nuevo)
    {
        nuevo.Id = _productos.Max(p => p.Id) + 1;
        _productos.Add(nuevo);
        return CreatedAtAction(nameof(GetTodos), nuevo);
    }
}
```

El **frontend** es exactamente el checkpoint de la sección 9 (`index.html`), el de la sección 19 (`estilos.css`) y el de esta sección (`app.js` con Fetch + localStorage), sin ningún cambio adicional — esa es la idea de haber construido todo de forma incremental: cada pieza nueva se apoya en las anteriores.

Con el backend corriendo (`dotnet run` en `TechStoreApi`) y el frontend abierto con Live Server, el catálogo carga los productos reales desde la API, permite agregar nuevos, y recuerda los favoritos entre recargas gracias a `localStorage`.

---

## Herramientas Recomendadas

### Visual Studio Code + Live Server

**VS Code** es el editor de código recomendado para desarrollo web. Es gratuito, liviano y tiene una gran cantidad de extensiones.

Extensión indispensable para esta unidad: **Live Server** (de Ritwick Dey).

- Instalar desde el panel de extensiones de VS Code (Ctrl+Shift+X) buscando "Live Server".
- Hacer clic derecho sobre `index.html` y seleccionar **"Open with Live Server"**.
- El navegador se abre automáticamente en `http://127.0.0.1:5500`.
- Cada vez que se guarda un archivo (`.html`, `.css`, `.js`), la página se **recarga automáticamente**.

Otras extensiones útiles:
- **Prettier**: formatea el código automáticamente al guardar.
- **Auto Rename Tag**: renombra la etiqueta de cierre cuando se modifica la de apertura.
- **CSS Peek**: permite ver los estilos CSS de un elemento directamente desde el HTML.

### Browser DevTools (F12)

Todos los navegadores modernos incluyen herramientas de desarrollo accesibles con **F12** (o clic derecho → "Inspeccionar").

| Pestaña | Para qué sirve |
|---|---|
| **Elements** | Ver y editar el HTML y CSS en tiempo real. Muy útil para experimentar con estilos y variables CSS. |
| **Console** | Ver mensajes de `console.log()`, errores y advertencias. También permite ejecutar código JS directamente. |
| **Network** | Ver todas las peticiones HTTP que hace la página (fetch, imágenes, etc.) con sus respuestas. |
| **Sources** | Ver y depurar el código JavaScript con breakpoints. |
| **Application** | Ver `localStorage`, `sessionStorage` y cookies — útil para inspeccionar los favoritos guardados. |

### Cómo usar la consola del navegador

```javascript
console.log("Valor de x:", x);
console.log("Producto:", { nombre: "Mouse", precio: 12000 });
console.warn("Advertencia: esto podría ser un problema");
console.error("Error: algo salió mal");

// Mostrar datos en formato tabla — ideal para inspeccionar el array de productos
console.table(productos);
```

En la consola también se puede escribir código JavaScript directamente y ejecutarlo con Enter. Es muy útil para probar expresiones rápidamente, o para inspeccionar el estado de `productos` y `favoritos` mientras se prueba la página.

---

*Programación I - UCSE | Unidad 7: Programación Visual*
