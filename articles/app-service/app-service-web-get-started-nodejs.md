---
title: 'Inicio rápido: Crear una aplicación web de Node.js'
description: Implementación de su primera aplicación Hola mundo de Node.js en Azure App Service en cuestión de minutos. Realice la implementación mediante Visual Studio Code, que es una de las distintas maneras de realizar implementaciones en App Service.
ms.assetid: 582bb3c2-164b-42f5-b081-95bfcb7a502a
ms.topic: quickstart
ms.date: 03/04/2020
ms.custom: mvc, devcenter, seodec18
ms.openlocfilehash: df1e0809c0122eae593543cd193a6477b03d0938
ms.sourcegitcommit: 366e95d58d5311ca4b62e6d0b2b47549e06a0d6d
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 05/01/2020
ms.locfileid: "82690789"
---
# <a name="create-a-nodejs-web-app-in-azure"></a>Creación de una aplicación web de Node.js en Azure 

Para empezar a trabajar con Azure App Service, cree una aplicación de Node.js o Express localmente con Visual Studio Code y, a continuación, implemente la aplicación en la nube. Dado que usa un nivel de App Service gratuito, completar este inicio rápido no supone ningún costo.

## <a name="prerequisites"></a>Prerrequisitos

- Una cuenta de Azure con una suscripción activa. [Cree una cuenta gratuita](https://azure.microsoft.com/free/?utm_source=campaign&utm_campaign=vscode-tutorial-app-service-extension&mktingSource=vscode-tutorial-app-service-extension).
- [Node.js y npm](https://nodejs.org) Ejecute el comando `node --version` para comprobar que Node.js está instalado.
- [Visual Studio Code](https://code.visualstudio.com/).
- La [extensión Azure App Service](https://marketplace.visualstudio.com/items?itemName=ms-azuretools.vscode-azureappservice) para Visual Studio Code.

## <a name="clone-and-run-a-local-nodejs-application"></a>Clonación y ejecución de una aplicación de Node.js local

1. En el equipo local, abra un terminal y clone el repositorio de ejemplo:

    ```bash
    git clone https://github.com/Azure-Samples/nodejs-docs-hello-world
    ```

1. Vaya a la carpeta de la nueva aplicación:

    ```bash
    cd nodejs-docs-hello-world
    ```

1. Inicie la aplicación para probarla localmente:

    ```bash
    npm start
    ```
    
1. Abra el explorador web y vaya a `http://localhost:1337`. El explorador debe mostrar "¡Hola mundo!".

1. Presione **Ctrl**+**C** en el terminal para detener el servidor.

> [!div class="nextstepaction"]
> [He tenido un problema](https://www.research.net/r/PWZWZ52?tutorial=node-deployment-azure-app-service&step=create-app)

## <a name="deploy-the-app-to-azure"></a>Implementar la aplicación en Azure

En esta sección, implementará la aplicación de Node.js en Azure con VS Code y la extensión Azure App Service.

1. En el terminal, asegúrese de que se encuentra en la carpeta *nodejs-docs-hello-world* y, a continuación, inicie Visual Studio Code con el siguiente comando:

    ```bash
    code .
    ```

1. En la barra de actividades de VS Code, seleccione el logotipo de Azure para mostrar el explorador de **AZURE APP SERVICE**. Seleccione **Iniciar sesión en Azure…** y siga las instrucciones. Si se producen errores, consulte [Solución de problemas de inicio de sesión en Azure](#troubleshooting-azure-sign-in) a continuación. Una vez que haya iniciado sesión, el explorador debe mostrar el nombre de la suscripción de Azure.

    ![Inicio de sesión en Azure](containers/media/quickstart-nodejs/sign-in.png)

1. En el explorador de **AZURE APP SERVICE** de VS Code, seleccione el icono de flecha arriba azul para implementar la aplicación en Azure. También puede invocar el mismo comando desde la **Paleta de comandos** (**Ctrl**+**Mayús**+**P**); para hacerlo, escriba "deploy to web app" y elija **Azure App Service: Deploy to Web App** (Azure App Service: Implementar en aplicación web).

    ![Implementación en Web App](containers/media/quickstart-nodejs/deploy.png)
        
1. Elija la carpeta *nodejs-docs-hello-world*.

1. Elija una opción de creación basada en el sistema operativo en el que desea realizar la implementación:

    - Linux: seleccione la opción **Create new Web App** (Crear nueva aplicación web).
    - Windows: seleccione la opción **Create new Web App... Advanced** (Crear nueva aplicación web...Avanzado).

1. Escriba un nombre único global para la aplicación web y presione **Intro**. El nombre debe ser único en todo Azure y usar solo caracteres alfanuméricos ("A-Z", "a-z" y "0-9") y guiones ("-").

1. Si el destino es Linux, seleccione una versión de Node.js cuando se le solicite. Se recomienda una versión **LTS**.

1. Si el destino es Windows, siga las indicaciones adicionales:
    1. Seleccione **Create a new resource group** (Crear un grupo de recursos nuevo) y escriba un nombre para el grupo de recursos, como `AppServiceQS-rg`.
    1. Seleccione **Windows** como sistema operativo.
    1. Seleccione **Create new App Service plan** (Crear nuevo plan de App Service) y, a continuación, escriba un nombre para el plan (por ejemplo, `AppServiceQS-plan`); después, seleccione **F1 Free** (F1 gratis) como plan de tarifa.
    1. Elija **Omitir por ahora** cuando se le pregunte sobre Application Insights.
    1. Elija una región cerca de usted o de los recursos a los que desea acceder.

1. Tras responder a todos los mensajes, VS Code muestra los recursos de Azure que se estén creando para la aplicación en la ventana emergente de notificación.

    Al realizar la implementación en Linux, seleccione **Yes** (Sí) cuando se le pida que actualice la configuración para ejecutar `npm install` en el servidor Linux de destino.

    ![Solicitud de actualización de la configuración en el servidor Linux de destino](containers/media/quickstart-nodejs/server-build.png)

1. Seleccione **Yes** (Sí) cuando aparezca el mensaje **Always deploy the workspace "nodejs-docs-hello-world" to (app name)** (Implementar siempre el área de trabajo "nodejs-docs-hello-world" en [nombre de la aplicación]). Al seleccionar **Yes** (Sí) se indica a VS Code que el destino sea automáticamente la misma aplicación web de App Service en las implementaciones posteriores.

1. Si implementa en Linux, seleccione **Browse Website** (Examinar sitio web) en el mensaje para ver la aplicación web recién implementada una vez finalizado el proceso. El explorador debe mostrar "¡Hola mundo!"

1. Si implementa en Windows, primero debe establecer el número de versión de Node.js para la aplicación web:

    1. En VS Code, expanda el nodo del nuevo servicio de aplicaciones, haga clic con el botón derecho en **Application Settings** (Configuración de la aplicación) y seleccione **Add New Setting...** (Agregar nueva configuración):

        ![Comando para agregar configuración de aplicación](containers/media/quickstart-nodejs/add-setting.png)

    1. Escriba `WEBSITE_NODE_DEFAULT_VERSION` como clave de configuración.
    1. Escriba `10.15.2` como valor de configuración.
    1. Haga clic con el botón derecho en el nodo del servicio de aplicaciones y seleccione **Restart** (Reiniciar).

        ![Comando para reiniciar el servicio de aplicaciones](containers/media/quickstart-nodejs/restart.png)

    1. Haga clic con el botón derecho en el nodo del servicio de aplicaciones una vez más y seleccione **Browse Website** (Examinar sitio web).

> [!div class="nextstepaction"]
> [He tenido un problema](https://www.research.net/r/PWZWZ52?tutorial=node-deployment-azure-app-service&step=deploy-app)

### <a name="troubleshooting-azure-sign-in"></a>Solución de problemas de inicio de sesión en Azure

Si ve el error **"Cannot find subscription with name [subscription ID]"** ("No se encuentra la suscripción con el nombre [identificador de suscripción]"), puede que sea debido a que esté detrás de un proxy y no se pueda establecer la conexión con la API de Azure. Configure las variables de entorno `HTTP_PROXY` y `HTTPS_PROXY` con su información de proxy en el terminal mediante `export`.

```bash
export HTTPS_PROXY=https://username:password@proxy:8080
export HTTP_PROXY=http://username:password@proxy:8080
```

Si el problema no se corrige después de configurar las variables de entorno, seleccione el botón anterior **He tenido un problema** para ponerse en contacto con nosotros.

### <a name="update-the-app"></a>Actualización de la aplicación

Puede implementar los cambios en esta aplicación si realiza modificaciones en VS Code, guarda los archivos y, a continuación, usa el mismo proceso que antes, solo que esta vez eligiendo la aplicación existente en lugar de crear una nueva.

## <a name="viewing-logs"></a>Visualización de los registros

Puede ver la salida del registro (llamadas a `console.log`) desde la aplicación directamente en la ventana de salida de VS Code.

1. En el explorador de **AZURE APP SERVICE**, haga clic con el botón derecho en el nodo de la aplicación y elija **Start Streaming Logs** (Iniciar transmisión de registros).

    ![Inicio de transmisión de registros](containers/media/quickstart-nodejs/view-logs.png)

1. Cuando se le pida, seleccione la opción para habilitar el registro y reinicie la aplicación. Después de reiniciar la aplicación, se abrirá la ventana de salida de VS Code con una conexión al streaming de registros. 

    ![Activación de los registros y reinicio](containers/media/quickstart-nodejs/enable-restart.png)

1. Después de unos segundos, la ventana de salida muestra un mensaje donde se indica que está conectado al servicio de transmisión de registros. Puede generar más actividad de salida actualizando la página en el explorador.

    <pre>
    Connecting to log stream...
    2020-03-04T19:29:44  Welcome, you are now connected to log-streaming service. The default timeout is 2 hours.
    Change the timeout with the App Setting SCM_LOGSTREAM_TIMEOUT (in seconds).    
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
