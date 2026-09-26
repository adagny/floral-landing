# Creaciones Florales Flora

Landing page de una página (mobile-first) para **Creaciones Florales Flora**, un negocio de arreglos florales de limpiapipas hechos a mano. Incluye un **panel de administración** (Decap CMS) para que la dueña actualice textos y fotos sin tocar código.

Hecho con **HTML5 + Tailwind CSS (CDN) + JavaScript vanilla**. Sin paso de compilación: son archivos estáticos.

## Estructura

```
.
├── index.html            # La página (lee el contenido desde content/site.json)
├── content/
│   └── site.json         # Todo el contenido editable (textos, números, galería)
├── images/               # Fotos subidas desde el panel /admin
└── admin/
    ├── index.html        # Panel de administración (Decap CMS)
    └── config.yml        # Configuración del CMS (campos, backend, imágenes)
```

## Ver la página en local

Necesitas servirla por HTTP (no abrir el archivo con `file://`, porque lee `content/site.json` con `fetch`).

```bash
npm start          # sirve el sitio en http://localhost:4173
# o, sin Node:
python3 -m http.server 4173
```

Luego abre http://localhost:4173/index.html

## Editar el contenido con el panel /admin

### Opción A — Editar en local (rápido, sin login)

En **dos terminales**, dentro de la carpeta del proyecto:

```bash
# Terminal 1: sirve el sitio
npm start

# Terminal 2: servidor local del CMS (guarda los cambios en tus archivos)
npm run cms
```

Abre http://localhost:4173/admin/ y pulsa **Login**. Los cambios que guardes se
escriben en `content/site.json` (y las fotos en `images/`). Esto funciona gracias
a `local_backend: true` en `admin/config.yml`.

### Opción B — Editar en producción (sitio publicado)

El panel guarda los cambios como commits en GitHub. Elige un método de autenticación:

- **Netlify (lo más simple para alguien no técnico):** publica el sitio en Netlify,
  activa **Netlify Identity** y **Git Gateway**, e invita a la dueña por correo. En
  `admin/config.yml` cambia el backend a:
  ```yaml
  backend:
    name: git-gateway
    branch: main
  ```
- **GitHub OAuth:** mantén `backend: name: github` (ya configurado con
  `repo: adagny/floral-landing`) y usa un pequeño proxy OAuth
  (por ejemplo desplegando `decap-server`/un OAuth provider) para el login con GitHub.

## Cómo se actualiza el contenido (resumen)

- **Textos** (portada, "Sobre Mí", estado del taller, lote): se editan en el panel o
  directamente en `content/site.json`.
- **Fotos de la galería:** sube la imagen desde el panel (campo "Foto") o coloca el
  archivo en `images/` y pon la ruta en `content/site.json`. Si un producto no tiene
  foto, se muestra una flor dibujada automáticamente.
- **Número de WhatsApp:** se cambia una sola vez en `content/site.json`
  (sección `whatsapp`) y se aplica a todos los botones.

## Publicar el sitio

Al ser estático, puedes usar cualquier hosting: **Netlify**, **GitHub Pages** o
**Cloudflare Pages**. Sube el contenido del repositorio tal cual (la raíz del sitio
es este directorio).
