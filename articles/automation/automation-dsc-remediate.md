---
title: Corrección de servidores de Azure Automation State Configuration no conformes
description: Cómo volver a aplicar las configuraciones a petición en los servidores donde ha variado el estado de configuración
services: automation
ms.service: automation
ms.subservice: dsc
author: mgreenegit
ms.author: migreene
ms.topic: conceptual
ms.date: 07/17/2019
manager: nirb
ms.openlocfilehash: cc5884e1f70bdccee4e7a113e6e3ee2d6604b50a
ms.sourcegitcommit: b80aafd2c71d7366838811e92bd234ddbab507b6
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 04/16/2020
ms.locfileid: "81406080"
---
# <a name="remediate-noncompliant-dsc-servers"></a>Corrección de servidores DSC no conformes

Cuando los servidores se registran con Azure Automation State Configuration, el modo de configuración se establece en `ApplyOnly`, `ApplyandMonitor`o `ApplyAndAutoCorrect`. Si el modo no está establecido en `ApplyAndAutoCorrect`, los servidores que dejan de ser conformes permanecen en ese estado de no conformidad hasta que se corrigen manualmente.

Azure Compute ofrece una característica denominada "Ejecutar comando" que permite a los clientes ejecutar scripts dentro de las máquinas virtuales.
En este documento se proporcionan scripts de ejemplo de esta característica que se pueden aplicar para corregir manualmente la variación de la configuración.

## <a name="correct-drift-of-windows-virtual-machines-using-powershell"></a>Corrección de variación en máquinas virtuales Windows con PowerShell

Si necesita instrucciones detalladas acerca de cómo usar la característica "Comando de ejecución" en máquinas virtuales Windows, consulte la página de documentación [Ejecución de scripts de PowerShell en una máquina virtual Windows con el comando Ejecutar](/azure/virtual-machines/windows/run-command).

Para hacer que un nodo de Azure Automation State Configuration descargue la configuración más reciente y la aplique, use el cmdlet [Update-DscConfiguration](/powershell/module/psdesiredstateconfiguration/update-dscconfiguration).

```powershell
Update-DscConfiguration -Wait -Verbose
```

## <a name="correct-drift-of-linux-virtual-machines"></a>Corrección de variación en máquinas virtuales Linux

Actualmente, no existe una funcionalidad similar para los servidores Linux.
La única opción es repetir el proceso de registro.

En el caso de los nodos de Azure, puede corregir esta variación en Azure Portal o mediante los cmdlets del módulo Az. Para más información sobre este proceso, consulte [Incorporación de máquinas para realizar la administración con Automation State Configuration](automation-dsc-onboarding.md#onboard-vms-by-using-the-azure-portal).
En el caso de los nodos híbridos, puede corregir esta variación con los scripts de Python incluidos.
Consulte el [repositorio de PowerShell DSC para Linux](https://github.com/Microsoft/PowerShell-DSC-for-Linux#performing-dsc-operations-from-the-linux-computer).

## <a name="next-steps"></a>Pasos siguientes

- Para ver una referencia de los cmdlets de PowerShell, consulte [Az.Automation](https://docs.microsoft.com/powershell/module/az.automation/?view=azps-3.7.0#automation
).
- Para ver un ejemplo del uso de State Configuration de Azure Automation en una canalización de implementación continua, consulte [Implementación continua mediante State Configuration de Azure Automation y Chocolatey](automation-dsc-cd-chocolatey.md).