---
title: Solución de problemas durante la incorporación de soluciones de administración de Azure Automation
description: Aprenda a solucionar los errores de incorporación de las soluciones Update Management, Change Tracking e Inventory
services: automation
author: mgoedtel
ms.author: magoedte
ms.date: 05/22/2019
ms.topic: conceptual
ms.service: automation
manager: carmonm
ms.openlocfilehash: c949556949e0c187d7c23c4dd32436e245bfbb95
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/27/2020
ms.locfileid: "75889331"
---
# <a name="troubleshoot-errors-when-onboarding-update-management-change-tracking-and-inventory"></a>Solución de errores al incorporar Update Management, Change Tracking e Inventory

Al incorporar soluciones como Update Management, Change Tracking o Inventory, pueden producirse errores. En este artículo se describen los diversos errores que pueden producirse y cómo resolverlos.

## <a name="known-issues"></a>Problemas conocidos

### <a name="scenario-renaming-a-registered-node-requires-unregister--register-again"></a><a name="node-rename"></a>Escenario: Para cambiar el nombre de un nodo registrado, es necesario anular el registro o registrarse de nuevo

#### <a name="issue"></a>Problema

Un nodo se registra en Azure Automation y, a continuación, se cambia el nombre de equipo del sistema operativo.  Los informes del nodo continúan apareciendo con el nombre original.

#### <a name="cause"></a>Causa

Al cambiar el nombre de los nodos registrados, no actualiza el nombre del nodo en Azure Automation.

#### <a name="resolution"></a>Solución

Anule el registro del nodo en State Configuration de Azure Automation y, a continuación, vuelva a registrarlo.  Los informes publicados en el servicio antes de ese momento ya no estarán disponibles.


### <a name="scenario-re-signing-certificates-via-https-proxy-is-not-supported"></a><a name="resigning-cert"></a>Escenario: No se permite volver a firmar certificados a través del proxy HTTPS

#### <a name="issue"></a>Problema

Los clientes han comunicado que, al conectarse con una solución de proxy que finaliza el tráfico HTTPS y, a continuación, vuelve a cifrar el tráfico mediante un nuevo certificado, el servicio no permite la conexión.

#### <a name="cause"></a>Causa

Azure Automation no permite volver a firmar los certificados usados para cifrar el tráfico.

#### <a name="resolution"></a>Solución

No hay ninguna solución alternativa para este problema.

## <a name="general-errors"></a>Errores generales

### <a name="scenario-onboarding-fails-with-the-message---the-solution-cannot-be-enabled"></a><a name="missing-write-permissions"></a>Escenario: error de incorporación con el mensaje: no se puede habilitar la solución

#### <a name="issue"></a>Problema

Al intentar incorporar una máquina virtual a una solución, recibe uno de los siguientes mensajes:

```error
The solution cannot be enabled due to missing permissions for the virtual machine or deployments
```

```error
The solution cannot be enabled on this VM because the permission to read the workspace is missing
```

#### <a name="cause"></a>Causa

Este error se produce porque faltan permisos en la máquina virtual, el área de trabajo o para el usuario, o dichos permisos son incorrectos.

#### <a name="resolution"></a>Solución

