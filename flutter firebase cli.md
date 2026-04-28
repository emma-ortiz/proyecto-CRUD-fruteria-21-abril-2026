¡Hola! Preparar tu entorno para trabajar con Flutter y Firebase es un excelente paso. Para integrar Firebase en tu aplicación Flutter de manera moderna y eficiente, necesitas usar la herramienta de línea de comandos de Firebase (Firebase CLI).

Aquí tienes la guía paso a paso, enfocada en Windows, para preparar tu sistema desde cero.

---

## 1. Verificación: ¿Están instalados Node.js y npm?

Antes de instalar cualquier cosa, primero debemos comprobar si tu computadora ya tiene el software necesario. **npm** (Node Package Manager) viene incluido por defecto cuando instalas **Node.js**.

### Pasos para verificar:
1. Abre tu terminal en Windows (puedes usar el **Símbolo del sistema (CMD)** o **PowerShell**).
2. Escribe el siguiente comando y presiona Enter:
   ```bash
   node -v
   ```
3. Luego, escribe este comando y presiona Enter:
   ```bash
   npm -v
   ```

**¿Qué versión utilizar?**
Si la terminal te devuelve números de versión (por ejemplo, `v20.12.2` y `10.5.0`), ya los tienes instalados. Para trabajar con Firebase y herramientas modernas de desarrollo, **siempre se recomienda usar la versión LTS (Long Term Support)** de Node.js (actualmente la versión 20.x o superior). Las versiones LTS garantizan estabilidad a largo plazo. 

*Si la terminal te arroja un error diciendo que el comando "no se reconoce", entonces no están instalados. Pasa a la siguiente sección.*

---

## 2. Instalación global de Node.js y npm en Windows

Si no tienes Node.js, instalarlo de manera global en Windows es un proceso muy sencillo utilizando su instalador oficial.

### Procedimiento paso a paso:
1. **Descargar el instalador:** Ve a la página oficial de Node.js (nodejs.org) y descarga el instalador para Windows (el archivo `.msi`) que tenga la etiqueta **"LTS (Recommended for most users)"**.
2. **Ejecutar el instalador:** Haz doble clic en el archivo descargado para iniciar la instalación.
3. **Aceptar los términos:** Lee y acepta el acuerdo de licencia. Haz clic en *Next*.
4. **Elegir la ruta:** Deja la ruta de instalación por defecto (usualmente `C:\Program Files\nodejs\`). Haz clic en *Next*.
5. **Configuración de instalación (Custom Setup):** En esta pantalla, asegúrate de que las opciones **"npm package manager"** y **"Add to PATH"** estén habilitadas (vienen así por defecto). Esto es crucial porque es lo que permite usar npm *de manera global* en cualquier carpeta de tu computadora. Haz clic en *Next*.
6. **Herramientas nativas:** Te preguntará si deseas instalar automáticamente herramientas adicionales (como Chocolatey o Python para compilar módulos nativos). Para Firebase CLI no es estrictamente necesario, así que puedes dejar la casilla desmarcada y dar clic en *Next*.
7. **Instalar:** Haz clic en *Install* y espera a que termine el proceso.
8. **Reiniciar terminal:** Una vez finalizado, **cierra tu terminal si la tenías abierta y abre una nueva**. Vuelve a ejecutar `node -v` y `npm -v` para confirmar que la instalación fue exitosa.

---

## 3. Comandos para usar Firebase

Una vez que npm y Node.js están listos en tu sistema de manera global, puedes proceder a instalar la CLI de Firebase y conectarla con tu cuenta.

### a) Cómo instalar y usar `firebase-tools`

`firebase-tools` es el paquete de npm que contiene la Firebase CLI. Para instalarlo de manera global en tu computadora, abre tu terminal y ejecuta:

```bash
npm install -g firebase-tools
```

* **¿Qué hace este comando?** La instrucción `npm install` le dice a Node.js que descargue un paquete. La bandera `-g` significa **"global"**, lo que asegura que el comando `firebase` esté disponible desde cualquier directorio en tu computadora, no solo en la carpeta actual.
* **Para verificar su instalación:** Escribe `firebase --version`. Debería devolverte el número de la versión instalada de Firebase.

### b) Cómo acceder a Firebase con tu cuenta de Google

Para que la terminal tenga permisos para modificar tus proyectos de Firebase, necesitas iniciar sesión. 

En tu terminal, ejecuta el siguiente comando:

```bash
firebase login
```

**Proceso de inicio de sesión:**
1. Es posible que la terminal te pregunte si deseas recolectar datos de uso e informes de errores de la CLI (`Allow Firebase to collect CLI and Emulator Suite usage and error reporting information?`). Responde **Y** (sí) o **N** (no) y presiona Enter.
2. Tu **navegador web predeterminado se abrirá automáticamente** en una página de inicio de sesión de Google.
3. Selecciona la cuenta de Google con la que creaste tu proyecto en la consola web de Firebase.
4. Haz clic en **"Permitir" (Allow)** para conceder a Firebase CLI acceso a tu cuenta.
5. Verás una pantalla de éxito en el navegador. Puedes cerrarla.
6. Regresa a tu terminal; deberías ver un mensaje que dice: `Success! Logged in as tu_correo@gmail.com`.

---

> **Nota importante para Flutter:**
> Ahora que tienes Firebase CLI instalado y tu sesión iniciada, el siguiente paso para integrar esto en tu aplicación Flutter es instalar **FlutterFire CLI**. Esto se hace ejecutando `dart pub global activate flutterfire_cli` en tu terminal, y luego corriendo `flutterfire configure` dentro de la carpeta de tu proyecto Flutter para que enlace automáticamente los archivos a la nube.
