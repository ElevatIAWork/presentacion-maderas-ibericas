# Investigación: validar RNC/cédula, avisos al vendedor, reporte diario y correos

*ElevatIA · 15 de septiembre de 2026. Los cuatro puntos que salieron de la reunión con Maderas.*

---

## 1. Validar el RNC o la cédula contra la DGII

### Lo primero: la DGII no tiene API oficial

No existe un servicio oficial al que un programa pueda preguntarle "¿este RNC es válido?". Hay
terceros que venden ese servicio, pero meterlos en el circuito significa una dependencia externa,
un costo por consulta y que los datos de los clientes de Maderas pasen por un proveedor ajeno.

### Lo que sí existe, y es mejor

La DGII **publica el registro completo** en un archivo descargable:

```
https://dgii.gov.do/app/WebApps/Consultas/RNC/DGII_RNC.zip
```

Lo descargamos y lo medimos. Esto es lo que trae, verificado, no supuesto:

| | |
|---|---|
| Tamaño | 22,8 MB comprimido · 90 MB en texto |
| Registros | **789.608** |
| RNC de empresa (9 dígitos) | 499.964 — de ellos **243.572 activos** |
| Cédulas (11 dígitos) | 289.617 |
| Campos | número, nombre, nombre comercial, actividad económica, fecha, **estado**, régimen |
| Estados | ACTIVO, SUSPENDIDO, DADO DE BAJA, CESE TEMPORAL, ANULADO, RECHAZADO |
| Última actualización | 5 de septiembre de 2026 |
| Codificación | Latin-1, **no** UTF-8 — si se lee como UTF-8 los acentos salen rotos |

**Contiene cédulas además de RNC**, que era la duda: 289.617 personas físicas registradas como
contribuyentes. Una cédula que nunca se registró en la DGII no aparece.

> ⚠️ **El servidor rechaza a quien no se identifique como navegador.** Con una petición normal
> devuelve **403 Forbidden**; con una cabecera `User-Agent` de navegador devuelve 200. Sin ese
> detalle, la descarga automática falla y parece que el archivo no existe.

### Antes de consultar nada: el dígito verificador

Tanto el RNC como la cédula llevan un **dígito de control calculado**. Se puede comprobar con
aritmética, sin consultar ninguna fuente, en microsegundos.

Lo probamos contra los 790.000 registros reales:

| | |
|---|---|
| RNC de empresa que pasan el dígito verificador | **100,0 %** de 499.964 |
| Cédulas que pasan el dígito verificador | **100,0 %** de 289.617 |
| Números inventados probados | todos **rechazados** |

Es decir: un número mal dictado por teléfono, mal transcrito o inventado **se detecta al instante
y gratis**, sin salir a internet. Eso cubre la mayoría de los casos reales, que no son fraude sino
un dígito mal entendido en una llamada.

### La arquitectura que proponemos

Dos capas, de la más barata a la más cara:

**Capa 1 — el dígito verificador.** Instantáneo, sin dependencias. Si falla, Lucy pide el número
otra vez en la misma llamada: *"Creo que no tomé bien el número, ¿me lo repite?"*. Resuelve el
problema real del canal de voz.

**Capa 2 — existencia y estado.** Una copia local del registro de la DGII, refrescada una vez por
semana por un flujo propio. Responde si el número existe, a nombre de quién y si está **ACTIVO**.
Sin latencia durante la llamada y sin depender de que la DGII esté disponible en ese momento.

Lo que **no** recomendamos es consultar a la DGII en vivo durante la llamada: el cliente está al
teléfono esperando, y una consulta externa lenta o caída deja a Lucy muda.

### Lo que hay que decidir antes de construirlo

- **Dónde se guarda la copia.** 790.000 registros no caben cómodos en una tabla de n8n. Se puede
  reducir a los 243.572 RNC activos, o guardarlo fuera. Hay que medirlo antes de elegir.
- **Qué hace Lucy si el RNC es válido pero está SUSPENDIDO o DADO DE BAJA.** Son 308.429 y 74.894
  registros respectivamente: no es un caso raro. ¿Se le da el catálogo igual, se avisa a ventas,
  se corta? Es decisión de Maderas, no nuestra.
