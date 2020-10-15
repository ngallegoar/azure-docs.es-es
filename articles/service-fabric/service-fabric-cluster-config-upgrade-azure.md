---
title: Actualización de la configuración de un clúster de Azure Service Fabric
description: Obtenga información acerca de cómo actualizar la configuración que ejecuta un clúster de Service Fabric en Azure con una plantilla de Resource Manager.
ms.topic: conceptual
ms.date: 11/09/2018
ms.openlocfilehash: 692dc2162159ab61a3ac527e12dac43438084a60
ms.sourcegitcommit: 829d951d5c90442a38012daaf77e86046018e5b9
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/09/2020
ms.locfileid: "91842723"
---
# <a name="upgrade-the-configuration-of-a-cluster-in-azure"></a>Actualización de la configuración de un clúster de Azure 

En este documento se explica cómo personalizar las diversas opciones de configuración para el clúster de Service Fabric. Para clústeres hospedados en Azure, puede personalizar la configuración en [Azure Portal](https://portal.azure.com) o mediante una plantilla de Azure Resource Manager.

> [!NOTE]
> No todas las configuraciones están disponibles en el portal, y es un [procedimiento recomendado personalizarlo mediante una plantilla de Azure Resource Manager](./service-fabric-best-practices-infrastructure-as-code.md); el portal es solo para el escenario de desarrollo/pruebas de Service Fabric.
> 


[!INCLUDE [updated-for-az](../../includes/updated-for-az.md)]

## <a name="customize-cluster-settings-using-resource-manager-templates"></a>Personalización de la configuración de clústeres mediante plantillas de Resource Manager
Los clústeres de Azure pueden configurarse a través de la plantilla JSON de Resource Manager. Para obtener más información acerca de las distintas opciones de configuración, vea [Configuration settings for clusters](service-fabric-cluster-fabric-settings.md) (Opciones de configuración para clústeres). Como ejemplo, en los pasos siguientes se muestra cómo agregar un nuevo valor *MaxDiskQuotaInMB* a la sección *Diagnostics* con Azure Resource Explorer.

1. Vaya a https://resources.azure.com.
2. Vaya a la suscripción; para ello, expanda **subscriptions** ->  **\<Your Subscription>**  -> **resourceGroups** ->  **\<Your Resource Group>**  -> **providers** -> **Microsoft.ServiceFabric** -> **clusters** ->  **\<Your Cluster Name>**
3. En la esquina superior derecha, seleccione **Read/Write** (Lectura y escritura).
4. Seleccione **Edit** (Editar), actualice el elemento JSON `fabricSettings` y agregue un nuevo elemento:

```json
      {
        "name": "Diagnostics",
        "parameters": [
          {
            "name": "MaxDiskQuotaInMB",
            "value": "65536"
          }
        ]
      }
```

También puede personalizar la configuración del clúster de una de las siguientes maneras con Azure Resource Manager:

- Use [Azure Portal](../azure-resource-manager/templates/export-template-portal.md) para exportar y actualizar la plantilla de Resource Manager.
- Use [PowerShell](../azure-resource-manager/management/manage-resources-powershell.md) para exportar y actualizar la plantilla de Resource Manager.
- Use la [CLI de Azure](../azure-resource-manager/management/manage-resources-cli.md) para exportar y actualizar la plantilla de Resource Manager.
- Use los comandos [Set-AzServiceFabricSetting](/powershell/module/az.servicefabric/set-azservicefabricsetting) y [Remove-AzServiceFabricSetting](/powershell/module/az.servicefabric/remove-azservicefabricsetting) de Azure PowerShell para modificar la configuración directamente.
- Use el comando [az sf cluster setting](/cli/azure/sf/cluster/setting) de la CLI de Azure para modificar la configuración directamente.

## <a name="next-steps"></a>Pasos siguientes
* Obtenga información sobre la [configuración de un clúster de Service Fabric](service-fabric-cluster-fabric-settings.md).
* Obtenga información sobre cómo [escalar o reducir horizontalmente el clúster](service-fabric-cluster-scale-in-out.md).
