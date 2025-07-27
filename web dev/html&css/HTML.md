## ¿Qué es HTML?

**HTML (HyperText Markup Language)** es el lenguaje de marcado para estructurar el contenido de una página web. Define *qué hay* en una página (títulos, párrafos, listas, imágenes, formularios…), pero **no** se encarga del estilo (CSS) ni del comportamiento (JavaScript).

**Idea clave:** HTML = estructura y significado.

---

## Requisitos
- Un **navegador** moderno (Chrome, Firefox, Edge o Safari).
- Un **editor de código** (Visual Studio Code o similar).
- Una **carpeta de proyecto** con archivos `.html`.

---
## Estructura mínima de un documento

```html

<!doctype html>

<html lang="es">

  <head>

    <meta charset="utf-8" />

    <meta name="viewport" content="width=device-width, initial-scale=1.0" />

    <title>Mi primera página</title>

  </head>

  <body>

    <h1>¡Hola, HTML!</h1>

    <p>Este es mi primer documento.</p>

  </body>

</html>

```

**Notas:**

- `<!doctype html>` activa el modo estándar en navegadores modernos.
- `<html>` envuelve todo el documento.
- `<head>` contiene **metadatos** (no visibles).
- `<body>` contiene el **contenido visible**.

  

---
## ¿Qué son las etiquetas y los atributos?

Una **etiqueta** define un **elemento** con la forma general:

```html

<nombre atributo="valor">contenido</nombre>

```

Algunos elementos son **vacíos** (no tienen contenido ni etiqueta de cierre), por ejemplo: `<img>`, `<br>`, `<meta>`, `<link>`, `<input>`.

Los **atributos** son pares `nombre="valor"` que ajustan el comportamiento o aportan información adicional. Ejemplos:

```html

<img src="foto.jpg" alt="Descripción">

<a href="https://ejemplo.com" target="_blank" rel="noopener noreferrer">Visitar</a>

```

**Atributos booleanos** (verdaderos si están presentes): `required`, `disabled`, `checked`, `autoplay`, etc. Pueden escribirse como `required` o `required="required"` indistintamente.

**Convención:** usa **minúsculas** para nombres de elementos y atributos.
  

---
## Semántica en HTML
La **semántica** significa elegir el elemento que mejor describe el contenido: `<header>`, `<nav>`, `<main>`, `<article>`, `<section>`, `<aside>`, `<footer>`, `<em>`, `<strong>`, etc. Esto mejora **accesibilidad**, **SEO** y **mantenibilidad**.

---

## Encabezados (`h1`–`h6`) y párrafos (`p`)

- `h1` a `h6` indican **niveles jerárquicos de títulos** (no se eligen por tamaño visual).
- `<p>` define párrafos. No metas listas, encabezados u otros bloques **dentro** de un `<p>`.

```html

<h1>Guía de HTML</h1>

<h2>Estructura</h2>

<p>Este es un párrafo correcto.</p>

```

---

  

## Elementos de texto comunes: `strong` y `em`

- `<strong>` indica **fuerte importancia** (más que “poner en negrita”).
- `<em>` indica **énfasis** (más que “poner en cursiva”).

```html

<p>Recuerda <strong>guardar</strong> tus cambios.</p>

<p>Esta palabra <em>realmente</em> merece énfasis.</p>

```

---

  
## Listas

- **Desordenadas**: `<ul>` con `<li>`
- **Ordenadas**: `<ol>` con `<li>` (atributos útiles: `start`, `reversed`, `type`; en `<li>` puedes usar `value`)

```html

<ul>

  <li>HTML</li>

  <li>CSS</li>

  <li>JavaScript</li>

</ul>

  

<ol start="10" reversed>

  <li>Décimo</li>

  <li value="3">Tercero forzado</li>

</ol>

```

---

## Malas prácticas frecuentes (y cómo evitarlas)

- Usar muchos `<br>` para hacer espacio → utiliza **márgenes/padding con CSS**.
- Elegir niveles de título por tamaño visual → usa encabezados por **estructura**.
- Poner imágenes informativas sin `alt` → siempre incluye un **`alt` descriptivo**.
- Usar etiquetas obsoletas como `<center>` → centra con **CSS**.

