---
title: Retirada de la API Azure Monitor
description: En este artículo se describe la retirada de las versiones anteriores de la API del proveedor de recursos OperationalInsights.
ms.subservice: logs
ms.topic: conceptual
author: bwren
ms.author: bwren
ms.date: 10/02/2020
ms.openlocfilehash: ad3287651716580aaf6ab8c5e819fd92a70e695d
ms.sourcegitcommit: dbe434f45f9d0f9d298076bf8c08672ceca416c6
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/17/2020
ms.locfileid: "92144104"
---
# <a name="operationalinsights-api-version-retirement"></a>Retirada de versiones de la API OperationalInsights
Microsoft notifica la retirada de una API con al menos 12 meses de antelación para facilitar la transición a una versión compatible o más reciente. Hemos publicado una versión nueva (2020-08-01) para las API del proveedor de recursos **OperationalInsights** y retiraremos las versiones anteriores de la API el 31 de octubre de 2023.

Le recomendamos que empiece a usar ya la versión 2020-08-01 para obtener las ventajas de la nueva funcionalidad como, por ejemplo, un [clúster dedicado](https://docs.microsoft.com/azure/azure-monitor/log-query/logs-dedicated-clusters), [claves administradas por el cliente](https://docs.microsoft.com/azure/azure-monitor/platform/customer-managed-keys), [vínculo privado](https://docs.microsoft.com/azure/azure-monitor/platform/private-link-security) y [exportación de datos](https://docs.microsoft.com/azure/azure-monitor/platform/logs-data-export). Además, las nuevas características, funcionalidades y optimizaciones solo se han agregado a la API actual.

Después del 31 de octubre de 2023, Azure Monitor ya no admitirá las versiones de API anteriores a 2020-08-01. Si prefiere no realizar la actualización, el servidor de Azure Monitor seguirá atendiendo las solicitudes enviadas desde las versiones anteriores hasta el 31 de octubre de 2023.

## <a name="migration-steps"></a>Pasos de migración
Según el método de configuración que use, debe actualizar la versión nueva en solicitudes **REST** y en **plantillas de Resource Manager** . Siga los ejemplos siguientes para actualizar la versión de la API:

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
