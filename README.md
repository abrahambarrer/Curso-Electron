# Curso Electron

## settings.json
El archivo `settings.json` modifica las características de apariencia del editor de VScode.

## Arquitectura de electron

* Sistema operativo: desde la aplicación se puede acceder a los elementos de programa que ofrece el sistema operativo, escribir en disco, acceder a la red, etc.

* Aplicación: se ejecuta sobre el sistema operativo.

En la aplicación se encuentran dos elementos:
* Proceso principal (gestiona el ciclo de vida de la aplicación)
* Se ejecuta en nodejs (lanza la aplicación, guarda el estado de la aplicación)

## Construcción de la aplicación

Los primeros pasos en el desarrollo de la aplicación incluyen la creación de los archivos `package.json` y `package-lock.json` por medio del comando `npm init`, donde se agrega información acerca del proyecto.

Nótese que en `package.json` se agrega el script comando `start` como `electron .`. En este mismo archivo se indica el archivo de entrada a la aplicación `main.js`.

### `index.html`

Se crea un archivo `index.html` estándar donde se agregan dos líneas de metadatos para especificar que no se agregue código de JavaScripts en las etiquetas tipo script.

Se desea que el archivo indique información sobre la versiones utilizadas de Node.js, Chromium y Electron de forma dinámica, es decir, con elementos HTML tipo `span`. Para ello, se incluirá un archivo `renderer.js`.

### `main.js`

1. Se importan los módulos `app`, `BrowserWindow` y `path` con la sintaxis CommonJS.
2. Se crea la función `crearVentanaPrincipal()` para configurar la ventana de la aplicación. Dentro de la función se añaden las preferencias con el archivo `preload.js`.
3. Se carga el archivo `index.html`.
4. Para utilzar la función de crear ventana, se pasa su referencia al método `app.whenReady()`.

### `preload.js`
1. Aquí se crea la función para obtener el id de los elementos span de los módulos deseados.
2. El archivo utiliza un evento que indica que el DOM se ha cargado correctamente. Según el id recuperado, obtiene el número de versión de módulo correspondiente.

## Cierre de ventana (macOS)
En el sistema operativo macOS ocurre un problema al cerrar una aplicación electron con el botón X: el ícono de electron sigue apareciendo en la barra de tareas. Para solucionarlo, se crean dos llamadas a la propiedad `app.on()`. Una para aplicar `app.quit()` si la plataforma es darwin (macOS), y la otra para detectar el número de ventanas abiertas.

## Generar ejecutable
Para generar un archivo ejecutable que pueda ser distribuido se requiere del módulo `electron packager`, para lo cual se instala con el comando:
```shell
npm i electron packager -g
```
Con el módulo instalado, y dentro de la carpea del proyecto, se ejecuta el comando:
```shell
electron-packager . platform=win32 --arch=x64
```