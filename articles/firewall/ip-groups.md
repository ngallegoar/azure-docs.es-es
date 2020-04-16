---
title: IP Groups en Azure Firewall
description: Los grupos IP permiten agrupar y administrar direcciones IP en las reglas de Azure Firewall.
services: firewall
author: vhorne
ms.service: firewall
ms.topic: conceptual
ms.date: 04/06/2020
ms.author: victorh
ms.openlocfilehash: e0638cbccd5e3bc282dbdd7d3b5918e29081a12b
ms.sourcegitcommit: 441db70765ff9042db87c60f4aa3c51df2afae2d
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 04/06/2020
ms.locfileid: "80757168"
---
# <a name="ip-groups-preview-in-azure-firewall"></a>IP Groups (versión preliminar) en Azure Firewall

> [!IMPORTANT]
> Esta versión preliminar pública se proporciona sin un acuerdo de nivel de servicio y no debe usarse para cargas de trabajo de producción. Puede que algunas características no se admitan, que tengan funcionalidades limitadas o que no estén disponibles en todas las ubicaciones de Azure. Para más información, consulte [Términos de uso complementarios de las versiones preliminares de Microsoft Azure](https://azure.microsoft.com/support/legal/preview-supplemental-terms/).

IP Groups permite agrupar y administrar direcciones IP en reglas de Azure Firewall de las siguientes maneras:

- Como una dirección de origen en reglas de DNAT
- Como una dirección de origen o destino en reglas de red
- Como una dirección de origen en reglas de aplicación


Un grupo IP puede tener una sola dirección IP, varias direcciones IP o uno o varios intervalos de direcciones IP.

Los grupos de IP se puede reutilizar en reglas de DNAT, red y aplicación de Azure Firewall para varios firewalls entre regiones y suscripciones de Azure. Los nombres de grupo deben ser únicos. Puede configurar un grupo de IP en Azure Portal, la CLI de Azure o la API REST. Para ayudarle a comenzar, se proporciona una plantilla de ejemplo.

## <a name="sample-format"></a>Formato de ejemplo

Los siguientes ejemplos de formato de dirección IPv4 son válidos para su uso en IP Groups:

- Dirección única: 10.0.0.0
- Notación CIDR: 10.1.0.0/32
- Intervalo de direcciones: 10.2.0.0-10.2.0.31

## <a name="create-an-ip-group"></a>Creación de un grupo de IP

Un grupo de IP se puede crear mediante Azure Portal, la CLI de Azure o la API REST. Para más información, consulte [Creación de grupos de IP](create-ip-group.md).

## <a name="browse-ip-groups"></a>Acceso a IP Groups
1. En la barra de búsqueda de Azure Portal, escriba **IP Groups** y selecciónelo. Puede ver la lista de los grupos de IP o puede seleccionar **Agregar** para crear uno.
2. Seleccione un grupo de IP para abrir la página de información general. Puede editar, agregar o eliminar direcciones IP o grupos de IP.

   ![Información general de grupos de IP](media/ip-groups/overview.png)

## <a name="manage-an-ip-group"></a>Administración de un grupo de IP

Puede ver todas las direcciones IP del grupo de IP y las reglas o los recursos a él asociados. Para eliminar un grupo de IP, primero debe desasociar el grupo de IP del recurso que lo usa.

1. Para ver o editar las direcciones IP, seleccione **Direcciones IP** en **Configuración** en el panel izquierdo.
2. Para agregar una o varias direcciones IP, seleccione **Agregar direcciones IP**. Se abre la página **Drag or Browse** (Arrastrar o examinar) para que las descargue; también puede escribir las direcciones manualmente.
3.    Seleccione los puntos suspensivos ( **…** ) de la derecha para editar o eliminar las direcciones IP. Para editar o eliminar varias direcciones IP, active las casillas y seleccione **Editar** o **Eliminar** en la parte superior.
4. Por último, puede exportar el archivo en formato CSV.

> [!NOTE]
> Si elimina todas las direcciones IP de un grupo de IP mientras todavía se está usando en una regla, esa regla se omite.


## <a name="use-an-ip-group"></a>Uso de un grupo de IP

Ahora puede seleccionar **IP Group** (Grupo de IP) como **Tipo de origen** o **Tipo de destino** para las direcciones IP al crear reglas de DNAT, aplicación o red de Azure Firewall.

> [!NOTE]
> No se admiten grupos de IP en la directiva de firewall. Actualmente solo se admiten con las reglas de firewall tradicionales.

![IP Groups en Firewall](media/ip-groups/fw-ipgroup.png)

## <a name="region-availability"></a>Disponibilidad en regiones

Los grupos IP están disponible en todas las regiones de la nube pública.

## <a name="ip-address-limits"></a>Límites de direcciones IP

Para 50 grupos IP o menos, puede tener un máximo de 5 000 direcciones IP individuales cada una por cada instancia de firewall. De 51 a 100 grupos IP, puede tener un máximo de 500 direcciones IP individuales cada una por cada instancia de firewall.

### <a name="examples"></a>Ejemplos

#### <a name="example-1-supported"></a>Ejemplo 1: compatible

|Grupos de IP  |Número de direcciones IP  |Notación  |Regla  |
|---------|---------|---------|---------|
|IPGroup1 |4096     |10.0.0.0/20  |Rule1|
|IPGroup2     |3|196.0.0.0 - 196.0.0.2|Rule1|
|IPGroup3     |1|1.2.3.4|Rule1|
|     |**Total 4 100**|         |         |
|     |         |         |         |

#### <a name="example-2-supported"></a>Ejemplo 2: compatible

|Grupos de IP  |Número de direcciones IP  |Notación  |Regla  |
|---------|---------|---------|---------|
|IPGroup1 |4096     |10.0.0.0/20  |Rule1|
|IPGroup2     |4096|11.0.0.0/20|Rule1|
|     |**Total 8 192**|         |         |

#### <a name="example-3-not-supported"></a>Ejemplo 3: no compatible

|Grupos de IP  |Número de direcciones IP  |Notación  |Regla  |
|---------|---------|---------|---------|
|IPGroup1 |8192     |10.0.0.0/20, 11.0.0.0/20  |Rule1|
|     |**Total 8 192**|||

#### <a name="example-4-supported"></a>Ejemplo 4: compatible

|Grupos de IP  |Número de direcciones IP  |Notación  |Regla  |
|---------|---------|---------|---------|
|IPGroup1 |4096     |10.0.0.0/20  |Rule1|
|IPGroup2     |4096|11.0.0.0/20|Rule2|
|     |**Total 8 192**|         |         |


## <a name="related-azure-powershell-cmdlets"></a>Cmdlets de Azure PowerShell relacionados

Se pueden usar los siguientes cmdlets de Azure PowerShell para crear y administrar grupos de IP:

- [New-AzIpGroup](https://docs.microsoft.com/powershell/module/az.network/new-azipgroup?view=azps-3.4.0)
- [Remove-AzIPGroup](https://docs.microsoft.com/powershell/module/az.network/remove-azipgroup?view=azps-3.4.0)
- [Get-AzIpGroup](https://docs.microsoft.com/powershell/module/az.network/get-azipgroup?view=azps-3.4.0)
- [Set-AzIpGroup](https://docs.microsoft.com/powershell/module/az.network/set-azipgroup?view=azps-3.4.0)
- [New-AzFirewallNetworkRule](https://docs.microsoft.com/powershell/module/az.network/new-azfirewallnetworkrule?view=azps-3.4.0)
- [New-AzFirewallApplicationRule](https://docs.microsoft.com/powershell/module/az.network/new-azfirewallapplicationrule?view=azps-3.4.0)
- [New-AzFirewallNatRule](https://docs.microsoft.com/powershell/module/az.network/new-azfirewallnatrule?view=azps-3.4.0)

## <a name="next-steps"></a>Pasos siguientes

- Consulte el tutorial [Implementación y configuración de Azure Firewall](tutorial-firewall-deploy-portal.md).