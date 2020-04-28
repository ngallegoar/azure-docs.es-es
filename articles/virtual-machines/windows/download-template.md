---
title: Descarga de la plantilla para una VM de Azure
description: Descargue la plantilla para una máquina virtual mediante el portal o PowerShell.
author: cynthn
manager: gwallace
ms.service: virtual-machines
ms.workload: infrastructure-services
ms.topic: article
ms.date: 11/17/2017
ms.author: cynthn
ms.openlocfilehash: af6905f0ba62a9053e44134348721312ade6b9d7
ms.sourcegitcommit: 09a124d851fbbab7bc0b14efd6ef4e0275c7ee88
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 04/23/2020
ms.locfileid: "82085389"
---
# <a name="download-the-template-for-a-vm"></a>Descargar la plantilla para una máquina virtual
Cuando se crea una máquina virtual en Azure mediante el portal o PowerShell, se crea automáticamente una plantilla de Resource Manager. Puede usar esta plantilla para duplicar rápidamente una implementación. La plantilla contiene información acerca de todos los recursos de un grupo de recursos. En el caso de una máquina virtual, significa que la plantilla contiene todo lo que se crea para ayudar a la máquina virtual de ese grupo de recursos, incluidos los recursos de red.

## <a name="download-the-template-using-the-portal"></a>Descarga de la plantilla mediante el portal
1. Inicie sesión en [Azure Portal](https://portal.azure.com/).
2. En el menú de la izquierda, seleccione **Máquinas virtuales**.
3. Seleccione la máquina virtual en la lista.
4. Seleccione **Exportar plantilla**.
5. Seleccione **Descargar** en el menú de la parte superior y guarde el archivo .zip en el equipo local.
6. Abra el archivo .zip y extraiga los archivos en una carpeta. El archivo .zip contiene:
   
   * parameters.json
   * template.json

El archivo template.json es la plantilla.

## <a name="download-the-template-using-powershell"></a>Descarga de la plantilla mediante PowerShell
También puede descargar el archivo de plantilla .json mediante el cmdlet [Export-AzResourceGroup](https://docs.microsoft.com/powershell/module/az.resources/export-azresourcegroup). Puede usar el parámetro `-path` para proporcionar el nombre de archivo y la ruta de acceso del archivo .json. En este ejemplo se muestra cómo descargar la plantilla para el grupo de recursos denominado **myResourceGroup** en la carpeta **C:\users\public\downloads** del equipo local.

```powershell
    Export-AzResourceGroup -ResourceGroupName "myResourceGroup" -Path "C:\users\public\downloads"
```

## <a name="next-steps"></a>Pasos siguientes
Para más información sobre la implementación de recursos mediante plantillas, consulte el [tutorial de plantillas de Resource Manager](../../azure-resource-manager/resource-manager-template-walkthrough.md).

