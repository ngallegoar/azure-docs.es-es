---
title: Administración de extensiones de máquina virtual con servidores habilitados para Azure Arc
description: Los servidores habilitados para Azure Arc pueden administrar la implementación de extensiones de máquina virtual que proporcionan tareas de automatización y configuración posteriores a la implementación con máquinas virtuales que no son de Azure.
ms.date: 10/19/2020
ms.topic: conceptual
ms.openlocfilehash: e9865761fd3e5897ee3f01cd3d6ca620d5ea2f4b
ms.sourcegitcommit: 9b8425300745ffe8d9b7fbe3c04199550d30e003
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/23/2020
ms.locfileid: "92460893"
---
# <a name="virtual-machine-extension-management-with-azure-arc-enabled-servers"></a>Administración de extensiones de máquina virtual con servidores habilitados para Azure Arc

Las extensiones de máquina virtual (VM) son pequeñas aplicaciones que realizan tareas de automatización y configuración posteriores a la implementación en máquinas virtuales de Azure. Por ejemplo, si una máquina virtual necesita que se instale software, protección antivirus o ejecutar un script en ella, se puede usar una extensión de máquina virtual.

Los servidores habilitados para Azure Arc permite implementar extensiones de máquina virtual de Azure en máquinas virtuales Windows y Linux que no son de Azure, lo que simplifica la administración de su máquina híbrida local, perimetral y otros entornos en la nube durante su ciclo de vida. Las extensiones de VM se pueden administrar con los siguientes métodos en las máquinas híbridas o servidores administrados por servidores habilitados para Arc:

- [Azure Portal](manage-vm-extensions-portal.md)
- La [CLI de Azure](manage-vm-extensions-cli.md)
- [Azure PowerShell](manage-vm-extensions-powershell.md)
- [Plantillas de Azure Resource Manager](manage-vm-extensions-template.md)

## <a name="key-benefits"></a>Ventajas principales

La compatibilidad con la extensión de máquina virtual de servidores habilitados para Azure Arc ofrece las ventajas principales siguientes:

- Usar [Azure Automation State Configuration](../../automation/automation-dsc-overview.md) para almacenar de forma centralizada las configuraciones y mantener el estado deseado de las máquinas conectadas híbridas que se hayan habilitado a través de la extensión de máquina virtual de DSC.

- Recopilar datos de registro para su análisis con los [registros de Azure Monitor](../../azure-monitor/platform/data-platform-logs.md) habilitados a través de la extensión de máquina virtual del agente de Log Analytics. Son especialmente útiles para llevar a cabo un análisis complejo de datos procedentes de orígenes diversos.

- Con [Azure Monitor para VM](../../azure-monitor/insights/vminsights-overview.md), se analiza el rendimiento de las VM Windows y Linux, y se supervisa el impacto de sus procesos y dependencias en otros recursos y procesos externos. Esto se logra al habilitar las extensiones de máquina virtual del agente de Log Analytics y Dependency Agent.

- Descargue y ejecute scripts en máquinas conectadas híbridas mediante la extensión de script personalizado. Esta extensión es útil para la configuración posterior a la implementación, la instalación de software o cualquier otra tarea de configuración o administración.

## <a name="availability"></a>Disponibilidad

La funcionalidad de la extensión de máquina virtual solo está disponible en la lista de [regiones admitidas](overview.md#supported-regions). Asegúrese de incorporar el equipo en una de estas regiones.

## <a name="extensions"></a>Extensiones

En esta versión se admiten las siguientes extensiones de máquina virtual en máquinas Windows y Linux.

|Extensión |SO |Publicador |Información adicional |
|----------|---|----------|-----------------------|
|CustomScriptExtension |Windows |Microsoft.Compute |[Extensión de script personalizado de Windows](../../virtual-machines/extensions/custom-script-windows.md)|
|DSC |Windows |Microsoft.PowerShell|[Extensión DSC de Windows PowerShell](../../virtual-machines/extensions/dsc-windows.md)|
|Agente de Log Analytics |Windows |Microsoft.EnterpriseCloud.Monitoring |[Extensión de máquina virtual de Log Analytics para Windows](../../virtual-machines/extensions/oms-windows.md)|
|Microsoft Dependency Agent | Windows |Microsoft.Compute | [Extensión de máquina virtual de Dependency Agent para Windows](../../virtual-machines/extensions/agent-dependency-windows.md)|
|CustomScript|Linux |Microsoft.Azure.Extension |[Extensión de script personalizado de Linux versión 2](../../virtual-machines/extensions/custom-script-linux.md) |
|DSC |Linux |Microsoft.OSTCExtensions |[Extensión DSC de PowerShell para Linux](../../virtual-machines/extensions/dsc-linux.md) |
|Agente de Log Analytics |Linux |Microsoft.EnterpriseCloud.Monitoring |[Extensión de máquina virtual de Log Analytics para Linux](../../virtual-machines/extensions/oms-linux.md) |
|Microsoft Dependency Agent | Linux |Microsoft.Compute | [Extensión de máquina virtual de Dependency Agent para Linux](../../virtual-machines/extensions/agent-dependency-linux.md) |

Para obtener información sobre el paquete del agente de Azure Connected Machine y los detalles del componente del agente de extensión, consulte [Información general del agente](agent-overview.md#agent-component-details).

## <a name="prerequisites"></a>Requisitos previos

Esta característica depende de los siguientes proveedores de recursos de Azure en su suscripción:

- **Microsoft.HybridCompute**
- **Microsoft.GuestConfiguration**

Si aún no están registrados, siga los pasos descritos en [Registro de proveedores de recursos de Azure](agent-overview.md#register-azure-resource-providers).

La extensión de máquina virtual del agente de Log Analytics para Linux requiere que Python 2.x esté instalado en el equipo de destino.

### <a name="connected-machine-agent"></a>Agente de Connected Machine

Compruebe que el equipo coincida con las [versiones compatibles](agent-overview.md#supported-operating-systems) de los sistemas operativos Windows y Linux para el agente de Azure Connected Machine.

La versión mínima del agente de Connected Machine que es compatible con esta característica en Windows y Linux es la 1.0.

Para actualizar la máquina a la versión requerida del agente, consulte [Actualización del agente](manage-agent.md#upgrading-agent).

## <a name="next-steps"></a>Pasos siguientes

Puede implementar, administrar y quitar extensiones de VM con la [CLI de Azure](manage-vm-extensions-cli.md), [PowerShell](manage-vm-extensions-powershell.md), [Azure Portal](manage-vm-extensions-portal.md) o [plantillas de Azure Resource Manager](manage-vm-extensions-template.md).
