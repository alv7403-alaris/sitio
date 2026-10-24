# CLAUDE.md — sitio grupoalaris.mx

Instrucciones permanentes para Claude al trabajar en este repositorio.
Léelo completo antes de cualquier cambio. El diseño visual está en `DESIGN.md`: léelo también.

## 1. Qué es este sitio

- Sitio de **Grupo Alaris**, empresa de construcción virtual y BIM en Puebla, México. Director general: Arq. Alfonso Leal Vargas (DRO).
- Repositorio `alv7403-alaris/sitio`, publicado con **GitHub Pages** en `grupoalaris.mx` (archivo `CNAME`). El dominio está registrado en Wix; solo los DNS apuntan aquí.
- Sitio **estático**: HTML, CSS y JavaScript sin servidor, sin base de datos y sin paso de compilación. No agregues frameworks (Next.js, React, Vue), gestores de paquetes ni `node_modules` salvo que el Arq. Leal lo pida expresamente.
- Idioma: **español de México** en todo el contenido (`<html lang="es-MX">`). Sistema métrico.

## 2. Mapa del sitio

| Ruta | Contenido | Notas |
|---|---|---|
| `/` | Página de inicio de Grupo Alaris | Página principal; cuidar especialmente |
| `/cv/` | CV del Arq. Leal + PDF | |
| `/socios/` | Alianzas (PICAGED, LBI Group, ITASA) | BEKOSA ya no forma parte: no volver a agregarla |
| `/dro/` | Cuaderno del DRO (beta) + `kit-normativa.zip` | Lleva GoatCounter; ver sección 6 |
| `/9y13/`, `/arboleda/`, `/depas/modelo/` | Páginas de proyecto con visor 3D (.glb) | |
| `/sitio/…` | Tableros privados para clientes (DEPAS, UMMANA, revisión de proyecto) | Excluidos en `robots.txt` |

Cada página es una carpeta con su `index.html`. Las ligas internas usan rutas relativas a la raíz (`/dro/`), terminadas en `/`.

## 3. Reglas de trabajo

1. **Cambios mínimos.** Modifica solo lo que se pidió. No reescribas páginas completas ni "mejores" secciones que no se mencionaron.
2. **Una página = un archivo autocontenido.** Estilos y scripts dentro del mismo `index.html`, salvo imágenes, modelos y descargas.
3. **Usa los tokens de `DESIGN.md`.** No inventes colores, fuentes ni espaciados nuevos. Si una página vieja usa otros, unifícala solo cuando se te pida trabajar esa página.
4. **No borres archivos** (modelos .glb, .zip, PDF, imágenes) sin confirmación explícita.
5. **No publiques datos de clientes** en páginas públicas: montos, contratos, nombres de particulares o direcciones de obra van solo en `/sitio/` y solo si el Arq. Leal lo indica.
6. Al terminar, di en dos o tres líneas qué cambió y en qué archivos.

## 4. Accesibilidad (WCAG 2.2 AA, obligatorio)

Ningún cambio se da por terminado si falla algo de esta lista:

- HTML semántico: un solo `<h1>` por página, jerarquía de títulos sin saltos, `<header>`, `<nav>`, `<main>`, `<footer>`.
- Liga "Saltar al contenido" como primer elemento enfocable.
- Todo se puede usar solo con teclado, en orden lógico, con foco visible (ver `DESIGN.md` §5).
- Contraste mínimo 4.5:1 en texto normal y 3:1 en texto grande e íconos. Respetar las reglas de color de `DESIGN.md` §2 (el rojo `#EC3237` y el oro sobre claro tienen restricciones).
- Toda imagen con `alt` descriptivo; las decorativas con `alt=""`.
- Formularios con `<label>` visible asociado, mensajes de error en texto y no solo con color.
- Objetivos táctiles de al menos 44 × 44 px.
- Visores 3D y tableros: alternativa en texto o imagen, y que no atrapen el foco del teclado.
- Respetar `prefers-reduced-motion` y `prefers-color-scheme`.
- Funcionar sin desbordamiento horizontal a 320 px de ancho y con zoom al 200 %.

Si están instalados los agentes de accesibilidad (`accessibility-agents`), pásales cada página modificada antes de terminar.

## 5. Seguridad

GitHub Pages no permite configurar encabezados HTTP, así que la protección va dentro de cada página:

