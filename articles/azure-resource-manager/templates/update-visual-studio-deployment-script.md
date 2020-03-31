---
title: Actualización del script de implementación de plantillas de Visual Studio para usar Az PowerShell
description: Actualización del script de implementación de plantillas de Visual Studio de AzureRM a Az PowerShell
author: cweining
ms.topic: conceptual
ms.date: 01/31/2020
ms.author: cweining
ms.openlocfilehash: 357e0289f3237ed32b0801280316225ba5530282
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/27/2020
ms.locfileid: "76963876"
---
# <a name="update-visual-studio-template-deployment-script-to-use-az-powershell-module"></a>Actualización del script de implementación de plantillas de Visual Studio para usar el módulo Az PowerShell

Visual Studio 16.4 admite el uso de Az PowerShell en el script de implementación de plantillas. Sin embargo, Visual Studio no instala automáticamente ese módulo. Para usar el módulo Az, debe realizar cuatro pasos:

1. [Desinstalación del módulo AzureRM](/powershell/azure/uninstall-az-ps#uninstall-the-azurerm-module)
1. [Instalación del módulo Az](/powershell/azure/install-az-ps)
1. Actualización de Visual Studio a la versión 16.4
1. Actualice el script de implementación en el proyecto.

## <a name="update-visual-studio-to-164"></a>Actualización de Visual Studio a la versión 16.4

Actualice la instalación de Visual Studio a la versión 16.4 o posterior. Durante la actualización, asegúrese de que el componente Azure PowerShell no esté activado. Como ha instalado el módulo Az usando la galería, no querrá volver a instalar el módulo AzureRM.

Si ya ha actualizado a la versión 16.4 y el componente Azure PowerShell estaba seleccionado, puede desinstalarlo ejecutando Instalador de Visual Studio. No seleccione el componente Azure PowerShell en la carga de trabajo de Azure ni en la página de componentes individuales.

## <a name="update-the-deployment-script-in-your-project"></a>Actualización del script de implementación en el proyecto

Reemplace todas las apariciones de la cadena "AzureRm" por "Az" en el script de implementación. Puede hacer referencia a las revisiones de este [gist](https://gist.github.com/cweining/d2da2479418ea403499c4306dcf4f619) para ver los cambios. Para más información sobre la actualización de scripts al módulo Az, consulte [Migración de Azure PowerShell desde AzureRM a Az](/powershell/azure/migrate-from-azurerm-to-az).

## <a name="next-steps"></a>Pasos siguientes

Para más información sobre el uso de proyectos de Visual Studio, consulte [Creación e implementación de grupos de recursos de Azure mediante Visual Studio](create-visual-studio-deployment-project.md).
