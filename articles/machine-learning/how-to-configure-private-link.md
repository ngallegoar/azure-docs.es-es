---
title: Configuración de un punto de conexión privado
titleSuffix: Azure Machine Learning
description: Use Azure Private Link para acceder de forma segura al área de trabajo de Azure Machine Learning desde una red virtual.
services: machine-learning
ms.service: machine-learning
ms.subservice: core
ms.topic: conceptual
ms.custom: how-to
ms.author: aashishb
author: aashishb
ms.reviewer: larryfr
ms.date: 09/30/2020
ms.openlocfilehash: 4ba7ec73ac70723e21b6acad571d62d14edd250a
ms.sourcegitcommit: d2222681e14700bdd65baef97de223fa91c22c55
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/07/2020
ms.locfileid: "91828128"
---
# <a name="configure-azure-private-link-for-an-azure-machine-learning-workspace"></a>Configuración de Azure Private Link para un área de trabajo de Azure Machine Learning

En este documento, aprenderá a usar Azure Private Link con el área de trabajo de Azure Machine Learning. Para obtener más información sobre la creación de una red virtual para Azure Machine Learning, consulte [Información general sobre la privacidad y el aislamiento de la red virtual](how-to-network-security-overview.md).

Azure Private Link le permite conectarse a su área de trabajo mediante un punto de conexión privado. El punto de conexión privado es un conjunto de direcciones IP privadas dentro de la red virtual. Después, puede limitar el acceso al área de trabajo para que solo se produzca en las direcciones IP privadas. Private Link ayuda a reducir el riesgo de una filtración de datos. Para más información sobre los puntos de conexión privados, consulte el artículo [Azure Private Link](/azure/private-link/private-link-overview).

> [!IMPORTANT]
> Azure Private Link no afecta al plano de control de Azure (operaciones de administración), como la eliminación del área de trabajo o la administración de los recursos de proceso. Por ejemplo, crear, actualizar o eliminar un destino de proceso. Estas operaciones se realizan sobre la red pública de Internet como de costumbre. Las operaciones del plano de datos, como el uso de Azure Machine Learning Studio, las API (incluidas las canalizaciones publicadas) o el SDK usan el punto de conexión privado.
>
> Si usa Mozilla Firefox, puede encontrar problemas al intentar acceder al punto de conexión privado del área de trabajo. Este problema puede estar relacionado con DNS a través de HTTPS en Mozilla. Como solución alternativa, se recomienda usar Microsoft Edge de Google Chrome.

## <a name="prerequisites"></a>Prerrequisitos

Si planea usar un área de trabajo compatible con un vínculo privado con una clave administrada por el cliente, debe solicitar esta característica mediante una incidencia de soporte técnico. Para obtener más información, consulte [Administración y configuración de cuotas](how-to-manage-quotas.md#private-endpoint-and-private-dns-quota-increases).

## <a name="limitations"></a>Limitaciones

El uso de un área de trabajo Azure Machine Learning con un vínculo privado no está disponible en las regiones de Azure Government ni en las regiones de Azure China 21Vianet.

## <a name="create-a-workspace-that-uses-a-private-endpoint"></a>Creación de un área de trabajo que usa un punto de conexión privado

Siga uno de los métodos a continuación para crear un área de trabajo con un punto de conexión privado:

> [!TIP]
> La plantilla de Azure Resource Manager puede crear una nueva red virtual si es necesario. Todos los otros métodos requieren una red virtual existente.

# <a name="resource-manager-template"></a>[Plantilla de Resource Manager](#tab/azure-resource-manager)

La plantilla de Azure Resource Manager en [https://github.com/Azure/azure-quickstart-templates/tree/master/201-machine-learning-advanced](https://github.com/Azure/azure-quickstart-templates/tree/master/201-machine-learning-advanced) proporciona una manera sencilla de crear un área de trabajo con un punto de conexión privado y una red virtual.

Para información sobre el uso de esta plantilla, que incluye los puntos de conexión privados, consulte [Uso de una plantilla de Azure Resource Manager para crear un área de trabajo para Azure Machine Learning](how-to-create-workspace-template.md).

# <a name="python"></a>[Python](#tab/python)

El SDK de Azure Machine Learning para Python proporciona la clase [PrivateEndpointConfig](https://docs.microsoft.com/python/api/azureml-core/azureml.core.privateendpointconfig?view=azure-ml-py), que se puede usar con [Workspace.create()](https://docs.microsoft.com/python/api/azureml-core/azureml.core.workspace.workspace?view=azure-ml-py#create-name--auth-none--subscription-id-none--resource-group-none--location-none--create-resource-group-true--sku--basic---tags-none--friendly-name-none--storage-account-none--key-vault-none--app-insights-none--container-registry-none--adb-workspace-none--cmk-keyvault-none--resource-cmk-uri-none--hbi-workspace-false--default-cpu-compute-target-none--default-gpu-compute-target-none--private-endpoint-config-none--private-endpoint-auto-approval-true--exist-ok-false--show-output-true-) para crear un área de trabajo con un punto de conexión privado. Esta clase requiere una red virtual existente.

# <a name="azure-cli"></a>[CLI de Azure](#tab/azure-cli)

La [extensión de la CLI de Azure para Machine Learning](reference-azure-machine-learning-cli.md) proporciona el comando [az ml workspace create](https://docs.microsoft.com/cli/azure/ext/azure-cli-ml/ml/workspace?view=azure-cli-latest#ext_azure_cli_ml_az_ml_workspace_create). Los siguientes parámetros para este comando se pueden usar para crear un área de trabajo con una red privada, pero se requiere una red virtual existente:

* `--pe-name`: nombre del punto de conexión privado creado.
* `--pe-auto-approval`: indica si las conexiones de punto de conexión privado al área de trabajo se deben aprobar automáticamente.
* `--pe-resource-group`: grupo de recursos en el que se va a crear el punto de conexión privado. Debe ser el mismo grupo que contiene la red virtual.
* `--pe-vnet-name`: red virtual existente en la que se va a crear el punto de conexión privado.
* `--pe-subnet-name`: nombre de la subred en la que se va a crear el punto de conexión privado. El valor predeterminado es `default`.

# <a name="portal"></a>[Portal](#tab/azure-portal)

La pestaña __Redes__ de Azure Machine Learning Studio le permite configurar un punto de conexión privado. Sin embargo, se requiere una red virtual existente. Para obtener más información, consulte [Creación de áreas de trabajo en el portal](how-to-manage-workspace.md).

---

## <a name="using-a-workspace-over-a-private-endpoint"></a>Uso de un área de trabajo mediante un punto de conexión privado

Dado que la comunicación con el área de trabajo solo se permite desde la red virtual, cualquier entorno de desarrollo que use el área de trabajo debe ser miembro de la red virtual. Por ejemplo, una máquina virtual en la red virtual.

> [!IMPORTANT]
> Para evitar la interrupción temporal de la conectividad, Microsoft recomienda vaciar la memoria caché de DNS en las máquinas que se conectan al área de trabajo después de habilitar Private Link. 

Para más información sobre Azure Virtual Machines, consulte [Documentación sobre Virtual Machines](/azure/virtual-machines/).


## <a name="next-steps"></a>Pasos siguientes

* Para más información sobre cómo proteger el área de trabajo de Azure Machine Learning, consulte el artículo [Información general sobre la privacidad y el aislamiento de la red virtual](how-to-network-security-overview.md).

* Si tiene pensado usar una solución DNS personalizada en la red virtual, consulte [cómo usar un área de trabajo con un servidor DNS personalizado](how-to-custom-dns.md).
