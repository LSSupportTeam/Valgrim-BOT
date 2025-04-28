# 🤖 Valgrim Bot - ¡Tu Bot de Discord para la Comunidad de Minecraft! ⛏️

¡Hola! 👋 Valgrim Bot es un bot de Discord diseñado para facilitar la gestión y dinamizar la interacción en tu servidor de Minecraft.  Desde la verificación de usuarios hasta un completo sistema de tickets de soporte, ¡Valgrim Bot lo tiene todo!

## 🌟 Características Principales

* **✅ Verificación de Minecraft:** ¡Verifica a tus jugadores de forma sencilla!  Vincula las cuentas de Minecraft con los perfiles de Discord.
* **🎫 Sistema de Tickets de Soporte:** ¿Necesitas ayuda?  ¡Crea tickets para reportar jugadores, reclamar compras o solicitar asistencia general!  El staff puede gestionar y cerrar tickets eficientemente.
* **💡 Sugerencias de la Comunidad:** ¡Recoge las ideas de tus miembros!  Permite a los usuarios enviar sugerencias y votarlas con reacciones 👍 y 👎.
* **🎨 Embeds Personalizados:** ¡Crea mensajes atractivos y visuales! Los desarrolladores pueden diseñar embeds con títulos, descripciones, colores, imágenes y pies de página.
* **👋 Mensajes de Bienvenida:** ¡Da la bienvenida a los nuevos miembros!  Envía mensajes personalizados a los recién llegados.
* **⌨️ Comandos de Barra (Slash Commands):** ¡Interactúa con el bot de forma intuitiva!  Utiliza los comandos de barra para acceder a las funciones.
* **✨ Modales Interactivos:** ¡Recopila información fácilmente!  Los modales permiten a los usuarios ingresar datos para la verificación, tickets, sugerencias y creación de embeds.
* **🖱️ Botones Dinámicos:** ¡Realiza acciones con un solo clic!  Los botones abren modales, crean tickets y cierran tickets.
* **📜 Transcripciones de Tickets:** ¡Guarda un registro de las conversaciones!  Las transcripciones de los tickets cerrados se guardan en archivos JSON.

## ⚙️ Configuración

1.  **⬇️ Instala Node.js:** Asegúrate de tener Node.js instalado en tu sistema.
2.  **📦 Instala Dependencias:**

    ```bash
    npm install discord.js dotenv
    ```

3.  **🔑 Configura las Variables de Entorno:**

    * Crea un archivo `.env` en el mismo directorio que `index.js`.
    * Añade las siguientes variables (¡reemplaza los valores de ejemplo!):

        ```
        BOT_TOKEN=TU_TOKEN_DE_BOT  # 🤖 El token de tu bot de Discord
        VERIFICATION_CHANNEL_ID=TU_ID_DEL_CANAL_DE_VERIFICACION  # ✅ ID del canal para la verificación
        VERIFICATION_ROLE_ID=TU_ID_DEL_ROL_DE_VERIFICACION  # 🛡️ (Opcional) ID del rol que se asigna a los usuarios verificados
        TICKET_CHANNEL_ID=TU_ID_DEL_CANAL_DE_TICKETS  # 🎫 ID del canal donde se crean los tickets
        TICKET_LOGS_CHANNEL_ID=TU_ID_DEL_CANAL_DE_REGISTROS_DE_TICKETS  # 📜 (Opcional) ID del canal para los registros de tickets
        WELCOME_CHANNEL_ID=TU_ID_DEL_CANAL_DE_BIENVENIDA  # 🎉 (Opcional) ID del canal para los mensajes de bienvenida
        DEVELOPERS_ID=TU_ID_DE_USUARIO_DE_DISCORD,OTRO_ID_DE_USUARIO  # 🧑‍💻 (Opcional) IDs de los desarrolladores (separados por comas)
        SUGGEST_CHANNEL_ID=TU_ID_DEL_CANAL_DE_SUGERENCIAS  # 💡 (Opcional) ID del canal donde se inicia el proceso de sugerencias
        SUGGEST_SEND_CHANNEL_ID=TU_ID_DEL_CANAL_DE_ENVIO_DE_SUGERENCIAS  # 📨 (Opcional) ID del canal donde se publican las sugerencias
        ```

4.  **🚀 Ejecuta el Bot:**

    ```bash
    node index.js
    ```

## 👨‍💻 Explicación del Código

### 📚 Dependencias

* **discord.js:** Una potente librería de Node.js para interactuar con la API de Discord.
* **dotenv:** Carga las variables de entorno desde un archivo `.env`.
* **fs (File System):** Módulo de Node.js para trabajar con archivos.

### 📂 Estructura de Archivos

├── index.js          # 🤖 El archivo principal del bot
├── config.json       # ⚙️ Configuración del bot (IDs de mensajes, etc.)
├── users.json        # 🤝 Datos de verificación de usuarios (Minecraft username <-> Discord ID)
├── data/             # 📁 (Opcional) Directorio para las transcripciones de tickets
└── .env              # 🤫 (¡No incluir en el repositorio!)  Variables de entorno (token, IDs de canales)

### ⚙️ Configuración (Archivos)

