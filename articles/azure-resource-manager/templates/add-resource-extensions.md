---
title: Configuración posterior a la implementación mediante el uso de extensiones
description: Aprenda cómo usar las extensiones de plantillas de Azure Resource Manager para proporcionar configuraciones posteriores a la implementación.
author: mumian
ms.topic: conceptual
ms.date: 12/14/2018
ms.author: jgao
ms.openlocfilehash: b3c4110c8761b3e8daf324d65ac7fa1dcbcdf61f
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/27/2020
ms.locfileid: "77023504"
---
# <a name="provide-post-deployment-configurations-by-using-extensions"></a>Suministro de configuraciones posteriores a la implementación mediante el uso de extensiones en Azure

Las extensiones de plantillas son aplicaciones pequeñas que realizan tareas de automatización y configuración posterior a la implementación en recursos de Azure. El más popular son las extensiones de máquina virtual. Consulte [Características y extensiones de las máquinas virtuales para Windows](../../virtual-machines/extensions/features-windows.md) o [Características y extensiones de las máquinas virtuales para Linux](../../virtual-machines/extensions/features-linux.md).

## <a name="extensions"></a>Extensiones

Las extensiones existentes son:

- [Microsoft.Compute/virtualMachines/extensions](https://docs.microsoft.com/azure/templates/microsoft.compute/2018-10-01/virtualmachines/extensions)
- [Microsoft.Compute virtualMachineScaleSets/extensions](https://docs.microsoft.com/azure/templates/microsoft.compute/2018-10-01/virtualmachinescalesets/extensions)
- [Microsoft.HDInsight clusters/extensions](https://docs.microsoft.com/azure/templates/microsoft.hdinsight/2018-06-01-preview/clusters)
- [Microsoft.Sql servers/databases/extensions](https://docs.microsoft.com/azure/templates/microsoft.sql/2014-04-01/servers/databases/extensions) 
- [Microsoft.Web/sites/siteextensions](https://docs.microsoft.com/azure/templates/microsoft.web/2016-08-01/sites/siteextensions)

Para averiguar las extensiones disponibles, vaya a la [referencia de plantilla](https://docs.microsoft.com/azure/templates/). En **Filtrar por título**, escriba **extensión**.

Para aprender cómo usar estas extensiones, consulte:

- [Tutorial: Implementación de extensiones de máquina virtual con plantillas de Azure Resource Manager](template-tutorial-deploy-vm-extensions.md).
- [Tutorial: Importación de archivos BACPAC de SQL con plantillas de Azure Resource Manager](template-tutorial-deploy-sql-extensions-bacpac.md)

## <a name="next-steps"></a>Pasos siguientes

> [!div class="nextstepaction"]
> [Tutorial: Implementación de extensiones de máquina virtual con plantillas de Azure Resource Manager](template-tutorial-deploy-vm-extensions.md)
