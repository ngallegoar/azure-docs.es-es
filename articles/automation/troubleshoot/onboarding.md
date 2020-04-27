---
title: Solución de problemas durante la incorporación de soluciones de administración de Azure Automation
description: Obtenga información acerca de cómo solucionar problemas de incorporación de soluciones.
services: automation
author: mgoedtel
ms.author: magoedte
ms.date: 05/22/2019
ms.topic: conceptual
ms.service: automation
manager: carmonm
ms.openlocfilehash: da5152b459f54cbaae5ec168f103f23a237edebd
ms.sourcegitcommit: acb82fc770128234f2e9222939826e3ade3a2a28
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 04/21/2020
ms.locfileid: "81679229"
---
# <a name="troubleshoot-solution-onboarding"></a>Solución de problemas de incorporación de soluciones

Podría recibir errores al incorporar la solución Update Management o la solución Change Tracking e Inventario. En este artículo se describen los diversos errores que pueden producirse y cómo resolverlos.

## <a name="known-issues"></a>Problemas conocidos

### <a name="scenario-renaming-a-registered-node-requires-unregister-or-register-again"></a><a name="node-rename"></a>Escenario: el cambio de nombre de un nodo registrado existe la anulación o la repetición del registro

#### <a name="issue"></a>Problema

Un nodo se registra en Azure Automation y, a continuación, se cambia el nombre de equipo del sistema operativo. Los informes del nodo continúan apareciendo con el nombre original.

#### <a name="cause"></a>Causa

Al cambiar el nombre de los nodos registrados, no actualiza el nombre del nodo en Azure Automation.

#### <a name="resolution"></a>Solución

Anule el registro del nodo en State Configuration de Azure Automation y, a continuación, vuelva a registrarlo. Los informes publicados en el servicio antes de ese momento ya no estarán disponibles.

### <a name="scenario-re-signing-certificates-via-https-proxy-is-not-supported"></a><a name="resigning-cert"></a>Escenario: No se permite volver a firmar certificados a través del proxy HTTPS

#### <a name="issue"></a>Problema

Al conectarse con una solución de proxy que finaliza el tráfico HTTPS y, a continuación, volver a cifrar el tráfico mediante un nuevo certificado, el servicio no permite la conexión.

#### <a name="cause"></a>Causa

Azure Automation no permite volver a firmar los certificados usados para cifrar el tráfico.

#### <a name="resolution"></a>Solución

Actualmente no hay ninguna solución alternativa para este problema.

## <a name="general-errors"></a>Errores generales

### <a name="scenario-onboarding-fails-with-the-message---the-solution-cannot-be-enabled"></a><a name="missing-write-permissions"></a>Escenario: error de incorporación con el mensaje: no se puede habilitar la solución

#### <a name="issue"></a>Problema

Al intentar incorporar una VM a una solución, recibe uno de los siguientes mensajes:

```error
The solution cannot be enabled due to missing permissions for the virtual machine or deployments
```

```error
The solution cannot be enabled on this VM because the permission to read the workspace is missing
```

#### <a name="cause"></a>Causa

Este error se produce porque faltan permisos para la VM, el área de trabajo o el usuario, o porque los permisos son incorrectos.

#### <a name="resolution"></a>Solución

