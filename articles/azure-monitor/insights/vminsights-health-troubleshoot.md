---
title: Solución de problemas del estado de invitado de Azure Monitor para VM (versión preliminar)
description: Describe los pasos para solucionar problemas que puede realizar cuando tenga problemas con el mantenimiento de las instancias de Azure Monitor para VM.
ms.subservice: ''
ms.topic: conceptual
author: bwren
ms.author: bwren
ms.date: 09/08/2020
ms.openlocfilehash: 048230bb85d2ac599efc6bd3fb326f98038cce57
ms.sourcegitcommit: c157b830430f9937a7fa7a3a6666dcb66caa338b
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 11/17/2020
ms.locfileid: "94686561"
---
# <a name="troubleshoot-azure-monitor-for-vms-guest-health-preview"></a>Solución de problemas del estado de invitado de Azure Monitor para VM (versión preliminar)
En este artículo se describen los pasos para solucionar problemas que puede realizar cuando tenga problemas con el mantenimiento de las instancias de Azure Monitor para VM.

## <a name="error-message-that-no-data-is-available"></a>Mensaje de error que indica que no hay datos disponibles 

![Sin datos](media/vminsights-health-troubleshoot/no-data.png)


### <a name="verify-that-the-virtual-machine-meets-configuration-requirements"></a>Compruebe que la máquina virtual cumple estos requisitos de configuración.

1. Compruebe que la máquina virtual sea una máquina virtual de Azure. Azure Arc para servidores no se admite de momento.
2. Compruebe que la máquina virtual usa un [sistema operativo](vminsights-health-enable.md?current-limitations.md) compatible.
3. Compruebe que la máquina virtual está instalada en una [región compatible](vminsights-health-enable.md?current-limitations.md).
4. Compruebe que el área de trabajo de Log Analytics está instalada en una [región compatible](vminsights-health-enable.md?current-limitations.md).

### <a name="verify-that-the-vm-is-properly-onboarded"></a>Comprobación de que la máquina virtual está incorporada correctamente
Compruebe que la extensión del agente de Azure Monitor y el agente de estado de la máquina virtual invitada se aprovisionaron correctamente en la máquina virtual. Seleccione **Extensiones** en el menú de la máquina virtual en Azure Portal. Si los dos agentes aparecen en la lista, consulte []().

![Extensiones de máquina virtual](media/vminsights-health-troubleshoot/extensions.png)

### <a name="verify-the-system-assigned-identity-is-enabled-on-the-virtual-machine"></a>Comprobación de que la identidad asignada por el sistema está habilitada en la máquina virtual
Compruebe que la identidad asignada por el sistema está habilitada en la máquina virtual. Seleccione **Identidad** en el menú de la máquina virtual en Azure Portal. Si los dos agentes aparecen en la lista, consulte []().

![Identidad asignada por el sistema](media/vminsights-health-troubleshoot/system-identity.png)

### <a name="verify-data-collection-rule"></a>Comprobación de la regla de recopilación de datos
Compruebe que la regla de recopilación de datos que especifica la extensión de mantenimiento como un origen de datos está asociada a la máquina virtual.

## <a name="error-message-for-bad-request-due-to-insufficient-permissions"></a>Mensaje de error debido a una solicitud incorrecta por permisos insuficientes
Este error indica que el proveedor de recursos **Microsoft.WorkloadMonitor** no se registró en la suscripción. Consulte [Tipos y proveedores de recursos de Azure](/azure/azure-resource-manager/management/resource-providers-and-types#register-resource-provider) para obtener información sobre registrar a este proveedor de recursos. 

![Solicitud incorrecta](media/vminsights-health-troubleshoot/bad-request.png)

## <a name="next-steps"></a>Pasos siguientes

- [Obtenga información general sobre la característica de estado de invitado de Azure Monitor para VM](vminsights-health-overview.md)