---

## Elementos de reemplazo (replaced elements)

Son elementos cuya representación proviene de una **fuente externa** al árbol de HTML (p. ej., una imagen o un control nativo). Ejemplos: `<img>`, `<input>`, `<video>`, `<audio>`.


---

## Atributos globales, `id` y `class`

- **Atributos globales**: `id`, `class`, `title`, `style`, `lang`, `dir`, `role`, etc.
- **`id`**: identificador **único** por página (útil para anclajes, scripts, estilos con `#id`).
- **`class`**: se puede repetir; agrupa elementos para estilos y scripts (estilos con `.clase`).

---

## Estilos por defecto del navegador

Los navegadores aplican **estilos del agente de usuario** (por ejemplo, `<div>` es `display: block`, `<em>` se ve en cursiva). Puedes sobreescribirlos con CSS.

  
---

## Metadatos esenciales

En el `<head>` conviene incluir:

```html

<meta charset="utf-8">

<meta name="viewport" content="width=device-width, initial-scale=1.0">

<title>Título descriptivo</title>

<link rel="icon" href="/favicon.ico" sizes="any">

```


**Otros metadatos útiles:**

```html

<meta name="robots" content="index, follow">

<meta name="theme-color" content="#0ea5e9">

<link rel="canonical" href="https://ejemplo.com/articulo">

<!-- Para previsualizaciones en redes sociales -->

<meta property="og:title" content="Título OG">

<meta property="og:image" content="https://ejemplo.com/imagen.jpg">

<meta name="twitter:card" content="summary_large_image">

```

---

## La etiqueta `style` (CSS embebido)

Puedes incluir CSS dentro de tu HTML:

```html

<style>

  body { font-family: system-ui, sans-serif; }

  h1 { line-height: 1.2; }

</style>

```

En proyectos reales, es mejor enlazar CSS externo con:

```html

<link rel="stylesheet" href="/estilos.css">

```

---

## HTML semántico: estructura típica


```html

<header>

  <h1>Blog de Tecnología</h1>

  <nav aria-label="Principal">

    <a href="/">Inicio</a>

    <a href="/articulos">Artículos</a>

    <a href="/contacto">Contacto</a>

  </nav>

</header>

  

<main id="contenido">

  <article>

    <header>

      <h2>Qué es HTML Semántico</h2>

      <p><small>Artículo de ejemplo</small></p>

    </header>

  

    <p>HTML semántico implica usar el elemento que mejor describe el contenido.</p>

  </article>

  

  <aside>

    <h3>Acerca de</h3>

    <p>Notas, enlaces y contenido secundario.</p>

  </aside>

</main>

  

<footer>

  <p><small>© 2025 Mi Sitio</small></p>

</footer>

```


- `<header>`: cabecera de la página o de una sección.

- `<nav>`: navegación principal o local.

- `<main>`: **contenido principal** (único por documento).

- `<article>`: contenido independiente (post, noticia, tarjeta).

- `<section>`: agrupa contenido temático con encabezado.

- `<aside>`: contenido tangencial (lateral, recuadros).

- `<footer>`: pie de página o sección.

- `<small>`: información secundaria (notas, disclaimers, avisos legales).


---

## Enlaces (`a`) y seguridad: `rel="noopener noreferrer"`


El enlace básico:

```html

<a href="https://ejemplo.com">Ir a ejemplo</a>

```

Para abrir en nueva pestaña y **mejorar seguridad**:

```html

<a href="https://externo.com" target="_blank" rel="noopener noreferrer">Abrir externo</a>

```

- `noopener` evita que la nueva página pueda manipular `window.opener`.

- `noreferrer` evita enviar el encabezado Referer (privacidad).

  

**Enlaces especiales:**

```html

<a href="mailto:usuario@dominio.com">Enviar correo</a>

<a href="tel:+573001234567">Llamar</a>

<a href="/archivo.pdf" download>Descargar PDF</a>

```

---

## `div` y `span`

- `<div>`: contenedor genérico **de bloque** (sin semántica).

- `<span>`: contenedor genérico **en línea** (sin semántica).

Úsalos cuando no exista una etiqueta semántica adecuada.

