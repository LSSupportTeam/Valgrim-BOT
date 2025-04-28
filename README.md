# 🤖 Valgrim Bot - El Bot de Discord Definitivo para tu Comunidad de Minecraft ⛏️

¡Bienvenido! 👋 Valgrim Bot es una herramienta poderosa y versátil diseñada para simplificar la administración y enriquecer la experiencia de tu servidor de Discord de Minecraft. Ofrece una amplia gama de funciones, desde la verificación de usuarios hasta un sistema de soporte completo y la gestión de sugerencias de la comunidad.

## 📌 Índice

1.  [🌟 Características](#1-características)
2.  [🚀 Inicio Rápido](#2-inicio-rápido)
3.  [⚙️ Configuración Detallada](#3-configuración-detallada)
4.  [👨‍💻 Desglose del Código](#4-desglose-del-código)
5.  [❓ Uso y Ejemplos](#5-uso-y-ejemplos)
6.  [⚠️ Notas Importantes](#6-notas-importantes)

## 1. 🌟 Características

Valgrim Bot está repleto de funciones para mejorar tu servidor:

* **✅ Verificación de Minecraft:**
    * Vincula las cuentas de Minecraft con los perfiles de Discord de manera segura.
    * Asegura que solo los jugadores legítimos tengan acceso a ciertas áreas.
* **🎫 Sistema de Tickets de Soporte:**
    * Permite a los usuarios crear tickets para reportar problemas, solicitar ayuda o realizar consultas.
    * Proporciona herramientas para que el personal administre y resuelva los tickets de manera eficiente.
* **💡 Sugerencias de la Comunidad:**
    * Fomenta la participación de la comunidad permitiendo a los usuarios enviar sugerencias.
    * Implementa un sistema de votación (👍/👎) para priorizar las ideas.
* **🎨 Embeds Personalizados:**
    * Crea mensajes visualmente atractivos y formateados para anuncios, información o cualquier otro propósito.
    * Ofrece opciones de personalización como títulos, descripciones, colores, imágenes y pies de página.
* **👋 Mensajes de Bienvenida:**
    * Recibe a los nuevos miembros con mensajes cálidos y personalizados.
    * Proporciona información importante sobre el servidor y cómo empezar.
* **⌨️ Comandos de Barra (Slash Commands):**
    * Utiliza la moderna interfaz de comandos de barra de Discord para una interacción más intuitiva.
    * Simplifica la ejecución de comandos y descubre funciones fácilmente.
* **✨ Modales Interactivos:**
    * Recopila información de los usuarios de forma estructurada y conveniente.
    * Utiliza modales para la verificación, la creación de tickets, la presentación de sugerencias y la creación de embeds.
* **🖱️ Botones Dinámicos:**
    * Permite a los usuarios realizar acciones rápidas con un solo clic.
    * Los botones se utilizan para abrir modales, crear tickets y cerrar tickets.
* **📜 Transcripciones de Tickets:**
    * Guarda un registro de la conversación en los tickets cerrados para futuras referencias.
    * Las transcripciones se almacenan como archivos JSON.

## 2. 🚀 Inicio Rápido

¿Quieres poner en marcha Valgrim Bot rápidamente? Sigue estos pasos:

1.  **Instala Node.js:** Asegúrate de tener Node.js instalado en tu sistema.
2.  **Instala las Dependencias:**

    ```bash
    npm install discord.js dotenv
    ```

3.  **Configura las Variables de Entorno:**

    * Crea un archivo `.env` en el mismo directorio que `index.js`.
    * Añade las variables necesarias (consulta la sección [Configuración Detallada](#3-configuración-detallada) para obtener una lista completa).

4.  **Ejecuta el Bot:**

    ```bash
    node index.js
    ```

## 3. ⚙️ Configuración Detallada

Para aprovechar al máximo Valgrim Bot, es importante configurarlo correctamente.

### 3.1. Variables de Entorno (.env)

El archivo `.env` almacena información confidencial y ajustes de configuración. Aquí tienes una lista de las variables que necesitas:

* `BOT_TOKEN`: El token de tu bot de Discord. ¡Mantén esto en secreto!
* `VERIFICATION_CHANNEL_ID`: El ID del canal donde se enviará el mensaje de verificación.
* `VERIFICATION_ROLE_ID` (Opcional): El ID del rol que se asignará a los usuarios verificados.
* `TICKET_CHANNEL_ID`: El ID del canal donde se enviará el mensaje del sistema de tickets.
* `TICKET_LOGS_CHANNEL_ID` (Opcional): El ID del canal donde se guardarán los registros de los tickets cerrados.
* `WELCOME_CHANNEL_ID` (Opcional): El ID del canal donde se enviarán los mensajes de bienvenida a los nuevos miembros.
* `DEVELOPERS_ID` (Opcional): Una lista separada por comas de los IDs de los usuarios de Discord que tienen permisos de desarrollador (para comandos como `/createembed`).
* `SUGGEST_CHANNEL_ID` (Opcional): El ID del canal donde se enviará el mensaje de configuración de sugerencias.
* `SUGGEST_SEND_CHANNEL_ID` (Opcional): El ID del canal donde se publicarán las sugerencias de los usuarios.

**Ejemplo de archivo .env:**
```bash
BOT_TOKEN=TU_TOKEN_AQUÍ
VERIFICATION_CHANNEL_ID=123456789012345678
TICKET_CHANNEL_ID=987654321098765432
```
### 3.2. Archivos de Configuración (config.json, users.json)

El bot utiliza dos archivos JSON para almacenar datos:

* `config.json`: Almacena los IDs de mensajes importantes (como los mensajes de verificación y tickets). Esto permite que el bot actualice o interactúe con estos mensajes después de reiniciar.
* `users.json`: Almacena la asignación entre los nombres de usuario de Minecraft y los IDs de usuario de Discord para la verificación.

Las funciones `saveConfig()` y `saveUsers()` actualizan estos archivos cuando es necesario.

## 4. 👨‍💻 Desglose del Código

Esta sección proporciona una descripción general del código del bot.

### 4.1. Dependencias

* `discord.js`: La biblioteca principal para interactuar con la API de Discord.
* `dotenv`: Carga las variables de entorno desde el archivo `.env`.
* `fs` (File System): El módulo de Node.js para trabajar con archivos.

### 4.2. Estructura de Archivos
```bash
├── index.js          # El archivo principal del bot
├── config.json       # Configuración del bot
├── users.json        # Datos de verificación de usuarios
├── data/             # (Opcional) Transcripciones de tickets
└── .env              # (¡No incluir en el repositorio!) Variables de entorno
```
### 4.3. Flujo de Trabajo Principal

1.  **Inicialización:** El bot se conecta a Discord, lee los archivos de configuración y registra los comandos de barra.
2.  **Manejo de Eventos:** El bot escucha varios eventos de Discord (por ejemplo, la creación de mensajes, las interacciones) y ejecuta el código correspondiente.
3.  **Interacciones:** El bot interactúa con los usuarios a través de mensajes, botones, modales y comandos de barra.
4.  **Guardado de Datos:** El bot guarda datos en los archivos de configuración y en archivos de transcripciones.

### 4.4. Componentes Clave

* **Verificación:**
    * Muestra un mensaje con un botón de verificación.
    * Utiliza un modal para recopilar el nombre de usuario de Minecraft.
    * Verifica el nombre de usuario y actualiza los datos del usuario.
* **Tickets:**
    * Muestra un mensaje con botones para diferentes tipos de tickets.
    * Crea canales de texto privados para cada ticket.
    * Gestiona el cierre de tickets y guarda las transcripciones.
* **Sugerencias:**
    * Permite a los usuarios enviar sugerencias a través de un modal.
    * Publica las sugerencias en un canal designado para la votación.
* **Embeds:**
    * Permite a los desarrolladores crear embeds personalizados mediante un comando de barra y un modal.
* **Comandos de Barra:**
    * Define y registra los comandos disponibles para los usuarios.
* **Modales y Botones:**
    * Utiliza modales para recopilar información de los usuarios.
    * Utiliza botones para activar diversas acciones.
* **Manejo de Errores:**
    * Implementa un manejo de errores robusto para garantizar la estabilidad.
* **Guardado de Archivos:**
    * Guarda datos en archivos JSON para la persistencia.

## 5. ❓ Uso y Ejemplos

Esta sección proporciona ejemplos de cómo usar las principales funciones del bot.

### 5.1. Verificación

1.  Haz clic en el botón "Verificar" en el canal de verificación.
2.  Ingresa tu nombre de usuario de Minecraft en el modal.
3.  El bot verificará tu cuenta y te asignará el rol correspondiente (si está configurado).

### 5.2. Creación de Tickets

1.  Haz clic en el botón correspondiente al tipo de ticket que deseas crear (por ejemplo, "Reportar Jugador").
2.  Proporciona los detalles necesarios en el modal.
3.  El bot creará un canal de ticket privado para tu solicitud.

### 5.3. Envío de Sugerencias

1.  Haz clic en el botón "Crear Sugerencia" en el canal de sugerencias.
2.  Describe tu sugerencia en el modal.
3.  Tu sugerencia se publicará para que la comunidad la vote.

### 5.4. Creación de Embeds (Solo Desarrolladores)

1.  Utiliza el comando de barra `/createembed`.
2.  Completa el modal con los detalles del embed.
3.  El bot enviará el embed al canal.

## 6. ⚠️ Notas Importantes

* **Configuración:** Asegúrate de configurar correctamente las variables de entorno y los permisos del bot.
* **Seguridad:** Mantén tu token de bot en secreto. Nunca lo compartas con nadie.
* **Permisos:** El bot necesita los permisos adecuados para funcionar correctamente (por ejemplo, administrar roles, crear canales).
* **Pruebas:** Prueba el bot a fondo en un entorno de desarrollo antes de implementarlo en tu servidor principal.
* **Extensibilidad:** El bot está diseñado para ser extensible. Puedes agregar fácilmente nuevas funciones y comandos.

¡Gracias por usar Valgrim Bot! Si tienes alguna pregunta, problema o sugerencia, no dudes en crear un issue en este repositorio.