Asegúrese de tener los [permisos necesarios para incorporar máquinas](../automation-role-based-access-control.md#onboarding-permissions) y, a continuación, vuelva a intentar incorporar la solución. Si recibe el error `The solution cannot be enabled on this VM because the permission to read the workspace is missing`, asegúrese de tener el permiso `Microsoft.OperationalInsights/workspaces/read` para averiguar si la VM está incorporada a un área de trabajo.

### <a name="scenario-onboarding-fails-with-the-message-failed-to-configure-automation-account-for-diagnostic-logging"></a><a name="diagnostic-logging"></a>Escenario: se produce un error en la incorporación con el mensaje siguiente. No se pudo configurar la cuenta de Automation para el registro de diagnóstico.

#### <a name="issue"></a>Problema

Al intentar incorporar una VM a una solución, recibe el mensaje siguiente:

```error
Failed to configure automation account for diagnostic logging
```

#### <a name="cause"></a>Causa

Este error puede producirse si el plan de tarifa no coincide con el modelo de facturación de la suscripción. Consulte [Supervisión del uso y costos estimados en Azure Monitor](https://aka.ms/PricingTierWarning).

#### <a name="resolution"></a>Solución

Cree manualmente el área de trabajo de Log Analytics y repita el proceso de incorporación para seleccionar el área de trabajo creada.

### <a name="scenario-computergroupqueryformaterror"></a><a name="computer-group-query-format-error"></a>Escenario: ComputerGroupQueryFormatError

#### <a name="issue"></a>Problema

Este código de error significa que la consulta de búsqueda guardada del grupo de equipos que se utilizó para definir el destino de la solución no tenía un formato correcto. 

#### <a name="cause"></a>Causa

Es posible que haya modificado la consulta o que lo haya hecho el sistema.

#### <a name="resolution"></a>Solución

Puede eliminar la consulta para la solución y, a continuación, volver a incorporar la solución, con lo que se vuelve a crear la consulta. La consulta puede encontrarse en el área de trabajo, en **Búsquedas guardadas**. El nombre de la consulta es **MicrosoftDefaultComputerGroup** y la categoría de la consulta es el nombre de la solución asociada. Si se habilitan varias soluciones, la consulta **MicrosoftDefaultComputerGroup** se muestra varias veces en **Búsquedas guardadas**.

### <a name="scenario-policyviolation"></a><a name="policy-violation"></a>Escenario: PolicyViolation

#### <a name="issue"></a>Problema

Este código de error indica que no se pudo realizar la implementación debido a la infracción de una o varias directivas.

#### <a name="cause"></a>Causa 

Una directiva impide que se complete la operación.

#### <a name="resolution"></a>Solución

Para implementar correctamente la solución, debe considerar la posibilidad de modificar la directiva indicada. Dado que hay muchos tipos diferentes de directivas que se pueden definir, los cambios necesarios dependen de la directiva que se ha infringido. Por ejemplo, si se ha definido una directiva en un grupo de recursos que deniega el permiso para cambiar el contenido de algunos recursos incluidos, puede elegir una de estas correcciones:

* Eliminar por completo la directiva.
* Intentar incorporar la incorporación a otro grupo de recursos.
* Cambiar el destino de la directiva a un recurso específico, por ejemplo, una cuenta de Automation.
* Revisar el conjunto de recursos que la directiva debe denegar según la configuración.

Compruebe las notificaciones en la esquina superior derecha de Azure Portal o desplácese hasta el grupo de recursos que contiene la cuenta de Automation y seleccione **Implementaciones** en **Configuración** para ver la implementación con errores. Para más información acerca de Azure Policy, consulte [Introducción a Azure Policy](../../governance/policy/overview.md?toc=%2fazure%2fautomation%2ftoc.json).

### <a name="scenario-errors-trying-to-unlink-a-workspace"></a><a name="unlink"></a>Escenario: Error al intentar desvincular un área de trabajo

#### <a name="issue"></a>Problema

Recibe el siguiente error al intentar desvincular un área de trabajo:

```error
The link cannot be updated or deleted because it is linked to Update Management and/or ChangeTracking Solutions.
```

#### <a name="cause"></a>Causa

Este error se produce cuando todavía dispone de soluciones activas en el área de trabajo de Log Analytics que dependen de que su cuenta de Automation y el área de trabajo de Log Analytics estén vinculadas.

### <a name="resolution"></a>Solución

Quite las siguientes soluciones del área de trabajo si las está usando:

* Administración de actualizaciones
* Change Tracking e Inventario
* Inicio y detención de máquinas virtuales durante las horas de trabajo

Una vez que quite las soluciones, puede desvincular el área de trabajo. Es importante limpiar los artefactos existentes en esas soluciones del área de trabajo y de la cuenta de Automation. 

* Para Update Management, quite las implementaciones de actualización (programaciones) de la cuenta de Automation.
* Para la solución Start/Stop VMs during off hours, quite los bloqueos de los componentes de la solución de la cuenta de Automation en **Configuración** > **Bloqueos**. Consulte [Quitar la solución Start/Stop VMs during off hours](../automation-solution-vm-management.md#remove-the-solution).

## <a name="log-analytics-for-windows-extension-failures"></a><a name="mma-extension-failures"></a>Errores de Log Analytics para la extensión de Windows

[!INCLUDE [log-analytics-agent-note](../../../includes/log-analytics-agent-note.md)] 

Se puede producir un error en la instalación de la extensión del agente de Log Analytics para Windows por diversos motivos. En la siguiente sección se describen los problemas de incorporación que pueden causar errores durante la implementación de la extensión del agente de Log Analytics para Windows.

>[!NOTE]
>El agente de Log Analytics para Windows es el nombre que se usa actualmente en Azure Automation para Microsoft Monitoring Agent (MMA).

### <a name="scenario-an-exception-occurred-during-a-webclient-request"></a><a name="webclient-exception"></a>Escenario: Excepción durante una solicitud WebClient

La extensión de Log Analytics para Windows en la VM no puede comunicarse con los recursos externos y la implementación produce un error.

#### <a name="issue"></a>Problema

Los siguientes son ejemplos de mensajes de error que se devuelven:

```error
Please verify the VM has a running VM agent, and can establish outbound connections to Azure storage.
```

```error
'Manifest download error from https://<endpoint>/<endpointId>/Microsoft.EnterpriseCloud.Monitoring_MicrosoftMonitoringAgent_australiaeast_manifest.xml. Error: UnknownError. An exception occurred during a WebClient request.
```

#### <a name="cause"></a>Causa

Algunas de las posibles causas de este error son:

* Un servidor proxy configurado en la VM solo permite puertos específicos.
* Una configuración de firewall ha bloqueado el acceso a los puertos y las direcciones necesarios.

#### <a name="resolution"></a>Solución

Asegúrese de que los puertos y las direcciones adecuados están abiertos para la comunicación. Para obtener una lista de direcciones y puertos, consulte [Planeamiento de la red](../automation-hybrid-runbook-worker.md#network-planning).

### <a name="scenario-install-failed-because-of-a-transient-environment-issues"></a><a name="transient-environment-issue"></a>Escenario: No se pudo realizar la instalación debido a problemas transitorios en el entorno

La instalación de la extensión de Log Analytics para Windows no se pudo realizar durante la implementación porque otra instalación o acción la bloquea.

#### <a name="issue"></a>Problema

Los siguientes son ejemplos de mensajes de error que se pueden devolver:

```error
The Microsoft Monitoring Agent failed to install on this machine. Please try to uninstall and reinstall the extension. If the issue persists, please contact support.
```

```error
'Install failed for plugin (name: Microsoft.EnterpriseCloud.Monitoring.MicrosoftMonitoringAgent, version 1.0.11081.4) with exception Command C:\Packages\Plugins\Microsoft.EnterpriseCloud.Monitoring.MicrosoftMonitoringAgent\1.0.11081.4\MMAExtensionInstall.exe of Microsoft.EnterpriseCloud.Monitoring.MicrosoftMonitoringAgent has exited with Exit code: 1618'
```

```error
'Install failed for plugin (name: Microsoft.EnterpriseCloud.Monitoring.MicrosoftMonitoringAgent, version 1.0.11081.2) with exception Command C:\Packages\Plugins\Microsoft.EnterpriseCloud.Monitoring.MicrosoftMonitoringAgent\1.0.11081.2\MMAExtensionInstall.exe of Microsoft.EnterpriseCloud.Monitoring.MicrosoftMonitoringAgent has exited with Exit code: 1601'
```

#### <a name="cause"></a>Causa

Algunas de las posibles causas de este error son:

* Hay otra instalación en curso.
* El sistema se desencadenó para reiniciarse durante la implementación de plantillas.

#### <a name="resolution"></a>Solución

Este error es transitorio por naturaleza. Vuelva a intentar la implementación para instalar la extensión.

### <a name="scenario-installation-timeout"></a><a name="installation-timeout"></a>Escenario: Tiempo de expiración de la instalación

No se completó la instalación de la extensión de Log Analytics para Windows porque se agotó el tiempo de espera.

#### <a name="issue"></a>Problema

El siguiente es un ejemplo de mensaje de error que se puede devolver:

```error
Install failed for plugin (name: Microsoft.EnterpriseCloud.Monitoring.MicrosoftMonitoringAgent, version 1.0.11081.4) with exception Command C:\Packages\Plugins\Microsoft.EnterpriseCloud.Monitoring.MicrosoftMonitoringAgent\1.0.11081.4\MMAExtensionInstall.exe of Microsoft.EnterpriseCloud.Monitoring.MicrosoftMonitoringAgent has exited with Exit code: 15614
```

#### <a name="cause"></a>Causa

Este tipos de error se produce porque la VM se somete a una carga pesada durante la instalación.

### <a name="resolution"></a>Solución

Intente instalar la extensión del agente de Log Analytics para Windows cuando la VM esté sometida a una carga menor.

## <a name="next-steps"></a>Pasos siguientes

Si su problema no aparece arriba o no puede resolverlo, intente obtener ayuda adicional a través de uno de los siguientes canales:

* Obtenga respuestas de expertos de Azure en los [foros de Azure](https://azure.microsoft.com/support/forums/).
* Póngase en contacto con [@AzureSupport](https://twitter.com/azuresupport), la cuenta oficial de Microsoft Azure para mejorar la experiencia del cliente, que pone en contacto a la comunidad de Azure con los recursos adecuados: respuestas, soporte técnico y expertos.
* Registrar un incidente de soporte técnico de Azure. Vaya al [sitio de soporte técnico de Azure](https://azure.microsoft.com/support/options/) y seleccione **Obtener soporte**.
