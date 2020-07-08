---
title: Módulo de PowerShell para Azure Lab Services | Microsoft Docs
description: En este artículo se proporciona información sobre un módulo de PowerShell que ayuda a administrar artefactos en Azure Lab Services.
ms.topic: article
ms.date: 06/26/2020
ms.openlocfilehash: d9c184f3917be378eb77d1bf4096bfebb5ee1884
ms.sourcegitcommit: 877491bd46921c11dd478bd25fc718ceee2dcc08
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 07/02/2020
ms.locfileid: "85445600"
---
# <a name="azlabservices-powershell-module-preview"></a>Módulo Az.LabServices de PowerShell (versión preliminar)
Az.LabServices es un módulo de PowerShell que simplifica la administración de Azure Lab Services. Proporciona funciones que admiten composición para crear, consultar, actualizar y eliminar cuentas de laboratorio, laboratorios, máquinas virtuales e imágenes. Para obtener más información sobre este módulo, vea la [Página principal de Az.LabServices en GitHub](https://github.com/Azure/azure-devtestlab/tree/master/samples/ClassroomLabs/Modules/Library).

> [!NOTE]
> Este módulo se encuentra en **versión preliminar**. 

## <a name="example-command"></a>Comando de ejemplo
Este es un ejemplo del uso de un comando de PowerShell que sirve para detener todas las máquinas virtuales en ejecución en todos los laboratorios.

```powershell
Get-AzLabAccount | Get-AzLab | Get-AzLabVm -Status Running | Stop-AzLabVm
```

## <a name="get-started"></a>Introducción
1. Instale [Azure PowerShell](https://docs.microsoft.com/powershell/azure/overview) si no existe en la máquina. 
2. Descargue [Az.LabServices.psm1](https://github.com/Azure/azure-devtestlab/blob/master/samples/ClassroomLabs/Modules/Library/Az.LabServices.psm1) en la máquina.
3. Importe el módulo:

    ```powershell
    Import-Module .\Az.LabServices.psm1
    ```
4. Ejecute el comando siguiente para enumerar los laboratorios de la suscripción.

    ```powershell
    Get-AzLabAccount | Get-AzLab
    ```

## <a name="next-steps"></a>Pasos siguientes
Vea la [Página principal de Az.LabServices en GitHub](https://github.com/Azure/azure-devtestlab/tree/master/samples/ClassroomLabs/Modules/Library).
