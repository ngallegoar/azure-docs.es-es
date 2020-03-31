---
title: 'Actualizaciones de servicio para los grupos de host de Windows Virtual Desktop: Azure'
description: Cómo crear un grupo host de validación para supervisar actualizaciones de servicio antes de implementar las actualizaciones en producción.
services: virtual-desktop
author: Heidilohr
ms.service: virtual-desktop
ms.topic: tutorial
ms.date: 03/13/2020
ms.author: helohr
manager: lizross
ms.openlocfilehash: f2b51213dfc6d7e55f76e78b92d12111f84736be
ms.sourcegitcommit: 0947111b263015136bca0e6ec5a8c570b3f700ff
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/24/2020
ms.locfileid: "79365396"
---
# <a name="tutorial-create-a-host-pool-to-validate-service-updates"></a>Tutorial: Creación de un grupo host para validar las actualizaciones del servicio

Los grupos hosts son una colección de una o más máquinas virtuales idénticas en entornos de inquilino de Windows Virtual Desktop. Antes de implementar grupos host en el entorno de producción, se recomienda encarecidamente crear un grupo host de validación. Primero, las actualizaciones se aplican a los grupos host de validación, lo que permite supervisar las actualizaciones de servicio antes de implementarlas en el entorno de producción. Sin un grupo host de validación, es posible que no se detecten cambios que introducen errores, lo que podría dar lugar a tiempos de inactividad para los usuarios en el entorno de producción.

Para garantizar que las aplicaciones funcionan con las actualizaciones más recientes, el grupo host de validación debe ser lo más parecido posible a los grupos host del entorno de producción. Los usuarios deben conectarse al grupo host de validación con la misma frecuencia que lo hacen al grupo host de producción. Si ha automatizado las pruebas en su grupo host, debe incluir las pruebas automatizadas en el grupo host de validación.

Puede depurar incidencias en el grupo host de validación con [la característica de diagnóstico](diagnostics-role-service.md) o con [artículos de solución de problemas de Windows Virtual Desktop](troubleshoot-set-up-overview.md).

>[!NOTE]
> Se recomienda que mantenga el grupo host de validación para probar todas las actualizaciones futuras.

Antes de empezar, y si aún no lo ha hecho, [descargue e importe el módulo de PowerShell para Windows Virtual Desktop](/powershell/windows-virtual-desktop/overview/). Después, ejecute el siguiente cmdlet para iniciar sesión en su cuenta:

```powershell
Add-RdsAccount -DeploymentUrl "https://rdbroker.wvd.microsoft.com"
```

## <a name="create-your-host-pool"></a>Creación del grupo host

Puede crear un grupo host siguiendo las instrucciones de cualquiera de estos artículos:
- [Tutorial: Creación de un grupo host con Azure Marketplace](create-host-pools-azure-marketplace.md)
- [Creación de un grupo host con una plantilla de Azure Resource Manager](create-host-pools-arm-template.md)
- [Creación de un grupo host con PowerShell](create-host-pools-powershell.md)

## <a name="define-your-host-pool-as-a-validation-host-pool"></a>Definición del grupo host como grupo host de validación

Ejecute los siguientes cmdlets de PowerShell para definir el nuevo grupo host como grupo host de validación. Reemplace los valores entre comillas por los valores pertinentes para su sesión:

```powershell
Add-RdsAccount -DeploymentUrl "https://rdbroker.wvd.microsoft.com"
Set-RdsHostPool -TenantName $myTenantName -Name "contosoHostPool" -ValidationEnv $true
```

Ejecute el siguiente cmdlet de PowerShell para confirmar que la propiedad de validación se ha definido. Reemplace los valores entre comillas por los valores pertinentes para su sesión.

```powershell
Get-RdsHostPool -TenantName $myTenantName -Name "contosoHostPool"
```

Los resultados del cmdlet deben ser similares a estos:

```
    TenantName          : contoso 
    TenantGroupName     : Default Tenant Group
    HostPoolName        : contosoHostPool
    FriendlyName        :
    Description         :
    Persistent          : False 
    CustomRdpProperty    : use multimon:i:0;
    MaxSessionLimit     : 10
    LoadBalancerType    : BreadthFirst
    ValidationEnv       : True
    Ring                :
```

## <a name="update-schedule"></a>Programación de actualizaciones

Las actualizaciones del servicio se realizan mensualmente. Si hay incidencias importantes, se proporcionarán actualizaciones críticas con mayor frecuencia.

## <a name="next-steps"></a>Pasos siguientes

Ahora que ha creado un grupo de hosts de validación, puede obtener información sobre cómo usar Azure Service Health para supervisar la implementación de Windows Virtual Desktop. 

> [!div class="nextstepaction"]
> [Configuración de alertas de servicio](./set-up-service-alerts.md)