- **Content-Security-Policy por `<meta>`** en cada `index.html`, permitiendo solo los orígenes que la página realmente usa:

  ```html
  <meta http-equiv="Content-Security-Policy" content="default-src 'self'; img-src 'self' data:; style-src 'self' 'unsafe-inline' https://fonts.googleapis.com; font-src https://fonts.gstatic.com; script-src 'self' 'unsafe-inline' https://cdnjs.cloudflare.com https://cdn.jsdelivr.net https://gc.zgo.at; connect-src 'self' https://*.goatcounter.com; base-uri 'self'; form-action 'self' mailto:">
  ```
  Ajusta la lista a lo que use cada página; no agregues orígenes "por si acaso". (La protección contra incrustar el sitio en otro, `frame-ancestors`, no funciona por `<meta>`; GitHub Pages no la permite.)
- **Referrer**: `<meta name="referrer" content="strict-origin-when-cross-origin">`.
- **Scripts externos** solo de cdnjs.cloudflare.com o cdn.jsdelivr.net, con **versión fija** en la URL y atributo `integrity` (SRI) + `crossorigin="anonymous"`. Nunca `@latest`.
- **Ligas externas** con `target="_blank"` llevan `rel="noopener noreferrer"`.
- **Nunca** pongas en el repositorio llaves de API, contraseñas, tokens ni datos personales de clientes. El repositorio es público.
- **`robots.txt` no protege nada**: solo pide a los buscadores que no indexen `/sitio/`. Cualquiera con la liga puede abrir esos tableros. No publiques ahí información que no deba ver un tercero.
- Sin `innerHTML` con texto que venga de la URL o del usuario; usa `textContent`.
- No uses `eval`, `new Function` ni `document.write`.

## 6. Medición (GoatCounter)

- Se usa GoatCounter (gratuito, sin cookies ni datos personales). No agregar Google Analytics, píxeles de Meta ni otros rastreadores.
- En `/dro/` el código de la cuenta está en la constante `CODIGO` dentro de `index.html`. El medidor solo corre en `grupoalaris.mx`; la copia descargada no envía nada. Conservar ese comportamiento.
- Los eventos existentes (descargas, pestañas, municipio, fichas, lista de control) no se renombran sin avisar, porque se pierde el histórico.

## 7. Rendimiento

- Imágenes en WebP con miniatura `_s.webp`; portada ≤ 250 KB.
- Modelos `.glb` comprimidos (Draco o Meshopt) y con carga diferida: el visor carga hasta que el usuario lo pide o llega a él.
- Fuentes de Google con `display=swap` y `preconnect`.
- Meta de Lighthouse en celular: Rendimiento ≥ 90, Accesibilidad 100, Buenas prácticas ≥ 95, SEO ≥ 95.

## 8. SEO y datos para compartir

Cada página pública lleva:

- `<title>` único (máx. 60 caracteres) y `<meta name="description">` (máx. 155).
- `<link rel="canonical" href="https://grupoalaris.mx/ruta/">`.
- Open Graph: `og:title`, `og:description`, `og:image` (1200 × 630), `og:url`, `og:locale` = `es_MX`.
- En la portada, datos estructurados JSON-LD tipo `LocalBusiness` con teléfono 221 329 6868, correo contacto@grupoalaris.mx y domicilio en Villa Universitaria, Puebla.
- Las páginas de `/sitio/` llevan `<meta name="robots" content="noindex, nofollow">`.

## 9. Antes de dar por terminado un cambio

- [ ] Se ve bien a 320 px, 768 px y 1280 px, en modo oscuro y claro.
- [ ] Recorrido completo con teclado (Tab / Mayús+Tab / Enter / Esc).
- [ ] Sin errores en la consola del navegador.
- [ ] Todas las ligas internas responden (sin 404).
- [ ] CSP de la página incluye exactamente los orígenes que usa.
- [ ] Ningún texto en rojo `#EC3237` menor a 24 px ni oro puro sobre fondo claro.
- [ ] Resumen breve del cambio para el Arq. Leal.

## 10. Datos de contacto (fuente única)

Grupo Alaris · Tel. 221 329 6868 · contacto@grupoalaris.mx
Privada 63 D Oriente 1409 Int. 1A, Villa Universitaria, C.P. 72589, Puebla, Pue.
