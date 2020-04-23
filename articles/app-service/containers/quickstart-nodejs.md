---
title: 'Inicio rápido: Creación de una aplicación Node.js en Linux'
description: Para empezar a trabajar con aplicaciones de Linux en Azure App Service, debe implementar su primera aplicación de Node.js en un contenedor Linux en App Service.
author: msangapu-msft
ms.author: msangapu
ms.date: 08/12/2019
ms.topic: quickstart
ms.devlang: javascript
ms.openlocfilehash: 08a246b07effb8d5f3e2473a4d7959882cf43235
ms.sourcegitcommit: acb82fc770128234f2e9222939826e3ade3a2a28
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 04/21/2020
ms.locfileid: "81687948"
---
# <a name="create-a-nodejs-app-in-azure"></a>Creación de una aplicación de Node.js en Azure

Azure App Service proporciona un servicio de hospedaje web muy escalable y con aplicación de revisiones de un modo automático. En este tutorial rápido se explica cómo se implementa una aplicación de Node.js en Azure App Service.

## <a name="prerequisites"></a>Prerrequisitos

Si no tiene una cuenta de Azure, [regístrese hoy mismo](https://azure.microsoft.com/free/?utm_source=campaign&utm_campaign=vscode-tutorial-app-service-extension&mktingSource=vscode-tutorial-app-service-extension) para obtener una cuenta gratuita con 200 USD en créditos de Azure para probar cualquier combinación de los servicios.

Necesita tener instalado [Visual Studio Code](https://code.visualstudio.com/), además de [Node.js y npm](https://nodejs.org/en/download), el administrador de paquetes de Node.js.

También tendrá que instalar la [extensión Azure App Service](vscode:extension/ms-azuretools.vscode-azureappservice), que puede usar para crear, administrar e implementar Azure App Service en Linux en la plataforma como servicio de Azure (PaaS).

### <a name="sign-in"></a>Iniciar sesión

Después de instalar la extensión, inicie sesión en su cuenta de Azure. En la barra de actividades, seleccione el logotipo de Azure para mostrar el explorador de **AZURE APP SERVICE**. Seleccione **Iniciar sesión en Azure…** y siga las instrucciones.

![inicio de sesión en Azure](./media/quickstart-nodejs/sign-in.png)

### <a name="troubleshooting"></a>Solución de problemas

Si ve el error **“No se encuentra la suscripción con el nombre [identificador de suscripción]”** , puede que sea debido a que esté detrás de un proxy y no se pueda establecer la conexión con la API de Azure. Configure las variables de entorno `HTTP_PROXY` y `HTTPS_PROXY` con su información de proxy en el terminal mediante `export`.

```sh
export HTTPS_PROXY=https://username:password@proxy:8080
export HTTP_PROXY=http://username:password@proxy:8080
```

Si el problema no se corrige después de configurar las variables de entorno, seleccione el botón siguiente **He tenido un problema** para ponerse en contacto con nosotros.

### <a name="prerequisite-check"></a>Comprobación de requisitos previos

Antes de continuar, asegúrese de haber instalado y configurado todos los requisitos previos.

En VS Code, verá su dirección de correo electrónico de Azure en la barra de estado, y la suscripción en el explorador de **AZURE APP SERVICE**.

> [!div class="nextstepaction"]
> [He tenido un problema](https://www.research.net/r/PWZWZ52?tutorial=node-deployment-azure-app-service&step=getting-started)

## <a name="create-your-nodejs-application"></a>Creación de una aplicación de Node.js

Después, cree una aplicación de Node.js que se pueda implementar en la nube. En este inicio rápido, se usa un generador de aplicaciones para aplicar scaffolding rápidamente en la aplicación desde un terminal.

> [!TIP]
> Si ya ha completado el [tutorial de Node.js](https://code.visualstudio.com/docs/nodejs/nodejs-tutorial), puede ir directamente al paso [Implementación en Azure](#deploy-to-azure).

### <a name="scaffold-a-new-application-with-the-express-generator"></a>Aplicación de scaffolding en una nueva aplicación con Express Generator

[Express](https://www.expressjs.com) es un marco popular para crear y ejecutar aplicaciones de Node.js. Puede aplicar scaffolding en una nueva aplicación de Express (lo que equivale a crear un aplicación) mediante la herramienta [Express Generator](https://expressjs.com/en/starter/generator.html). Express Generator se proporciona como módulo de npm y se puede ejecutar directamente (sin instalación) mediante la herramienta de línea de comandos de npm `npx`.

```bash
npx express-generator myExpressApp --view pug --git
```

Los parámetros `--view pug --git` indican al generador que use el motor de plantillas [pug](https://pugjs.org/api/getting-started.html) (antes conocido como `jade`) y que cree un archivo de `.gitignore`.

Para instalar todas las dependencias de la aplicación, vaya a la nueva carpeta y ejecute `npm install`.

```bash
cd myExpressApp
npm install
```

### <a name="run-the-application"></a>Ejecución de la aplicación

A continuación, asegúrese de que se ejecute la aplicación. Desde el terminal, inicie la aplicación con el comando `npm start` para iniciar el servidor.

```bash
npm start
```

Ahora, abra el explorador y vaya a [http://localhost:3000](http://localhost:3000), donde verá algo similar a esto:

![Ejecución de una aplicación de Express](./media/quickstart-nodejs/express.png)

> [!div class="nextstepaction"]
> [He tenido un problema](https://www.research.net/r/PWZWZ52?tutorial=node-deployment-azure-app-service&step=create-app)

## <a name="deploy-to-azure"></a>Implementar en Azure

En esta sección, implementará la aplicación de Node.js con VS Code y la extensión Azure App Service. En este inicio rápido, se usa el modelo de implementación más básico, donde la aplicación se comprime e implementa en una instancia de Azure App Service en Linux.

### <a name="deploy-using-azure-app-service"></a>Implementación mediante Azure App Service

Primero, abra la carpeta de la aplicación en VS Code.

```bash
code .
```

En el explorador de **AZURE APP SERVICE**, seleccione el icono de flecha arriba azul para implementar la aplicación en Azure.

![Implementación en Web App](./media/quickstart-nodejs/deploy.png)

> [!TIP]
> También puede implementar desde la **Paleta de comandos** (CTRL + MAYÚS + P); para hacerlo, escriba “implementar en Web App” y ejecute el comando **Azure App Service: implementar en Web App**.

1. Seleccione el directorio que tenga abierto actualmente, `myExpressApp`.

1. Elija **Create new Web App** (Crear aplicación web), que se implementa en App Service en Linux de forma predeterminada.

1. Escriba un nombre único global para la Web App y presione ENTRAR. Los caracteres válidos para un nombre de aplicación son “a-z”, “0-9” y “-”.

1. Seleccione la **versión de Node.js** (le recomendamos que use “LTS”).

    En el canal de notificación, se muestran los recursos de Azure que se estén creando para la aplicación.

1. Seleccione **Sí** cuando se le pida que actualice la configuración para ejecutar `npm install` en el servidor de destino. Después, se implementará la aplicación.

    ![Implementación configurada](./media/quickstart-nodejs/server-build.png)

1. Cuando se inicie la aplicación, se le pedirá que actualice el área de trabajo para que las implementaciones posteriores usen como destino automáticamente la misma instancia de App Service Web App. Seleccione **Sí** para asegurarse de que los cambios se implementen en la aplicación correcta.

    ![Implementación configurada](./media/quickstart-nodejs/save-configuration.png)

> [!TIP]
> Asegúrese de que la aplicación escuche en el puerto proporcionado por la variable de entorno PORT: `process.env.PORT`.

### <a name="browse-the-app-in-azure"></a>Examen de la aplicación en Azure

Una vez finalizada la implementación, seleccione **Browse Website** (Examinar sitio web) en el mensaje para ver la aplicación web recién implementada.

### <a name="troubleshooting"></a>Solución de problemas

Si ve el error **“No tiene permiso para ver este directorio o esta página”** , puede que la aplicación no se haya iniciado correctamente. Vaya a la sección siguiente y vea la salida de registros para identificar y corregir el error. Si no puede corregirlo, seleccione el botón siguiente **He tenido un problema** para ponerse en contacto con nosotros. Estaremos encantados de ayudarle.

> [!div class="nextstepaction"]
> [He tenido un problema](https://www.research.net/r/PWZWZ52?tutorial=node-deployment-azure-app-service&step=deploy-app)

### <a name="update-the-app"></a>Actualización de la aplicación

Para implementar los cambios en esta aplicación, siga el mismo proceso y seleccione la aplicación existente, en lugar de crear otra.

## <a name="viewing-logs"></a>Visualización de los registros

En esta sección aprenderá a ver (o "copiar") los registros desde la aplicación de App Service en ejecución. Las llamadas realizadas a `console.log` en la aplicación se muestran en la ventana de salida en Visual Studio Code.

Busque la aplicación en el explorador de **AZURE APP SERVICE**, haga clic con el botón derecho en la aplicación y seleccione **Ver registros de streaming**.

Cuando se le pida, seleccione la opción para habilitar el registro y reinicie la aplicación. Después de reiniciar la aplicación, se abrirá la ventana de salida de VS Code con una conexión al streaming de registros.

![Visualización de los registros de streaming](./media/quickstart-nodejs/view-logs.png)

![Activación de los registros y reinicio](./media/quickstart-nodejs/enable-restart.png)

Después de unos segundos, verá un mensaje donde se indica que está conectado al servicio de streaming de registros. Actualice la página varias veces para ver más actividad.

<pre>
2019-09-20 20:37:39.574 INFO  - Initiating warmup request to container msdocs-vscode-node_2_00ac292a for site msdocs-vscode-node
2019-09-20 20:37:55.011 INFO  - Waiting for response to warmup request for container msdocs-vscode-node_2_00ac292a. Elapsed time = 15.4373071 sec
2019-09-20 20:38:08.233 INFO  - Container msdocs-vscode-node_2_00ac292a for site msdocs-vscode-node initialized successfully and is ready to serve requests.
2019-09-20T20:38:21  Startup Request, url: /Default.cshtml, method: GET, type: request, pid: 61,1,7, SCM_SKIP_SSL_VALIDATION: 0, SCM_BIN_PATH: /opt/Kudu/bin, ScmType: None
</pre>

> [!div class="nextstepaction"]
> [He tenido un problema](https://www.research.net/r/PWZWZ52?tutorial=node-deployment-azure-app-service&step=tailing-logs)

## <a name="next-steps"></a>Pasos siguientes

Ha completado correctamente este inicio rápido.

A continuación, vea otras extensiones de Azure.

* [Cosmos DB](https://marketplace.visualstudio.com/items?itemName=ms-azuretools.vscode-cosmosdb)
* [Funciones de Azure](https://marketplace.visualstudio.com/items?itemName=ms-azuretools.vscode-azurefunctions)
* [Herramientas de Docker](https://marketplace.visualstudio.com/items?itemName=PeterJausovec.vscode-docker)
* [Herramientas de la CLI de Azure](https://marketplace.visualstudio.com/items?itemName=ms-vscode.azurecli)
* [Herramientas de Azure Resource Manager](https://marketplace.visualstudio.com/items?itemName=msazurermtools.azurerm-vscode-tools)

También puede conseguirlas todas si instala el paquete de extensiones [Azure Tools](https://marketplace.visualstudio.com/items?itemName=ms-vscode.vscode-node-azure-pack).
