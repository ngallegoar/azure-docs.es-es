---
title: Creación o modificación de un emparejamiento directo mediante PowerShell
titleSuffix: Azure
description: Creación o modificación de un emparejamiento directo mediante PowerShell
services: internet-peering
author: prmitiki
ms.service: internet-peering
ms.topic: article
ms.date: 11/27/2019
ms.author: prmitiki
ms.openlocfilehash: 69031550bdab1535213c78f81426fa76e8ea62ad
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/27/2020
ms.locfileid: "75773647"
---
# <a name="create-or-modify-a-direct-peering-using-powershell"></a>Creación o modificación de un emparejamiento directo mediante PowerShell

En este artículo se describe cómo crear un emparejamiento directo de Microsoft con cmdlets de PowerShell y el modelo de implementación de Resource Manager. También se muestra cómo comprobar el estado del recurso, así como la forma de actualizarlo o eliminarlo y desaprovisionarlo.

Si lo prefiere, puede completar esta guía mediante el [portal](howto-direct-portal.md).

## <a name="before-you-begin"></a>Antes de empezar
* Revise la sección [Requisitos previos](prerequisites.md) y el [Tutorial del emparejamiento directo](walkthrough-direct-all.md) antes de comenzar la configuración.
* Si ya dispone de un emparejamiento directo con Microsoft, que no se convierte en un recurso de Azure, consulte el artículo [Conversión de un emparejamiento directo heredado en un recurso de Azure mediante PowerShell](howto-legacy-direct-powershell.md).

### <a name="working-with-azure-powershell"></a>Trabajo con Azure PowerShell
[!INCLUDE [CloudShell](./includes/cloudshell-powershell-about.md)]

## <a name="create-and-provision-a-direct-peering"></a>Creación y aprovisionamiento de un emparejamiento directo

### <a name="sign-in-to-your-azure-account-and-select-your-subscription"></a>Iniciar sesión en la cuenta de Azure y seleccione la suscripción
[!INCLUDE [Account](./includes/account-powershell.md)]

### <a name="get-the-list-of-supported-peering-locations-for-direct-peering"></a><a name=direct-location></a>Obtención de la lista de ubicaciones de emparejamiento compatibles para el emparejamiento directo
[!INCLUDE [direct-location](./includes/direct-powershell-create-location.md)]

### <a name="create-a-direct-peering"></a><a name=create></a>Creación de un emparejamiento directo
[!INCLUDE [direct-peering](./includes/direct-powershell-create-connection.md)]

### <a name="verify-direct-peering"></a><a name=get></a>Verificación de un emparejamiento directo
[!INCLUDE [peering-direct-get](./includes/direct-powershell-get.md)]

## <a name="modify-a-direct-peering"></a><a name="modify"></a>Modificación de un emparejamiento directo
[!INCLUDE [peering-direct-modify](./includes/direct-powershell-modify.md)]

## <a name="deprovision-a-direct-peering"></a><a name="delete"></a>Desaprovisionamiento de un emparejamiento directo
[!INCLUDE [peering-direct-delete](./includes/delete.md)]

## <a name="next-steps"></a>Pasos siguientes

* [Creación o modificación de un emparejamiento de Exchange mediante PowerShell](howto-exchange-powershell.md)
* [Conversión de un emparejamiento de Exchange heredado en un recurso de Azure mediante PowerShell](howto-legacy-exchange-powershell.md)

## <a name="additional-resources"></a>Recursos adicionales
Puede obtener una descripción detallada de todos los parámetros ejecutando el siguiente comando:

```powershell
Get-Help Get-AzPeering -detailed
```

Para obtener más información, consulte las [preguntas más frecuentes sobre el emparejamiento de Internet](faqs.md).
