---
title: Corrección de servidores de Azure Automation State Configuration
description: Cómo volver a aplicar las configuraciones a petición en los servidores donde ha variado el estado de configuración
services: automation
ms.service: automation
ms.subservice: dsc
author: mgreenegit
ms.author: migreene
ms.topic: conceptual
ms.date: 07/17/2019
manager: nirb
ms.openlocfilehash: f4ca76f4be9d00e185f8774fc33296d1af1aeece
ms.sourcegitcommit: 3c318f6c2a46e0d062a725d88cc8eb2d3fa2f96a
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 04/02/2020
ms.locfileid: "80585498"
---
# <a name="remediate-non-compliant-dsc-servers"></a>Corrección de servidores DSC no compatibles

Al registrar los servidores con Azure Automation State Configuration, el valor de "Modo de configuración" se establece en ApplyOnly, ApplyandMonitor o ApplyAndAutoCorrect.
Si el modo no está establecido en AutoCorrect, los servidores que, por cualquier motivo, dejan de estar en un estado compatible deberán corregirse manualmente para que vuelvan a un estado compatible.

Azure Compute ofrece una característica denominada "Ejecutar comando" que permite a los clientes ejecutar scripts dentro de las máquinas virtuales.
En este documento se proporcionan scripts de ejemplo de esta característica que se pueden aplicar para corregir manualmente la variación de la configuración.

## <a name="correct-drift-of-windows-virtual-machines-using-powershell"></a>Corrección de variación en máquinas virtuales Windows con PowerShell

Para obtener instrucciones detalladas sobre cómo usar la característica "Ejecutar comando" en máquinas virtuales Windows, consulte la página de documentación [Ejecución de scripts de PowerShell en la máquina virtual Windows con el comando Ejecutar](/azure/virtual-machines/windows/run-command).

Para hacer que un nodo de Azure Automation State Configuration descargue la configuración más reciente y la aplique, use el cmdlet `Update-DscConfiguration`.
Para obtener más información, consulte la documentación del cmdlet [Update-DscConfiguration](/powershell/module/psdesiredstateconfiguration/update-dscconfiguration).

```powershell
Update-DscConfiguration -Wait -Verbose
```

## <a name="correct-drift-of-linux-virtual-machines"></a>Corrección de variación en máquinas virtuales Linux

Actualmente, no existe una funcionalidad similar para los servidores Linux.
La única opción es repetir el proceso de registro.
En los nodos de Azure, la corrección de la variación puede realizarse desde el portal o mediante los cmdlets Az Automation.
Para obtener más detalles sobre este proceso, consulte la página [Incorporación de máquinas para su administración mediante Azure Automation State Configuration](/azure/automation/automation-dsc-onboarding#onboard-a-vm-using-azure-portal).
En el caso de los nodos híbridos, la corrección de la variación puede realizarse con los scripts de Python que están incluidos.
Consulte la documentación del [repositorio PowerShell DSC para Linux](https://github.com/Microsoft/PowerShell-DSC-for-Linux#performing-dsc-operations-from-the-linux-computer).

## <a name="next-steps"></a>Pasos siguientes

- Para la referencia de cmdlets de PowerShell, consulte [cmdlets de Azure Automation State Configuration](/powershell/module/azurerm.automation/#automation)
- Para ver un ejemplo del uso de Azure Automation State Configuration en una canalización de implementación continua, consulte [Implementación continua mediante Azure Automation State Configuration y Chocolatey](automation-dsc-cd-chocolatey.md)
