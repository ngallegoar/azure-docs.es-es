---
title: Uso de Proceso local con Kubernetes con Visual Studio (versión preliminar)
services: azure-dev-spaces
ms.date: 06/02/2020
ms.topic: conceptual
description: Obtenga información sobre cómo usar Proceso local con Kubernetes con Visual Studio para conectar el equipo de desarrollo a un clúster de Kubernetes con Azure Dev Spaces.
keywords: Proceso local con Kubernetes, Azure Dev Spaces, Dev Spaces, Docker, Kubernetes, Azure, AKS, Azure Kubernetes Service, contenedores
ms.openlocfilehash: 8e89baeac8d9042a87685ed1268ed694db8d9455
ms.sourcegitcommit: 69156ae3c1e22cc570dda7f7234145c8226cc162
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 06/03/2020
ms.locfileid: "84316556"
---
# <a name="use-local-process-with-kubernetes-with-visual-studio-preview"></a>Uso de Proceso local con Kubernetes con Visual Studio (versión preliminar)

Proceso local con Kubernetes le permite ejecutar y depurar el código en el equipo de desarrollo mientras sigue conectado al clúster de Kubernetes con el resto de aplicaciones o servicios. Por ejemplo, si tiene una arquitectura de microservicios de gran tamaño con muchos servicios y bases de datos interdependientes, la replicación de esas dependencias en el equipo de desarrollo puede resultar difícil. Además, el proceso de compilación e implementación del código en el clúster de Kubernetes para cada cambio de código durante el desarrollo del bucle interno puede resultar lento y difícil de usar con un depurador, además de llevarle mucho tiempo.

Proceso local con Kubernetes evita tener que compilar e implementar el código en el clúster creando en su lugar una conexión directa entre el equipo de desarrollo y el clúster. La conexión del equipo de desarrollo al clúster durante la depuración le permite probar y desarrollar rápidamente el servicio en el contexto de la aplicación completa sin necesidad de crear ninguna configuración de Docker o Kubernetes.

Proceso local con Kubernetes redirige el tráfico entre el clúster de Kubernetes conectado y el equipo de desarrollo. Esta redirección del tráfico permite que el código del equipo de desarrollo y los servicios que se ejecutan en el clúster de Kubernetes se comuniquen como si estuvieran en el mismo clúster de Kubernetes. Proceso local con Kubernetes también proporciona una manera de replicar variables de entorno y volúmenes montados disponibles para pods en el clúster de Kubernetes en el equipo de desarrollo. El hecho de proporcionar acceso a las variables de entorno y a los volúmenes montados en el equipo de desarrollo le permite trabajar rápidamente en el código sin tener que replicar dichas dependencias manualmente.

En esta guía, aprenderá a usar Proceso local con Kubernetes para redirigir el tráfico entre el clúster de Kubernetes y el código que se ejecuta en el equipo de desarrollo. También se proporciona un script para implementar una aplicación de ejemplo de gran tamaño con varios microservicios en un clúster de Kubernetes.

> [!IMPORTANT]
> Esta funcionalidad actualmente está en su versión preliminar. Las versiones preliminares están a su disposición con la condición de que acepte los [términos de uso adicionales][preview-terms]. Es posible que algunos de los aspectos de esta característica cambien antes de ofrecer disponibilidad general.

## <a name="before-you-begin"></a>Antes de empezar

En esta guía se usa la [aplicación de ejemplo Bike Sharing de Azure Dev Spaces][bike-sharing-github] para demostrar la conexión del equipo de desarrollo a un clúster de Kubernetes. Si ya tiene una aplicación que se ejecuta en un clúster de Kubernetes, puede seguir los pasos que se indican a continuación y usar los nombres de sus servicios.

### <a name="prerequisites"></a>Requisitos previos

