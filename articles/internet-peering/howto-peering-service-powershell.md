---
title: Habilitación de Azure Peering Service en un emparejamiento directo mediante PowerShell
titleSuffix: Azure
description: Habilitación de Azure Peering Service en un emparejamiento directo mediante PowerShell
services: internet-peering
author: prmitiki
ms.service: internet-peering
ms.topic: article
ms.date: 11/27/2019
ms.author: prmitiki
ms.openlocfilehash: c029b822d00ce545e3623a6212421a55b2d1971f
ms.sourcegitcommit: acb82fc770128234f2e9222939826e3ade3a2a28
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 04/21/2020
ms.locfileid: "81686995"
---
# <a name="enable-azure-peering-service-on-a-direct-peering-by-using-powershell"></a>Habilitación de Azure Peering Service en un emparejamiento directo mediante PowerShell

En este artículo se describe cómo habilitar Azure [Peering Service](overview-peering-service.md) en un emparejamiento directo con cmdlets de PowerShell y el modelo de implementación de Azure Resource Manager.

Si lo prefiere, puede realizar los pasos de esta guía mediante [Azure Portal](howto-peering-service-portal.md).

## <a name="before-you-begin"></a>Antes de empezar
* Revise los [requisitos previos](prerequisites.md) antes de comenzar la configuración.
* Elija un emparejamiento directo en la suscripción en la que quiera habilitar Peering Service. Si no tiene ninguna, convierta un emparejamiento directo heredado o cree uno:
    * Para convertir un emparejamiento directo heredado, siga las instrucciones que se indican en [Conversión de un emparejamiento directo heredado en un recurso de Azure mediante PowerShell](howto-legacy-direct-powershell.md).
    * Para crear un nuevo emparejamiento directo, siga las instrucciones en [Creación o modificación de un emparejamiento directo mediante PowerShell](howto-direct-powershell.md).

### <a name="work-with-azure-powershell"></a>Funcionamiento con Azure PowerShell
[!INCLUDE [CloudShell](./includes/cloudshell-powershell-about.md)]

## <a name="enable-peering-service-on-a-direct-peering"></a>Habilitación de Peering Service en un emparejamiento directo

### <a name="view-direct-peering"></a><a name= get></a>Visualización de un emparejamiento directo
[!INCLUDE [peering-direct-get](./includes/direct-powershell-get.md)]

### <a name="enable-the-direct-peering-for-peering-service"></a><a name= get></a>Habilitación del emparejamiento directo para Peering Service

Después de obtener el emparejamiento directo en el paso anterior, habilítelo para Peering Service.
[!INCLUDE [peering-direct-modify](./includes/peering-service-direct-powershell.md)]

## <a name="modify-a-direct-peering-connection"></a>Modificación de una conexión de emparejamiento directo

Si tiene que modificar la configuración de la conexión, consulte la sección "Modificación de un emparejamiento directo" en [Creación o modificación de un emparejamiento directo mediante PowerShell](howto-direct-powershell.md).

## <a name="next-steps"></a>Pasos siguientes

* [Creación o modificación de un emparejamiento de Exchange mediante PowerShell](howto-exchange-powershell.md)
* [Conversión de un emparejamiento de Exchange heredado en un recurso de Azure mediante PowerShell](howto-legacy-exchange-powershell.md)

## <a name="additional-resources"></a>Recursos adicionales
Puede obtener una descripción detallada de todos los parámetros ejecutando el siguiente comando:

```powershell
Get-Help Get-AzPeering -detailed
```

Puede consultar las preguntas frecuentes en [Preguntas más frecuentes sobre Peering Service](service-faqs.md).