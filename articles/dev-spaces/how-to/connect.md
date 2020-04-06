---
title: Conexión de un equipo de desarrollo a un clúster de AKS (versión preliminar)
services: azure-dev-spaces
ms.date: 11/04/2019
ms.topic: conceptual
description: Obtenga información sobre cómo conectar el equipo de desarrollo a un clúster de AKS con Azure Dev Spaces
keywords: Azure Dev Spaces, Dev Spaces, Docker, Kubernetes, Azure, AKS, Azure Kubernetes Service, contenedores
ms.openlocfilehash: 772f221a8047a71902f36fa98ded6c24b5e02d27
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/28/2020
ms.locfileid: "80235015"
---
# <a name="connect-your-development-computer-to-an-aks-cluster-preview"></a>Conexión de un equipo de desarrollo a un clúster de AKS (versión preliminar)

Azure Dev Spaces le permite ejecutar y depurar código con o sin un contenedor en el equipo de desarrollo, mientras sigue conectado al clúster de Kubernetes con el resto de la aplicación o los servicios. La conexión del equipo de desarrollo al clúster le ayuda a desarrollar rápidamente la aplicación y realizar pruebas de un extremo a otro sin tener que crear ninguna configuración de Docker o Kubernetes. También puede conectarse al clúster de AKS sin que afecte a otras cargas de trabajo o usuarios que puedan estar usando el mismo clúster.

Azure Dev Spaces redirige el tráfico entre el clúster de AKS conectado y el equipo de desarrollo. Esta redirección del tráfico permite que el código del equipo de desarrollo y los servicios que se ejecutan en el clúster de AKS se comuniquen como si estuvieran en el mismo clúster de AKS. Dado que el código se ejecuta en el equipo de desarrollo, también tiene flexibilidad en las herramientas de desarrollo que usa para ejecutar y depurar ese código. Azure Dev Spaces también proporciona una manera de replicar variables de entorno y archivos montados disponibles para pods en el clúster de AKS en el equipo de desarrollo.

En esta guía, aprenderá a:

* Configurar Azure Dev Spaces en un clúster de Kubernetes administrado en Azure.
* Implementar una aplicación de gran tamaño con varios microservicios en un espacio de desarrollo.
* Use Azure Dev Spaces para redirigir el tráfico entre el clúster de AKS conectado y el código que se ejecuta en el equipo de desarrollo.

