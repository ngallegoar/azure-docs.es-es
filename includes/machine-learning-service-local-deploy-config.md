---
author: Blackmist
ms.service: machine-learning
ms.topic: include
ms.date: 03/16/2020
ms.author: larryfr
ms.openlocfilehash: 60c754aeec3157abee02eb136dcf148c440d8da0
ms.sourcegitcommit: 829d951d5c90442a38012daaf77e86046018e5b9
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/09/2020
ms.locfileid: "79477279"
---
Las entradas del documento `deploymentconfig.json` se asignan a los parámetros de [LocalWebservice.deploy_configuration](https://docs.microsoft.com/python/api/azureml-core/azureml.core.webservice.local.localwebservicedeploymentconfiguration?view=azure-ml-py). En la tabla siguiente se describe la asignación entre las entidades del documento JSON y los parámetros del método:

| Entidad JSON | Parámetro del método | Descripción |
| ----- | ----- | ----- |
| `computeType` | N/D | El destino de proceso. Para los destinos locales, el valor tiene que ser `local`. |
| `port` | `port` | Puerto local en el que se va a exponer el punto de conexión HTTP del servicio. |

Este elemento JSON es un ejemplo de la configuración de implementación que se puede usar con la CLI:

```json
{
    "computeType": "local",
    "port": 32267
}
```