* **`config.json`:** Almacena los IDs de mensajes clave.
* **`users.json`:** Guarda la relación entre los nombres de usuario de Minecraft y los IDs de Discord.

    * `saveConfig()` y `saveUsers()` actualizan estos archivos.

### 🚀 Inicialización del Bot

* Crea un nuevo cliente de Discord con intents.
* Lee `config.json` y `users.json`.
* `client.once('ready', ...)`:  Se ejecuta una vez cuando el bot se conecta.
    * Registra el bot como "listo".
    * Configura los mensajes de verificación y tickets.
    * Registra los comandos de barra.
* Accede a las variables de entorno con `process.env`.

### ✅ Sistema de Verificación

* Envía un mensaje de verificación con un botón.
* Al hacer clic, se abre un modal para ingresar el nombre de usuario de Minecraft.
* Valida el nombre de usuario.
* Comprueba si ya está registrado o verificado.
* Si tiene éxito:
    * Guarda los datos en `users.json`.
    * Asigna el rol de verificación (si está configurado).
    * Cambia el apodo del usuario (si está configurado).
* Maneja errores (rol no encontrado, permisos, nombre de usuario no válido).

### 🎫 Sistema de Tickets

* Envía un mensaje con botones para diferentes tipos de tickets.
* Al hacer clic, se abre un modal para los detalles.
* Crea un nuevo canal de texto `ticket-nombredeusuario`.
* Configura los permisos del canal (usuario, staff, bot).
* Envía un embed con la información del usuario y la solicitud.
* Añade un botón para cerrar el ticket.
* Registra la creación del ticket.

### 💡 Sistema de Sugerencias

* `/setup-suggest` (solo desarrolladores) crea un embed con instrucciones.
* Un botón "💡 Crear Sugerencia" abre el modal.
* Los usuarios envían sugerencias a través del modal.
* El bot envía la sugerencia como un embed y añade reacciones 👍 y 👎.

### 🎨 Embeds Personalizados

* `/createembed` (solo desarrolladores) permite crear embeds.
* Un modal recopila el título, la descripción, el color, la imagen y el pie de página.
* El bot envía el embed.
* Valida el color y la URL de la imagen.

### 👋 Mensajes de Bienvenida

* `client.on('guildMemberAdd', ...)`:  Se ejecuta cuando un nuevo miembro se une.
* Envía un mensaje de bienvenida.

### ⌨️ Comandos de Barra (Slash Commands)

* Se registran con `client.application?.commands.set()`.
* Ejemplos: `/createembed`, `/setup-suggest`.
* Se pueden gestionar los permisos por usuario.

### ✨ Modales Interactivos

* `client.on('interactionCreate', ...)`:  Maneja todas las interacciones.
* `interaction.isModalSubmit()`:  Procesa los datos de los modales.
* Obtiene los datos del modal con `interaction.fields.getTextInputValue()`.

### 🖱️ Botones Dinámicos

* `interaction.isButton()`:  Maneja los clics de los botones.
* Identifica el botón con `interaction.customId`.
* Los clics abren modales, crean tickets y cierran tickets.
* `closeTicket()` cierra los tickets, genera transcripciones y las envía al canal de registros.

### 🚨 Manejo de Errores

* `try...catch`:  Manejo de errores.
* `console.error()`:  Registra los errores.
* Envía mensajes de error a los usuarios.
* Maneja casos como canales o roles no encontrados.

### 💾 Guardado de Archivos

* `fs.writeFileSync()`:  Guarda datos en archivos.
* `JSON.stringify()`:  Convierte objetos a JSON.
* Las transcripciones se guardan en `data/` como `transcript-ticketId.json`.

### 🔑 Variables de Entorno

* Archivo `.env`:  Almacena información confidencial.
* `dotenv.config()`:  Carga las variables.
* Accede a las variables con `process.env.VARIABLE_NAME`.

### 🚪 Inicio de Sesión del Bot

* `client.login(token)`:  Inicia sesión en Discord.
* Obtiene el token de `process.env.BOT_TOKEN`.
* Si no se encuentra el token, muestra un error y se cierra.

## 🚀 Uso

### ✅ Verificación

1.  Haz clic en "✅ Verificar".
2.  Ingresa tu nombre de usuario de Minecraft.
3.  El bot verifica, asigna el rol y cambia el apodo (si está configurado).

### 🎫 Creación de Tickets

1.  Haz clic en el botón del tipo de ticket.
2.  Completa el modal.
3.  El bot crea un canal de ticket y notifica al staff.

### 💡 Envío de Sugerencias

1.  Haz clic en "💡 Crear Sugerencia".
2.  Ingresa tu sugerencia.
3.  El bot la publica para votar.

### 🎨 Creación de Embeds (Desarrolladores)

1.  Usa `/createembed`.
2.  Completa el modal del embed.
3.  El bot envía el embed.

## ⚠️ Notas Importantes

* ¡Configura bien las variables de entorno!
* ¡Los permisos son cruciales!
* Asegúrate de que el bot pueda escribir en los archivos.
* ¡Prueba el bot a fondo!
* ¡Puedes ampliarlo con más funciones!

¡Espero que te guste Valgrim Bot!  Si tienes alguna pregunta o sugerencia, ¡no dudes en abrir un issue! 💖
