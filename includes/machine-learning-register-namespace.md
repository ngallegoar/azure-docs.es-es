---
author: Blackmist
ms.service: machine-learning
ms.topic: include
ms.date: 12/01/2020
ms.author: larryfr
ms.openlocfilehash: 6e9f349f2fd90fdb12d8d26ca904e504b75d9db2
ms.sourcegitcommit: 6a350f39e2f04500ecb7235f5d88682eb4910ae8
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 12/01/2020
ms.locfileid: "96445193"
---
* Al crear una nueva área de trabajo, puede crear automáticamente los servicios necesarios para esa área de trabajo o usar los servicios ya existentes. Si quiere usar los __servicios ya existentes de una suscripción de Azure diferente__ al área de trabajo, debe registrar el espacio de nombres de Azure Machine Learning en la suscripción que contiene esos servicios. Por ejemplo, al crear un área de trabajo en la suscripción A que usa una cuenta de almacenamiento de la suscripción B, el espacio de nombres de Azure Machine Learning debe estar registrado en la suscripción B para poder usar la cuenta de almacenamiento con el área de trabajo.

    El proveedor de recursos para Azure Machine Learning es __Microsoft.MachineLearningService__. Para obtener información sobre cómo comprobar si está registrado y cómo realizar el registro, consulte el artículo [Proveedores de recursos de Azure y sus tipos](../articles/azure-resource-manager/management/resource-providers-and-types.md).

    > [!IMPORTANT]
    > Esta opción solo se aplica a los recursos proporcionados durante la creación del área de trabajo para cuentas de Azure Storage, el registro de contenedores de Azure, Azure Key Vault y Application Insights.