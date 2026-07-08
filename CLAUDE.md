# Peña Donostiarras Bartzelona — Web

Web oficial de la peña de la Real Sociedad en Barcelona. Sitio estático de una sola página, sin build ni dependencias: todo el CSS y JS viven inline en `index.html`.

**Producción: https://donostiarrasbartzelona.com/** (dominio propio comprado en Namecheap, DNS apuntando a GitHub Pages, HTTPS forzado). Cualquier cambio que se pida debe acabar publicado ahí: hacer `git add` + `commit` + `push` a `main` al terminar la tarea, no dejarlo solo en local.

## Estructura

- `index.html` — única página. Secciones: hero (slideshow Ken Burns) → stats → nosotros → galería (filtros + lightbox + "ver más") → prensa → historia (timeline) → FAQ (`<details>`) → únete → contacto (formulario → WhatsApp + mapa embebido) → footer.
- `imgs/` — fotos en WebP con nombres URL-safe (`pena-<evento>-<año>-0N.webp`), favicons recortados del escudo (`favicon-*.png`, `apple-touch-icon.png`), logo de navegación (`logo-nav.webp`) y `og-image.jpg` (1200×630). Los originales (jpeg/png de móvil, carpetas por evento) se conservan en `imgs/` como fuente pero **no se referencian desde el HTML ni se suben al repo** (son de varios MB cada carpeta; solo se commitean los `.webp` ya optimizados).
- `CNAME` — necesario para que GitHub Pages sirva el dominio propio. No borrar.
- `robots.txt`, `sitemap.xml`, `llms.txt` — SEO/GEO. El sitemap incluye imágenes.
- Sin framework, sin bundler, sin formularios de backend: el formulario de contacto abre `wa.me` con el mensaje pre-rellenado.

## Datos del negocio (no inventar, verificar antes de cambiar)

- Peña oficial de la Real Sociedad, fundada el **5 de octubre de 2014**. Miembro de **Realaren Lagun Taldeak**.
- Local: **C/ Cantabria 79, Sant Martí, 08020 Barcelona**.
- WhatsApp: **+34 618 433 942** (aparece en varios sitios del HTML; si cambia, buscar y reemplazar todas las ocurrencias).
- Redes: X @DonostiarrasBCN y @DBartzelona · Instagram @donostiarrasbartzelona · blog donostiarrasbartzelona.blogspot.com

## SEO — reglas de mantenimiento

- URL base actual: `https://donostiarrasbartzelona.com/`. Está en: canonical, OG/Twitter, ambos JSON-LD, `sitemap.xml`, `robots.txt` y `llms.txt`. Si el dominio cambiara alguna vez, actualizar todos con `sed` y no olvidar el `CNAME`.
- Hay una etiqueta `<meta name="google-site-verification" ...>` en el `<head>` — es la verificación de la propiedad en Google Search Console (cuenta personal de Ricardo). No quitarla.
- Hay dos bloques JSON-LD en el `<head>`: `SportsClub`+`WebSite` y `FAQPage`. La `FAQPage` debe mantenerse **sincronizada con la FAQ visible** de la sección `#faq` (mismas preguntas y respuestas).
- Cada foto nueva de galería necesita: WebP, `width`/`height` reales, `loading="lazy"`, alt descriptivo en español, `data-cap` para el pie del lightbox y `data-category` (`viajes` | `copa` | `aniversarios` | `encuentros`) para que funcione con los filtros.
- Hitos nuevos van a tres sitios: timeline de `#historia`, `llms.txt` y (si procede) la galería.
- `lastmod` del sitemap: actualizar al tocar contenido. Las `<image:image>` del sitemap deben apuntar a fotos que existan realmente y cuyo contenido coincida con el `<image:title>` — verificarlo viendo la foto, no fiarse del nombre de archivo.

## Convenciones visuales

