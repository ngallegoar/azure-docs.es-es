---
title: Azure Cloud Shell para usuarios de Windows | Microsoft Docs
description: Guía para los usuarios que no están familiarizados con los sistemas Linux
services: azure
documentationcenter: ''
author: maertendMSFT
manager: hemantm
tags: azure-resource-manager
ms.assetid: ''
ms.service: azure
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm-linux
ms.devlang: na
ms.topic: article
ms.date: 08/03/2018
ms.author: damaerte
ms.openlocfilehash: 4fc4f6523eb19294cabdf6b5b910dd346a877502
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/27/2020
ms.locfileid: "67204135"
---
# <a name="powershell-in-azure-cloud-shell-for-windows-users"></a>PowerShell en Azure Cloud Shell para usuarios de Windows

En mayo de 2018, se [anunciaron](https://azure.microsoft.com/blog/pscloudshellrefresh/) cambios en PowerShell en Azure Cloud Shell.
La experiencia de PowerShell en Azure Cloud Shell se llama ahora [PowerShell Core 6](https://github.com/powershell/powershell) en un entorno Linux.
Con este cambio, puede haber algunas diferencias en la experiencia de PowerShell en Cloud Shell en comparación con lo que se espera en Windows PowerShell.

## <a name="file-system-case-sensitivity"></a>Mayúsculas y minúsculas en el sistema de archivos

El sistema de archivos no distingue mayúsculas de minúsculas en Windows, mientras que, en Linux, si las distingue.
Antes, `file.txt` y `FILE.txt` se consideraban el mismo archivo, pero ahora se consideran diferentes.
Se deben usar las mayúsculas y minúsculas correctas con `tab-completing` en el sistema de archivos.
En experiencias de PowerShell específicas, por ejemplo en los nombres de cmdlets de `tab-completing`, en los parámetros y en los valores, no se distinguen mayúsculas de minúsculas.

## <a name="windows-powershell-aliases-vs-linux-utilities"></a>Alias de Windows PowerShell en comparación con utilidades de Linux

Algunos alias existentes de PowerShell tienen los mismos nombres que los comandos de Linux integrados, como `cat`, `ls`, `sort`, `sleep`, etc. En PowerShell Core 6, se han quitado los alias que entran en conflicto con los comandos de Linux integrados.
A continuación se muestran los alias comunes que se han quitado, así como sus comandos equivalentes:  

|Alias eliminado   |Comando equivalente   |
|---|---|
|`cat`    | `Get-Content` |
|`curl`   | `Invoke-WebRequest` |
|`diff`   | `Compare-Object` |
|`ls`     | `dir` <br> `Get-ChildItem` |
|`mv`     | `Move-Item`   |
|`rm`     | `Remove-Item` |
|`sleep`  | `Start-Sleep` |
|`sort`   | `Sort-Object` |
|`wget`   | `Invoke-WebRequest` |

## <a name="persisting-home"></a>$HOME que persiste

Los usuarios anteriores solo podían conservar los scripts y otros archivos en su unidad en la nube.
Ahora, el directorio $HOME del usuario también se conserva entre sesiones.

## <a name="powershell-profile"></a>Perfil de PowerShell

De forma predeterminada, no se crea el perfil de PowerShell de un usuario.
Para crear el perfil, cree un directorio `PowerShell` en `$HOME/.config`.

```azurepowershell-interactive
mkdir (Split-Path $profile.CurrentUserAllHosts)
```

En `$HOME/.config/PowerShell`, puede crear los archivos del perfil `profile.ps1` y `Microsoft.PowerShell_profile.ps1`.

## <a name="whats-new-in-powershell-core-6"></a>Novedades de PowerShell Core 6

Para más información acerca de cuáles son las novedades de PowerShell Core 6, consulte los [Documentos de PowerShell](https://docs.microsoft.com/powershell/scripting/whats-new/what-s-new-in-powershell-core-60?view=powershell-6) y la entrada de blog [Introducción a PowerShell Core](https://blogs.msdn.microsoft.com/powershell/2017/06/09/getting-started-with-powershell-core-on-windows-mac-and-linux/).
