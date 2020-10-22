---
title: archivo de inclusión
description: archivo de inclusión
author: ggailey777
ms.service: azure-functions
ms.topic: include
ms.date: 10/19/2018
ms.author: glenga
ms.custom: include file
ms.openlocfilehash: c63cee1c451918569e9b7aa2e76209e8069d86ac
ms.sourcegitcommit: 419c8c8061c0ff6dc12c66ad6eda1b266d2f40bd
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/18/2020
ms.locfileid: "92167666"
---
Especifica cuántas llamadas a funciones se agregan cuando se [calculan las métricas para Application Insights](../articles/azure-functions/configure-monitoring.md#configure-the-aggregator). 

```json
{
    "aggregator": {
        "batchSize": 1000,
        "flushTimeout": "00:00:30"
    }
}
```

|Propiedad |Valor predeterminado  | Descripción |
|---------|---------|---------| 
|batchSize|1000|Número máximo de solicitudes para agregar.| 
|flushTimeout|00:00:30|Período máximo de tiempo para agregar.| 

Las llamadas a funciones se agregan cuando se alcanza el primero de los dos límites.