---
## `role` (ARIA)

El atributo `role` añade semántica accesible cuando el HTML **nativo** no basta. Regla de oro: **usa primero elementos HTML adecuados**; añade `role` solo si es necesario (y sin duplicar semántica ya provista por el elemento).

---

## Elementos en línea vs. de bloque

- **Bloque**: ocupan ancho completo y empiezan en línea nueva (`<div>`, `<p>`, `<section>`, etc.).
- **En línea**: ocupan solo su contenido y se integran en la línea (`<span>`, `<a>`, `<strong>`, etc.).

Estas categorías impactan el **formato** (layout) que aplica CSS.

---
## Formularios: estructura y etiquetas clave

Estructura básica:

```html

<form action="/procesar" method="post">

  <fieldset>

    <legend>Contacto</legend>

  

    <label for="nombre">Nombre</label>

    <input id="nombre" name="nombre" required>

  

    <label for="email">Email</label>

    <input id="email" name="email" type="email" required>

  

    <button type="submit">Enviar</button>

  </fieldset>

</form>

```

- `<form>` agrupa controles y envía datos.
- `<fieldset>` y `<legend>` agrupan temáticamente.
- `<label for="id">` asocia texto accesible al control con ese `id`.
- `<input>` tiene múltiples **tipos** (`text`, `email`, `number`, etc.).

**Validación nativa:**

```html

<input type="email" required>

<input type="number" min="1" max="10">

<input type="text" pattern="[A-Za-z\s]+" title="Solo letras y espacios">

```

**`<datalist>`** (sugerencias para un `<input list="...">`):

```html

<label for="pais">País</label>

<input id="pais" name="pais" list="paises">

  

<datalist id="paises">

  <option value="Colombia"></option>

  <option value="México"></option>

  <option value="España"></option>

</datalist>

```

**`<details>`** (contenido desplegable):

```html

<details>

  <summary>Ver términos y condiciones</summary>

  <p>Contenido detallado…</p>

</details>

```

**Submit: `input` vs `button`**

- `<input type="submit" value="Enviar">` es simple.
- `<button type="submit">Enviar</button>` permite contenido HTML interno (iconos, `<strong>`, etc.).

---

## Multimedia: `<video>` y `<audio>`

```html

<video controls preload="metadata" width="640" poster="/portada.jpg">

  <source src="/clip.webm" type="video/webm">

  <source src="/clip.mp4" type="video/mp4">

  <p>Tu navegador no soporta video. <a href="/clip.mp4">Descargar</a></p>

</video>

  

<audio controls preload="metadata">

  <source src="/audio.ogg" type="audio/ogg">

  <source src="/audio.mp3" type="audio/mpeg">

  <p>Tu navegador no soporta audio.</p>

</audio>

```

**Atributos útiles:** `controls`, `autoplay`, `muted`, `loop`, `preload`, `poster` (video). *Nota:* los navegadores suelen requerir `muted` para permitir `autoplay`.

---
## Rendimiento: `loading="lazy"` en imágenes e iframes

```html

<img src="/foto.jpg" alt="Descripción" loading="lazy" width="800" height="600">

<iframe src="mapa.html" loading="lazy" title="Mapa embebido"></iframe>

```

- Carga diferida para contenido fuera de la vista (mejora **rendimiento**).
- Especifica `width`/`height` para evitar saltos de diseño (*layout shift*).

---

## iframes

```html

<iframe

  src="https://www.ejemplo.com"

  title="Contenido incrustado"

  loading="lazy"

  referrerpolicy="no-referrer"

  sandbox="allow-scripts"

  allow="fullscreen; clipboard-read; clipboard-write">

</iframe>

```

- `sandbox` aplica **restricciones**; añades capacidades de forma explícita.
- `referrerpolicy` controla el encabezado **Referer**.
- `allow` define permisos (Permissions Policy) como fullscreen, portapapeles, etc.


---

## `dialog` (modales nativos)

```html

<button id="abrir">Abrir modal</button>

  

<dialog id="modal">

  <h2>Suscripción</h2>

  <p>Déjanos tu email…</p>

  <form method="dialog">

    <button value="cancel">Cancelar</button>

    <button value="ok">Aceptar</button>

  </form>

</dialog>

  

<script>

  const dlg = document.getElementById('modal');

  document.getElementById('abrir').addEventListener('click', () => dlg.showModal());

</script>

```

