---
title: 'Script de PowerShell: Enumeración de los recursos compartidos existentes en Azure Data Share'
description: Este script de PowerShell enumera y muestra los detalles de los recursos compartidos.
services: data-share
author: joannapea
ms.author: joanpo
ms.service: data-share
ms.topic: article
ms.date: 07/07/2019
ms.openlocfilehash: 54b4b6cedc2dc09e4a9d5c4b7b937e80750e17c6
ms.sourcegitcommit: 8d8deb9a406165de5050522681b782fb2917762d
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/20/2020
ms.locfileid: "92220829"
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
| [Get-AzDataShare](/powershell/module/az.datashare/get-azdatashare) | Obtiene y enumera las listas de recursos compartidos de una cuenta. |
|||

## <a name="next-steps"></a>Pasos siguientes

Para más información sobre Azure PowerShell, consulte la [documentación de Azure PowerShell](/powershell/).

Encontrará más ejemplos de scripts de PowerShell para Azure Data Share en el artículo [Ejemplos de PowerShell para Azure Data Share](../../samples-powershell.md).
