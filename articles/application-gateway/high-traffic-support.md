---
title: Compatibilidad con volúmenes de tráfico elevado de Application Gateway
description: En este artículo se proporcionan guías para configurar Azure Application Gateway a fin de admitir escenarios de volúmenes de tráfico de red elevado.
services: application-gateway
author: caya
ms.service: application-gateway
ms.topic: conceptual
ms.date: 03/24/2020
ms.author: caya
ms.openlocfilehash: a5f7569fc46d4678ca0c12299e33caa3c78df849
ms.sourcegitcommit: d22a86a1329be8fd1913ce4d1bfbd2a125b2bcae
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 11/26/2020
ms.locfileid: "96182927"
---
# <a name="application-gateway-high-traffic-support"></a>Compatibilidad con tráfico elevado en Application Gateway

>[!NOTE]
> En este artículo se describen algunas instrucciones sugeridas que le ayudarán a configurar Application Gateway para administrar el tráfico adicional en los volúmenes de tráfico elevados que pueden presentarse. Los umbrales de alerta son solo sugerencias y son de naturaleza genérica. Los usuarios pueden determinar los umbrales de alerta en función de sus previsiones de uso y carga de trabajo.

Puede usar Application Gateway con el firewall de aplicaciones web (WAF) para contar con una manera escalable y segura de administrar el tráfico a las aplicaciones web.

Es importante que escale su instancia de Application Gateway en función del tráfico e incluya un poco de búfer; de este modo, estará preparado para cualquier aumento de tráfico o actividad, y minimizará el impacto que pueda tener en su calidad de servicio. Las sugerencias siguientes lo ayudarán a configurar Application Gateway con WAF para administrar el tráfico adicional.