> [!IMPORTANT]
> Esta funcionalidad actualmente está en su versión preliminar. Las versiones preliminares están a su disposición con la condición de que acepte los [términos de uso adicionales](https://azure.microsoft.com/support/legal/preview-supplemental-terms/). Es posible que algunos de los aspectos de esta característica cambien antes de ofrecer disponibilidad general.

## <a name="before-you-begin"></a>Antes de empezar

En esta guía se usa la [aplicación de ejemplo Bike Sharing de Azure Dev Spaces](https://github.com/Azure/dev-spaces/tree/master/samples/BikeSharingApp) para demostrar la conexión del equipo de desarrollo a un clúster de AKS. Siga las instrucciones de la página [LÉAME de la aplicación de ejemplo Bike Sharing de Azure Dev Spaces](https://github.com/Azure/dev-spaces/blob/master/samples/BikeSharingApp/README.md) para ejecutar la aplicación de ejemplo. Como alternativa, si tiene su propia aplicación en un clúster de AKS, puede seguir los pasos que se indican a continuación y usar los nombres de sus propios servicios y pods.

### <a name="limitations"></a>Limitaciones

* UDP no se admite en este momento.

### <a name="prerequisites"></a>Prerrequisitos

* Suscripción a Azure. Si no tiene una suscripción a Azure, puede crear una [cuenta gratuita](https://azure.microsoft.com/free).
* [La CLI de Azure instalada][azure-cli].
* [Visual Studio Code][vs-code] con la extensión [Azure Dev Spaces][azds-vs-code] instalada y ejecutándose en MacOS o Windows 10.
* La [aplicación de ejemplo Bike Sharing de Azure Dev Spaces](https://github.com/Azure/dev-spaces/tree/master/samples/BikeSharingApp) o su propia aplicación que se ejecuta en un clúster de AKS.

## <a name="connect-your-development-computer"></a>Conectar el equipo de desarrollo

Abra *dev-spaces/samples/BikeSharingApp/Bikes* en Visual Studio Code y use la extensión de Azure Dev Spaces para conectar el equipo de desarrollo al clúster de AKS.

Para usar la extensión de Azure Dev Spaces, abra la paleta de comandos en Visual Studio Code. Para ello, haga clic en *Ver* y después en *Paleta de comandos*. Empiece a escribir `Azure Dev Spaces: Redirect` y haga clic en `Azure Dev Spaces: Redirect an existing Kubernetes service to my machine [Preview]`, `Azure Dev Spaces: Redirect an existing Kubernetes pod to my machine [Preview]` o `Azure Dev Spaces: Redirect a new Kubernetes pod to my machine [Preview]`.

![Comandos:](../media/how-to-connect/connect-commands.png)

### <a name="select-a-redirection-option"></a>Seleccionar una opción de redirección

Si ejecuta `Azure Dev Spaces: Redirect an existing Kubernetes service to my machine [Preview]`, se le pedirá que elija un servicio de Kubernetes existente:

![Elegir servidor](../media/how-to-connect/connect-choose-service.png)

Esta opción redirige todo el tráfico del clúster de AKS para este servicio a la versión de la aplicación que se ejecuta en el equipo de desarrollo. Si este servicio tiene varios pods en ejecución en el clúster de AKS, todo el tráfico de este servicio se enrutará solo al equipo de desarrollo. Azure Dev Spaces también enruta todo el tráfico saliente desde la aplicación de vuelta al clúster de AKS.

Si ejecuta `Azure Dev Spaces: Redirect an existing Kubernetes pod to my machine [Preview]`, se le pedirá que elija un pod específico:

![Elegir el pod](../media/how-to-connect/connect-choose-pod.png)

Esta opción se conecta a un pod específico. Esta opción es útil para interactuar con los pods que no envían ni reciben tráfico y replican pods terminados. Si el pod envía y recibe tráfico, esta opción se comporta de forma similar a `Azure Dev Spaces: Redirect an existing Kubernetes service to my machine [Preview]` y redirigirá todo el tráfico del clúster de AKS para todos los pods relacionados con el servicio del pod seleccionado.

Si ejecuta `Azure Dev Spaces: Redirect a new Kubernetes pod to my machine [Preview]`, no se le pedirá que seleccione un pod o servicio existente. Esta opción redirige todo el tráfico saliente desde la aplicación que se ejecuta en el equipo de desarrollo al clúster de AKS.

En este ejemplo, elija `Azure Dev Spaces: Redirect an existing Kubernetes service to my machine [Preview]` y seleccione el servicio *Bikes*.

### <a name="select-a-connection-mode"></a>Seleccionar un modo de conexión

Después de seleccionar la opción de redirección, se le pedirá que elija el modo de conexión *Reemplazar* o *Clonar*.

![Reemplazar o clonar](../media/how-to-connect/connect-replace-clone.png)

La opción *Reemplazar* reemplaza el pod o el servicio actual en el clúster de AKS y redirige todo el tráfico de ese servicio al equipo de desarrollo. Esta opción puede ser perjudicial para otros servicios en el clúster de AKS que interactúen con el servicio que está redirigiendo y puede que no funcione hasta que inicie la aplicación en el equipo de desarrollo. La opción *Clonar* permite elegir un espacio de desarrollo secundario existente o crear un nuevo espacio de desarrollo secundario para redirigir el tráfico de un pod o servicio al equipo de desarrollo. Esta opción le permite trabajar de forma aislada y no interrumpir otros servicios, ya que solo se redirigirá al equipo de desarrollo el tráfico a ese espacio de desarrollo secundario. La opción *Clonar* requiere que el clúster de AKS tenga Azure Dev Spaces habilitado.

En este ejemplo, elija *Reemplazar*.

> [!NOTE]
> Si el pod del servicio existente tiene varios contenedores, también se le pedirá que elija el contenedor de la aplicación.

### <a name="select-a-port-for-your-application"></a>Seleccionar un puerto para la aplicación

Después de seleccionar el modo de conexión, se le pedirá que escriba el puerto TCP de la aplicación local. Si la aplicación abre varios puertos, sepárelos con una coma, por ejemplo, *80,81*. Si la aplicación no acepta solicitudes de red, escriba *0*. Para este ejemplo, escriba *3000*.

![Conectar elegir puerto](../media/how-to-connect/connect-choose-port.png)

### <a name="confirm-you-are-connected"></a>Confirmar que está conectado

Después de seleccionar el puerto TCP de la aplicación, Azure Dev Spaces establecerá una conexión con el clúster de AKS. Azure Dev Spaces inserta un agente en el clúster de AKS para redirigir el tráfico entre el clúster de AKS y el equipo de desarrollo. El establecimiento de esta conexión puede tardar unos minutos. Azure Dev Spaces también solicitará acceso de administrador para modificar el archivo de *hosts* en el equipo de desarrollo.

> [!IMPORTANT]
> Una vez que Azure Dev Spaces establezca una conexión con el clúster de AKS, es posible que los demás servicios del clúster de AKS no funcionen correctamente hasta que inicie el servicio en el equipo de desarrollo si elige el modo de conexión *Reemplazar*. En su lugar, puede elegir el modo de conexión *Clonar* para crear un espacio de desarrollo secundario para el redireccionamiento y evitar cualquier interrupción en el espacio primario. Además, si el servicio tiene una dependencia que no está disponible en el equipo de desarrollo, es posible que tenga que modificar la aplicación o proporcionar una [configuración adicional](#additional-configuration).

Azure Dev Spaces abre una ventana de terminal titulada *AZDS Connect - Bikes* después de establecer una conexión con el clúster de AKS. Esta ventana de terminal tiene todas las variables de entorno y las entradas de DNS configuradas desde el clúster de AKS. Cualquier código que se ejecute en esta ventana de terminal o que use el depurador de Visual Studio Code se conecta al clúster de AKS.

![Terminal](../media/how-to-connect/connect-terminal.png)

Además, Azure Dev Spaces crea una ventana con el título *Dev Spaces Connect* con todos los resultados.

![Output](../media/how-to-connect/connect-output.png)

Azure Dev Spaces también tiene un elemento de barra de estado que muestra el estado de la conexión.

![Status](../media/how-to-connect/connect-status.png)

Compruebe que la barra de estado muestre *Dev Spaces: Connected to dev/bikes en el puerto local 3000*.

### <a name="configure-your-application-on-your-development-computer"></a>Configurar la aplicación en el equipo de desarrollo

Abra la ventana de terminal *AZDS Connect - Bikes* y ejecute `npm install`:

```console
$ npm install

> fsevents@1.2.9 install ./dev-spaces/samples/BikeSharingApp/Bikes/node_modules/fsevents
> node install
...
```

Haga clic en *Depurar* y, después, en *Abrir configuraciones*. Si se le pide que seleccione un entorno, elija *Node.js*. Esto crea un archivo `.vscode/launch.json`. Reemplace el contenido de ese archivo con lo siguiente:

```json
{
    "configurations": [
        {
            "type": "node",
            "request": "launch",
            "name": "Launch via NPM",
            "runtimeExecutable": "npm",
            "runtimeArgs": [
                "run-script",
                "debug"
            ],
            "port": 9229
        }
    ]
}
```

Abra [package.json](https://github.com/Azure/dev-spaces/blob/master/samples/BikeSharingApp/Bikes/package.json) y agregue un script de depuración:

```json
  "devDependencies": {
    "nodemon": "^1.18.10"
  },
  "scripts": {
    "debug": "node --nolazy --inspect-brk=9229 server.js"
  }
```

### <a name="start-your-application-on-your-development-computer"></a>Iniciar la aplicación en el equipo de desarrollo

Haga clic en el icono *Depurar* de la izquierda y haga clic en el botón Inicio junto a *Iniciar mediante NPM* en la parte superior.

![Iniciar mediante NPM](../media/how-to-connect/launch-npm.png)

La aplicación se iniciará y Azure Dev Spaces redirigirá el tráfico entre el clúster de AKS conectado y el equipo de desarrollo. Verá mensajes similares a los siguientes en la *Consola de depuración*:

```console
/usr/local/bin/npm run-script debug 
...
Debugger attached.
Collection: bikes
MongoDB connection string: mongodb://databases-mongo
Connected to MongoDB
Listening on port 3000
```

Para navegar al servicio *bikesharingweb*, haga clic en la barra de estado Azure Dev Spaces y elija la dirección URL pública de la aplicación. También encontrará la dirección URL pública en el comando `azds list-uris` que ejecutó anteriormente. Si no usa Azure Dev Spaces en el clúster, use la dirección IP o la dirección URL de la aplicación para el espacio de nombres que está usando. En el ejemplo anterior, la dirección URL pública para el servicio *bikesharingweb* es `http://dev.bikesharingweb.fedcab0987.eus.azds.io/`. Seleccione *Aurelia Briggs (customer)* (Aurelia Briggs [cliente]) como usuario y, después, seleccione una bicicleta para alquilar.

### <a name="set-a-break-point"></a>Establecer un punto de interrupción

Abra [server.js](https://github.com/Azure/dev-spaces/blob/master/samples/BikeSharingApp/Bikes/server.js#L233) y haga clic en algún lugar de la línea 233 para colocar ahí el cursor. Para establecer un punto de interrupción, pulse *F9* o haga clic en *Depurar* y en *Alternar punto de interrupción*.

Para navegar al servicio *bikesharingweb*, abra la dirección URL pública. Seleccione *Aurelia Briggs (customer)* (Aurelia Briggs [cliente]) como usuario y, después, seleccione una bicicleta para alquilar. Observe que la imagen de la bicicleta no se carga. Vuelva a Visual Studio Code y observe que la línea 233 está resaltada. El punto de interrupción que estableció puso en pausa el servicio en la línea 233. Para reanudar el servicio, pulse *F5* o haga clic en *Depurar* y en *Continuar*. Vuelva al explorador y compruebe que ve una imagen de marcador de posición para la bicicleta.

Quite el punto de interrupción colocando el cursor en la línea 233 en `server.js` y pulsando *F9*.

### <a name="update-your-application"></a>Actualización de la aplicación

Edite `server.js` para quitar las líneas 232 y 233:

```javascript
    // Hard code image url *FIX ME*
    theBike.imageUrl = "/static/logo.svg";
```

Ahora, la sección debe tener un aspecto similar al siguiente:

```javascript
    var theBike = result;
    theBike.id = theBike._id;
    delete theBike._id;
```

Guarde los cambios y haga clic en *Depurar* y después en *Reiniciar depuración*. Actualice el explorador y compruebe que ya no ve una imagen de marcador de posición para la bicicleta.

Haga clic en *Depurar* y en *Detener depuración* para detener el depurador. Haga clic en la barra de estado Azure Dev Spaces para desconectarse del clúster de AKS.

## <a name="additional-configuration"></a>Configuración adicional

Azure Dev Spaces puede controlar el tráfico de enrutamiento y las variables del entorno de replicación sin ninguna configuración adicional. Si necesita descargar los archivos que se montan en el contenedor del clúster de AKS, como un archivo ConfigMap, puede crear un `azds-local.env` para descargar esos archivos en el equipo de desarrollo.

Aquí tiene un `azds-local.env` de ejemplo:

```
# This downloads the "whitelist" volume from the container,
# saves it to a temporary directory on your development computer,
# and sets the full path to an environment variable called WHITELIST_PATH.

WHITELIST_PATH=${volumes.whitelist}/whitelist

# This downloads a file from the container's 'default-token-<any>' mount directory 
# to /var/run/secrets/kubernetes.io/serviceaccount on your development computer.

KUBERNETES_IN_CLUSTER_CONFIG_OVERWRITE=${volumes.default-token-*|/var/run/secrets/kubernetes.io/serviceaccount}


# This makes the myapp1 service available to your development computer
# regardless of the AKS cluster you are connected to and
# sets the local IP to an environment variable called MYAPP1_SERVICE_HOST.

# If the myapp1 service is made available in this way, 
# you can also access it using "myapp1" and "myapp1.svc.cluster.local"
# in addition to the IP in the MYAPP1_SERVICE_HOST environment variable.

MYAPP1_SERVICE_HOST=${services.myapp1}

# This makes the service myapp2 in namespace mynamespace available to your 
# development computer regardless of the AKS cluster you are connected to and
# sets the local IP to an environment variable called MYAPP2_SERVICE_HOST.

MYAPP2_SERVICE_HOST=${services.mynamespace.myapp2}
```

## <a name="using-logging-and-diagnostics"></a>Uso de registro y diagnóstico

La salida del registro se escribe en la ventana *Dev Spaces Connect* después de conectar el equipo de desarrollo al clúster de AKS.

![Output](../media/how-to-connect/connect-output.png)

Haga clic en la barra de estado de Azure Dev Spaces y elija *Mostrar información de diagnóstico*. Este comando imprime las variables de entorno actuales y las entradas de DNS en la salida del registro.

![Salida con diagnósticos](../media/how-to-connect/connect-output-diagnostics.png)

Además, puede encontrar los registros de diagnóstico en el directorio `Azure Dev Spaces` en el [ directorio *TEMP* del equipo de desarrollo][azds-tmp-dir].

## <a name="next-steps"></a>Pasos siguientes

Aprenda a usar las acciones de GitHub y Azure Dev Spaces para probar los cambios de una solicitud de incorporación de cambios directamente en AKS antes de que dicha solicitud se combine en la rama principal del repositorio.

> [!div class="nextstepaction"]
> [Acciones de GitHub y Azure Kubernetes Service][gh-actions]

[azds-tmp-dir]: ../troubleshooting.md#before-you-begin
[azds-vs-code]: https://marketplace.visualstudio.com/items?itemName=azuredevspaces.azds
[azure-cli]: /cli/azure/install-azure-cli?view=azure-cli-latest
[bike-sharing-github]: https://github.com/Azure/dev-spaces/tree/master/samples/BikeSharingApp
[gh-actions]: github-actions.md
[supported-regions]: https://azure.microsoft.com/global-infrastructure/services/?products=kubernetes-service
[team-quickstart]: ../quickstart-team-development.md
[vs-code]: https://code.visualstudio.com/download