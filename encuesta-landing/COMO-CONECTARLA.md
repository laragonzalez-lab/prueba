# Encuestas Honimunn: cómo ponerlas en marcha

Hay **8 piezas**, cada una en su propio archivo:

| Pieza | Archivo |
|---|---|
| Correo perdidos · España | `correos/correo-perdidos-es.html` |
| Correo perdidos · México | `correos/correo-perdidos-mx.html` |
| Correo vuelta de viaje · España | `correos/correo-viaje-es.html` |
| Correo vuelta de viaje · México | `correos/correo-viaje-mx.html` |
| Encuesta perdidos · España | `encuesta-landing/perdidos-es/index.html` |
| Encuesta perdidos · México | `encuesta-landing/perdidos-mx/index.html` |
| Encuesta vuelta de viaje · España | `encuesta-landing/viaje-es/index.html` |
| Encuesta vuelta de viaje · México | `encuesta-landing/viaje-mx/index.html` |

Funciona así:

1. La pareja recibe el correo.
2. Toca un botón y se abre su encuesta, con su nombre y con esa respuesta ya marcada.
3. Al pulsar «Enviar respuestas», las respuestas se guardan en su ficha de HubSpot.

Al principio de cada correo hay una nota con el **asunto**, el **preheader** (el texto que se ve junto al asunto en la bandeja de entrada) y **cuándo** se envía.

Las cuatro encuestas están ahora en **modo prueba**: no envían nada y, al terminar, enseñan lo que se enviaría.

---

## Paso 1. Revisar los datos personalizados de los correos

Los correos llevan cuatro huecos que tu herramienta de envío rellena con los datos de cada pareja. He usado el mismo formato que vuestra newsletter de confirmación:

| Hueco | Qué pone |
|---|---|
| `{{cliente.nombre}}` | Nombre de la pareja |
| `{{cliente.email}}` | Email de la pareja. **Imprescindible**: sin él, la respuesta no se puede asociar a nadie. |
| `{{viaje.destino}}` | Destino, por ejemplo «Japón» |
| `{{enlace_baja}}` | Enlace para darse de baja |

Si enviáis desde **HubSpot**, cámbialos con «Buscar y reemplazar» por `{{contact.firstname}}`, `{{contact.email}}`, el campo de destino que uséis y `{{unsubscribe_link}}`.

Los botones apuntan a `https://encuesta.honimunn.com/...`. Si al final la encuesta queda en otra dirección (paso 5), reemplaza `https://encuesta.honimunn.com` por la nueva en los cuatro correos.

## Paso 2. Crear en HubSpot los campos donde se guardan las respuestas

En HubSpot: **icono del engranaje (Configuración) → Propiedades → Propiedades de contacto → Crear propiedad**.

El «nombre interno» tiene que ser **exactamente** el que pone aquí.

**Encuesta de perdidos**

| Etiqueta | Nombre interno | Tipo |
|---|---|---|
| Motivo de no reserva | `motivo_no_reserva` | Texto de una línea |
| Motivo de no reserva (otro) | `motivo_no_reserva_otro` | Texto de una línea |
| Valoración atención Travel Designer | `valoracion_atencion_td` | Número |
| Qué podríamos mejorar | `que_podriamos_mejorar` | Texto de varias líneas |
| Quiere ideas de viaje | `quiere_ideas_viaje` | Texto de una línea |

**Encuesta de vuelta de viaje**

| Etiqueta | Nombre interno | Tipo |
|---|---|---|
| NPS viaje (0-10) | `nps_viaje` | Número |
| Valoración atención Travel Designer | `valoracion_atencion_td` | (ya creada arriba) |
| Valoración vuelos y traslados | `valoracion_vuelos_traslados` | Número |
| Valoración alojamiento | `valoracion_alojamiento` | Número |
| Valoración experiencias | `valoracion_experiencias` | Número |
| Valoración calidad-precio | `valoracion_calidad_precio` | Número |
| Tuvo problema en el viaje | `tuvo_problema_viaje` | Texto de una línea |
| Detalle del problema | `detalle_problema_viaje` | Texto de varias líneas |
| Lo mejor del viaje | `lo_mejor_del_viaje` | Texto de varias líneas |
| Qué mejoraríais | `que_mejorariais` | Texto de varias líneas |
| Quiere ideas de viaje | `quiere_ideas_viaje` | (ya creada arriba) |

