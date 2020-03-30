---
title: Integración de soluciones de seguridad en Azure Security Center | Microsoft Docs
description: Aprenda cómo Azure Security Center se integra con los asociados para mejorar la seguridad general de los recursos de Azure.
services: security-center
documentationcenter: na
author: memildin
manager: rkarlin
ms.assetid: 6af354da-f27a-467a-8b7e-6cbcf70fdbcb
ms.service: security-center
ms.topic: conceptual
ms.devlang: na
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 03/20/2019
ms.author: memildin
ms.openlocfilehash: 23a00c766dbb38853c57c91e7f59ec364390c44b
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/28/2020
ms.locfileid: "79225288"
---
# <a name="integrate-security-solutions-in-azure-security-center"></a>Integración de soluciones de seguridad en Azure Security Center
Este documento le ayuda a administrar las soluciones de seguridad que ya está conectadas a Azure Security Center y a agregar otras nuevas.

> [!NOTE]
> Un subconjunto de las soluciones de seguridad se retiró el 31 de julio de 2019. Para obtener más información y servicios alternativos, consulte [Retirada de las características de Security Center (julio de 2019)](security-center-features-retirement-july2019.md#menu_solutions).

## <a name="integrated-azure-security-solutions"></a>Soluciones de seguridad de Azure integradas
Security Center facilita la habilitación de soluciones de seguridad integradas en Azure. Dicha integración aporta las siguientes ventajas:

- **Implementación simplificada**: Security Center ofrece un aprovisionamiento optimizado de soluciones de asociados integradas. En el caso de soluciones antimalware y de evaluación de vulnerabilidades, Security Center puede aprovisionar el agente en las máquinas virtuales. En el caso de aplicaciones de firewall, Security Center puede ocuparse de gran parte de la configuración de red necesaria.
- **Detecciones integradas**: los eventos de seguridad de las soluciones de asociados se recopilan, agregan y muestran automáticamente como parte de las alertas e incidentes de Security Center. Estos eventos también se fusionan con las detecciones procedentes de otros orígenes para proporcionar funcionalidades avanzadas de detección de amenazas.
- **Administración y supervisión de del mantenimiento unificadas**: los clientes puede usar eventos de mantenimiento integrados para supervisar todas las soluciones de asociado de un vistazo. La administración básica está disponible con un acceso sencillo a la configuración avanzada mediante la solución de asociado.

Actualmente, las soluciones de seguridad integradas incluyen la evaluación de vulnerabilidades por [Qualys](https://www.qualys.com/public-cloud/#azure) y [Rapid7](https://www.rapid7.com/products/insightvm/) y el firewall de aplicaciones web de Microsoft Application Gateway.

> [!NOTE]
> Security Center no instala Microsoft Monitoring Agent en aplicaciones virtuales de asociados, porque la mayoría de los proveedores de seguridad prohíben que se ejecuten agentes externos en su aplicación.
>
>

## <a name="how-security-solutions-are-integrated"></a>Integración de soluciones de seguridad
Las soluciones de seguridad de Azure que se implementan desde Security Center se conectan automáticamente. También se pueden conectar otros orígenes de datos de seguridad, como equipos que se ejecutan en el entorno local o en otras nubes.

![Integración de soluciones de asociados](./media/security-center-partner-integration/security-center-partner-integration-fig8.png)

## <a name="manage-integrated-azure-security-solutions-and-other-data-sources"></a>Administración de soluciones de seguridad de Azure integradas y otros orígenes de datos

1. Inicie sesión en [Azure Portal](https://azure.microsoft.com/features/azure-portal/).

2. En el **menú de Microsoft Azure**, seleccione **Security Center**. Se abre **Security Center: Información general**.

3. En el menú de Security Center, seleccione **Soluciones de seguridad**.

   ![Introducción a Security Center](./media/security-center-partner-integration/overview.png)

En **Soluciones de seguridad**, puede ver el mantenimiento de las soluciones de seguridad integradas de Azure y ejecutar tareas de administración básicas.

### <a name="connected-solutions"></a>Soluciones conectadas

La sección **Soluciones conectadas** incluye soluciones de seguridad que están conectadas actualmente a Security Center. También muestra el estado de mantenimiento de cada solución.  

![Soluciones conectadas](./media/security-center-partner-integration/security-center-partner-integration-fig4.png)

El estado de una solución de asociado puede ser:

* Correcto (verde): no hay problemas de mantenimiento.
* Incorrecto (rojo): hay una incidencia de mantenimiento que requiere atención inmediata.
* Problemas de mantenimiento (naranja): la solución ha dejado de informar sobre su estado.
* No comunicado (gris): la solución aún no ha notificado nada y no hay datos de mantenimiento disponibles. Es posible que el estado de una solución no se notifique si se conectó recientemente y todavía se está implementando.

> [!NOTE]
> Si los datos sobre el estado de mantenimiento no están disponibles, Security Center muestra la fecha y hora del último evento recibido para indicar si la solución está enviando notificaciones o no. Si no hay disponible ningún dato de mantenimiento y no se han recibido alertas en los últimos 14 días, Security Center indica que la solución es incorrecta o que no notifica nada.
>
>

1. Seleccione **Ver** para más información y opciones adicionales, entre las que se incluyen:

   - **Consola de solución**. Se abre la experiencia de administración de esta solución.
   - **Vincular VM**. Se abre la página Vincular aplicaciones. Aquí puede conectar recursos a la solución de asociados.
   - **Eliminar solución**.
   - **Configurar**.

   ![Detalle de solución de asociado](./media/security-center-partner-solutions/partner-solutions-detail.png)

### <a name="discovered-solutions"></a>Soluciones detectadas

Security Center detecta automáticamente las soluciones de seguridad que se ejecutan en Azure, pero que no están conectadas a Security Center y las muestra en la sección **Soluciones detectadas**. Estas soluciones incluyen las de Azure, como [Azure AD Identity Protection](https://docs.microsoft.com/azure/active-directory/active-directory-identityprotection) y soluciones de asociados.

> [!NOTE]
> El nivel estándar de Security Center es necesario en el nivel de suscripción de la característica de soluciones detectadas. Para más información sobre los planes de tarifa, consulte [Precios](security-center-pricing.md).
>
>

Seleccione **Conectar** en una solución para integrarla con Security Center y recibir alertas de seguridad.

### <a name="add-data-sources"></a>Agregar orígenes de datos

La sección **Agregar orígenes de datos** incluye otros orígenes de datos disponibles que se pueden conectar. Para obtener instrucciones acerca de cómo agregar datos desde cualquiera de estos orígenes, haga clic en **ADD**.

![Orígenes de datos](./media/security-center-partner-integration/security-center-partner-integration-fig7.png)

## <a name="exporting-data-to-a-siem"></a>Exportación de datos a un SIEM

> [!NOTE]
> A fin de obtener más información sobre un método más sencillo (actualmente en versión preliminar) para la exportación de datos a un SIEM, vea [Exportación de alertas y recomendaciones de seguridad (versión preliminar)](continuous-export.md). El método nuevo no usa el registro de actividad como intermediario y permite la exportación directa desde Security Center a Event Hubs (y luego al SIEM). También admite la exportación de recomendaciones de seguridad.


Puede configurar SIEM u otras herramientas de supervisión para recibir eventos de Azure Security Center.

Todos los eventos de Azure Security Center se publican en el [registro de actividad](../monitoring-and-diagnostics/monitoring-overview-activity-logs.md) de Azure Monitor. Azure Monitor usa una [canalización consolidada](../azure-monitor/platform/stream-monitoring-data-event-hubs.md) para transmitir los datos a un centro de eventos, donde se pueden extraer en la herramienta de supervisión.

En las secciones siguientes se describe cómo configurar los datos para que se transmitan en secuencias a un centro de eventos. En estos pasos se supone que ya tiene Azure Security Center configurado en su suscripción a Azure.

### <a name="high-level-overview"></a>Información general de alto nivel

![Información general de alto nivel](media/security-center-export-data-to-siem/overview.png)

### <a name="what-is-the-azure-security-data-exposed-to-siem"></a>¿Qué datos de seguridad de Azure se exponen a SIEM?

En esta versión, se exponen las [alertas de seguridad.](../security-center/security-center-managing-and-responding-alerts.md) En las próximas versiones, ampliaremos el conjunto de datos con recomendaciones de seguridad.

### <a name="how-to-set-up-the-pipeline"></a>Configuración de la canalización

#### <a name="create-an-event-hub"></a>Creación de un Centro de eventos

Antes de comenzar, [cree un espacio de nombres de Event Hubs](../event-hubs/event-hubs-create.md): el destino de todos los datos de supervisión.

#### <a name="stream-the-azure-activity-log-to-event-hubs"></a>Transmisión del registro de actividad de Azure a Event Hubs

Consulte el artículo [Transmisión del registro de actividad a Event Hubs](../azure-monitor/platform/activity-logs-stream-event-hubs.md).

#### <a name="install-a-partner-siem-connector"></a>Instalación de un conector SIEM de asociado 

El enrutamiento de los datos de supervisión a un centro de eventos con Azure Monitor facilita la integración con las herramientas de asociados de SIEM y de supervisión.

Consulte el siguiente artículo para conocer la lista de [SIEM admitidos](../azure-monitor/platform/stream-monitoring-data-event-hubs.md#partner-tools-with-azure-monitor-integration).

### <a name="example-for-querying-data"></a>Ejemplo de consulta de datos 

Estas son algunas de las consultas de Splunk que puede usar para extraer datos de alertas:

| **Descripción de la consulta** | **Consultar** |
|----|----|
| Todas las alertas| index=main Microsoft.Security/locations/alerts|
| Resumen del recuento de operaciones por su nombre| index=main sourcetype="amal:security" \| table operationName \| stats count by operationName|
| Obtención de información de las alertas: hora, nombre, estado, identificador y suscripción | index=main Microsoft.Security/locations/alerts \| table \_time, properties.eventName, State, properties.operationId, am_subscriptionId |


## <a name="next-steps"></a>Pasos siguientes

En este documento, ha aprendido a integrar soluciones de asociados en Security Center. Para más información sobre Security Center, consulte el siguiente artículo:

* [Supervisión del estado de seguridad en Security Center](security-center-monitoring.md). Aprenda a supervisar el estado de los recursos de Azure.