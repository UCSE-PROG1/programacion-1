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
