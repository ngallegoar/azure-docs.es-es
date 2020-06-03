---
title: Introducción a Hybrid Runbook Worker de Azure Automation
description: En este artículo se ofrece información general de Hybrid Runbook Worker, que puede usar para ejecutar runbooks en máquinas de su centro de datos local o proveedor de nube.
services: automation
ms.subservice: process-automation
ms.date: 04/05/2019
ms.topic: conceptual
ms.openlocfilehash: 9305d0d6443c923c680af0d5fafc58887dadb902
ms.sourcegitcommit: 0b80a5802343ea769a91f91a8cdbdf1b67a932d3
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 05/25/2020
ms.locfileid: "83835298"
---
# <a name="hybrid-runbook-worker-overview"></a>Introducción a Hybrid Runbook Worker

Es posible que los runbooks de Azure Automation no tengan acceso a los recursos de otras nubes o del entorno local, porque se ejecutan en plataforma de nube de Azure. La característica Hybrid Runbook Worker de Azure Automation permite ejecutar runbooks directamente en el equipo que hospeda el rol y en los recursos del entorno para administrar dichos recursos locales. Los runbooks se almacenan y administran en Azure Automation y después se entregan a uno o más equipos asignados.

En la imagen siguiente se muestra esta funcionalidad:

![Introducción a Hybrid Runbook Worker](media/automation-hybrid-runbook-worker/automation.png)

