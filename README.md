# Fundación Sonriente — Sitio Web

Sitio institucional de la **Fundación Sonriente – Alfonso Rojas** (NIT 901.120.906‑0).
Está construido con **Jekyll** + **Tailwind CSS v4** + **Flowbite**, se despliega automáticamente en **GitHub Pages** y está publicado en el dominio propio [**fundacionsonriente.com**](https://fundacionsonriente.com).

> "¡Sonreír es Gratis!" — Construimos sonrisas que cambian vidas desde 2015.

---

## Tabla de contenido

1. [Stack técnico](#stack-técnico)
2. [Estructura del proyecto](#estructura-del-proyecto)
3. [Cómo correr el sitio en local](#cómo-correr-el-sitio-en-local)
4. [Editar contenido sin tocar código (YAML)](#editar-contenido-sin-tocar-código-yaml)
5. [Cambiar imágenes y PDFs](#cambiar-imágenes-y-pdfs)
6. [Editar las páginas](#editar-las-páginas)
7. [Sistema de diseño](#sistema-de-diseño)
8. [URLs limpias](#urls-limpias)
9. [Despliegue y dominio](#despliegue-y-dominio)
10. [Errores comunes](#errores-comunes)

---

## Stack técnico

| Pieza | Para qué sirve |
|-------|----------------|
| **Jekyll 4** | Generador de sitios estáticos. Convierte plantillas + datos en HTML. |
| **Tailwind CSS v4** | Sistema de estilos por utilidades. Compila en build. |
| **Flowbite** | Componentes base (navbar mobile, carrusel, etc.). |
| **Liquid** | Lenguaje de plantillas que usa Jekyll (`{% for %}`, `{% include %}`, `{{ variable }}`). |
| **PostCSS + cssnano + autoprefixer** | Optimización de CSS. |
| **GitHub Pages** | Hosting estático gratuito desde el repo (rama `gh-pages`). |
| **GitHub Actions** | CI que compila Jekyll y publica en `gh-pages` en cada push a `main`. |

---

## Estructura del proyecto

```
fundacionsonriente.com/
├─ _config.yml              # Configuración global de Jekyll
├─ CNAME                    # Dominio propio para GitHub Pages
├─ Gemfile / Gemfile.lock   # Dependencias Ruby (Jekyll)
├─ package.json             # Dependencias Node (Tailwind, Flowbite, PostCSS)
├─ postcss.config.js        # Pipeline de CSS
├─ tailwind.config.js       # Configuración Tailwind
│
├─ _layouts/
│   └─ default.html         # Plantilla base de TODAS las páginas (head, navbar, footer, scripts)
│
├─ _includes/               # Partials reutilizables
│   ├─ navbar.html          # Navbar sticky con liquid glass
│   ├─ footer.html          # Footer con franja tricolor y datos institucionales
│   ├─ carousel.html        # Carrusel Flowbite (toma de _data/carousel.yml)
│   ├─ gallery_item.html    # Tarjeta de imagen para la galería
│   ├─ programa_card.html   # Tarjeta de programa
│   ├─ team_card.html       # Tarjeta de integrante del equipo
│   └─ documento_card.html  # Tarjeta de descarga de documento oficial
│
├─ _data/                   # Contenido editable sin tocar HTML
│   ├─ team.yml             # Equipo directivo
│   ├─ programas.yml        # Programas activos
│   ├─ gallery.yml          # Imágenes de la galería
│   ├─ documentos.yml       # Documentos oficiales (transparencia)
│   └─ carousel.yml         # Slides del carrusel
│
├─ _actividades/            # Colección (Jekyll) de actividades/eventos próximos
│
├─ assets/
│   ├─ css/main.css         # Hoja única de estilos (Tailwind + tokens + componentes)
│   ├─ img/                 # Fotos (actividades_*, fotos del equipo, logo, etc.)
│   └─ pdf/                 # Documentos oficiales descargables
│
├─ index.html               # Inicio
├─ nosotros.html            # Quiénes somos, misión, visión, equipo, valores
├─ estados-financieros.html # Transparencia: cifras + documentos + datos legales
├─ donaciones.html          # Información para donar
├─ voluntariado.html        # Información para voluntarios
├─ actividades.html         # Listado de programas
├─ calendario.html          # Próximos eventos
├─ galeria.html             # Galería de fotos
├─ contactanos.html         # Formulario y datos de contacto
│
└─ _site/                   # Salida compilada (NO se edita, Jekyll la regenera)
```

---

## Cómo correr el sitio en local

### Prerrequisitos (solo la primera vez)

- **Ruby ≥ 3.0** y **Bundler**: `gem install bundler`
- **Node ≥ 18** y **npm**
- Dependencias del proyecto:
  ```bash
  bundle install        # Instala Jekyll y plugins (lee Gemfile)
  npm install           # Instala Tailwind, PostCSS, Flowbite (lee package.json)
  ```

### Levantar el servidor de desarrollo

```bash
npm run serve
```

Eso ejecuta `bundle exec jekyll serve --livereload`. Abre [http://localhost:4000](http://localhost:4000).
Cambia cualquier archivo `.html` / `.yml` / `.css` y el navegador refresca solo.

### Otros comandos

```bash
npm run build        # Build estándar
npm run build:prod   # Build con JEKYLL_ENV=production (CSS minificado por cssnano)
```

---

## Editar contenido sin tocar código (YAML)

La regla es: **si quieres cambiar lo que dice o aparece en una sección, primero busca el `.yml` en `_data/`**. El HTML solo arma la estructura; los datos vienen de YAML.

### Equipo — `_data/team.yml`

```yaml
- name: Neyer Mauricio Turriago Gutiérrez
  description: Fundador y Director Ejecutivo
  img: mauricio_turriago.png       # debe existir en assets/img/
  info: Lidera la fundación desde 2015 con la convicción de que una sonrisa transforma vidas.
```

- Para agregar a alguien nuevo, sube la foto a `assets/img/` y añade un bloque.
- El partial `_includes/team_card.html` se encarga del diseño.

### Programas — `_data/programas.yml`

```yaml
- title: Programa Clown
  description: Llevamos alegría con payasitos hospitalarios...
  icon_color: red                  # blue | yellow | red | mint | peach
  icon: heart                      # ver _includes/programa_card.html
```

### Galería — `_data/gallery.yml`

```yaml
- img: actividades_1.png           # ruta dentro de assets/img/
  alt: Niños y niñas en una jornada del Programa Clown
  programa: Programa Clown         # opcional (categoría que aparece al hover)
  span: tall                       # tall | wide | (vacío = cuadrada)
```

`span` controla el tamaño de la celda en el grid masonry:
- `tall` → ocupa 2 filas verticalmente
- `wide` → ocupa 2 columnas horizontalmente
- vacío → cuadrada (1×1)

### Documentos oficiales — `_data/documentos.yml`

Documentos de la página de Transparencia. Para agregar uno:

1. Sube el archivo a `assets/pdf/` (puede ser PDF o DOCX).
2. Añade un bloque al YAML:

```yaml
- title: "Estatutos de la fundación"
  description: "Acta del máximo órgano directivo con los estatutos..."
  file: "ESTATUTOS.pdf"             # nombre exacto del archivo en assets/pdf/
  cta: "Descargar estatutos"
  tag: "Vigente"                    # texto del badge superior derecho
  tag_style: "quiet"                # default | quiet | yellow
  icon: "book"                      # file | doc | cert | book | vision
  icon_style: "default"             # default | yellow | red
```

El nombre del archivo puede tener espacios y tildes — Jekyll los codifica automáticamente vía `url_encode`.

### Carrusel — `_data/carousel.yml`

```yaml
- img: nombre.png        # debe estar en assets/img/carousel/
  label: Texto accesible
```

---

## Cambiar imágenes y PDFs

| Tipo | Dónde va | Cómo se referencia |
|------|----------|---------------------|
| Fotos generales | `assets/img/` | En YAML: solo el nombre (`actividades_1.png`). En HTML: `{{ "/assets/img/archivo.png" \| relative_url }}` |
| Fotos del carrusel | `assets/img/carousel/` | Solo el nombre en `carousel.yml` |
| PDFs | `assets/pdf/` | Solo el nombre en `documentos.yml` |
| Logo / favicon | `assets/img/logo.png` y `favicon.ico` | Referenciados en `_layouts/default.html` y `_includes/footer.html` |

> ⚠️ Las rutas en HTML SIEMPRE usan el filtro `| relative_url` (o `relative_url` después de `append`), nunca rutas absolutas crudas. Es lo que hace que el sitio funcione tanto en `localhost` como en producción.

---

## Editar las páginas

Cada página vive en la raíz como un `.html` con front matter (las líneas entre `---` al inicio):

```yaml
---
layout: default                    # usa _layouts/default.html
title: "Transparencia"             # aparece en <title> y como h1 si la página lo usa
description: "Cifras financieras..." # aparece en <meta name="description"> (SEO)
permalink: /                       # ruta del archivo en el sitio (solo si quieres sobrescribir)
---
```

Por defecto, todas las páginas usan permalink **limpio** (sin `.html`):
- `nosotros.html` → `https://fundacionsonriente.com/nosotros/`
- `estados-financieros.html` → `https://fundacionsonriente.com/estados-financieros/`

(definido en `_config.yml` con `permalink: /:basename/`).

### Patrón típico de una sección

```html
<section class="section-white">
  <div class="container-prose py-20 lg:py-28">
    <span class="eyebrow">Título pequeño</span>
    <h2 class="mt-4 text-3xl sm:text-4xl lg:text-5xl font-extrabold text-ink-900">
      Título principal
    </h2>
    <p class="mt-5 text-ink-500">Descripción</p>
    <!-- contenido -->
  </div>
</section>
```

---

## Sistema de diseño

Todo el CSS está en `assets/css/main.css`. Usa el sistema `@theme` de Tailwind v4 para tokens y `@layer components` para clases compuestas.

### Paleta institucional

| Token | HEX | Uso |
|-------|-----|-----|
| `brand-blue` | `#1860c4` | Azul institucional principal |
| `brand-yellow` | `#f3b50a` | Amarillo de acento (CTAs, highlights) |
| `brand-red` | `#d72828` | Rojo institucional |
| `brand-blue-soft` | `#e8f1ff` | Azul muy claro para fondos cálidos |
| `ink-50 → ink-900` | grises | Texto y fondos neutros |

### Componentes (clases reutilizables)

| Clase | Descripción |
|-------|-------------|
| `.btn .btn-accent` | Botón primario (amarillo/azul) |
| `.btn .btn-ghost` | Botón secundario (borde) |
| `.btn-link` | Link tipo "Ver más →" |
| `.card` | Tarjeta blanca con borde y sombra suave |
| `.card-blue` / `.card-yellow` / `.card-red` / `.card-mint` / `.card-peach` | Tarjeta con fondo de color |
| `.icon-chip` (`-yellow`, `-red`, `-solid-blue`, etc.) | Cuadradito redondeado con icono |
| `.eyebrow` | Mini título en uppercase espaciado |
| `.section-white` / `-soft` / `-warm` / `-cool` / `-festive` | Variantes de sección con fondo |
| `.tag` (`-yellow`, `-quiet`) | Badge pequeño |
| `.container-prose` | Contenedor con max-width y padding horizontal |

### Animaciones disponibles

| Clase | Efecto |
|-------|--------|
| `.animate-fade-up` | Aparece desde abajo con fade |
| `.animate-fade-in` | Fade simple |
| `.animate-float` | Flotación vertical suave (6s) |
| `.animate-float-slow` | Flotación + rotación leve (7s) |
| `.animate-drift` | Desplazamiento diagonal (8s) |
| `.animate-wiggle` | Bamboleo (3.5s) |
| `.animate-spin-slow` | Giro lento (18s) |
| `.animate-bounce-soft` | Rebote suave (2.4s) |
| `.animate-pulse-soft` | Pulsación de opacidad + escala |
| `[data-reveal]` | Aparece al entrar al viewport (scroll-reveal vía IntersectionObserver) |

Modificadores de retraso: `.delay-100 / -200 / -300 / -500 / -700 / -1000`.
Todas respetan `prefers-reduced-motion`.

### Navbar con liquid glass

El `#site-nav` tiene `backdrop-filter: blur(20px) saturate(180%)` y se vuelve más opaco al scrollear (clase `is-scrolled` añadida por el script al final de `_includes/navbar.html`).

---

## URLs limpias

Configurado en `_config.yml`:

```yaml
defaults:
  - scope:
      path: ""
      type: pages
    values:
      permalink: /:basename/
```

Eso genera `nombre/index.html` en lugar de `nombre.html`, lo que produce URLs sin extensión:

```
/                  → index.html
/nosotros/         → nosotros.html
/estados-financieros/
/donaciones/
/voluntariado/
/actividades/
/calendario/
/galeria/
/contactanos/
```

**Importante**: en código nunca escribir `/nosotros.html`. Siempre `{{ "/nosotros" | relative_url }}`.

---

## Despliegue y dominio

### Cómo se publica el sitio

1. Haces push a la rama `main` en GitHub.
2. GitHub Actions (`.github/workflows/...`) ejecuta `bundle exec jekyll build` y publica el `_site` resultante en la rama `gh-pages`.
3. GitHub Pages sirve la rama `gh-pages` en el dominio configurado.

Cualquier cambio en `main` está online en ~30–60 segundos.

### Dominio propio

Configurado en `_config.yml`:

```yaml
url: 'https://fundacionsonriente.com'
baseurl: ''
```

Y un archivo `CNAME` en la raíz con el contenido:

```
fundacionsonriente.com
```

En el panel del registrar del dominio están configurados:
- 4 registros **A** en el apex (`@`) apuntando a IPs de GitHub Pages: `185.199.108.153`, `185.199.109.153`, `185.199.110.153`, `185.199.111.153`
- Un registro **CNAME** en `www` apuntando a `fundacionsonriente.github.io`

En el repo → `Settings → Pages`: el dominio personalizado está configurado a `fundacionsonriente.com` con **Enforce HTTPS** activado. El certificado SSL lo emite GitHub vía Let's Encrypt y se renueva solo.

### Si algún día cambias de dominio

1. Edita `_config.yml`: `url` y, si fuera necesario, `baseurl`.
2. Reemplaza el contenido del archivo `CNAME`.
3. Actualiza los DNS del registrar.
4. Actualiza el custom domain en `Settings → Pages`.

---

## Errores comunes

| Síntoma | Causa probable | Solución |
|---------|----------------|----------|
| El CSS no carga en producción | Path absoluto sin `relative_url` | Usa siempre `{{ "/ruta" \| relative_url }}` |
| 404 al hacer click en un link | Falta la barra final en la URL | Las rutas son `/nosotros/`, no `/nosotros` |
| Error al hacer build YAML | Hay un `:` dentro de un string sin comillas | Envolver el string en `"..."` |
| El puerto 4000 está ocupado | Ya hay otro `jekyll serve` corriendo | `lsof -i :4000` y mata el proceso, o usa `--port 4001` |
| Cambios no aparecen | Caché del navegador o de Fastly (GitHub Pages) | Refresca con `Ctrl+Shift+R` o espera ~5 min |
| Tildes en nombre de archivo no funcionan | Falta codificación | Usa el filtro `| url_encode` (ya lo hace `documento_card.html`) |
| Card flotante se sale en mobile | Falta `hidden md:block` | Añadir la clase para ocultar en pantallas pequeñas |

---

## Contacto y créditos

- **Fundación Sonriente — Alfonso Rojas** · NIT 901.120.906‑0
- **Sede**: Calle 6 sur # 23 - 51, Santa María 2, Multifamiliar 10, casa 16 · Villavicencio, Meta
- **Tel**: (+57) 321 997 2131
- **Email**: fundacionsonriente@hotmail.com
- **Redes**: [@fundacionsonriente](https://www.instagram.com/fundacionsonriente/)
