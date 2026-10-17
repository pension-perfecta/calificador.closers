# DESIGN.md · Pensión Perfecta

Sistema de diseño de las herramientas internas. Cualquier agente que construya una página de este
proyecto lee este archivo antes de escribir una línea de CSS.

Los valores salen de `index.html`, que es la referencia viva. Si algo aquí no coincide con esa
página, gana la página y se actualiza este archivo.

> **Nota para quien venga después.** La primera versión de este documento se escribió copiando el
> hub tal como estaba y describía un sistema plano, sin sombras ni textura. Después se usó ese
> mismo documento para vetar cualquier cambio visual, y el rediseño no avanzó. Si vas a rediseñar,
> este archivo define **la marca**, no el techo de lo que puedes hacer. Lo inamovible es la
> tipografía, la paleta y las reglas duras del final. Todo lo demás se discute.

---

## Overview

Herramientas de trabajo para un equipo de ventas que las usa en turnos de ocho horas, muchas veces
con el cliente en la línea. Prioridad: que se lea rápido, que se encuentre rápido y que no canse.

El carácter es **verde institucional profundo con un solo acento, tipografía grande y apretada en
los títulos, y profundidad hecha con capas y sombras teñidas**. No son landing pages y tampoco
son hojas de cálculo.

---

## Colors

Todo color se declara como token en `:root`. Ningún componente usa un hex literal.

### Marca y acento

| Token | Claro | Oscuro | Para qué |
|---|---|---|---|
| `--deep` | `#00302B` | `#081A17` | Masthead, superficies oscuras |
| `--deep-2` | `#00201C` | `#04100E` | La tarjeta principal, lo más profundo |
| `--on-deep` | `#FFFFFF` | `#E6F5EF` | Texto sobre superficie oscura |
| `--on-deep-2` | `#9FC4B8` | `#86AA9E` | Texto secundario sobre oscuro |
| `--accent` | `#00C688` | `#00C688` | El único acento |
| `--accent-2` | `#00A872` | `#00E39B` | Variante para hover y resplandor |
| `--accent-soft` | `#E0F7EE` | `#0A2A22` | Fondo de chip, fila seleccionada |
| `--accent-ink` | `#00604F` | `#5BE7B8` | Texto en acento sobre fondo claro |
| `--gold` | `#7E5900` | `#E0A83C` | Avisos. Se usa poco |
| `--gold-soft` | `#FBF1D6` | `#231E0E` | Fondo de aviso |

### Superficie

| Token | Claro | Oscuro |
|---|---|---|
| `--ground` | `#EBF3EF` | `#04100E` |
| `--ground-2` | `#F5FAF7` | `#071714` |
| `--panel` | `#FFFFFF` | `#0D1E1B` |
| `--panel-2` | `#F2F8F5` | `#122622` |
| `--line` | `#CFE3DB` | `#28463F` |
| `--line-2` | `#E3EFE9` | `#1B332E` |

El `body` lleva un gradiente vertical muy leve de `--ground` a `--ground-2` y de regreso.
`--line` separa bloques distintos; `--line-2` separa renglones dentro de un bloque.

### Texto

| Token | Claro | Oscuro |
|---|---|---|
| `--ink` | `#06302C` | `#E6F5EF` |
| `--ink-2` | `#38544E` | `#AECCC3` |
| `--muted` | `#5A716B` | `#8FAEA4` |

**No existe un token más claro que `--muted` para texto.** El que había antes daba 3.0:1 y
reprobaba. `--muted` sobre `--panel` da 5.4:1.

### Contraste, verificado

| Combinación | Ratio |
|---|---|
| `--muted` sobre `--panel` | 5.4:1 |
| `--muted` sobre `--panel-2` | 5.0:1 |
| `--deep-2` sobre `--accent` (badge "Nuevo") | 7.5:1 |
| `--on-deep` sobre `--deep` | 11.1:1 |
| `--on-deep-2` sobre `--deep` | 7.4:1 |

Blanco sobre `--accent` da **2.2:1** y está prohibido. Para texto sobre acento se usa `--deep-2`.

### Los tres estados del tema

Obligatorio. El visor puede estar en claro, en oscuro, o sin elegir, que es el caso más común.

```css
:root{ color-scheme:light; /* paleta clara completa */ }
@media (prefers-color-scheme:dark){ :root:not([data-theme="light"]){ color-scheme:dark; /* tokens */ } }
:root[data-theme="dark"]{ color-scheme:dark; /* los mismos tokens */ }
```

