---
title: 'Tutorial: Actualización de una aplicación que se ejecuta en Azure Service Fabric Mesh'
description: En este tutorial, aprenderá a actualizar una aplicación de Service Fabric que se ejecuta en Service Fabric Mesh.
author: georgewallace
ms.topic: tutorial
ms.date: 01/11/2019
ms.author: gwallace
ms.custom: mvc, devcenter
ms.openlocfilehash: eea00fe80b71f97ea280a3a76de5012175bcd61c
ms.sourcegitcommit: 829d951d5c90442a38012daaf77e86046018e5b9
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/09/2020
ms.locfileid: "91841992"
---
# <a name="tutorial-upgrade-a-service-fabric-application-running-in-service-fabric-mesh"></a>Tutorial: Actualización de una aplicación de Service Fabric que se ejecuta en Azure Service Fabric Mesh

Este tutorial es la tercera parte de una serie. Obtendrá información sobre cómo actualizar una aplicación de Service Fabric que estaba [implementada previamente en Service Fabric Mesh](service-fabric-mesh-tutorial-template-deploy-app.md) mediante el aumento de los recursos de CPU asignados.  Cuando haya terminado, tendrá un servicio de front-end web que se ejecuta con recursos de CPU superiores.

En la tercera parte de la serie, se aprende a:

> [!div class="checklist"]
> * Cambiar las configuraciones de aplicación
> * Actualizar una aplicación que se ejecuta en Service Fabric Mesh

En esta serie de tutoriales, se aprende a:
> [!div class="checklist"]
> * [Implementar una aplicación en Service Fabric Mesh mediante una plantilla](service-fabric-mesh-tutorial-template-deploy-app.md)
> * [Escalar una aplicación que se ejecuta en Service Fabric Mesh](service-fabric-mesh-tutorial-template-scale-services.md)
> * Actualizar una aplicación que se ejecuta en Service Fabric Mesh
> * [Eliminar una aplicación](service-fabric-mesh-tutorial-template-remove-app.md)

[!INCLUDE [preview note](./includes/include-preview-note.md)]

## <a name="prerequisites"></a>Prerequisites

Antes de empezar este tutorial:

* Si no tiene una suscripción a Azure, puede [crear una cuenta gratuita](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) antes de empezar.

* Abra [Azure Cloud Shell](service-fabric-mesh-howto-setup-cli.md) o [instale la CLI de Azure y la CLI de Service Fabric Mesh localmente](service-fabric-mesh-howto-setup-cli.md#install-the-azure-service-fabric-mesh-cli).

## <a name="upgrade-application-configurations"></a>Actualización de las configuraciones de aplicación

Una de las principales ventajas de implementar aplicaciones en Service Fabric Mesh es la capacidad de actualizar la configuración de aplicación de un modo sencillo.  Por ejemplo, los recursos de CPU o memoria de los servicios.

Este tutorial usa el ejemplo To Do List, que se [implementó previamente](service-fabric-mesh-tutorial-template-deploy-app.md) y ahora debe estar en ejecución. La aplicación tiene dos servicios: WebFrontEnd y ToDoService. Cada servicio se implementó inicialmente con 0,5 recursos de CPU.  Para ver los recursos de CPU del servicio WebFrontEnd, ejecute lo siguiente:

```azurecli
az mesh service show --resource-group myResourceGroup --name WebFrontEnd --app-name todolistapp
```

En la plantilla de implementación del recurso de aplicación, cada servicio tiene una propiedad *cpu* que se puede usar para establecer los recursos de CPU solicitados. Una aplicación puede constar de varios servicios, cada uno con un único valor de *cpu*, que se implementan y administran de forma conjunta. Para aumentar los recursos de CPU del servicio de front-end web, modifique el valor de *cpu* en la plantilla de implementación o el archivo de parámetros.  Después, actualice la aplicación.

### <a name="modify-the-deployment-template-parameters"></a>Modificación de los parámetros de la plantilla de implementación

Cuando haya valores en la plantilla que prevea cambiar una vez implementada la aplicación o si le gustaría tener la opción de cambiar a una base según implementación (si tiene previsto volver a usar esta plantilla para otras implementaciones), el procedimiento recomendado es parametrizar los valores.

Antes, la aplicación se implementó con los archivos de la [plantilla de implementación mesh_rp.windows.json](https://github.com/Azure-Samples/service-fabric-mesh/blob/master/templates/todolist/mesh_rp.windows.json) y [ de los parámetros mesh_rp.windows.parameter.json](https://github.com/Azure-Samples/service-fabric-mesh/blob/master/templates/todolist/mesh_rp.windows.parameters.json).

Abra el archivo de [parámetros mesh_rp.windows.parameter.json](https://github.com/Azure-Samples/service-fabric-mesh/blob/master/templates/todolist/mesh_rp.windows.parameters.json) localmente y establezca el valor de *frontEndCpu* en 1:

```json
      "frontEndCpu":{
        "value": "1"
      }
```

Guarde los cambios del archivo de parámetros.  

El parámetro *frontEndCpu* se declara en la sección *parameters* de la [plantilla de implementación mesh_rp.windows.json](https://github.com/Azure-Samples/service-fabric-mesh/blob/master/templates/todolist/mesh_rp.windows.json):

```json
"frontEndCpu": {
    "defaultValue": "0.5",
    "type": "string",
    "metadata": {
        "description": "The CPU resources for the front end web service."
    }
}
```

La propiedad *codePackages->resources->requests->cpu* del servicio WebFrontEnd hace referencia al parámetro *frontEndCpu*:

```json
    "services": [
          {
            "name": "WebFrontEnd",
            "properties": {
              "description": "WebFrontEnd description.",
              "osType": "Windows",
              "codePackages": [
                {
                  "name": "WebFrontEnd",
                  "image": "[parameters('frontEndImage')]",
                  ...
                  "resources": {
                    "requests": {
                      "cpu": "[parameters('frontEndCpu')]",
                      "memoryInGB": "[parameters('frontEndMemory')]"
                    }
                  },
                  "imageRegistryCredential": {
                    "server": "[parameters('registryServer')]",
                    "username": "[parameters('registryUserName')]",
                    "password": "[parameters('registryPassword')]"
                  }
                }
              ],
              ...
```

### <a name="upgrade-your-application"></a>Actualizar la aplicación

Mientras se ejecuta la aplicación, puede actualizarla si vuelve a implementar la plantilla y el archivo de parámetros actualizado:

```azurecli
az mesh deployment create --resource-group myResourceGroup --template-file c:\temp\mesh_rp.windows.json --parameters c:\temp\mesh_rp.windows.parameters.json
```

De este modo, se iniciará una actualización gradual de la aplicación y debería ver el aumento de los recursos de CPU en unos minutos.  Para ver los recursos de CPU del servicio WebFrontEnd, ejecute lo siguiente:

```azurecli
az mesh service show --resource-group myResourceGroup --name WebFrontEnd --app-name todolistapp
```

## <a name="next-steps"></a>Pasos siguientes

En esta parte del tutorial, ha aprendido a:

> [!div class="checklist"]
> * Cambiar las configuraciones de aplicación
> * Actualizar una aplicación que se ejecuta en Service Fabric Mesh

Avance hasta el siguiente tutorial:
> [!div class="nextstepaction"]
> [Eliminación de una aplicación de Service Fabric Mesh](service-fabric-mesh-tutorial-template-remove-app.md)
