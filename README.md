# Presentación — ElevatIA × Maderas Ibéricas

Estado de los cuatro bots de Maderas Ibéricas y las decisiones pendientes de su parte.
Preparada para la reunión con Jiban del 15 de septiembre de 2026.

## Cómo se usa

Va **bot por bot**: para cada uno, su estado y enseguida las preguntas que nos hacen falta
de ese bot. Lucy WhatsApp, Lucy Voz, Clara Cobros y el Cotizador DWG, en ese orden. Al final,
lo que salió de la última reunión y **lo pendiente** — qué le toca a cada lado.

Son 24 láminas y 24 preguntas.

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

## ⚠️ No cambiar la visibilidad sin preguntar

**Esta presentación la están leyendo los líderes de Maderas Ibéricas.** Mientras eso siga siendo
cierto, el repositorio se queda público.

Pasarlo a privado no solo rompe el enlace: **desactiva GitHub Pages por completo**. Volver a
ponerlo público **no** reactiva el sitio — sigue dando 404 hasta que se vuelve a crear a mano:

```bash
gh api -X POST repos/ElevatIAWork/presentacion-maderas-ibericas/pages -f "source[branch]=main" -f "source[path]=/"
```

y hay que esperar unos 45 segundos a que reconstruya. Pasó el 16/09/2026: se cerró apoyándose en
un "devuélvelo a privado cuando pase la reunión" dicho días antes, y los gerentes se encontraron
un 404. **Una instrucción vieja no es permiso para hoy.**

Cuando de verdad ya nadie lo necesite, se cierra así — y se avisa antes a quien tenga el enlace:

```bash
gh repo edit ElevatIAWork/presentacion-maderas-ibericas --visibility private --accept-visibility-change-consequences
```

## Si el enlace se cae

No hace falta esperar a nadie: **abrir la presentación y `Ctrl+P` → Guardar como PDF**. Salen las
24 páginas, una por lámina, y ese archivo se puede mandar por WhatsApp sin depender de GitHub.
Conviene tener una copia guardada de antemano, justamente para no necesitar el sitio para hacerla.