Todo token se declara primero en `:root` a secas. `color-scheme` va en los tres bloques, si no los
scrollbars y controles nativos se quedan en claro. `body` siempre pinta `background` desde un token.
Y van dos `<meta name="theme-color">`, uno por tema.

---

## Typography

### Familia

```
"DM Sans", -apple-system, system-ui, "Segoe UI", sans-serif
```

Se carga el **eje óptico**: `family=DM+Sans:opsz,wght@9..40,400..900`. Una sola familia en todo el
proyecto. No se mezcla con serif ni con mono.

### Escala

| Rol | Tamaño | Peso | Tracking | Extra |
|---|---|---|---|---|
| Display | `clamp(40px,15.4vw,100px)` | 800 | `-.035em` | `opsz 40`, versales, una línea |
| Título de tarjeta principal | `clamp(30px,3.5vw,42px)` | 800 | `-.038em` | `opsz 32` |
| Título de tarjeta | 25px | 800 | `-.038em` | |
| Fila de lista | 15px | 700 | `-.016em` | |
| Cuerpo | 15px | 400 | normal | |
| Interfaz | 13.5px | 500 / 700 | normal | |
| Nota y pie | 12.5px | 400 | normal | |
| Micro etiqueta | 11px | 700 | `.13em` | versales |

### Principios

- Los títulos van apretados; el cuerpo, normal. El tracking negativo solo de 20px para arriba.
- **Las versales llevan menos tracking negativo que la caja baja.** `-.035em` en versales equivale
  visualmente a `-.045em` en caja baja.
- `text-wrap: balance` en todos los títulos.
- Columna de lectura cerca de 65 caracteres.
- `font-variant-numeric: tabular-nums` en el `body`, para que cualquier número en columna alinee.
- Mayúsculas solo en etiquetas micro, nunca en frases.

### Un título que no se parte

Cuando un título debe ir en una línea, lleva `white-space:nowrap` y `clamp()` con `vw`.
**Se mide en el navegador, no se estima:** a 1440, 1280, 1024, 768, 414, 390 y 360, dejando al menos
**9% de holgura** entre el ancho del texto y el del contenedor. Con 2px de sobra se corta en cuanto
la fuente carga distinto o el usuario trae otro zoom.

---

## Layout

### Espaciado

Escala en uso: **8, 10, 11, 12, 14, 18, 22, 24, 26, 30, 36**. Se separa con `gap`, no con márgenes
por elemento.

### Contenedores

| Contenedor | Ancho |
|---|---|
| Página | `1120px`, con 24px de canal (17px en móvil) |
| Columna de lectura | `44ch` a `46ch` |

La página nunca hace scroll horizontal. Lo ancho va en su propio contenedor con `overflow-x:auto`.

### Composición

- **Asimetría a propósito.** El hero es `1.62fr / 1fr`, no dos mitades iguales. Lo que más se usa
  manda, y se nota.
- **Capas.** El lienzo sube con `margin-top:-70px` y se encima sobre el masthead. Eso es lo que
  hace que se vea construida y no apilada.
- **Un panel, no varias cajas.** Las listas relacionadas van en un solo contenedor partido por
  `--line-2`, no en cartas separadas flotando.
- Una columna en móvil, dos de 880px para arriba. Las tarjetas en fila igualan alto con
  `align-items:stretch` y el enlace de cierre se fija abajo con `margin-top:auto`.

---

## Elevación y profundidad

Tres niveles, siempre **teñidos de verde**, nunca negro puro.

```css
--l1: 0 1px 1.5px rgba(0,48,43,.05), 0 8px 20px -14px rgba(0,48,43,.4);
--l2: 0 1px 2px rgba(0,48,43,.06), 0 26px 50px -28px rgba(0,48,43,.52);
--l3: 0 30px 70px -30px rgba(0,48,43,.4);
```

`--l1` en reposo, `--l2` en hover, `--l3` en lo que de verdad flota: la tarjeta principal y la
paleta de comandos.

**Textura.** Una capa fija de grano al 3%, `pointer-events:none`, en `multiply` sobre claro y
`screen` sobre oscuro. Quita lo plano digital sin que se note.

**Resplandor.** Gradientes radiales de acento muy abiertos y a baja opacidad, en el masthead y
siguiendo al cursor en la tarjeta principal. Nunca gradiente lineal de 45 grados.

---

## Formas