- Colores en custom properties de `:root` (azul Real `--blue: #0057A8`, fondos oscuros). No hardcodear hex nuevos.
- Tipografías: **Barlow Condensed** (titulares, uppercase) + **Barlow** (texto), Google Fonts.
- Animaciones: elementos con clase `.rev` se revelan con IntersectionObserver (escalonado por hermanos). Todo respeta `prefers-reduced-motion`.
- Ojo: el `backdrop-filter` del nav lo convierte en containing block de sus descendientes `position:fixed` — por eso el menú móvil usa `top/left/width/height` explícitos en vez de `inset:0`. No "simplificar" eso.

## Galería: filtros + "ver más" (`#galeria`)

- Cada `.gal-item` es un `<button data-category="..." data-cap="...">` con una `<img>` dentro y un `<span class="cap">` para el texto corto que se ve al hacer hover.
- Filtros (`.gal-filter[data-filter]`) marcan `gal-hide` en los items que no tocan. El límite de "ver más" (`GAL_PAGE_SIZE = 18` en el JS) marca `gal-more-hide` en los que superan ese número **dentro del filtro activo**; cambiar de filtro resetea el límite. El lightbox (`lbSet`) solo navega entre items sin `gal-hide` ni `gal-more-hide`.
- Para variedad visual, cada evento tiene como máximo una foto "hero" con clase `tall` (vertical, `grid-row:span 2`) o `wide` (panorámica, `grid-column:span 2`); el resto de fotos del evento van sin esas clases.

## Optimización de imágenes (ImageMagick instalado vía Homebrew)

**Usar siempre `magick` con `-auto-orient`, nunca `cwebp` directo** — `cwebp` ignora el tag EXIF de orientación y dejaba las fotos verticales de móvil giradas de lado (le pasó una vez a esta web, ver historial de commits de julio 2026).

```bash
magick "origen.jpg" -auto-orient -resize '1600x1600>' -quality 82 "imgs/pena-<evento>-<año>-0N.webp"   # foto de galería (el '>' evita escalar hacia arriba fotos pequeñas)
magick logo-transparent.png -resize x220 imgs/logo-nav.webp
```

Antes de escribir el `alt`/`data-cap` de una foto nueva, **verla** con la herramienta de lectura de imágenes — nunca inferir el contenido por el nombre de archivo o la carpeta, y nunca describir varias fotos de memoria tras verlas en lote: se ha demostrado que así se cruzan los pies de foto.

## Verificar y publicar

- Probar en local: `python3 -m http.server 8742` y abrir `http://localhost:8742/`. Revisar siempre 375px (menú hamburguesa, lightbox, filtros) y 1440px.
- Publicación: GitHub Pages sirve la rama `main` del repo `ricardoba74/donostiarras-bartzelona`, con dominio propio `donostiarrasbartzelona.com` (registro en Namecheap, DNS: 4 registros A a las IPs de GitHub Pages + CNAME `www` → `ricardoba74.github.io.`; el `TXT` de `@` es del email forwarding de Namecheap, no tocar). Hacer commit + push a `main` publica directamente; comprobar con `gh api repos/ricardoba74/donostiarras-bartzelona/pages/builds/latest` que el build coincide con el último commit.
- **Nunca intentar automatizar el login a Namecheap o Google con credenciales reales del usuario** (riesgo de bloqueo de cuenta, 2FA, y hay alternativa sin riesgo guiando al usuario paso a paso). Si hace falta tocar DNS o Search Console, dar instrucciones exactas y dejar que el usuario haga los clics.

## Pendiente (fuera del código)

- Crear Google Business Profile del local (clave para "peña Real Sociedad Barcelona" en Maps) — probablemente más impacto que el SEO on-page para búsquedas locales.
- Los 4 vídeos de `imgs/VIDEOS ANIVERSARIOS/` (fuente sin procesar, no está en el repo) siguen pendientes de integrar en la web — el usuario los considera muy relevantes.
