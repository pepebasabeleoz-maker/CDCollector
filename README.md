# CD Collector — instalación gratis desde Windows

Es una PWA (app web instalable), NO una aplicación Swift nativa. Necesita HTTPS para la cámara y una conexión para consultar Discogs.

## 1. Obtener token de Discogs

Iniciá sesión en Discogs. En Settings > Developers, generá un personal access token. **Nunca lo pegues en index.html, app.js, GitHub ni en el navegador.**

## 2. Crear el proxy gratuito en Cloudflare

Creá una cuenta gratuita en https://dash.cloudflare.com/ y abrí Workers & Pages > Create > Worker. Pegá el contenido de `worker.js` y desplegalo. En Settings > Variables and Secrets añadí `DISCOGS_TOKEN` como **Secret** con tu token. Copiá la URL HTTPS pública del Worker (ejemplo `https://cdcollector.usuario.workers.dev`). La ruta `/search?q=...` debe devolver JSON con `results`.

Advertencia: el Worker de ejemplo acepta consultas públicas y usa tu token, por lo que otra persona con su URL podría consumir tu cuota de API. Es apropiado para pruebas personales; para uso permanente conviene restringir orígenes, agregar control de acceso y limitación de peticiones. CORS no es un mecanismo de autenticación.

## 3. Publicar la web gratis

En GitHub creá un repositorio público, subí `index.html`, `app.js`, `sw.js`, `manifest.webmanifest` e `icon.svg` (NO `worker.js` con secretos; el archivo no contiene el token, pero tampoco es necesario publicarlo). Activá Settings > Pages > Deploy from branch > main > root. Esperá a que GitHub muestre tu URL HTTPS.

## 4. Instalar en iPhone

Abrí esa URL en Safari > Compartir > Añadir a pantalla de inicio > Añadir. Abrí CD Collector desde su ícono. Entrá a Ajustes, pegá la URL del Worker y guardá. Permití la cámara cuando escanees.

## 5. Uso y límites

Buscá por código de barras, título o número de catálogo; elegí la edición y tocá Añadir. La colección queda en el almacenamiento local de ese navegador/iPhone. **No se añade automáticamente a la colección de tu cuenta Discogs.** Exportá JSON periódicamente. No existe sincronización entre dispositivos. El escáner usa `html5-qrcode` cargado desde un CDN; necesita internet para la primera carga y la compatibilidad depende de Safari. Siempre existe búsqueda manual.

Para una futura sincronización de escritura con Discogs se necesita autenticación OAuth y un backend seguro. No introduzcas contraseñas de Discogs en esta aplicación.
