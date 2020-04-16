---
title: Extensión de máquina virtual del agente de Azure Network Watcher para Linux
description: Implemente el agente de Network Watcher en la máquina virtual Linux mediante una extensión de máquina virtual.
services: virtual-machines-linux
documentationcenter: ''
author: mimckitt
manager: vashan
editor: ''
tags: azure-resource-manager
ms.assetid: 5c81e94c-e127-4dd2-ae83-a236c4512345
ms.service: virtual-machines-linux
ms.topic: article
ms.tgt_pltfrm: vm-linux
ms.workload: infrastructure-services
ms.date: 02/14/2017
ms.author: mimckitt
ms.openlocfilehash: 476241ad3d4077f2da0c513e3c989218701232ba
ms.sourcegitcommit: 8dc84e8b04390f39a3c11e9b0eaf3264861fcafc
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 04/13/2020
ms.locfileid: "81255760"
---
# <a name="network-watcher-agent-virtual-machine-extension-for-linux"></a>Extensión de máquina virtual del agente de Network Watcher para Linux

## <a name="overview"></a>Información general

[Azure Network Watcher](/azure/network-watcher/) es un servicio de supervisión, diagnóstico y análisis del rendimiento de red que permite supervisar redes de Azure. La extensión de máquina virtual (VM) del agente de Network Watcher es un requisito para algunas características de Network Watcher en las VM de Azure, como capturar el tráfico de red a petición y otras funcionalidades avanzadas.

En este artículo, se especifican las plataformas compatibles y las opciones de implementación de la extensión de VM del agente de Network Watcher para Linux. La instalación del agente no interrumpe la VM ni requiere reiniciarla. Puede implementar la extensión en máquinas virtuales que implemente. Si la máquina virtual se ha implementado mediante un servicio de Azure, consulte la documentación del servicio para decidir si permite o no la instalación de extensiones en la máquina virtual.

## <a name="prerequisites"></a>Prerrequisitos

### <a name="operating-system"></a>Sistema operativo

La extensión del agente de Network Watcher puede configurarse para las siguientes distribuciones de Linux:

| Distribución | Versión |
|---|---|
| Ubuntu | 12+ |
| Debian | 7 y 8 |
| Red Hat | 6 y 7 |
| Oracle Linux | 6.8+ y 7 |
| SUSE Linux Enterprise Server | 11 y 12 |
| OpenSUSE Leap | 42.3+ |
| CentOS | 6.5+ y 7 |
| CoreOS | 899.17.0+ |


### <a name="internet-connectivity"></a>Conectividad de Internet

Parte de la funcionalidad del agente de Network Watcher requiere que una VM esté conectada a Internet. Sin la capacidad de establecer conexiones salientes, algunas de las características del agente de Network Watcher pueden funcionar de forma incorrecta o dejar de estar disponibles. Para obtener más información sobre la funcionalidad de Network Watcher que requiere el agente, consulte la[documentación de Network Watcher](/azure/network-watcher/).

## <a name="extension-schema"></a>Esquema de extensión

El siguiente JSON muestra el esquema para la extensión del agente de Network Watcher. La extensión no requiere ni admite ninguna configuración proporcionada por el usuario. La extensión se basa en su configuración predeterminada.

```json
{
    "type": "extensions",
    "name": "Microsoft.Azure.NetworkWatcher",
    "apiVersion": "[variables('apiVersion')]",
    "location": "[resourceGroup().location]",
    "dependsOn": [
        "[concat('Microsoft.Compute/virtualMachines/', variables('vmName'))]"
    ],
    "properties": {
        "publisher": "Microsoft.Azure.NetworkWatcher",
        "type": "NetworkWatcherAgentLinux",
        "typeHandlerVersion": "1.4",
        "autoUpgradeMinorVersion": true
    }
}
```

### <a name="property-values"></a>Valores de propiedad

| Nombre | Valor / ejemplo |
| ---- | ---- |
| apiVersion | 2015-06-15 |
| publisher | Microsoft.Azure.NetworkWatcher |
| type | NetworkWatcherAgentLinux |
| typeHandlerVersion | 1.4 |

## <a name="template-deployment"></a>Implementación de plantilla

Puede implementar las extensiones de VM de Azure con una plantilla de Azure Resource Manager. Para implementar la extensión del agente de Network Watcher, utilice el esquema JSON anterior en la plantilla.

## <a name="azure-classic-cli-deployment"></a>Implementación de la CLI de Azure clásica

[!INCLUDE [classic-vm-deprecation](../../../includes/classic-vm-deprecation.md)]

En el ejemplo siguiente, se implementa la extensión de VM del agente de Network Watcher en una VM existente implementada a través del modelo de implementación clásica:

```console
azure config mode asm
azure vm extension set myVM1 NetworkWatcherAgentLinux Microsoft.Azure.NetworkWatcher 1.4
```

## <a name="azure-cli-deployment"></a>Implementación de la CLI de Azure

En el ejemplo siguiente, se implementa la extensión de VM del agente de Network Watcher en una VM existente implementada a través de Resource Manager:

```azurecli
az vm extension set --resource-group myResourceGroup1 --vm-name myVM1 --name NetworkWatcherAgentLinux --publisher Microsoft.Azure.NetworkWatcher --version 1.4
```

## <a name="troubleshooting-and-support"></a>Solución de problemas y soporte técnico

### <a name="troubleshooting"></a>Solución de problemas

Puede recuperar los datos sobre el estado de las implementaciones de extensiones mediante Azure Portal o la CLI de Azure.

En el ejemplo siguiente, se muestra el estado de implementación de la extensión NetworkWatcherAgentLinux en una máquina virtual implementada mediante Resource Manager, mediante la CLI de Azure:

```azurecli
az vm extension show --name NetworkWatcherAgentLinux --resource-group myResourceGroup1 --vm-name myVM1
```

### <a name="support"></a>Soporte técnico

Si necesita más ayuda con cualquier aspecto de este artículo, puede consultar la [documentación de Network Watcher](/azure/network-watcher/) o ponerse en contacto con los expertos de Azure en los [foros de MSDN Azure o Stack Overflow](https://azure.microsoft.com/support/forums/). Como alternativa, puede registrar un incidente de soporte técnico de Azure. Vaya al [sitio de soporte técnico de Azure](https://azure.microsoft.com/support/options/) y seleccione **Obtener soporte**. Para obtener más información sobre el uso del soporte técnico de Azure, consulte las [preguntas más frecuentes de Microsoft Azure](https://azure.microsoft.com/support/faq/).
