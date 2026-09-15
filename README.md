# Presentación — ElevatIA × Maderas Ibéricas

Estado de los cuatro bots de Maderas Ibéricas y las decisiones pendientes de su parte.
Preparada para la reunión con Jiban del 15 de septiembre de 2026.

## Cómo se usa

Abrir `index.html` en Chrome y pulsar `F11` para pantalla completa.

| | |
|---|---|
| Flechas o espacio | avanzar y retroceder |
| `Home` | volver al inicio |
| `R` | desmarcar todas las respuestas |

Cada pregunta es una casilla. Al marcarla sube el contador del pie, y la lámina de cierre
muestra cuántas quedaron abiertas. **Las marcas se guardan en el navegador**, así que la misma
presentación sirve de acta de la reunión. No recargar la página mientras se presenta.

## Qué hay aquí

| Archivo | Para qué |
|---|---|
| `index.html` | la presentación |
| `revisar.html` | las mismas láminas apiladas en una sola página, para leerlas de corrido |
| `artefacto.html` | la misma presentación sin las etiquetas de documento, para publicarla |

`revisar.html` y `artefacto.html` se generan a partir de `index.html`: no se editan a mano.

## Aviso

Contiene información interna de Maderas Ibéricas — situación de la cartera, correos del personal
y detalles de sus sistemas.

El repositorio es **público** porque GitHub Pages no sirve páginas desde repos privados, y hacía
falta un link presentable. Para limitar la exposición, las páginas llevan
`<meta name="robots" content="noindex, nofollow">`: **no aparecen en Google**, solo las abre quien
tenga el enlace.

Aun así, el contenido queda accesible para quien dé con la dirección. Cuando la reunión pase y el
link deje de hacer falta, conviene devolverlo a privado:

```bash
gh repo edit Marlon5710/presentacion-maderas-ibericas --visibility private --accept-visibility-change-consequences
```
