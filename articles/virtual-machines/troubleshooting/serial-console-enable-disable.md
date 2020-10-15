---
title: Habilitación y deshabilitación de la consola serie de Azure | Microsoft Docs
description: Cómo habilitar y deshabilitar el servicio de consola serie de Azure
services: virtual-machines
documentationcenter: ''
author: asinn826
manager: borisb
editor: ''
tags: azure-resource-manager
ms.service: virtual-machines
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: vm
ms.workload: infrastructure-services
ms.date: 8/20/2019
ms.author: alsin
ms.openlocfilehash: e09e08f8ba36cf576bc27551254225adee3bb0fd
ms.sourcegitcommit: 829d951d5c90442a38012daaf77e86046018e5b9
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/09/2020
ms.locfileid: "75451296"
---
# <a name="enable-and-disable-the-azure-serial-console"></a>Habilitación y deshabilitación de la consola serie de Azure

Al igual que sucede con cualquier otro recurso, la consola serie de Azure se puede habilitar y deshabilitar. La consola serie está habilitada de forma predeterminada para todas las suscripciones de Azure global. Actualmente, al deshabilitar la consola serie, se deshabilitará el servicio para toda la suscripción. Para deshabilitar o volver a habilitar la consola serie para una suscripción es necesario tener acceso de nivel de colaborador o superior en la suscripción.

También puede deshabilitar la consola serie para una máquina virtual individual o una instancia de conjunto de escalado de máquinas virtuales mediante la deshabilitación de los diagnósticos de arranque. Necesitará acceso de nivel de colaborador o superior en la máquina virtual o el conjunto de escalado de máquinas virtuales y en la cuenta de almacenamiento de diagnósticos de arranque.

## <a name="vm-level-disable"></a>Deshabilitación a nivel de VM
La consola serie puede deshabilitarse para una máquina virtual o un conjunto de escalado de máquinas virtuales específicos al deshabilitar la configuración de diagnóstico de arranque. Desactive el diagnóstico de arranque desde Azure Portal para deshabilitar la consola serie para la máquina virtual o el conjunto de escalado de máquinas virtuales. Si utiliza la consola serie en un conjunto de escalado de máquinas virtuales, asegúrese de actualizar las instancias al modelo más reciente.


## <a name="subscription-level-enabledisable"></a>Habilitación y deshabilitación de nivel de suscripción

> [!NOTE]
> Asegúrese de que se encuentra en la nube correcta (nube pública de Azure o nube de Azure US Government) antes de ejecutar este comando. Puede consultar `az cloud list` y establecer la nube con `az cloud set -n <Name of cloud>`.

### <a name="azure-cli"></a>Azure CLI

La consola serie se puede deshabilitar y volver a habilitar para toda una suscripción mediante los siguientes comandos en la CLI de Azure (puede usar el botón "Probar" para iniciar una instancia de Azure Cloud Shell en la que puede ejecutar los comandos):

Para deshabilitar la consola serie para una suscripción, use estos comandos:
```azurecli-interactive
subscriptionId=$(az account show --output=json | jq -r .id)

az resource invoke-action --action disableConsole --ids "/subscriptions/$subscriptionId/providers/Microsoft.SerialConsole/consoleServices/default" --api-version="2018-05-01"
```

Para habilitar la consola serie para una suscripción, use estos comandos:
```azurecli-interactive
subscriptionId=$(az account show --output=json | jq -r .id)

az resource invoke-action --action enableConsole --ids "/subscriptions/$subscriptionId/providers/Microsoft.SerialConsole/consoleServices/default" --api-version="2018-05-01"
```

Para obtener el estado habilitado o deshabilitado actual de la consola serie para una suscripción, use estos comandos:
```azurecli-interactive
subscriptionId=$(az account show --output=json | jq -r .id)

az resource show --ids "/subscriptions/$subscriptionId/providers/Microsoft.SerialConsole/consoleServices/default" --output=json --api-version="2018-05-01" | jq .properties
```

### <a name="powershell"></a>PowerShell

También se puede habilitar y deshabilitar la consola serie mediante PowerShell.

Para deshabilitar la consola serie para una suscripción, use estos comandos:
```azurepowershell-interactive
$subscription=(Get-AzContext).Subscription.Id

Invoke-AzResourceAction -Action disableConsole -ResourceId /subscriptions/$subscription/providers/Microsoft.SerialConsole/consoleServices/default -ApiVersion 2018-05-01
```

Para habilitar la consola serie para una suscripción, use estos comandos:
```azurepowershell-interactive
$subscription=(Get-AzContext).Subscription.Id

Invoke-AzResourceAction -Action enableConsole -ResourceId /subscriptions/$subscription/providers/Microsoft.SerialConsole/consoleServices/default -ApiVersion 2018-05-01
```

## <a name="next-steps"></a>Pasos siguientes
* Más información sobre la [consola serie de Azure para máquinas virtuales Linux](./serial-console-linux.md)
* Más información sobre la [consola serie de Azure para máquinas virtuales Windows](./serial-console-windows.md)
* Más información sobre las [opciones de administración de energía en la consola serie de Azure](./serial-console-power-options.md)