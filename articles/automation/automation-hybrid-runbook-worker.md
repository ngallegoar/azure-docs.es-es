---
title: Hybrid Runbook Worker de Azure Automation
description: Este artículo brinda información sobre cómo instalar y usar Hybrid Runbook Worker, una característica de Azure Automation que puede usar para ejecutar runbooks en máquinas de su centro de datos local o proveedor de la nube.
services: automation
ms.subservice: process-automation
ms.date: 04/05/2019
ms.topic: conceptual
ms.openlocfilehash: 861c7ffa647c8d2f37b32c359253ca991eeb314f
ms.sourcegitcommit: b55d7c87dc645d8e5eb1e8f05f5afa38d7574846
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 04/16/2020
ms.locfileid: "81457712"
---
# <a name="automate-resources-in-your-datacenter-or-cloud-by-using-hybrid-runbook-worker"></a>Automatización de recursos en los centros de datos o nube con Hybrid Runbook Worker

Es posible que los runbooks de Azure Automation no tengan acceso a los recursos de otras nubes o del entorno local, porque se ejecutan en plataforma de nube de Azure. La característica Hybrid Runbook Worker de Azure Automation permite ejecutar runbooks directamente en el equipo que hospeda el rol y en los recursos del entorno para administrar dichos recursos locales. Los runbooks se almacenan y administran en Azure Automation y después se entregan a uno o más equipos asignados.

En la imagen siguiente se muestra esta funcionalidad:

![Introducción a Hybrid Runbook Worker](media/automation-hybrid-runbook-worker/automation.png)

Cada Trabajo híbrido de runbook es miembro de un grupo de Trabajos híbridos de runbook que especifica cuando instala el agente. Un grupo puede incluir solo un agente, pero puede instalar varios agentes en un grupo para contar con alta disponibilidad. Cada máquina puede hospedar una instancia de Hybrid Worker que informe a una cuenta de Automation.

