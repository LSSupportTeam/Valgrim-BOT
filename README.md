# Valgrim Bot - Bot de Discord para Comunidades de Minecraft

Este bot de Discord, llamado Valgrim Bot, está diseñado para mejorar la gestión y la interacción de un servidor de comunidad de Minecraft. Proporciona funciones como verificación de usuarios, sistema de tickets de soporte, envío de sugerencias y embeds personalizables.

## Tabla de Contenidos

* [Funciones](#funciones)
* [Configuración](#configuración)
* [Explicación del Código](#explicación-del-código)
    * [Dependencias](#dependencias)
    * [Estructura de Archivos](#estructura-de-archivos)
    * [Configuración](#configuración-1)
    * [Inicialización del Bot](#inicialización-del-bot)
    * [Sistema de Verificación](#sistema-de-verificación)
    * [Sistema de Tickets](#sistema-de-tickets)
    * [Sistema de Sugerencias](#sistema-de-sugerencias)
    * [Embeds Personalizados](#embeds-personalizados)
    * [Mensajes de Bienvenida](#mensajes-de-bienvenida)
    * [Comandos de Barra (Slash Commands)](#comandos-de-barra-slash-commands)
    * [Interacciones con Modales](#interacciones-con-modales)
    * [Interacciones con Botones](#interacciones-con-botones)
    * [Manejo de Errores](#manejo-de-errores)
    * [Guardado de Archivos](#guardado-de-archivos)
    * [Variables de Entorno](#variables-de-entorno)
    * [Inicio de Sesión del Bot](#inicio-de-sesión-del-bot)
* [Uso](#uso)
    * [Verificación](#verificación-1)
    * [Creación de Tickets](#creación-de-tickets)
    * [Envío de Sugerencias](#envío-de-sugerencias)
    * [Creación de Embeds (Solo Desarrolladores)](#creación-de-embeds-solo-desarrolladores)
* [Notas](#notas)

## Funciones

* **Verificación de Cuenta de Minecraft:** Los usuarios pueden verificar sus cuentas de Minecraft a través de un modal, vinculando su ID de Discord con su nombre en el juego.
* **Sistema de Tickets de Soporte:** Los usuarios pueden crear tickets para reportar jugadores, reclamar compras o solicitar soporte general. El personal puede administrar y cerrar los tickets con registro de transcripciones.
* **Sistema de Sugerencias:** Los usuarios pueden enviar sugerencias, y estas se publican en un canal designado para la votación de la comunidad (reacciones 👍/👎).
* **Embeds Personalizados:** Los desarrolladores pueden crear y enviar embeds personalizados a cualquier canal utilizando un comando de barra y un formulario modal.
* **Mensajes de Bienvenida:** Los nuevos miembros reciben un mensaje de bienvenida personalizado en un canal designado.
* **Comandos de Barra (Slash Commands):** Utiliza los comandos de barra de Discord para una experiencia más interactiva y fácil de usar (por ejemplo, `/createembed`, `/setup-suggest`).
* **Interacciones con Modales:** Utiliza modales para recopilar la entrada del usuario para la verificación, la creación de tickets, las sugerencias y la creación de embeds.
* **Interacciones con Botones:** Utiliza botones para activar acciones como abrir modales, crear tickets y cerrar tickets.
* **Registro de Transcripciones:** Cuando se cierra un ticket, se guarda una transcripción de los mensajes del canal como un archivo JSON y se envía a un canal de registros.
* **Archivos de Configuración:** Utiliza `config.json` y `users.json` para almacenar la configuración del bot y los datos de verificación de usuarios.

## Configuración

1.  **Instalar Node.js:** Asegúrate de tener Node.js instalado en tu sistema.
2.  **Instalar Dependencias:**
    ```bash
    npm install discord.js dotenv
    ```
3.  **Configurar Variables de Entorno:**
    * Crea un archivo `.env` en el mismo directorio que `index.js`.
    * Agrega las siguientes variables, reemplazando los marcadores de posición con tus valores reales:
        ```
        BOT_TOKEN=TU_TOKEN_DE_BOT
        VERIFICATION_CHANNEL_ID=TU_ID_DEL_CANAL_DE_VERIFICACIÓN
        VERIFICATION_ROLE_ID=TU_ID_DEL_ROL_DE_VERIFICACIÓN  (Opcional)
        TICKET_CHANNEL_ID=TU_ID_DEL_CANAL_DE_TICKETS
        TICKET_LOGS_CHANNEL_ID=TU_ID_DEL_CANAL_DE_REGISTROS_DE_TICKETS (Opcional)
        WELCOME_CHANNEL_ID=TU_ID_DEL_CANAL_DE_BIENVENIDA (Opcional)
        DEVELOPERS_ID=TU_ID_DE_USUARIO_DE_DISCORD,OTRO_ID_DE_USUARIO (Lista separada por comas, Opcional)
        SUGGEST_CHANNEL_ID=TU_ID_DEL_CANAL_DE_SUGERENCIAS (Opcional)
        SUGGEST_SEND_CHANNEL_ID=TU_ID_DEL_CANAL_DE_ENVÍO_DE_SUGERENCIAS (Opcional)
        ```
4.  **Ejecutar el Bot:**
    ```bash
    node index.js
    ```

## Explicación del Código

### Dependencias

* **discord.js:** Una poderosa biblioteca de Node.js para interactuar con la API de Discord. Se utiliza para todo, desde la conexión a Discord hasta la gestión de mensajes, usuarios y canales.
* **dotenv:** Un módulo que carga las variables de entorno desde un archivo `.env` a `process.env`. Esto es crucial para almacenar datos confidenciales como el token del bot de forma segura.
* **fs (Sistema de Archivos):** El módulo de sistema de archivos incorporado de Node.js se utiliza para leer y escribir datos en archivos (`config.json`, `users.json` y archivos de transcripción).

### Estructura de Archivos

* `index.js`: El archivo principal que contiene el código del bot.
* `config.json`: Almacena la configuración del bot (por ejemplo, los ID de los mensajes).
* `users.json`: Almacena la asignación de nombres de usuario de Minecraft a los ID de usuario de Discord para la verificación.
* `data/`: Directorio para almacenar las transcripciones de los tickets (archivos JSON).
* `.env`: (No incluido en el repositorio por seguridad) Almacena información confidencial como el token del bot y los ID de los canales.

### Configuración

El bot utiliza dos archivos JSON para la configuración:

* **`config.json`:** Este archivo almacena los ID de los mensajes importantes (como el embed de verificación y el embed de los tickets). Esto permite que el bot actualice o interactúe con estos mensajes específicos después de un reinicio. Inicialmente se crea como un objeto JSON vacío si no existe.
* **`users.json`:** Este archivo almacena una asignación de nombres de usuario de Minecraft a los ID de usuario de Discord. Así es como el bot realiza un seguimiento de qué usuario de Discord ha verificado qué cuenta de Minecraft. También se crea como un objeto JSON vacío si no existe.

Las funciones `saveConfig()` y `saveUsers()` se utilizan para actualizar estos archivos cada vez que se realizan cambios.

### Inicialización del Bot

* El código inicializa un nuevo cliente de Discord con intents específicos. Los intents definen qué tipos de eventos recibirá el bot de Discord (por ejemplo, mensajes de gremio, miembros del gremio).
* Lee los archivos `config.json` y `users.json`.
* El listener de eventos `client.once('ready', ...)` es crucial. Este código se ejecuta *solo una vez*, cuando el bot se conecta por primera vez a Discord. Se utiliza para:
    * Registrar un mensaje de "bot listo".
    * Configurar los mensajes del sistema de verificación y tickets (enviándolos si no existen, o buscándolos y actualizándolos si existen).
    * Registrar los comandos de barra.
* El bot usa `process.env` para acceder a las variables de entorno.

### Sistema de Verificación

* Se envía un mensaje de verificación a un canal especificado (`VERIFICATION_CHANNEL_ID`).
* Este mensaje contiene un embed con instrucciones y un botón ("✅ Verificar").
* Cuando un usuario hace clic en el botón, se muestra un modal (`minecraft_username_modal`), que le solicita que ingrese su nombre de usuario de Minecraft.
* El bot valida el nombre de usuario (longitud y caracteres permitidos).
* Comprueba si el nombre de usuario ya está registrado o si el usuario de Discord ya se ha verificado.
* Si la verificación es exitosa:
    * El nombre de usuario y el ID de Discord se almacenan en `users.json`.
    * El bot intenta dar al usuario un rol de verificación (`VERIFICATION_ROLE_ID`).
    * El bot intenta cambiar el apodo del usuario a su nombre de usuario de Minecraft.
* Se incluye el manejo de errores para gestionar los casos en los que no se encuentra el rol, el bot no tiene permisos o el nombre de usuario no es válido.

### Sistema de Tickets

* Se envía un mensaje del sistema de tickets a un canal especificado (`TICKET_CHANNEL_ID`).
* Este mensaje contiene un embed con información sobre el sistema de soporte y botones para diferentes tipos de tickets ("👮 Reportar Jugador", "❗ Reclamar Compra", "❓ Soporte General").
* Cuando un usuario hace clic en un botón de ticket, se muestra un modal para recopilar detalles sobre el problema.
* El bot crea un nuevo canal de texto llamado `ticket-nombredeusuario` en la categoría especificada.
* Los permisos se configuran para que solo el usuario, los roles del personal (`STAFF_ROLE_IDS`) y el bot puedan ver e interactuar con el canal del ticket.
* Se envía un embed al canal del ticket con la información del usuario y los detalles de su solicitud.
* Se agrega un botón de "🔒 Cerrar Ticket" al canal del ticket.
* La creación del ticket se registra en un canal de registros especificado (`TICKET_LOGS_CHANNEL_ID`).

### Sistema de Sugerencias

* El comando de barra `/setup-suggest` (solo para desarrolladores) crea un embed en el `SUGGEST_CHANNEL_ID` con instrucciones sobre cómo enviar sugerencias.
* Un botón de "💡 Crear Sugerencia" abre el `suggest_modal`.
* Los usuarios envían sugerencias a través del modal.
* El bot envía la sugerencia como un embed al `SUGGEST_SEND_CHANNEL_ID`.
* El bot agrega reacciones 👍 y 👎 al mensaje de sugerencia para la votación.

### Embeds Personalizados

* El comando de barra `/createembed` (solo para desarrolladores) permite a los desarrolladores crear embeds personalizados.
* Se utiliza un modal (`embed_modal`) para recopilar el título, la descripción, el color, la URL de la imagen y el pie de página del embed.
* El bot construye y envía el embed al canal donde se utilizó el comando.
* Se realiza la validación del color y la validación de la URL de la imagen.

### Mensajes de Bienvenida

* El listener de eventos `client.on('guildMemberAdd', ...)` se activa cuando un nuevo miembro se une al servidor.
* El bot envía un mensaje de bienvenida a un canal especificado (`WELCOME_CHANNEL_ID`).
* El mensaje de bienvenida incluye el nombre del usuario, una descripción del servidor e instrucciones sobre cómo comenzar.

### Comandos de Barra (Slash Commands)

* Los comandos de barra se registran utilizando `client.application?.commands.set()`.
* El bot utiliza comandos de barra para `/createembed` (crear embed personalizado) y `/setup-suggest` (configurar canal de sugerencias).
* Los permisos se pueden manejar verificando los ID de los usuarios (por ejemplo, solo los desarrolladores pueden usar `/createembed`).

### Interacciones con Modales

* `client.on('interactionCreate', async (interaction) => { ... })` maneja todas las interacciones.
* `interaction.isModalSubmit()` se utiliza para procesar los datos enviados a través de los modales.
* El bot extrae los datos de los campos del modal utilizando `interaction.fields.getTextInputValue()`.
* El bot realiza acciones basadas en el `customId` del modal (por ejemplo, verificar un usuario, crear un ticket, enviar un embed).

### Interacciones con Botones

* `interaction.isButton()` se utiliza para manejar los clics de los botones.
* El bot identifica el botón en el que se hizo clic utilizando `interaction.customId`.
* Los clics de los botones activan acciones como abrir modales, crear tickets y cerrar tickets.
* La función `closeTicket()` maneja el cierre de los tickets, la generación de transcripciones y su envío al canal de registros.

### Manejo de Errores

* El código incluye un extenso manejo de errores mediante bloques `try...catch`.
* Los errores se registran en la consola utilizando `console.error()`.
* El bot envía mensajes de error informativos al usuario en el canal o como mensajes efímeros (visibles solo para el usuario).
* El bot maneja los casos en los que no se encuentran canales, roles o variables de entorno.

### Guardado de Archivos

* El módulo `fs` se utiliza para guardar datos en `config.json`, `users.json` y archivos de transcripción.
* `fs.writeFileSync()` se utiliza para escribir datos de forma síncrona.
* `JSON.stringify()` se utiliza para convertir objetos de JavaScript en cadenas JSON.
* Las transcripciones se guardan como archivos JSON en el directorio `data/`, con el nombre `transcript-ticketId.json`.

### Variables de Entorno

* El archivo `.env` se utiliza para almacenar información confidencial y ajustes de configuración.
* `dotenv.config()` carga estas variables en `process.env`.
* El bot accede a las variables de entorno utilizando `process.env.VARIABLE_NAME`.
* Este enfoque es crucial para la seguridad (mantener el token del bot en secreto) y la flexibilidad (permitir cambios de configuración sin modificar el código).

### Inicio de Sesión del Bot

* El bot inicia sesión en Discord utilizando `client.login(token)`.
* El token del bot se recupera de la variable de entorno `BOT_TOKEN`.
* Si no se encuentra el token, el bot registra un error fatal y se cierra.

## Uso

### Verificación

1.  Haz clic en el botón "✅ Verificar" en el canal de verificación.
2.  Ingresa tu nombre de usuario de Minecraft en el modal.
3.  El bot verificará tu cuenta, asignará el rol de verificación y cambiará tu apodo (si está configurado y los permisos lo permiten).

### Creación de Tickets

1.  Haz clic en el botón apropiado para tu problema en el canal de tickets:
    * "👮 Reportar Jugador"
    * "❗ Reclamar Compra"
    * "❓ Soporte General"
2.  Completa el modal con los detalles necesarios.
3.  El bot creará un nuevo canal de tickets y notificará al personal.

### Envío de Sugerencias

1.  Haz clic en el botón "💡 Crear Sugerencia" en el canal de sugerencias.
2.  Ingresa tu sugerencia en el modal.
3.  El bot publicará tu sugerencia en el canal de sugerencias para la votación.

### Creación de Embeds (Solo Desarrolladores)

1.  Usa el comando de barra `/createembed`.
2.  Completa el modal con los detalles del embed.
3.  El bot enviará el embed al canal donde usaste el comando.

## Notas

* Este bot requiere una configuración cuidadosa de las variables de entorno para funcionar correctamente.
* Los permisos adecuados para el bot y los roles son esenciales para funciones como la asignación de roles y los cambios de apodo.
* El bot utiliza el almacenamiento de archivos, así que asegúrate de que el proceso del bot tenga acceso de escritura al directorio `config.json`, `users.json` y `data/`.
* El código incluye el manejo de errores, pero se recomienda realizar pruebas exhaustivas.
* La funcionalidad del bot se puede ampliar agregando más listeners de eventos, comandos de barra e interacciones.
