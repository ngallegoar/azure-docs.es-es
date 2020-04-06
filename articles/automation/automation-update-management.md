---
title: Solución Update Management de Azure
description: En este artículo se describe cómo usar la solución Azure Update Management para administrar las actualizaciones de las máquinas Windows y Linux.
services: automation
ms.subservice: update-management
ms.date: 02/27/2020
ms.topic: conceptual
ms.openlocfilehash: c76b14e4f08ec930159498da4a35fdad0341929e
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/28/2020
ms.locfileid: "79235480"
---
# <a name="update-management-solution-in-azure"></a>Solución Update Management de Azure

Puede usar la solución Update Management de Azure Automation para administrar las actualizaciones del sistema operativo de las máquinas Windows y Linux de Azure, en entornos locales y en otros entornos en la nube. Puede evaluar rápidamente el estado de las actualizaciones disponibles en todas las máquinas agente y administrar el proceso de instalación de las actualizaciones necesarias para los servidores.

Puede habilitar Update Management para las máquinas virtuales (VM) mediante los siguientes métodos:

- Desde la [cuenta de Azure Automation](automation-onboard-solutions-from-automation-account.md) para una o varias máquinas de Azure y manualmente para máquinas que no son de Azure.

- Para una sola máquina virtual de Azure desde la página de la máquina virtual en Azure Portal. Este escenario está disponible para las VM [Linux](../virtual-machines/linux/tutorial-config-management.md#enable-update-management) y [Windows](../virtual-machines/windows/tutorial-config-management.md#enable-update-management).

- Para [varias máquinas virtuales de Azure](manage-update-multi.md), selecciónelas en la página **Máquinas virtuales** de Azure Portal. 

> [!NOTE]
> La solución de Update Management requiere vincular un área de trabajo de Log Analytics a la cuenta de Automation. Para ver una lista definitiva de regiones admitidas, consulte [Asignaciones de áreas de trabajo](./how-to/region-mappings.md). Las asignaciones de regiones no afectan a la capacidad de administrar VM en una región independiente de la cuenta de Automation.

[!INCLUDE [azure-monitor-log-analytics-rebrand](../../includes/azure-monitor-log-analytics-rebrand.md)]

Hay disponible una [plantilla de Azure Resource Manager](automation-update-management-deploy-template.md) que le permite implementar la solución Update Management en una cuenta de Automation nueva o existente y en el área de trabajo de Log Analytics de la suscripción.

## <a name="solution-overview"></a>Información general de la solución

Las máquinas administradas por Update Management usan las siguientes configuraciones para realizar valoraciones y actualizar implementaciones:

* Agente de Log Analytics para Windows o Linux
* Extensión DSC (configuración de estado deseado) de PowerShell para Linux
* Hybrid Runbook Worker de Automation
* Microsoft Update o Windows Server Update Services (WSUS) para máquinas Windows

El siguiente diagrama muestra cómo la solución evalúa y aplica las actualizaciones de seguridad a todas las máquinas Windows Server y Linux conectadas de un área de trabajo:

![Flujo del proceso de Update Management](./media/automation-update-management/update-mgmt-updateworkflow.png)

Se puede utilizar Update Management para incorporar de forma nativa máquinas en varias suscripciones en el mismo inquilino.

Después de publicarse un paquete, la revisión tarda de 2 a 3 horas en aparecer en las máquinas Linux para su evaluación. Para las máquinas Windows, la revisión tarda de 12 a 15 horas en aparecer para su evaluación tras su publicación.

Después de que una máquina completa un examen de cumplimiento de las actualizaciones, el agente reenvía la información de forma masiva a los registros de Azure Monitor. En una máquina Windows, el examen de cumplimiento se ejecuta cada 12 horas de forma predeterminada.

Además del examen programado, el examen de cumplimiento de las actualizaciones se inicia a los 15 minutos del reinicio del agente de Log Analytics, así como antes y después de la instalación de actualizaciones.

En una máquina Linux, el examen de cumplimiento se realiza cada hora de manera predeterminada. Si se reinicia el agente de Log Analytics, se inicia un examen de cumplimiento al cabo de 15 minutos.

La solución informa del grado de actualización de la máquina en función del origen configurado para la sincronización. Si la máquina Windows está configurada para informar a WSUS, en función de cuándo WSUS se sincronizó por última vez con Microsoft Update, los resultados pueden diferir de lo que se muestra en Microsoft Update. Este comportamiento es el mismo para las máquinas Linux que están configuradas para informar a un repositorio local en lugar de a uno público.

> [!NOTE]
> Para informar correctamente al servicio, Update Management requiere la habilitación de ciertas direcciones URL y puertos. Para más información sobre estos requisitos, consulte [Planeamiento de red para Hybrid Worker](automation-hybrid-runbook-worker.md#network-planning).

Puede implementar e instalar las actualizaciones de software en las máquinas que requieren las actualizaciones mediante la creación de una implementación programada. Las actualizaciones clasificadas como *Opcional* no se incluyen en el ámbito de implementación en máquinas Windows. Solo se incluyen las actualizaciones necesarias.

La implementación programada define qué máquina de destino reciben las actualizaciones aplicables. Lo hace mediante la especificación explícita de determinadas máquinas o por medio de la selección de un [grupo de equipos](../azure-monitor/platform/computer-groups.md) que se basa en las búsquedas de registros de un conjunto determinado de máquinas (o en una [consulta de Azure](automation-update-management-query-logs.md) que selecciona de forma dinámica las máquinas virtuales de Azure en función de los criterios especificados). Estos grupos son diferentes de la [configuración de ámbito](../azure-monitor/insights/solution-targeting.md), que solo se usa para determinar qué máquinas obtienen los módulos de administración que habilitan la solución.

También se especifica una programación para aprobar y establecer un período de tiempo durante el que se pueden instalar actualizaciones. Este período se denomina ventana de mantenimiento. Un intervalo de 20 minutos de la ventana de mantenimiento se reserva para los reinicios, suponiendo que sea necesario reiniciar y que haya seleccionado la opción de reinicio adecuada. Si la aplicación de revisiones tarda más de lo esperado y la ventana de mantenimiento dura menos de 20 minutos, no se producirá un reinicio.

Los Runbooks instalan las actualizaciones en Azure Automation. No puede ver estos runbooks, que no requieren ninguna configuración. Cuando se crea una implementación de actualizaciones, esta crea una programación que inicia un runbook de actualización maestro a la hora especificada para las máquinas incluidas. El runbook maestro inicia un runbook secundario en cada agente para instalar las actualizaciones necesarias.

En la fecha y hora especificadas en la implementación de actualizaciones, las máquinas de destino ejecutan la implementación en paralelo. Antes de la instalación, se ejecuta un examen para comprobar que las actualizaciones siguen siendo necesarias. En las máquinas cliente de WSUS, si no se aprueban las actualizaciones en WSUS, se produce un error en la implementación de actualizaciones.

No se permite tener registrada una máquina para Update Management en más de un área de trabajo de Log Analytics (también conocido como hospedaje múltiple).

## <a name="clients"></a>Clientes

### <a name="supported-client-types"></a>Tipos de cliente admitidos

En la tabla siguiente se muestra una lista de sistemas operativos compatibles para evaluaciones de actualizaciones. La aplicación de revisiones requiere Hybrid Runbook Worker. Para obtener información sobre los requisitos de Hybrid Runbook Worker, consulte las guías de instalación para [HRW en Windows](automation-windows-hrw-install.md) y [HRW en Linux](automation-linux-hrw-install.md#installing-a-linux-hybrid-runbook-worker).

|Sistema operativo  |Notas  |
|---------|---------|
|Windows Server 2019 (Datacenter/Datacenter Core/Standard)<br><br>Windows Server 2016 (Datacenter/Datacenter Core/Standard)<br><br>Windows Server 2012 R2 (Datacenter/Standard)<br><br>Windows Server 2012 || 
|Windows Server 2008 R2 (RTM and SP1 Standard)| Update Management solo permite realizar valoraciones de este sistema operativo. No permite aplicar revisiones, ya que [Hybrid Runbook Worker](automation-windows-hrw-install.md) no es compatible con Windows Server 2008 R2. |
|CentOS 6 (x86/x64) y 7 (x64)      | Los agentes de Linux requieren acceso a un repositorio de actualización. La aplicación de revisiones basada en la clasificación requiere `yum` para devolver los datos de seguridad que CentOS no tiene en sus versiones RTM. Para más información sobre la aplicación de revisiones basadas en clasificaciones en CentOS, consulte [Actualización de clasificaciones en Linux](automation-view-update-assessments.md#linux-2).          |
|Red Hat Enterprise (x86/x64) 6 y 7 (x64)     | Los agentes de Linux requieren acceso a un repositorio de actualización.        |
|SUSE Linux Enterprise Server 11 (x86/x64) y 12 (x64)     | Los agentes de Linux requieren acceso a un repositorio de actualización.        |
|Ubuntu 14.04 LTS, 16.04 LTS y 18.04 (x86/x64)      |Los agentes de Linux requieren acceso a un repositorio de actualización.         |

> [!NOTE]
> Los conjuntos de escalado de máquinas virtuales de Azure se pueden administrar a través de Update Management. Update Management funciona en las propias instancias y no en la imagen base. Deberá programar las actualizaciones de forma incremental, de modo que no todas las instancias de VM se actualicen a la vez.
> Puede agregar nodos para conjuntos de escalado de máquinas virtuales siguiendo los pasos que se describen en [Incorporación de una máquina que no es de Azure](automation-tutorial-installed-software.md#onboard-a-non-azure-machine).

### <a name="unsupported-client-types"></a>Tipos de cliente no admitidos

En la tabla siguiente se enumeran los sistemas operativos no admitidos:

|Sistema operativo  |Notas  |
|---------|---------|
|Cliente Windows     | No se admiten sistemas operativos cliente (por ejemplo, Windows 7 y Windows 10).        |
|Windows Server 2016 Nano Server     | No compatible.       |
|Nodos de Azure Kubernetes Service | No compatible. Use el proceso de aplicación de revisiones que se describe en [Aplicación de actualizaciones de kernel y seguridad en los nodos de Linux en Azure Kubernetes Service (AKS)](../aks/node-updates-kured.md)|

### <a name="client-requirements"></a>Requisitos del cliente

La información siguiente describe los requisitos del cliente específicos del sistema operativo. Para obtener más instrucciones, vea [Planeamiento de red](#ports).

#### <a name="windows"></a>Windows

Los agentes de Windows deben estar configurados para comunicarse con un servidor de WSUS o requieren acceso a Microsoft Update.

Puede utilizar Update Management con Configuration Manager. Para más información sobre escenarios de integración, consulte este artículo sobre la [integración de Configuration Manager con Update Management](oms-solution-updatemgmt-sccmintegration.md#configuration). Es necesario el [agente de Windows de Log Analytics](../azure-monitor/platform/agent-windows.md). Este agente se instala automáticamente si está incorporando una VM de Azure.

De forma predeterminada, las VM Windows implementadas desde Azure Marketplace se establecen para recibir actualizaciones automáticas del servicio de Windows Update. Este comportamiento no cambia cuando se agrega esta solución o se agregan VM Windows al área de trabajo. Si no administra activamente las actualizaciones mediante esta solución, se aplica el comportamiento predeterminado (las actualizaciones se aplican automáticamente).

> [!NOTE]
> Un usuario puede modificar la directiva de grupo para que los reinicios de la máquina solo los pueda realizar el usuario, no el sistema. Es posible que las máquinas administradas se bloqueen si Update Management no tiene derechos para reiniciar la máquina sin la intervención manual del usuario.
>
> Para obtener más información, consulte el tema sobre la [configuración de la directiva de grupo para actualizaciones automáticas](https://docs.microsoft.com/windows-server/administration/windows-server-update-services/deploy/4-configure-group-policy-settings-for-automatic-updates).

#### <a name="linux"></a>Linux

En Linux, la máquina requiere acceso a un repositorio de actualización. El repositorio de actualización puede ser público o privado. Se requiere TLS 1.1 o TLS 1.2 para interactuar con Update Management. Un agente de Log Analytics para Linux que esté configurado para informar a varias áreas de trabajo de Log Analytics no es compatible con esta solución. La máquina también debe tener instalado Python 2.x.

Para más información sobre cómo instalar el agente de Log Analytics para Linux y descargar la versión más reciente, consulte [Agente de Log Analytics para Linux](../azure-monitor/platform/agent-linux.md). Para más información sobre cómo instalar el agente de Log Analytics para Windows, consulte [Conexión de equipos Windows a Azure Monitor](../log-analytics/log-analytics-windows-agent.md).

Las VM que se crearon a partir de las imágenes a petición de Red Hat Enterprise Linux (RHEL) que están disponibles en Azure Marketplace están registradas para acceder a la instancia de [Red Hat Update Infrastructure (RHUI)](../virtual-machines/workloads/redhat/redhat-rhui.md) implementada en Azure. Cualquier otra distribución de Linux se debe actualizar desde el repositorio de archivos en línea de la distribución mediante los métodos de distribución admitidos.

## <a name="permissions"></a>Permisos

Para crear y administrar implementaciones de actualizaciones, necesita permisos concretos. Para más información sobre estos permisos, consulte [Acceso basado en rol: Update Management](automation-role-based-access-control.md#update-management).

## <a name="solution-components"></a>Componentes de soluciones

La solución consta de los siguientes recursos. Estos recursos se agregan automáticamente a la cuenta de Automation al habilitar la solución. 

### <a name="hybrid-worker-groups"></a>Grupos de Hybrid Worker

Después de habilitar esta solución, las máquinas Windows conectadas directamente al área de trabajo de Log Analytics se configuran automáticamente como Hybrid Runbook Worker para admitir los runbooks que se incluyen en esta solución.

Cada máquina Windows administrada por la solución se muestra en el panel **Grupos de Hybrid Worker** como un **grupo Hybrid Worker del sistema** para la cuenta de Automation. Las soluciones usan la convención de nomenclatura *FQDN_GUID de nombre de host*. No puede usar estos grupos como destino con runbooks de su cuenta. Si lo intenta, se producirá un error. Estos grupos están diseñados únicamente para admitir esta solución de administración.

Puede agregar la máquina Windows a un grupo de Hybrid Runbook Worker en la cuenta de Automation para admitir runbooks de Automation si usa la misma cuenta para la solución y la pertenencia a grupos de Hybrid Runbook Worker. Esta funcionalidad se agregó en la versión 7.2.12024.0 de Hybrid Runbook Worker.

### <a name="management-packs"></a>Módulos de administración

Si el grupo de administración de System Center Operations Manager está [conectado a un área de trabajo de Log Analytics](../azure-monitor/platform/om-agents.md), se instalarán los siguientes módulos de administración en Operations Manager. Estos módulos de administración también se instalan en las máquinas Windows conectadas directamente después de agregar la solución. No es necesario configurar ni administrar dichos módulos.

* Intelligence Pack Update Assessment de Microsoft System Center Advisor (Microsoft.IntelligencePacks.UpdateAssessment)
* Microsoft.IntelligencePack.UpdateAssessment.Configuration (Microsoft.IntelligencePack.UpdateAssessment.Configuration)
* Módulo de administración de Update Deployment

> [!NOTE]
> Si tiene un grupo de administración de Operations Manager 1807 o 2019 conectado a un área de trabajo de Log Analytics con agentes configurados en el grupo de administración para recopilar los datos de registro, debe invalidar la regla siguiente para administrarlos con Update Management: Invalide el parámetro **IsAutoRegistrationEnabled** y establézcalo en **True** en la regla **Microsoft.IntelligencePacks.AzureAutomation.HybridAgent.Init**.

Para más información sobre cómo se actualizan los módulos de administración de soluciones, consulte [Conexión de Operations Manager con registros de Azure Monitor](../azure-monitor/platform/om-agents.md).

> [!NOTE]
> Para máquinas con el agente de Operations Manager, se debe actualizar el agente al agente de Log Analytics para Windows o Linux para que Update Management pueda administrarlo por completo. Para aprender a actualizar el agente, consulte [Actualización de Operations Manager Agent](https://docs.microsoft.com/system-center/scom/deploy-upgrade-agents). En entornos que usan Operations Manager, debe ejecutar System Center Operations Manager 2012 R2 UR 14 o posterior.

## <a name="data-collection"></a>datos, recopilación

### <a name="supported-agents"></a>Agentes admitidos

En la tabla siguiente se describen los orígenes conectados compatibles con esta solución:

| Origen conectado | Compatible | Descripción |
| --- | --- | --- |
| Agentes de Windows |Sí |La solución recopila información sobre las actualizaciones del sistema de los agentes de Windows e inicia a continuación la instalación de las actualizaciones necesarias. |
| Agentes de Linux |Sí |La solución recopila información sobre las actualizaciones del sistema de los agentes de Linux e inicia a continuación la instalación de las actualizaciones necesarias en las distribuciones admitidas. |
| Grupo de administración de Operations Manager |Sí |La solución recopila información acerca de las actualizaciones del sistema de agentes en un grupo de administración conectado.<br/><br/>No se requiere ninguna conexión directa entre el agente de Operations Manager y los registros de Azure Monitor. Los datos se reenvían desde el grupo de administración al área de trabajo de Log Analytics. |

### <a name="collection-frequency"></a>Frecuencia de recopilación

Para cada máquina Windows administrada, se realiza un examen dos veces al día. Cada 15 minutos, se llama a la API de Windows para consultar la hora de la última actualización y determinar si ha cambiado el estado. Si ha cambiado el estado, se inicia un examen de cumplimiento.

Para cada máquina Linux administrada se realiza un examen cada hora.

Puede que transcurran entre 30 minutos y 6 horas antes de que se muestren los datos actualizados de las máquinas administradas.

El uso medio de datos de los registros de Azure Monitor para una máquina que utiliza Update Management es aproximadamente de 25 megabytes (MB) al mes. Este valor es solo una aproximación y está sujeto a cambios en función de su entorno. Se recomienda supervisar el entorno para realizar un seguimiento del uso exacto. Para más información sobre cómo analizar el uso de datos, consulte [Administración del uso y los costos](../azure-monitor/platform/manage-cost-storage.md).

## <a name="network-planning"></a><a name="ports"></a>Planeamiento de red

Las direcciones siguientes se requieren específicamente para Update Management. La comunicación con estas direcciones se realiza a través del puerto 443.

|Azure Public  |Azure Government  |
|---------|---------|
|*.ods.opinsights.azure.com    | *.ods.opinsights.azure.us         |
|*.oms.opinsights.azure.com     | *.oms.opinsights.azure.us        |
|*.blob.core.windows.net | *.blob.core.usgovcloudapi.net|
|*.azure-automation.net | *.azure-automation.us|

En el caso de máquinas Windows, también debe permitir el tráfico a los puntos de conexión que necesita Windows Update. Puede encontrar una lista actualizada de los puntos de conexión necesarios en [Problemas relacionados con HTTP/Proxy](/windows/deployment/update/windows-update-troubleshooting#issues-related-to-httpproxy). Si tiene un [servidor de Windows Update](/windows-server/administration/windows-server-update-services/plan/plan-your-wsus-deployment) local, también debe permitir el tráfico al servidor especificado en la [clave de WSUS](/windows/deployment/update/waas-wu-settings#configuring-automatic-updates-by-editing-the-registry).

En el caso de máquinas de Red Hat Linux, consulte las [direcciones IP de los servidores de entrega de contenido de RHUI](../virtual-machines/workloads/redhat/redhat-rhui.md#the-ips-for-the-rhui-content-delivery-servers) de los puntos de conexión necesarios. Para otras distribuciones de Linux, consulte la documentación del proveedor.

Para más información sobre los puertos que requiere Hybrid Runbook Worker, consulte los [puertos de los roles de Hybrid Worker](automation-hybrid-runbook-worker.md#hybrid-worker-role).

Se recomienda utilizar las direcciones mostradas al definir las excepciones. Para obtener las direcciones IP, puede descargar los [intervalos de IP del centro de datos de Microsoft Azure](https://www.microsoft.com/download/details.aspx?id=41653). Este archivo se actualiza semanalmente y refleja los intervalos implementados en ese momento, así como los próximos cambios en los intervalos de direcciones IP.

Siga las instrucciones de [Conexión de equipos sin acceso a Internet](../azure-monitor/platform/gateway.md) para configurar máquinas que no tienen acceso a Internet.

## <a name="update-classifications"></a>Clasificaciones de actualizaciones

En las siguientes tablas se muestran las clasificaciones de actualizaciones en Update Management, junto con una definición de cada clasificación.

### <a name="windows"></a>Windows

|clasificación  |Descripción  |
|---------|---------|
|Actualizaciones críticas     | Actualización para un problema específico que resuelve un error crítico no relacionado con la seguridad.        |
|Actualizaciones de seguridad     | Actualización para un problema específico del producto relacionado con la seguridad.        |
|Paquetes acumulativos de actualizaciones     | Conjunto acumulativo de revisiones que se empaquetan para facilitar la implementación.        |
|Feature Packs     | Nuevas características del producto que se distribuyen fuera de una versión del producto.        |
|Service Packs     | Un conjunto acumulativo de revisiones que se aplican a una aplicación.        |
|Actualizaciones de definiciones     | Una actualización de archivos de definiciones de virus o de otra índole.        |
|Herramientas     | Utilidad o característica que ayuda a realizar una o más tareas.        |
|Actualizaciones     | Actualización de una aplicación o archivo que están instalados actualmente.        |

### <a name="linux"></a><a name="linux-2"></a>Linux

|clasificación  |Descripción  |
|---------|---------|
|Actualizaciones críticas y de seguridad     | Actualizaciones para un problema específico o un problema de un producto específico relacionado con la seguridad.         |
|Otras actualizaciones     | Todas las demás actualizaciones que ni son críticas por naturaleza ni son actualizaciones de seguridad.        |

En Linux, Update Management puede distinguir entre actualizaciones críticas y de seguridad en la nube y mostrar al mismo tiempo datos de evaluación debido al enriquecimiento de datos en la nube. Para aplicar revisiones, Update Management se basa en los datos de clasificación disponibles en la máquina. A diferencia de otras distribuciones, CentOS no dispone de esta información en la versión RTM. Si tiene máquinas de CentOS configuradas para devolver datos de seguridad para el siguiente comando, Update Management puede aplicar revisiones basadas en clasificaciones.

```bash
sudo yum -q --security check-update
```

Actualmente no hay ningún método compatible para habilitar la disponibilidad de datos de clasificación nativos en CentOS. En este momento, solo se proporciona soporte técnico a clientes que puedan haberla habilitado por su cuenta. 

Para clasificar las actualizaciones en la versión 6 de Red Hat Enterprise, debe instalar el complemento de seguridad de yum. En Red Hat Enterprise Linux 7, el complemento ya forma parte de yum, por lo que no es necesario instalar nada. Para más información, consulte el siguiente [artículo de conocimientos](https://access.redhat.com/solutions/10021) de Red Hat.

## <a name="integrate-with-configuration-manager"></a>Integración con Configuration Manager

Los clientes que han invertido en Microsoft Endpoint Configuration Manager para administrar equipos, servidores y dispositivos móviles también dependen de la fuerza y la madurez de Configuration Manager para que les ayude a administrar las actualizaciones de software. Configuration Manager es parte de su ciclo de administración de actualizaciones de software (SUM).

Para aprender a integrar la solución de administración con Configuration Manager, consulte este artículo sobre la [integración de Configuration Manager con Update Management](oms-solution-updatemgmt-sccmintegration.md).

### <a name="third-party-updates-on-windows"></a>Actualizaciones de terceros en Windows

Update Management se basa en el repositorio de actualización configurado localmente para actualizar los sistemas Windows compatibles. Este es WSUS o Windows Update. Herramientas como [System Center Updates Publisher](https://docs.microsoft.com/configmgr/sum/tools/updates-publisher) (Updates Publisher) le permiten importar y publicar actualizaciones personalizadas con WSUS. Este escenario permite que Update Management actualice las máquinas que usan Configuration Manager como repositorio de actualizaciones con software de terceros. Para obtener información sobre cómo configurar Updates Publisher, consulte [Instalar Updates Publisher](https://docs.microsoft.com/configmgr/sum/tools/install-updates-publisher).

## <a name="enable-update-management"></a><a name="onboard"></a>Habilitar Update Management

Para empezar a actualizar los sistemas, debe habilitar la solución Update Management. Las siguientes son los métodos recomendados y admitidos para incorporar la solución:

- [Desde una máquina virtual](automation-onboard-solutions-from-vm.md)

- [Navegando desde diferentes máquinas](automation-onboard-solutions-from-browse.md)

- [Desde la cuenta de Automation](automation-onboard-solutions-from-automation-account.md)

- [Con un runbook de Azure Automation](automation-onboard-solutions.md)

- [Con una plantilla de Azure Resource Manager](automation-update-management-deploy-template.md)

## <a name="next-steps"></a>Pasos siguientes

Consulte las [Preguntas más frecuentes](automation-faq.md) de Azure Automation para revisar preguntas comunes sobre esta solución.
