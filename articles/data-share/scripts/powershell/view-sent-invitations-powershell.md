---
title: 'Script de PowerShell: Enumerar las invitaciones de Azure Data Share enviadas a un consumidor | Microsoft Docs'
description: Este script de PowerShell acepta invitaciones de un recurso compartido de datos existente.
services: data-share
author: joannapea
ms.service: data-share
ms.workload: data-services
ms.tgt_pltfrm: na
ms.topic: article
ms.date: 07/07/2019
ms.author: joanpo
ms.openlocfilehash: 157b844afd9d59b6c03c7ae8585bc2182370d3a3
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/27/2020
ms.locfileid: "70307148"
---
# <a name="use-powershell-to-get-a-data-share-invitation"></a>Uso de PowerShell para obtener una invitación de un recurso compartido de datos

Este script de PowerShell obtiene las invitaciones enviadas a un consumidor.

## <a name="sample-script"></a>Script de ejemplo
```powershell
# Set variables with your own values
$invitationId = "<invitationId>"
$location = "<location>"

#List invitations sent to a consumer
Get-AzDataShareInvitation

#Get a specific invitation sent to a consumer
Get-AzDataShareInvitation -location -invitationId 

```

## <a name="script-explanation"></a>Explicación del script

Este script usa los siguientes comandos: 

| Get-Help | Notas |
|---|---|
| [Get-AzDataShareInvitation](/powershell/module/az.datashare/get-azdatashareinvitation?view=azps-2.6.0) | Obtiene y enumera las invitaciones de recurso compartido de datos enviadas. |
|||

## <a name="next-steps"></a>Pasos siguientes

Para más información sobre Azure PowerShell, consulte la [documentación de Azure PowerShell](https://docs.microsoft.com/powershell/).

Encontrará más ejemplos de scripts de PowerShell para Azure Data Share en el artículo [Ejemplos de PowerShell para Azure Data Share](../../samples-powershell.md).
