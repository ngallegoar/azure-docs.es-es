---
title: Retirada de la API Azure Monitor
description: En este artículo se describe la retirada de las versiones anteriores de la API del proveedor de recursos OperationalInsights.
ms.subservice: logs
ms.topic: conceptual
author: bwren
ms.author: bwren
ms.date: 10/29/2020
ms.openlocfilehash: e2b12d7a2206ab369328563af438c6ef1ea39327
ms.sourcegitcommit: d22a86a1329be8fd1913ce4d1bfbd2a125b2bcae
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 11/26/2020
ms.locfileid: "96184117"
---
# <a name="operationalinsights-api-version-retirement"></a>Retirada de versiones de la API OperationalInsights
Microsoft notifica la retirada de una API con al menos 12 meses de antelación para facilitar la transición a una versión compatible o más reciente. Hemos publicado una versión nueva (2020-08-01) para las API del proveedor de recursos **OperationalInsights** y retiraremos las versiones anteriores de la API el 29 de octubre de 2024.

Le recomendamos que empiece a usar ya la versión 2020-08-01 para obtener las ventajas de la nueva funcionalidad como, por ejemplo, un [clúster dedicado](../log-query/logs-dedicated-clusters.md), [claves administradas por el cliente](./customer-managed-keys.md), [vínculo privado](./private-link-security.md) y [exportación de datos](./logs-data-export.md). Además, las nuevas características, funcionalidades y optimizaciones solo se han agregado a la API actual.

Después del 29 de febrero de 2024, Azure Monitor ya no admitirá las versiones de API anteriores a la 2020-08-01. Si prefiere no realizar la actualización, el servidor de Azure Monitor seguirá atendiendo las solicitudes enviadas desde las versiones anteriores hasta el 29 de febrero de 2024.

## <a name="migration-steps"></a>Pasos de migración
Según el método de configuración que use, debe actualizar la versión nueva en solicitudes **REST** y en **plantillas de Resource Manager**. Siga los ejemplos siguientes para actualizar la versión de la API:

1. Las solicitudes de API REST usan la versión de la API de la dirección URL de la solicitud. Reemplace esa versión por la versión más reciente (2020-08-01) como se muestra en el ejemplo siguiente.

    ```rest
    https://management.azure.com/subscriptions/{subscriptionId}/resourcegroups/{resourceGroupName}/providers/Microsoft.OperationalInsights/workspaces/{workspaceName}?api-version=2020-08-01
    ```

2. Las plantillas de Azure Resource Manager usan la versión de la API que aparece en la propiedad **apiVersion** del recurso. Reemplace esa versión por la versión más reciente (2020-08-01) como se muestra en el ejemplo siguiente.


    ```json
    {
        "$schema": "https://schema.management.azure.com/schemas/2019-08-01/deploymentTemplate.json#",
        "contentVersion": "1.0.0.0",
        "parameters": {
            "workspaceName": {
                "type": "string",
                "metadata": {
                "description": "Name of the workspace."
                }
            },
            "resources": [
            {
                "type": "Microsoft.OperationalInsights/workspaces",
                "name": "[parameters('workspaceName')]",
                "apiVersion": "2020-08-01",
                "location": "westus",
                "properties": {
                    "sku": {
                        "name": "pergb2018"
                    },
                    "retentionInDays": 30,
                    "features": {
                        "searchVersion": 1,
                        "legacy": 0,
                        "enableLogAccessUsingOnlyResourcePermissions": true
                    }
                }
            }
        ]
    }
    }
    ```


## <a name="next-steps"></a>Pasos siguientes

- Consulte la [referencia para la API del área de trabajo OperationalInsights](/rest/api/loganalytics/workspaces).