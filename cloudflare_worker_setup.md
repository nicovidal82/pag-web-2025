# Configuración del Cloudflare Worker para el Formulario de Contacto

Este documento describe cómo configurar y desplegar el Cloudflare Worker (`worker.js`) para manejar el envío de correos electrónicos del formulario de contacto de SYNOTEC.

## 1. Requisitos Previos

1.  Una cuenta de **Cloudflare** activa.
2.  Una cuenta de **SendGrid** (o un servicio de correo transaccional similar) para el envío de correos.
3.  Una **clave API de SendGrid** con permisos para enviar correos.
4.  Una dirección de correo electrónico de remitente verificada en SendGrid (ej. `no-reply@synotec.cl`).

## 2. Despliegue del Worker

1.  **Cree un nuevo Worker** en su panel de Cloudflare.
2.  Copie el contenido del archivo `worker.js` y péguelo en el editor del Worker.
3.  **Guarde y Despliegue** el Worker. Anote la URL del Worker (ej. `https://synotec-form-handler.yourusername.workers.dev`).

## 3. Configuración de Variables de Entorno (Secrets)

Para que el Worker funcione de forma segura, debe configurar las siguientes variables como **Secrets** en la configuración de su Worker (no las ponga directamente en el código):

| Nombre del Secret | Descripción | Valor de Ejemplo |
| :--- | :--- | :--- |
| `SENDGRID_API_KEY` | Su clave API de SendGrid. | `SG.xxxxxxxxxxxxxxxxxxxx` |
| `SENDER_EMAIL` | El correo electrónico verificado en SendGrid que usará como remitente. | `no-reply@synotec.cl` |

**Nota Importante:** El Worker está configurado para enviar el correo a **`nvidal@synotec.cl`** (variable `RECIPIENT_EMAIL` dentro de `worker.js`).

## 4. Configuración del Frontend (Página Web)

Una vez que el Worker esté desplegado y tenga su URL, debe actualizar el archivo `script.js` de la página web.

1.  Abra el archivo `script.js` de su proyecto.
2.  Busque la línea que define la URL del Worker y reemplace el placeholder con la URL real de su Worker:

    ```javascript
    // En script.js
    const WORKER_URL = 'https://SU-URL-DEL-WORKER.workers.dev'; 
    ```

3.  Asegúrese de que la función de envío en `script.js` esté utilizando `fetch` para enviar los datos del formulario a esta `WORKER_URL`.

## 5. Pruebas

Después de actualizar `script.js` y desplegar el Worker, pruebe el formulario. Si todo está configurado correctamente, el Worker recibirá los datos y SendGrid enviará el correo a `nvidal@synotec.cl`.

**¡Listo! Con esta configuración, su formulario de contacto estará enviando correos de forma segura y eficiente a través de Cloudflare Workers.**
