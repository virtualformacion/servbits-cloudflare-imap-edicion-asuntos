# SERVBITS 1.3 — Cloudflare Pages, IMAP y permisos por usuario

Para actualizar la instalación existente, lee primero **ACTUALIZAR-ASUNTOS-Y-PERMISOS.md**. Incluye la lista exacta de archivos y qué datos conservar.

- Build: `npm run build`; directorio de salida: `dist`.
- Variables: `GITHUB_TOKEN`, `GITHUB_REPO`, `GITHUB_FILE_PATH`, `GITHUB_BRANCH` y `MAILBOX_ENCRYPTION_KEY`.
- Usuarios, contraseñas, vencimientos, correos autorizados y `disabledSubjects`: `script.js` en GitHub.
- Administrador: `FIXED_ADMIN` en `functions/api/updateusers.js`.
- Asuntos: `lib/mail-rules.js` en GitHub.
- Buzones: `mailboxes.json` en GitHub, con claves de aplicación cifradas AES-GCM. Conserva la clave de cifrado actual de Cloudflare.

En el panel puedes gestionar usuarios, buzones y asuntos de códigos, además de elegir consultas permitidas de forma independiente para cada usuario. Los usuarios antiguos y los nuevos tienen todos los asuntos habilitados por defecto.

Para cada buzón Gmail, introduce el correo y su clave de aplicación de 16 letras. El panel acepta espacios y los elimina. Carpeta vacía detecta Todos los correos; INBOX consulta solo recibidos. Puedes crear, editar, probar, activar/desactivar y eliminar buzones. Una clave vacía al editar conserva la actual. IMAP usa TLS directo a imap.gmail.com:993, lectura con EXAMINE y BODY.PEEK.

Se consultan hasta diez buzones activos, dos en paralelo, y los treinta mensajes más recientes por buzón dentro de diez minutos. Se mantienen los resultados HTML/texto o redirección a Netflix, los reenvíos identificables por destinatario y los alias Gmail, conservando las extensiones +tag. No se borran correos ni se marcan como leídos. Se mantienen los límites de memoria y tiempo del cliente IMAP anterior y las cuotas del plan de Cloudflare/GitHub.

El servidor exige una sesión de usuario y comprueba sus permisos y correos autorizados actuales antes de entregar resultados. Las sesiones duran ocho horas. Los datos siguen almacenados con el formato de GitHub solicitado; esta mejora no cambia el almacenamiento de contraseñas de usuarios.

`npm test` ejecuta las pruebas automatizadas. No incluye una conexión real a tus buzones; prueba tus casos de correo después de desplegar. No subas secretos, node_modules ni dist al repositorio.
