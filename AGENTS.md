# Guía de mantenimiento — Aqua Tower

Este repositorio contiene una página estática de enlaces para Aqua Tower. No utiliza framework, compilador ni dependencias: GitHub Pages publica directamente los archivos HTML, CSS, JavaScript e imágenes del repositorio.

## Estructura del proyecto

- `index.html`: contenido, textos, enlaces, metadatos y JavaScript del botón para compartir.
- `styles.css`: diseño responsive, portada, degradados, tarjetas y estilos para móvil.
- `assets/`: imágenes utilizadas por la página.
- `CNAME`: dominio personalizado de GitHub Pages.
- `.nojekyll`: evita que GitHub Pages procese el sitio con Jekyll.
- `.github/workflows/pages.yml`: automatización que publica el sitio.
- `README.md`: resumen breve de publicación y DNS.

## Cómo editar la página

Trabajar siempre desde la raíz:

```powershell
Set-Location C:\dev\LinkTree\xgroup-aquatower
```

Antes de empezar, revisar el estado del repositorio:

```powershell
git status --short --branch
git pull --ff-only
```

### Cambiar textos o enlaces

Editar `index.html`. Los enlaces externos deben conservar:

```html
target="_blank" rel="noopener noreferrer"
```

Enlaces actuales:

- Instagram: `https://www.instagram.com/xgroup.bo`
- WhatsApp: `https://wa.me/59163548333`
- App Store: `https://apps.apple.com/bo/app/xgroup-inmobiliario/id6785610009`
- Ubicación: `https://maps.app.goo.gl/5SXbZUsUExBgqKAq8?g_st=ic`

### Cambiar estilos

Editar `styles.css`. Mantener los estilos móviles dentro de las reglas `@media` del final. Después de modificar la portada, comprobar especialmente:

- Que el logo AQUA TOWER no quede recortado.
- Que no aparezca desplazamiento horizontal.
- Que el texto siga siendo legible sobre el degradado.
- Que las tarjetas no se salgan de la pantalla.
- Que el botón para compartir siga visible y accesible.

### Cambiar imágenes

Guardar las imágenes dentro de `assets/` y utilizar rutas relativas, por ejemplo:

```html
<img src="assets/nueva-imagen.jpg" alt="Descripción de la imagen">
```

Optimizar las imágenes antes de subirlas. Evitar nombres con espacios y archivos innecesariamente grandes. La portada actual es `assets/aqua-edificio.jpeg`, en formato vertical `9:16`; si se reemplaza, conservar esa proporción y mantener el foco visual en la parte superior.

## Cómo ver el sitio localmente

La forma recomendada en Windows es iniciar el servidor HTTP incluido con Python:

```powershell
Set-Location C:\dev\LinkTree\xgroup-aquatower
python -m http.server 4173 --bind 127.0.0.1
```

Abrir en el navegador:

```text
http://127.0.0.1:4173/
```

Detener el servidor con `Ctrl+C`.

No es recomendable abrir `index.html` directamente con `file://`, porque algunas funciones del navegador —como portapapeles, rutas y políticas de seguridad— pueden comportarse de manera diferente.

### Validación antes de publicar

Probar como mínimo:

1. Escritorio, alrededor de `1366 × 768`.
2. Móvil, alrededor de `390 × 844`.
3. Instagram, WhatsApp, App Store y ubicación.
4. Botón para compartir.
5. Ausencia de errores en la consola del navegador.
6. Ausencia de cambios de formato problemáticos:

```powershell
git diff --check
git status --short
```

## Cómo publicar

La publicación normal se realiza haciendo push a la rama `main`:

```powershell
git add index.html styles.css assets AGENTS.md
git commit -m "Describe el cambio realizado"
git push origin main
```

Agregar solamente los archivos relacionados con el cambio. No usar `git add .` si existen imágenes, QR u otros archivos locales que no deben publicarse.

Para consultar el despliegue con GitHub CLI:

```powershell
gh run list --repo equiposesfera/xgroup-aquatower --workflow pages.yml --limit 5
```

Para esperar un despliegue específico:

```powershell
gh run watch ID_DEL_RUN --repo equiposesfera/xgroup-aquatower --exit-status
```

Repositorio:

```text
https://github.com/equiposesfera/xgroup-aquatower
```

Página provisional de GitHub:

```text
https://equiposesfera.github.io/xgroup-aquatower/
```

## Cómo funciona GitHub Pages

El archivo `.github/workflows/pages.yml` define un workflow llamado `Deploy static site to Pages`.

Cuando se envía un commit a `main`:

1. GitHub Actions descarga el repositorio.
2. `actions/configure-pages` prepara GitHub Pages.
3. `actions/upload-pages-artifact` empaqueta el contenido del repositorio.
4. `actions/deploy-pages` publica el artefacto.
5. La página queda disponible cuando el workflow termina con estado `success`.

El sitio es estático: no existe un servidor de aplicación, base de datos ni proceso de compilación. Los cambios publicados son exactamente los archivos presentes en el commit de `main`.

No modificar ni eliminar estos archivos sin entender su función:

- `.github/workflows/pages.yml`
- `.nojekyll`
- `CNAME`

## Dominio personalizado

`CNAME` contiene:

```text
xgroup-aquatower.proyecto.com.bo
```

En el proveedor DNS de `proyecto.com.bo` debe existir:

```text
Tipo: CNAME
Nombre: xgroup-aquatower
Destino: equiposesfera.github.io
```

El DNS no se administra desde este repositorio. Si el dominio deja de funcionar, comprobar:

1. Que el archivo `CNAME` conserve el dominio correcto.
2. Que GitHub muestre el dominio en **Settings → Pages**.
3. Que el registro DNS siga apuntando a `equiposesfera.github.io`.
4. Que el último workflow haya terminado correctamente.
5. Que **Enforce HTTPS** esté activado cuando GitHub lo permita.

Los cambios DNS pueden tardar en propagarse. Mientras tanto, la URL `github.io` sirve para verificar el despliegue.

## Recuperación y buenas prácticas

- No reescribir el historial de `main` ni hacer `push --force`.
- No borrar `.git`, el workflow, `CNAME` o `.nojekyll`.
- Revisar `git status` antes de cada commit.
- Mantener textos e imágenes propiedad de Aqua Tower/Xgroup o con autorización de uso.
- Si un despliegue falla, abrir la ejecución en la pestaña **Actions** del repositorio y revisar el primer paso marcado en rojo.
- Para revertir un cambio publicado, crear un commit de reversión en vez de alterar el historial:

```powershell
git revert ID_DEL_COMMIT
git push origin main
```