Asegúrese de tener los permisos correctos para incorporar la máquina virtual. Revise los [permisos necesarios para incorporar máquinas](../automation-role-based-access-control.md#onboarding) y vuelva a intentar incorporar la solución. Si recibe el error `The solution cannot be enabled on this VM because the permission to read the workspace is missing`, asegúrese de tener el permiso `Microsoft.OperationalInsights/workspaces/read` para averiguar si la máquina virtual está incorporada a un área de trabajo.

### <a name="scenario-onboarding-fails-with-the-message---failed-to-configure-automation-account-for-diagnostic-logging"></a><a name="diagnostic-logging"></a>Escenario: Se produce un error en la incorporación con el mensaje: "No se pudo configurar la cuenta de Automation para el registro de diagnóstico"

#### <a name="issue"></a>Problema

Al intentar incorporar una máquina virtual a una solución, recibe el mensaje siguiente:

```error
Failed to configure automation account for diagnostic logging
```

#### <a name="cause"></a>Causa

Este error puede producirse si el plan de tarifa no coincide con el modelo de facturación de la suscripción. Para más información, consulte [Supervisión del uso y costos estimados en Azure Monitor](https://aka.ms/PricingTierWarning).

#### <a name="resolution"></a>Solución

Cree manualmente el área de trabajo de Log Analytics y repita el proceso de incorporación para seleccionar el área de trabajo creada.

### <a name="scenario-computergroupqueryformaterror"></a><a name="computer-group-query-format-error"></a>Escenario: ComputerGroupQueryFormatError

#### <a name="issue"></a>Problema

Este código de error significa que la consulta de búsqueda guardada del grupo de equipos que se utilizó tomando a la solución como objetivo no tenía un formato correcto. 

#### <a name="cause"></a>Causa

Puede que haya cambiado la consulta, o puede que lo haya hecho el sistema.

#### <a name="resolution"></a>Solución

Puede eliminar la consulta para esta solución y reincorporar la solución, con lo que se vuelve a crear la consulta. La consulta puede encontrarse en el área de trabajo, en **Búsquedas guardadas**. El nombre de la consulta es **MicrosoftDefaultComputerGroup**, y la categoría de la consulta es el nombre de la solución asociada a esta consulta. Si se habilitan varias soluciones, **MicrosoftDefaultComputerGroup** se muestra varias veces en **Búsquedas guardadas**.

### <a name="scenario-policyviolation"></a><a name="policy-violation"></a>Escenario: PolicyViolation

#### <a name="issue"></a>Problema

Este código de error significa que no se pudo realizar la implementación debido a la infracción de una o varias directivas.

#### <a name="cause"></a>Causa 

Existe una directiva que impide que se complete la operación.

#### <a name="resolution"></a>Solución

Para implementar correctamente la solución, debe considerar modificar la directiva indicada. Dado que hay muchos tipos diferentes de directivas que se pueden definir, los cambios específicos necesarios dependen de la directiva que se ha infringido. Por ejemplo, si se define una directiva en un grupo de recursos que deniega el permiso para cambiar el contenido de ciertos tipos de recursos dentro de ese grupo de recursos podría realizar cualquiera de las siguientes acciones:

* Eliminar por completo la directiva.
* Intentar realizar una incorporación a otro grupo de recursos.
* Revisar la directiva, por ejemplo:
  * Cambiar el destino de la directiva por un recurso concreto (por ejemplo, por una cuenta de Automation específica).
  * Revisar el conjunto de recursos para los que se configuró en la directiva denegar el acceso.

Compruebe las notificaciones en la esquina superior derecha de Azure Portal o desplácese hasta el grupo de recursos que contiene la cuenta de Automation y seleccione **Implementaciones** en **Configuración** para ver la implementación con errores. Para obtener más información sobre Azure Policy, visite: [Introducción a Azure Policy](../../governance/policy/overview.md?toc=%2fazure%2fautomation%2ftoc.json).

### <a name="scenario-errors-trying-to-unlink-a-workspace"></a><a name="unlink"></a>Escenario: Error al intentar desvincular un área de trabajo

#### <a name="issue"></a>Problema

Recibe el siguiente error al intentar desvincular un área de trabajo:

```error
The link cannot be updated or deleted because it is linked to Update Management and/or ChangeTracking Solutions.
```

#### <a name="cause"></a>Causa

Este error se produce cuando todavía dispone de soluciones activas en el área de trabajo de Log Analytics que dependen de que su cuenta de Automation y el área de trabajo de Log Analytics estén vinculadas.

### <a name="resolution"></a>Solución

Para resolver este problema, deberá quitar las siguientes soluciones del área de trabajo si las está usando:

* Administración de actualizaciones
* Seguimiento de cambios
* Inicio y detención de máquinas virtuales durante las horas de trabajo

Una vez que quite las soluciones, puede desvincular el área de trabajo. Es importante limpiar los artefactos existentes en esas soluciones del área de trabajo y también de la cuenta de Automation.  

* Administración de actualizaciones
  * Eliminación de implementaciones de actualizaciones (programaciones) de la cuenta de Automation
* Inicio y detención de máquinas virtuales durante las horas de trabajo
  * Quite los bloqueos sobre los componentes de soluciones de la cuenta de Automation en **Configuración** > **Bloqueos**.
  * Para conocer otros pasos para quitar la solución Start/Stop VMs during off-hours, consulte, [Eliminación de la solución Start/Stop VM during off-hours](../automation-solution-vm-management.md#remove-the-solution).

## <a name="mma-extension-failures"></a><a name="mma-extension-failures"></a>Errores de extensión MMA

[!INCLUDE [log-analytics-agent-note](../../../includes/log-analytics-agent-note.md)] 

Al implementar una solución, se implementan varios recursos relacionados. Uno de esos recursos es la extensión Microsoft Monitoring Agent o el Agente de Log Analytics para Linux. Estas son extensiones de máquina virtual que instala el agente invitado de la máquina virtual que es responsable de comunicarse con el área de trabajo de Log Analytics, con la finalidad de coordinar posteriormente la descarga de archivos binarios y de otro tipo de los que depende la solución que se va a incorporar una vez que comienza la ejecución.
Lo normal es que primero sepa de los errores de instalación de MMA o del Agente de Log Analytics para Linux por una notificación que aparece en el centro de notificaciones. Al hacer clic en esa notificación se proporciona más información sobre el error en concreto. Al navegar al recurso Grupos de recursos y luego al elemento Implementaciones que contiene, también se proporcionan detalles sobre los errores de implementación que se han producido.
La instalación de MMA o del Agente de Log Analytics para Linux puede generar errores por varias razones, y los pasos para solucionar estos errores varían en función del problema. Siga los pasos específicos de solución de problemas.

En la siguiente sección se describen diversos problemas con los que se puede encontrar al incorporar soluciones que pueden ocasionar errores en la implementación de la extensión MMA.

### <a name="scenario-an-exception-occurred-during-a-webclient-request"></a><a name="webclient-exception"></a>Escenario: Excepción durante una solicitud WebClient

La extensión MMA en la máquina virtual no puede comunicarse con los recursos externos y la implementación produce un error.

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

* Hay un servidor proxy configurado en la máquina virtual que solo admite puertos específicos.

* Una configuración de firewall ha bloqueado el acceso a los puertos y las direcciones necesarios.

#### <a name="resolution"></a>Solución

Asegúrese de que los puertos y las direcciones adecuados están abiertos para la comunicación. Para obtener una lista de direcciones y puertos, consulte [Planeamiento de la red](../automation-hybrid-runbook-worker.md#network-planning).

### <a name="scenario-install-failed-because-of-a-transient-environment-issues"></a><a name="transient-environment-issue"></a>Escenario: No se pudo realizar la instalación debido a problemas transitorios en el entorno

La instalación de la extensión Microsoft Monitoring Agent no se pudo realizar durante la implementación debido a que hay otra instalación o acción que la bloquea.

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

Este es un error transitorio por naturaleza. Vuelva a intentar la implementación para instalar la extensión.

### <a name="scenario-installation-timeout"></a><a name="installation-timeout"></a>Escenario: Tiempo de expiración de la instalación

No se completó la instalación de la extensión de MMA debido a que se agotó el tiempo de espera.

#### <a name="issue"></a>Problema

El siguiente es un ejemplo de un mensaje de error que se puede devolver:

```error
Install failed for plugin (name: Microsoft.EnterpriseCloud.Monitoring.MicrosoftMonitoringAgent, version 1.0.11081.4) with exception Command C:\Packages\Plugins\Microsoft.EnterpriseCloud.Monitoring.MicrosoftMonitoringAgent\1.0.11081.4\MMAExtensionInstall.exe of Microsoft.EnterpriseCloud.Monitoring.MicrosoftMonitoringAgent has exited with Exit code: 15614
```

#### <a name="cause"></a>Causa

Este error se debe a que la máquina virtual está bajo una carga pesada durante la instalación.

### <a name="resolution"></a>Solución

Intente instalar la extensión MMA cuando la máquina virtual esté bajo una carga inferior.

## <a name="next-steps"></a>Pasos siguientes

Si su problema no aparece o es incapaz de resolverlo, visite uno de nuestros canales para obtener ayuda adicional:

* Obtenga respuestas de expertos de Azure en los [foros de Azure](https://azure.microsoft.com/support/forums/).
* Póngase en contacto con [@AzureSupport](https://twitter.com/azuresupport): la cuenta de Microsoft Azure oficial para mejorar la experiencia del cliente, que pone en contacto a la comunidad de Azure con los recursos adecuados: respuestas, soporte técnico y expertos.
* Si necesita más ayuda, puede registrar un incidente de soporte técnico de Azure. Vaya al [sitio de soporte técnico de Azure](https://azure.microsoft.com/support/options/) y seleccione **Obtener soporte**.
