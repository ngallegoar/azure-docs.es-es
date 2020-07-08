---
title: Configuración posterior a la implementación mediante el uso de extensiones
description: Aprenda cómo usar las extensiones de plantillas de Azure Resource Manager para proporcionar configuraciones posteriores a la implementación.
author: mumian
ms.topic: conceptual
ms.date: 12/14/2018
ms.author: jgao
ms.openlocfilehash: be55138a2aa6dc0552c7556438ffd43705687c87
ms.sourcegitcommit: bcb962e74ee5302d0b9242b1ee006f769a94cfb8
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 07/07/2020
ms.locfileid: "86055058"
---
# <a name="provide-post-deployment-configurations-by-using-extensions"></a>Suministro de configuraciones posteriores a la implementación mediante el uso de extensiones en Azure

Las extensiones de plantillas son aplicaciones pequeñas que realizan tareas de automatización y configuración posterior a la implementación en recursos de Azure. El más popular son las extensiones de máquina virtual. Consulte [Características y extensiones de las máquinas virtuales para Windows](../../virtual-machines/extensions/features-windows.md) o [Características y extensiones de las máquinas virtuales para Linux](../../virtual-machines/extensions/features-linux.md).

## <a name="extensions"></a>Extensiones

Las extensiones existentes son:

- [Microsoft.Compute/virtualMachines/extensions](/azure/templates/microsoft.compute/2018-10-01/virtualmachines/extensions)
- [Microsoft.Compute virtualMachineScaleSets/extensions](/azure/templates/microsoft.compute/2018-10-01/virtualmachinescalesets/extensions)
- [Microsoft.HDInsight clusters/extensions](/azure/templates/microsoft.hdinsight/2018-06-01-preview/clusters)
- [Microsoft.Sql servers/databases/extensions](/azure/templates/microsoft.sql/2014-04-01/servers/databases/extensions) 
- [Microsoft.Web/sites/siteextensions](/azure/templates/microsoft.web/2016-08-01/sites/siteextensions)

Para averiguar las extensiones disponibles, vaya a la [referencia de plantilla](/azure/templates/). En **Filtrar por título**, escriba **extensión**.

Para aprender cómo usar estas extensiones, consulte:

- [Tutorial: Implementación de extensiones de máquina virtual con plantillas de Azure Resource Manager](template-tutorial-deploy-vm-extensions.md).
- [Tutorial: Importación de archivos BACPAC de SQL con plantillas de Azure Resource Manager](template-tutorial-deploy-sql-extensions-bacpac.md)

## <a name="next-steps"></a>Pasos siguientes

> [!div class="nextstepaction"]
> [Tutorial: Implementación de extensiones de máquina virtual con plantillas de Azure Resource Manager](template-tutorial-deploy-vm-extensions.md)
