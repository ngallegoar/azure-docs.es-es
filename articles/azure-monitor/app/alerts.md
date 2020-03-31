---
title: Establecimiento de alertas en Azure Application Insights
description: Reciba notificaciones acerca de tiempos de respuesta lentos, excepciones y otros cambios de rendimiento o uso de la aplicación web.
ms.topic: conceptual
ms.date: 01/23/2019
ms.reviewer: lagayhar
ms.subservice: alerts
ms.openlocfilehash: 28fd59556a586b85a6d3caf188d9e02c11d31e3b
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/28/2020
ms.locfileid: "80295076"
---
# <a name="set-alerts-in-application-insights"></a>Definición de alertas en Application Insights

[Azure Application Insights][start] puede avisarle sobre los cambios en las métricas de rendimiento o de uso de la aplicación web. 

Application Insights supervisa la aplicación activa en una [amplia variedad de plataformas][platforms] para ayudarlo a diagnosticar problemas de rendimiento y a entender los patrones de uso.

Hay varios tipos de alertas:

* [**Alertas de métricas**](../../azure-monitor/platform/alerts-metric-overview.md) le avisan cuando una métrica cruza un valor de umbral durante un período determinado: por ejemplo, tiempos de respuesta, recuentos de excepciones, uso de la CPU o vistas de página.
* [**Alertas de registro**](../../azure-monitor/platform/alerts-unified-log.md) se usan para describir aquellas alertas en que la señal de alerta se basa en una consulta de Kusto personalizada.
* Las [**pruebas web**][availability] le informan de que el sitio no está disponible en Internet o responde lentamente. [Más información][availability].
* [**Diagnósticos proactivos**](../../azure-monitor/app/proactive-diagnostics.md) se configuran automáticamente para que le notifiquen acerca de patrones de rendimiento no habituales.

## <a name="how-to-set-an-exception-alert-using-custom-log-search"></a>Establecimiento de una alerta de excepción con búsqueda de registros personalizada

En esta sección, veremos cómo establecer una alerta de excepción basada en consultas. En este ejemplo, supongamos que queremos que se genere una alerta cuando la tasa de error sea superior al 10 % en las últimas 24 horas.

1. Vaya al recurso de Application Insights en Azure Portal.
2. En el lado izquierdo, bajo configurar, haga clic en **Alerta**.

    ![En el lado izquierdo, bajo configurar, clic en Alerta](./media/alerts/1appinsightalert.png)

3. En la parte superior de la pestaña de alerta, seleccione **Nueva regla de alertas**.

     ![En la parte superior de la pestaña de alerta, clic en Nueva regla de alertas](./media/alerts/2createalert.png)

4. El recurso se debería seleccionar automáticamente. Para establecer una condición, haga clic en **Agregar condición**.

    ![Clic en agregar condición](./media/alerts/3addcondition.png)

5. En la pestaña de configurar lógica de señal seleccione **Búsqueda de registros personalizada**.

    ![Clic en búsqueda de registros personalizada](./media/alerts/4customlogsearch.png)