El elemento `<dialog>` gestiona foco y fondo de forma nativa (útil para accesibilidad).


---

## Apéndices rápidos


- **Seguridad en enlaces externos**: `target="_blank"` + `rel="noopener noreferrer"`.
- **Convención editorial**: escribe elementos y atributos en **minúsculas**.
- **Evita etiquetas obsoletas** (p. ej., `<center>`); usa CSS para presentación.

---

## Proyecto de ejemplo “todo‑en‑uno”

  

```html

<!doctype html>

<html lang="es">

<head>

  <meta charset="utf-8" />

  <title>Demostración HTML</title>

  <meta name="viewport" content="width=device-width, initial-scale=1.0">

  <meta name="description" content="Demostración de HTML semántico, formularios y multimedia.">

  <meta name="theme-color" content="#0ea5e9">

  <link rel="icon" href="/favicon.ico" sizes="any">

  <link rel="canonical" href="https://ejemplo.com/demostracion">

  <style>

    body { font-family: system-ui, sans-serif; max-width: 72ch; margin: 2rem auto; line-height: 1.6; }

    header, footer, nav, main, article, aside, section { padding-block: .5rem; }

    nav a { margin-right: 1rem; }

    figure { margin: 0; }

  </style>

</head>

<body>

  <header>

    <h1>Demostración HTML</h1>

    <nav aria-label="Principal">

      <a href="#articulo">Artículo</a>

      <a href="#formulario">Formulario</a>

      <a href="#media">Multimedia</a>

    </nav>

  </header>

  

  <main id="contenido">

    <article id="articulo">

      <header>

        <h2>HTML Semántico</h2>

        <p><small>Ejemplo sin marcas de tiempo</small></p>

      </header>

      <p>Este <strong>artículo</strong> ejemplifica el uso de secciones, listas y enlaces.</p>

      <section>

        <h3>Lista</h3>

        <ul>

          <li>Elemento uno</li>

          <li>Elemento dos</li>

        </ul>

      </section>

      <aside>

        <h3>Nota</h3>

        <p><em>Recuerda validar el documento y las formas.</em></p>

      </aside>

    </article>

  

    <section id="formulario">

      <h2>Formulario</h2>

      <form>

        <fieldset>

          <legend>Contacto</legend>

          <label for="nombre">Nombre</label>

          <input id="nombre" name="nombre" required>

  

          <label for="email">Email</label>

          <input id="email" name="email" type="email" required>

  

          <label for="pais">País</label>

          <input id="pais" name="pais" list="paises" />

          <datalist id="paises">

            <option value="Colombia"></option>

            <option value="México"></option>

            <option value="España"></option>

          </datalist>

  

          <details>

            <summary>Ver aviso de privacidad</summary>

            <p>…texto…</p>

          </details>

  

          <button type="submit">Enviar</button>

        </fieldset>

      </form>

    </section>

  

    <section id="media">

      <h2>Multimedia</h2>

      <figure>

        <img src="foto.jpg" alt="Vista panorámica" loading="lazy" width="800" height="533">

        <figcaption><small>Imagen con carga diferida.</small></figcaption>

      </figure>

  

      <video controls preload="metadata" width="480" poster="poster.jpg">

        <source src="clip.mp4" type="video/mp4">

        <p>Tu navegador no soporta video.</p>

      </video>

  

      <p>

        Recurso externo:

        <a href="https://developer.mozilla.org/" target="_blank" rel="noopener noreferrer">MDN Web Docs</a>

      </p>

    </section>

  </main>

  

  <footer>

    <p><small>© 2025 — Demostración HTML</small></p>

  </footer>

</body>

</html>

```

---

## Para seguir practicando

1. Reorganiza un artículo largo con `<section>` y encabezados semánticos.
2. Añade a tu formulario validación nativa (`required`, `pattern`) y un `<datalist>`.
3. Inserta un `<iframe>` con `sandbox` y `referrerpolicy`.
4. Crea un modal con `<dialog>` y ábrelo con `showModal()`.