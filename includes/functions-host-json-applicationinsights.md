---
title: archivo de inclusión
description: archivo de inclusión
author: ggailey777
ms.service: azure-functions
ms.topic: include
ms.date: 10/19/2018
ms.author: glenga
ms.custom: include file
ms.openlocfilehash: a925076dfccd30c73febb2aadc8692667ea01525
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/28/2020
ms.locfileid: "76279592"
---
Controla la [característica de muestreo de Application Insights](../articles/azure-functions/functions-monitoring.md#configure-sampling).

```json
{
    "applicationInsights": {
        "sampling": {
          "isEnabled": true,
          "maxTelemetryItemsPerSecond" : 5
        }
    }
}
```

|Propiedad  |Valor predeterminado | Descripción |
|---------|---------|---------| 
|isEnabled|true|Habilita o deshabilita el muestreo.| 
|maxTelemetryItemsPerSecond|5|Umbral donde comienza el muestreo.| 
