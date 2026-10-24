# DESIGN.md — Grupo Alaris (grupoalaris.mx)

Sistema de diseño del sitio. Claude lo lee antes de crear o modificar cualquier página.
Si una página nueva contradice este archivo, gana este archivo.

## 1. Carácter

- Construcción virtual: precisión, datos medibles, obra real. "Solo se paga lo que está en el modelo".
- Esquema oscuro por defecto, acento oro, alto contraste, tipografía firme.
- Sobrio y técnico, no decorativo. Nada de degradados de colores, sombras exageradas ni ilustraciones genéricas.
- La imagen principal siempre es obra o modelo propio (render, foto de obra, visor 3D), nunca foto de stock.

## 2. Color

### Paleta de marca (no se modifica)

| Token | Hex | Uso |
|---|---|---|
| `--negro` | `#201E1F` | Fondo principal en oscuro, texto en claro |
| `--oro` | `#D1AE6C` | Acento principal |
| `--rojo` | `#EC3237` | Acento de marca: logotipo, filetes, íconos, cifras grandes |

### Tokens de interfaz — modo oscuro (por defecto)

| Token | Hex | Uso | Contraste |
|---|---|---|---|
| `--bg` | `#201E1F` | Fondo de página | — |
| `--panel` | `#2A2728` | Tarjetas y paneles | — |
| `--line` | `#3D393A` | Bordes y divisores | — |
| `--ink` | `#EDEAE4` | Texto principal | 13.8:1 |
| `--muted` | `#A8A29A` | Texto secundario | 6.5:1 sobre bg, 5.9:1 sobre panel |
| `--accent` | `#D1AE6C` | Enlaces, títulos de acento, foco | 7.9:1 sobre bg |
| `--red-text` | `#FF5A5F` | Rojo cuando es texto pequeño | 4.9:1 sobre panel |
| `--red-fill` | `#C8252B` | Fondo de botón rojo (texto blanco) | 5.6:1 |

### Tokens de interfaz — modo claro

| Token | Hex | Uso | Contraste |
|---|---|---|---|
| `--bg` | `#EDEAE4` | Fondo de página | — |
| `--panel` | `#E2DED6` | Tarjetas | — |
| `--line` | `#CFC9BF` | Bordes | — |
| `--ink` | `#201E1F` | Texto principal | 13.8:1 |
| `--muted` | `#5B5652` | Texto secundario | 6.0:1 |
| `--accent` | `#7A5F2A` | Oro oscurecido para texto y enlaces | 5.0:1 |
| `--red-text` | `#B8161C` | Rojo para texto | 5.5:1 |
| `--red-fill` | `#C8252B` | Fondo de botón rojo | 5.6:1 con blanco |

### Reglas de color (obligatorias)

1. `#EC3237` **nunca** se usa como texto menor a 24 px (o 19 px en negritas): da 4.0:1 sobre negro. Para texto pequeño usar `--red-text`.
2. **Nunca** texto blanco sobre `#EC3237` (4.1:1). Botones rojos usan `--red-fill`.
3. **Nunca** `#D1AE6C` como texto sobre fondo claro (1.75:1). En modo claro el oro de texto es `--accent` (`#7A5F2A`); el oro puro solo como relleno con texto negro encima (7.9:1).
4. El color nunca es el único portador de significado: estados (avance, retraso, aprobado) llevan también ícono o texto.
5. Colores de estado (tableros de obra): ok `#7FB77E`, alerta `#E0A44B`, crítico `--red-text`; siempre con etiqueta de texto.

### Implementación

```css
:root{--bg:#201E1F;--panel:#2A2728;--line:#3D393A;--ink:#EDEAE4;--muted:#A8A29A;
  --accent:#D1AE6C;--red-text:#FF5A5F;--red-fill:#C8252B;--rojo:#EC3237;--oro:#D1AE6C;--negro:#201E1F}
@media (prefers-color-scheme: light){:root:not([data-theme="dark"]){
  --bg:#EDEAE4;--panel:#E2DED6;--line:#CFC9BF;--ink:#201E1F;--muted:#5B5652;
  --accent:#7A5F2A;--red-text:#B8161C}}
:root[data-theme="light"]{--bg:#EDEAE4;--panel:#E2DED6;--line:#CFC9BF;--ink:#201E1F;
  --muted:#5B5652;--accent:#7A5F2A;--red-text:#B8161C}
body{background:var(--bg);color:var(--ink)}
```

