---
title: 'Tutorial: Escalado de una aplicación que se ejecuta en Azure Service Fabric Mesh'
description: En este tutorial, aprenderá a escalar los servicios en una aplicación que se ejecuta en Service Fabric Mesh.
author: georgewallace
ms.topic: tutorial
ms.date: 01/11/2019
ms.author: gwallace
ms.custom: mvc, devcenter
ms.openlocfilehash: aad0da53dc667039a62d2115c1afe4e2c6662bff
ms.sourcegitcommit: 829d951d5c90442a38012daaf77e86046018e5b9
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/09/2020
ms.locfileid: "91840173"
---
# <a name="tutorial-scale-an-application-running-in-service-fabric-mesh"></a>Tutorial: Escala de una aplicación que se ejecuta en Service Fabric Mesh

Este tutorial es la segunda parte de una serie. Aprenda a escalar manualmente el número de instancias de servicio de una aplicación que se [implementó previamente en Service Fabric Mesh](service-fabric-mesh-tutorial-template-deploy-app.md). Cuando haya terminado, tendrá un servicio front-end que ejecuta tres instancias y un servicio de datos que ejecuta dos.

En la segunda parte de la serie, se aprende a:

> [!div class="checklist"]
> * Configurar el número deseado de instancias de servicio
> * Realizar una actualización

En esta serie de tutoriales, se aprende a:
> [!div class="checklist"]
> * [Implementar una aplicación en Service Fabric Mesh mediante una plantilla](service-fabric-mesh-tutorial-template-deploy-app.md)
> * Escala de una aplicación que se ejecuta en Service Fabric Mesh
> * [Actualizar una aplicación que se ejecuta en Service Fabric Mesh](service-fabric-mesh-tutorial-template-upgrade-app.md)
> * [Eliminar una aplicación](service-fabric-mesh-tutorial-template-remove-app.md)

[!INCLUDE [preview note](./includes/include-preview-note.md)]

## <a name="prerequisites"></a>Prerrequisitos

Antes de empezar este tutorial:

* Si no tiene una suscripción a Azure, puede [crear una cuenta gratuita](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) antes de empezar.