- **Qué pasa con quien no tiene RNC.** Un cliente particular que compra sin comprobante fiscal no
  está en el registro. Bloquearlo sería cerrarle la puerta a ventas legítimas.

---

## 2. Aviso al vendedor cuando se hace un pedido y cuando el cliente paga

### Lo que ya existe

| Flujo | Estado | Qué hace |
|---|---|---|
| `Lucy Voz I - Aviso al Area` | encendido | avisa al área al terminar la llamada, y ya manda correo |
| `Lucy Voz C - Aviso Instantaneo - Pedido Listo` | encendido | avisa cuando el pedido está listo para recoger |

O sea que **el canal de correo ya está montado** y funcionando. No hay que construirlo desde cero.

### Lo que falta

**A quién.** Hoy el aviso va al área. Lo que se pidió es que vaya **al vendedor concreto** que
atiende ese cliente. Para eso hace falta saber qué vendedor corresponde a cada cliente — un dato
que hoy no tenemos: el directorio que nos pasaron lista cuatro personas en Ventas, pero no dice
cuál atiende a quién.

**El correo de pago.** Es un aviso nuevo, disparado cuando el cliente paga. Aquí la pregunta de
fondo es **cómo se entera el sistema de que pagó**: si es porque la factura desaparece del estado
de cuenta de ADM —como hace Clara— el aviso puede salir solo; si es porque alguien lo marca a
mano, hace falta dónde lo marca.

---

## 3. Reporte diario de Lucy

### Lo que ya existe

| Flujo | Estado | |
|---|---|---|
| `Lucy Voz G - Reporte Diario Gerencia` | **apagado** | ya menciona cotizaciones y pedidos, y manda correo |
| `Lucy - Métricas Semanales` | encendido | resumen semanal, sin desglose de cotizaciones ni pedidos |

**Ya hay un reporte diario construido y apagado.** Antes de hacer uno nuevo hay que abrirlo y ver
qué le falta: puede que solo haya que completarlo y encenderlo.

### Lo que se pidió

Un reporte al cierre de cada día con **todas las cotizaciones y todos los pedidos** de la jornada.
Es el equivalente exacto al que Clara ya le manda a Angelo, y ese ya está resuelto: mismo sistema
de marca, mismo formato, mismo criterio de "primero lo que necesita que alguien haga algo".

Se puede reutilizar entero. El trabajo real no es el correo, es **de dónde salen los datos**:
Airtable, ADM, o las dos — y eso enlaza con la pregunta que ya está en la presentación sobre si
Airtable se queda o se va.

---

## 4. Unificar los correos

### El problema, con datos

En el directorio que nos pasaron conviven **dos dominios en el mismo equipo**:

| Dominio | Quiénes |
|---|---|
| `@maderasibericas.com` | Casandra, Ana, Nicolys, Inés, Emperatriz, Maribel, Angelo, Esmerlyn, Ana Decena, Yeisi |
| `@finsawood.com` | Junior (Proyectos), Brenda (Ventas), Henry (`ventas07@`) |

Y los clientes reciben instrucciones de mandar comprobantes a `contabilidad@finsawood.com`,
mientras que quien gestiona el cobro —Angelo— está en `@maderasibericas.com`.

### Por qué importa para los bots

Cada bot que manda correo necesita una dirección de salida autorizada. **Con dos dominios hay que
autorizar dos veces**, y cada vez que alguien cambia de dominio hay que revisar qué bots le
escriben. Un correo que sale de un dominio sin autorización cae en spam o se rechaza — y el fallo
es silencioso.

### Lo que hay que saber antes de tocar nada

Esto no es una decisión técnica nuestra: **es de marca**. `finsawood.com` y `maderasibericas.com`
son dos nombres comerciales, y puede haber una razón de negocio para mantener los dos. Lo que sí
podemos decir es el costo técnico de mantenerlos separados, y que se decida con ese dato encima.

La pregunta concreta: **¿desde qué dominio escriben los bots a los clientes?** Una sola respuesta
resuelve Clara, Lucy y lo que venga después.