6. En la pestaña de búsqueda de registros personalizada, escriba la consulta en el cuadro "Consulta de búsqueda". En este ejemplo, usaremos la siguiente consulta de Kusto.
    ```kusto
    let percentthreshold = 10;
    let period = 24h;
    requests
    | where timestamp >ago(period)
    | summarize requestsCount = sum(itemCount)
    | project requestsCount, exceptionsCount = toscalar(exceptions | where timestamp >ago(period) | summarize sum(itemCount))
    | extend exceptionsRate = toreal(exceptionsCount)/toreal(requestsCount) * 100
    | where exceptionsRate > percentthreshold

    ```

    ![Escribir la consulta en el cuadro de consulta de búsqueda](./media/alerts/5searchquery.png)
    
    > [!NOTE]
    > También puede aplicar estos pasos para otros tipos de alertas basadas en consultas. Puede aprender más sobre el lenguaje de consulta Kusto en este [documento de introducción a Kusto](https://docs.microsoft.com/azure/kusto/concepts/) o esta [hoja de referencia rápida de SQL a Kusto](https://docs.microsoft.com/azure/kusto/query/sqlcheatsheet).

7. Bajo "Lógica de alerta", elija si se basa en el número de resultados o unidades métricas. A continuación, seleccione la condición (mayor que, igual a, menor que) y un umbral. Mientras cambia estos valores, es posible que observe que cambia la frase de la vista previa de la condición. En este ejemplo, se usa "igual a".

    ![En Lógica de alerta, elegir entre las opciones proporcionadas para basado en y condición, y luego indicar un umbral](./media/alerts/6alertlogic.png)

8. En "Se evaluó basándose en", establezca el período y la frecuencia. El período de aquí debe coincidir con el valor que pusimos para el período en la consulta anterior. A continuación, haga clic en **Listo**.

    ![Establecer el período y la frecuencia en la parte inferior y, a continuación, clic en listo](./media/alerts/7evaluate.png)

9. Ahora vemos la condición que hemos creado con el costo mensual estimado. Debajo de ["Grupos de acciones"](../platform/action-groups.md), puede crear un grupo de recursos nuevo o seleccionar uno existente. Si lo desea, puede personalizar las acciones.

    ![clic en los botones seleccionar o crear en el grupo de acciones](./media/alerts/8actiongroup.png)

10. Por último, agregue los detalles de la alerta (nombre de la regla de alertas, descripción, gravedad). Cuando haya terminado, haga clic en **Crear regla de alertas** en la parte inferior.

    ![En detalle de alertas, escribir el nombre de la regla de alerta, escribir una descripción y elegir un nivel de gravedad](./media/alerts/9alertdetails.png)

## <a name="how-to-unsubscribe-from-classic-alert-e-mail-notifications"></a>Cancelación de la suscripción a las notificaciones clásicas de alerta por correo electrónico

Esta sección se aplica a las **alertas de disponibilidad clásica**, las **alertas de métricas clásicas de Application Insights** y las **alertas de anomalías de error clásicas**.

Recibe notificaciones de correo electrónico para estas alertas clásicas si ocurre cualquiera de las siguientes acciones:

* Su dirección de correo electrónico figura en el campo de los destinatarios de correo electrónico de notificación en la configuración de la regla de alertas.

* La opción para enviar notificaciones por correo electrónico a los usuarios que dispongan de determinados roles para la suscripción está activa, y ocupa un rol correspondiente para esa suscripción de Azure.

![Captura de pantalla de notificación de alerta](./media/alerts/alert-notification.png)

Para controlar mejor la seguridad y privacidad, por lo general, se recomienda especificar explícitamente los destinatarios de la notificación para las alertas clásicas en el campo **Destinatarios del correo electrónico de notificación**. La opción de notificar a todos los usuarios que tienen determinados roles se proporciona por compatibilidad con versiones anteriores.

Para cancelar la suscripción a notificaciones de correo electrónico generadas por una regla de alerta determinada, quite su dirección de correo electrónico del campo **Destinatarios del correo electrónico de notificación**.

Si su dirección de correo electrónico no aparece explícitamente, se recomienda que deshabilite la opción de notificar automáticamente a todos los miembros de determinados roles y, en su lugar, cree una lista con los correos electrónicos de todos los usuarios que tengan que recibir notificaciones de esa regla de alertas en el campo Destinatarios del correo electrónico de notificación.

## <a name="who-receives-the-classic-alert-notifications"></a>¿Quién recibe las notificaciones de alerta (clásicas)?

Esta sección solo se aplica a las alertas clásicas y le ayudará a optimizar las notificaciones de alerta para asegurarse de que solo reciban las notificaciones los destinatarios que elija. Para más información sobre la diferencia entre [alertas clásicas](../platform/alerts-classic.overview.md) y la nueva experiencia de alertas, consulte el [artículo de información general de alertas](../platform/alerts-overview.md). Para controlar las notificaciones de alertas en la nueva experiencia de alertas, use [grupos de acciones](../platform/action-groups.md).

* Se recomienda el uso de destinatarios específicos para las notificaciones de alerta clásicas.

* Para alertas basadas en alguna métrica de Application Insights (incluidas las métricas de disponibilidad), la opción de casilla de verificación **masiva o grupo**, si está habilitada, se envía a los usuarios con roles de propietario, colaborador o lector de la suscripción. De hecho, _todos_ los usuarios con acceso a la suscripción del recurso de Application Insights están dentro del ámbito y recibirán notificaciones.

> [!NOTE]
> Si actualmente usa la opción de casilla de verificación **masiva o grupo** y la deshabilita, no podrá revertir el cambio.

Use la nueva experiencia de alertas o las alertas prácticamente en tiempo real si tiene que enviar notificaciones a los usuarios según sus roles. Con los [grupos de acciones](../platform/action-groups.md), puede configurar el envío de notificaciones por correo electrónico a usuarios con cualquiera de los roles de propietario, colaborador o lector (no combinados conjuntamente como única opción).

## <a name="automation"></a>Automation
* [Uso de PowerShell para automatizar la configuración de alertas](../../azure-monitor/app/powershell-alerts.md)
* [Uso de Webhook para automatizar la respuesta a alertas](../../azure-monitor/platform/alerts-webhooks.md)

## <a name="see-also"></a>Consulte también
* [Pruebas web de disponibilidad](../../azure-monitor/app/monitor-web-app-availability.md)
* [Use PowerShell to set alerts in Application Insights (Uso de PowerShell para definir alertas en Application Insights)](../../azure-monitor/app/powershell-alerts.md)
* [Proactive diagnostics](../../azure-monitor/app/proactive-diagnostics.md) 

<!--Link references-->

[availability]: ../../azure-monitor/app/monitor-web-app-availability.md
[client]: ../../azure-monitor/app/javascript.md
[platforms]: ../../azure-monitor/app/platforms.md
[roles]: ../../azure-monitor/app/resources-roles-access-control.md
[start]: ../../azure-monitor/app/app-insights-overview.md

