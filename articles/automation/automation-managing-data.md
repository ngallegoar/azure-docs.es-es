---
title: Administración de datos de Azure Automation
description: En este artículo se describen conceptos de administración de datos en Azure Automation, incluidas la retención de datos y la copia de seguridad.
services: automation
ms.subservice: shared-capabilities
ms.date: 03/23/2020
ms.topic: conceptual
ms.openlocfilehash: de60ef31a39a698f9a797a5836546f9b75b67594
ms.sourcegitcommit: 0b80a5802343ea769a91f91a8cdbdf1b67a932d3
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 05/25/2020
ms.locfileid: "83835213"
---
# <a name="management-of-azure-automation-data"></a>Administración de datos de Azure Automation

Este artículo contiene varios temas para administrar datos en un entorno de Azure Automation.

## <a name="data-retention"></a>Retención de datos

Cuando elimina un recurso en Azure Automation, se conserva durante un número de días para fines de auditoría antes de su eliminación permanente. Durante este tiempo, no es posible ver ni usar el recurso. Esta directiva también se aplica a los recursos que pertenecen a una cuenta de Automation eliminada.

La tabla siguiente resume la directiva de retención para distintos recursos.

| data | Directiva |
|:--- |:--- |
| Cuentas |Una cuenta se elimina de manera permanente 30 días después de que un usuario la elimine. |
| Recursos |Un recurso se elimina de forma permanente 30 días después de que un usuario lo elimina o 30 días después de que un usuario elimina la cuenta que contiene el recurso. |
| Nodos de DSC |Un nodo de DSC se elimina de forma permanente 30 días después de que se anula el registro del nodo de la cuenta de Automation mediante Azure Portal o el cmdlet [Unregister-AzAutomationDscNode](https://docs.microsoft.com/powershell/module/az.automation/unregister-azautomationdscnode?view=azps-3.7.0) de Windows PowerShell. Los nodos también se eliminan de forma permanente 30 días después de que el usuario elimine la cuenta que contiene el nodo. |
| Trabajos |Un trabajo se elimina de forma permanente 30 días después de cualquier modificación, por ejemplo, después de que el trabajo se complete, detenga o suspenda. |
| Módulos |Un módulo se elimina de forma permanente 30 días después de que un usuario lo elimina o 30 días después de que un usuario elimina la cuenta que contiene el módulo. |
| Configuraciones de nodo y archivos MOF |La configuración de nodo anterior se elimina de forma permanente 30 días después de que se genere una nueva configuración. |
| Informes de nodo |Un informe de nodo se elimina de forma permanente 90 días después de que se genere un nuevo informe para ese nodo. |
| Runbooks |Un runbook se elimina de forma permanente 30 días después de que un usuario lo elimina o 30 días después de que un usuario elimina la cuenta que contiene el recurso. |

La directiva de retención se aplica a todos los usuarios y, por el momento, no se puede personalizar. Sin embargo, si tiene que conservar los datos durante un periodo de tiempo mayor, puede [reenviar los datos de trabajos de Azure Automation a los registros de Azure Monitor](automation-manage-send-joblogs-log-analytics.md).

## <a name="data-backup"></a>Copia de seguridad de datos

Cuando elimina una cuenta de Automation en Azure, se eliminan todos los objetos de esa cuenta. Entre los objetos se incluyen runbooks, módulos, configuraciones, valores, trabajos y recursos. No es posible recuperar los objetos una vez eliminada la cuenta. Puede usar la información siguiente para crear una copia de seguridad de los contenidos de la cuenta de Automation antes de eliminarla.

### <a name="runbooks"></a>Runbooks

Puede exportar los runbooks a archivos de script con Azure Portal o el cmdlet [Get-AzureAutomationRunbookDefinition](https://docs.microsoft.com/powershell/module/servicemanagement/azure/get-azureautomationrunbookdefinition) en Windows PowerShell. Puede importar estos archivos de script en otra cuenta de Automation, como se indica en [Administración de runbooks en Azure Automation](manage-runbooks.md).

### <a name="integration-modules"></a>Módulos de integración

No es posible exportar módulos de integración desde Azure Automation. Debe hacer que estén disponibles fuera de la cuenta de Automation.

### <a name="assets"></a>Recursos

No se pueden exportar recursos de Azure Automation: certificados, conexiones, credenciales, programaciones y variables. En su lugar, puede usar los cmdlets de Azure Portal y de Azure para anotar los detalles de estos recursos. Posteriormente, puede usar esos detalles para crear todos los recursos utilizados por los runbooks que importe a otra cuenta de Automation.

No es posible recuperar el valor de variables cifradas o del campo de contraseña de las credenciales mediante cmdlets. Si no conoce estos valores, puede recuperarlos en un runbook. Para recuperar valores de variables, consulte [Recursos de variables en Azure Automation](shared-resources/variables.md). Para más información sobre cómo recuperar valores de credenciales, consulte [Recursos de credenciales en Azure Automation](shared-resources/credentials.md).

 ### <a name="dsc-configurations"></a>Configuraciones DSC

Puede exportar las configuraciones de DSC a archivos de script con Azure Portal o con el cmdlet [Export-AzAutomationDscConfiguration](https://docs.microsoft.com/powershell/module/az.automation/export-azautomationdscconfiguration?view=azps-3.7.0
) en Windows PowerShell. Puede importar y usar estas configuraciones en otra cuenta de Automation.

## <a name="geo-replication-in-azure-automation"></a>Replicación geográfica en Azure Automation

La replicación geográfica es estándar en cuentas de Azure Automation. Puede elegir una región primaria al configurar su cuenta. El servicio de replicación geográfica interna de Automation asigna automáticamente una región secundaria a la cuenta. Después, el servicio realiza una copia de seguridad continua de los datos de la cuenta de la región primaria en la región secundaria. Puede encontrar la lista completa de las regiones primarias y secundarias en [Continuidad empresarial y recuperación ante desastres (BCDR): Regiones emparejadas de Azure](https://docs.microsoft.com/azure/best-practices-availability-paired-regions). 

La copia de seguridad creada por el servicio de replicación geográfica de Automation es una copia completa de los recursos de Automation, las configuraciones, etc. Esta copia de seguridad se puede usar si la región primaria deja de funcionar y pierde datos. En el improbable caso de que se pierdan datos de una región primaria, Microsoft intenta recuperarlos. Si la empresa no puede recuperar los datos principales, utiliza la conmutación automática por error y le informa de la situación a través de su suscripción de Azure. 

El servicio de replicación geográfica de Automation no es accesible directamente para los clientes externos si se produce un error regional. Si desea mantener la configuración de Automation y los runbooks durante errores regionales:

1. Seleccione una región secundaria para emparejar con la región geográfica de la cuenta de Automation principal.

2. Cree una cuenta de Automation en la región secundaria.

3. En la cuenta principal, exporte los runbooks como archivos de script.

4. Importe los runbooks a su cuenta de Automation en la región secundaria.

## <a name="next-steps"></a>Pasos siguientes

* Para más información acerca de los recursos seguros de Azure Automation, consulte [Cifrado de recursos protegidos en Azure Automation](automation-secure-asset-encryption.md).
* Para más información sobre la replicación geográfica, consulte [Creación y uso de la replicación geográfica activa](https://docs.microsoft.com/azure/sql-database/sql-database-active-geo-replication).