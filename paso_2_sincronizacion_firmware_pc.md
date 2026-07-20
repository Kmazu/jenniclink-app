# Guía Exclusiva - Paso 2: Obtención y Sincronización del Archivo de Firmware (.bin)

Este documento es una guía dedicada y minuciosamente detallada sobre cómo preparar una computadora de escritorio o laptop con **Linux/Ubuntu** para transferir y sincronizar los archivos de firmware `.bin` hacia la aplicación móvil **JennicLink Pro** a través de la red Wi-Fi local.

---

## 1. MÉTODOS A: Sincronización Directa por SSH/SFTP (Recomendado sin programas extra)

Este método permite que el celular se conecte de forma remota y segura a la laptop de Ubuntu, navegue por sus carpetas y descargue los archivos `.bin` directamente a la app sin instalar software de terceros en la PC.

### PASO A.1: Preparación Única en la PC con Ubuntu (Servidor SSH)
En la computadora donde están guardados los archivos `.bin`, debes preparar el sistema por única vez ejecutando los siguientes comandos en la terminal (`Ctrl + Alt + T`):

1. **Instalar el servidor OpenSSH**:
   Por defecto, Ubuntu Desktop no acepta conexiones externas. Instálalo ejecutando:
   ```bash
   sudo apt update && sudo apt install -y openssh-server
   ```
   *(Escribe tu contraseña de usuario de Ubuntu cuando la consola te la solicite).*

2. **Verificar que el servicio SSH esté activo**:
   Confirma que el servicio se esté ejecutando en segundo plano con:
   ```bash
   sudo systemctl status ssh
   ```
   *(Debe aparecer en color verde el mensaje `active (running)`).* Si no está activo, inícialo y habilítalo con:
   ```bash
   sudo systemctl enable --now ssh
   ```

3. **Abrir el Puerto 22 en el Firewall de Ubuntu (`ufw`)**:
   El servicio SSH utiliza el **Puerto 22** de forma predeterminada. Si la laptop tiene el firewall encendido, debes permitir el tráfico entrante ejecutando:
   ```bash
   sudo ufw allow 22/tcp
   ```
   *(Este paso es fundamental para evitar el error "Connection refused" o "Timed out" desde la aplicación móvil).*

4. **Habilitar la autenticación por Contraseña (Si aplica)**:
   Si la computadora pertenece a una red corporativa con políticas estrictas de seguridad, verifica que permita el inicio de sesión mediante contraseña:
   * Abre el archivo de configuración en nano:
     ```bash
     sudo nano /etc/ssh/sshd_config
     ```
   * Asegúrate de que la línea `PasswordAuthentication` diga `yes` (y que no tenga un símbolo `#` al inicio).
   * Guarda los cambios con `Ctrl + O` -> `Enter` y sal con `Ctrl + X`.
   * Reinicia el servicio para aplicar los cambios:
     ```bash
     sudo systemctl restart ssh
     ```

5. **Identificar la Dirección IP de Wi-Fi de la PC**:
   En la terminal de la computadora ejecuta:
   ```bash
   hostname -I
   ```
   * Te saldrán una o varias direcciones IP. **Elige la IP que pertenezca a la red Wi-Fi local** (por lo general empieza por `192.168.1.xxx` o `172.20.10.xxx`).
   * *Nota importante*: Ignora direcciones IP que empiecen por `10.9.x.x` (corresponden a interfaces de VPN que no son accesibles directamente desde el celular).

---

### PASO A.2: Configuración y Descarga en la App del Celular

1. Conecta tu celular al **mismo Wi-Fi** que la laptop Ubuntu.
2. Abre la aplicación **JennicLink Pro** en tu teléfono y ve a la pestaña **Grabador (Flasher)**.
3. En la tarjeta superior **"Sincronización de PC (Wi-Fi)"**, completa los campos fijados:
   * **IP de tu PC**: Escribe la IP obtenida en la PC (ejemplo: `192.168.1.40`).
   * **Usuario SSH**: Ingresa el nombre de usuario de Ubuntu (ejemplo: `innovex`).
   * **Contraseña**: Escribe la contraseña con la que inicias sesión en la laptop.
   * **Ruta en PC**:
     * Para buscar en tus documentos y descargas: escribe `/home/tu_usuario` (ej: `/home/innovex`).
     * Para buscar en absolutamente **todo el disco duro**: escribe `/`.
4. Presiona el botón **"Sinc"**:
   * El celular se conectará de forma segura a la laptop por el puerto 22.
   * La app realizará una búsqueda recursiva por las subcarpetas encontrando todos los archivos con extensión `.bin`.
5. En la lista desplegable de resultados en el celular, presiona el botón **"Bajar"** al lado del archivo `.bin` deseado.
   * El archivo se descargará a la memoria interna de la aplicación y quedará guardado para siempre, permitiéndote flashear en terreno **incluso sin internet ni Wi-Fi**.

---

## 2. MÉTODO B: Sincronización mediante Servidor Autónomo (`simple_server.py`)

Si estás en la computadora de un cliente o compañero donde **no se puede o no se quiere instalar el servicio de SSH**, puedes usar el script portátil en Python:

1. En la computadora, descarga o copia el archivo `simple_server.py`.
2. Coloca los archivos de firmware `.bin` en la carpeta **Descargas** de la PC.
3. Abre la terminal en esa carpeta y ejecuta:
   ```bash
   python3 simple_server.py
   ```
   *(El script abrirá un servidor liviano en el **Puerto 5000**).*
4. En el celular, desmarca la opción de SSH si estuviera activa, ingresa la IP de esa PC y presiona **"Sinc"**.

---

## 3. MÉTODO C: Transferencia Manual Directa al Celular

Si recibiste el archivo `.bin` a través de WhatsApp, Telegram, correo electrónico o mediante un pendrive USB:

1. Descarga o copia el archivo en la memoria interna del teléfono (por ejemplo, en la carpeta `Descargas` / `Downloads`).
2. Al estar en el celular, la aplicación **JennicLink Pro** detectará automáticamente el archivo `.bin` en la lista desplegable de firmwares locales sin requerir sincronización Wi-Fi.
