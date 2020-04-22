---
title: Introducción a Azure Automation State Configuration
description: Este artículo es una introducción a Azure Automation State Configuration (DSC), sus condiciones y problemas conocidos.
keywords: powershell dsc, configuración de estado deseado, powershell dsc azure
services: automation
ms.service: automation
ms.subservice: dsc
author: mgoedtel
ms.author: magoedte
ms.date: 04/15/2020
ms.topic: conceptual
manager: carmonm
ms.openlocfilehash: 1166f5a1d7586c54255120a656b060c93f842fd9
ms.sourcegitcommit: b80aafd2c71d7366838811e92bd234ddbab507b6
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 04/16/2020
ms.locfileid: "81406182"
---
# <a name="state-configuration-overview"></a>Introducción a State Configuration

Azure Automation State Configuration es un servicio de Azure que le permite escribir, administrar y compilar [configuraciones](/powershell/scripting/dsc/configurations/configurations) de Desired State Configuration (DSC) de PowerShell. El servicio también importa [recursos de DSC](/powershell/scripting/dsc/resources/resources) y asigna configuraciones a los nodos de destino, todo en la nube.

## <a name="why-use-azure-automation-state-configuration"></a>Razones para usar Azure Automation State Configuration

Azure Automation State Configuration proporciona varias ventajas con respecto a DSC fuera de Azure.

### <a name="built-in-pull-server"></a>Servidor de extracción integrado

Azure Automation State Configuration proporciona un servidor de extracción de DSC similar a la [característica de Windows DSC-Service](/powershell/scripting/dsc/pull-server/pullserver). Los nodos de destino pueden recibir automáticamente configuraciones, ajustarse al estado deseado y enviar informes de cumplimiento. El servidor de extracción integrado en Azure Automation elimina la necesidad de configurar y mantener su propio servidor de extracción. Azure Automation puede tener como destino máquinas físicas o virtuales Windows o Linux, en la nube o en un entorno local.

### <a name="manage-all-your-dsc-artifacts"></a>Administración de todos los artefactos de DSC

Azure Automation State Configuration proporciona la misma capa de administración a [Desired State Configuration de PowerShell](/powershell/scripting/dsc/overview/overview) que la que ofrece para scripting de PowerShell. Desde Azure Portal o desde PowerShell, puede administrar todas las configuraciones, recursos y nodos de destino de DSC.

![Captura de pantalla de la página de Azure Automation](./media/automation-dsc-overview/azure-automation-blade.png)

### <a name="import-reporting-data-into-azure-monitor-logs"></a>Importación de datos de informes en los registros de Azure Monitor

Los nodos que se administran con Azure Automation State Configuration envían datos de informe de estado detallados al servidor de extracción integrado. Puede configurar Azure Automation State Configuration para que envíe estos datos a su área de trabajo de Log Analytics. Para más información, consulte [Reenvío de datos de informes de Azure Automation State Configuration a registros de Azure Monitor](automation-dsc-diagnostics.md).

## <a name="prerequisites"></a>Prerrequisitos

Tenga en cuenta los siguientes requisitos al utilizar Azure Automation State Configuration para DSC.

### <a name="operating-system-requirements"></a>Requisitos de sistema operativo

Para nodos que ejecutan Windows, se admiten las siguientes versiones:

- Windows Server 2019
- Windows Server 2016
- Windows Server 2012 R2
- Windows Server 2012
- Windows Server 2008 R2 SP1
- Windows 10
- Windows 8.1
- Windows 7

>[!NOTE]
>Como la SKU del producto [Microsoft Hyper-V Server](/windows-server/virtualization/hyper-v/hyper-v-server-2016) independiente no contiene una implementación de DSC, no puede administrarse en DSC de PowerShell ni en Azure Automation State Configuration.

