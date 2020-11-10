---
title: 'Inicio rápido: Configuración de los registros de flujo de NSG mediante una plantilla de Azure Resource Manager'
description: Aprenda cómo habilitar los registros de flujo de NSG mediante programación con una plantilla de Azure Resource Manager (plantilla de ARM) y Azure PowerShell.
services: network-watcher
author: damendo
Customer intent: I need to enable the NSG flow logs using Azure Resource Manager Template
ms.service: network-watcher
ms.topic: quickstart
ms.date: 07/22/2020
ms.author: damendo
ms.custom: subject-armqs
ms.openlocfilehash: 96f30c05527754cbce3b7593c8d62fb56844d41e
ms.sourcegitcommit: 4f4a2b16ff3a76e5d39e3fcf295bca19cff43540
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/30/2020
ms.locfileid: "93042751"
---
# <a name="quickstart-configure-nsg-flow-logs-using-an-arm-template"></a>Inicio rápido: Configuración de los registros de flujo de NSG mediante una plantilla de ARM

En este inicio rápido habilitará los [registros de flujo de NSG](network-watcher-nsg-flow-logging-overview.md) con una plantilla de [Azure Resource Manager](../azure-resource-manager/management/overview.md) (plantilla de ARM) y Azure PowerShell.

[!INCLUDE [About Azure Resource Manager](../../includes/resource-manager-quickstart-introduction.md)]

Comenzaremos con información general de las propiedades del objeto del registro de flujo de NSG, seguida de algunas plantillas de ejemplo. A continuación, usaremos una instancia local de PowerShell para implementar la plantilla.

Si su entorno cumple los requisitos previos y está familiarizado con el uso de plantillas de Resource Manager, seleccione el botón **Implementar en Azure**. La plantilla se abrirá en Azure Portal.

[![Implementación en Azure](../media/template-deployments/deploy-to-azure.svg)](https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2FAzure%2Fazure-quickstart-templates%2Fmaster%2F101-networkwatcher-flowLogs-create%2Fazuredeploy.json)

## <a name="prerequisites"></a>Requisitos previos

Si no tiene una suscripción a Azure, cree una [cuenta gratuita](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) antes de empezar.

## <a name="review-the-template"></a>Revisión de la plantilla