* [Instale localmente la CLI de Azure y la CLI de Service Fabric Mesh](service-fabric-mesh-howto-setup-cli.md#install-the-azure-service-fabric-mesh-cli).

## <a name="manually-scale-your-services-in-or-out"></a>Escalar los servicios vertical u horizontalmente de forma manual

Una de las principales ventajas de implementar aplicaciones en Service Fabric Mesh es la capacidad de escalar vertical u horizontalmente los servicios de un modo sencillo. Esto se debe usar para controlar diferentes cantidades de carga en los servicios o bien para mejorar la disponibilidad.

Este tutorial usa el ejemplo To Do List, que se [implementó previamente](service-fabric-mesh-tutorial-template-deploy-app.md) y ahora debe estar en ejecución. La aplicación tiene dos servicios: WebFrontEnd y ToDoService. Cada servicio se implementó inicialmente con un número de réplicas de 1.  Para ver el número de réplicas en ejecución para el servicio WebFrontEnd, ejecute lo siguiente:

```azurecli
az mesh service show --resource-group myResourceGroup --name WebFrontEnd --app-name todolistapp --query "replicaCount"
```

Para ver el número de réplicas en ejecución para el servicio ToDoService, ejecute lo siguiente:

```azurecli
az mesh service show --resource-group myResourceGroup --name ToDoService --app-name todolistapp --query "replicaCount"
```

En la plantilla de implementación para el recurso de aplicación, cada servicio tiene una propiedad *replicaCount* que puede usarse para establecer el número de veces que desea que ese servicio se implemente. Una aplicación puede constar de varios servicios, cada uno con un único número de *replicaCount*, que se implementan y administran de forma conjunta. Para escalar el número de réplicas de servicio, modifique el valor de *replicaCount* para cada servicio que desee escalar en el archivo de parámetros o la plantilla de implementación.  Después, actualice la aplicación.

### <a name="modify-the-deployment-template-parameters"></a>Modificación de los parámetros de la plantilla de implementación

Cuando haya valores en la plantilla que prevea cambiar una vez implementada la aplicación o si le gustaría tener la opción de cambiar a una base según implementación (si tiene previsto volver a usar esta plantilla para otras implementaciones), el procedimiento recomendado es parametrizar los valores.

Antes, la aplicación se implementó con los archivos de la [plantilla de implementación mesh_rp.windows.json](https://github.com/Azure-Samples/service-fabric-mesh/blob/master/templates/todolist/mesh_rp.windows.json) y [ de los parámetros mesh_rp.windows.parameter.json](https://github.com/Azure-Samples/service-fabric-mesh/blob/master/templates/todolist/mesh_rp.windows.parameters.json).

Abra el archivo [de parámetros mesh_rp.windows.parameter.json parámetros](https://github.com/Azure-Samples/service-fabric-mesh/blob/master/templates/todolist/mesh_rp.windows.parameters.json) localmente y establezca el valor de *frontEndReplicaCount* en 3 y el valor de *serviceReplicaCount* en 2:

```json
      "frontEndReplicaCount":{
        "value": "3"
      },
      "serviceReplicaCount":{
        "value": "2"
      }
```

Guarde los cambios del archivo de parámetros.  Los parámetros *frontEndReplicaCount* y *serviceReplicaCount* se declaran en la sección *parameters* de la [plantilla de implementación mesh_rp.windows.json](https://github.com/Azure-Samples/service-fabric-mesh/blob/master/templates/todolist/mesh_rp.windows.json):

```json
"frontEndReplicaCount":{
      "defaultValue": "1",
      "type": "string"
    },
    "serviceReplicaCount":{
      "defaultValue": "1",
      "type": "string"
    }
```

La propiedad *replicaCount* del servicio WebFrontEnd hace referencia al parámetro *frontEndReplicaCount* y la propiedad *replicaCount* del servicio ToDoService hace referencia al parámetro *serviceReplicaCount*:

```json
    "services": [
    {
    "name": "WebFrontEnd",
    "properties": {
        "description": "WebFrontEnd description.",
        "osType": "Windows",
        "codePackages": [
        {
            ...
        }
        ],
        "replicaCount": "[parameters('frontEndReplicaCount')]",
        "networkRefs": [
        {
            "name": "[resourceId('Microsoft.ServiceFabricMesh/networks', 'todolistappNetwork')]"
        }
        ]
    }
    },
    {
    "name": "ToDoService",
    "properties": {
        "description": "ToDoService description.",
        "osType": "Windows",
        "codePackages": [
        {
            ...
        }
        ],
        "replicaCount": "[parameters('serviceReplicaCount')]",
        "networkRefs": [
        {
            "name": "[resourceId('Microsoft.ServiceFabricMesh/networks', 'todolistappNetwork')]"
        }
        ]
    }
    }
],
```

Una vez que se ha modificado la plantilla, actualice la aplicación.

### <a name="upgrade-your-application"></a>Actualizar la aplicación

Mientras se ejecuta la aplicación, puede actualizarla si vuelve a implementar la plantilla y el archivo de parámetros actualizado:

```azurecli
az mesh deployment create --resource-group myResourceGroup --template-file c:\temp\mesh_rp.windows.json --parameters c:\temp\mesh_rp.windows.parameters.json
```

De este modo, se iniciará una actualización gradual de la aplicación y debería ver un aumento en las instancias de los servicios en unos minutos.  Para ver el número de réplicas en ejecución para el servicio WebFrontEnd, ejecute lo siguiente:

```azurecli
az mesh service show --resource-group myResourceGroup --name WebFrontEnd --app-name todolistapp --query "replicaCount"
```

Para ver el número de réplicas en ejecución para el servicio ToDoService, ejecute lo siguiente:

```azurecli
az mesh service show --resource-group myResourceGroup --name ToDoService --app-name todolistapp --query "replicaCount"
```

## <a name="next-steps"></a>Pasos siguientes

En esta parte del tutorial, ha aprendido a:

> [!div class="checklist"]
> * Configurar el número deseado de instancias de servicio
> * Realizar una actualización

Avance hasta el siguiente tutorial:
> [!div class="nextstepaction"]
> [Actualizar una aplicación que se ejecuta en Service Fabric Mesh](service-fabric-mesh-tutorial-template-upgrade-app.md)
