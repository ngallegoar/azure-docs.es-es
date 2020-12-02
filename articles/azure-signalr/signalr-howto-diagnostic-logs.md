---
title: Registros de recursos para Azure SignalR Service
description: Aprenda a configurar registros de recursos para Azure SignalR Service y a usarlos para solucionar problemas por sí mismo.
author: wanlwanl
ms.service: signalr
ms.topic: conceptual
ms.date: 12/17/2019
ms.author: wanl
ms.openlocfilehash: 5ad40ca051677ced0c6d8b5c35e8563272ff598f
ms.sourcegitcommit: d22a86a1329be8fd1913ce4d1bfbd2a125b2bcae
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 11/26/2020
ms.locfileid: "96183981"
---
# <a name="resource-logs-for-azure-signalr-service"></a>Registros de recursos para Azure SignalR Service

En este tutorial se explica qué son los registros de recursos de Azure SignalR Service, cómo se configuran y cómo se solucionan sus problemas. 

## <a name="prerequisites"></a>Requisitos previos
Para habilitar los registros de recursos, necesitará un lugar donde almacenar los datos de registro. En este tutorial se utiliza Azure Storage y Log Analytics.

* [Azure Storage](../azure-monitor/platform/resource-logs.md#send-to-azure-storage): conserva los registros de recursos para auditorías de directivas, análisis estáticos o copias de seguridad.
* [Log Analytics](../azure-monitor/platform/resource-logs.md#send-to-log-analytics-workspace): una herramienta flexible de búsqueda y de análisis de registros que permite el análisis de los registros sin procesar que genera un recurso de Azure.

## <a name="set-up-resource-logs-for-an-azure-signalr-service"></a>Configuración de registros de recursos para Azure SignalR Service

Puede ver registros de recursos para Azure SignalR Service. Estos registros proporcionan una vista más completa de la conectividad con la instancia de Azure SignalR Service. Los registros de recursos proporcionan información detallada de cada conexión. Por ejemplo, información básica (identificador de usuario, identificador de conexión y tipo de transporte, etc.) e información de eventos (evento Connect, Disconnect, Abort, etc.) de la conexión. Los registros de recursos se pueden usar para la identificación de problemas, el seguimiento de la conexión y el análisis.

### <a name="enable-resource-logs"></a>Habilitación de registros de recursos

Los registros de recursos están inhabilitados de forma predeterminada. Para habilitar los registros de recursos, siga estos pasos:

1. En [Azure Portal](https://portal.azure.com), en **Supervisión**, haga clic en **Configuración de diagnóstico**.

    ![Navegación por el panel a la configuración de diagnóstico](./media/signalr-tutorial-diagnostic-logs/diagnostic-settings-menu-item.png)

1. Haga clic en **Agregar configuración de diagnóstico**.

    ![Agregar registros de recursos](./media/signalr-tutorial-diagnostic-logs/add-diagnostic-setting.png)

1. Establezca el destino de archivo que desee. Actualmente, se admiten **Archivar en una cuenta de almacenamiento** y **Enviar a Log Analytics**.

1. Seleccione los registros que desea archivar.

    ![Panel Configuración de diagnóstico](./media/signalr-tutorial-diagnostic-logs/diagnostics-settings-pane.png)


1. Guarde la nueva configuración de diagnóstico.

La nueva configuración surte efecto en unos 10 minutos. Después, los registros aparecen en el destino de archivo configurado, en el panel **Registros de diagnóstico**.

Para obtener más información sobre el diagnóstico de configuración, consulte la [información general sobre los registros de recursos de Azure](../azure-monitor/platform/platform-logs-overview.md).

### <a name="resource-logs-categories"></a>Categorías de registros de recursos

Azure SignalR Service captura los registros de recursos en una sola categoría:

* **Todos los registros**: realice un seguimiento de las conexiones que se conectan a Azure SignalR Service. Los registros proporcionan información sobre conexión y desconexión, autenticación y limitación. Para obtener más información, vea la próxima sección.

### <a name="archive-to-a-storage-account"></a>Archivar en una cuenta de almacenamiento

Los registros se almacenan en la cuenta de almacenamiento configurada en el panel **Registros de diagnóstico**. Se crea automáticamente un contenedor denominado `insights-logs-alllogs` para almacenar los registros de recursos. Dentro del contenedor, los registros se almacenan en el archivo `resourceId=/SUBSCRIPTIONS/XXXXXXXX-XXXX-XXXX-XXXX-XXXXXXXXXXXX/RESOURCEGROUPS/XXXX/PROVIDERS/MICROSOFT.SIGNALRSERVICE/SIGNALR/XXX/y=YYYY/m=MM/d=DD/h=HH/m=00/PT1H.json`. Básicamente, la ruta de acceso se combina mediante `resource ID` y `Date Time`. Los archivos de registro se dividen por `hour`. Por lo tanto, los minutos siempre son `m=00`.

Todos los registros se almacenan en el formato de notación de objetos JavaScript (JSON). Cada entrada tiene campos de cadena que usan el formato descrito en las secciones siguientes.

Las cadenas JSON de registros de archivo incluyen elementos enumerados en las tablas siguientes:

**Formato**

Nombre | Descripción
------- | -------
time | Hora del evento de registro
level | Nivel del evento de registro
resourceId | Identificador de recurso de Azure SignalR Service
ubicación | Ubicación de Azure SignalR Service
category | Categoría del evento de registro
operationName | Nombre de operación del evento
callerIpAddress | Dirección IP del servidor o cliente
properties | Propiedades detalladas relacionadas con este evento de registro. Para más información, consulte la siguiente tabla de propiedades.

**Propiedades de tabla**

Nombre | Descripción
------- | -------
type | Tipo del evento de registro. Actualmente, se proporciona información sobre la conectividad con Azure SignalR Service. Solo está disponible el tipo `ConnectivityLogs`.
collection | Colección del evento de registro. Los valores permitidos son: `Connection`, `Authorization` y `Throttling`
connectionId | Identidad de la conexión
transportType | Tipo de transporte de la conexión. Los valores permitidos son: `Websockets` \| `ServerSentEvents` \| `LongPolling`
connectionType | Tipo de la conexión. Los valores permitidos son: `Server` \| `Client` `Server`: conexión desde el lado servidor; `Client`: conexión desde el lado cliente.
userId | Identidad del usuario
message | Mensaje detallado del evento de registro

El código siguiente es un ejemplo de una cadena JSON de registro de archivo:

```json
{
    "properties": {
        "message": "Entered Serverless mode.",
        "type": "ConnectivityLogs",
        "collection": "Connection",
        "connectionId": "xxxxxxxxxxxxxxxxxxxxxxxxxxxxxxx",
        "userId": "User",
        "transportType": "WebSockets",
        "connectionType": "Client"
    },
    "operationName": "ServerlessModeEntered",
    "category": "AllLogs",
    "level": "Informational",
    "callerIpAddress": "xxx.xxx.xxx.xxx",
    "time": "2019-01-01T00:00:00Z",
    "resourceId": "/SUBSCRIPTIONS/XXXXXXXX-XXXX-XXXX-XXXX-XXXXXXXXXXXX/RESOURCEGROUPS/XXXX/PROVIDERS/MICROSOFT.SIGNALRSERVICE/SIGNALR/XXX",
    "location": "xxxx"
}
```

### <a name="archive-logs-schema-for-log-analytics"></a>Esquema de registros de archivo para Log Analytics

Para ver los registros de recursos, siga estos pasos:

1. Haga clic en `Logs` en la instancia de Log Analytics de destino.

    ![Elemento de menú de Log Analytics](./media/signalr-tutorial-diagnostic-logs/log-analytics-menu-item.png)

2. Escriba `SignalRServiceDiagnosticLogs` y seleccione el intervalo de tiempo para consultar los registros de recursos. Para una consulta avanzada, consulte [Introducción a los análisis de registros de Azure Monitor](../azure-monitor/log-query/log-analytics-tutorial.md).

    ![Registro de consultas en Log Analytics](./media/signalr-tutorial-diagnostic-logs/query-log-in-log-analytics.png)

Las columnas de registros de archivo incluyen elementos enumerados en la tabla siguiente:

Nombre | Descripción
------- | ------- 
TimeGenerated | Hora del evento de registro
Colección | Colección del evento de registro. Los valores permitidos son: `Connection`, `Authorization` y `Throttling`
OperationName | Nombre de operación del evento
Location | Ubicación de Azure SignalR Service
Nivel | Nivel del evento de registro
CallerIpAddress | Dirección IP del servidor o cliente
Message | Mensaje detallado del evento de registro
UserId | Identidad del usuario
ConnectionId | Identidad de la conexión
ConnectionType | Tipo de la conexión. Los valores permitidos son: `Server` \| `Client` `Server`: conexión desde el lado servidor; `Client`: conexión desde el lado cliente.
TransportType | Tipo de transporte de la conexión. Los valores permitidos son: `Websockets` \| `ServerSentEvents` \| `LongPolling`

### <a name="troubleshooting-with-resource-logs"></a>Solución de problemas con registros de recursos

Para solucionar problemas de Azure SignalR Service, puede habilitar los registros del lado cliente o servidor a fin de capturar errores. En la actualidad, Azure SignalR Service expone registros de recursos. También puede habilitar registros para el lado de servicio.

Cuando se encuentra en una situación de crecimiento o interrupción inesperada de la conexión, puede aprovechar los registros de recursos para solucionar problemas.

Los problemas típicos suelen ser cambios inesperados en el número de conexiones, conexiones que llegan a los límites de conexión y errores de autorización. Consulte las secciones siguientes sobre el modo de solucionar problemas.

#### <a name="unexpected-connection-number-changes"></a>Cambios inesperados en el número de conexiones

##### <a name="unexpected-connection-dropping"></a>Interrupción inesperada de la conexión

Si se produce una interrupción inesperada de la conexión, debe habilitar los registros en los lados servicio, servidor y cliente.

Si se desconecta una conexión, los registros de recursos registrarán este evento de desconexión; verá `ConnectionAborted` o `ConnectionEnded` en `operationName`.

La diferencia entre `ConnectionAborted` y `ConnectionEnded` es que `ConnectionEnded` es una desconexión esperada desencadenada por el lado cliente o servidor. Aunque `ConnectionAborted` suele ser un evento de interrupción inesperada de la conexión, se proporcionará el motivo de la anulación en `message`.

Las opciones de la anulación se muestran en la siguiente tabla:

Motivo | Descripción
------- | ------- 
El número de conexiones llega al límite | El número de conexiones llega al límite del nivel de precios actual. Considere la posibilidad de escalar la unidad de servicio.
La aplicación ha cerrado la conexión | El servidor de aplicaciones desencadena la anulación. Se puede considerar como una anulación esperada
Tiempo de expiración del ping de conexión | Normalmente se debe a un problema de red. Considere la posibilidad de comprobar la disponibilidad del servidor de aplicaciones desde Internet.
Recarga de servicios, reconexión | Azure SignalR Service se está cargando de nuevo. Azure SignalR admite la reconexión automática, por lo que puede esperar a que se vuelva a conectar o puede conectarlo de nuevo manualmente a Azure SignalR Service.
Error transitorio del servidor interno | Se produce un error transitorio en Azure SignalR Service, se debería recuperar automáticamente.
Conexión del servidor interrumpida | Interrupciones de conexión del servidor con error desconocido, considere, en primer lugar, la posibilidad de solucionar el problema con el registro del lado servicio, servidor o cliente. Intente excluir los problemas básicos (por ejemplo, problemas de red, problemas del servidor de aplicaciones, etc.). Si el problema no se soluciona, póngase en contacto con nosotros para obtener más ayuda. Para más información, consulte [Obtenga ayuda](#get-help). 

##### <a name="unexpected-connection-growing"></a>Crecimiento de conexiones inesperado

Para solucionar los problemas relacionados con un crecimiento de conexiones inesperado, lo primero que debe hacer es filtrar las conexiones adicionales. Puede agregar un identificador único de usuario de prueba a la conexión del cliente de prueba. A continuación, compruébelo con los registros de recursos; si ve que hay más de una conexión de cliente con el mismo identificador de usuario o IP de prueba, es probable que el lado de cliente cree y establezca más conexiones de las esperadas. Compruebe el lado cliente.

#### <a name="authorization-failure"></a>Error de autorización

Si recibe un mensaje 401 No autorizado para las solicitudes de cliente, compruebe los registros de recursos. Si encuentra `Failed to validate audience. Expected Audiences: <valid audience>. Actual Audiences: <actual audience>`, significa que no hay ninguna audiencia del token de acceso válida. Intente usar las audiencias válidas sugeridas en el registro.


#### <a name="throttling"></a>Limitaciones

Si no puede establecer conexiones de cliente de SignalR con Azure SignalR Service, compruebe los registros de recursos. Si encuentra `Connection count reaches limit` en el registro de recursos, establece demasiadas conexiones con SignalR Service, lo que provoca que se alcance el límite del número de conexiones. Considere la posibilidad de escalar SignalR Service. Si encuentra `Message count reaches limit` en el registro de recursos, significa que utiliza el nivel gratuito y ha llegado al máximo de la cuota de mensajes. Si desea enviar más mensajes, considere la posibilidad de cambiar SignalR Service al nivel estándar para enviar mensajes adicionales. Para más información, consulte [Precios de Azure SignalR Service](https://azure.microsoft.com/pricing/details/signalr-service/).

### <a name="get-help"></a>Obtener ayuda

Se recomienda que solucione primero los problemas por sí mismo. La mayoría de los problemas se deben a problemas de red o del servidor de aplicaciones. Siga la [guía de solución de problemas con el registro de recursos](#troubleshooting-with-resource-logs) y la [guía básica de solución de problemas](https://github.com/Azure/azure-signalr/blob/dev/docs/tsg.md) para detectar la causa principal.
Si el problema sigue sin resolverse, considere la posibilidad de abrir una incidencia en GitHub o crear un vale en Azure Portal.
Proporcione:
1. Intervalo de tiempo de 30 minutos en el que se produce el problema
2. Identificador de recurso de Azure SignalR Service
3. Detalles del problema, de la forma más específica posible: Por ejemplo, AppServer no envía mensajes, interrupciones de las conexiones del cliente, etc.
4. Registros recopilados del lado servidor o cliente y otros materiales que pueden ser útiles
5. [Opcional] Código de reproducción

> [!NOTE]
> Si abre la incidencia en GitHub, mantenga la información confidencial (por ejemplo, el identificador de recurso y los registros de servidor o cliente) privada y envíesela solo a los miembros de la organización de Microsoft de forma privada.