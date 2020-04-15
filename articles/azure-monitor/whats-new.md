---
title: Novedades en la documentación de Azure Monitor
description: Se realizan actualizaciones significativas de la documentación de Azure Monitor todos los meses.
ms.subservice: ''
ms.topic: overview
author: bwren
ms.author: bwren
ms.date: 03/05/2020
ms.openlocfilehash: a2e6d2a459a6713aa9372496fc3a933c0a886ed9
ms.sourcegitcommit: 6397c1774a1358c79138976071989287f4a81a83
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 04/07/2020
ms.locfileid: "80802611"
---
# <a name="whats-new-in-azure-monitor-documentation"></a>Novedades en la documentación de Azure Monitor
En este artículo se enumeran los artículos de Azure Monitor que son nuevos o que se han actualizado de forma significativa. Se actualizará la primera semana de cada mes para que incluya las actualizaciones de artículos del mes anterior.

## <a name="march-2020"></a>Marzo de 2020

### <a name="general"></a>General
- [Introducción a Azure Monitor](overview.md): se ha agregado un vídeo de introducción a Azure Monitor.
- [Configuración de la clave administrada por el cliente de Azure Monitor](platform/customer-managed-keys.md): actualizaciones generales.
- [Referencia de datos de Azure Monitor](/azure/azure-monitor/reference): nuevo sitio.

### <a name="alerts"></a>Alertas

- [Crear, ver y administrar las alertas del registro de actividad mediante Azure Monitor](platform/alerts-activity-log.md): explicación adicional de la plantilla de Resource Manager.
- [Descripción del funcionamiento de las alertas de métricas en Azure Monitor.](platform/alerts-metric-overview.md) : se ha actualizado para la compatibilidad con la Administración Pública.
- [Solución de problemas de alertas y notificaciones de Azure Monitor](platform/alerts-troubleshoot.md): nuevo artículo.

