---
title: archivo de inclusión
description: archivo de inclusión
author: cynthn
ms.service: virtual-machines
ms.topic: include
ms.date: 09/30/2019
ms.author: cynthn
ms.openlocfilehash: feea9696316723a2750be6fc1e13001224320324
ms.sourcegitcommit: af1cbaaa4f0faa53f91fbde4d6009ffb7662f7eb
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 04/22/2020
ms.locfileid: "81865617"
---
Con el crecimiento significativo de las máquinas virtuales hospedadas en Azure, es importante identificar los problemas de rendimiento y de estado que afectan a las aplicaciones y los servicios de infraestructura que admiten. Azure incluye supervisión básica de forma predeterminada, con las métricas de uso de CPU, uso de disco, uso de memoria y tráfico de red que el hipervisor del host recopila. Se pueden recopilar datos de métricas y de registro adicionales con [extensiones](../articles/virtual-machines/windows/extensions-features.md) para configurar el diagnóstico en las máquinas virtuales desde el sistema operativo invitado.

Para detectar y ayudar a diagnosticar problemas de rendimiento y mantenimiento del sistema operativo invitado, los componentes de aplicación web de Java o basados en .NET que se ejecutan dentro de la máquina virtual, Azure Monitor ofrece supervisión centralizada con características completas como Azure Monitor para VM y Application Insights.

## <a name="diagnostics-and-metrics"></a>Diagnósticos y métricas 

