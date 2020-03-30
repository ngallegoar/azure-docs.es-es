---
title: Preguntas más frecuentes sobre Azure Automation | Microsoft Docs
description: Respuestas a las preguntas más frecuentes sobre Azure Automation.
services: automation
ms.subservice: ''
ms.topic: conceptual
author: mgoedtel
ms.author: magoedte
ms.date: 02/25/2020
ms.openlocfilehash: 129a5316c2e7be329b479c79706791e993d20b74
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/28/2020
ms.locfileid: "77921759"
---
# <a name="azure-automation-frequently-asked-questions"></a>Preguntas más frecuentes sobre Azure Automation

En este artículo de preguntas más frecuentes de Microsoft, se presenta una lista con las preguntas frecuentes sobre Azure Automation. Si tiene alguna otra pregunta sobre sus funcionalidades, vaya al foro de discusión y publíquela. Si una pregunta es frecuente, se agrega a este artículo para que se pueda encontrar de forma rápida y sencilla.

## <a name="update-management-solution"></a>Solución Update Management

### <a name="can-i-prevent-unexpected-os-level-upgrades"></a>¿Puedo evitar actualizaciones inesperadas de nivel de sistema operativo?

En algunas variantes de Linux, como Red Hat Enterprise Linux, las actualizaciones de nivel de sistema operativo pueden producirse mediante paquetes. Esto puede llevar a ejecuciones de Update Management en las que cambia el número de versión del sistema operativo. Como Update Management usa los mismos métodos para actualizar paquetes que un administrador utilizaría localmente en la máquina Linux, este comportamiento es deliberado.

Para evitar la actualización de la versión del sistema operativo con las implementaciones de Update Management, use la característica **Exclusión**.

En Red Hat Enterprise Linux, el nombre del paquete para excluir sería: redhat-release-server.x86_64.

### <a name="why-arent-criticalsecurity-updates-applied"></a>¿Por qué no se aplican las actualizaciones de seguridad o críticas?

Al implementar actualizaciones en una máquina Linux, puede seleccionar clasificaciones de actualizaciones. Esta opción filtra las actualizaciones que se aplican a la máquina que cumple los criterios especificados. Este filtro se aplica localmente en el equipo cuando se implementa la actualización.

Dado que Update Management realiza el enriquecimiento de actualizaciones en la nube, algunas actualizaciones pueden marcarse en Update Management como de impacto para la seguridad, aunque la máquina local no tenga esa información. Como resultado, si aplica actualizaciones críticas a una máquina Linux, puede haber actualizaciones que no estén marcadas como de impacto para la seguridad en esa máquina y que, por lo tanto, no se apliquen. Sin embargo, es posible que Update Management todavía notifique que esa máquina no es compatible porque tiene información adicional acerca de la actualización pertinente.

La implementación de actualizaciones mediante la clasificación de actualizaciones no funciona en las versiones RTM de CentOS. Para implementar correctamente actualizaciones para CentOS, seleccione todas las clasificaciones para asegurarse de que se aplican las actualizaciones. Para SUSE, al seleccionar *solo* **Otras actualizaciones** como clasificación, puede que se instalen también algunas actualizaciones de seguridad si primero se requieren actualizaciones de seguridad relacionadas con zypper (administrador de paquetes) o sus dependencias. Este comportamiento es una limitación de zypper. En algunos casos, puede que se le pida volver a ejecutar la implementación de actualizaciones. Para ello, compruebe el registro de actualizaciones.

### <a name="can-i-deploy-updates-across-azure-tenants"></a>¿Puedo implementar actualizaciones en los inquilinos de Azure?

Si tiene máquinas en otro inquilino de Azure que informen a Update Management de que necesita una revisión, tendrá que usar la solución alternativa siguiente para programarlas. Puede usar el cmdlet [New-AzureRmAutomationSchedule](/powershell/module/azurerm.automation/new-azurermautomationschedule) con el modificador `-ForUpdate` para crear una programación, así como usar el cmdlet [New-AzureRmAutomationSoftwareUpdateConfiguration](/powershell/module/azurerm.automation/new-azurermautomationsoftwareupdateconfiguration
) y pasar las máquinas del otro inquilino al parámetro `-NonAzureComputer`. El ejemplo siguiente muestra cómo hacerlo:

```azurepowershell-interactive
$nonAzurecomputers = @("server-01", "server-02")

$startTime = ([DateTime]::Now).AddMinutes(10)

$sched = New-AzureRmAutomationSchedule -ResourceGroupName mygroup -AutomationAccountName myaccount -Name myupdateconfig -Description test-OneTime -OneTime -StartTime $startTime -ForUpdate

New-AzureRmAutomationSoftwareUpdateConfiguration  -ResourceGroupName $rg -AutomationAccountName <automationAccountName> -Schedule $sched -Windows -NonAzureComputer $nonAzurecomputers -Duration (New-TimeSpan -Hours 2) -IncludedUpdateClassification Security,UpdateRollup -ExcludedKbNumber KB01,KB02 -IncludedKbNumber KB100
```

## <a name="next-steps"></a>Pasos siguientes

Si su pregunta no se ha respondido aquí, puede consultar los foros siguientes para preguntas y respuestas adicionales.

- [Azure Automation](https://social.msdn.microsoft.com/Forums/home?forum=azureautomation&filter=alltypes&sort=lastpostdesc)

Para comentarios generales sobre la solución Update Management, visite el [foro de comentarios](https://feedback.azure.com/forums/905242-update-management).