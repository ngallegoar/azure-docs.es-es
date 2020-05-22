---
title: Incorporación de una API en Azure Static Web Apps con Azure Functions
description: Para empezar a usar Azure Static Web Apps, agregue una API sin servidor a la aplicación web estática mediante Azure Functions.
services: static-web-apps
author: manekinekko
ms.service: static-web-apps
ms.topic: how-to
ms.date: 05/08/2020
ms.author: wachegha
ms.openlocfilehash: a6aee5c8049e03a43c547f419f6c6646617e651c
ms.sourcegitcommit: bb0afd0df5563cc53f76a642fd8fc709e366568b
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 05/19/2020
ms.locfileid: "83596154"
---
# <a name="add-an-api-to-azure-static-web-apps-preview-with-azure-functions"></a>Incorporación de una API en la versión preliminar de Azure Static Web Apps con Azure Functions

Puede agregar API sin servidor a Azure Static Web Apps a través de la integración con Azure Functions. En este artículo se muestra cómo agregar e implementar una API en un sitio de Azure Static Web Apps.

Para obtener información sobre cómo proteger las rutas de la API, consulte la [guía de enrutamiento](routes.md).

## <a name="prerequisites"></a>Prerrequisitos

- Cuenta de Azure con una suscripción activa. [Cree una cuenta gratuita](https://azure.microsoft.com/free).
- [Visual Studio Code](https://code.visualstudio.com/)
- [Extensión de Azure Functions](https://marketplace.visualstudio.com/items?itemName=ms-azuretools.vscode-azurefunctions) para Visual Studio Code
- Extensión [Live Server de Visual Studio Code](https://marketplace.visualstudio.com/items?itemName=ritwickdey.LiveServer).

## <a name="create-a-git-repository"></a>Creación de un repositorio Git 

En los pasos siguientes se muestra cómo crear un nuevo repositorio y clonar los archivos en el equipo.

1. Diríjase a https://github.com/staticwebdev/vanilla-basic/generate para crear un nuevo repositorio.
1. En el cuadro _Nombre del repositorio_, escriba **my-vanilla-api**.
1. Haga clic en **Create repository from template** (Crear repositorio a partir de plantilla).

   :::image type="content" source="media/add-api/create-repository.png" alt-text="Creación de un nuevo repositorio a partir de vanilla-basic":::

Una vez creado el proyecto, puede usar Visual Studio Code para clonar el repositorio Git.

1. Presione **F1** para abrir un comando en la paleta de comandos.
1. Pegue la dirección URL en el cuadro _Git: Clone_ (Git: Clonar) y presione **Entrar**.

   :::image type="content" source="media/add-api/vscode-git-0.png" alt-text="Clonación de un proyecto de GitHub con Visual Studio Code":::

   :::image type="content" source="media/add-api/github-clone-url.png" alt-text="Clonación de un proyecto de GitHub con Visual Studio Code":::


## <a name="create-your-local-project"></a>Creación del proyecto local

En esta sección se usa Visual Studio Code para crear un proyecto local en Azure Functions. Más adelante, se publicará la aplicación de funciones en Azure.

1. Dentro del proyecto _my-vanilla-api_, cree una subcarpeta denominada **api**.

   > [!NOTE]
   > Puede asignar cualquier nombre a esta carpeta. En este ejemplo se usa `api`. 

2. Presione **F1** para abrir la paleta de comandos.
3. Escriba **Azure Functions: Crear nuevo proyecto...**
4. Presione **Entrar**.
5. Seleccione **Examinar**.
6. Seleccione la carpeta **api** como directorio del área de trabajo del proyecto.
7. Elija **Seleccionar**.

   :::image type="content" source="media/add-api/create-azure-functions-vscode-1.png" alt-text="Creación de una nueva instancia de Azure Functions mediante Visual Studio Code":::

8. Escriba la siguiente información cuando se le indique:

    - _Seleccione un lenguaje para el proyecto de funciones_: Elija **JavaScript**.
    - _Seleccione una plantilla para la primera función del proyecto_: Elija **desencadenador HTTP**.
    - _Especifique un nombre de función_: Escriba **GetMessage**.
    - _Nivel de autorización_: Elija **Anonymous** (anónimo), que permite que cualquiera llame al punto de conexión de la función.
        - Para obtener información sobre los niveles de autorización, consulte [Claves de autorización](../azure-functions/functions-bindings-http-webhook-trigger.md#authorization-keys).

9. Con esta información, Visual Studio Code genera un proyecto de Azure Functions con un desencadenador HTTP.
    - Puede ver los archivos del proyecto local en la ventana del explorador de Visual Studio Code.
    - Para obtener más información sobre los archivos que se crean, consulte [Archivos del proyecto generados](https://docs.microsoft.com/azure/azure-functions/functions-develop-vs-code#generated-project-files).

10. Ahora la aplicación debe tener una estructura de proyecto similar a la de este ejemplo.

    ```files
    ├── api
    │   ├── GetMessage
    │   │   ├── function.json
    │   │   ├── index.js
    │   │   └── sample.dat
    │   ├── host.json
    │   ├── local.settings.json
    │   ├── package.json
    │   └── proxies.json
    ├── index.html
    ├── readme.md
    └── styles.css
    ```

11. A continuación, actualice la función `GetMessage` en _api/GetMessage/index.js_ con el código siguiente.

    ```JavaScript
    module.exports = async function (context, req) {
      context.res = {
        body: { 
          text: "Hello from the API" 
        }
      };
    };
    ```

12. Actualice la configuración de `GetMessage` en `api/GetMessage/function.json` con las siguientes opciones.

    ```json
    {
      "bindings": [
        {
          "authLevel": "anonymous",
          "type": "httpTrigger",
          "direction": "in",
          "name": "req",
          "methods": [
            "get"
          ],
          "route": "message"
        },
        {
          "type": "http",
          "direction": "out",
          "name": "res"
        }
      ]
    }
    ```
    
Con la configuración anterior, el punto de conexión de la API:

- Se desencadena cuando se realiza una solicitud HTTP a la función
- Está disponible para todas las solicitudes, independientemente del estado de autenticación
- Está expuesta a través de la ruta _/api/message_

## <a name="run-the-function-locally"></a>Ejecución local de la función

Visual Studio Code se integra con [Azure Functions Core Tools](https://docs.microsoft.com/azure/azure-functions/functions-run-local) para que pueda ejecutar este proyecto en el equipo de desarrollo local antes de publicarlo en Azure.

1. Presione **F5** para ejecutar la función; de este modo, se inicia la aplicación de funciones y el resultado de Core Tools se muestra en el panel _Terminal_.

2. Si Azure Functions Core Tools aún no está instalado, seleccione **Instalar** en la ventana.

    Cuando se instala Core Tools, la aplicación se inicia en el panel _Terminal_. Como parte de la salida, puede ver el punto de conexión de la dirección URL de la función desencadenada por HTTP que se ejecuta localmente.

    :::image type="content" source="media/add-api/create-azure-functions-vscode-2.png" alt-text="Creación de una nueva instancia de Azure Functions mediante Visual Studio Code":::

3. Con Core Tools en ejecución, vaya a la siguiente dirección URL para ejecutar una solicitud `GET`.

   <http://localhost:7071/api/message>

   Se devuelve una respuesta, que tiene un aspecto similar al siguiente en el explorador:

   :::image type="content" source="media/add-api/create-azure-functions-vscode-3.png" alt-text="Creación de una nueva instancia de Azure Functions mediante Visual Studio Code":::

Después de comprobar que la función se ejecuta correctamente, puede llamar a la API desde la aplicación de JavaScript.

### <a name="call-the-api-from-the-application"></a>Llamada a la API desde la aplicación

[!INCLUDE [static-web-apps-local-proxy](../../includes/static-web-apps-local-proxy.md)]


#### <a name="update-files-to-access-the-api"></a>Actualización de archivos para acceder a la API

1. A continuación, actualice el contenido del archivo _index.html_ con el código siguiente para capturar el texto de la función de la API y mostrarlo en la pantalla:

   ```html
   <!DOCTYPE html>
   <html lang="en">

   <head>
     <meta charset="UTF-8">
     <meta name="viewport" content="width=device-width, initial-scale=1.0">
     <link rel="stylesheet" href="styles.css">
     <title>Vanilla JavaScript App</title>
   </head>

   <body>
     <main>
       <h1>Vanilla JavaScript App</h1>
       <p>Loading message from the API: <b id="name">...</b></p>
     </main>

     <script>
       (async function() {
         let { text } = await( await fetch(`/api/message`)).json();
         document.querySelector('#name').textContent = text;
       }())
     </script>
   </body>

   </html>
   ```

   Con Core Tools en ejecución, use la extensión [Live Server](https://marketplace.visualstudio.com/items?itemName=ritwickdey.LiveServer) de Visual Studio Code para proporcionar el archivo _index.html_ y abrirlo en el explorador. 

2. Presione **F1** y elija **Live Server: Open with Live Server** (Abrir con Live Server).

   :::image type="content" source="media/add-api/create-azure-functions-vscode-4.png" alt-text="Creación de una nueva instancia de Azure Functions mediante Visual Studio Code":::

   > [!NOTE]
   > Puede usar otros servidores proxy o HTTP para proporcionar el archivo `index.html`. No podrá acceder a `index.html` desde `file:///`.

### <a name="commit-and-push-your-changes-to-github"></a>Confirmación y envío de los cambios a GitHub

Con Visual Studio Code, confirme ("commit") e inserte los cambios ("push") en el repositorio Git remoto.

1. Presione **F1** para abrir la paleta de comandos.
1. Escriba **Git: Commit All** (Git: Confirmar todo)
1. Adición de un mensaje de confirmación
1. Escriba **Git: push** (Git: enviar cambios)

## <a name="create-a-static-web-app"></a>Creación de una aplicación web estática

:::image type="content" source="media/add-api/create-static-app-on-azure-portal-1.png" alt-text="Creación de una aplicación estática en Azure Portal: pantalla 1":::

1. Vaya a [Azure Portal](https://portal.azure.com).
1. Haga clic en **Crear un recurso**.
1. Busque **Static Web Apps**.
1. Haga clic en **Static Web Apps (versión preliminar)** .
1. Haga clic en **Crear**
1. Seleccione su _Suscripción a Azure_.
1. Seleccione o cree un nuevo _grupo de recursos_.
1. Asigne a la aplicación el nombre **my-vanilla-api**.
1. Seleccione la _región_ más cercana a la suya.
1. Seleccione la **SKU** _gratis_.
1. Haga clic en el botón **Iniciar sesión con GitHub** y realice la autenticación con GitHub.
1. Seleccione su _organización_ preferida.
1. Seleccione **my-vanilla-api** del menú desplegable _Repositorio_.
1. Seleccione **master** en el menú desplegable _Rama_.
1. Haga clic en el botón **Siguiente: Compilar >** para editar la configuración de compilación.

:::image type="content" source="media/add-api/create-static-app-on-azure-portal-2.png" alt-text="Creación de una aplicación estática en Azure Portal: pantalla 2":::

A continuación, agregue los siguientes detalles de compilación.

1. Escriba **./** como _Ubicación de la aplicación_.

1. Escriba **api** en el cuadro _Ubicación de la API_.

   Este es el nombre predeterminado de la carpeta de la API creada en el paso anterior.
   
1. Borre el valor predeterminado de la _Ubicación del artefacto de la aplicación_ y deje el cuadro vacío.

1. Haga clic en **Revisar + crear**.

| Configuración | Descripción             | Obligatorio |
| -------- | ----------------------- |
|  Ubicación de la aplicación | Ubicación del código fuente de la aplicación estática. | Sí |
|  Ubicación de la API | Ubicación del back-end de la API. Apunta a la carpeta raíz del proyecto de la aplicación de Azure Functions | No |
|  Ubicación del artefacto de la aplicación | Ubicación de la carpeta de salida de la compilación. Algunos marcos de JavaScript de front-end tienen un paso de compilación que coloca los archivos de producción en una carpeta. Esta configuración apunta a la carpeta de salida de la compilación. | No |

:::image type="content" source="media/add-api/create-static-app-on-azure-portal-3.png" alt-text="Creación de una aplicación estática en Azure Portal: pantalla 3":::

1. Haga clic en **Crear**
1. Espere a que finalice la implementación (esto puede tardar un momento).
1. Vaya a `https://github.com/<YOUR_GITHUB_ACCOUNT>/my-vanilla-api/actions?query=workflow%3A"Azure+Pages+CI%2FCD"`.
1. Asegúrese de que la compilación se haya realizado correctamente.

:::image type="content" source="media/add-api/github-workflow-0.png" alt-text="Flujo de trabajo de GitHub":::

La API implementada estará disponible en `https://<STATIC_APP_NAME>.azurestaticapps.net/api/<FUNCTION_OR_ROUTE_NAME>`.

:::image type="content" source="media/add-api/github-workflow-1.png" alt-text="Flujo de trabajo de GitHub":::

También puede encontrar la dirección URL de la aplicación en Azure Portal:

:::image type="content" source="media/add-api/static-app-url-from-portal.png" alt-text="Acceso a la dirección URL de la aplicación estática desde Azure Portal":::

También puede acceder directamente a su aplicación de Azure Static Web App desde `https://<STATIC_APP_NAME>.azurestaticapps.net` y comprobar el resultado en el explorador.

## <a name="clean-up-resources"></a>Limpieza de recursos

Si no quiere conservar esta aplicación para usos adicionales, puede usar los pasos siguientes para eliminar la aplicación de Azure Static Web App y sus recursos relacionados.

1. Vaya a [Azure Portal](https://portal.azure.com).
1. En la barra de búsqueda de la parte superior, escriba **Grupos de recursos**.
1. Haga clic en **Grupos de recursos**. 
1. Seleccione **myResourceGroup**.
1. En la página _myResourceGroup_, asegúrese de que los recursos enumerados sean los que desea eliminar.
1. Seleccionar **Eliminar**
1. Escriba **myResourceGroup** en el cuadro de texto.
1. Seleccione **Eliminar**.


## <a name="next-steps"></a>Pasos siguientes

> [!div class="nextstepaction"]
> [Configuración de aplicaciones](./application-settings.md)
