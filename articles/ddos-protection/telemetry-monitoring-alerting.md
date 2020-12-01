---
title: Visualización y configuración de DDoS Protection
description: Aprenda a ver y configurar la telemetría de DDoS Protection.
services: ddos-protection
documentationcenter: na
author: yitoh
ms.service: ddos-protection
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 09/08/2020
ms.author: yitoh
ms.openlocfilehash: 5c80e5e611c275c2a2262963aa0759075fca836b
ms.sourcegitcommit: 9889a3983b88222c30275fd0cfe60807976fd65b
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 11/20/2020
ms.locfileid: "94989401"
---
# <a name="view-and-configure-ddos-protection-telemetry"></a>Visualización y configuración de DDoS Protection

La versión estándar de Azure DDoS Protection proporciona información detallada y visualización de ataques con DDoS Attack Analytics. Los clientes que protegen sus redes virtuales frente a ataques DDoS disponen de visibilidad detallada sobre el tráfico de ataques y las acciones llevadas a cabo para mitigarlos mediante informes de mitigación de ataques y registros de flujo de mitigación. La telemetría enriquecida se expone a través de Azure Monitor, incluidas las métricas detalladas mientras dure un ataque DDoS. Las alertas se pueden configurar para cualquiera de las métricas de Azure Monitor que expone DDoS Protection. El registro se puede integrar con [Azure Sentinel](../sentinel/connect-azure-ddos-protection.md), Splunk (Azure Event Hubs), OMS Log Analytics y Azure Storage para realizar análisis avanzados con la interfaz de Azure Monitor Diagnostics.

En este tutorial, aprenderá a:

> [!div class="checklist"]
> * Configuración de alertas para métricas de protección contra DDoS
> * Uso de telemetría de protección contra DDoS
> * Visualización de las directivas de mitigación de DDoS
> * Ver las alertas de protección DDoS en Azure Security Center

## <a name="prerequisites"></a>Prerrequisitos

- Si no tiene una suscripción a Azure, cree una [cuenta gratuita](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) antes de empezar.
- Para poder completar los pasos de este tutorial, primero debe crear un [plan de protección de Azure DDoS Estándar](manage-ddos-protection.md).

## <a name="configure-alerts-for-ddos-protection-metrics"></a>Configuración de alertas para métricas de protección contra DDoS

Puede seleccionar cualquiera de las métricas de protección contra DDoS disponibles para que le avisen cuando hay una mitigación activa durante un ataque mediante la configuración de alertas de Azure Monitor. Cuando se cumplan las condiciones, recibirá un correo electrónico de alerta en la dirección especificada:

