---
title: Compatibilidad con volúmenes de tráfico elevado de Application Gateway
description: En este artículo se proporcionan guías para configurar Azure Application Gateway a fin de admitir escenarios de volúmenes de tráfico de red elevado.
services: application-gateway
author: caya
ms.service: application-gateway
ms.topic: article
ms.date: 03/24/2020
ms.author: caya
ms.openlocfilehash: 56622ee2c014bd8dbca7c61ec00b927c56f63a40
ms.sourcegitcommit: 493b27fbfd7917c3823a1e4c313d07331d1b732f
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 05/21/2020
ms.locfileid: "83740406"
---
# <a name="application-gateway-high-traffic-support"></a>Compatibilidad con tráfico elevado en Application Gateway

>[!NOTE]
> En este artículo se describen algunas instrucciones sugeridas que le ayudarán a configurar Application Gateway para administrar el tráfico adicional ocasionado por volúmenes de tráfico elevados que pueden presentarse debido a la crisis provocada por la COVID-19.

Puede usar Application Gateway con el firewall de aplicaciones web (WAF) para contar con una manera escalable y segura de administrar el tráfico a las aplicaciones web.

Las sugerencias siguientes lo ayudarán a configurar Application Gateway con WAF para administrar el tráfico adicional.

## <a name="use-the-v2-sku-over-v1-for-its-autoscaling-capabilities-and-performance-benefits"></a>Uso de la SKU v2 en lugar de la v1 por sus funcionalidades de escalado automático y sus ventajas en cuanto al rendimiento
La SKU v2 ofrece escalado automático para garantizar que Application Gateway se puede escalar verticalmente a medida que aumenta el tráfico. También ofrece otras ventajas importantes en cuanto al rendimiento, como un rendimiento de descarga de TLS cinco veces mejor, tiempos de implementación y actualización más rápidos, redundancia de zona, etc., en comparación con la versión 1. Para más información, consulte la [documentación de la v2](https://docs.microsoft.com/azure/application-gateway/application-gateway-autoscaling-zone-redundant). 

## <a name="set-maximum-instance-count-to-the-maximum-possible-125"></a>Establecimiento del recuento máximo de instancias en el máximo posible (125)
 
Si suponemos que tiene una SKU v2 de Application Gateway, establecer el recuento máximo de instancias en el valor máximo posible de 125 permite que Application Gateway escale de manera horizontal según sea necesario. Esto le perite controlar el posible aumento del tráfico a las aplicaciones. Solo se le cobrarán las unidades de capacidad (CU) que use.  

## <a name="set-your-minimum-instance-count-based-on-your-average-cu-usage"></a>Establecimiento del recuento mínimo de instancias en función del uso de CU promedio

Si suponemos que tiene una SKU v2 de Application Gateway, el escalado automático tarda de seis a siete minutos en escalar horizontalmente. Con un recuento mínimo de instancias mayor, Application Gateway puede controlar mejor el tráfico cuando aumenta la carga, porque un pico en el tráfico no requiere una operación de escalado automático.  

## <a name="alert-if-a-certain-metric-surpasses-75-of-average-cu-utilization"></a>Alerta si una métrica determinada supera el 75 % del uso de CU promedio 
Consulte la [documentación sobre las métricas de Application Gateway](https://docs.microsoft.com/azure/application-gateway/application-gateway-metrics#metrics-visualization) para una explicación detallada de las métricas y otros tutoriales. 

### <a name="example-setting-up-an-alert-on-75-of-average-cu-usage"></a>Ejemplo: Configuración de una alerta para el 75 % del uso de CU promedio

En este ejemplo se muestra cómo usar Azure Portal para configurar una alerta para cuando se alcance el 75 % del uso de CU promedio. 
1. Vaya a su instancia de **Application Gateway**.
2. En el panel de la izquierda, seleccione **Métricas** en la pestaña **Supervisión**. 
3. Agregue una métrica para **Average Current Compute Units** (Unidades de proceso actuales promedio). 
![Configuración de la métrica de WAF](./media/application-gateway-covid-guidelines/waf-setup-metrics.png)
4. Si configuró el recuento mínimo de instancias como su uso de CU promedio, continúe y establezca una alerta para cuando el 75 % de las instancias mínimas esté en uso. Por ejemplo, si el uso promedio es 10 CU, establezca una alerta para 7,5 CU. Esta alerta le avisa si aumenta el uso y le da tiempo para responder. Puede aumentar el mínimo si cree que este tráfico se mantendrá para avisarle de que es posible que el tráfico esté aumentando. 
![Configuración de la alerta de WAF](./media/application-gateway-covid-guidelines/waf-setup-monitoring-alert.png)

> [!NOTE]
> Puede configurar la alerta para que se produzca al llegar a un porcentaje de uso de CU inferior o mayor en función de qué tan sensible quiere que sea con respecto a posibles picos de tráfico.

## <a name="set-up-waf-with-geofiltering-and-bot-protection-to-stop-attacks"></a>Configuración de WAF con filtrado geográfico y protección contra bots para detener los ataques
Si quiere agregar una capa de seguridad frente a la aplicación, use la SKU WAF_v2 de Application Gateway por sus funcionalidades de WAF. Puede configurar la SKU v2 para permitir solo el acceso a las aplicaciones desde países o regiones determinados. Puede configurar una regla personalizada de WAF para permitir o bloquear de manera explícita el tráfico en función de la geolocalización. Para más información, consulte las [reglas personalizadas de filtrado geográfico](https://docs.microsoft.com/azure/web-application-firewall/ag/geomatch-custom-rules) y el artículo sobre cómo [configurar las reglas personalizadas en la SKU WAF_v2 de Application Gateway a través de PowerShell](https://docs.microsoft.com/azure/web-application-firewall/ag/configure-waf-custom-rules).

Habilite la protección contra bots para bloquear bots malintencionados conocidos. Esto debería disminuir la cantidad de tráfico que llega a la aplicación. Para más configuración, consulte las [instrucciones para configurar la protección contra bots](https://docs.microsoft.com/azure/web-application-firewall/ag/configure-waf-custom-rules).

## <a name="turn-on-diagnostics-on-application-gateway-and-waf"></a>Activación de diagnósticos en Application Gateway y WAF

Los registros de diagnóstico permiten ver los registros de firewall, los registros de rendimiento y los registros de acceso. Puede usar estos registros en Azure para administrar y solucionar problemas de Application Gateway. Para más información consulte la [documentación sobre los diagnósticos](https://docs.microsoft.com/azure/application-gateway/application-gateway-diagnostics#diagnostic-logging). 

## <a name="set-up-an-tls-policy-for-extra-security"></a>Configuración de una directiva TLS para seguridad adicional
Asegúrese de que usa la versión más reciente de la directiva TLS ([AppGwSslPolicy20170401S](https://docs.microsoft.com/azure/application-gateway/application-gateway-ssl-policy-overview#appgwsslpolicy20170401s)). Esto exige el uso de TLS 1.2 y cifrados más seguros. Para más información, consulte cómo [configurar las versiones de directivas TLS y conjuntos de cifrado a través de PowerShell](https://docs.microsoft.com/azure/application-gateway/application-gateway-configure-ssl-policy-powershell).
