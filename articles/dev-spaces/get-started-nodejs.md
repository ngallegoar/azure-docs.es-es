---
title: 'Creación de un espacio de desarrollo de Kubernetes: Visual Studio Code y Node.js'
services: azure-dev-spaces
ms.date: 09/26/2018
ms.topic: tutorial
description: En este tutorial se muestra cómo usar Azure Dev Spaces y Visual Studio Code para depurar e iterar rápidamente una aplicación Node.js en Azure Kubernetes Service
keywords: Docker, Kubernetes, Azure, AKS, Azure Kubernetes Service, contenedores, Helm, service mesh, enrutamiento de service mesh, kubectl, k8s
ms.openlocfilehash: 6571e23c3ca9b67d4db3c9c7bcea1e4a3b80e4c1
ms.sourcegitcommit: 253d4c7ab41e4eb11cd9995190cd5536fcec5a3c
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/25/2020
ms.locfileid: "80240514"
---
# <a name="create-a-kubernetes-dev-space-visual-studio-code-and-nodejs-with-azure-dev-spaces"></a>Creación de un espacio de desarrollo de Kubernetes: Visual Studio Code y Node.js con Azure Dev Spaces

En esta guía, aprenderá a:

- Crear un entorno basado en Kubernetes en Azure que esté optimizado para el desarrollo (un _espacio de desarrollo_).
- Desarrollar código de forma iterativa en contenedores con VS Code y la línea de comandos.
- Desarrollar y probar de forma productiva el código en un entorno de equipo.

> [!Note]
> **Si se queda bloqueado** en cualquier momento, consulte la sección [Solución de problemas](troubleshooting.md).

## <a name="install-the-azure-cli"></a>Instalación de la CLI de Azure
Azure Dev Spaces requiere una configuración mínima de la máquina local. La mayor parte de la configuración del espacio de desarrollo se almacena en la nube y se puede compartir con otros usuarios. Para comenzar, descargue y ejecute la [CLI de Azure](/cli/azure/install-azure-cli?view=azure-cli-latest).

### <a name="sign-in-to-azure-cli"></a>Inicio de sesión en la CLI de Azure
Inicie sesión en Azure. Escriba el siguiente comando en una ventana de terminal:

```azurecli
az login
```