Una instancia de Hybrid Runbook Worker puede ejecutar el sistema operativo Windows o Linux. Para su supervisión, requiere el uso de Azure Monitor y un agente de Log Analytics para el sistema operativo admitido. Para más información, consulte [Azure Monitor](automation-runbook-execution.md#azure-monitor).

Cada Trabajo híbrido de runbook es miembro de un grupo de Trabajos híbridos de runbook que especifica cuando instala el agente. Un grupo puede incluir solo un agente, pero puede instalar varios agentes en un grupo para contar con alta disponibilidad. Cada máquina puede hospedar una instancia de Hybrid Worker que informe a una cuenta de Automation. 

Cuando se inicia un runbook en Hybrid Runbook Worker, se especifica el grupo en el que se ejecuta. Cada rol de trabajo del grupo sondea Azure Automation para ver si hay trabajos disponibles. Si un trabajo está disponible, el primer rol trabajo en obtener el trabajo lo toma. El tiempo de procesamiento de la cola de trabajos depende de la carga y del perfil de hardware de Hybrid Worker. No se puede especificar un trabajo determinado. 

Use una instancia de Hybrid Runbook Worker en lugar de un [espacio aislado de Azure](automation-runbook-execution.md#runbook-execution-environment), ya que no tiene muchos de los [límites](../azure-resource-manager/management/azure-subscription-service-limits.md#automation-limits) del espacio aislado en el espacio en disco, la memoria o los sockets de red. Los límites de una instancia de Hybrid Worker solo están relacionados con los propios recursos del rol de trabajo. 

> [!NOTE]
> Las instancias de Hybrid Runbook Worker no están restringidas por el límite tiempo de [distribución equilibrada](automation-runbook-execution.md#fair-share) que tienen los espacios aislados de Azure. 

## <a name="hybrid-runbook-worker-installation"></a>Instalación de una instancia de Hybrid Runbook Worker

El proceso para instalar una instancia de Hybrid Runbook Worker depende del sistema operativo. En la tabla siguiente, se definen los tipos de implementación.

|Sistema operativo  |Tipos de implementación  |
|---------|---------|
|Windows     | [Automatizado](automation-windows-hrw-install.md#automated-deployment)<br>[Manual](automation-windows-hrw-install.md#manual-deployment)        |
|Linux     | [Python](automation-linux-hrw-install.md#install-a-linux-hybrid-runbook-worker)        |

El método de instalación recomendado consiste en utilizar un runbook de Azure Automation para automatizar por completo el proceso de configuración de los equipos Windows. El segundo método es un procedimiento paso a paso para instalar y configurar el rol manualmente. En el caso de las máquinas de Linux, debe ejecutar un script de Python para instalar el agente en la máquina.

## <a name="network-planning"></a><a name="network-planning"></a>Planeamiento de red

Para que Hybrid Runbook Worker se conecte y se registre con Azure Automation, debe tener acceso al número de puerto y a las direcciones URL que se describen en esta sección. El rol de trabajo también ha de tener acceso a los [puertos y las direcciones URL necesarios para que el agente de Log Analytics](../azure-monitor/platform/agent-windows.md) se conecte al área de trabajo Log Analytics de Azure Monitor.

[!INCLUDE [azure-monitor-log-analytics-rebrand](../../includes/azure-monitor-log-analytics-rebrand.md)]

Los siguientes puertos y direcciones URL son necesarios para la instancia de Hybrid Runbook Worker:

* Puerto: solo se requiere el puerto TCP 443 para el acceso a Internet
* URL global: *.azure-automation.net
* Direcciones URL globales de US Gov Virginia: *.azure-automation.us
* Servicio de agente: https://\<workspaceId\>.agentsvc.azure-automation.net

Se recomienda utilizar las direcciones mostradas al definir [excepciones](automation-runbook-execution.md#exceptions). Puede descargar los [intervalos de direcciones IP del centro de datos de Microsoft Azure](https://www.microsoft.com/en-us/download/details.aspx?id=56519). Este archivo se actualiza semanalmente y tiene los intervalos implementados en ese momento y los próximos cambios en los intervalos de direcciones IP.

### <a name="dns-records-per-region"></a>Registros DNS por región

Si tiene una cuenta de Automation definida para una región específica, puede restringir la comunicación de Hybrid Runbook Worker con ese centro de datos regional. En la tabla siguiente se proporciona el registro de DNS para cada región.

| **Región** | **Registro de DNS** |
| --- | --- |
| Centro de Australia |ac-jobruntimedata-prod-su1.azure-automation.net</br>ac-agentservice-prod-1.azure-automation.net |
| Este de Australia |ae-jobruntimedata-prod-su1.azure-automation.net</br>ae-agentservice-prod-1.azure-automation.net |
| Sudeste de Australia |ase-jobruntimedata-prod-su1.azure-automation.net</br>ase-agentservice-prod-1.azure-automation.net |
| Centro de Canadá |cc-jobruntimedata-prod-su1.azure-automation.net</br>cc-agentservice-prod-1.azure-automation.net |
| Centro de la India |cid-jobruntimedata-prod-su1.azure-automation.net</br>cid-agentservice-prod-1.azure-automation.net |
| Este de EE. UU. 2 |eus2-jobruntimedata-prod-su1.azure-automation.net</br>eus2-agentservice-prod-1.azure-automation.net |
| Japón Oriental |jpe-jobruntimedata-prod-su1.azure-automation.net</br>jpe-agentservice-prod-1.azure-automation.net |
| Norte de Europa |ne-jobruntimedata-prod-su1.azure-automation.net</br>ne-agentservice-prod-1.azure-automation.net |
| Centro-sur de EE. UU. |scus-jobruntimedata-prod-su1.azure-automation.net</br>scus-agentservice-prod-1.azure-automation.net |
| Sudeste de Asia |sea-jobruntimedata-prod-su1.azure-automation.net</br>sea-agentservice-prod-1.azure-automation.net|
| Sur de Reino Unido 2 | uks-jobruntimedata-prod-su1.azure-automation.net</br>uks-agentservice-prod-1.azure-automation.net |
| US Gov - Virginia | usge-jobruntimedata-prod-su1.azure-automation.us<br>usge-agentservice-prod-1.azure-automation.us |
| Centro-Oeste de EE. UU. | wcus-jobruntimedata-prod-su1.azure-automation.net</br>wcus-agentservice-prod-1.azure-automation.net |
| Oeste de Europa |we-jobruntimedata-prod-su1.azure-automation.net</br>we-agentservice-prod-1.azure-automation.net |
| Oeste de EE. UU. 2 |wus2-jobruntimedata-prod-su1.azure-automation.net</br>wus2-agentservice-prod-1.azure-automation.net |

Para obtener una lista de direcciones IP de regiones en lugar de nombres de regiones, descargue el archivo XML [Direcciones IP de los centros de datos de Azure](https://www.microsoft.com/download/details.aspx?id=41653) desde el Centro de descarga de Microsoft. Semanalmente, se publica un archivo de direcciones IP actualizado. 

El archivo de direcciones IP muestra los intervalos de direcciones IP que se usan en los centros de datos de Microsoft Azure. Incluye intervalos de proceso, SQL y almacenamiento, y refleja los intervalos implementados actualmente y los próximos cambios en los intervalos IP. Los nuevos intervalos que aparecen en el archivo no se utilizan en los centros de datos durante al menos una semana.

Es conveniente descargar el nuevo archivo de direcciones IP cada semana. A continuación, actualice el sitio para identificar correctamente los servicios que se ejecutan en Azure. 

> [!NOTE]
> Si utiliza Azure ExpressRoute, recuerde que el archivo de direcciones IP se usa para actualizar la publicidad del Protocolo de puerta de enlace de borde (BGP) del espacio de Azure la primera semana de cada mes.

### <a name="proxy-server-use"></a>Uso del servidor proxy

Si se usa un servidor proxy para realizar la comunicación entre Azure Automation y el agente de Log Analytics, asegúrese de que sea posible acceder a los recursos adecuados. El tiempo de expiración para las solicitudes de Hybrid Runbook Worker y los servicios de Automation es de 30 segundos. Después de tres intentos, se produce un error en una solicitud. 

### <a name="firewall-use"></a>Uso del firewall

Si usa un firewall para restringir el acceso a Internet, tendrá que configurarlo para que permita el acceso. Si usa la puerta de enlace de Log Analytics como proxy, asegúrese de que está configurada para instancias de Hybrid Runbook Worker. Consulte el artículo [Conexión de equipos sin acceso a Internet mediante la puerta de enlace de Log Analytics en Azure Monitor](https://docs.microsoft.com/azure/log-analytics/log-analytics-oms-gateway).

## <a name="update-management-on-hybrid-runbook-worker"></a>Update Management en Hybrid Runbook Worker

Si habilita [Update Management](automation-update-management.md) en Azure Automation, los equipos conectados al área de trabajo de Log Analytics se configurarán automáticamente como Hybrid Runbook Worker. Cada rol de trabajo puede admitir runbooks destinados a Update Management. 

Un equipo configurado de esta manera no se registra en ningún grupo de Hybrid Runbook Worker ya definido en la cuenta de Automation. Puede agregar el equipo a un grupo de Hybrid Runbook Worker, pero debe usar la misma cuenta para Update Management y la pertenencia al grupo de Hybrid Runbook Worker. Esta funcionalidad se agregó a la versión 7.2.12024.0 de Hybrid Runbook Worker.

### <a name="update-management-addresses-for-hybrid-runbook-worker"></a>Direcciones de Update Management para Hybrid Runbook Worker

Además de las direcciones y los puertos estándar que necesita Hybrid Runbook Worker, Update Management necesita las direcciones de la tabla siguiente. La comunicación con estas direcciones usa el puerto 443.

|Azure Public  |Azure Government  |
|---------|---------|
|*.ods.opinsights.azure.com     | *.ods.opinsights.azure.us         |
|*.oms.opinsights.azure.com     | *.oms.opinsights.azure.us        |
|*.blob.core.windows.net | *.blob.core.usgovcloudapi.net|

## <a name="azure-automation-state-configuration-on-a-hybrid-runbook-worker"></a>State Configuration de Azure Automation en una instancia de Hybrid Runbook Worker

Puede ejecutar [State Configuration de Azure Automation](automation-dsc-overview.md) en una instancia de Hybrid Runbook Worker. Para administrar la configuración de los servidores que admiten la instancia de Hybrid Runbook Worker, debe agregar dichos servidores como nodos de DSC. Consulte el artículo [Incorporación de máquinas para su administración mediante Azure Automation State Configuration](automation-dsc-onboarding.md).

## <a name="runbooks-on-a-hybrid-runbook-worker"></a>Runbooks en Hybrid Runbook Worker

Podría tener runbooks que administren recursos en el equipo local o se ejecuten con recursos en el entorno local en el que se implementa una instancia de Hybrid Runbook Worker. En este caso, puede elegir ejecutar los runbooks en la instancia de Hybrid Worker en lugar de en una cuenta de Automation. La estructura de los runbooks que se ejecutan en una instancia de Hybrid Runbook Worker es idéntica a la de los que ejecuta en la cuenta de Automation. Consulte [Ejecución de runbooks en Hybrid Runbook Worker](automation-hrw-run-runbooks.md).

### <a name="hybrid-runbook-worker-jobs"></a>Trabajos de Hybrid Runbook Worker

Los trabajos de Hybrid Runbook Worker se ejecutan en la cuenta del **sistema** local en Windows o en la [cuenta nxautomation](automation-runbook-execution.md#log-analytics-agent-for-linux) en Linux. Azure Automation controla los trabajos en Hybrid Runbook Worker de forma ligeramente diferente a los trabajos que se ejecutan en espacios aislados de Azure. Consulte [Dónde ejecutar los runbooks](automation-runbook-execution.md#runbook-execution-environment).

Si la máquina host de Hybrid Runbook Worker se reinicia, cualquier trabajo de runbook en ejecución se reinicia desde el principio o desde el último punto de comprobación para runbooks de Flujo de trabajo de PowerShell. Si un trabajo de runbook se reinicia más de tres veces, se suspende.

### <a name="runbook-permissions-for-a-hybrid-runbook-worker"></a>Permisos del runbook para una instancia de Hybrid Runbook Worker

Puesto que acceden a recursos que no son de Azure, los runbooks que se ejecutan en una instancia de Hybrid Runbook Worker no pueden usar el mecanismo de autenticación que usan normalmente los runbooks que se autentican en los recursos de Azure. Un runbook proporciona su propia autenticación a los recursos locales o configura la autenticación mediante [identidades administradas para los recursos de Azure](../active-directory/managed-identities-azure-resources/tutorial-windows-vm-access-arm.md#grant-your-vm-access-to-a-resource-group-in-resource-manager). También puede especificar una cuenta de ejecución para proporcionar un contexto de usuario para todos los runbooks.

## <a name="next-steps"></a>Pasos siguientes

* Consulte [Ejecución de runbooks en Hybrid Runbook Worker](automation-hrw-run-runbooks.md) para más información sobre cómo configurar los runbooks para automatizar los procesos del centro de datos local o de otros entornos de nube.
* Para aprender a solucionar problemas con las instancias de Hybrid Runbook Worker, consulte [Solución de incidencias de Hybrid Runbook Worker](troubleshoot/hybrid-runbook-worker.md#general).