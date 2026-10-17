# DESIGN.md · Pensión Perfecta

Sistema de diseño de las herramientas internas de Pensión Perfecta. Cualquier agente que construya
una página de este proyecto lee este archivo antes de escribir una línea de CSS.

Los valores no son propuestas: están tomados de `calificador-web/index.html` y verificados contra
pensionperfecta.com. Si algo aquí no coincide con el código, gana el código y se actualiza este archivo.

---

## Overview

Herramientas de trabajo para un equipo de ventas que las usa en turnos de ocho horas, muchas veces
con el cliente en la línea. Prioridad: que se lea rápido y no canse. No son landing pages.

El carácter es **verde institucional sobre fondo claro con mucho aire**, tipografía apretada en
títulos y texto chico pero legible. Nada de gradientes, nada de sombras pesadas, nada de hero
a pantalla completa.

---

## Colors

Todo color se declara como token en `:root`. Ningún componente usa un hex literal.

### Brand & Accent

| Token | Claro | Oscuro | Para qué |
|---|---|---|---|
| `--brand` | `#00443E` | `#102320` | Barras, encabezados, botón primario |
| `--brand-ink` | `#FFFFFF` | `#E4F5EE` | Texto sobre `--brand` |
| `--accent` | `#00C688` | `#00C688` | El único acento. Estados activos, énfasis |
| `--accent-soft` | `#E3F8EF` | `#0C2C25` | Fondo de chip, badge, fila destacada |
| `--accent-ink` | `#00695A` | `#4FE3B0` | Texto sobre `--accent-soft` |
| `--gold` | `#B07D05` | `#E2A93A` | Advertencia y pendientes. Se usa poco |
| `--gold-soft` | `#FBF0D2` | `#2A2411` | Fondo de aviso |

`#00554E` es el verde de titulares de los documentos de dirección (Google Docs). En web se usa
`--brand`.

### Surface

| Token | Claro | Oscuro |
|---|---|---|
| `--ground` | `#EEF6F2` | `#050F0D` |
| `--panel` | `#FFFFFF` | `#102320` |
| `--panel-2` | `#F0F7F3` | `#17322C` |
| `--line` | `#BCD9D1` | `#2A4A42` |
| `--line-2` | `#DBEBE3` | `#1E3B34` |

`--line` separa bloques distintos. `--line-2` separa renglones dentro de un mismo bloque.

### Text

| Token | Claro | Oscuro | Para qué |
|---|---|---|---|
| `--ink` | `#0C3B39` | `#E4F5EE` | Texto principal y títulos |
| `--ink-2` | `#3F5A54` | `#AECCC2` | Texto secundario |
| `--muted` | `#5B6E6D` | `#8AA79C` | Notas, pies, etiquetas |
| `--faint` | `#789E92` | `#7A9A8F` | Lo que casi no importa |

### Los tres estados del tema

Obligatorio. El visor puede estar en claro, oscuro, o sin elegir.

```css
:root{ /* paleta clara completa */ }
@media (prefers-color-scheme:dark){ :root:not([data-theme="light"]){ /* solo tokens */ } }
:root[data-theme="dark"]{ /* los mismos tokens */ }
```

Todo token se declara primero en `:root` a secas. Un color que solo existe dentro del media query
o dentro de `[data-theme]` deja la página ilegible en el estado sin marcar, que es el más común.
`body` siempre pinta `background` desde un token.

---

## Typography

### Font Family

```
"DM Sans", -apple-system, system-ui, "Segoe UI", sans-serif
```

Pesos cargados: 400, 500, 600, 700, 800. Se carga de Google Fonts con `display=swap`.
Una sola familia en todo el proyecto. No se mezcla con serif ni con mono.

### Hierarchy

| Rol | Tamaño | Peso | Tracking |
|---|---|---|---|
| Título de página | 28px | 800 | `-.025em` |
| Título de sección | 24px | 800 | `-.02em` |
| Subtítulo | 20.5px | 700 | `-.02em` |
| Título de tarjeta | 16px | 700 | `-.005em` |
| Cuerpo | 15px | 400 | normal |
| Cuerpo chico | 14px / 14.5px | 400 | normal |
| Texto de interfaz | 13.5px | 500 / 600 | normal |
| Nota y pie | 12.5px | 400 | normal |
| Etiqueta en mayúsculas | 10.5px | 700 | `.03em` |

### Principles

- Los títulos van apretados, el cuerpo normal. El tracking negativo solo en 20px para arriba.
- `text-wrap: balance` en todos los títulos.
- Columna de lectura cerca de 65 caracteres.
- Los números que se comparan en columna llevan `font-variant-numeric: tabular-nums`.
- Mayúsculas solo en etiquetas de 10.5px, nunca en frases.

---

## Layout

### Spacing System