Cuando se inicia un runbook en Hybrid Runbook Worker, se especifica el grupo en el que se ejecuta. Cada rol de trabajo del grupo sondea Azure Automation para ver si hay trabajos disponibles. Si un trabajo está disponible, el primer rol trabajo en obtener el trabajo lo toma. El tiempo de procesamiento de la cola de trabajos depende de la carga y del perfil de hardware de Hybrid Worker. No se puede especificar un trabajo determinado. Las instancias de Hybrid Runbook Worker no comparten muchos de los límites que tienen los espacios aislados de Azure. No tienen los mismos límites de espacio en disco, memoria o sockets de red. Las instancias de Hybrid Runbook Worker solo se ven limitadas por los recursos de Hybrid Runbook Worker propiamente dicho. Además, las instancias de Hybrid Runbook Worker no comparten el límite de tiempo de 180 minutos de [distribución equilibrada](automation-runbook-execution.md#fair-share) que comparten los espacios aislados de Azure. Para conocer mejor los límites de servicio de los espacios aislados de Azure y las instancias de Hybrid Runbook Worker, consulte la página de [límites](../azure-resource-manager/management/azure-subscription-service-limits.md#automation-limits) del trabajo.

## <a name="install-a-hybrid-runbook-worker"></a>Instalación de una instancia de Hybrid Runbook Worker

El proceso para instalar una instancia de Hybrid Runbook Worker depende del sistema operativo. En la tabla siguiente, se definen los tipos de implementación.

|SO  |Tipos de implementación  |
|---------|---------|
|Windows     | [PowerShell](automation-windows-hrw-install.md#automated-deployment)<br>[Manual](automation-windows-hrw-install.md#manual-deployment)        |
|Linux     | [Python](automation-linux-hrw-install.md#installing-a-linux-hybrid-runbook-worker)        |

El método de instalación recomendado consiste en utilizar un runbook de Automation para automatizar completamente el proceso de configuración de los equipos Windows. El segundo método es un procedimiento paso a paso para instalar y configurar el rol manualmente. En el caso de las máquinas de Linux, debe ejecutar un script de Python para instalar el agente en la máquina.

> [!NOTE]
> Para administrar la configuración de los servidores que admiten el rol de Hybrid Runbook Worker con Desired State Configuration (DSC), debe agregar los servidores como nodos de DSC. Para obtener más información sobre su incorporación para la administración con DSC, consulte [Incorporación de máquinas para administrarlas con DSC de Azure Automation](automation-dsc-onboarding.md).
>
>Si habilita la [solución Update Management](automation-update-management.md), los equipos conectados al área de trabajo de Azure Log Analytics se configurarán automáticamente como una instancia de Hybrid Runbook Worker para admitir los runbooks que se incluyen en esta solución. Sin embargo, el equipo no está registrado en ningún grupo de Hybrid Worker ya definido en la cuenta de Automation. El equipo se puede agregar a un grupo de Hybrid Runbook Worker en la cuenta de Automation para admitir los runbooks de Automation siempre que use la misma cuenta para la solución y la pertenencia a grupos de Hybrid Runbook Worker. Esta funcionalidad se agregó a la versión 7.2.12024.0 de Hybrid Runbook Worker.

Revise la [información para planear la red](#network-planning) antes de empezar a implementar una instancia de Hybrid Runbook Worker. Después de haber implementado correctamente el trabajo, revise la [ejecución de runbooks en Hybrid Runbook Worker](automation-hrw-run-runbooks.md) para más información sobre cómo configurar los runbooks para automatizar los procesos del centro de datos local o en otro entorno de nube.

El equipo se puede agregar a un grupo de Hybrid Runbook Worker en la cuenta de Automation para admitir los runbooks de Automation siempre que use la misma cuenta para la solución y la pertenencia a grupos de Hybrid Runbook Worker. Esta funcionalidad se agregó a la versión 7.2.12024.0 de Hybrid Runbook Worker.

## <a name="a-nameremove-a-hybrid-runbook-workerremove-a-hybrid-runbook-worker-from-an-on-premises-computer"></a><a name="remove-a-hybrid-runbook-worker">Eliminación de Hybrid Runbook Worker de un equipo local

Puede quitar una instancia de Hybrid Runbook Worker de un equipo local tal y como se describe en esta sección dedicada a Windows y Linux.

### <a name="remove-the-worker-on-windows"></a>Eliminación del trabajo en Windows

1. En Azure Portal, abra su cuenta de Automation.
2. En **Configuración de la cuenta**, seleccione **Claves** y anote los valores de **URL** y **Clave de acceso primaria**.

3. Abra una sesión de PowerShell en modo de administrador y ejecute el siguiente comando con los valores de la dirección URL y la clave de acceso principal. Utilice el parámetro `Verbose` para ver un registro detallado del proceso de eliminación. Para eliminar máquinas obsoletas del grupo Hybrid Worker, use el parámetro `machineName` opcional.

```powershell-interactive
Remove-HybridRunbookWorker -url <URL> -key <PrimaryAccessKey> -machineName <ComputerName>
```

### <a name="remove-the-worker-on-linux"></a>Eliminación del trabajo en Linux

Puede usar el comando `ls /var/opt/microsoft/omsagent` en Hybrid Runbook Worker para obtener el identificador del área de trabajo. Se creará una carpeta cuyo nombre será el identificador del área de trabajo.

```bash
sudo python onboarding.py --deregister --endpoint="<URL>" --key="<PrimaryAccessKey>" --groupname="Example" --workspaceid="<workspaceId>"
```

> [!NOTE]
> Este código no quita el agente de Log Analytics para Linux del equipo. Solo quita la funcionalidad y la configuración del rol de Hybrid Runbook Worker.

## <a name="remove-a-hybrid-worker-group"></a>Eliminación de un grupo de Hybrid Worker

Para quitar un grupo de Hybrid Runbook Worker, primero debe quitar la instancia de Hybrid Runbook Worker de todos los equipos que sean miembros del grupo. Después, siga estos pasos para quitar el grupo:

1. Abra la cuenta de Automation en Azure Portal.
2. Seleccione **Grupos de Hybrid Worker** en **Automatización de procesos**. Seleccione el grupo que quiere eliminar. Aparece la página de propiedades de ese grupo.

   ![Página de propiedades](media/automation-hybrid-runbook-worker/automation-hybrid-runbook-worker-group-properties.png)

3. En la página de propiedades del grupo seleccionado, seleccione **Eliminar**. Un mensaje le solicita que confirme esta acción. Seleccione **Sí** si está seguro de que quiere continuar.

   ![Mensaje de confirmación](media/automation-hybrid-runbook-worker/automation-hybrid-runbook-worker-confirm-delete.png)

   Este proceso puede tardar varios segundos en finalizar. Puede realizar un seguimiento de su progreso en **Notificaciones** en el menú.

## <a name="configure-your-network"></a><a name="network-planning"></a>Configure la red

### <a name="hybrid-worker-role"></a>Rol de Hybrid Worker

Para que Hybrid Runbook Worker se conecte y se registre con Azure Automation, debe tener acceso al número de puerto y a las direcciones URL que se describen en esta sección. Este acceso se agrega a los [puertos y las direcciones URL necesarios para que el agente de Log Analytics](../azure-monitor/platform/agent-windows.md) se conecte a los registros de Azure Monitor.

[!INCLUDE [azure-monitor-log-analytics-rebrand](../../includes/azure-monitor-log-analytics-rebrand.md)]

Si usa un servidor proxy para realizar la comunicación entre el agente y el servicio Azure Automation, asegúrese de que los recursos adecuados son accesibles. El tiempo de espera para las solicitudes de Hybrid Runbook Worker y los servicios de Automation es de 30 segundos. Después de tres intentos se producirá un error en la solicitud. Si usa un firewall para restringir el acceso a Internet, tendrá que configurarlo para que permita el acceso. Si usa la puerta de enlace de Log Analytics como proxy, asegúrese de que está configurada para instancias de Hybrid Worker. Para obtener instrucciones sobre cómo hacerlo, vea [Configuración de Hybrid Workers de Automation](https://docs.microsoft.com/azure/log-analytics/log-analytics-oms-gateway).

Los siguientes puertos y direcciones URL son necesarios para que el rol Hybrid Runbook Worker se comunique con Automation:

* Puerto: solo se requiere el puerto TCP 443 para el acceso a Internet.
* URL global: *.azure-automation.net
* Direcciones URL globales de US Gov Virginia: *.azure-automation.us
* Servicio de agente: https://\<workspaceId\>.agentsvc.azure-automation.net

Se recomienda utilizar las direcciones mostradas al definir las excepciones. Puede descargar los [intervalos de direcciones IP del centro de datos de Microsoft Azure](https://www.microsoft.com/en-us/download/details.aspx?id=56519). Este archivo se actualiza semanalmente y tiene los intervalos implementados en ese momento y los próximos cambios en los intervalos de direcciones IP.

Si tiene una cuenta de Automation definida para una región específica, puede restringir la comunicación con ese centro de datos regional. En la tabla siguiente se proporciona el registro de DNS para cada región:

| **Región** | **Registro de DNS** |
| --- | --- |
| Centro-Oeste de EE. UU. | wcus-jobruntimedata-prod-su1.azure-automation.net</br>wcus-agentservice-prod-1.azure-automation.net |
| Centro-sur de EE. UU. |scus-jobruntimedata-prod-su1.azure-automation.net</br>scus-agentservice-prod-1.azure-automation.net |
| Este de EE. UU. 2 |eus2-jobruntimedata-prod-su1.azure-automation.net</br>eus2-agentservice-prod-1.azure-automation.net |
| Oeste de EE. UU. 2 |wus2-jobruntimedata-prod-su1.azure-automation.net</br>wus2-agentservice-prod-1.azure-automation.net |
| Centro de Canadá |cc-jobruntimedata-prod-su1.azure-automation.net</br>cc-agentservice-prod-1.azure-automation.net |
| Oeste de Europa |we-jobruntimedata-prod-su1.azure-automation.net</br>we-agentservice-prod-1.azure-automation.net |
| Norte de Europa |ne-jobruntimedata-prod-su1.azure-automation.net</br>ne-agentservice-prod-1.azure-automation.net |
| Sudeste de Asia |sea-jobruntimedata-prod-su1.azure-automation.net</br>sea-agentservice-prod-1.azure-automation.net|
| Centro de la India |cid-jobruntimedata-prod-su1.azure-automation.net</br>cid-agentservice-prod-1.azure-automation.net |
| Japón Oriental |jpe-jobruntimedata-prod-su1.azure-automation.net</br>jpe-agentservice-prod-1.azure-automation.net |
| Centro de Australia |ac-jobruntimedata-prod-su1.azure-automation.net</br>ac-agentservice-prod-1.azure-automation.net |
| Este de Australia |ae-jobruntimedata-prod-su1.azure-automation.net</br>ae-agentservice-prod-1.azure-automation.net |
| Sudeste de Australia |ase-jobruntimedata-prod-su1.azure-automation.net</br>ase-agentservice-prod-1.azure-automation.net |
| Sur de Reino Unido 2 | uks-jobruntimedata-prod-su1.azure-automation.net</br>uks-agentservice-prod-1.azure-automation.net |
| US Gov - Virginia | usge-jobruntimedata-prod-su1.azure-automation.us<br>usge-agentservice-prod-1.azure-automation.us |

Para obtener una lista de direcciones IP de regiones en lugar de nombres de regiones, descargue el archivo XML [Direcciones IP de los centros de datos de Azure](https://www.microsoft.com/download/details.aspx?id=41653) desde el Centro de descarga de Microsoft.

> [!NOTE]
> El archivo XML de direcciones IP de los centros de datos de Azure enumera los intervalos de direcciones IP que se usan en los centros de datos de Microsoft Azure. El archivo incluye el proceso, SQL y los intervalos de almacenamiento.
>
>Semanalmente, se publica un archivo actualizado. El archivo refleja los intervalos implementados actualmente y los próximos cambios en los intervalos IP. Los nuevos intervalos que aparecen en el archivo no se utilizan en los centros de datos durante al menos una semana.
>
> Descargar el archivo XML nuevo cada semana es una buena idea. A continuación, actualice el sitio para identificar correctamente los servicios que se ejecutan en Azure. Los usuarios de Azure ExpressRoute deberían observar que este archivo se usa para actualizar la publicidad del Protocolo de puerta de enlace de borde (BGP) del espacio de Azure la primera semana de cada mes.

### <a name="update-management"></a>Administración de actualizaciones

Además de las direcciones y los puertos estándar que necesita Hybrid Runbook Worker, las direcciones siguientes se requieren específicamente para Update Management. La comunicación con estas direcciones se realiza a través del puerto 443.

|Azure Public  |Azure Government  |
|---------|---------|
|*.ods.opinsights.azure.com     |*.ods.opinsights.azure.us         |
|*.oms.opinsights.azure.com     | *.oms.opinsights.azure.us        |
|*.blob.core.windows.net|*.blob.core.usgovcloudapi.net|

## <a name="next-steps"></a>Pasos siguientes

* Consulte [Ejecución de runbooks en Hybrid Runbook Worker](automation-hrw-run-runbooks.md) para más información sobre cómo configurar los runbooks para automatizar los procesos del centro de datos local o de otros entornos de nube.
* Para más información acerca de cómo solucionar problemas con las instancias de Hybrid Runbook Worker, consulte [Solución de problemas de Hybrid Runbook Worker](troubleshoot/hybrid-runbook-worker.md#general).