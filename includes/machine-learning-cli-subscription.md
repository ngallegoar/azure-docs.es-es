---
author: Blackmist
ms.service: machine-learning
ms.topic: include
ms.date: 03/26/2020
ms.author: larryfr
ms.openlocfilehash: 6147dff71ff8c93c8c46b5921b37db459ea73e4a
ms.sourcegitcommit: a43a59e44c14d349d597c3d2fd2bc779989c71d7
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 11/25/2020
ms.locfileid: "96026338"
---
> [!TIP]
> Después de iniciar sesión, verá una lista de suscripciones asociadas con su cuenta de Azure. La información de suscripción con `isDefault: true` es la suscripción actualmente activada para los comandos de la CLI de Azure. Esta suscripción debe ser la misma que la que contiene el área de trabajo de Azure Machine Learning. Puede buscar el identificador de suscripción en [Azure Portal](https://portal.azure.com); para ello, visite la página de información general del área de trabajo. También puede usar el SDK para obtener el identificador de suscripción del objeto del área de trabajo. Por ejemplo, `Workspace.from_config().subscription_id`.
> 
> Para seleccionar otra suscripción, utilice el comando `az account set -s <subscription name or ID>` y especifique el nombre o identificador de la suscripción a los que desea cambiar. Para obtener más información sobre la selección de la suscripción, consulte [Uso de varias suscripciones de Azure](/cli/azure/manage-azure-subscriptions-azure-cli?view=azure-cli-latest).