Escala real en uso: **7, 8, 9, 10, 12, 16, 20, 24, 32**. Se separa con `gap`, no con márgenes
por elemento.

### Grid & Container

| Contenedor | Ancho |
|---|---|
| Página completa | `1060px` |
| Columna de lectura | `760px` / `860px` |
| Bloque angosto | `620px` / `640px` |

Canal lateral de 16px en móvil. La página nunca hace scroll horizontal: lo ancho (tablas, código)
va en su propio contenedor con `overflow-x: auto`.

### Whitespace Philosophy

El aire separa, no el borde. Antes de meter una línea o una sombra, se prueba con más espacio.

### Responsive Strategy

Una columna en móvil, dos de 760px para arriba, tres solo si el contenido llena las tres. Las
tarjetas en fila se igualan en alto con `flex` y `flex:1` en el cuerpo, nunca con alturas fijas.

---

## Elevation & Depth

Plano. La jerarquía se hace con fondo y borde, no con sombra.

- Tarjeta normal: `--panel` sobre `--ground`, borde `1px solid var(--line)`.
- Lo que sí resalta: `--accent-soft` de fondo, o borde `--accent`.
- Sombra: como mucho una, muy suave, y solo en algo que de verdad flota (menú, modal).
- Cero gradientes. Cero glassmorphism.

---

## Shapes

### Border Radius Scale

| Valor | Para qué |
|---|---|
| `4px` | Chip, badge |
| `5px` | Input, control chico |
| `8px` | Botón |
| `9px` | Tarjeta, panel |
| `13px` | Bloque hero, contenedor grande |

No se usa un solo radio para todo. El radio dice el tamaño del objeto.

---

## Components

### Buttons

- **Primario:** fondo `--brand`, texto `--brand-ink`, radio 8px, peso 600, 13.5px.
- **Secundario:** fondo `--panel`, borde `--line`, texto `--ink`.
- **Terciario:** solo texto, color `--accent-ink`.
- Foco siempre visible. El texto del botón dice lo que pasa: "Guardar", y el aviso después dice "Guardado".

### Links e iconos de dirección

Convención fija del hub y no se cambia:

- **Chevron** `m9 18 6-6-6-6` = va a otra página del mismo sitio.
- **Diagonal** `M7 17 17 7M9 7h8v8` = se sale a otro sitio.

### Cards & Containers

Las tarjetas de una misma fila comparten borde, radio, padding y posición de cada elemento.
Alto igualado por `flex`, no por `height`. El texto que puede crecer se envuelve o hace scroll
en su propio contenedor; texto cortado es un bug.

### Tablas

Encabezado con fondo `--accent-soft`, bordes `--line`, 10pt, filas alternas en `--panel-2`.
Números alineados con `tabular-nums`.

### Estados por color

Verde `--accent` es énfasis, no "correcto". Para semáforo operativo se usa el `--gold` de aviso
y un rojo semántico aparte; nunca se reusa el acento de marca como estado.

---

## Do's and Don'ts

### Do

- Declarar todos los tokens en `:root` antes de redefinirlos.
- Pintar `background` en `body` desde un token.
- Igualar alturas con `flex`, no con `height`.
- Dejar la página legible en su primer cuadro, sin scroll y sin animación de entrada.
- Escribir en español de México, frases cortas, verbos activos.

### Don't

- **Nunca `max-width` en un párrafo, nota o subtítulo.** El ancho de lectura lo controla el
  contenedor o `column-count`. Acotar texto dentro de un bloque ancho deja hueco a la derecha;
  pasó cinco veces en el tablero de agosto. Única excepción: una columna de lectura que ocupa
  ella sola el ancho de la página.
- Nada de em-dash en ningún texto. Punto y coma, dos puntos o frases cortas.
- Nada de emoji como marcador de sección. Los emoji de las etapas del CRM son dato, no adorno.
- Nada de hero a `100vh`.
- Nada de gradiente morado a azul, crema con serif y terracota, ni Inter o Space Grotesk como
  tipografía "segura". Aquí la tipografía es DM Sans.
- No meter un segundo color de acento. Hay uno.

---

## Agent Prompt Guide

Cuando construyas una página de este proyecto:

> Usa el sistema de `DESIGN.md` de Pensión Perfecta. DM Sans en una sola familia. Tokens en
> `:root` con los tres estados de tema. Verde `--brand` para estructura, `--accent` como único
> acento. Diseño plano: fondo y borde, sin sombra ni gradiente. Radios 4/5/8/9/13 según el tamaño
> del objeto. Contenedor de 1060px, lectura en 760px. Ningún párrafo lleva `max-width`. Español
> de México, frases cortas, cero em-dash.

Para auditar lo que ya existe, el skill `web-design-guidelines`. Para rediseñar sin romper,
`redesign-existing-projects`. Para pantallas nuevas de herramienta interna, `minimalist-ui`.
