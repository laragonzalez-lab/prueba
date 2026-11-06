# Encuesta Honimunn: cómo ponerla en marcha

La encuesta es un solo archivo, `index.html`, dentro de esta carpeta. Funciona así:

1. La pareja recibe un email desde HubSpot con botones.
2. Al tocar un botón se abre esta página, ya con su nombre y con la respuesta del botón marcada.
3. Al pulsar «Enviar respuestas», las respuestas se guardan en la ficha de la pareja en HubSpot.

Ahora mismo está en **modo prueba**: no envía nada y, al terminar, enseña lo que se enviaría. Para dejarla funcionando hay que hacer cinco pasos, y solo una vez.

---

## Paso 1. Crear en HubSpot los campos donde se guardan las respuestas

En HubSpot: **icono del engranaje (Configuración) → Propiedades → Propiedades de contacto → Crear propiedad**.

Crea estas propiedades. El «nombre interno» tiene que ser **exactamente** el que pone aquí. HubSpot lo rellena solo a partir de la etiqueta; revísalo y corrígelo si no coincide.

**Encuesta de perdidos**

| Etiqueta (lo que ves) | Nombre interno | Tipo de campo |
|---|---|---|
| Motivo de no reserva | `motivo_no_reserva` | Texto de una línea |
| Motivo de no reserva (otro) | `motivo_no_reserva_otro` | Texto de una línea |
| Valoración atención Travel Designer | `valoracion_atencion_td` | Número |
| Qué podríamos mejorar | `que_podriamos_mejorar` | Texto de varias líneas |
| Quiere ideas de viaje | `quiere_ideas_viaje` | Texto de una línea |

**Encuesta de vuelta de viaje**

| Etiqueta | Nombre interno | Tipo de campo |
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

Las respuestas llegan siempre en español de España, también las de México («Precio», «Por libre», «Sí», «No»...). Así los informes juntan los dos mercados.

## Paso 2. Crear dos formularios en HubSpot y apuntar sus códigos

En HubSpot: **Marketing → Formularios → Crear formulario**.

1. Crea uno llamado **«Encuesta perdidos»**. Añádele el campo *Correo* y los cinco campos de perdidos del paso 1.
2. Crea otro llamado **«Encuesta vuelta de viaje»**. Añádele *Correo* y los campos de vuelta de viaje.
3. Publica los dos. Nadie va a ver estos formularios: sirven de buzón para recibir las respuestas.
4. Apunta dos códigos de cada formulario. Los encuentras en el botón **Compartir** o **Insertar**, dentro del código que aparece:
   - `portalId`: un número, por ejemplo `12345678`. Es el mismo en los dos formularios.
   - `formId`: un código largo con guiones, por ejemplo `a1b2c3d4-...`. Es distinto en cada formulario.

## Paso 3. Poner esos códigos en la encuesta

1. Abre `index.html` con un editor de texto sencillo: el Bloc de notas en Windows, o TextEdit en Mac en modo «texto sin formato».
2. Busca el bloque que empieza por `AJUSTES`. Está al principio del código JavaScript.
3. Cámbialo así, con vuestros códigos:

```js
modo: 'hubspot',
hubspot: {
  portalId: '12345678',
  formPerdidos: 'código-del-formulario-perdidos',
  formViaje: 'código-del-formulario-vuelta-de-viaje'
},
```

4. En el mismo bloque, cambia también:
   - `resenaGoogle`: pon el enlace directo para dejar reseña. En vuestro Perfil de Empresa de Google: «Pedir reseñas» → copiar enlace.
   - `privacidad`: comprueba que la dirección de vuestra política de privacidad es la correcta.
5. Guarda el archivo.

## Paso 4. Publicar la página en internet (gratis)

La forma más sencilla es Netlify Drop:

1. Entra en https://app.netlify.com/drop y crea una cuenta gratuita.
2. Arrastra la carpeta `encuesta-landing` entera a la página.
3. En unos segundos te da una dirección del tipo `https://nombre-raro.netlify.app`. Esa es tu encuesta.
4. Opcional: en Netlify, *Domain settings*, puedes ponerle una dirección propia, como `encuesta.honimunn.com`. Quien gestione vuestro dominio tendrá que añadir un registro, y Netlify explica cuál.

Si más adelante cambias algo en `index.html`, vuelve a arrastrar la carpeta en la pestaña *Deploys* del mismo sitio.

## Paso 5. Los enlaces de los botones del email

En el email de HubSpot, cada botón lleva un enlace a la encuesta con unos datos al final. Cambia `TU-ENCUESTA` por la dirección del paso 4.

**Vuelta de viaje.** Un botón por cada número del 0 al 10, y solo cambia la cifra final:

```
https://TU-ENCUESTA/?tipo=viaje&mercado=es&nombre={{contact.firstname}}&email={{contact.email}}&r=9
```

**Perdidos.** Un botón por cada motivo. Al final va `r=precio`, `r=otra`, `r=libre`, `r=tiempo`, `r=propuesta`, `r=planes` u `r=otro`:

```
https://TU-ENCUESTA/?tipo=perdidos&mercado=es&nombre={{contact.firstname}}&email={{contact.email}}&r=precio
```

- Para México, cambia `mercado=es` por `mercado=mx`.
- Los textos entre llaves `{{...}}` los rellena HubSpot con los datos de cada contacto. Puedes insertarlos con el botón *Personalizar* del editor de enlaces.
- **El `email` es imprescindible.** Sin él, HubSpot recibe la respuesta pero no sabe de qué pareja es.

## Paso 6. Probarla

Mándate el email a ti mismo, toca un botón, rellena la encuesta y envíala. En un par de minutos, las respuestas deberían aparecer en tu ficha de contacto en HubSpot.

**Si sale el mensaje «No hemos podido enviar vuestras respuestas»**, casi siempre es por una de estas tres cosas:
- Un código del paso 3 está mal copiado.
- Falta crear una propiedad del paso 1, o su nombre interno no coincide.
- La cuenta tiene activado el consentimiento RGPD en los formularios. Revísalo en la configuración del formulario.

---

## Recomendado: que HubSpot actúe solo

Con las respuestas en HubSpot se pueden montar flujos de trabajo (*Automatización → Flujos de trabajo*):

- **NPS de 0 a 6** → crear una tarea «Llamar en 48 h» para el Travel Designer. Es lo que la encuesta promete a la pareja, así que hay que cumplirlo.
- **NPS de 7 u 8** → tarea «Escribir para entender qué faltó».
- **Quiere ideas de viaje = Sí** → añadir a la lista de Munnviajes y repetidores.

## Otras opciones

- **Sin HubSpot:** con `modo: 'webhook'` y una dirección de Make, Zapier o Google Apps Script en `webhookUrl`, las respuestas pueden ir a una hoja de Google.
- **Typeform:** no se puede conectar a esta página. Habría que rehacer las preguntas dentro de Typeform y se perdería este diseño.
