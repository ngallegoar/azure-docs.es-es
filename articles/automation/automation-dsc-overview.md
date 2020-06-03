---
title: Introducción a Azure Automation State Configuration
description: En este artículo se proporciona una introducción sobre Azure Automation State Configuration.
keywords: powershell dsc, configuración de estado deseado, powershell dsc azure
services: automation
ms.service: automation
ms.subservice: dsc
author: mgoedtel
ms.author: magoedte
ms.date: 11/06/2018
ms.topic: conceptual
manager: carmonm
ms.openlocfilehash: 9880915061c0639aebe30bdb33258d7c79e155d7
ms.sourcegitcommit: 0b80a5802343ea769a91f91a8cdbdf1b67a932d3
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 05/25/2020
ms.locfileid: "83836896"
---
# <a name="azure-automation-state-configuration-overview"></a>Introducción a Azure Automation State Configuration

Azure Automation State Configuration es un servicio de administración de configuración de Azure que le permite escribir, administrar y compilar [configuraciones](/powershell/scripting/dsc/configurations/configurations) de PowerShell Desired State Configuration (DSC) para nodo de cualquier centro de datos en la nube local. El servicio también importa [recursos de DSC](/powershell/scripting/dsc/resources/resources) y asigna configuraciones a los nodos de destino, todos en la nube. Para acceder a Azure Automation State Configuration desde Azure Portal, seleccione **State Configuration (DSC)** en **Administración de configuración**. 

Puede utilizar Azure Automation State Configuration para administrar diversas máquinas:

- Azure Virtual Machines
- Azure Virtual Machines (clásico)
- Máquinas físicas y virtuales con Windows locales o en una nube que no sea Azure (incluidas las instancias de AWS EC2)
- Máquinas físicas y virtuales con Linux locales, en Azure o en una nube que no sea Azure

Si no está preparado para administrar la configuración de las máquinas desde la nube, Azure Automation State Configuration también se puede usar como punto de conexión meramente informativo. Esta característica permite establecer (insertar) configuraciones a través de DSC y ver detalles de los informes en Azure Automation.

