# CLAUDE.md — Sport Medical Center (MEDICALSPORT)

Convenciones del sitio para que cualquier sesión mantenga la misma línea sin re-explicar.

## Qué es

Sitio web de **Sport Medical Center**, clínica de Medicina del Deporte y Rehabilitación Física
del **Dr. Saúl Arredondo Barragán** en Zitácuaro, Michoacán. Es una **landing de una sola
página** orientada a conversión por **WhatsApp**.

El producto ancla es la **plantilla ortopédica ($1,690)**. La objeción a vencer **no es el
precio**, es la indiferencia: la gente normaliza el dolor de pie. Por eso la página abre con
conciencia del problema y la oferta llega después. Todo contenido nuevo debe respetar ese orden.

## Stack

- **HTML estático** + **Tailwind CSS por CDN** (`cdn.tailwindcss.com`) + **JavaScript vanilla**.
- **No hay paso de build.** Se edita `index.html` y se despliega tal cual.
- **Google Fonts:** Barlow Condensed (700) y Barlow (300–700).
- Todo vive en un solo archivo: estilos, contenido y JS están inline en `index.html`.
- Verificación headless con **puppeteer-core** manejando el Chrome del sistema (ver abajo).

## Repo y despliegue

- Repo: **`oscararmando2/MEDICALSPORT`** (público), local en `/Users/oscar/Desktop/SPORT MEDICAL`.
- **Producción:** `https://oscararmando2.github.io/MEDICALSPORT/` publica desde **`main`** vía
  **GitHub Pages**. Un push a `main` sale en vivo; tarda ~1 minuto y hay que contar con la caché
  del navegador (Cmd+Shift+R).
- Flujo: rama → PR → merge a `main`. Commits en español, imperativo, con
  `Co-Authored-By: Claude Opus 5 <noreply@anthropic.com>`.
- **`docs/` y el PDF del cuestionario están en `.gitignore` a propósito**: el repo es público y
  eso es estrategia interna del cliente. No versionarlos ni subirlos a ningún lado.

## Estructura

```
index.html              # todo el sitio
assets/favicon.svg      # isotipo del logo
assets/espalda.png      # ilustración 05 del efecto dominó (llegó como PNG, no SVG)
docs/                   # brief, identidad visual y prompts — NO versionado
```

## Sistema de diseño

Tokens en `:root` de `index.html`. **Nada de hex sueltos en los componentes.**

```css
--azul-900:#0B2A6B;  --azul-800:#14357F;  --azul:#1F3A93;   /* azul de marca, el del logo */
--azul-600:#2450C8;  --azul-500:#2E63E8;  --azul-300:#8FB0F7;  --azul-100:#DEE8FC;
--hueso:#F5F8FD;  --tinta:#0D1526;  --gris:#54617A;
--wa:#25D366;                                /* SOLO WhatsApp */
--ease:cubic-bezier(.22,1,.36,1);            /* la curva de todo el sitio */
--r:18px;
```

- **Paleta monocromática azul.** El verde `--wa` aparece **únicamente** en los botones de
  WhatsApp: es color de plataforma, no de marca. No introducir un tercer color sin aprobación.
- **Tipografía:** `Barlow Condensed` para titulares y cifras, `Barlow` para cuerpo.
  **Nunca Inter, Poppins ni Montserrat.**
- **Iconos e ilustraciones en SVG inline.** Nunca emojis.
- Una sola curva de easing (`--ease`). Entradas = opacidad + desplazamiento vertical corto.

## Secciones (en orden)

`#top` hero · `#contenido` el error · `#domino` efecto dominó · `#servicios` ·
`#plantillas` · `#precios` · `#doctor` · `#faq` · `#ubicacion` · CTA final · footer.

## Patrones / cómo se hacen las cosas aquí

- **Español solamente.** La clínica es local, no hay versión en inglés ni i18n.
- **Reveals:** clase `.rv` + IntersectionObserver añade `.on`. Delay escalonado por índice.
- **Sin scroll horizontal:** `overflow-x:clip` en `html` y `body`. Si aparece, se diagnostica
  el elemento culpable, no se tapa.
