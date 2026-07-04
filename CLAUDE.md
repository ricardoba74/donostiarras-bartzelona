# Peña Donostiarras Bartzelona — Web

Web oficial de la peña de la Real Sociedad en Barcelona. Sitio estático de una sola página, sin build ni dependencias: todo el CSS y JS viven inline en `index.html`.

## Estructura

- `index.html` — única página. Secciones: hero (slideshow Ken Burns) → stats → nosotros → galería (lightbox) → prensa → historia (timeline) → FAQ (`<details>`) → únete → contacto (formulario → WhatsApp + mapa embebido) → footer.
- `imgs/` — fotos en WebP con nombres URL-safe (`pena-*.webp`), favicons recortados del escudo (`favicon-*.png`, `apple-touch-icon.png`), logo de navegación (`logo-nav.webp`) y `og-image.jpg` (1200×630). Los originales (`Peña*.jpeg`, `logo*.png`) se conservan como fuente pero **no se referencian desde el HTML**.
- `robots.txt`, `sitemap.xml`, `llms.txt` — SEO/GEO. El sitemap incluye imágenes.
- Sin framework, sin bundler, sin formularios de backend: el formulario de contacto abre `wa.me` con el mensaje pre-rellenado.

## Datos del negocio (no inventar, verificar antes de cambiar)

- Peña oficial de la Real Sociedad, fundada el **5 de octubre de 2014**. Miembro de **Realaren Lagun Taldeak**.
- Local: **C/ Cantabria 79, Sant Martí, 08020 Barcelona**.
- WhatsApp: **+34 618 433 942** (aparece en varios sitios del HTML; si cambia, buscar y reemplazar todas las ocurrencias).
- Redes: X @DonostiarrasBCN y @DBartzelona · Instagram @donostiarrasbartzelona · blog donostiarrasbartzelona.blogspot.com

## SEO — reglas de mantenimiento

- URL base actual: `https://ricardoba74.github.io/donostiarras-bartzelona/` (GitHub Pages, sin dominio propio de momento). Está en: canonical, OG/Twitter, ambos JSON-LD, `sitemap.xml`, `robots.txt` y `llms.txt`. **Si algún día hay dominio propio, actualizar todos.**
- Hay dos bloques JSON-LD en el `<head>`: `SportsClub`+`WebSite` y `FAQPage`. La `FAQPage` debe mantenerse **sincronizada con la FAQ visible** de la sección `#faq` (mismas preguntas y respuestas).
- Cada foto nueva de galería necesita: WebP, `width`/`height` reales, `loading="lazy"`, alt descriptivo en español y `data-cap` para el pie del lightbox.
- Hitos nuevos van a tres sitios: timeline de `#historia`, `llms.txt` y (si procede) la galería.
- `lastmod` del sitemap: actualizar al tocar contenido.

## Convenciones visuales

- Colores en custom properties de `:root` (azul Real `--blue: #0057A8`, fondos oscuros). No hardcodear hex nuevos.
- Tipografías: **Barlow Condensed** (titulares, uppercase) + **Barlow** (texto), Google Fonts.
- Animaciones: elementos con clase `.rev` se revelan con IntersectionObserver (escalonado por hermanos). Todo respeta `prefers-reduced-motion`.
- Ojo: el `backdrop-filter` del nav lo convierte en containing block de sus descendientes `position:fixed` — por eso el menú móvil usa `top/left/width/height` explícitos en vez de `inset:0`. No "simplificar" eso.

## Optimización de imágenes (cwebp e ImageMagick instalados vía Homebrew)

```bash
cwebp -q 82 foto.jpg -o imgs/pena-nombre.webp        # foto de galería
magick logo-transparent.png -resize x220 imgs/logo-nav.webp
```

## Verificar y publicar

- Probar en local: `python3 -m http.server 8742` y abrir `http://localhost:8742/`. Revisar siempre 375px (menú hamburguesa, lightbox) y 1440px.
- Publicación: GitHub Pages sirve la rama `main` del repo `ricardoba74/donostiarras-bartzelona` — hacer commit + push publica directamente.

## Pendiente (fuera del código)

- Alta en Google Search Console + envío del sitemap.
- Crear Google Business Profile del local (clave para "peña Real Sociedad Barcelona" en Maps).
- Valorar compra de dominio propio (p. ej. `donostiarrasbartzelona.com`) → CNAME en GitHub Pages + actualizar URLs.
