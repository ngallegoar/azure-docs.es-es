---
title: 'Tutorial: Eliminación de una aplicación que se ejecuta en Azure Service Fabric Mesh'
description: En este tutorial, sabrá cómo quitar una aplicación que se ejecuta en Service Fabric Mesh y eliminar recursos.
author: dkkapur
ms.topic: tutorial
ms.date: 01/11/2019
ms.author: dekapur
ms.custom: mvc, devcenter
ms.openlocfilehash: 4780f81d23f0183837d2aafb9a8e5e2c41faa1cf
ms.sourcegitcommit: 0947111b263015136bca0e6ec5a8c570b3f700ff
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/24/2020
ms.locfileid: "75351795"
---
# <a name="tutorial-remove-an-application-and-resources"></a>Tutorial: Eliminación de una aplicación y recursos

Este tutorial es la cuarta parte de una serie. Obtendrá información sobre cómo quitar una aplicación en ejecución que [se implementó previamente en Service Fabric Mesh](service-fabric-mesh-tutorial-template-deploy-app.md). 

En la parte número cuatro de la serie, se aprende a:

> [!div class="checklist"]
> * Eliminar una aplicación que se ejecuta en Service Fabric Mesh
> * Eliminar los recursos de la aplicación

En esta serie de tutoriales, se aprende a:
> [!div class="checklist"]
> * [Implementar una aplicación en Service Fabric Mesh mediante una plantilla](service-fabric-mesh-tutorial-template-deploy-app.md)
> * [Escalar una aplicación que se ejecuta en Service Fabric Mesh](service-fabric-mesh-tutorial-template-scale-services.md)
> * [Actualizar una aplicación que se ejecuta en Service Fabric Mesh](service-fabric-mesh-tutorial-template-upgrade-app.md)
> * Eliminación de una aplicación

[!INCLUDE [preview note](./includes/include-preview-note.md)]

## <a name="prerequisites"></a>Prerequisites

Antes de empezar este tutorial:

* Si no tiene una suscripción a Azure, puede [crear una cuenta gratuita](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) antes de empezar.

* Abra [Azure Cloud Shell](service-fabric-mesh-howto-setup-cli.md) o [instale la CLI de Azure y la CLI de Service Fabric Mesh localmente](service-fabric-mesh-howto-setup-cli.md#install-the-azure-service-fabric-mesh-cli).

## <a name="delete-the-resource-group-and-all-the-resources"></a>Eliminación del grupo de recursos y todos los recursos

Cuando ya no los necesite, elimine todos los recursos que haya creado. Anteriormente, [creó un grupo de recursos](service-fabric-mesh-tutorial-template-deploy-app.md#create-a-container-registry) para hospedar la instancia de Azure Container Registry y los recursos de aplicación de Service Fabric Mesh.  Puede eliminar este grupo de recursos, lo que suprimirá todos los recursos asociados.

```azurecli
az group delete --resource-group myResourceGroup
```

```powershell
Remove-AzureRmResourceGroup -Name myResourceGroup
```

## <a name="individually-delete-the-resources"></a>Eliminación individual de los recursos
También puede eliminar individualmente la instancia de ACR, la aplicación Service Fabric Mesh y los recursos de red.

Para eliminar la instancia de ACR, siga estos pasos:

```azurecli
az acr delete --resource-group myResourceGroup --name myContainerRegistry
```

Para crear una aplicación Service Fabric Mesh, realice lo siguiente:

```azurecli
az mesh app delete --resource-group myResourceGroup --name todolistapp
```

Para eliminar la red, siga estos pasos:
```azurecli
az mesh network delete --resource-group myResourceGroup --name todolistappNetwork
```

## <a name="next-steps"></a>Pasos siguientes

En esta parte del tutorial, ha aprendido a:

> [!div class="checklist"]
> * Eliminar una aplicación que se ejecuta en Service Fabric Mesh
> * Eliminar los recursos de la aplicación