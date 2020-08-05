---
title: 'Ejecución de varios servicios dependientes: Node.js y Visual Studio Code'
services: azure-dev-spaces
ms.date: 11/21/2018
ms.topic: tutorial
description: En este tutorial se muestra cómo usar Azure Dev Spaces y Visual Studio Code para depurar una aplicación Node.js multiservicio en Azure Kubernetes Service
keywords: Docker, Kubernetes, Azure, AKS, Azure Kubernetes Service, contenedores, Helm, service mesh, enrutamiento de service mesh, kubectl, k8s
ms.custom: devx-track-javascript
ms.openlocfilehash: 7a8f97df976b7e989021308385ef930e539a1e56
ms.sourcegitcommit: e71da24cc108efc2c194007f976f74dd596ab013
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 07/29/2020
ms.locfileid: "87414860"
---
# <a name="running-multiple-dependent-services-nodejs-and-visual-studio-code-with-azure-dev-spaces"></a>Ejecución de varios servicios dependientes: Node.js y Visual Studio Code con Azure Dev Spaces

En este tutorial, aprenderá cómo puede desarrollar aplicaciones de varios servicios con Azure Dev Spaces, junto con algunas de las ventajas que proporciona Dev Spaces.

## <a name="call-a-service-running-in-a-separate-container"></a>Llamada a un servicio que se ejecuta en un contenedor diferente

En esta sección va a crear un segundo servicio `mywebapi` y a hacer que `webfrontend` lo llame. Cada servicio se ejecutará en contenedores diferentes. A continuación, realizará la depuración entre los dos contenedores.

![El diagrama muestra el servicio de webfrontend que llama (como se indica mediante una flecha) al servicio mywebapi.](media/common/multi-container.png)

### <a name="open-sample-code-for-mywebapi"></a>Abra el código de ejemplo de *mywebapi*.
Ya debería tener el código de ejemplo de `mywebapi` necesario en esta guía, en una carpeta llamada `samples` (si no es así, vaya a https://github.com/Azure/dev-spaces y seleccione **Clone or Download** (Clonar o descargar) para descargar el repositorio de GitHub). El código de esta sección se encuentra en `samples/nodejs/getting-started/mywebapi`.

### <a name="run-mywebapi"></a>Ejecución de *mywebapi*
1. Abra la carpeta `mywebapi` en una *ventana independiente de VS Code*.
1. Abra la **Paleta de comandos** (mediante el menú **Vista | Paleta de comandos**) y use Autocompletar para escribir y seleccionar este comando: `Azure Dev Spaces: Prepare configuration files for Azure Dev Spaces`. Este comando no se debe confundir con el comando `azds prep`, que configura el proyecto para la implementación.
1. Presione F5 y espere a que el servicio se compile e implemente. Sabrá que está listo cuando aparezca el mensaje *Listening on port 80* (Escuchando en el puerto 80) en la consola de depuración.
1. Anote la dirección URL del punto de conexión; se parecerá a esta `http://localhost:<portnumber>`. **Sugerencia: La barra de estado de VS Code se volverá de color naranja y mostrará una dirección URL en la que se puede hacer clic.** Puede parecer que el contenedor se ejecuta localmente, pero en realidad se ejecuta en el entorno de desarrollo de Azure. La razón de la dirección de localhost es que `mywebapi` no ha definido ningún punto de conexión público y solo se puede acceder a él desde la instancia de Kubernetes. Para mayor comodidad, y para facilitar la interacción con el servicio privado desde la máquina local, Azure Dev Spaces crea un túnel SSH temporal al contenedor que se ejecuta en Azure.
1. Cuando `mywebapi` esté listo, abra el explorador en la dirección de localhost. Verá una respuesta del servicio `mywebapi` ("Hello from mywebapi").


### <a name="make-a-request-from-webfrontend-to-mywebapi"></a>Realización de una solicitud de *webfrontend* a *mywebapi*
Vamos ahora a escribir código en `webfrontend` que realiza una solicitud a `mywebapi`.
1. Cambie a la ventana de VS Code para `webfrontend`.
1. Agregue estas líneas de código en la parte superior de `server.js`:
    ```javascript
    var request = require('request');
    ```

3. *Reemplace* el código por el controlador GET `/api`. Al administrar una solicitud, se realiza a su vez una llamada a `mywebapi` y se devuelven los resultados de ambos servicios.

    ```javascript
    app.get('/api', function (req, res) {
       request({
          uri: 'http://mywebapi',
          headers: {
             /* propagate the dev space routing header */
             'azds-route-as': req.headers['azds-route-as']
          }
       }, function (error, response, body) {
           res.send('Hello from webfrontend and ' + body);
       });
    });
    ```
   1. *Quite* la línea `server.close()` al final de `server.js`

En el código de ejemplo anterior se reenvía el encabezado `azds-route-as` de la solicitud entrante a la solicitud saliente. Más adelante verá cómo esta función ayuda a los equipos con el desarrollo en colaboración.

### <a name="debug-across-multiple-services"></a>Depuración entre varios servicios
1. En este momento, `mywebapi` aún se estará ejecutando con el depurador asociado. Si no es así, presione F5 en el proyecto `mywebapi`.
1. Establezca un punto de interrupción en el controlador GET `/` predeterminado [en la línea 8 de `server.js`](https://github.com/Azure/dev-spaces/blob/master/samples/nodejs/getting-started/mywebapi/server.js#L8).
1. En el proyecto `webfrontend`, establezca un punto de interrupción justo antes de que se envíe una solicitud GET a `http://mywebapi`.
1. Presione F5 en el proyecto `webfrontend`.
1. Abra la aplicación web y recorra el código de ambos servicios. La aplicación web debe mostrar un mensaje concatenado por los dos servicios: "Hello from webfrontend y Hello from mywebapi".

### <a name="well-done"></a>¡Listo!
Ahora tiene una aplicación de varios contenedores donde cada uno se puede desarrollar e implementar por separado.

## <a name="next-steps"></a>Pasos siguientes

> [!div class="nextstepaction"]
> [Más información sobre el desarrollo en Dev Spaces](team-development-nodejs.md)
