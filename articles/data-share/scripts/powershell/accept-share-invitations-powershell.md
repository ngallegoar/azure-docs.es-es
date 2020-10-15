---
title: 'Script de PowerShell: Aceptar una invitación de una instancia de Azure Data Share| Microsoft Docs'
description: Este script de PowerShell acepta invitaciones de un recurso compartido de datos existente.
services: data-share
author: joannapea
ms.service: data-share
ms.workload: data-services
ms.tgt_pltfrm: na
ms.topic: article
ms.date: 07/07/2019
ms.author: joanpo
ms.openlocfilehash: 246e3550650dfd458b4aeecda3b5b7733e49f017
ms.sourcegitcommit: 829d951d5c90442a38012daaf77e86046018e5b9
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/09/2020
ms.locfileid: "70307328"
---
# <a name="use-powershell-to-accept-a-data-share-invitation"></a>Uso de PowerShell para aceptar una invitación de un recurso compartido de datos

Este script de PowerShell acepta las invitaciones enviadas a un consumidor.

## <a name="sample-script"></a>Script de ejemplo
```powershell
#List invitations sent to a consumer
Get-AzDataShareInvitation

# Set variables with your own values
$resourceGroupName = "<Resource group name>"
$dataShareAccountName = "<Data share account name>"
$dataShareName = "<Data share name>"
$invitationId = "<Invitation id>"

#Accept a specific invitation by creating a share subscription
New-AzDataShareSubscription -ResourceGroupName $resourceGroupName -AccountName $dataShareAccountName -Name $dataShareName -InvitationId $invitationId

```

## <a name="script-explanation"></a>Explicación del script

Este script usa los siguientes comandos: 

| Get-Help | Notas |
|---|---|
| [Get-AzDataShareInvitation](/powershell/module/az.datashare/get-azdatashareinvitation?view=azps-2.6.0) | Obtiene y enumera las invitaciones de recurso compartido de datos enviadas. |
| [New-AzDataShareSubscription](/powershell/module/az.datashare/get-azdatasharesubscription?view=azps-2.6.0) | Cree una suscripción de un recurso compartido de datos. |
|||

## <a name="next-steps"></a>Pasos siguientes

Para más información sobre Azure PowerShell, consulte la [documentación de Azure PowerShell](https://docs.microsoft.com/powershell/).

Encontrará más ejemplos de scripts de PowerShell para Azure Data Share en el artículo [Ejemplos de PowerShell para Azure Data Share](../../samples-powershell.md).

