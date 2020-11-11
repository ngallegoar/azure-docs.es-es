---
title: Registros de flujo e informes de Azure DDoS Protection Estándar
description: Aprenda a configurar informes y registros de flujo.
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
ms.openlocfilehash: 5a6fc8e9b316f7c4740ee27fe72c5f056f071d73
ms.sourcegitcommit: d76108b476259fe3f5f20a91ed2c237c1577df14
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/29/2020
ms.locfileid: "92912722"
---
# <a name="configure-ddos-attack-mitigation-reports-and-flow-logs"></a>Configuración de registros de flujo e informes de mitigación de ataques DDoS 

La versión estándar de Azure DDoS Protection proporciona información detallada y visualización de ataques con DDoS Attack Analytics. Los clientes que protegen sus redes virtuales frente a ataques DDoS disponen de visibilidad detallada sobre el tráfico de ataques y las acciones llevadas a cabo para mitigarlos mediante informes de mitigación de ataques y registros de flujo de mitigación. La telemetría enriquecida se expone a través de Azure Monitor, incluidas las métricas detalladas mientras dure un ataque DDoS. Las alertas se pueden configurar para cualquiera de las métricas de Azure Monitor que expone DDoS Protection. El registro se puede integrar con [Azure Sentinel](https://docs.microsoft.com/azure/sentinel/connect-azure-ddos-protection), Splunk (Azure Event Hubs), OMS Log Analytics y Azure Storage para realizar análisis avanzados con la interfaz de Azure Monitor Diagnostics.

En este tutorial, aprenderá a:

> [!div class="checklist"]
> * Visualización y configuración de informes de mitigación de ataques DDoS
> * Visualización y configuración de registros de flujo de mitigación de ataques DDoS

## <a name="prerequisites"></a>Prerrequisitos

- Si no tiene una suscripción a Azure, cree una [cuenta gratuita](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) antes de empezar.
- Para poder completar los pasos de este tutorial, primero debe crear un [plan de protección de Azure DDoS Estándar](manage-ddos-protection.md).

## <a name="view-and-configure-ddos-attack-mitigation-reports"></a>Visualización y configuración de informes de mitigación de ataques DDoS

Los informes de mitigación de ataques usan los datos del protocolo Netflow que se agrega para proporcionar información detallada sobre el ataque en el recurso. Cada vez que un recurso IP público es objeto de ataque, tan pronto se inicia la mitigación se inicia la generación de informes. Durante todo el período de mitigación, habrá un informe incremental que se genera cada cinco minutos y un informe posterior a la mitigación. Su finalidad es garantizar que en caso de que el ataque DDoS continúe durante mucho tiempo, podrá ver la instantánea más actual del informe de mitigación cada cinco minutos y un resumen completo una vez finalizada la mitigación del ataque. 

1. Seleccione **Todos los servicios** en la parte superior izquierda del portal.
2. Escriba *Monitor* en el cuadro **Filtrar**. Seleccione **Monitor** cuando aparezca en los resultados.
3. En **CONFIGURACIÓN** , seleccione **Configuración de diagnóstico**.
4. Seleccione la **suscripción** y el **grupo de recursos** que contienen la dirección IP pública que desea registrar.
5. Seleccione la **dirección IP pública** para el **tipo de recurso** , luego seleccione la dirección IP pública específica para la que desea registrar las métricas.
6. Seleccione **Turn on diagnostics to collect the DDoSMitigationReports log** (Activar diagnósticos para recopilar el registro DDoSMitigationReports) y, luego, seleccione tantas opciones de las siguientes como necesite:

    - **Archivar en una cuenta de almacenamiento** : los datos se escriben en una cuenta de Azure Storage. Para más información sobre esta opción, consulte [Archivo de registros de recursos](../azure-monitor/platform/archive-diagnostic-logs.md?toc=%2fazure%2fvirtual-network%2ftoc.json).
    - **Transmisión a un centro de eventos** : permite que un receptor de registros seleccione los registros mediante una instancia de Azure Event Hub. Los centros de eventos habilitan la integración con Splunk y otros sistemas SIEM. Para más información sobre esta opción, consulte [Transmisión de registros de recursos a un centro de eventos](../azure-monitor/platform/resource-logs-stream-event-hubs.md?toc=%2fazure%2fvirtual-network%2ftoc.json).
    - **Enviar a Log Analytics** : Escribe registros en el servicio Azure Monitor. Para obtener más información sobre esta opción, consulte [Recopilación de registros para su uso en los registros de Azure Monitor](../azure-monitor/platform/collect-azure-metrics-logs.md?toc=%2fazure%2fvirtual-network%2ftoc.json).

Tanto los informes de mitigación incremental como los posteriores a los ataques incluyen los siguientes campos:
- Vectores de ataque
- Estadísticas de tráfico
- Motivo de paquetes descartados
- Protocolos implicados
- 10 países o regiones de origen principales
- 10 ASN de origen principales

## <a name="view-and-configure-ddos-attack-mitigation-flow-logs"></a>Visualización y configuración de registros de flujo de mitigación de ataques DDoS
Los registros de flujo de mitigación de ataques le permiten revisar casi en tiempo real el tráfico descartado, el tráfico reenviado y otros puntos de datos interesantes durante un ataque DDoS activo. Puede ingerir el flujo constante de estos datos en Azure Sentinel o en sus sistemas SIEM de terceros mediante un centro de eventos para la supervisión casi en tiempo real, realizar acciones y solucionar la necesidad de sus operaciones de defensa.

1. Seleccione **Todos los servicios** en la parte superior izquierda del portal.
2. Escriba *Monitor* en el cuadro **Filtrar**. Seleccione **Monitor** cuando aparezca en los resultados.
3. En **CONFIGURACIÓN** , seleccione **Configuración de diagnóstico**.
4. Seleccione la **suscripción** y el **grupo de recursos** que contienen la dirección IP pública que desea registrar.
5. Seleccione la **dirección IP pública** para el **tipo de recurso** , luego seleccione la dirección IP pública específica para la que desea registrar las métricas.
6. Seleccione **Turn on diagnostics to collect the DDoSMitigationFlowLogs log** (Activar diagnósticos para recopilar el registro DDoSMitigationFlowLogs) y, luego, seleccione tantas opciones de las siguientes como necesite:

    - **Archivar en una cuenta de almacenamiento** : los datos se escriben en una cuenta de Azure Storage. Para más información sobre esta opción, consulte [Archivo de registros de recursos](../azure-monitor/platform/archive-diagnostic-logs.md?toc=%2fazure%2fvirtual-network%2ftoc.json).
    - **Transmisión a un centro de eventos** : permite que un receptor de registros seleccione los registros mediante una instancia de Azure Event Hub. Los centros de eventos habilitan la integración con Splunk y otros sistemas SIEM. Para más información sobre esta opción, consulte [Transmisión de registros de recursos a un centro de eventos](../azure-monitor/platform/resource-logs-stream-event-hubs.md?toc=%2fazure%2fvirtual-network%2ftoc.json).
    - **Enviar a Log Analytics** : Escribe registros en el servicio Azure Monitor. Para obtener más información sobre esta opción, consulte [Recopilación de registros para su uso en los registros de Azure Monitor](../azure-monitor/platform/collect-azure-metrics-logs.md?toc=%2fazure%2fvirtual-network%2ftoc.json).

### <a name="azure-sentinel-data-connector"></a>Conector de datos de Azure Sentinel

Puede conectarse a Azure Sentinel, ver y analizar los datos de los libros, crear alertas personalizadas e incorporarlas en procesos de investigación. Para conectarse a Azure Sentinel, consulte [Conexión a Azure Sentinel](https://docs.microsoft.com/azure/sentinel/connect-azure-ddos-protection). 

![Conector de DDoS de Azure Sentinel](./media/ddos-attack-telemetry/azure-sentinel-ddos.png)

### <a name="azure-ddos-protection-workbook"></a>Libro de Azure DDoS Protection

Para ver los datos de registros de flujo en el panel de análisis de Azure, puede importar el panel de ejemplo desde https://github.com/Azure/Azure-Network-Security/tree/master/Azure%20DDoS%20Protection/Azure%20DDoS%20Protection%20Workbook.

Los registros de flujo tienen los siguientes campos: 
- IP de origen
- IP de destino
- Puerto de origen 
- Puerto de destino 
- Tipo de protocolo 
- Acción realizada durante la mitigación

![Libro de DDoS Protection](./media/ddos-attack-telemetry/ddos-attack-analytics-workbook.png)

El análisis de ataques solo funcionará si el estándar DDoS Protection está habilitado en la red virtual de la dirección IP pública. 

## <a name="sample-log-outputs"></a>Resultados del registro de ejemplo

Las siguientes capturas de pantallas muestran los resultados del registro de ejemplo:

### <a name="ddosmitigationflowlogs"></a>DDoSMitigationFlowLogs

![DDoS Protection DDoSMitigationFlowLogs](./media/ddos-attack-telemetry/ddos-mitigation-flow-logs.png)

### <a name="ddosprotectionnotifications"></a>DDoSProtectionNotifications

![DDoS Protection DDoSProtectionNotifications](./media/ddos-attack-telemetry/ddos-protection-notifications.png)

### <a name="ddosmitigationreports"></a>DDoSMitigationReports

![DDoS Protection DDoSMitigationReports](./media/ddos-attack-telemetry/ddos-mitigation-reports.png)

## <a name="next-steps"></a>Pasos siguientes

En este tutorial, ha aprendido a:

- Visualización y configuración de informes de mitigación de ataques DDoS
- Visualización y configuración de registros de flujo de mitigación de ataques DDoS

Para obtener información sobre cómo probar y simular un ataque DDoS, consulte la guía de pruebas de simulación:

> [!div class="nextstepaction"]
> [Pruebas mediante simulaciones](test-through-simulations.md)

