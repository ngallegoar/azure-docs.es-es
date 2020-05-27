---
title: Creación de un área de trabajo de Log Analytics mediante Azure PowerShell | Microsoft Docs
description: Aprenda a crear un área de trabajo de Log Analytics para habilitar soluciones de administración y recopilación de datos en sus entornos tanto locales como en la nube con Azure PowerShell.
ms.subservice: logs
ms.topic: conceptual
author: bwren
ms.author: bwren
ms.date: 03/12/2019
ms.openlocfilehash: 3547062f100eeb4dfa741a0566d737a0eac776a0
ms.sourcegitcommit: fc0431755effdc4da9a716f908298e34530b1238
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 05/24/2020
ms.locfileid: "83816450"
---
# <a name="create-a-log-analytics-workspace-with-azure-powershell"></a>Creación de un área de trabajo de Log Analytics con Azure PowerShell

El módulo de Azure PowerShell se usa para crear y administrar recursos de Azure desde la línea de comandos de PowerShell o en scripts. En esta guía de inicio rápido se muestra cómo usar el módulo de Azure PowerShell para implementar un área de trabajo de Log Analytics en Azure Monitor. Un área de trabajo de Log Analytics es un entorno único de datos de registro de Azure Monitor. Cada área de trabajo tiene su propio repositorio de datos y configuración, y las soluciones y orígenes de datos están configurados para almacenar sus datos en una determinada área de trabajo. Necesitará un área de trabajo de Log Analytics si tiene intención de recopilar datos de los orígenes siguientes:

* Recursos de Azure de la suscripción  
* Equipos locales supervisados por System Center Operations Manager  
* Colecciones de dispositivos de Configuration Manager  
* Datos de diagnóstico o registro de Azure Storage  
 
Para otros orígenes, como las máquinas virtuales de Azure y la máquinas virtuales Windows o Linux del entorno, consulte los temas siguientes:

* [Recopilación de datos de máquinas virtuales de Azure](../learn/quick-collect-azurevm.md)
* [Recopilación de datos de un equipo Linux híbrido](../learn/quick-collect-linux-computer.md)
* [Recopilación de datos de un equipo Windows híbrido](quick-collect-windows-computer.md)

Si no tiene una suscripción a Azure, cree una [cuenta gratuita](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) antes de empezar.

[!INCLUDE [updated-for-az](../../../includes/updated-for-az.md)]

[!INCLUDE [cloud-shell-try-it.md](../../../includes/cloud-shell-try-it.md)]

Si decide instalar y usar PowerShell localmente, para este tutorial se requiere el módulo de Azure PowerShell Az. Ejecute `Get-Module -ListAvailable Az` para encontrar la versión. Si necesita actualizarla, consulte [Instalación del módulo de Azure PowerShell](/powershell/azure/install-az-ps). Si PowerShell se ejecuta localmente, también debe ejecutar `Connect-AzAccount` para crear una conexión con Azure.

## <a name="create-a-workspace"></a>Crear un área de trabajo
Cree un área de trabajo con [New-AzResourceGroupDeployment](/powershell/module/az.resources/new-azresourcegroupdeployment). En el ejemplo siguiente se creará un área de trabajo en la ubicación *Este de EE. UU.* mediante una plantilla de Resource Manager desde la máquina local. La plantilla JSON está configurada para solicitar solo el nombre del área de trabajo y especifica un valor predeterminado para los restantes parámetros que es probable que se utilice como configuración estándar en su entorno. 

Para obtener información sobre las regiones compatibles, consulte [las regiones en las que Log Analytics está disponible](https://azure.microsoft.com/regions/services/) y busque Azure Monitor desde el campo **Buscar un producto**. 

Los siguientes parámetros establecen un valor predeterminado:

* Ubicación: el valor predeterminado es Este de EE. UU.
* SKU: el valor predeterminado es el nuevo plan de tarifa por GB publicado en el modelo de precios de abril de 2018

>[!WARNING]
>Si se crea o configura un área de trabajo de Log Analytics en una suscripción que ha elegido el nuevo modelo de precios de abril de 2018, el único plan de tarifa válido de Log Analytics es **PerGB2018**. 
>

### <a name="create-and-deploy-template"></a>Creación e implementación de una plantilla

1. Copie y pegue la siguiente sintaxis JSON en el archivo:

    ```json
    {
    "$schema": "https://schema.management.azure.com/schemas/2014-04-01-preview/deploymentTemplate.json#",
    "contentVersion": "1.0.0.0",
    "parameters": {
        "workspaceName": {
            "type": "String",
            "metadata": {
              "description": "Specifies the name of the workspace."
            }
        },
        "location": {
            "type": "String",
            "allowedValues": [
              "eastus",
              "westus"
            ],
            "defaultValue": "eastus",
            "metadata": {
              "description": "Specifies the location in which to create the workspace."
            }
        },
        "sku": {
            "type": "String",
            "allowedValues": [
              "Standalone",
              "PerNode",
              "PerGB2018"
            ],
            "defaultValue": "PerGB2018",
            "metadata": {
            "description": "Specifies the service tier of the workspace: Standalone, PerNode, Per-GB"
        }
          }
    },
    "resources": [
        {
            "type": "Microsoft.OperationalInsights/workspaces",
            "name": "[parameters('workspaceName')]",
            "apiVersion": "2015-11-01-preview",
            "location": "[parameters('location')]",
            "properties": {
                "sku": {
                    "Name": "[parameters('sku')]"
                },
                "features": {
                    "searchVersion": 1
                }
            }
          }
       ]
    }
    ```

2. Edite la plantilla para adecuarla a sus requisitos. Consulte la referencia [Plantilla Microsoft.OperationalInsights/workspaces](https://docs.microsoft.com/azure/templates/microsoft.operationalinsights/2015-11-01-preview/workspaces) para saber qué propiedades y valores son compatibles. 
3. Guarde este archivo como **deploylaworkspacetemplate.json** en una carpeta local.   
4. Está listo para implementar esta plantilla. Use los siguientes comandos desde la carpeta que contiene la plantilla. Cuando se le pida un nombre de área de trabajo, proporcione un nombre que sea globalmente único en todas las suscripciones de Azure.

    ```powershell
        New-AzResourceGroupDeployment -Name <deployment-name> -ResourceGroupName <resource-group-name> -TemplateFile deploylaworkspacetemplate.json
    ```

La implementación puede demorar unos minutos en completarse. Cuando termine, verá un mensaje similar al siguiente que incluye el resultado:

![Resultado de ejemplo cuando se completa la implementación](media/quick-create-workspace-posh/template-output-01.png)

## <a name="next-steps"></a>Pasos siguientes
Ahora que tiene un área de trabajo disponible, puede configurar la recopilación de datos de telemetría de supervisión, realizar búsquedas en el registro para analizar dichos datos y agregar una solución de administración que proporcione datos adicionales e información de los análisis.  

* Para habilitar la recopilación de datos de recursos de Azure con Azure Diagnostics o Azure Storage, consulte [Recopilación de registros y métricas de Azure para servicios de Azure Monitor](../platform/collect-azure-metrics-logs.md).  
* [Agregue System Center Operations Manager como origen de datos](../platform/om-agents.md) para recopilar datos de agentes que informan a su grupo de administración de Operations Manager y almacenarlos en el repositorio del área de trabajo de Log Analytics.  
* Conecte [Configuration Manager](../platform/collect-sccm.md) para importar equipos que son miembros de colecciones en la jerarquía.  
* Revise las [soluciones de supervisión](../insights/solutions.md) disponibles y cómo agregar o quitar una solución del área de trabajo.
