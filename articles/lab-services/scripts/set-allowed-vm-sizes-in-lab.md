---
title: 'Script de PowerShell: establecimiento de los tamaños de máquina virtual permitidos en Azure Lab Services | Microsoft Docs'
description: Este artículo incluye un script de PowerShell de ejemplo que establece los tamaños de máquina virtual en Azure Lab Services.
services: lab-services
author: spelluru
manager: ''
editor: ''
ms.service: lab-services
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/24/2020
ms.author: spelluru
ms.openlocfilehash: a1b0e9a4aed475f04ec8dcffa9bc95b7c7c713e1
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/27/2020
ms.locfileid: "76760475"
---
# <a name="use-powershell-to-set-allowed-vm-sizes-in-azure-lab-services"></a>Uso de PowerShell para establecer los tamaños de máquina virtual permitidos en Azure Lab Services

Este script de PowerShell de ejemplo establece los tamaños de máquina virtual en Azure Lab Services.

[!INCLUDE [updated-for-az](../../../includes/updated-for-az.md)]

[!INCLUDE [sample-powershell-install](../../../includes/sample-powershell-install-no-ssh-az.md)]

## <a name="prerequisites"></a>Prerequisites
* **Un laboratorio**. Este script requiere que disponga de un laboratorio existente. 

## <a name="sample-script"></a>Script de ejemplo

[!code-powershell[main](../../../powershell_scripts/devtest-lab/set-allowed-vm-sizes-in-lab/set-allowed-vm-sizes-in-lab.ps1 "Add external user to a lab")]

## <a name="script-explanation"></a>Explicación del script

Este script usa los siguientes comandos: 

| Get-Help | Notas |
|---|---|
| Find-AzResource | Busca recursos en función de los parámetros especificados. |
| [Get-AzResource](/powershell/module/az.resources/get-azresource) | Obtiene recursos. |
| [Set-AzResource](/powershell/module/az.resources/set-azresource) | Modifica un recurso. |
| [New-AzResource](/powershell/module/az.resources/new-azresource) | Crea un recurso. |

## <a name="next-steps"></a>Pasos siguientes

Para más información sobre Azure PowerShell, consulte la [documentación de Azure PowerShell](https://docs.microsoft.com/powershell/).

Encontrará más ejemplos de script de PowerShell de Azure Lab Services en los [ejemplos de PowerShell de Azure Lab Services](../samples-powershell.md).