### <a name="application-insights"></a>Application Insights
- [Automatización de Azure Application Insights con PowerShell](app/powershell.md): se han agregado ejemplos de ARMClient.
- [Exportación continua de telemetría desde Application Insights](app/export-telemetry.md):se ha agregado una tabla con detalles de la estructura de exportación.
- [Habilitación de Snapshot Debugger para aplicaciones de .NET en Azure App Service](app/snapshot-debugger-appservice.md): se ha agregado una plantilla de Resource Manager de ejemplo.
- [Administración del uso y los costos de Application Insights](app/pricing.md): se ha agregado información sobre la alerta de límite de datos.
- [Supervisión de aplicaciones de Python con Azure Monitor (versión preliminar)](app/opencensus-python.md): se han agregado métricas estándar.
- [Compatibilidad del mapa de origen con las aplicaciones de JavaScript: Application Insights para Azure Monitor](https://docs.microsoft.com/azure/azure-monitor/app/source-map-support): nuevo artículo.


### <a name="containers"></a>Contenedores
- [Preguntas más frecuentes sobre Azure Monitor](faq.md): actualización de Azure Monitor para contenedores.
- [Configuración de la supervisión de GPU con Azure Monitor para contenedores](insights/container-insights-gpu-monitoring.md): nuevo artículo.

### <a name="insights"></a>Información detallada
- [Solución de administración de Office 365 en Azure](insights/solution-office-365.md): fecha de desuso actualizada.

### <a name="logs"></a>Registros
- [Optimización de las consultas de registro en Azure Monitor](log-query/query-optimization.md): se ha agregado la condición de CPU para el análisis de XML y JSON.
- [Eliminación y recuperación de un área de trabajo de Azure Log Analytics](platform/delete-workspace.md): se ha agregado información de solución de problemas.
- [Uso de registros de Azure Monitor con Azure Logic Apps y Power Automate](platform/logicapp-flow-connector.md): se ha actualizado con un nuevo conector de Azure Monitor.

### <a name="metrics"></a>Métricas
- [Desuso de métricas de disco en Azure Portal](https://docs.microsoft.com/azure/azure-monitor/platform/portal-disk-metrics-deprecation): nuevo artículo.
- [Tutorial: Creación de un gráfico de métricas en Azure Monitor](learn/tutorial-metrics-explorer.md): se ha agregado un vídeo.

### <a name="platform-logs"></a>Registros de plataforma
- [Recopilación y análisis del registro de actividad de Azure en Azure Monitor](platform/activity-log-collect.md): se ha redactado de nuevo para explicar mejor la recopilación del registro de actividad con configuración de diagnóstico.

### <a name="virtual-machines"></a>Máquinas virtuales
- [Supervisión de máquinas virtuales de Azure con Azure Monitor](insights/monitor-vm-azure.md): nuevo artículo.
- [Inicio rápido: Supervisión de máquinas virtuales de Azure con Azure Monitor](learn/quick-monitor-azure-vm.md): se ha actualizado para agregar Azure Monitor para máquinas virtuales.
- [Creación de alertas a partir de Azure Monitor para VM](insights/vminsights-alerts.md): nuevo artículo.
- [Información general sobre la habilitación de Azure Monitor para VM](insights/vminsights-enable-overview.md): actualización de los vínculos de descarga de agentes.


Actualizaciones generales para disponibilidad general de Azure Monitor para VM
- [¿Qué es Azure Monitor para VM?](insights/vminsights-overview.md)
- [Preguntas frecuentes sobre Azure Monitor para VM (GA)](insights/vminsights-ga-release-faq.md) 
- [Habilitación de Azure Monitor para VM mediante Azure Policy](insights/vminsights-enable-at-scale-policy.md) 
- [Cómo representar el rendimiento en gráficos con Azure Monitor para VM](insights/vminsights-performance.md)
- [Cómo consultar registros de Azure Monitor para VM](insights/vminsights-log-search.md)
- [Visualización de las dependencias de aplicación con Azure Monitor para VM](insights/vminsights-maps.md) 


### <a name="visualizations"></a>Visualizaciones
- [Visualización de datos de Azure Monitor](visualizations.md): se ha actualizado para indicar el desuso planeado del Diseñador de vistas.





## <a name="february-2020"></a>Febrero de 2020

### <a name="agents"></a>Agentes
Actualizaciones múltiples como parte de la reescritura del contenido de la extensión de diagnóstico.

- [Introducción a los agentes de supervisión de Azure](platform/agents-overview.md): reestructuración de las tablas para aclarar mejor las características únicas de cada agente.
- [Introducción a las extensiones de Azure Diagnostics](platform/diagnostics-extension-overview.md): se ha reescrito en su totalidad.
- [Uso de Blob Storage para IIS y Table Storage para eventos en Azure Monitor](platform/diagnostics-extension-logs.md): reescritura general con fines de actualización y claridad.
- [Instalación y configuración de Windows Azure Diagnostics Extension (WAD)](platform/diagnostics-extension-windows-install.md): artículo nuevo. 
- [Esquema de Windows Diagnostics Extension](platform/diagnostics-extension-schema-windows.md): se ha reorganizado.
- [Envío de datos desde la extensión de Diagnósticos de Windows Azure a Azure Event Hubs](platform/diagnostics-extension-stream-event-hubs.md): se ha reescrito y actualizado por completo.
- [Almacenamiento y visualización de datos de diagnóstico en Azure Storage](/azure/cloud-services/diagnostics-extension-to-storage): se ha reescrito y actualizado por completo.
- [Extensión de máquina virtual de Log Analytics para Windows](../virtual-machines/extensions/oms-windows.md): aclara la relación con el agente de Log Analytics.
- [Extensión de máquina virtual de Azure Monitor para Linux](../virtual-machines/extensions/oms-linux.md): aclara la relación con el agente de Log Analytics.




### <a name="application-insights"></a>Application Insights
- [Cadenas de conexión en Azure Application Insights](app/sdk-connection-string.md): artículo nuevo.

### <a name="insights-and-solutions"></a>Conclusiones y soluciones

#### <a name="azure-monitor-for-containers"></a>Azure Monitor para contenedores
- [Integración de Azure Active Directory con Azure Kubernetes Service](../aks/azure-ad-integration.md): se ha agregado una nota para crear una aplicación cliente que admita clústeres habilitados para RBAC con el fin de admitir Azure Monitor para contenedores.

#### <a name="azure-monitor-for-vms"></a>Azure Monitor para máquinas virtuales
- [Preguntas frecuentes sobre Azure Monitor para VM (disponibilidad general)](insights/vminsights-ga-release-faq.md): cambia el modo en que se almacenan los datos de rendimiento.

#### <a name="office-365"></a>Office 365
- [Solución de administración de Office 365 en Azure](insights/solution-office-365.md): fecha de desuso actualizada.


### <a name="logs"></a>Registros
- [Optimización de las consultas de registro en Azure Monitor](log-query/query-optimization.md): artículo nuevo.
- [Administración del uso y los costos de los registros de Azure Monitor](platform/manage-cost-storage.md): mejora de las consultas de ejemplo para ayudarle a comprender el uso.

### <a name="metrics"></a>Métricas
- [Métricas de plataforma de Azure Monitor exportables a través de la configuración del diagnóstico](platform/metrics-supported-export-diagnostic-settings.md): se ha agregado una sección sobre el cambio en el comportamiento de los valores NULL y cero.


### <a name="visualizations"></a>Visualizaciones
Varios artículos nuevos para la guía de conversión del diseñador de vistas a los libros.

- [Guía de transición del diseñador de vistas a los libros de Azure Monitor](platform/view-designer-conversion-overview.md): artículo nuevo.
- [Opciones de conversión del diseñador de vistas a los libros de Azure Monitor](platform/view-designer-conversion-options.md): artículo nuevo.
- [Conversión de elementos del diseñador de vistas a los libros de Azure Monitor](platform/view-designer-conversion-tiles.md): artículo nuevo.
- [Resumen y acceso a la conversión del diseñador de vistas a libros de Azure Monitor](platform/view-designer-conversion-access.md): artículo nuevo.
- [Tareas comunes de la conversión del diseñador de vistas a libros de Azure Monitor](platform/view-designer-conversion-tasks.md): artículo nuevo.
- [Ejemplos de conversión del diseñador de vistas a libros de Azure Monitor](platform/view-designer-conversion-examples.md): artículo nuevo.




## <a name="january-2020"></a>Enero de 2020

### <a name="general"></a>General
- [¿Qué supervisa Azure Monitor?](monitor-reference.md) - Nuevo artículo.

### <a name="agents"></a>Agentes
- [Recopilación de datos de registro con el agente de Azure Log Analytics](platform/log-analytics-agent.md): se ha actualizado la tabla de requisitos del firewall de red.


### <a name="alerts"></a>Alertas
- [Creación y administración de grupos de acciones en Azure Portal](platform/action-groups.md): se ha eliminado la configuración de las funciones v2 que ya no es necesaria.
- [Creación de una alerta de métrica con una plantilla de Resource Manager](platform/alerts-metric-create-templates.md): se ha agregado un ejemplo para el parámetro *ignoreDataBefore*.  Se han agregado restricciones sobre las reglas de varios criterios.
- [Uso de la API REST de alertas de Log Analytics](platform/api-alerts.md): se ha corregido el ejemplo de JSON.


### <a name="application-insights"></a>Application Insights
- [Direcciones IP que emplean Application Insights y Log Analytics](app/ip-addresses.md): se ha actualizado la sección de prueba de disponibilidad con la incorporación de una regla de puerto de entrada que permite el tráfico mediante grupos de seguridad de red de Azure.
- [Solución de problemas relacionados con Azure Application Insights Profiler](app/profiler-troubleshooting.md): se ha actualizado la solución de problemas general.
- [Muestreo de telemetría en Azure Application Insights](app/sampling.md): se ha actualizado y reestructurado para mejorar la legibilidad en función de los comentarios de los clientes.


### <a name="data-security"></a>Seguridad de los datos
- [Configuración de la clave administrada por el cliente de Azure Monitor](platform/customer-managed-keys.md): nuevo artículo.

### <a name="insights-and-solutions"></a>Conclusiones y soluciones

#### <a name="azure-monitor-for-containers"></a>Azure Monitor para contenedores
- [Configuración de la recopilación de datos del agente de Azure Monitor para contenedores](insights/container-insights-agent-config.md): se han agregado detalles para la actualización del agente en Red Hat OpenShift en Azure e información adicional para distinguir los métodos de actualización del agente.
- [Creación de alertas de rendimiento para Azure Monitor para contenedores](insights/container-insights-alerts.md): se ha revisado la información y se han actualizado los pasos para crear una alerta en los datos de rendimiento almacenados en el área de trabajo mediante alertas de contexto del área de trabajo.
- [Supervisión de Kubernetes con Azure Monitor para contenedores](insights/container-insights-analyze.md): se ha actualizado el artículo introductorio y el de análisis relacionado con la compatibilidad de los clústeres de Windows Kubernetes.
- [Configuración de clústeres de Red Hat OpenShift en Azure con Azure Monitor para contenedores](insights/container-insights-azure-redhat-setup.md): se han agregado detalles para la actualización del agente en Red Hat OpenShift en Azure e información adicional para distinguir los métodos de actualización del agente.
- [Configuración de los clústeres híbridos de Kubernetes con Azure Monitor para contenedores](insights/container-insights-hybrid-setup.md): se ha actualizado para reflejar la compatibilidad agregada para el puerto seguro 10250 con Kubelet's cAdvisor.
- [Administración de Azure Monitor para el agente de contenedores](insights/container-insights-manage-agent.md): se han actualizado los detalles relacionados con el comportamiento y la configuración de la recopilación de métricas de Red Hat OpenShift en Azure en comparación con otros tipos de clústeres de Kubernetes.
- [Configurar la integración de Prometheus con Azure Monitor para contenedores](insights/container-insights-prometheus-integration.md): se han actualizado los detalles relacionados con el comportamiento y la configuración de la recopilación de métricas de Red Hat OpenShift en Azure en comparación con otros tipos de clústeres de Kubernetes.
- [Actualización de Azure Monitor para contenedores para habilitar métricas](insights/container-insights-update-metrics.md): se han actualizado los detalles relacionados con el comportamiento y la configuración de la recopilación de métricas de Red Hat OpenShift en Azure en comparación con otros tipos de clústeres de Kubernetes.


#### <a name="azure-monitor-for-vms"></a>Azure Monitor para máquinas virtuales
- [Preguntas frecuentes sobre Azure Monitor para VM (disponible de forma general)](insights/vminsights-ga-release-faq.md): se ha agregado información sobre la actualización del área de trabajo y los agentes a la nueva versión.

#### <a name="office-365"></a>Office 365
- [Solución de administración de Office 365 en Azure](insights/solution-office-365.md): se han agregado detalles y preguntas frecuentes sobre la migración a una solución de Office 365 en Azure Sentinel. Se ha eliminado la sección de incorporación.



### <a name="logs"></a>Registros
- [Administración de áreas de trabajo de Log Analytics en Azure Monitor](platform/manage-access.md): se ha actualizado la sección de "No acciones".
- [Administración del uso y los costos de los registros de Azure Monitor](platform/manage-cost-storage.md): se ha agregado claridad al cálculo del volumen de datos en la sección Modelo de precios.
- [Uso de las plantillas de Azure Resource Manager para crear y configurar un área de trabajo de Log Analytics](platform/template-workspace-configuration.md): se ha actualizado la plantilla con nuevos planes de tarifa.


### <a name="platform-logs"></a>Registros de plataforma
- [Recopilación del registro de actividad de Azure con configuración de diagnóstico: Azure Monitor](platform/diagnostic-settings-legacy.md): se ha agregado información adicional sobre las propiedades modificadas.
- [Exportación del registro de actividad de Azure](platform/activity-log-export.md): se ha actualizado con los cambios de la interfaz de usuario. 





## <a name="december-2019"></a>Diciembre de 2019

### <a name="agents"></a>Agentes
- [Conexión de equipos Linux a Azure Monitor](platform/agent-linux.md): nuevo artículo.

### <a name="alerts"></a>Alertas
- [Creación de una alerta de métrica con una plantilla de Resource Manager](platform/alerts-metric-create-templates.md): se ha agregado un ejemplo para métricas personalizadas.
- [Creación de alertas con umbrales dinámicos en Azure Monitor](platform/alerts-dynamic-thresholds.md): se ha agregado una sección sobre la interpretación de gráficos de umbrales dinámicos.
- [Información general de alertas y supervisión de notificaciones en Azure](platform/alerts-overview.md): se ha actualizado la consulta de Resource Graph.
- [Recursos compatibles para las alertas de métricas de Azure Monitor](platform/alerts-metric-near-real-time.md): actualización sobre las métricas y dimensiones admitidas.
- [Cambio de la API heredada de alertas de Log Analytics a la nueva API de Alertas de Azure](platform/alerts-log-api-switch.md): se ha agregado una nota sobre el nombre de alerta modificado.
- [Descripción del funcionamiento de las alertas de métricas en Azure Monitor.](platform/alerts-metric-overview.md) se han agregado los tipos de recursos admitidos para la supervisión a escala.

### <a name="application-insights"></a>Application Insights
- [Application Insights para las aplicaciones de servicio de trabajo (aplicaciones sin HTTP)](app/worker-service.md): se ha agregado un nivel de registro predeterminado al código de C#. Se ha actualizado la versión de referencia del paquete.
- [Referencia de ApplicationInsights.config: Azure](app/configuration-with-applicationinsights-config.md): se ha actualizado el código de ejemplo.
- [Automatización de Azure Application Insights con PowerShell](app/powershell.md): se ha actualizado la plantilla de Resource Manager.
- [Paquetes NuGet de Application Insights de Azure Monitor](app/nuget.md): se han agregado otras versiones de paquetes.
- [Creación de un recurso de Azure Application Insights](app/create-new-resource.md): se ha agregado una nota al nombre único global.
- [Diagnóstico con Live Metrics Stream: Azure Application Insights](app/live-stream.md): se ha actualizado el requisito de versión del SDK de ASP.NET Core.
- [Contadores de eventos en Application Insights](app/eventcounters.md): se ha actualizado la categoría y la tabla de customMetrics.
- [Exploración de registros de seguimiento de Java en Azure Application Insights](app/java-trace-logs.md): se ha agregado la configuración del umbral de registro del agente de Java.
- [Direcciones IP que emplean Application Insights y Log Analytics](app/ip-addresses.md): se han actualizado las direcciones IP para Live Metrics Stream.
- [Supervisar el rendimiento de Azure App Services](app/azure-web-apps.md): se ha agregado compatibilidad con ASP.NET Core 3.0. 
- [Supervisión de aplicaciones de Python con Azure Monitor (versión preliminar)](app/opencensus-python.md): se ha agregado una aclaración para la asignación de esquemas de Python para OpenCensus en Azure. Esquema de Monitor
- [Notas de la versión de Azure Application Insights](app/release-notes.md): se han agregado notas para las versiones más antiguas.
- [Canales de telemetría en Azure Application Insights](app/telemetry-channels.md): se ha actualizado la duración de los datos descartados durante el período ampliado de la conexión perdida.
- [Muestreo de telemetría en Azure Application Insights](app/sampling.md): se ha corregido un fragmento de código del elemento TelemetryInitializer.
- [Solución de problemas de Application Insights en un proyecto web de Java](app/java-troubleshoot.md): se ha eliminado una instrucción acerca de la no admisión de la recopilación de dependencias en JDK 9.

### <a name="insights-and-solutions"></a>Conclusiones y soluciones
- [Preguntas más frecuentes sobre Azure Monitor para contenedores](insights/container-insights-faq.md): se ha agregado una pregunta sobre los campos Image y Name.
- [Solución Azure SQL Analytics en Azure Monitor](insights/azure-sql.md): se han actualizado los tiempos de espera de la base de datos para la compatibilidad con Instancia administrada.
- [Configuración de la recopilación de datos del agente de Azure Monitor para contenedores](insights/container-insights-agent-config.md): se ha agregado la configuración de enrich_container_logs.
- [Configuración de los clústeres híbridos de Kubernetes con Azure Monitor para contenedores](insights/container-insights-hybrid-setup.md): se ha agregado la sección de solución de problemas.
- [Supervisión de Active Directory Replication Status con Azure Monitor](insights/ad-replication-status.md): se ha actualizado el requisito previo de .NET Framework.
- [Solución Network Performance Monitor en Azure](insights/network-performance-monitor.md): se han agregado regiones compatibles.
- [Optimización del entorno de Active Directory con Azure Monitor](insights/ad-assessment.md): se ha actualizado el requisito previo de .NET Framework.
- [Optimización del entorno de SQL Server con Azure Monitor](insights/sql-assessment.md): se ha actualizado el requisito previo de .NET Framework.
- [Optimización del entorno de System Center Operations Manager con Azure Log Analytics](insights/scom-assessment.md): se ha actualizado el requisito previo de .NET Framework.
- [Conexiones compatibles con IT Service Management Connector en Azure Log Analytics](platform/itsmc-connections.md): se ha agregado "New York" al requisito previo de id. de cliente y secreto de cliente.

### <a name="logs"></a>Registros
- [Eliminación y recuperación de un área de trabajo de Azure Log Analytics](platform/delete-workspace.md): se ha agregado un método con PowerShell.
- [Diseño de la implementación de registros de Azure Monitor](platform/design-logs-deployment.md): se ha aumentado la velocidad de ingesta de un área de trabajo.

### <a name="metrics"></a>Métricas
- [Métricas de plataforma de Azure Monitor que pueden exportarse con la configuración de diagnóstico](platform/metrics-supported-export-diagnostic-settings.md): nuevo artículo.

### <a name="platform-logs"></a>Registros de plataforma
Se han actualizado varios artículos como parte de la reestructuración de contenido de los registros de la plataforma en función de la nueva característica para configurar el registro de actividad mediante la configuración de diagnóstico.

- [Archivado de registros de recursos de Azure en la cuenta de almacenamiento](platform/resource-logs-collect-storage.md)
- [Esquema de eventos del registro de actividad de Azure](platform/activity-log-schema.md)
- [Límites de servicio de Azure Monitor](service-limits.md)
- [Recopilación y análisis de los registros de actividad de Azure en un área de trabajo de Log Analytics](platform/activity-log-collect.md)
- [Recopilación del registro de actividad de Azure con configuración de diagnóstico (versión preliminar): Azure Monitor](platform/diagnostic-settings-legacy.md)
- [Recopilación de registros de actividad de Azure en un área de trabajo de Log Analytics entre inquilinos de Azure](platform/activity-log-collect-tenants.md)
- [Recopilación de registros de recursos de Azure en el área de trabajo de Log Analytics](platform/resource-logs-collect-workspace.md)
- [Creación de la configuración de diagnóstico en Azure con una plantilla de Resource Manager](platform/diagnostic-settings-template.md)
- [Creación de una configuración de diagnóstico para recopilar registros y métricas en Azure](platform/diagnostic-settings.md)
- [Exportación del registro de actividad de Azure](platform/activity-log-export.md)
- [Introducción a los registros de plataforma Azure](platform/platform-logs-overview.md)
- [Transmisión de datos de supervisión de Azure a un centro de eventos](platform/stream-monitoring-data-event-hubs.md)
- [Transmisión de registros de plataforma de Azure a un centro de eventos](platform/resource-logs-stream-event-hubs.md)

### <a name="quickstarts-and-tutorials"></a>Guías de inicio rápido y tutoriales

- [Creación de un gráfico de métricas en Azure Monitor](learn/tutorial-metrics-explorer.md): nuevo artículo.
- [Recopilación de registros de recurso de un recurso de Azure y análisis con Azure Monitor](learn/tutorial-resource-logs.md): nuevo artículo.
- [Supervisión de recursos de Azure con Azure Monitor](learn/quick-monitor-azure-resource.md): nuevo artículo.
   
## <a name="next-steps"></a>Pasos siguientes

- Para colaborar en la documentación de Azure Monitor, consulte la [Guía para colaboradores de Microsoft Docs](https://docs.microsoft.com/contribute/).