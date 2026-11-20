# glonys.com

Sitio web de empresa de Glonys. HTML y CSS estáticos, sin JavaScript, sin dependencias, sin analíticas y sin recursos externos.

## Estructura

```
site/                  ← lo único que se publica
  index.html           inicio (inglés)
  privacy.html         política de privacidad
  terms.html           términos de uso
  404.html             página de error (GitHub Pages la sirve sola)
  es/                  versión en español (index, privacy, terms)
  assets/style.css     estilos
  assets/favicon.svg   favicon / logo
  assets/og-image.png  imagen para Open Graph (1200×630)
  robots.txt, sitemap.xml, CNAME, .nojekyll
.github/workflows/pages.yml   despliegue a GitHub Pages
```

Las rutas internas son absolutas (`/assets/...`) para que la 404 funcione en cualquier URL. Para verlo en local:

```sh
cd site && python3 -m http.server 8000   # abrir http://localhost:8000
```

## Pasos para publicarlo

El workflow se ejecuta al hacer push a `main` (o a mano desde la pestaña **Actions**). Fusiona esta rama en `main` primero.

### 1. Activar GitHub Pages

1. En el repo: **Settings → Pages**.
2. En **Build and deployment → Source**, elige **GitHub Actions**.
3. Haz push a `main` (o **Actions → Deploy to GitHub Pages → Run workflow**) y espera a que termine en verde.

### 2. (Recomendado) Verificar el dominio en GitHub

Evita que otra cuenta pueda apropiarse de `glonys.com` en Pages.

1. **Settings de tu cuenta u organización** (no del repo) → **Pages → Add a domain** → `glonys.com`.
2. GitHub te da un registro **TXT** (`_github-pages-challenge-<cuenta>.glonys.com`). Créalo en tu DNS y pulsa **Verify**.

### 3. Registros DNS de glonys.com

En el panel de tu proveedor de DNS, borra cualquier A/AAAA/CNAME anterior de `@` y `www` y crea:

| Tipo  | Nombre | Valor                  |
|-------|--------|------------------------|
| A     | `@`    | `185.199.108.153`      |
| A     | `@`    | `185.199.109.153`      |
| A     | `@`    | `185.199.110.153`      |
| A     | `@`    | `185.199.111.153`      |
| AAAA  | `@`    | `2606:50c0:8000::153`  |
| AAAA  | `@`    | `2606:50c0:8001::153`  |
| AAAA  | `@`    | `2606:50c0:8002::153`  |
| AAAA  | `@`    | `2606:50c0:8003::153`  |
| CNAME | `www`  | `thecom-tech.github.io` |

- Los AAAA (IPv6) son opcionales pero recomendados.
- El CNAME de `www` apunta a `<propietario>.github.io`, **sin** el nombre del repo. Si el repo cambia de propietario, cambia este valor.
- Si usas Cloudflare, deja estos registros en modo **DNS only** (nube gris) al menos hasta que GitHub emita el certificado.

Comprobar (la propagación puede tardar desde minutos hasta 24 h):

```sh
dig glonys.com +noall +answer
dig www.glonys.com +noall +answer
```

### 4. Dominio propio y HTTPS

1. **Settings → Pages → Custom domain**: escribe `glonys.com` y pulsa **Save**. (Con despliegue por Actions, GitHub usa este ajuste; el archivo `site/CNAME` se mantiene como referencia.)
2. Espera a que el **DNS check** salga correcto. GitHub emite el certificado automáticamente (puede tardar hasta 1 h).
3. Marca **Enforce HTTPS**. Si la casilla está gris, el certificado aún no está listo: espera y recarga.
4. Comprueba que `http://glonys.com`, `https://www.glonys.com` y `https://glonys.com/no-existe` redirigen o responden bien.

### 5. Email

El sitio usa `hello@glonys.com` (botón "Request early access" y contacto). Asegúrate de que ese buzón existe y recibe correo (registros MX del dominio) antes de enviar la solicitud a AWS Activate.

## Mantenimiento

- Si cambias la política de privacidad o los términos, actualiza la fecha "Last updated" en las dos versiones (EN y ES).
- Si añades páginas, añádelas también a `site/sitemap.xml` y enlaza su versión en el otro idioma.