Consulte la [documentación de métricas](./application-gateway-metrics.md) para obtener una lista completa de las métricas que ofrece Application Gateway. Consulte la [visualización de métricas](./application-gateway-metrics.md#metrics-visualization) en Azure Portal y la [documentación de Azure Monitor](../azure-monitor/platform/alerts-metric.md) sobre cómo establecer alertas para las métricas.

## <a name="scaling-for-application-gateway-v1-sku-standardwaf-sku"></a>Escalado para la SKU de Application Gateway v1 (SKU estándar/WAF)

### <a name="set-your-instance-count-based-on-your-peak-cpu-usage"></a>Establecimiento del recuento de instancias en función del uso máximo de CPU
Si usa una puerta de enlace de SKU v1, tendrá la capacidad de configurar hasta 32 instancias de Application Gateway para escalado. Compruebe el uso de CPU de Application Gateway en el último mes para conocer los picos de actividad por encima del 80 %. Esta información está disponible como una métrica que puede supervisar. Se recomienda establecer el recuento de instancias según el uso máximo con un 10 % a un 20 % de búfer adicional para incluir los picos de tráfico.

:::image type="content" source="./media/application-gateway-covid-guidelines/v1-cpu-utilization-inline.png" alt-text="Métrica de uso de CPU en v1" lightbox="./media/application-gateway-covid-guidelines/v1-cpu-utilization-exp.png":::

### <a name="use-the-v2-sku-over-v1-for-its-autoscaling-capabilities-and-performance-benefits"></a>Uso de la SKU v2 en lugar de la v1 por sus funcionalidades de escalado automático y sus ventajas en cuanto al rendimiento
La SKU v2 ofrece escalado automático para garantizar que Application Gateway se puede escalar verticalmente a medida que aumenta el tráfico. También ofrece otras ventajas importantes en cuanto al rendimiento, como un rendimiento de descarga de TLS cinco veces mejor, tiempos de implementación y actualización más rápidos, redundancia de zona, etc., en comparación con la versión 1. Para obtener más información, consulte la [documentación de la v2](./application-gateway-autoscaling-zone-redundant.md) y visite nuestra [documentación de migración](./migrate-v1-v2.md) de la versión v1 a v2 para aprender a migrar las puertas de enlace de la SKU v1 existentes a la SKU v2. 

## <a name="autoscaling-for-application-gateway-v2-sku-standard_v2waf_v2-sku"></a>Escalado automático para la SKU de Application Gateway v2 (SKU de Standard_v2/WAF_v2)

### <a name="set-maximum-instance-count-to-the-maximum-possible-125"></a>Establecimiento del recuento máximo de instancias en el máximo posible (125)
 
En el caso de una SKU de Application Gateway v2, establecer el recuento máximo de instancias en el valor máximo posible de 125 permite que Application Gateway escale de manera horizontal según sea necesario. Esto le perite controlar el posible aumento del tráfico a las aplicaciones. Solo se le cobrarán las unidades de capacidad (CU) que use. 

Asegúrese de comprobar el tamaño de la subred y el recuento de direcciones IP disponibles en la subred, así como de establecer el recuento de instancias máximo en función de esta información. Si la subred no tiene suficiente espacio para alojar a las instancias, tendrá que volver a crear la puerta de enlace en la misma subred o en otra diferente que tenga suficiente capacidad. 

:::image type="content" source="./media/application-gateway-covid-guidelines/v2-autoscaling-max-instances-inline.png" alt-text="Configuración de escalado automático v2" lightbox="./media/application-gateway-covid-guidelines/v2-autoscaling-max-instances-exp.png":::

### <a name="set-your-minimum-instance-count-based-on-your-average-compute-unit-usage"></a>Establecimiento del recuento mínimo de instancias en función del uso de unidad de proceso promedio

En el caso de la SKU de Application Gateway v2, el escalado automático tarda entre seis y siete minutos en escalar horizontalmente y aprovisionar un conjunto adicional de instancias preparado para recibir el tráfico. Hasta entonces, si hay picos de actividad breves en el tráfico, es posible que las instancias de puerta de enlace existentes se sometan a una sobrecarga y esto puede provocar una latencia o pérdida de tráfico inesperadas. 

Se recomienda establecer el recuento mínimo de instancias en un nivel óptimo. Por ejemplo, si necesita 50 instancias para controlar el tráfico durante la carga máxima, establecer el mínimo entre 25 y 30 es una buena idea, en lugar de un número menor que 10, de modo que incluso cuando haya ráfagas breves de tráfico, Application Gateway podrá administrarlo y tendrá tiempo suficiente para que el escalado automático responda y surta efecto.

Compruebe la métrica de unidad de proceso durante el último mes. La métrica de unidad de proceso es una representación del uso de CPU de la puerta de enlace y, en función del uso máximo dividido entre 10, puede establecer el número mínimo de instancias necesarias. Tenga en cuenta que una instancia de Application Gateway puede administrar un mínimo de diez unidades de proceso

:::image type="content" source="./media/application-gateway-covid-guidelines/compute-unit-metrics-inline.png" alt-text="Métricas de unidad de proceso v2" lightbox="./media/application-gateway-covid-guidelines/compute-unit-metrics-exp.png":::

## <a name="manual-scaling-for-application-gateway-v2-sku-standard_v2waf_v2"></a>Escalado manual para la SKU de Application Gateway v2 (Standard_v2/WAF_v2)

### <a name="set-your-instance-count-based-on-your-peak-compute-unit-usage"></a>Establecimiento del recuento de instancias en función del uso máximo de unidades de proceso 

A diferencia del escalado automático, en el escalado manual debe establecer manualmente el número de instancias de la puerta de enlace de aplicaciones en función de las necesidades de tráfico. Se recomienda establecer el recuento de instancias según el uso máximo con un 10 % a un 20 % de búfer adicional para incluir los picos de tráfico. Por ejemplo, si el tráfico requiere 50 instancias en el máximo de actividad, aprovisione de 55 a 60 instancias para administrar los picos de tráfico inesperados que se pueden producir.

Compruebe la métrica de unidad de proceso durante el último mes. La métrica de unidad de proceso es una representación del uso de CPU de la puerta de enlace y, en función del uso máximo dividido entre 10, puede establecer el número de instancias necesarias, ya que una instancia de Application Gateway puede administrar un mínimo de 10 unidades de proceso.

## <a name="monitoring-and-alerting"></a>Supervisión y alertas

Para recibir notificaciones de las anomalías de tráfico o uso, puede configurar alertas sobre determinadas métricas. Consulte la [documentación de métricas](./application-gateway-metrics.md) para obtener una lista completa de las métricas que ofrece Application Gateway. Consulte la [visualización de métricas](./application-gateway-metrics.md#metrics-visualization) en Azure Portal y la [documentación de Azure Monitor](../azure-monitor/platform/alerts-metric.md) sobre cómo establecer alertas para las métricas.

## <a name="alerts-for-application-gateway-v1-sku-standardwaf"></a>Alertas para la SKU de Application Gateway v1 (Estándar/WAF)

### <a name="alert-if-average-cpu-utilization-crosses-80"></a>Envío de alerta si el uso de CPU promedio supera el 80 %

En condiciones normales, el uso de CPU no debe superar con regularidad el 90 %, ya que esto puede provocar latencia en los sitios web hospedados tras Application Gateway e interrumpir la experiencia del cliente. Para controlar o mejorar indirectamente el uso de CPU, puede modificar la configuración de Application Gateway aumentando el número de instancias o pasando a un tamaño de SKU mayor, o bien haciendo ambas cosas. Establezca una alerta si la métrica de uso de CPU supera el promedio del 80 %.

### <a name="alert-if-unhealthy-host-count-crosses-threshold"></a>Envío de alerta si el número de hosts incorrectos supera el umbral

Esta métrica indica el número de servidores back-end que Application Gateway no puede sondear correctamente. Detectará problemas en los que las instancias de Application Gateway no pueden conectarse al servidor back-end. Envíe una alerta si este número supera el 20 % de la capacidad de back-end. Por ejemplo, si actualmente tiene 30 servidores back-end en su grupo de back-end, establezca una alerta si el número de hosts incorrectos es superior a 6.

### <a name="alert-if-response-status-4xx-5xx-crosses-threshold"></a>Envío de alerta si el estado de respuesta (4xx, 5xx) supera el umbral 

Cree una alerta cuando el estado de respuesta de Application Gateway sea 4xx o 5xx. Podría haber una respuesta 4xx o 5xx ocasional debido a problemas transitorios. Debe observar la puerta de enlace en producción para determinar el umbral estático, o usar el umbral dinámico para la alerta.

### <a name="alert-if-failed-requests-crosses-threshold"></a>Envío de alerta si las solicitudes con error superan el umbral 

Cree una alerta cuando la métrica de solicitudes con error supere el umbral. Debe observar la puerta de enlace en producción para determinar el umbral estático, o usar el umbral dinámico para la alerta.

### <a name="example-setting-up-an-alert-for-more-than-100-failed-requests-in-the-last-5-minutes"></a>Ejemplo: configuración de una alerta para más de 100 solicitudes con error en los últimos 5 minutos

En este ejemplo se muestra cómo usar Azure Portal para configurar una alerta cuando el recuento de solicitudes con error en los últimos 5 minutos sea superior a 100.
1. Vaya a su instancia de **Application Gateway**.
2. En el panel de la izquierda, seleccione **Métricas** en la pestaña **Supervisión**. 
3. Agregue una métrica para las **solicitudes con error**.
4. Haga clic en **Nueva regla de alerta** y defina la condición y las acciones.
5. Haga clic en **Crear regla de alerta** para crear y habilitar la alerta.

:::image type="content" source="./media/application-gateway-covid-guidelines/create-alerts-inline.png" alt-text="Creación de alertas v2" lightbox="./media/application-gateway-covid-guidelines/create-alerts-exp.png":::

## <a name="alerts-for-application-gateway-v2-sku-standard_v2waf_v2"></a>Alertas para la SKU de Application Gateway v2 (Standard_v2/WAF_v2)

### <a name="alert-if-compute-unit-utilization-crosses-75-of-average-usage"></a>Alerta si el uso de la unidad de proceso supera el 75 % del uso promedio 

La unidad de proceso es la medición del uso de proceso por parte de la instancia de Application Gateway. Compruebe el promedio de uso de la unidad de proceso en el último mes y configure una alerta si supera el 75 % del uso. Por ejemplo, si el uso promedio es 10 unidades de proceso, establezca una alerta para 7,5 unidades de proceso. Esta alerta le avisa si aumenta el uso y le da tiempo para responder. Puede aumentar el mínimo si cree que este tráfico se mantendrá para avisarle de que es posible que el tráfico esté aumentando. Siga las sugerencias de escalado anteriores para escalar horizontalmente según sea necesario.

### <a name="example-setting-up-an-alert-on-75-of-average-cu-usage"></a>Ejemplo: Configuración de una alerta para el 75 % del uso de CU promedio

En este ejemplo se muestra cómo usar Azure Portal para configurar una alerta para cuando se alcance el 75 % del uso de CU promedio. 
1. Vaya a su instancia de **Application Gateway**.
2. En el panel de la izquierda, seleccione **Métricas** en la pestaña **Supervisión**. 
3. Agregue una métrica para **Average Current Compute Units** (Unidades de proceso actuales promedio). 
4. Si configuró el recuento mínimo de instancias como su uso de CU promedio, continúe y establezca una alerta para cuando el 75 % de las instancias mínimas esté en uso. Por ejemplo, si el uso promedio es 10 CU, establezca una alerta para 7,5 CU. Esta alerta le avisa si aumenta el uso y le da tiempo para responder. Puede aumentar el mínimo si cree que este tráfico se mantendrá para avisarle de que es posible que el tráfico esté aumentando. 

:::image type="content" source="./media/application-gateway-covid-guidelines/compute-unit-alert-inline.png" alt-text="Alertas de unidad de proceso v2" lightbox="./media/application-gateway-covid-guidelines/compute-unit-alert-exp.png":::

> [!NOTE]
> Puede configurar la alerta para que se produzca al llegar a un porcentaje de uso de CU inferior o mayor en función de qué tan sensible quiere que sea con respecto a posibles picos de tráfico.

### <a name="alert-if-capacity-unit-utilization-crosses-75-of-peak-usage"></a>Envío de alerta si el uso de la unidad de capacidad supera el 75 % del uso máximo 

Las unidades de capacidad representan el uso general de la puerta de enlace en términos de rendimiento, proceso y recuento de conexiones. Compruebe el uso máximo de la unidad de capacidad en el último mes y configure una alerta si supera el 75 % del uso. Por ejemplo, si el uso máximo es 100 unidades de capacidad, establezca una alerta para 75 unidades de capacidad. Siga las dos sugerencias anteriores para escalar horizontalmente según sea necesario.

### <a name="alert-if-unhealthy-host-count-crosses-threshold"></a>Envío de alerta si el número de hosts incorrectos supera el umbral 

Esta métrica indica el número de servidores back-end que Application Gateway no puede sondear correctamente. Detectará problemas en los que las instancias de Application Gateway no pueden conectarse al servidor back-end. Envíe una alerta si este número supera el 20 % de la capacidad de back-end. Por ejemplo, si actualmente tiene 30 servidores back-end en su grupo de back-end, establezca una alerta si el número de hosts incorrectos es superior a 6.

### <a name="alert-if-response-status-4xx-5xx-crosses-threshold"></a>Envío de alerta si el estado de respuesta (4xx, 5xx) supera el umbral 

Cree una alerta cuando el estado de respuesta de Application Gateway sea 4xx o 5xx. Podría haber una respuesta 4xx o 5xx ocasional debido a problemas transitorios. Debe observar la puerta de enlace en producción para determinar el umbral estático, o usar el umbral dinámico para la alerta.

### <a name="alert-if-failed-requests-crosses-threshold"></a>Envío de alerta si las solicitudes con error superan el umbral 

Cree una alerta cuando la métrica de solicitudes con error supere el umbral. Debe observar la puerta de enlace en producción para determinar el umbral estático, o usar el umbral dinámico para la alerta.

### <a name="alert-if-backend-last-byte-response-time-crosses-threshold"></a>Envío de alerta si el tiempo de respuesta del último byte de back-end supera el umbral 

Esta métrica indica el intervalo de tiempo entre el inicio del establecimiento de una conexión con el servidor back-end y la recepción del último byte del cuerpo de la respuesta. Cree una alerta si la latencia de respuesta de back-end es mayor que un umbral determinado en relación con lo habitual. Por ejemplo, establezca esta alerta para que se le avise cuando la latencia de respuesta de back-end aumente en más del 30 % del valor habitual.

### <a name="alert-if-application-gateway-total-time-crosses-threshold"></a>Envío de alerta si el tiempo total de Application Gateway supera el umbral

Este es el intervalo desde el momento en que Application Gateway recibe el primer byte de la solicitud HTTP hasta el momento en que se envía el último byte de respuesta al cliente. Debe crear una alerta si la latencia de respuesta de back-end es mayor que un umbral determinado en relación con lo habitual. Por ejemplo, puede establecer que se le avise cuando la latencia de tiempo total aumente en más del 30 % del valor habitual.

## <a name="set-up-waf-with-geo-filtering-and-bot-protection-to-stop-attacks"></a>Configuración de WAF con filtrado geográfico y protección contra bots para detener los ataques
Si quiere agregar una capa de seguridad frente a la aplicación, use la SKU WAF_v2 de Application Gateway por sus funcionalidades de WAF. Puede configurar la SKU v2 para permitir solo el acceso a las aplicaciones desde países o regiones determinados. Puede configurar una regla personalizada de WAF para permitir o bloquear de manera explícita el tráfico en función de la ubicación geográfica. Para más información, consulte las [reglas personalizadas de filtrado geográfico](../web-application-firewall/ag/geomatch-custom-rules.md) y el artículo sobre cómo [configurar las reglas personalizadas en la SKU WAF_v2 de Application Gateway a través de PowerShell](../web-application-firewall/ag/configure-waf-custom-rules.md).

Habilite la protección contra bots para bloquear bots malintencionados conocidos. Esto debería disminuir la cantidad de tráfico que llega a la aplicación. Para más configuración, consulte las [instrucciones para configurar la protección contra bots](../web-application-firewall/ag/configure-waf-custom-rules.md).

## <a name="turn-on-diagnostics-on-application-gateway-and-waf"></a>Activación de diagnósticos en Application Gateway y WAF

Los registros de diagnóstico permiten ver los registros de firewall, los registros de rendimiento y los registros de acceso. Puede usar estos registros en Azure para administrar y solucionar problemas de Application Gateway. Para más información consulte la [documentación sobre los diagnósticos](./application-gateway-diagnostics.md#diagnostic-logging). 

## <a name="set-up-an-tls-policy-for-extra-security"></a>Configuración de una directiva TLS para seguridad adicional
Asegúrese de que usa la versión más reciente de la directiva TLS ([AppGwSslPolicy20170401S](./application-gateway-ssl-policy-overview.md#appgwsslpolicy20170401s)). Esto exige el uso de TLS 1.2 y cifrados más seguros. Para más información, consulte cómo [configurar las versiones de directivas TLS y conjuntos de cifrado a través de PowerShell](./application-gateway-configure-ssl-policy-powershell.md).