## 3. Tipografía

Una sola familia de sistema para todo el sitio (hoy hay cuatro; unificar al tocar cada página):

| Rol | Familia | Pesos | Uso |
|---|---|---|---|
| Títulos | Archivo (ancho variable) | 700–800 | H1–H3, cifras de portada |
| Texto | IBM Plex Sans | 400, 500, 600 | Párrafos, interfaz, formularios |
| Datos | IBM Plex Mono | 400, 500 | Cantidades, claves (ALR-2026-006), unidades, tablas |

```html
<link rel="preconnect" href="https://fonts.googleapis.com">
<link rel="preconnect" href="https://fonts.gstatic.com" crossorigin>
<link href="https://fonts.googleapis.com/css2?family=Archivo:wdth,wght@62..125,500..800&family=IBM+Plex+Sans:wght@400;500;600&family=IBM+Plex+Mono:wght@400;500&display=swap" rel="stylesheet">
```

- Escala (rem): 0.875 · 1 · 1.125 · 1.375 · 1.75 · 2.25 · 3 · `clamp(2.5rem, 6vw, 4.5rem)` para H1 de portada.
- Texto base 1 rem (16 px), interlineado 1.6; títulos 1.1–1.2.
- Ancho de párrafo máximo 68 caracteres (`max-width: 68ch`).
- Cifras en tablas con `font-variant-numeric: tabular-nums`.
- Unidades siempre con espacio y en sistema métrico: `125.40 m²`, `3.5 t`, `18 m³`.

## 4. Espaciado, retícula y forma

- Escala de espaciado (px): 4 · 8 · 12 · 16 · 24 · 32 · 48 · 64 · 96.
- Contenedor máximo 1200 px; margen lateral 16 px en celular, 24 px en tableta, 32 px en escritorio.
- Radio de esquina: 4 px en controles, 8 px en tarjetas. Nada redondeado tipo "píldora" salvo etiquetas de estado.
- Sombras: casi nunca. La separación se logra con `--line` y `--panel`.
- Puntos de quiebre: 600 px, 900 px, 1200 px. Diseñar primero para celular (el cliente revisa avance desde el teléfono).

## 5. Componentes

- **Botón primario**: fondo `--accent` (oro), texto `--negro`, 600, alto mínimo 44 px.
- **Botón secundario**: borde 1 px `--accent`, texto `--accent`, fondo transparente.
- **Botón de alerta o llamada fuerte**: fondo `--red-fill`, texto blanco.
- **Enlace**: `--accent`, subrayado visible (no solo color).
- **Foco de teclado**: `outline: 2px solid var(--accent); outline-offset: 2px;` en todo elemento interactivo. Nunca `outline: none` sin reemplazo.
- **Tarjeta de proyecto**: imagen 16:9 con `alt` descriptivo, clave del proyecto en Plex Mono, nombre en Archivo, municipio y estatus.
- **Tabla de datos**: encabezado con `<th scope="col">`, filas alternas con `--panel`, cifras alineadas a la derecha.
- **Banner de beta** (/dro/, revisión de proyecto): franja superior `--panel` con borde izquierdo `--accent`, texto "Versión beta" y liga a contacto@grupoalaris.mx.
- **Visor 3D** (.glb): siempre con texto alternativo y liga de descarga o imagen fija para quien no pueda usar el visor.
- **Pie de página**: Grupo Alaris · tel. 221 329 6868 · contacto@grupoalaris.mx · Privada 63 D Oriente 1409 Int. 1A, Villa Universitaria, C.P. 72589, Puebla, Pue.

## 6. Imágenes

- Formato WebP, con versión ligera `_s.webp` para miniaturas (patrón ya usado en /9y13/img/).
- Imagen de portada máximo 250 KB; miniaturas máximo 60 KB.
- Siempre `width`, `height`, `loading="lazy"` (salvo la imagen principal) y `alt` que diga qué se ve, no "imagen".

## 7. Movimiento

- Transiciones de 150–200 ms, solo en color, opacidad y posición.
- Todo movimiento se desactiva con `@media (prefers-reduced-motion: reduce)`.
- Nada que parpadee ni se reproduzca solo con sonido.
