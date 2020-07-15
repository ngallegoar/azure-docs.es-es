---
author: erhopf
ms.service: cognitive-services
ms.topic: include
ms.date: 08/06/2019
ms.author: erhopf
ms.openlocfilehash: 54ee19f3e278b424384ff55d7065713584235df1
ms.sourcegitcommit: 5cace04239f5efef4c1eed78144191a8b7d7fee8
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 07/08/2020
ms.locfileid: "86144210"
---
## <a name="set-up"></a>Configurar

### <a name="create-a-translator-resource"></a>Creación de un recurso de Traductor

Los servicios de Azure Cognitive Services se representan por medio de recursos de Azure a los que se suscribe. Cree un recurso para Traductor mediante [Azure Portal](https://docs.microsoft.com/azure/cognitive-services/cognitive-services-apis-create-account) o la [CLI de Azure](https://docs.microsoft.com/azure/cognitive-services/cognitive-services-apis-create-account-cli) en la máquina local. También puede:

* Vea un recurso existente en [Azure Portal](https://portal.azure.com/).

Después de obtener una clave del recurso o la suscripción de evaluación, cree dos [variables de entorno](https://docs.microsoft.com/azure/cognitive-services/cognitive-services-apis-create-account#configure-an-environment-variable-for-authentication):

* `TRANSLATOR_TEXT_SUBSCRIPTION_KEY`: la clave de suscripción del recurso de Traductor.
* `TRANSLATOR_TEXT_ENDPOINT`: el punto de conexión global para Traductor. Mediante `https://api.cognitive.microsofttranslator.com/`.