Puede configurar y supervisar la recopilación de [datos de diagnóstico](https://docs.microsoft.com/cli/azure/vm/diagnostics) con [métricas](../articles/monitoring-and-diagnostics/monitoring-overview-metrics.md) en Azure Portal, la CLI de Azure, Azure PowerShell y la programación de Interfaces de programación de aplicaciones (API). Por ejemplo, puede:

- **Observar métricas básicas para la máquina virtual.** En la pantalla Información general de Azure Portal, entre las métricas básicas que se muestran se incluye el uso de CPU, el uso de la red, el total de bytes de disco y las operaciones de disco por segundo.

- **Habilitar y observar la recopilación de diagnósticos de arranque y a través de Azure Portal.** Cuando lleva su propia imagen a Azure o incluso cuando arranca una de las imágenes de la plataforma, puede haber muchas razones por las que una máquina virtual pueda entrar en un estado que no permita el arranque. Puede habilitar fácilmente el diagnóstico de arranque cuando se crea una máquina virtual, haciendo clic en **Habilitado** en Diagnóstico de arranque, en la sección Supervisión de la pantalla Configuración.

    Cuando las máquinas virtuales arrancan, el agente de los diagnósticos de arranque captura la salida del arranque y la almacena en Azure Storage. Estos datos se pueden utilizar para solucionar los problemas de arranque de la máquina virtual. Los diagnósticos de arranque no se habilitan automáticamente al crear una máquina virtual mediante las herramientas de línea de comandos. Antes de habilitar los diagnósticos de arranque, es preciso crear una cuenta de almacenamiento para almacenar los registros de arranque. Si habilita el diagnóstico de arranque en Azure Portal, se crea automáticamente una cuenta de almacenamiento.

    Si no habilitó el diagnóstico de arranque cuando se creó la máquina virtual, siempre puede habilitarlo más adelante con la [CLI de Azure](https://docs.microsoft.com/cli/azure/vm/boot-diagnostics), [Azure PowerShell](https://docs.microsoft.com/powershell/module/az.compute/set-azvmbootdiagnostic) o una [plantilla de Azure Resource Manager](../articles/virtual-machines/windows/extensions-diagnostics-template.md).

- **Habilitar la recopilación de datos de diagnóstico del sistema operativo invitado.** Cuando se crea una máquina virtual, en la pantalla de configuración tiene la oportunidad de habilitar los diagnósticos del sistema operativo invitado. Cuando habilita la recopilación de datos de diagnóstico, la [extensión IaaSDiagnostics para Linux](../articles/virtual-machines/linux/diagnostic-extension.md) o la [extensión IaaSDiagnostics para Windows](../articles/virtual-machines/windows/ps-extensions-diagnostics.md) se agregan a la máquina virtual, lo que le permite recopilar más datos de disco, CPU y memoria.

    Con los datos de diagnóstico recopilados, puede configurar el escalado automático para las máquinas virtuales. También puede configurar [registros de Azure Monitor](../articles/azure-monitor/platform/data-platform-logs.md) para almacenar los datos y configurar alertas para informarle cuando el rendimiento no sea correcto.

## <a name="alerts"></a>Alertas

Puede crear [alertas](../articles/azure-monitor/platform/alerts-overview.md) basadas en métricas de rendimiento concretas. Algunos problemas de los que se puede alertar son, por ejemplo, cuando el uso promedio de la CPU supera un umbral concreto o cuando el espacio libre en disco disponible cae por debajo de una cantidad determinada. Es posible configurar alertas en [Azure Portal](../articles/azure-monitor/platform/alerts-metric.md#create-with-azure-portal), con [plantillas de Azure Resource Manager](../articles/azure-monitor/platform/alerts-metric-create-templates.md) o la [CLI de Azure](../articles/azure-monitor/platform/alerts-metric.md#with-azure-cli).

## <a name="azure-service-health"></a>Azure Service Health

[Azure Service Health](../articles/service-health/service-health-overview.md) proporciona soporte técnico e información orientativa personalizada cuando se producen problemas en los servicios de Azure que le afectan, y le ayuda a prepararse para los próximos mantenimientos planeados. Azure Service Health le informa de las alertas a usted y a los equipos por medio de notificaciones específicas y flexibles.

## <a name="azure-resource-health"></a>Azure Resource Health

[Azure Resource health](../articles/service-health/resource-health-overview.md) ayuda a diagnosticar si un problema de Azure afecta a los recursos y, en su caso, a obtener soporte técnico. Le informa acerca del mantenimiento actual y pasado de los recursos y le ayuda a mitigar los problemas. Resource Health le proporciona soporte técnico si necesita ayuda con los problemas de los servicios de Azure.

## <a name="azure-activity-log"></a>Azure Activity Log

[Azure Activity Log](../articles/azure-monitor/platform/platform-logs-overview.md) es un registro de suscripción que proporciona información sobre los eventos de nivel de suscripción que se han producido en Azure. Este registro incluye una serie de datos, desde datos operativos de Azure Resource Manager hasta actualizaciones en eventos de Service Health. Puede hacer clic en Activity Log, en Azure Portal, para ver el registro de la máquina virtual.

Estas son algunas de las tareas que puede hacer con el registro de actividad:

- Crear una [alerta basada en un evento de Activity Log](../articles/azure-monitor/platform/platform-logs-overview.md).
- [Transmitirlo a un centro de eventos](../articles/azure-monitor/platform/activity-logs-stream-event-hubs.md) para la ingesta en un servicio de terceros o una solución de análisis personalizado como Power BI.
- Analizarlo en Power BI con el [paquete de contenido de Power BI](https://powerbi.microsoft.com/documentation/powerbi-content-pack-azure-audit-logs/).
- [Guardarlo en una cuenta de almacenamiento](../articles/azure-monitor/platform/archive-activity-log.md)para archivarlo o inspeccionarlo manualmente. Puede especificar el tiempo de retención (en días) mediante el perfil de registro.

También puede tener acceso a datos de registro de actividad mediante [Azure PowerShell](https://docs.microsoft.com/powershell/module/azurerm.insights/), la [CLI de Azure](https://docs.microsoft.com/cli/azure/monitor) o las [API de REST de Monitor](https://docs.microsoft.com/rest/api/monitor/).

[Los registros de recursos de Azure](../articles/azure-monitor/platform/platform-logs-overview.md) son registros emitidos por una máquina virtual que proporcionan datos exhaustivos y frecuentes acerca de su funcionamiento. Los registros de recursos se diferencian del registro de actividad en que proporcionan una visión general acerca de las operaciones realizadas dentro de la máquina virtual.

Estas son algunas de las cosas que puede hacer con los registros de diagnóstico:

- [Guardarlos en una cuenta de almacenamiento](../articles/azure-monitor/platform/archive-diagnostic-logs.md) para auditarlos o para inspeccionarlos manualmente. Puede especificar el tiempo de retención (en días) mediante Configuración de diagnóstico de recursos.
- [Transmitirlos a Event Hubs](../articles/azure-monitor/platform/resource-logs-stream-event-hubs.md) para la ingesta en un servicio de terceros o una solución de análisis personalizado como Power BI.
- Analizarlos con [Log Analytics](../articles/log-analytics/log-analytics-azure-storage.md).

## <a name="advanced-monitoring"></a>Supervisión avanzada

Habilite tanto [Azure Monitor para VM](../articles/azure-monitor/insights/vminsights-overview.md) como [Application Insights](../articles/azure-monitor/app/app-insights-overview.md) para dar visibilidad a la aplicación o el servicio que la máquina virtual de Azure y los conjuntos de escalado de máquinas virtuales admiten e identificar los problemas del sistema operativo invitado o de la carga de trabajo que se ejecuta en la máquina virtual para comprender si está afectando a la disponibilidad o el rendimiento de la aplicación o si es un problema de la aplicación.

La solución Azure Monitor para VM supervisa las máquinas virtuales de Azure a escala al analizar el rendimiento y el estado de las máquinas virtuales Windows y Linux, incluidos los diferentes procesos y las dependencias interconectadas con otros recursos y con los procesos externos que detecta. Incluye varios gráficos de rendimiento de las tendencias para ayudarle en la investigación de problemas y evaluar la capacidad de las máquinas virtuales. El mapa de dependencias muestra las máquinas supervisadas y no supervisadas, las conexiones de red con errores y las conexiones activas entre los procesos y estas máquinas. También muestra gráficos de tendencias con métricas de conexión de red estándar. En combinación con Application Insights, puede supervisar la aplicación y capturar datos de telemetría, tales como solicitudes HTTP o excepciones, para que pueda correlacionar los problemas entre las máquinas virtuales y la aplicación. Configure las [alertas de Azure Monitor](../articles/azure-monitor/platform/alerts-overview.md) para recibir alertas sobre las condiciones importantes detectadas durante la supervisión de los datos recopilados por Azure Monitor para VM.

## <a name="next-steps"></a>Pasos siguientes

- Siga los pasos de [Supervisión de una máquina virtual Windows con Azure PowerShell](../articles/virtual-machines/windows/tutorial-monitoring.md) o de [Supervisión de una máquina Virtual Linux con la CLI de Azure](../articles/virtual-machines/linux/tutorial-monitoring.md).

- Más información sobre las prácticas recomendadas de la [supervisión y el diagnóstico](https://docs.microsoft.com/azure/architecture/best-practices/monitoring).