- **`prefers-reduced-motion` se respeta en cada animación**, sin excepción.
- **Horarios:** la fuente de verdad es el objeto `HORARIO` en el JS (minutos desde medianoche).
  De ahí salen la píldora de abierto/cerrado, la tabla y el `openingHoursSpecification` del
  JSON-LD. Si cambia el horario hay que tocar los tres.
- **SEO:** hay `MedicalClinic` en JSON-LD al final del archivo. Mantenerlo sincronizado con
  dirección, teléfonos, precios y horarios.
- **Accesibilidad:** foco visible en todo (`:focus-visible`), objetivos táctiles de 44px mínimo,
  contraste 4.5:1.

### Efecto dominó (`#domino`)

Sección fijada (`position:sticky`) que revela el texto primero y luego **una tarjeta por tramo
de scroll**. La línea de calor avanza de izquierda a derecha.

- **Escritorio (≥900px):** 520vh, las 5 tarjetas se acumulan en fila.
- **Móvil y tablet (<900px):** 540vh, se ve **una sola tarjeta a pantalla completa** y el scroll
  la reemplaza (`.is-act` entra, `.is-past` sale). El encabezado se encoge con `.is-min` y el
  bloque de cierre está colapsado hasta el último tiempo.
- Con `prefers-reduced-motion` **no se fija**: todo apilado y visible.

**Trampa importante:** los trazos animados se marcan con la clase **`.tz`**, nunca con un
selector de atributo `[pathLength]`. El parser de CSS pasa a minúsculas el nombre del atributo
en el selector y en SVG distingue mayúsculas, así que la regla nunca llega a aplicar y los
trazos quedan invisibles.

### Ilustraciones

Vienen del canvas de Claude Design. El prompt está en `docs/PROMPT-CLAUDE-DESIGN-DOMINO.md`.
Requisitos para que entren sin tocar el JS: `<svg>` inline, `viewBox="0 0 120 150"`,
`stroke="currentColor"`, `fill="none"`, y cada elemento dibujado con `class="tz"` y
`pathLength="1"`.

## Cuidado con lo que se afirma

Es un sitio de salud. **Nada de promesas de curación, garantías de resultado ni testimonios
inventados.** El disclaimer del footer se queda.

- **No mencionar el diplomado en plantillas del Dr. Saúl.** Oscar lo pidió retirar el
  7 de octubre de 2026; no va en el sitio, ni en piezas, ni en copy de redes.
- El diferenciador que **sí** se comunica: médico especialista en Medicina del Deporte + el
  estudio de pisada detrás de cada plantilla.
- Las restricciones legales de comunicación (pregunta 34 del cuestionario) **siguen sin
  contestar**. Hasta que el cliente las defina, la postura es conservadora.

## Verificación

El Browser pane de la app **no repinta** de forma fiable en este proyecto (devuelve capturas en
blanco). Usar **puppeteer-core** con el Chrome del sistema:

```
/Applications/Google Chrome.app/Contents/MacOS/Google Chrome
```

Servir el sitio aparte (`python3 -m http.server 4321 -d .`), scrollear a puntos concretos,
disparar `window.dispatchEvent(new Event('scroll'))` y leer el estado del DOM. Un warm-up
—bajar al fondo y volver— antes de medir evita falsos negativos.

Antes de dar por buena una pieza: 1440 y 390 px, sin overflow horizontal, con y sin
`prefers-reduced-motion`.

## Pendientes

- Foto real del Dr. Saúl (hoy hay un marcador circular con el isotipo)
- Logo en vector para sustituir el SVG reconstruido
- Imagen OG (`assets/og-smc-v1.png`): está referenciada en los metadatos pero no existe,
  así que al compartir el enlace no sale preview
- Confirmar colonia y CP para el mapa y el perfil de Google
- Rehacer tres ilustraciones del dominó: la pisada trae cara y trazos rojos, la cadera es una
  silueta con ropa interior y flechas de reducción, y la espalda es PNG en vez de SVG. Los
  trazos `#F26B6B` rompen la paleta monocromática
- Testimonios reales (el cuestionario no los trae; **no se inventan**)
