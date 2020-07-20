---
author: ggailey777
ms.service: azure-functions
ms.topic: include
ms.date: 06/15/2020
ms.author: glenga
ms.openlocfilehash: 49818cf59da2d63cef4bb0bdca38d38a2feafca5
ms.sourcegitcommit: 1e6c13dc1917f85983772812a3c62c265150d1e7
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 07/09/2020
ms.locfileid: "86169922"
---
| | |
|--|--|
|**`<DESTINATION>`**| Destino al que se envían los registros. Los valores válidos son `AppInsights` y `Blob`.<br/>Cuando use `AppInsights`, asegúrese de que [Application Insights está habilitado en la aplicación de funciones](../articles/azure-functions/functions-monitoring.md#enable-application-insights-integration).<br/>Al establecer el destino en `Blob`, los registros se crean en un contenedor de blobs denominado `azure-functions-scale-controller` en la cuenta de almacenamiento predeterminada establecida en la configuración de la aplicación `AzureWebJobsStorage`. |
|**`<VERBOSITY>`** | Especifica el nivel de registro. Los valores admitidos son `None`, `Warning` y `Verbose`.<br/>Cuando se establece en `Verbose`, el controlador de escala registra una razón para cada cambio en el número de trabajos, así como información sobre los desencadenadores que se aplican a esas decisiones. Los registros detallados incluyen advertencias de desencadenador y los valores hash que usan los desencadenadores antes y después de que se ejecute el controlador de escala. |

> [!CAUTION]
> No deje habilitado el registro del controlador de escala. Habilite el registro hasta que haya recopilado suficientes datos para entender cómo se comporta el controlador de escala y, a continuación, deshabilítelo.