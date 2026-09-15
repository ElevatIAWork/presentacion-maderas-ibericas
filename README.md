# Presentación — ElevatIA × Maderas Ibéricas

Estado de los cuatro bots de Maderas Ibéricas y las decisiones pendientes de su parte.
Preparada para la reunión con Jiban del 15 de septiembre de 2026.

## Cómo se usa

Se abre en cualquier navegador y **se lee de corrido, desplazándose**. No hay botones ni nada que
pulsar, y no depende de JavaScript: si el script no cargara, la página se vería igual.

Para PDF: `Ctrl+P` y guardar como PDF. Sale una sección por página, en horizontal.

## Qué hay aquí

| Archivo | Para qué |
|---|---|
| `index.html` | la presentación |
| `artefacto.html` | la misma presentación sin las etiquetas de documento, para publicarla |

`artefacto.html` se genera a partir de `index.html`: no se edita a mano.

## Aviso

Contiene información interna de Maderas Ibéricas — situación de la cartera, correos del personal
y detalles de sus sistemas.

El repositorio vive bajo la organización **ElevatIAWork**, no bajo una cuenta personal, para que
el enlace lleve el nombre de la empresa y no el de nadie en particular. Se publica en:

```
https://elevatiawork.github.io/presentacion-maderas-ibericas/
```

Es **público** porque GitHub Pages no sirve páginas desde repos privados, y hacía falta un link
presentable. Para limitar la exposición, las páginas llevan
`<meta name="robots" content="noindex, nofollow">`: **no aparecen en Google**, solo las abre quien
tenga el enlace.

Aun así, el contenido queda accesible para quien dé con la dirección. Cuando la reunión pase y el
link deje de hacer falta, conviene devolverlo a privado:

```bash
gh repo edit ElevatIAWork/presentacion-maderas-ibericas --visibility private --accept-visibility-change-consequences
```
