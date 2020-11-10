---
title: 'Inicio rápido: Implementación de una aplicación "Hola mundo" en Azure Service Fabric Mesh'
description: En este inicio rápido se muestra cómo implementar una aplicación de Service Fabric Mesh en Azure Service Fabric Mesh.
author: georgewallace
ms.author: gwallace
ms.date: 11/27/2018
ms.topic: quickstart
ms.openlocfilehash: 0c6bb8ba680612acbd497598b3e56efa5c28244a
ms.sourcegitcommit: 4b76c284eb3d2b81b103430371a10abb912a83f4
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 11/01/2020
ms.locfileid: "93146252"
---
# <a name="quickstart-deploy-hello-world-to-service-fabric-mesh"></a>Inicio rápido: Implementación de una aplicación "Hola mundo" en Service Fabric Mesh

[Service Fabric Mesh](service-fabric-mesh-overview.md) facilita la creación y administración de aplicaciones de microservicio en Azure, sin tener que aprovisionar máquinas virtuales. En este inicio rápido, creará una aplicación "Hola mundo" en Azure y la expondrá en Internet. Esta operación se completa en un solo comando. Dentro de un par de minutos, podrá ver esta vista en el explorador:

![Aplicación Hola mundo en el explorador][sfm-app-browser]

[!INCLUDE [preview note](./includes/include-preview-note.md)]

Antes de empezar, si no tiene una cuenta de Azure, [cree una gratuita](https://azure.microsoft.com/free/).

## <a name="set-up-service-fabric-mesh-cli"></a>Configuración de la CLI de Service Fabric Mesh 
Puede usar Azure Cloud Shell o una instalación local de la CLI de Azure para completar esta guía de inicio rápido. Instale el módulo de extensión de la CLI de Azure Service Fabric Mesh siguiendo estas [instrucciones](service-fabric-mesh-howto-setup-cli.md).

## <a name="sign-in-to-azure"></a>Inicio de sesión en Azure
Inicie sesión en Azure y configure la suscripción.

```azurecli-interactive
az login
az account set --subscription "<subscriptionID>"
```

## <a name="create-resource-group"></a>Creación de un grupo de recursos
Cree un grupo de recursos en el que implementar la aplicación. Puede usar un grupo de recursos existente y omitir este paso. 

```azurecli-interactive
az group create --name myResourceGroup --location eastus 
```

## <a name="deploy-the-application"></a>Implementación de la aplicación

>[!NOTE]
> Desde el 2 de noviembre de 2020, [se aplican límites de frecuencia de descarga](https://docs.docker.com/docker-hub/download-rate-limit/) a las solicitudes anónimas y autenticadas a Docker Hub desde las cuentas del plan gratuito de Docker y se aplican por la dirección IP. 
> 
> Estas plantillas utilizan imágenes públicas de Docker Hub. Tenga en cuenta que puede tener una limitación de frecuencia. Para más información, consulte [Autenticación con Docker Hub](https://docs.microsoft.com/azure/container-registry/buffer-gate-public-content#authenticate-with-docker-hub).

Cree su aplicación en el grupo de recursos mediante el comando `az mesh deployment create`.  Ejecute lo siguiente:

```azurecli-interactive
az mesh deployment create --resource-group myResourceGroup --template-uri https://raw.githubusercontent.com/Azure-Samples/service-fabric-mesh/master/templates/helloworld/helloworld.linux.json --parameters "{'location': {'value': 'eastus'}}" 
```

El comando anterior implementa una aplicación Linux con la [plantilla linux.json](https://raw.githubusercontent.com/Azure-Samples/service-fabric-mesh/master/templates/helloworld/helloworld.linux.json). Si quiere implementar una aplicación Windows, use la [plantilla windows.json](https://raw.githubusercontent.com/Azure-Samples/service-fabric-mesh/master/templates/helloworld/helloworld.windows.json). Las imágenes de contenedor de Windows son más grandes que las imágenes de contenedor de Linux y pueden tardar más en implementarse.

Este comando producirá un fragmento de código de JSON que se muestra a continuación. En la sección ```outputs``` de la salida de JSON, copie la propiedad ```publicIPAddress```.

```json
"outputs": {
    "publicIPAddress": {
    "type": "String",
    "value": "40.83.78.216"
    }
}
```

Esta información proviene de la sección ```outputs``` de la plantilla ARM. Como se muestra a continuación, esta sección hace referencia al recurso de la puerta de enlace para capturar la dirección IP pública. 

```json
  "outputs": {
    "publicIPAddress": {
      "value": "[reference('helloWorldGateway').ipAddress]",
      "type": "string"
    }
  }
```

## <a name="open-the-application"></a>Abrir la aplicación
Una vez que la aplicación se ha implementado correctamente, copie la dirección IP pública del punto de conexión de servicio de la salida de la CLI. Abra la dirección IP en un explorador web. Se muestra una página web con el logotipo del servicio Azure Service Fabric Mesh.

## <a name="check-the-application-details"></a>Comprobación de los detalles de la aplicación
Puede comprobar el estado de la aplicación mediante el comando `az mesh app show`. Este comando proporciona información útil que puede seguir.

El nombre de la aplicación en este inicio rápido es `helloWorldApp`; para recopilar los detalles sobre la aplicación, ejecute el siguiente comando:

```azurecli-interactive
az mesh app show --resource-group myResourceGroup --name helloWorldApp
```

## <a name="see-the-application-logs"></a>Consulta de los registros de aplicaciones

Examine los registros de la aplicación implementada mediante el comando `az mesh code-package-log get`:

```azurecli-interactive
az mesh code-package-log get --resource-group myResourceGroup --application-name helloWorldApp --service-name helloWorldService --replica-name 0 --code-package-name helloWorldCode
```

## <a name="clean-up-resources"></a>Limpieza de recursos

Cuando esté listo para eliminar la aplicación, ejecute el comando [az group delete][az-group-delete] para quitar el grupo de recursos y la aplicación y los recursos de red que contiene.

```azurecli-interactive
az group delete --name myResourceGroup
```

## <a name="next-steps"></a>Pasos siguientes

Para obtener más información sobre cómo crear e implementar aplicaciones de Service Fabric mesh, continúe con el tutorial.
> [!div class="nextstepaction"]
> [Creación e implementación de una aplicación web multiservicio](service-fabric-mesh-tutorial-create-dotnetcore.md)

<!-- Images -->
[sfm-app-browser]: ./media/service-fabric-mesh-quickstart-deploy-container/HelloWorld.png

<!-- Links / Internal -->
[az-group-delete]: /cli/azure/group
[azure-cli-install]: /cli/azure/install-azure-cli?view=azure-cli-latest