> [!Note]
> Si no tiene una suscripción a Azure, puede crear una [cuenta gratuita](https://azure.microsoft.com/free).

#### <a name="if-you-have-multiple-azure-subscriptions"></a>Si tiene varias suscripciones a Azure...
Para ver sus suscripciones, ejecute: 

```azurecli
az account list --output table
```

Busque la suscripción que tiene *True* para *IsDefault*.
Si esta no es la suscripción que desea usar, puede cambiar la suscripción predeterminada:

```azurecli
az account set --subscription <subscription ID>
```

## <a name="create-a-kubernetes-cluster-enabled-for-azure-dev-spaces"></a>Creación de un clúster de Kubernetes habilitado para Azure Dev Spaces

En el símbolo del sistema, cree el grupo de recursos en una [región que admita Azure Dev Spaces][supported-regions].

```azurecli
az group create --name MyResourceGroup --location <region>
```

Crear un clúster de Kubernetes con el siguiente comando:

```azurecli
az aks create -g MyResourceGroup -n MyAKS --location <region> --generate-ssh-keys
```

La operación de creación del clúster tarda unos minutos.

### <a name="configure-your-aks-cluster-to-use-azure-dev-spaces"></a>Configuración del clúster de AKS para usar Azure Dev Spaces

Escriba el siguiente comando de la CLI de Azure, mediante el grupo de recursos que contiene el clúster de AKS, y su nombre del clúster de AKS. El comando configura el clúster para la compatibilidad con Azure Dev Spaces.

   ```azurecli
   az aks use-dev-spaces -g MyResourceGroup -n MyAKS
   ```

> [!IMPORTANT]
> El proceso de configuración de Azure Dev Spaces quitará el espacio de nombres `azds` del clúster, si existe.

## <a name="get-kubernetes-debugging-for-vs-code"></a>Obtención de depuración de Kubernetes para VS Code
Hay características enriquecidas, como la depuración de Kubernetes, disponibles para los desarrolladores de .NET Core y Node.js que usan VS Code.

1. Si no lo tiene, instale [VS Code](https://code.visualstudio.com/Download).
1. Descargue e instale la [extensión para VS de Azure Dev Spaces](https://marketplace.visualstudio.com/items?itemName=azuredevspaces.azds). Haga clic en Instalar una vez que se encuentre en la página de Marketplace de la extensión, y de nuevo en VS Code. 

## <a name="create-a-nodejs-container-in-kubernetes"></a>Creación de un contenedor de Node.js en Kubernetes

En esta sección, va a crear una aplicación web de Node.js y a ejecutarla en un contenedor de Kubernetes.

### <a name="create-a-nodejs-web-app"></a>Creación de una aplicación web de Node.js
Descargue el código de GitHub en [https://github.com/Azure/dev-spaces](https://github.com/Azure/dev-spaces) y seleccione **Clone or Download** (Clonar o descargar) para descargar el repositorio de GitHub en su entorno local. El código de esta guía se encuentra en `samples/nodejs/getting-started/webfrontend`.

## <a name="prepare-code-for-docker-and-kubernetes-development"></a>Preparación del código para el desarrollo de Docker y Kubernetes
Hasta ahora, tiene una aplicación web básica que se puede ejecutar localmente. Ahora podrá incluirla en contenedores creando activos que definen el contenedor de la aplicación y cómo se implementará en Kubernetes. Esta tarea es fácil de hacer con Azure Dev Spaces: 

1. Ejecute VS Code y abra la carpeta `webfrontend`. (Puede omitir cualquier mensaje predeterminado para agregar recursos de depuración o restaurar el proyecto).
1. Abra el terminal integrado en VS Code (mediante el menú **Ver | Terminal integrado**.
1. Ejecute este comando (asegúrese de que **webfrontend** es la carpeta actual):

    ```cmd
    azds prep --enable-ingress
    ```

El comando `azds prep` de la CLI de Azure comando genera recursos de Docker y Kubernetes con la configuración predeterminada:
* `./Dockerfile` describe la imagen de contenedor de la aplicación y cómo el código fuente se compila y se ejecuta dentro del contenedor.
* A [gráfico de Helm](https://docs.helm.sh) bajo `./charts/webfrontend` describe cómo implementar el contenedor en Kubernetes.

> [!TIP]
> Azure Dev Spaces usa [Dockerfile y el gráfico de Helm](how-dev-spaces-works-prep.md#prepare-your-code) del proyecto para compilar y ejecutar el código, pero el usuario puede modificar estos archivos si desea cambiar la forma en que se compila y se ejecuta el proyecto.

Por ahora, no es necesario comprender todo el contenido de estos archivos. Sin embargo, vale la pena señalar que **los mismos recursos de configuración como código de Kubernetes y Docker se pueden usar desde el desarrollo hasta la producción, lo que proporciona una mejor coherencia entre diferentes entornos.**
 
Un archivo denominado `./azds.yaml` también se genera mediante el comando `prep`; se trata del archivo de configuración para Azure Dev Spaces. Complementa los artefactos de Docker y Kubernetes sin configuración adicional que posibilita una experiencia de desarrollo iterativo en Azure.

## <a name="build-and-run-code-in-kubernetes"></a>Compilación y ejecución de código en Kubernetes
Vamos a ejecutar el código. En la ventana de terminal, ejecute este comando desde la **carpeta de código raíz** WebFrontEnd:

```cmd
azds up
```

Esté atento en la salida del comando, ya que observará varios aspectos a medida que avanza:
- El código fuente se sincroniza con el espacio de desarrollo en Azure.
- Se crea una imagen de contenedor en Azure según lo que especifiquen los recursos de Docker en la carpeta de código.
- Los objetos de Kubernetes que se crean utilizan la imagen de contenedor según lo que especifica el gráfico de Helm en la carpeta de código.
- Se muestra información acerca de los puntos de conexión del contenedor. En nuestro caso, se espera una dirección URL HTTP pública.
- Suponiendo que las fases anteriores se completen correctamente, debería empezar a ver la salida `stdout` (y `stderr`) a medida que se inicia el contenedor.

> [!Note]
> Estos pasos tardarán más tiempo la primera vez que se ejecute el comando `up`, pero las ejecuciones posteriores deberían ser más rápidas.

### <a name="test-the-web-app"></a>Prueba de la aplicación web
Examine la salida de la consola para obtener información sobre la dirección URL pública que creó el comando `up`. Tendrá el formato siguiente: 

```
(pending registration) Service 'webfrontend' port 'http' will be available at <url>
Service 'webfrontend' port 'http' is available at http://webfrontend.1234567890abcdef1234.eus.azds.io/
Service 'webfrontend' port 80 (TCP) is available at 'http://localhost:<port>'
```

Identifique la dirección URL pública para el servicio en la salida desde el comando `up`. Finaliza en `.azds.io`. En el ejemplo anterior, la dirección URL pública es `http://webfrontend.1234567890abcdef1234.eus.azds.io/`.

Para ver la aplicación web, abra la dirección URL pública en un explorador. Además, tenga en cuenta que la salida `stdout` y `stderr` se transmite a la ventana del terminal *azds-trace* a medida que interactúa con su aplicación web. También verá la información de seguimiento de las solicitudes HTTP a medida que pasan por el sistema. Esto le facilita el seguimiento de llamadas complejas de múltiples servicios durante el desarrollo. La instrumentación agregada por Dev Spaces proporciona este seguimiento de las solicitudes.

> [!Note]
> Además de la dirección URL pública, puede usar la dirección URL alternativa `http://localhost:<portnumber>` que se muestra en la salida de la consola. Si utiliza la dirección URL del host local, puede parecer que el contenedor se ejecuta localmente, pero en realidad se ejecuta en Azure. Azure Dev Spaces usa la funcionalidad de *reenvío de puerto* de Kubernetes para asignar el puerto localhost al contenedor que se ejecuta en AKS. Esto facilita la interacción con el servicio desde la máquina local.

### <a name="update-a-content-file"></a>Actualización de un archivo de contenido
Con Azure Dev Spaces no se trata solo de conseguir que el código se ejecute en Kubernetes, sino de permitirle ver rápida e iterativamente que los cambios de código surtan efecto en un entorno de Kubernetes en la nube.

1. Busque el archivo `./public/index.html` y realice una edición en el código HTML. Por ejemplo, cambie el color de fondo de la página por un tono azul [en la línea 15](https://github.com/Azure/dev-spaces/blob/master/samples/nodejs/getting-started/webfrontend/public/index.html#L15):

    ```html
    <body style="background-color: #95B9C7; margin-left:10px; margin-right:10px;">
    ```

1. Guarde el archivo. Momentos después, en la ventana de terminal verá un mensaje que indica que se ha actualizado un archivo en el contenedor en ejecución.
1. Regrese al explorador y actualice la página. Verá la actualización del color.

¿Qué ha ocurrido? Las ediciones en los archivos de contenido, como HTML y CSS, no requieren que se reinicie el proceso Node.js, por lo que un comando `azds up` activo sincronizará automáticamente cualquier archivo de contenido modificado directamente en el contenedor en ejecución en Azure, proporcionando así una forma rápida de ver las ediciones del contenido.

### <a name="test-from-a-mobile-device"></a>Prueba en un dispositivo móvil
Abra la aplicación web en un dispositivo móvil mediante la dirección URL pública para webfrontend. Se recomienda que copie y envíe la dirección URL desde el escritorio al dispositivo para no tener que escribir la dirección larga. Cuando la aplicación web se cargue en el dispositivo móvil, notará que la interfaz de usuario no se muestra correctamente en un dispositivo pequeño.

Para solucionar este problema, va a agregar una etiqueta META `viewport`:
1. Abra el archivo `./public/index.html`.
1. Agregue la metaetiqueta `viewport` al elemento existente `head` que comienza [en la línea 6](https://github.com/Azure/dev-spaces/blob/master/samples/nodejs/getting-started/webfrontend/public/index.html#L6):

    ```html
    <head>
        <!-- Add this line -->
        <meta name="viewport" content="width=device-width, initial-scale=1">
    </head>
    ```

1. Guarde el archivo.
1. Actualice el explorador del dispositivo. Ahora verá la aplicación web correctamente representada. 

Este es un ejemplo de cómo algunos problemas no se encuentran hasta que se realiza una prueba en los dispositivos en los que se debe usar una aplicación. Con Azure Dev Spaces de Visual Studio, puede iterar rápidamente su código y validar cualquier cambio en los dispositivos de destino.

### <a name="update-a-code-file"></a>Actualización de un archivo de código
Actualizar los archivos de código del lado servidor requiere un poco más de trabajo, porque hay que reiniciar la aplicación Node.js.

1. En la ventana de terminal, presione `Ctrl+C` (para detener `azds up`).
1. Abra el archivo de código denominado `server.js` y modifique el mensaje de saludo del servicio: 

    ```javascript
    res.send('Hello from webfrontend running in Azure!');
    ```

3. Guarde el archivo.
1. Ejecute `azds up` en la ventana de terminal. 

Este comando vuelve a crear la imagen del contenedor y vuelve a implementar el gráfico de Helm. Vuelva a cargar la página del explorador para ver que los cambios de código surten efecto.

Pero hay un método *más rápido* para desarrollar código, que se verá en la siguiente sección. 

## <a name="debug-a-container-in-kubernetes"></a>Depuración de un contenedor en Kubernetes

En esta sección, usará VS Code para depurar directamente el contenedor que se ejecuta en Azure. También aprenderá a obtener un bucle de edición-ejecución-prueba más rápido.

![](media/common/edit-refresh-see.png)

> [!Note]
> **Si se queda bloqueado** en cualquier momento, consulte la sección [Solución de problemas](troubleshooting.md) o publique un comentario en esta página.

### <a name="initialize-debug-assets-with-the-vs-code-extension"></a>Inicialización de activos de depuración con la extensión de VS Code
En primer lugar, debe configurar el proyecto de código para que VS Code se comunique con nuestro espacio de desarrollo en Azure. La extensión de VS Code para Azure Dev Spaces proporciona un comando auxiliar para establecer la configuración de depuración. 

Abra la **Paleta de comandos** (mediante el menú **Vista | Paleta de comandos**) y use Autocompletar para escribir y seleccionar este comando: `Azure Dev Spaces: Prepare configuration files for Azure Dev Spaces`. 

Esto agrega la configuración de depuración para los espacios de Azure Dev Spaces en la carpeta `.vscode`. Este comando no se debe confundir con el comando `azds prep`, que configura el proyecto para la implementación.

![](media/common/command-palette.png)

### <a name="select-the-azds-debug-configuration"></a>Selección de la configuración de depuración de AZDS
1. Para abrir la vista de depuración, haga clic en el icono de depuración en la **barra de actividad** en el lateral de VS Code.
1. Seleccione **Launch Program (AZDS)** [Iniciar programa (AZDS)] como la configuración de depuración activa.

![](media/get-started-node/debug-configuration-nodejs2.png)

> [!Note]
> Si no ve ningún comando de Azure Dev Spaces en la paleta de comandos, asegúrese de haber [instalado la extensión de Visual Studio Code para Azure Dev Spaces](get-started-nodejs.md#get-kubernetes-debugging-for-vs-code).

### <a name="debug-the-container-in-kubernetes"></a>Depuración del contenedor en Kubernetes
Presione **F5**  para depurar el código en Kubernetes.

De forma similar al comando `up`, el código se sincroniza con el entorno de desarrollo cuando comienza la depuración, y se crea un contenedor que se implementa en Kubernetes. En esta ocasión, el depurador se asocia al contenedor remoto.

> [!Tip]
> La barra de estado de VS Code se volverá de color naranja, lo que indica que el depurador se ha asociado. También mostrará una dirección URL interactiva, que puede usar para abrir rápidamente el sitio web.

![](media/common/vscode-status-bar-url.png)

Establezca un punto de interrupción en un archivo de código del lado servidor, por ejemplo, dentro de `app.get('/api'...` en [la línea 13 de `server.js`](https://github.com/Azure/dev-spaces/blob/master/samples/nodejs/getting-started/webfrontend/server.js#L13). 

    ```javascript
    app.get('/api', function (req, res) {
        res.send('Hello from webfrontend');
    });
    ```

Actualice la página del explorador o presione el botón *Say It Again* (Repetir); llegará al punto de interrupción y podrá ejecutar el código paso a paso.

Tiene acceso completo a la información de depuración, igual que si el código se ejecutara localmente, como la pila de llamadas, las variables locales o la información de excepción, por ejemplo.

### <a name="edit-code-and-refresh-the-debug-session"></a>Edición del código y actualización de la sesión de depuración
Con el depurador activo, realice una edición de código; por ejemplo, vuelva a modificar el mensaje de saludo en [la línea 13 de `server.js`](https://github.com/Azure/dev-spaces/blob/master/samples/nodejs/getting-started/webfrontend/server.js#L13):

```javascript
app.get('/api', function (req, res) {
    res.send('**** Hello from webfrontend running in Azure! ****');
});
```

Guarde el archivo y, en el **panel de acciones de depuración**, haga clic en el botón **Reiniciar**. 

![](media/common/debug-action-refresh.png)

En lugar de volver a crear e implementar una nueva imagen de contenedor cada vez que se realizan ediciones en el código, lo que a menudo lleva un tiempo considerable, Azure Dev Spaces reinicia el proceso Node.js entre sesiones de depuración para proporcionar un bucle de edición/depuración más rápido.

Actualice la aplicación web en el explorador o presione el botón *Say It Again* (Repetir). Verá que aparece el mensaje personalizado en la interfaz de usuario.

### <a name="use-nodemon-to-develop-even-faster"></a>Uso de NodeMon para acelerar el desarrollo
*Nodemon* es una conocida herramienta que usan los desarrolladores de Node.js para el desarrollo rápido. En lugar de reiniciar manualmente el proceso de Node cada vez que se realiza una edición del código en el lado servidor, los desarrolladores a menudo configuran su proyecto de Node para que *nodemon* supervise los cambios en el archivo y reinicie automáticamente el proceso del servidor. En este estilo de trabajo, el desarrollador solo actualiza el explorador después de realizar una edición del código.

Con Azure Dev Spaces, puede utilizar muchos de los mismos flujos de trabajo de desarrollo que emplea con el desarrollo local. Para ilustrar esto, el proyecto `webfrontend` de ejemplo se ha configurado para usar *nodemon* (está configurado como una dependencia de desarrollo en `package.json`).

Realice estos pasos:
1. Detenga el depurador de VS Code.
1. Haga clic en el icono de depuración en la **barra de actividad** en el lateral de VS Code. 
1. Seleccione **Attach (AZDS)** [Asociar (AZDS)] como la configuración de depuración activa.
1. Presione F5.

En esta configuración, el contenedor está configurado para iniciar *nodemon*. Cuando se realizan ediciones del código de servidor, *nodemon* reinicia automáticamente el proceso de Node, tal y como lo hace en el desarrollo local. 
1. Edite el mensaje de saludo de nuevo en `server.js` y guarde el archivo.
1. Actualice el explorador o haga clic en el botón *Say It Again* (Repetir) para que los cambios surtan efecto.

**Ahora tiene un método para iterar rápidamente el código y depurarlo directamente en Kubernetes.** A continuación, verá cómo puede crear y llamar a un segundo contenedor.

## <a name="next-steps"></a>Pasos siguientes

> [!div class="nextstepaction"]
> [Aprenda sobre el desarrollo de varios servicios](multi-service-nodejs.md)


[supported-regions]: https://azure.microsoft.com/global-infrastructure/services/?products=kubernetes-service
