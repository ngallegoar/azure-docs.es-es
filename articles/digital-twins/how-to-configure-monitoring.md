---
title: 'Configuración de la supervisión: Azure Digital Twins | Microsoft Docs'
description: Aprenda a configurar las opciones de supervisión y registro de Azure Digital Twins.
ms.author: alinast
author: alinamstanciu
manager: bertvanhoof
ms.service: digital-twins
services: digital-twins
ms.topic: conceptual
ms.date: 01/21/2020
ms.custom: seodec18
ms.openlocfilehash: e35e18be20af3bd9f6fdc9541f9abfe857a6b87c
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/27/2020
ms.locfileid: "76511865"
---
# <a name="how-to-configure-monitoring-in-azure-digital-twins"></a>Configuración de la supervisión en Azure Digital Twins

Azure Digital Twins es compatible con un eficaz sistema de registro, supervisión y análisis. Los desarrolladores de soluciones pueden usar los registros de Azure Monitor, los registros de diagnóstico, el registro de actividad y otros servicios para dar soporte a las complejas necesidades de supervisión de una aplicación de IoT. Las opciones de registro se pueden combinar para consultar o mostrar registros en diferentes servicios y ofrecer una cobertura de registro pormenorizada para muchos servicios.

En este artículo se resumen las opciones de registro y supervisión y cómo combinarlas de manera específica para Azure Digital Twins.

[!INCLUDE [azure-monitor-log-analytics-rebrand](../../includes/azure-monitor-log-analytics-rebrand.md)]

## <a name="review-activity-logs"></a>Revisión de registros de actividad

Los [registros de actividad](../azure-monitor/platform/platform-logs-overview.md) de Azure proporcionan información rápida sobre los eventos a nivel de suscripción y los historiales de operación para cada instancia de servicio de Azure.

Los eventos a nivel de suscripción incluyen:

* Creación de recursos
* Eliminación de recursos
* Creación de secretos de aplicación
* Integración con otros servicios

El registro de actividad de Azure Digital Twins está habilitado de forma predeterminada; para encontrarlo en Azure Portal, puede hacer lo siguiente:

1. Seleccionar la instancia de Azure Digital Twins.
1. Elegir **Registro de actividad** para que aparezca el panel de pantalla:

    [![Registro de actividad](media/how-to-configure-monitoring/activity-log.png)](media/how-to-configure-monitoring/activity-log.png#lightbox)

Para el registro de actividad avanzada:

1. Seleccione la opción **Registros** para mostrar la **información general de Activity Log Analytics**:

    [![Número de selección](media/how-to-configure-monitoring/activity-log-select.png)](media/how-to-configure-monitoring/activity-log-select.png#lightbox)

1. La **información general de Activity Log Analytics** resume los datos de registro de actividad esenciales:

    [![Introducción al análisis de registros de actividad]( media/how-to-configure-monitoring/log-analytics-overview.png)]( media/how-to-configure-monitoring/log-analytics-overview.png#lightbox)

>[!TIP]
>Use los **registros de actividad** para obtener información rápida sobre los eventos a nivel de suscripción.

## <a name="enable-customer-diagnostic-logs"></a>Habilitación de registros de diagnóstico de cliente

Es posible establecer la [configuración de diagnóstico](../azure-monitor/platform/platform-logs-overview.md) de Azure para cada instancia de Azure a fin de complementar el registro de actividad. Si bien los registros de actividad pertenecen a los eventos de nivel de suscripción, el registro de diagnóstico proporciona información sobre el historial de operaciones de los propios recursos.

Estos son algunos ejemplos del registro de diagnóstico:

* El tiempo de ejecución de una función definida por el usuario
* El código de estado de respuesta de una solicitud de API correcta
* La recuperación de una clave de aplicación de un almacén

Para habilitar los registros de diagnóstico para una instancia:

1. Abra el recurso en Azure Portal.
1. Seleccione **Configuración de diagnóstico**:

    [![Configuración de diagnóstico número uno](media/how-to-configure-monitoring/diagnostic-settings-one.png)](media/how-to-configure-monitoring/diagnostic-settings-one.png#lightbox)

1. Seleccione **Activar diagnóstico** para recopilar datos (si no se ha habilitado anteriormente).
1. Rellene los campos solicitados y seleccione cómo y dónde se guardarán los datos:

    [![Configuración de diagnóstico número dos](media/how-to-configure-monitoring/diagnostic-settings-two.png)](media/how-to-configure-monitoring/diagnostic-settings-two.png#lightbox)

    Los registros de diagnóstico suelen guardarse con [Azure File Storage](../storage/files/storage-files-deployment-guide.md) y compartirse con [registros de Azure Monitor](../azure-monitor/log-query/get-started-portal.md). Ambas opciones se pueden seleccionar.

>[!TIP]
>Use los **registros de diagnóstico** para obtener información sobre las operaciones de recursos.

## <a name="azure-monitor-and-log-analytics"></a>Azure Monitor y Log Analytics

Las aplicaciones de IoT reúnen recursos dispares, dispositivos, ubicaciones y datos en un mismo lugar. El registro pormenorizado proporciona información detallada sobre cada pieza, servicio o componente específico de la arquitectura general de la aplicación, pero a menudo se requiere una perspectiva unificada para abordar el mantenimiento y la depuración.

Azure Monitor incluye el eficaz servicio de análisis de registros, que permite ver y analizar los orígenes de registro en una sola ubicación. Azure Monitor, por tanto, es muy útil para analizar los registros dentro de sofisticadas aplicaciones de IoT.

Estos son algunos ejemplos de uso:

* Consultar varios historiales de registro de diagnóstico
* Ver los registros de varias funciones definidas por el usuario
* Mostrar los registros de dos o más servicios en un período de tiempo específico

La consulta del registro completo se proporciona a través de [registros de Azure Monitor](../azure-monitor/log-query/log-query-overview.md). Para configurar estas eficaces características:

1. Busque **Log Analytics** en Azure Portal.
1. Se mostrarán las instancias de **área de trabajo de Log Analytics** disponibles. Elija una y seleccione **Registros** para consultar:

    [![Log Analytics](media/how-to-configure-monitoring/log-analytics.png)](media/how-to-configure-monitoring/log-analytics.png#lightbox)

1. Si aún no tiene una instancia de **área de trabajo de Log Analytics**, puede crear un área de trabajo seleccionando el botón **Agregar**:

    [![Creación de OMS](media/how-to-configure-monitoring/log-analytics-oms.png)](media/how-to-configure-monitoring/log-analytics-oms.png#lightbox)

Una vez que esté aprovisionada su instancia de **área de trabajo de Log Analytics**, puede usar funciones de consulta potentes para buscar entradas en varios registros o buscar mediante criterios específicos mediante **Administración de registros**:

   [![Administración de registros](media/how-to-configure-monitoring/log-analytics-management.png)](media/how-to-configure-monitoring/log-analytics-management.png#lightbox)

Para obtener más información acerca de estas eficaces operaciones de consulta, consulte la [introducción a las consultas](../azure-monitor/log-query/get-started-queries.md).

> [!NOTE]
> Podría experimentar un retraso de 5 minutos al enviar eventos al **área de trabajo de Log Analytics** por primera vez.

Los registros de Azure Monitor también dispone de potentes servicios de notificación de errores y alertas, que pueden verse al seleccionar **Diagnóstico y solución de problemas**:

   [![Notificaciones de alerta y error](media/how-to-configure-monitoring/log-analytics-notifications.png)](media/how-to-configure-monitoring/log-analytics-notifications.png#lightbox)

>[!TIP]
>Use el **área de trabajo de Log Analytics** para consultar los historiales de registro de las funcionalidades, suscripciones o servicios de varias aplicaciones.

## <a name="other-options"></a>Otras opciones

Azure Digital Twins también admite el registro y la auditoría de seguridad específicos de una aplicación. Para obtener una introducción exhaustiva de todas las opciones de registro de Azure disponibles para la instancia de Azure Digital Twins, lea el artículo [Registro y auditoría de Azure](../security/fundamentals/log-audit.md).

## <a name="next-steps"></a>Pasos siguientes

- Obtenga más información sobre los [registros de actividad](../azure-monitor/platform/platform-logs-overview.md).

- Profundice en la configuración de diagnóstico de Azure consultando la [información general sobre los registros de diagnóstico de Azure](../azure-monitor/platform/platform-logs-overview.md).

- Obtenga más información sobre [registros de Azure Monitor](../azure-monitor/log-query/get-started-portal.md).