Las respuestas de España y de México llegan con las mismas palabras («Precio», «Por libre», «Sí»...), así que los informes pueden juntar los dos mercados.

## Paso 3. Crear dos formularios en HubSpot

En HubSpot: **Marketing → Formularios → Crear formulario**.

1. **«Encuesta perdidos»**: con el campo *Correo* y los campos de perdidos del paso 2.
2. **«Encuesta vuelta de viaje»**: con *Correo* y los campos de vuelta de viaje.
3. Publica los dos. Nadie los verá: solo sirven de buzón para recibir las respuestas.
4. Apunta los códigos. Están en **Compartir** o **Insertar**, dentro del código que aparece:
   - `portalId`: un número, el mismo para los dos formularios.
   - `formId`: un código largo con guiones, distinto en cada formulario.

Las dos encuestas de perdidos (España y México) usan el mismo formulario. Las dos de vuelta de viaje, también.

## Paso 4. Poner los códigos en cada encuesta

Haz esto en cada uno de los cuatro archivos `index.html` de las encuestas:

1. Ábrelo con un editor de texto sencillo: el Bloc de notas en Windows, o TextEdit en Mac en modo «texto sin formato».
2. Busca el bloque `AJUSTES`, al principio del código JavaScript.
3. Cámbialo así:

```js
modo: 'hubspot',
hubspot: {
  portalId: '12345678',
  formId: 'código-del-formulario'
},
```

   Usa el `formId` de «Encuesta perdidos» en las encuestas de perdidos, y el de «Encuesta vuelta de viaje» en las de vuelta de viaje.

4. En el mismo bloque:
   - `resenaGoogle`: el enlace directo para dejar reseña. En el Perfil de Empresa de Google: «Pedir reseñas» → copiar enlace.
   - `privacidad`: comprueba que la dirección de la política de privacidad es la correcta.
5. Guarda el archivo.

## Paso 5. Publicar las encuestas en internet (gratis)

1. Entra en https://app.netlify.com/drop y crea una cuenta gratuita.
2. Arrastra la carpeta `encuesta-landing` entera.
3. Netlify te da una dirección, por ejemplo `https://nombre-raro.netlify.app`. Las encuestas quedan en:
   - `.../perdidos-es/`
   - `.../perdidos-mx/`
   - `.../viaje-es/`
   - `.../viaje-mx/`
4. Para usar `encuesta.honimunn.com` (la dirección que ya llevan los correos), ve a *Domain settings* en Netlify. Quien gestione vuestro dominio tendrá que añadir un registro, y Netlify explica cuál.

Si cambias algo más adelante, vuelve a arrastrar la carpeta en la pestaña *Deploys* del mismo sitio.

## Paso 6. Probar

Mándate cada correo a ti mismo, toca un botón, rellena la encuesta y envíala. En un par de minutos las respuestas deberían aparecer en tu ficha de HubSpot.

**Si sale «No hemos podido enviar vuestras respuestas»**, casi siempre es por una de estas causas:
- Un código del paso 4 está mal copiado.
- Falta una propiedad del paso 2, o su nombre interno no coincide.
- La cuenta tiene activado el consentimiento RGPD en los formularios.

---

## Recomendado: que HubSpot actúe solo

En *Automatización → Flujos de trabajo*:

- **NPS de 0 a 6** → tarea «Llamar en 48 h» para el Travel Designer. La encuesta se lo promete a la pareja.
- **NPS de 7 u 8** → tarea «Escribir para entender qué faltó».
- **Quiere ideas de viaje = Sí** → añadir a la lista de Munnviajes y repetidores.
