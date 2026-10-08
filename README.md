# MEDICALSPORT — Sport Medical Center

Sitio web de **Sport Medical Center**, clínica de Medicina del Deporte y Rehabilitación Física
del **Dr. Saúl Arredondo Barragán** en Zitácuaro, Michoacán.

**En vivo:** https://oscararmando2.github.io/MEDICALSPORT/

## Qué es

Landing de una sola página, orientada a conversión por **WhatsApp**. El producto ancla es la
**plantilla ortopédica personalizada ($1,690)** y la objeción principal a vencer no es el precio,
sino la indiferencia: la gente normaliza el dolor de pie. Por eso la página abre con conciencia
del problema antes de ofertar.

## Stack

HTML estático + **Tailwind CSS por CDN** + JavaScript vanilla. **Sin paso de build**: se edita
`index.html` y se despliega tal cual. Fuentes de Google (Barlow Condensed + Barlow).

## Estructura

```
index.html          # todo el sitio (estilos, contenido y JS inline)
assets/favicon.svg  # isotipo del logo
docs/               # brief e identidad visual — NO versionado (ver .gitignore)
```

## Secciones

1. Hero — el gancho + estado abierto/cerrado en vivo + CTA WhatsApp
2. El error — conciencia del problema y señales de alerta
3. Efecto dominó — el pie tira la siguiente ficha: tobillo, rodilla, cadera, espalda
4. Servicios — plantillas (dominante), rehabilitación, control de peso
5. Plantillas — dos tipos, proceso de 4 pasos, qué traer al estudio
6. Precios — transparentes, consulta destacada como entrada
7. El doctor — credibilidad
8. Preguntas frecuentes — las dudas reales que recibe la clínica
9. Ubicación y horarios — tabla con el día de hoy resaltado
10. CTA final + footer

### Efecto dominó

Sección fijada (`position:sticky`) de 520vh en escritorio. El progreso del scroll
revela primero el bloque de texto y después una tarjeta a la vez, mientras la línea
de calor avanza de izquierda a derecha y cada hueso se dibuja trazo a trazo
(`stroke-dashoffset` sobre elementos con `pathLength="1"`).

En móvil y tablet (<900px) **también se fija**, pero cambia de formato: se ve
**una sola tarjeta a pantalla completa** y el scroll la reemplaza por la siguiente
(`.is-act` entra, `.is-past` sale hacia arriba). El encabezado se encoge (`.is-min`)
y el bloque de cierre está colapsado hasta el último tiempo, para que la tarjeta
ocupe toda la altura disponible.

Solo con `prefers-reduced-motion` no se fija: todo queda apilado y visible.

**Ojo:** los trazos animados se marcan con la clase `.tz`, no con `[pathLength]`.
El parser de CSS pasa a minúsculas el nombre del atributo en el selector y en SVG
distingue mayúsculas, así que un selector de atributo nunca llega a aplicar.

Las ilustraciones vienen de Claude Design. Cuatro son SVG en línea y se dibujan trazo
a trazo; **la quinta (la espalda) es un PNG** (`assets/espalda.png`), así que solo hace
fundido, no se dibuja. Si algún día llega en SVG, sustituir el `<img class="hueso-img">`
por el `<svg class="hueso">` y se anima sola.

## Convenciones

- **Paleta monocromática azul** tomada del logo. El verde aparece *únicamente* en el botón de
  WhatsApp (color de plataforma, no de marca).
- Tipografía: `Barlow Condensed` para titulares, `Barlow` para cuerpo. Nunca Inter/Poppins/Montserrat.
- Tokens CSS en `:root`. Nada de hex sueltos en los componentes.
- Iconos **SVG en línea**, nunca emojis.
- `prefers-reduced-motion` respetado en cada animación.
- Sin scroll horizontal (`overflow-x:clip`).
- Todo el texto en español. La clínica es local; no hay versión en inglés.
- **Cuidado con las afirmaciones médicas**: nada de promesas de curación, garantías ni
  testimonios inventados. El disclaimer del footer se queda.

## Despliegue

GitHub Pages desde `main`. Un push a `main` publica; ojo con la caché del navegador
(Cmd+Shift+R).

## Pendientes

- Foto real del Dr. Saúl (hoy hay un marcador circular con el isotipo)
- Logo en vector para reemplazar el SVG reconstruido
- Imagen OG (`assets/og-smc-v1.png`) — referenciada en los metadatos, aún no existe
- Rehacer tres ilustraciones del efecto dominó: la pisada trae cara y trazos rojos, la
  cadera es una silueta con ropa interior y flechas de reducción, y la espalda es PNG en
  vez de SVG. Los trazos rojos (`#F26B6B`) rompen la paleta monocromática azul
- Confirmar colonia y CP para el mapa y el perfil de Google
- Testimonios reales (el cuestionario no los trae; no se inventan)