La plantilla usada en este inicio rápido forma parte de las [plantillas de inicio rápido de Azure](https://azure.microsoft.com/resources/templates/101-networkwatcher-flowlogs-create).

:::code language="json" source="~/quickstart-templates/101-networkwatcher-flowlogs-create/azuredeploy.json":::

En la plantilla se definen varios recursos:

- [Microsoft.Storage/storageAccounts](/azure/templates/microsoft.storage/storageaccounts)
- [Microsoft.Resources/deployments](/azure/templates/microsoft.resources/deployments)

## <a name="nsg-flow-logs-object"></a>Objeto de los registros de flujo de NSG

A continuación se muestra el objeto de los registros de flujo de NSG con todos los parámetros. Para obtener información general completa de las propiedades, consulte [Microsoft.Network networkWatchers/flowLogs](/azure/templates/microsoft.network/networkwatchers/flowlogs).

```json
{
  "name": "string",
  "type": "Microsoft.Network/networkWatchers/flowLogs",
  "location": "string",
  "apiVersion": "2019-09-01",
  "properties": {
    "targetResourceId": "string",
    "storageId": "string",
    "enabled": "boolean",
    "flowAnalyticsConfiguration": {
      "networkWatcherFlowAnalyticsConfiguration": {
        "enabled": "boolean",
        "workspaceResourceId": "string",
        "trafficAnalyticsInterval": "integer"
      },
      "retentionPolicy": {
        "days": "integer",
        "enabled": "boolean"
      },
      "format": {
        "type": "string",
        "version": "integer"
      }
    }
  }
}
```

Para crear un recurso `Microsoft.Network/networkWatchers/flowLogs`, agregue el código JSON anterior a la sección de recursos de la plantilla.

## <a name="creating-your-template"></a>Creación de la plantilla

Si es la primera vez que usa plantillas de ARM, puede obtener información sobre ellas con los vínculos siguientes.

- [Implementación de recursos con las plantillas de ARM y Azure PowerShell](../azure-resource-manager/templates/deploy-powershell.md#deploy-local-template)
- [Tutorial: Creación e implementación de su primera plantilla de Resource Manager](../azure-resource-manager/templates/template-tutorial-create-first-template.md)

La plantilla usada en este inicio rápido forma parte de las [plantillas de inicio rápido de Azure](https://azure.microsoft.com/resources/templates/101-networkwatcher-flowlogs-create).

El siguiente ejemplo de plantilla completa es la versión más sencilla con los parámetros mínimos pasados para configurar los registros de flujo de NSG. Para más ejemplos, vaya a esta [guía paso a paso](network-watcher-nsg-flow-logging-azure-resource-manager.md).

**Ejemplo** : La siguiente plantilla habilita los registros de flujo de NSG en un NSG de destino y los almacena en una cuenta de almacenamiento específica.

```json
{
  "$schema": "https://schema.management.azure.com/schemas/2019-04-01/deploymentTemplate.json#",
  "contentVersion": "1.0.0.0",
  "apiProfile": "2019-09-01",
  "resources": [
    {
      "name": "NetworkWatcher_centraluseuap/Microsoft.NetworkDalanDemoPerimeterNSG",
      "type": "Microsoft.Network/networkWatchers/FlowLogs/",
      "location": "centraluseuap",
      "apiVersion": "2019-09-01",
      "properties": {
        "targetResourceId": "/subscriptions/<subscription Id>/resourceGroups/DalanDemo/providers/Microsoft.Network/networkSecurityGroups/PerimeterNSG",
        "storageId": "/subscriptions/<subscription Id>/resourceGroups/MyCanaryFlowLog/providers/Microsoft.Storage/storageAccounts/storagev2ira",
        "enabled": true,
        "flowAnalyticsConfiguration": {},
        "retentionPolicy": {},
        "format": {}
      }
    }
  ]
}
```

> [!NOTE]
> - El nombre del recurso tiene el formato _Recurso primario_Recurso secundario_. En este caso, el recurso primario es la instancia regional de Network Watcher (formato: NetworkWatcher_NombreDeRegión. Ejemplo: NetworkWatcher_centraluseuap)
> - `targetResourceId` es el identificador de recurso del NSG de destino.
> - `storageId` es el identificador de recurso de la cuenta de almacenamiento de destino.

## <a name="deploy-the-template"></a>Implementación de la plantilla

En este tutorial se da por supuesto que tiene un grupo de recursos existente y un NSG en el que puede habilitar el registro de flujo.
Puede guardar cualquiera de las plantillas de ejemplo anteriores de forma local como `azuredeploy.json`. Actualice los valores de las propiedades para que apunten a recursos válidos de su suscripción.

Para implementar la plantilla, ejecute el siguiente comando en PowerShell.

```azurepowershell-interactive
$context = Get-AzSubscription -SubscriptionId <subscription Id>
Set-AzContext $context
New-AzResourceGroupDeployment -Name EnableFlowLog -ResourceGroupName NetworkWatcherRG `
    -TemplateFile "C:\MyTemplates\azuredeploy.json"
```

> [!NOTE]
> Los comandos anteriores implementan un recurso en el grupo de recursos NetworkWatcherRG y no en el grupo de recursos que contiene NSG.

## <a name="validate-the-deployment"></a>Validación de la implementación

Hay un par de maneras de comprobar si la implementación se ha realizado correctamente. La consola de PowerShell debería mostrar `ProvisioningState` como `Succeeded`. Además, puede visitar la [página del portal de registros de flujo de NSG](https://ms.portal.azure.com/#blade/Microsoft_Azure_Network/NetworkWatcherMenuBlade/flowLogs) para confirmar los cambios. Si se produjeron problemas con la implementación, consulte [Solución de errores comunes de implementación de Azure con Azure Resource Manager](../azure-resource-manager/templates/common-deployment-errors.md).

## <a name="clean-up-resources"></a>Limpieza de recursos

Azure permite la eliminación de recursos mediante el modo de implementación `Complete`. Para eliminar un recurso de registros de flujo, especifique una implementación en modo `Complete` sin incluir el recurso que quiere eliminar. Más información sobre el [modo de implementación completo](../azure-resource-manager/templates/deployment-modes.md#complete-mode).

Como alternativa, puede deshabilitar un registro de flujo de NSG desde Azure Portal como se indica en los pasos siguientes:

1. Inicie sesión en Azure Portal.
1. En la esquina superior izquierda del portal, seleccione **Todos los servicios**. En el cuadro **Filtrar** , escriba _Network Watcher_. Cuando aparezca la opción **Network Watcher** en los resultados de búsqueda, selecciónela.
1. En **REGISTROS** , seleccione **Registros de flujo de NSG**.
1. En la lista de grupos de seguridad de red, seleccione aquel para el que desea deshabilitar los registros de flujo.
1. En **Configuración de los registros de flujo** , establezca el estado del registro de flujos como **Desactivado**.
1. Desplácese hacia abajo y seleccione **Guardar**.

## <a name="next-steps"></a>Pasos siguientes

En este inicio rápido, ha habilitado los registros de flujo de NSG. Ahora, tiene que aprender a visualizar los datos de flujo de NSG con:

- [Microsoft Power BI](network-watcher-visualize-nsg-flow-logs-power-bi.md)
- [Herramientas de código abierto](network-watcher-visualize-nsg-flow-logs-open-source-tools.md)
- [Análisis de tráfico de Azure](traffic-analytics.md)