> [!NOTE]
> La administración de máquinas virtuales de Azure con State Configuration de Azure Automation se incluye sin cargo extra alguno si la versión de la extensión Desired State Configuration de máquina virtual de Azure instalada es superior a 2.70. Consulte la [**página de precios de Automation**](https://azure.microsoft.com/pricing/details/automation/) para más información.

## <a name="why-use-azure-automation-state-configuration"></a>Razones para usar Azure Automation State Configuration

Azure Automation State Configuration proporciona varias ventajas con respecto al uso de DSC fuera de Azure. Este servicio permite la escalabilidad en miles de máquinas de forma rápida y sencilla desde una ubicación central y segura. Puede habilitar fácilmente máquinas, asignarles configuraciones declarativas y ver informes que muestran el cumplimiento por parte de cada máquina del estado deseado que se especifique.

Azure Automation State Configuration es a DSC lo que los runbooks de Azure Automation son al scripting de PowerShell. En otras palabras, Azure Automation le ayuda a administrar las configuraciones de DSC, de la misma manera que lo hace con los scripts de PowerShell. 

### <a name="built-in-pull-server"></a>Servidor de extracción integrado

Azure Automation State Configuration proporciona un servidor de extracción de DSC similar a la [característica de Windows DSC-Service](/powershell/scripting/dsc/pull-server/pullserver). Los nodos de destino pueden recibir automáticamente configuraciones, ajustarse al estado deseado y enviar informes de cumplimiento. El servidor de extracción integrado en Azure Automation elimina la necesidad de configurar y mantener su propio servidor de extracción. Azure Automation puede tener como destino máquinas físicas o virtuales Windows o Linux, en la nube o en un entorno local.

### <a name="management-of-all-your-dsc-artifacts"></a>Administración de todos los artefactos de DSC

Azure Automation State Configuration proporciona la misma capa de administración a [Desired State Configuration de PowerShell](/powershell/scripting/dsc/overview/overview) que la que ofrece para scripting de PowerShell. Desde Azure Portal o desde PowerShell, puede administrar todas las configuraciones, recursos y nodos de destino de DSC.

![Captura de pantalla de la página de Azure Automation](./media/automation-dsc-overview/azure-automation-blade.png)

### <a name="import-of-reporting-data-into-azure-monitor-logs"></a>Importación de datos de informes a registros de Azure Monitor

Los nodos que se administran con Azure Automation State Configuration envían datos de informe de estado detallados al servidor de extracción integrado. Puede configurar Azure Automation State Configuration para que envíe estos datos a su área de trabajo de Log Analytics. Consulte [Reenvío de datos de informes de Azure Automation State Configuration a registros de Azure Monitor](automation-dsc-diagnostics.md).

## <a name="prerequisites-for-using-azure-automation-state-configuration"></a>Requisitos previos para el uso de Azure Automation State Configuration

Tenga en cuenta los requisitos de esta sección al usar Azure Automation State Configuration.

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
>La SKU del producto independiente [Microsoft Hyper-V Server](/windows-server/virtualization/hyper-v/hyper-v-server-2016) no contiene ninguna implementación de DSC. Por lo tanto, no se puede administrar con DSC de PowerShell o con Azure Automation State Configuration.

Para los nodos que ejecutan Linux, la extensión DSC de Linux admite todas las distribuciones de Linux que se incluyen en [Distribuciones de Linux admitidas](https://github.com/Azure/azure-linux-extensions/tree/master/DSC#4-supported-linux-distributions).

### <a name="dsc-requirements"></a>Requisitos de DSC

En todos los nodos de Windows que se ejecutan en Azure, [WMF 5.1](https://docs.microsoft.com/powershell/scripting/wmf/setup/install-configure) se instala cuando se habilitan las máquinas. Para los nodos que ejecutan Windows Server 2012 y Windows 7, se habilita [WinRM](https://docs.microsoft.com/powershell/scripting/dsc/troubleshooting/troubleshooting#winrm-dependency).

Para todos los nodos de Linux que se ejecutan en Azure, [DSC de PowerShell para Linux](https://github.com/Microsoft/PowerShell-DSC-for-Linux) se instala cuando se habilitan las máquinas.

### <a name="configuration-of-private-networks"></a><a name="network-planning"></a>Configuración de redes privadas

Si los nodos se encuentran en una red privada, se necesitan el puerto y las direcciones URL siguientes. Estos recursos proporcionan conectividad de red para que el nodo administrado y el DSC permitido se comuniquen con Azure Automation.

* Puerto: solo se requiere el puerto TCP 443 para el acceso a Internet
* Dirección URL global: * **.azure-automation.net**
* Dirección URL global de US Gov Virginia: * **.azure-automation.us**
* Servicio de agente: **https://\<workspaceId\>.agentsvc.azure-automation.net**

Si usa recursos de DSC que se comunican entre nodos, como los [recursos WaitFor*](https://docs.microsoft.com/powershell/scripting/dsc/reference/resources/windows/waitForAllResource), también debe permitir el tráfico entre nodos. Consulte la documentación de cada recurso de DSC para comprender estos requisitos de red.

#### <a name="proxy-support"></a>Compatibilidad con proxy

La compatibilidad con proxy para el agente DSC está disponible en Windows, versión 1809 y posteriores. Para habilitar esta opción, establezca los valores de las propiedades `ProxyURL` y `ProxyCredential` en el [script de metaconfiguraciones](automation-dsc-onboarding.md#generate-dsc-metaconfigurations) que se utiliza para registrar los nodos. 

>[!NOTE]
>Azure Automation State Configuration no proporciona compatibilidad con el proxy de DSC para versiones anteriores de Windows.

Para los nodos de Linux, el agente de DSC admite el proxy y utiliza la variable `http_proxy` para determinar la dirección URL. Para obtener más información sobre el soporte con proxy, vea [Generación de metaconfiguraciones de DSC](automation-dsc-onboarding.md#generate-dsc-metaconfigurations).

#### <a name="azure-automation-state-configuration-network-ranges-and-namespace"></a>Intervalos y espacios de nombres de red de Azure Automation State Configuration

Se recomienda utilizar las direcciones mostradas a continuación al definir las excepciones. Puede descargar los [intervalos de direcciones IP del centro de datos de Microsoft Azure](https://www.microsoft.com/download/details.aspx?id=41653). Este archivo se actualiza semanalmente y tiene los intervalos implementados en ese momento y los próximos cambios en los intervalos de direcciones IP.

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

Para obtener una lista de direcciones IP de regiones en lugar de nombres de regiones, descargue el archivo XML [Direcciones IP de los centros de datos de Azure](https://www.microsoft.com/download/details.aspx?id=41653) desde el Centro de descarga de Microsoft.

> [!NOTE]
> El archivo XML de direcciones IP de los centros de datos de Azure enumera los intervalos de direcciones IP que se usan en los centros de datos de Microsoft Azure. El archivo incluye el proceso, SQL y los intervalos de almacenamiento.
>
>Semanalmente, se publica un archivo actualizado. El archivo refleja los intervalos implementados actualmente y los próximos cambios en los intervalos IP. Los nuevos intervalos que aparecen en el archivo no se utilizan en los centros de datos durante al menos una semana. Descargar el archivo XML nuevo cada semana es una buena idea. A continuación, actualice el sitio para identificar correctamente los servicios que se ejecutan en Azure. 

Los usuarios de Azure ExpressRoute deberían observar que este archivo se usa para actualizar la publicidad del Protocolo de puerta de enlace de borde (BGP) del espacio de Azure la primera semana de cada mes.

## <a name="next-steps"></a>Pasos siguientes

- Para dar los primeros pasos, consulte [Introducción a Azure Automation State Configuration](automation-dsc-getting-started.md).
- Para aprender a habilitar nodos, consulte el artículo sobre [Habilitar Azure Automation State Configuration](automation-dsc-onboarding.md).
- Para aprender a compilar configuraciones de DSC para poder asignarlas a los nodos de destino, consulte [Compilación de configuraciones de DSC en Azure Automation State Configuration](automation-dsc-compile.md).
- Para ver un ejemplo del uso de Azure Automation State Configuration en una canalización de implementación continua, vea [Configuración de la implementación continua con Chocolatey](automation-dsc-cd-chocolatey.md).
- Para obtener información de precios, consulte [Precios de State Configuration de Azure Automation](https://azure.microsoft.com/pricing/details/automation/).
- Para ver una referencia de los cmdlets de PowerShell, consulte [Az.Automation](https://docs.microsoft.com/powershell/module/az.automation/?view=azps-3.7.0#automation
).