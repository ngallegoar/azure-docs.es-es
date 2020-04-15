---
title: Retirada de características de Security Center (julio de 2019) | Microsoft Docs
description: En este artículo se describen las características de Security Center que se retiraron el 31 de julio de 2019.
services: security-center
author: memildin
manager: rkarlin
ms.service: security-center
ms.topic: article
ms.date: 09/10/2019
ms.author: memildin
ms.openlocfilehash: 4a760fe1e5df7cf614a68f8fa8a05926326d3edf
ms.sourcegitcommit: 3c318f6c2a46e0d062a725d88cc8eb2d3fa2f96a
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 04/02/2020
ms.locfileid: "80583215"
---
# <a name="retirement-of-security-center-features-july-2019"></a>Retirada de características de Security Center (julio de 2019)

> [!NOTE]
> En este documento se detalla la lista de características que se retiraron de Azure Security Center el 31 de julio de 2019.
>
>

Se realizaron varias [mejoras](https://azure.microsoft.com/updates/?product=security-center) en Azure Security Center durante los seis meses previos.
Con estas funcionalidades mejoradas, se quitaron algunas características redundantes y las API relacionadas de Security Center el 31 de julio de 2019.

La mayoría de estas características que se retiran se pueden reemplazar por otras funcionalidades en Azure Security Center o Azure Log Analytics. Se pueden implementar otras características mediante [Azure Sentinel (versión preliminar)](https://azure.microsoft.com/services/azure-sentinel/).

Las características de Security Center retiradas son:

- [Panel de eventos](#menu_events)
- [Entrada de menú de búsqueda](#menu_search)
- [Vínculo Ver versión clásica de Identidad y acceso en Identidad y acceso (versión preliminar)](#menu_classicidentity)
- [Botón de mapa de eventos de seguridad de Mapa de alertas de seguridad (versión preliminar)](#menu_securityeventsmap)
- [Reglas de alerta personalizadas (versión preliminar)](#menu_customalerts)
- [Botón de investigación de las alertas de seguridad de protección contra amenazas](#menu_investigate)
- [Un subconjunto de soluciones de seguridad](#menu_solutions)
- [Edición de las opciones de configuración de seguridad para directivas de seguridad](#menu_securityconfigurations)
- [Panel Seguridad y auditoría (usado originalmente en el portal de OMS) para las áreas de trabajo de Log Analytics](#menu_securityomsdashboard)

En este artículo se proporciona información detallada para cada característica retirada y los pasos que puede seguir para implementar características de reemplazo.

## <a name="events-dashboard"></a>Panel de eventos<a name="menu_events"></a>

Security Center usa el agente de Log Analytics para recopilar distintos eventos y configuraciones relacionados con la seguridad de los equipos. Almacena estos eventos en las áreas de trabajo. El [panel de eventos](https://docs.microsoft.com/azure/security-center/security-center-events-dashboard) le permite ver los datos y le ofrece un punto de entrada a Log Analytics.

Se ha retirado el panel de eventos que aparecía al seleccionar un área de trabajo:

![Panel de eventos][2]

### <a name="events-dashboard---the-new-experience"></a>Panel de eventos: la nueva experiencia

Se recomienda usar las funcionalidades nativas de Azure Log Analytics para ver los eventos importantes en las áreas de trabajo.

Si ha creado los eventos importantes personalizados en Security Center, se podrá acceder a ellos. En Log Analytics, vaya a **Seleccionar área de trabajo** > **Búsquedas guardadas**. Los datos no se perderán o modificarán. Los eventos importantes nativos también están disponibles en la misma pantalla de Log Analytics.

![Búsquedas guardadas de área de trabajo][3]

## <a name="search-menu-entry"></a>Entrada de menú de búsqueda<a name="menu_search"></a>

Azure Security Center actualmente usa la búsqueda de registros de Azure Monitor para recuperar y analizar los datos de seguridad. Esta pantalla sirve como una ventana de la página de búsqueda de Log Analytics y permite a los usuarios ejecutar consultas de búsqueda en sus áreas de trabajo seleccionadas. Para más información, consulte [Búsqueda de registros en Azure Security Center](https://docs.microsoft.com/azure/security-center/security-center-search). Se ha retirado esta ventana de búsqueda:

![Página de búsqueda][4]

### <a name="search-menu-entry---the-new-experience"></a>Entrada de menú de búsqueda: la nueva experiencia

Le animamos a usar las funcionalidades nativas de Azure Log Analytics para realizar consultas de búsqueda en las áreas de trabajo. Vaya a Azure Log Analytics y seleccione **Registros**.

![Página de registros de Log Analytics][5]

## <a name="classic-identity--access-preview"></a>Versión clásica de Identidad y acceso (versión preliminar)<a name="menu_classicidentity"></a>

Actualmente, la experiencia Versión clásica de Identidad y acceso de Security Center muestra un panel de información de identidad y acceso en Log Analytics. Para ver este panel:

1. Seleccione **Ver versión clásica de Identidad y acceso**.

   ![Página de identidad][6]

1. Vea **Panel Identidad y acceso**.

    ![Página de identidad: selección del área de trabajo][7]

1. Seleccione un área de trabajo que desee abrir en el panel **Identidad y acceso** en Log Analytics para ver la información de identidad y acceso sobre el área de trabajo.

   ![Página de identidad: panel][8]

Se han retirado las tres pantallas que se muestran en los pasos anteriores. Los datos siguen estando disponibles en la solución de seguridad de Log Analytics y no se modificaron ni quitaron.

### <a name="classic-identity--access-preview---the-new-experience"></a>Versión clásica de Identidad y acceso (versión preliminar): la nueva experiencia

El panel de Log Analytics ha mostrado información sobre un área de trabajo. Sin embargo, las funcionalidades nativas de Security Center proporcionan visibilidad en todas las suscripciones y todas las áreas de trabajo asociadas con ellas. Puede acceder a una vista fácil de usar que le permite centrarse en lo importante con recomendaciones clasificadas conforme a su puntuación de seguridad.

Se puede acceder a todas las características del panel **Identidad y acceso** de Log Analytics seleccionando **Identidad y acceso (versión preliminar)** dentro de Security Center.

![Página de identidad: retirada de la experiencia clásica][9]

## <a name="security-events-map"></a>Mapa de eventos de seguridad<a name="menu_securityeventsmap"></a>

Security Center proporciona un mapa de las alertas de seguridad para ayudarle a identificar las amenazas de seguridad. El botón **Ir al mapa de eventos de seguridad** de ese mapa abre un panel que le permite ver los eventos de seguridad sin procesar en el área de trabajo seleccionada.

Se ha quitado el botón **Ir al mapa de eventos de seguridad** y el panel por área de trabajo.

![Mapa de alertas de seguridad: botón][10]

Cuando selecciona el botón **Ir al mapa de eventos de seguridad**, se abre el panel de inteligencia sobre amenazas (retirado actualmente).

![Panel Inteligencia sobre amenazas][11]

Cuando elige un área de trabajo para ver su panel de inteligencia sobre amenazas, se abre la pantalla de mapa de alertas de seguridad (retirado actualmente) en Log Analytics.

![Mapa de alertas de seguridad en Log Analytics][12]

Los datos existentes siguen estando disponibles en la solución de seguridad de Log Analytics y no se modificaron ni quitaron.

### <a name="security-events-map---the-new-experience"></a>Mapa de eventos de seguridad: la nueva experiencia

Le animamos a que use la funcionalidad de mapa de las alertas integrada en Security Center: **Mapa de alertas de seguridad (versión preliminar)** . Esta funcionalidad proporciona una experiencia optimizada y funciona en todas las suscripciones y áreas de trabajo asociadas. Ofrece una vista de alto nivel en el entorno y no se centra en una única área de trabajo.

## <a name="custom-alert-rules-preview"></a>Reglas de alerta personalizadas (versión preliminar)<a name="menu_customalerts"></a>

El 30 de junio de 2019 se [retiró la experiencia de alertas personalizadas](https://docs.microsoft.com/azure/security-center/security-center-custom-alert) dado que se retiró su infraestructura subyacente. Después de la fecha de retirada, ya no se generan alertas de seguridad personalizadas.
Se recomienda que habilite [Azure Sentinel](https://azure.microsoft.com/services/azure-sentinel/) y vuelva a crear las alertas personalizadas allí. Como alternativa, puede crear las alertas con alertas de registro de Azure Monitor.

Para crear alertas personalizadas con Azure Sentinel:

1. [Abra Azure Sentinel](https://portal.azure.com/#create/Microsoft.ASI/preview) y seleccione el área de trabajo donde se almacenan las alertas personalizadas.
1. Seleccione **Analytics**en el menú.
1. Siga las instrucciones del siguiente [tutorial](https://docs.microsoft.com/azure/sentinel/tutorial-detect-threats) sobre cómo crear alertas personalizadas en Azure Sentinel.

Si no le interesa usar Azure Sentinel, puede crear las alertas con alertas de registro de Azure Monitor. Para instrucciones, consulte  [Creación, visualización y administración de alertas de registro mediante Azure Monitor](https://docs.microsoft.com/azure/azure-monitor/platform/alerts-log) y [Alertas de registro en Azure Monitor](https://docs.microsoft.com/azure/azure-monitor/platform/alerts-unified-log).

![Alertas personalizadas][13]

Para más información sobre la retirada de alertas personalizadas, vea [Reglas de alerta personalizadas en Azure Security Center (versión preliminar)](https://docs.microsoft.com/azure/security-center/security-center-custom-alert).

## <a name="security-alerts-investigation"></a>Investigación de alertas de seguridad<a name="menu_investigate"></a>

La característica Investigación de Security Center le ayuda a evaluar un posible incidente de seguridad. La característica permite entender el ámbito de un incidente y localizar su causa raíz. Se ha quitado esta característica de Security Center porque se ha reemplazado por una experiencia mejorada en [Azure Sentinel](https://azure.microsoft.com/services/azure-sentinel/).

![Incidente de seguridad][14]

Cuando se selecciona el botón **Investigar** desde una pantalla **Incidente de seguridad**, se abre el Panel de investigación (versión preliminar) en Log Analytics. Se ha retirado el Panel de investigación.

Los datos existentes siguen estando disponibles en la solución de seguridad de Log Analytics y no se modificaron ni quitaron.

![Panel de investigación en Log Analytics][15]

### <a name="investigation---the-new-experience"></a>Investigación: la nueva experiencia

Le recomendamos que realice la transición a [Azure Sentinel](https://azure.microsoft.com/services/azure-sentinel/) para una experiencia de investigación enriquecida. Azure Sentinel proporciona eficaces herramientas de búsqueda y consulta para buscar amenazas de seguridad en los orígenes de datos de la organización.

## <a name="subset-of-security-solutions"></a>Subconjunto de soluciones de seguridad<a name="menu_solutions"></a>

Security Center puede habilitar [soluciones de seguridad integradas en Azure](https://docs.microsoft.com/azure/security-center/security-center-partner-integration). Se han retirado las siguientes soluciones de asociados de Security Center. Estas soluciones están habilitadas en [Azure Sentinel](https://azure.microsoft.com/services/azure-sentinel/) junto con una serie de orígenes de datos adicionales.

- [Firewall y soluciones de firewall de aplicaciones web de la siguiente generación](https://docs.microsoft.com/azure/sentinel/connect-data-sources)
- [Integración de soluciones de seguridad que admiten Common Event Format (CEF)](https://docs.microsoft.com/azure/sentinel/connect-common-event-format)
- [Microsoft Advanced Threat Analytics](https://docs.microsoft.com/azure/sentinel/connect-azure-atp)
- [Azure AD Identity Protection](https://docs.microsoft.com/azure/sentinel/connect-azure-ad-identity-protection)

Después de la retirada, no puede agregar o modificar ninguno de los tipos de soluciones mencionados en la lista anterior, ya sea desde la interfaz de usuario o desde la API. Azure Security Center ya no detectará ninguna nueva instancia de estas soluciones de asociados.

Si tiene soluciones conectadas existentes, le animamos a que realice la transición a Azure Sentinel.

![Soluciones de centros de seguridad][16]

## <a name="edit-security-configurations-for-security-policies"></a>Edición de las opciones de configuración de seguridad para directivas de seguridad<a name="menu_securityconfigurations"></a>

Azure Security Center supervisa las configuraciones de seguridad mediante un conjunto de [más de 150 reglas recomendadas](https://gallery.technet.microsoft.com/Azure-Security-Center-a789e335) para proteger el sistema operativo. Estas reglas pertenecen a firewalls, auditoría, directivas de contraseñas, etc. Si se detecta que la configuración de una maquina es vulnerable, Security Center genera una recomendación de seguridad. La [pantalla Edición de las opciones de configuración de seguridad](https://docs.microsoft.com/azure/security-center/security-center-customize-os-security-config) permite a los clientes personalizar la configuración de seguridad predeterminada del sistema operativo en Security Center.

Se ha retirado esta característica en versión preliminar. Si quiere restablecer las configuraciones de seguridad a sus valores predeterminados tras la fecha de retirada, puede hacerlo con la API o PowerShell si sigue las [siguientes instrucciones](https://github.com/Azure/Azure-Security-Center/tree/master/Powershell%20scripts/Reset%20security%20configurations%20customization).

![Edición de las opciones de configuración de seguridad][17]

### <a name="edit-security-configurations---the-new-experience"></a>Edición de las opciones de configuración de seguridad: la nueva experiencia

Tenemos previsto habilitar Security Center para admitir el [agente de configuración de invitados](https://docs.microsoft.com/azure/governance/policy/concepts/guest-configuration). Dicha actualización permitirá que un conjunto de características mucho más completo, incluida la compatibilidad con más sistemas operativos y la integración de las directivas en invitado de Azure para las configuraciones de invitado. Después de que estos cambios estén habilitados, también tendrá la posibilidad de controlar las configuraciones a escala y aplicarlas automáticamente a los nuevos recursos.

## <a name="security-and-audit-dashboard-for-log-analytics-workspaces"></a>Panel Seguridad y auditoría para áreas de trabajo de Log Analytics<a name="menu_securityomsdashboard"></a>

El panel Seguridad y auditoría se utilizaba originalmente en el portal de OMS. En Log Analytics, el panel proporciona una introducción por área de trabajo de amenazas y eventos de seguridad importantes, un mapa de inteligencia de amenazas y una evaluación de identidad y acceso de eventos de seguridad que se guardan en el área de trabajo. Se ha retirado el panel. Como ya recomendamos en la interfaz de usuario del panel, le aconsejamos que realice la transición a Azure Security Center.

![Panel de seguridad de log Analytics][18]

### <a name="security-and-audit-dashboard---the-new-experience"></a>Panel Seguridad y auditoría: la nueva experiencia

Le recomendamos que cambie a Azure Security Center. Proporciona la misma información general de seguridad en varias suscripciones y las áreas de trabajo asociadas a ellas, además de un completo conjunto de características.

Puede obtener las consultas originales de Log Analytics que rellenan el panel Seguridad y auditoría en el [repositorio de GitHub](https://github.com/Azure/Azure-Security-Center/tree/master/Legacy%20Log%20Analytics%20dashboards) de Security Center.

## <a name="next-steps"></a>Pasos siguientes

- Más información sobre [Azure Security Center](https://docs.microsoft.com/azure/security-center/)
- Más información sobre [Azure Sentinel](https://docs.microsoft.com/azure/sentinel).

<!--Image references - events-->
[1]: ./media/security-center-features-retirement-july2019/asc_events_dashboard.png
[2]: ./media/security-center-features-retirement-july2019/asc_events_dashboard_inner.png
[3]: ./media/security-center-features-retirement-july2019/workspace_saved_searches.png
<!--Image references - search-->
[4]: ./media/security-center-features-retirement-july2019/asc_search.png
[5]: ./media/security-center-features-retirement-july2019/workspace_logs.png
<!--Image references - classic identity and access-->
[6]: ./media/security-center-features-retirement-july2019/asc_identity.png
[7]: ./media/security-center-features-retirement-july2019/asc_identity_workspace_selection.png
[8]: ./media/security-center-features-retirement-july2019/loganalytics_dashboard_identity.png
[9]: ./media/security-center-features-retirement-july2019/asc_identity_nobuttonhighlight.png
<!--Image references - alerts map-->
[10]: ./media/security-center-features-retirement-july2019/asc_security_alerts_map.png
[11]: ./media/security-center-features-retirement-july2019/asc_threat_intellignece_dashboard.png
[12]: ./media/security-center-features-retirement-july2019/loganalytics_security_alerts_map.png
<!--Image references - custom alerts-->
[13]: ./media/security-center-features-retirement-july2019/asc_custom_alerts.png
<!--Image references - Investigation-->
[14]: ./media/security-center-features-retirement-july2019/asc-security-incident.png
[15]: ./media/security-center-features-retirement-july2019/loganalytics_investigation_dashboard.png
<!--Image references - Solutions-->
[16]: ./media/security-center-features-retirement-july2019/asc_security_solutions.png
<!--Image references - Edit security configurations-->
[17]: ./media/security-center-features-retirement-july2019/asc_edit_security_configurations.png
<!--Image references - Security dashboard in log analytics-->
[18]: ./media/security-center-features-retirement-july2019/loganalytics_security_dashboard.png
