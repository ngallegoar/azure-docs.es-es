---
title: 'Tutorial: Creación de un área de trabajo de Azure ML con una plantilla de Azure Resource Manager'
description: En este tutorial, usará una plantilla de Azure Resource Manager para implementar rápidamente un área de trabajo de Azure para aprendizaje automático.
services: machine-learning
author: lobrien
ms.author: laobri
ms.custom: subject-armqs
ms.date: 05/26/2020
ms.service: machine-learning
ms.subservice: core
ms.topic: tutorial
ms.openlocfilehash: b6eac711ed6c420330b036cb6094c841508cee9f
ms.sourcegitcommit: a9784a3fd208f19c8814fe22da9e70fcf1da9c93
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 05/22/2020
ms.locfileid: "83790720"
---
# <a name="tutorial-deploy-an-azure-machine-learning-workspace-using-a-resource-manager-template"></a>Tutorial: Implementación de un área de trabajo de Azure Machine Learning mediante una plantilla de Azure Resource Manager
[!INCLUDE [applies-to-skus](../../includes/aml-applies-to-basic-enterprise-sku.md)]

En este tutorial aprenderá a crear un área de trabajo de Azure Machine Learning mediante una plantilla de Azure Resource Manager. Las áreas de trabajo de Azure Machine Learning organizan todos los activos de aprendizaje automático de los conjuntos de datos de base de referencia para los modelos implementados. Las áreas de trabajo son una ubicación única para colaborar con sus colegas en la creación, ejecución y revisión de experimentos, administrar el entrenamiento y la inferencia de recursos de proceso, así como supervisar y controlar la versión de los modelos implementados.

[!INCLUDE [About Azure Resource Manager](../../includes/resource-manager-quickstart-introduction.md)]

## <a name="prerequisites"></a>Prerrequisitos

* Suscripción a Azure. Si no tiene una suscripción a Azure, cree una [cuenta gratuita](https://aka.ms/AMLFree) antes de empezar.

* Para usar los comandos de la CLI de este documento desde su **entorno local**, necesita la [CLI de Azure](https://docs.microsoft.com/cli/azure/install-azure-cli?view=azure-cli-latest).

## <a name="create-a-workspace"></a>Crear un área de trabajo

### <a name="review-the-template"></a>Revisión de la plantilla

La plantilla usada en este inicio rápido forma parte de las [plantillas de inicio rápido de Azure](https://azure.microsoft.com/resources/templates/101-machine-learning-create/).

:::code language="json" source="~/quickstart-templates/101-machine-learning-create/azuredeploy.json":::

Los recursos siguientes se definen en la plantilla:

* [Microsoft.MachineLearningServices/workspaces](/azure/templates/microsoft.machinelearningservices/workspaces): Cree un área de trabajo de Aprendizaje automático de Azure. En esta plantilla, la ubicación y el nombre son parámetros que el usuario puede pasar o escribir interactivamente.

### <a name="deploy-the-template"></a>Implementación de la plantilla 

Para usar la plantilla desde la CLI de Azure, inicie sesión y elija su suscripción (consulte [Inicio de sesión con la CLI de Azure](https://docs.microsoft.com/cli/azure/authenticate-azure-cli?view=azure-cli-latest)). A continuación, ejecute:

```azurecli-interactive
read -p "Enter a project name that is used for generating resource names:" projectName &&
read -p "Enter the location (i.e. centralus):" location &&
templateUri="https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/101-machine-learning-create/azuredeploy.json" &&
resourceGroupName="${projectName}rg" &&
workspaceName="${projectName}ws" &&
az group create --name $resourceGroupName --location "$location" &&
az deployment group create --resource-group $resourceGroupName --template-uri $templateUri --parameters workspaceName=$workspaceName location=$location && 
echo "Press [ENTER] to continue ..." &&
read
```

Cuando ejecute el comando anterior, escriba:

1. Un nombre de proyecto que formará la base de los nombres del grupo de recursos creado y el área de trabajo de Azure Machine Learning.
1. La ubicación de Azure en la que desea crear la implementación

## <a name="review-deployed-resources"></a>Revisión de los recursos implementados

Para ver el área de trabajo de Azure ML:

1. Vaya a https://portal.azure.com. 
1. Iniciar sesión 
1. Elija el área de trabajo que acaba de crear.

Verá la página principal de Azure Machine Learning: 

:::image type="content" source="media/tutorial-resource-manager-workspace/workspace-home.png" alt-text="Captura de pantalla del espacio de trabajo de Azure Machine Learning":::

Para ver todos los recursos asociados a la implementación, haga clic en el vínculo de la parte superior izquierda con el nombre del área de trabajo (en la captura de pantalla, `my_templated_ws`). El vínculo le lleva al grupo de recursos de Azure Portal. El nombre del grupo de recursos es `{projectName}rg` y el área de trabajo se denomina `{projectName}ws`.

## <a name="clean-up-resources"></a>Limpieza de recursos

Si no desea usar esta área de trabajo, elimínela. Dado que el área de trabajo está asociada a otros recursos, como una cuenta de almacenamiento, es probable que desee eliminar todo el grupo de recursos que ha creado. Puede eliminar el grupo de recursos mediante el portal si hace clic en el botón "Eliminar" y confirma. Además, puede eliminar el grupo de recursos desde la CLI con: 

```azurecli-interactive
echo "Enter the Resource Group name:" &&
read resourceGroupName &&
az group delete --name $resourceGroupName &&
echo "Press [ENTER] to continue ..."
```

## <a name="next-steps"></a>Pasos siguientes

En este tutorial creó un área de trabajo de Azure Machine Learning mediante una plantilla de Azure Resource Manager. Si desea explorar Azure Machine Learning, continúe con el tutorial. 

> [!div class="nextstepaction"]
> [Tutorial: Creación del primer experimento de ML con el SDK de Python](tutorial-1st-experiment-sdk-setup.md)