* Suscripción a Azure. Si no tiene una suscripción a Azure, puede crear una [cuenta gratuita](https://azure.microsoft.com/free).
* [La CLI de Azure instalada][azure-cli].
* Versión preliminar 2 16.7 de [Visual Studio 2019][visual-studio] o una posterior que se ejecute en Windows 10 con las cargas de trabajo de *ASP.NET y desarrollo web* y *desarrollo de Azure* instaladas.
* Tener la [CLI de Azure Dev Spaces instalada][azds-cli].

### <a name="enable-the-local-process-with-kubernetes-preview-feature-in-visual-studio"></a>Habilitación de la característica en versión preliminar Proceso local con Kubernetes en Visual Studio

Para habilitar Proceso local con Kubernetes en Visual Studio, haga clic en *Herramientas* > *Opciones* > *Entorno* > *Características en versión preliminar*. Seleccione *Habilitar la depuración local para los servicios de Kubernetes*.

Además, para las aplicaciones de consola .NET, instale paquete NuGet *Microsoft.VisualStudio.Azure.Kubernetes.Tools.Targets*.

## <a name="create-a-kubernetes-cluster"></a>Creación de un clúster de Kubernetes

Cree un clúster de AKS en una [región admitida][supported-regions]. Los siguientes comandos permiten crear un grupo de recursos llamado *MyResourceGroup* y un clúster de AKS denominado *MyAKS*.

```azurecli-interactive
az group create \
    --name MyResourceGroup \
    --location eastus

az aks create \
    --resource-group MyResourceGroup \
    --name MyAKS \
    --location eastus \
    --node-count 3 \
    --generate-ssh-keys
```

## <a name="install-the-sample-application"></a>Instale la aplicación de ejemplo.

Instale la aplicación de ejemplo en el clúster con el script proporcionado. Puede ejecutar este script en el equipo de desarrollo o mediante [Azure Cloud Shell][azure-cloud-shell].

```azurecli-interactive
git clone https://github.com/Azure/dev-spaces
cd dev-spaces/
chmod +x ./local-process-quickstart.sh
./local-process-quickstart.sh -g MyResourceGroup -n MyAKS
```

Navegue hasta la aplicación de ejemplo en la que se ejecuta el clúster. Para ello, abra su URL pública, que se muestra en la salida del script de instalación.

```console
$ ./local-process-quickstart.sh -g MyResourceGroup -n MyAKS
Defaulting Dev spaces repository root to current directory : ~/dev-spaces
Setting the Kube context
...
To try out the app, open the url:
dev.bikesharingweb.EXTERNAL_IP.nip.io
```

En el ejemplo anterior, la URL pública es `dev.bikesharingweb.EXTERNAL_IP.nip.io`.

## <a name="connect-to-your-cluster-and-debug-a-service"></a>Conexión al clúster y depuración de un servicio

En el equipo de desarrollo, descargue y configure la CLI de Kubernetes para conectarse al clúster de Kubernetes mediante [az aks get-credentials][az-aks-get-credentials].

```azurecli
az aks get-credentials --resource-group MyResourceGroup --name MyAKS
```

Abra*dev-spaces/samples/BikeSharingApp/ReservationEngine/app.csproj* en la [aplicación de ejemplo Bike Sharing de Azure Dev Spaces][bike-sharing-github] en Visual Studio.

En el proyecto, seleccione *Proceso local con Kubernetes* en el menú desplegable de configuración de inicio, como se muestra a continuación.

![Elegir Proceso local con Kubernetes](../media/local-process-kubernetes-visual-studio/choose-local-process.png)

Haga clic en el botón Inicio junto a *Proceso local con Kubernetes*. En el cuadro de diálogo *Proceso local con Kubernetes*, haga lo siguiente:

* Seleccione su suscripción.
* Seleccione *MyAKS* para el clúster.
* Seleccione *dev* para el espacio de nombres.
* Seleccione *reservationengine* para el servicio que se va a redirigir.
* Seleccione *app* para el perfil de inicio.
* Seleccione `http://dev.bikesharingweb.EXTERNAL_IP.nip.io` para la dirección URL que debe iniciar el explorador.

![Elegir el clúster de Proceso local con Kubernetes](../media/local-process-kubernetes-visual-studio/choose-local-process-cluster.png)

> [!IMPORTANT]
> Solo puede redirigir servicios que tengan un único pod.

Haga clic en *Guardar e iniciar la depuración*.

Todo el tráfico del clúster de Kubernetes se redirige para el servicio *reservationengine* a la versión de la aplicación que se ejecuta en el equipo de desarrollo. Proceso local con Kubernetes también enruta todo el tráfico saliente desde la aplicación al clúster de Kubernetes.

> [!NOTE]
> Se le pedirá que permita que *KubernetesDNSManager* se ejecute con privilegios elevados y modifique el archivo de hosts.

El equipo de desarrollo está conectado cuando la barra de estado muestra que está conectado en el servicio *reservationengine*.

![Equipo de desarrollo conectado](../media/local-process-kubernetes-visual-studio/development-computer-connected.png)

> [!NOTE]
> En los inicios posteriores, no se le mostrará el cuadro de diálogo *Proceso local con Kubernetes*. Esta configuración se actualiza en el panel *Depurar* de las propiedades del proyecto.

Una vez que el equipo de desarrollo está conectado, el tráfico comienza a redirigirse a dicho equipo para el servicio que está reemplazando.

## <a name="set-a-break-point"></a>Establecer un punto de interrupción

Abra [BikesHelper.cs][bikeshelper-cs-breakpoint] y haga clic en cualquier lugar de la línea 26 para colocar ahí el cursor. Para establecer un punto de interrupción, pulse *F9* o haga clic en *Depurar* y en *Alternar punto de interrupción*.

Abra la dirección URL pública para navegar a la aplicación de ejemplo. Seleccione *Aurelia Briggs (customer)* (Aurelia Briggs [cliente]) como usuario y, después, seleccione una bicicleta para alquilar. Haga clic en *Rent Bike* (Alquilar bicicleta). Vuelva a Visual Studio y observe que la línea 26 está resaltada. El punto de interrupción que estableció puso en pausa el servicio en la línea 26. Para reanudar el servicio, pulse *F5* o haga clic en *Depurar* y en *Continuar*. Vuelva al explorador y verifique que la página muestra que ha alquilado la bicicleta.

Quite el punto de interrupción colocando el cursor en la línea 26 en `BikesHelper.cs` y pulsando *F9*.

> [!NOTE]
> De forma predeterminada, al detener la tarea de depuración también se desconecta el equipo de desarrollo del clúster de Kubernetes. Para cambiar este comportamiento, puede cambiar el valor *Disconnect after debugging* (Desconectarse después de la depuración) a *false* en la sección *Kubernetes Debugging Tools* (Herramientas de depuración de Kubernetes) de las opciones de depuración. Después de actualizar esta configuración, el equipo de desarrollo permanecerá conectado cuando detenga e inicie la depuración. Para desconectar el equipo de desarrollo del clúster, haga clic en el botón *Desconectar* de la barra de herramientas.
>
> Si Visual Studio finaliza de forma repentina la conexión al clúster o se finaliza, es posible que el servicio que está redirigiendo no se restaure a su estado original antes de que establezca conexión con Proceso local con Kubernetes. Para solucionar este problema, consulte la [guía de solución de problemas][troubleshooting].

## <a name="using-logging-and-diagnostics"></a>Uso de registro y diagnóstico

Puede encontrar los registros de diagnóstico en el directorio `Azure Dev Spaces` del [directorio *TEMP* del equipo de desarrollo][azds-tmp-dir].

## <a name="remove-the-sample-application-from-your-cluster"></a>Eliminación de la aplicación de ejemplo del clúster

Use el script proporcionado para eliminar aplicación de ejemplo del clúster.

```azurecli-interactive
./local-process-quickstart.sh -c -g MyResourceGroup -n MyAKS
```

## <a name="next-steps"></a>Pasos siguientes

Aprenda a usar las acciones de GitHub y Azure Dev Spaces para probar los cambios de una solicitud de incorporación de cambios directamente en AKS antes de que dicha solicitud se combine en la rama principal del repositorio.

> [!div class="nextstepaction"]
> [Acciones de GitHub y Azure Kubernetes Service][gh-actions]

[azds-cli]: install-dev-spaces.md#install-the-client-side-tools
[azds-tmp-dir]: ../troubleshooting.md#before-you-begin
[azds-vs-code]: https://marketplace.visualstudio.com/items?itemName=azuredevspaces.azds
[azure-cli]: /cli/azure/install-azure-cli?view=azure-cli-latest
[azure-cloud-shell]: ../../cloud-shell/overview.md
[az-aks-get-credentials]: /cli/azure/aks?view=azure-cli-latest#az-aks-get-credentials
[az-aks-vs-code]: https://marketplace.visualstudio.com/items?itemName=ms-kubernetes-tools.vscode-aks-tools
[bike-sharing-github]: https://github.com/Azure/dev-spaces/tree/master/samples/BikeSharingApp
[gh-actions]: github-actions.md
[preview-terms]: https://azure.microsoft.com/support/legal/preview-supplemental-terms/
[bikeshelper-cs-breakpoint]: https://github.com/Azure/dev-spaces/blob/master/samples/BikeSharingApp/ReservationEngine/BikesHelper.cs#L26
[supported-regions]: https://azure.microsoft.com/global-infrastructure/services/?products=kubernetes-service
[troubleshooting]: ../troubleshooting.md#fail-to-restore-original-configuration-of-deployment-on-cluster
[visual-studio]: https://www.visualstudio.com/vs/