| Radio | Para qué |
|---|---|
| `5px` | Chip, badge |
| `9px` | Marca del logo |
| `12px` – `13px` | Fila de lista, chip de herramienta |
| `14px` | Campo de búsqueda |
| `18px` | Paleta de comandos |
| `20px` (`--r`) | Tarjeta, panel |

El radio dice el tamaño del objeto. No se usa uno solo para todo.

---

## Componentes

### Paleta de comandos

Es la acción principal del hub, no un extra. Se abre con `/`, con `Ctrl+K` o `Cmd+K`, y con el
campo del masthead. Flechas para moverse, Enter para abrir, Esc para cerrar.

Obligatorio: `role="dialog"` con `aria-modal="true"`, el input como `combobox` con
`aria-activedescendant`, la lista como `listbox` con `aria-selected`, y **el foco regresa** al
elemento que la abrió.

### Tarjetas

- **Principal:** fondo `--deep-2`, texto `--on-deep`, sombra `--l3`, acento en el ojo y en el enlace.
- **Secundaria:** fondo `--panel`, borde `--line`, sombra `--l1` que sube a `--l2` en hover.
- Hover `translateY(-4px)`, presionado `translateY(-1px)`. Transición de 260ms con
  `cubic-bezier(.2,.75,.3,1)`.

### Iconos

Anillo fino de `1.5px` con el icono dentro, no cuadrito redondeado relleno. Trazo de 2 a 2.2.
Convención de dirección, fija:

- **Chevron** `m9 18 6-6-6-6` = otra página del mismo sitio.
- **Diagonal** `M7 17 17 7M9 7h8v8` = se sale a otro sitio.

Todo SVG decorativo lleva `aria-hidden="true"`.

### Una sola fuente de verdad

Las listas, el riel de herramientas y la paleta se generan desde **un mismo arreglo en JS**.
Agregar un destino es una línea y aparece en los tres lugares. No se duplica markup.

---

## Movimiento

Entrada escalonada de 60ms entre bloques, 460ms de duración, **desde `opacity:.45`, nunca desde
cero**: la página tiene que ser legible en su primer cuadro, que es lo que ve una miniatura o
alguien que llega y se va.

Se anima solo `transform` y `opacity`. Nada de `transition:all`.

```css
@media (prefers-reduced-motion:reduce){
  *,*::before,*::after{animation-duration:.01ms !important;animation-iteration-count:1 !important;
    transition-duration:.01ms !important;scroll-behavior:auto !important}
}
```

---

## Reglas duras

Estas no se discuten.

1. **Ningún `max-width` en un párrafo, nota o subtítulo.** El ancho de lectura lo controla el
   contenedor o `column-count`. Acotar texto dentro de un bloque ancho deja hueco a la derecha;
   pasó cinco veces en el tablero de agosto. Excepción: una columna de lectura que ocupa ella sola
   el ancho de la página.
2. **Cero em-dash**, en el código y en el texto. Punto y coma, dos puntos o frases cortas.
3. **DM Sans y el verde de marca no se cambian.** Vienen del sitio real de Pensión Perfecta.
4. **Un solo acento.** No se agrega un segundo color de marca.
5. **Nada de emoji como marcador de sección.** Los emoji de las etapas del CRM son dato, no adorno.
6. **Landmarks, skip link y foco visible** en toda página nueva.
7. Español de México, frases cortas, verbos activos.

### Lo que no se usa, y por qué

Gradiente morado a azul, crema con serif y terracota, Inter o Space Grotesk como tipografía
"segura", hero a `100vh`, fotos de stock, y tarjetas todas con el mismo radio y la misma sombra.
Son las huellas más reconocibles del diseño generado y no tienen nada que ver con esta marca.

---

## Prompt para un agente

> Usa el sistema de `DESIGN.md` de Pensión Perfecta. DM Sans con eje óptico, una sola familia.
> Tokens en `:root` con los tres estados de tema y `color-scheme` en los tres. Verde `--deep` para
> las superficies oscuras y `--accent` como único acento. Profundidad con las tres sombras teñidas
> de verde, grano al 3% y resplandores radiales; nada de gradientes lineales. Radios de 5 a 20
> según el tamaño del objeto. Contenedor de 1120px. Composición asimétrica y con capas, no rejillas
> de tarjetas iguales. Ningún párrafo lleva `max-width`. Español de México, frases cortas, cero
> em-dash.

Para auditar lo que ya existe, el skill `web-design-guidelines`. Para rediseñar sin romper,
`redesign-existing-projects`. Para pantallas nuevas de herramienta interna, `minimalist-ui`.
