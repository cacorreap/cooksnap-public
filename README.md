# CookSnap — descarga pública

Página de descarga y distribución del APK de **CookSnap** (recetario con IA para Android).
El código fuente de la app vive en un repositorio aparte; aquí solo está la web de descarga.

- **Página:** https://cacorreap.github.io/cooksnap-public/
- **Descarga directa (último APK):** https://github.com/cacorreap/cooksnap-public/releases/latest/download/cooksnap.apk
- **Privacidad:** https://cacorreap.github.io/cooksnap-public/privacy.html
- **Eliminación de datos:** https://cacorreap.github.io/cooksnap-public/eliminar-datos.html

## Cómo publicar una versión nueva

El APK **no** se sube a git (está en `.gitignore`). Se publica como asset de un **Release**:

1. Genera el APK (`eas build --profile preview --platform android`) y renómbralo a `cooksnap.apk`.
2. En GitHub → pestaña **Releases** → **Draft a new release**.
3. Crea un tag (ej. `v1.0.0`), arrastra el `cooksnap.apk` como asset y publica.

El link de descarga de la página siempre apunta al **último** release (`releases/latest`),
así que no hay que tocar el HTML al actualizar.

## Activar GitHub Pages

GitHub → **Settings → Pages** → Source: **Deploy from a branch** → rama `main`, carpeta `/ (root)`.
El archivo `.nojekyll` evita que GitHub procese el sitio con Jekyll.

## Estructura

- `index.html` — página de descarga.
- `privacy.html` — política de privacidad.
- `eliminar-datos.html` — eliminación de datos.
- `assets/` — icono y logo.
