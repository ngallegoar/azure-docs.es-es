---
author: Blackmist
ms.service: machine-learning
ms.topic: include
ms.date: 03/26/2020
ms.author: larryfr
ms.openlocfilehash: df4735699f71256598bfd418a505ae8d3e5b816c
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/28/2020
ms.locfileid: "80296876"
---
Después de iniciar sesión, verá una lista de suscripciones asociadas con su cuenta de Azure. La información de suscripción con `isDefault: true` es la suscripción actualmente activada para los comandos de la CLI de Azure. Esta suscripción debe ser la misma que la que contiene el área de trabajo de Azure Machine Learning. Puede buscar el identificador de suscripción en [Azure Portal](https://portal.azure.com); para ello, visite la página de información general del área de trabajo. También puede usar el SDK para obtener el identificador de suscripción del objeto del área de trabajo. Por ejemplo, `Workspace.from_config().subscription_id`.
    
Para seleccionar otra suscripción, utilice el comando [az account set](https://docs.microsoft.com/cli/azure/account#az-account-set) con el identificador de suscripción al que desea cambiar. Para obtener más información sobre la selección de la suscripción, consulte [Uso de varias suscripciones de Azure](https://docs.microsoft.com/cli/azure/manage-azure-subscriptions-azure-cli?view=azure-cli-latest).