---
title: 'Script de PowerShell: Lista de recursos compartidos existentes en Azure Data Share | Microsoft Docs'
description: Este script de PowerShell enumera y muestra los detalles de los recursos compartidos.
services: data-share
author: joannapea
ms.service: data-share
ms.workload: data-services
ms.tgt_pltfrm: na
ms.topic: article
ms.date: 07/07/2019
ms.author: joanpo
ms.openlocfilehash: 6314bd348c22c901001b88eda6875181a2f69df4
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/27/2020
ms.locfileid: "70307138"
---
# <a name="use-powershell-to-view-the-details-of-a-sent-data-share"></a>Usar PowerShell para ver los detalles de un recurso compartido de datos enviado

Este script de PowerShell enumera los recursos compartidos de datos de una cuenta existente y obtiene los detalles de un recurso compartido específico.


## <a name="sample-script"></a>Script de ejemplo

```powershell

# Set variables with your own values
$resourceGroupName = "<Resource group name>"
$dataShareAccountName = "<Data share account name>"
$dataShareName = "<Data share name>"

#Lists all data shares within an account
Get-AzDataShare -ResourceGroupName $resourceGroupName -AccountName $dataShareAccountName

#Gets details of a specific data share
Get-AzDataShare -ResourceGroupName $resourceGroupName -AccountName $dataShareAccountName -Name $dataShareName

```


## <a name="script-explanation"></a>Explicación del script

Este script usa los siguientes comandos: 

| Get-Help | Notas |
|---|---|
| [Get-AzDataShare](/powershell/module/az.datashare/get-azdatashare?view=azps-2.6.0) | Obtiene y enumera las listas de recursos compartidos de una cuenta. |
|||

## <a name="next-steps"></a>Pasos siguientes

Para más información sobre Azure PowerShell, consulte la [documentación de Azure PowerShell](https://docs.microsoft.com/powershell/).

Encontrará más ejemplos de scripts de PowerShell para Azure Data Share en el artículo [Ejemplos de PowerShell para Azure Data Share](../../samples-powershell.md).