1. Seleccione **Todos los servicios** en la parte superior izquierda del portal.
2. Escriba *Monitor* en el cuadro **Filtrar**. Seleccione **Monitor** cuando aparezca en los resultados.
3. Seleccione **Métricas** en **SERVICIOS COMPARTIDOS**.
4. Escriba o seleccione sus propios valores, o bien escriba los valores de ejemplo siguientes, acepte el resto de los valores predeterminados y, luego, seleccione **Aceptar**:

    |Configuración                  |Value                                                                                               |
    |---------                |---------                                                                                           |
    |Nombre                     | Escriba _MyDdosAlert_.                                                                                |
    |Subscription             | Seleccione la suscripción que contiene la dirección IP pública para la que desea recibir alertas.        |
    |Resource group           | Seleccione el grupo de recursos que contiene la dirección IP pública para la que desea recibir alertas.      |
    |Resource                 | Seleccione la dirección IP pública que contiene la dirección IP pública para la que desea recibir alertas. DDoS supervisa las direcciones IP públicas asignadas a los recursos dentro de una red virtual. Si no tiene ningún recurso con direcciones IP públicas en la red virtual, primero debe crear un recurso con una dirección IP pública. Puede supervisar la dirección IP pública de todos los recursos implementados a través de Resource Manager (no clásico) que aparecen en [Red virtual para servicios de Azure](../virtual-network/virtual-network-for-azure-services.md#services-that-can-be-deployed-into-a-virtual-network), excepto para entornos de Azure App Service y Azure VPN Gateway. Para continuar con este tutorial, puede crear rápidamente una máquina virtual [Windows](../virtual-machines/windows/quick-create-portal.md?toc=%2fazure%2fvirtual-network%2ftoc.json) o [Linux](../virtual-machines/linux/quick-create-portal.md?toc=%2fazure%2fvirtual-network%2ftoc.json).                   |
    |Métrica                   | Seleccione **Bajo ataque DDoS o no**.                                                                |
    |Umbral                | 1: un **1** significa que está siendo atacado. Un **0** significa que no está siendo atacado.                         |
    |Período                   | Seleccione el valor que prefiera.                                                                   |
    |Notificar por correo electrónico         | Active la casilla.                                                                                 |
    |Administrador adicional | Escriba su dirección de correo electrónico si no es propietario del correo electrónico, colaborador o lector de la suscripción. |

    Dentro de unos pocos minutos después de la detección del ataque, recibirá un correo electrónico con métricas de Azure Monitor que tiene un aspecto similares a la imagen siguiente:

    ![Alerta de ataque](./media/manage-ddos-protection/ddos-alert.png)


Para simular un ataque de DDoS para validar la alerta, consulte [Validación de la detección de DDoS](test-through-simulations.md).

Puede aprender más sobre [configurar webhooks](../azure-monitor/platform/alerts-webhooks.md?toc=%2fazure%2fvirtual-network%2ftoc.json) y [aplicaciones lógicas](../logic-apps/logic-apps-overview.md?toc=%2fazure%2fvirtual-network%2ftoc.json) para la creación de alertas.

## <a name="use-ddos-protection-telemetry"></a>Uso de telemetría de protección contra DDoS

La telemetría para un ataque se proporciona a través de Azure Monitor en tiempo real. La telemetría está disponible solo durante el tiempo en que una dirección IP pública está en proceso de mitigación. No ve ninguna telemetría antes o después de mitigar un ataque.

1. Seleccione **Todos los servicios** en la parte superior izquierda del portal.
2. Escriba *Monitor* en el cuadro **Filtrar**. Seleccione **Monitor** cuando aparezca en los resultados.
3. Seleccione **Métricas** en **SERVICIOS COMPARTIDOS**.
4. Seleccione la **suscripción** y el **grupo de recursos** que contienen la dirección IP pública para la que desea la telemetría.
5. Seleccione la **dirección IP pública** para el **tipo de recurso**, luego seleccione la dirección IP pública específica para la que desea la telemetría.
6. En el lado izquierdo de la pantalla se muestra una serie de **métricas disponibles**. Cuando se seleccionan estas métricas, se representan en el **gráfico de métricas de Azure Monitor**, en la pantalla de información general.
7. Seleccione el tipo de **agregación** como **Máximo**.

Los nombres de las métricas presentan distintos tipos de paquetes y bytes frente a paquetes, con una construcción básica de nombres de etiqueta en cada métrica, como se muestra aquí:

- **Nombre de etiqueta de eliminado** (por ejemplo, **Paquetes de entrada quitados por DDoS**): El número de paquetes que el sistema de protección contra DDoS elimina o limpia.
- **Nombre de etiqueta de reenviado** (por ejemplo, **Paquetes de entrada reenviados por DDoS**): el número de paquetes que reenvía el sistema DDoS a la dirección IP virtual de destino (tráfico que no se filtró).
- **Sin nombre de etiqueta** (por ejemplo **DDoS de paquetes de entrada**): el número total de paquetes que han llegado al sistema de limpieza (representa la suma de los paquetes eliminados y los reenviados).

Esta [regla de alertas de Azure Monitor](https://github.com/Azure/Azure-Network-Security/tree/master/Azure%20DDoS%20Protection/Azure%20Monitor%20Alert%20-%20DDoS%20Mitigation%20Started) ejecutará una consulta simple para detectar cuándo se está produciendo una mitigación de DDoS activa. Para simular un ataque de DDoS para validar la telemetría, consulte [Validación de la detección de DDoS](test-through-simulations.md). 

## <a name="view-ddos-mitigation-policies"></a>Visualización de las directivas de mitigación de DDoS

DDoS Protection Standard aplica tres directivas de mitigación de ajuste automático (TCP SYN, TCP y UDP) a cada dirección IP pública del recurso protegido, en la red virtual que tiene habilitado DDoS. Puede ver los umbrales de directiva si selecciona las métricas **Paquetes entrantes TCP para desencadenar la mitigación de DDoS** y **Paquetes entrantes UDP para desencadenar la mitigación de DDoS** con el tipo de **agregación** como "Máximo", tal como se muestra en la siguiente imagen:

![Vista de directivas de mitigación](./media/manage-ddos-protection/view-mitigation-policies.png)

Los umbrales de las directivas se configuran automáticamente con nuestro sistema de generación de perfiles de tráfico de red basado en aprendizaje automático de Azure. Solo cuando se supera el umbral de la directiva, la mitigación de DDoS se produce para la dirección IP que está siendo atacada.

## <a name="view-ddos-protection-alerts-in-azure-security-center"></a>Ver las alertas de protección DDoS en Azure Security Center

Azure Security Center proporciona una lista de [alertas de seguridad](../security-center/security-center-managing-and-responding-alerts.md), que cuentan con información para ayudarle a investigar y solucionar problemas. Gracias a esta característica, obtendrá una vista unificada de las alertas, incluyendo las alertas relacionadas con los ataques DDoS y las acciones que se tomaron para mitigar el ataque a corto plazo.
Hay dos alertas específicas que verá en cualquier mitigación y detección de ataques DDoS:

- **Ataque DDoS detectado para IP pública**: Esta alerta se genera cuando el servicio de protección contra DDoS detecta que una de sus direcciones IP públicas es el objetivo de un ataque DDoS.
- **Ataque DDoS mitigado para IP pública**: Esta alerta se genera cuando se ha mitigado un ataque a la dirección IP pública.
Para ver las alertas, abra el **Centro de seguridad** en Azure Portal. En **Protección contra amenazas**, seleccione **Alertas de seguridad**. La siguiente captura de pantalla muestra un ejemplo de las alertas de ataques DDoS.

![Alerta de ataque DDoS en Azure Security Center](./media/manage-ddos-protection/ddos-alert-asc.png)

Las alertas incluyen información general sobre la dirección IP pública que está bajo ataque, información de inteligencia geográfica y de amenazas y los pasos para solucionar el problema.

## <a name="next-steps"></a>Pasos siguientes

En este tutorial, ha aprendido a:

- Configuración de alertas para métricas de protección contra DDoS
- Uso de telemetría de protección contra DDoS
- Visualización de las directivas de mitigación de DDoS
- Ver las alertas de protección DDoS en Azure Security Center

Para obtener información sobre cómo configurar los informes de mitigación de ataques y los registros de flujo, continúe con el siguiente tutorial.

> [!div class="nextstepaction"]
> [Configuración de registros de flujo e informes de mitigación de ataques DDoS](reports-and-flow-logs.md)