Para los nodos que ejecutan Linux, la extensión DSC de Linux admite todas las distribuciones de Linux que se incluyen en [Distribuciones de Linux admitidas](https://github.com/Azure/azure-linux-extensions/tree/master/DSC#4-supported-linux-distributions).

### <a name="dsc-requirements"></a>Requisitos de DSC

Durante la incorporación, [Windows Management Framework 5.1](https://docs.microsoft.com/powershell/scripting/wmf/setup/install-configure) se instala en todos los nodos de Windows que se ejecutan en Azure. Para los nodos que ejecutan Windows Server 2012 y Windows 7, se habilita [WinRM](https://docs.microsoft.com/powershell/scripting/dsc/troubleshooting/troubleshooting#winrm-dependency).

Para todos los nodos de Linux que se ejecutan en Azure, [DSC de PowerShell para Linux](https://github.com/Microsoft/PowerShell-DSC-for-Linux) se instala durante la incorporación.

### <a name="configuration-of-private-networks"></a><a name="network-planning"></a>Configuración de redes privadas

Si los nodos se encuentran en una red privada, se necesitan el puerto y las direcciones URL siguientes. Estos recursos proporcionan conectividad de red para que el nodo administrado y el DSC permitido se comuniquen con Azure Automation.

* Puerto: solo se requiere el puerto TCP 443 para el acceso a Internet
* Dirección URL global: * **.azure-automation.net**
* Dirección URL global de US Gov Virginia: * **.azure-automation.us**
* Servicio de agente: **https://\<workspaceId\>.agentsvc.azure-automation.net**

Si usa recursos de DSC que se comunican entre nodos, como los [recursos WaitFor*](https://docs.microsoft.com/powershell/scripting/dsc/reference/resources/windows/waitForAllResource), también debe permitir la comunicación entre nodos. Para conocer estos requisitos de red, consulte la documentación de cada recurso de DSC.

#### <a name="proxy-support"></a>Compatibilidad con proxy

La compatibilidad con proxy para el agente DSC está disponible en Windows, versión 1809 y posteriores. Para habilitar esta opción, puede establecer los valores de `ProxyURL` y `ProxyCredential` en el [script de metaconfiguraciones](automation-dsc-onboarding.md#generate-dsc-metaconfigurations) que utilizó para registrar los nodos.

>[!NOTE]
>Azure Automation State Configuration no proporciona compatibilidad con el proxy de DSC en versiones anteriores de Windows.

En el caso de los nodos de Linux, el agente de DSC es compatible con los servidores proxy y usa la variable `http_proxy` para especificar la dirección URL.

#### <a name="azure-automation-state-configuration-network-ranges-and-namespace"></a>Intervalos y espacios de nombres de red de Azure Automation State Configuration

Cuando defina excepciones, es conveniente que utilice las direcciones IP que se indican en la tabla siguiente. En el caso de las direcciones IP, puede descargar el archivo XML con los [intervalos IP del centro de datos de Microsoft Azure](https://www.microsoft.com/download/details.aspx?id=41653) desde el Centro de descarga de Microsoft. El archivo contiene los intervalos implementados actualmente y los próximos cambios en los intervalos IP. Se actualiza semanalmente.

Si tiene una cuenta de Automation definida para una región específica, puede restringir la comunicación con ese centro de datos regional. En la tabla siguiente se proporciona el registro de DNS para cada región:

| **Región** | **Registro de DNS** |
| --- | --- |
| Centro-Oeste de EE. UU. | wcus-jobruntimedata-prod-su1.azure-automation.net</br>wcus-agentservice-prod-1.azure-automation.net |
| Centro-sur de EE. UU. |scus-jobruntimedata-prod-su1.azure-automation.net</br>scus-agentservice-prod-1.azure-automation.net |
| Este de EE. UU.    | eus-jobruntimedata-prod-su1.azure-automation.net</br>eus-agentservice-prod-1.azure-automation.net |
| Este de EE. UU. 2 |eus2-jobruntimedata-prod-su1.azure-automation.net</br>eus2-agentservice-prod-1.azure-automation.net |
| Centro de Canadá |cc-jobruntimedata-prod-su1.azure-automation.net</br>cc-agentservice-prod-1.azure-automation.net |
| Oeste de Europa |we-jobruntimedata-prod-su1.azure-automation.net</br>we-agentservice-prod-1.azure-automation.net |
| Norte de Europa |ne-jobruntimedata-prod-su1.azure-automation.net</br>ne-agentservice-prod-1.azure-automation.net |
| Sudeste de Asia |sea-jobruntimedata-prod-su1.azure-automation.net</br>sea-agentservice-prod-1.azure-automation.net|
| Centro de la India |cid-jobruntimedata-prod-su1.azure-automation.net</br>cid-agentservice-prod-1.azure-automation.net |
| Japón Oriental |jpe-jobruntimedata-prod-su1.azure-automation.net</br>jpe-agentservice-prod-1.azure-automation.net |
| Sudeste de Australia |ase-jobruntimedata-prod-su1.azure-automation.net</br>ase-agentservice-prod-1.azure-automation.net |
| Sur de Reino Unido 2 | uks-jobruntimedata-prod-su1.azure-automation.net</br>uks-agentservice-prod-1.azure-automation.net |
| US Gov - Virginia | usge-jobruntimedata-prod-su1.azure-automation.us<br>usge-agentservice-prod-1.azure-automation.us |

> [!NOTE]
> El archivo XML de direcciones IP de los centros de datos de Azure enumera los intervalos de direcciones IP que se usan en los centros de datos de Microsoft Azure. El archivo incluye el proceso, SQL y los intervalos de almacenamiento.
>
>Semanalmente, se publica un archivo actualizado. El archivo refleja los intervalos implementados actualmente y los próximos cambios en los intervalos IP. Los nuevos intervalos que aparecen en el archivo no se utilizan en los centros de datos durante al menos una semana. Es conveniente descargar un nuevo archivo XML cada semana. Después, puede actualizar el sitio para identificar correctamente los servicios que se ejecutan en Azure. 

Si es un usuario de Azure ExpressRoute, tenga en cuenta que este archivo se usa para actualizar la publicidad del Protocolo de puerta de enlace de borde (BGP) del espacio de Azure la primera semana de cada mes.

## <a name="next-steps"></a>Pasos siguientes

- Para empezar a trabajar con DSC en Azure Automation State Configuration, consulte [Introducción a Azure Automation State Configuration](automation-dsc-getting-started.md).
- Para aprender a incorporar nodos, consulte [Incorporación de máquinas para realizar la administración con Azure Automation State Configuration](automation-dsc-onboarding.md).
- Si necesita más información sobre la compilación de configuraciones de DSC para poder asignarlas a los nodos de destino, consulte [Compilación de configuraciones de DSC en Azure Automation State Configuration](automation-dsc-compile.md).
- Para ver una referencia de los cmdlets de PowerShell, consulte [Az.Automation](https://docs.microsoft.com/powershell/module/az.automation/?view=azps-3.7.0#automation).
- Para obtener información de precios, consulte [Precios de State Configuration de Azure Automation](https://azure.microsoft.com/pricing/details/automation/).
- Para ver un ejemplo sobre el uso de Azure Automation State Configuration en una canalización de implementación continua, consulte [Implementación continua en máquinas virtuales con Azure Automation State Configuration y Chocolatey](automation-dsc-cd-chocolatey.md).
