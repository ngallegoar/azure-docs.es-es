---
title: Supervisión de las API publicadas en Azure API Management | Microsoft Docs
description: Siga los pasos de este tutorial para aprender a supervisar su API en Azure API Management.
services: api-management
author: vladvino
manager: cfowler
ms.service: api-management
ms.workload: mobile
ms.custom: mvc
ms.topic: tutorial
ms.date: 06/15/2018
ms.author: apimpm
ms.openlocfilehash: 989608b9a087599ab73864ae2605fbffcf3221d9
ms.sourcegitcommit: 55b2bbbd47809b98c50709256885998af8b7d0c5
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 06/18/2020
ms.locfileid: "84982057"
---
# <a name="monitor-published-apis"></a>Supervisión de las API publicadas

Con Azure Monitor, puede visualizar, consultar, enrutar, archivar y realizar acciones en las métricas o los registros procedentes de los recursos de Azure.

En este tutorial, aprenderá a:

> [!div class="checklist"]
> * Visualización de registros de actividad
> * Visualización de registros de recursos
> * Visualización de las métricas de las API 
> * Configurar una regla de alerta cuando la API recibe llamadas no autorizadas

En el vídeo siguiente se muestra cómo supervisar API Management con Azure Monitor. 

> [!VIDEO https://channel9.msdn.com/Blogs/AzureApiMgmt/Monitor-API-Management-with-Azure-Monitor/player]

## <a name="prerequisites"></a>Requisitos previos

+ Conocer la [terminología de API Management de Azure](api-management-terminology.md).
+ Complete el siguiente inicio rápido: [Creación de una instancia de Azure API Management](get-started-create-service-instance.md).
+ Además, realice el siguiente tutorial: [Importación y publicación de la primera API](import-and-publish.md).

[!INCLUDE [premium-dev-standard-basic.md](../../includes/api-management-availability-premium-dev-standard-basic.md)]

## <a name="view-metrics-of-your-apis"></a>Visualización de las métricas de las API

API Management emite métricas cada minuto, lo que le ofrece visibilidad casi en tiempo real sobre el estado y el mantenimiento de las API. A continuación se muestran las dos métricas que se usan con más frecuencia. Para obtener una lista de todas las métricas disponibles, consulte las [métricas compatibles](https://docs.microsoft.com/azure/azure-monitor/platform/metrics-supported#microsoftapimanagementservice).

* Capacity (Capacidad): ayuda a tomar decisiones acerca de cómo actualizar o degradar los servicios de APIM. La métrica se emite por minuto y refleja la capacidad de la puerta de enlace en el momento del informe. La métrica oscila entre 0 y 100, y se calcula en función de los recursos de la puerta de enlace, como el uso de la CPU y de la memoria.
* Requests (Solicitudes): ayuda a analizar el tráfico de API que pasa por los servicios de APIM. La métrica se emite por minuto y notifica el número de solicitudes de puerta de enlace con dimensiones que incluyen los códigos de respuesta, la ubicación, el nombre de host y los errores. 

> [!IMPORTANT]
> Las siguientes métricas han quedado en desuso a partir de mayo de 2019 y se retirarán en agosto de 2023: Total Gateway Requests (Solicitudes totales de puerta de enlace), Successful Gateway Requests (Solicitudes de puerta de enlace correctas), Unauthorized Gateway Requests (Solicitudes de puerta de enlace no autorizadas), Failed Gateway Requests (Solicitudes de puerta de enlace con error), Other Gateway Requests (Otras solicitudes de puerta de enlace). Migre a la métrica Requests (Solicitudes), que proporciona una funcionalidad equivalente.

![gráfico de métricas](./media/api-management-azure-monitor/apim-monitor-metrics.png)

Para acceder a la métrica:

1. Seleccione **Métricas** en el menú situado cerca de la parte inferior de la página.

    ![Métricas](./media/api-management-azure-monitor/api-management-metrics-blade.png)

2. En la lista desplegable, seleccione las métricas que le interesen. Por ejemplo, **Requests**. 
3. El gráfico muestra el número total de llamadas API.
4. El gráfico se puede filtrar mediante las dimensiones de la métrica de **Solicitudes**. Por ejemplo, haga clic en **Agregar filtro**, elija **Backend Response Code** (Código de respuesta de back-end), escriba 500 como valor. Ahora el gráfico muestra el número de solicitudes erróneas en el back-end de la API.   

## <a name="set-up-an-alert-rule-for-unauthorized-request"></a>Configuración de una regla de alerta para solicitudes no autorizadas

Puede configurar la recepción de alertas en función de métricas y registros de actividad. Azure Monitor permite configurar una alerta que haga lo siguiente cuando se desencadena:

* Enviar una notificación por correo electrónico
* Llamar a un webhook
* Invocar una aplicación lógica de Azure

Para configurar alertas:

1. Seleccione **Alertas** en la barra de menús cerca de la parte inferior de la página.

    ![alerts](./media/api-management-azure-monitor/alert-menu-item.png)

2. Haga clic en una **Nueva regla de alerta** para esta alerta.
3. Haga clic en **Agregar condición**.
4. Seleccione **Métricas** en el cuadro desplegable del tipo de señal.
5. Seleccione **Unauthorized Gateway Requests** (Solicitudes de puerta de enlace no autorizadas) como la señal que se desea supervisar.

    ![alerts](./media/api-management-azure-monitor/signal-type.png)

6. En la vista **Configurar lógica de señal**, especifique un umbral después del cual debe activarse la alerta y haga clic en **Hecho**.

    ![alerts](./media/api-management-azure-monitor/threshold.png)

7. Seleccione un grupo de acciones existente o cree uno nuevo. En el ejemplo siguiente, se enviará un correo electrónico a los administradores. 

    ![alerts](./media/api-management-azure-monitor/action-details.png)

8. Escriba un nombre y la descripción para la regla de alertas y elija el nivel de gravedad. 
9. Presione **Crear regla de alertas**.
10. Ahora, intente llamar a Conference API sin una clave de API. La alerta se desencadenará y se enviará un correo electrónico a los administradores. 

## <a name="activity-logs"></a>Registros de actividad

Los registros de actividad proporcionan información sobre las operaciones llevadas a cabo en los servicios API Management. Con los registros de actividades, puede determinar los interrogantes “qué, quién y cuándo” de las operaciones de escritura (PUT, POST, DELETE) llevadas a cabo en los servicios API Management.

> [!NOTE]
> Los registros de actividad no incluyen las operaciones de lectura (GET) ni las realizadas en Azure Portal o mediante las API de administración originales.

Puede acceder a registros de actividad en el servicio API Management o a los registros de todos los recursos de Azure en Azure Monitor. 

![registros de actividad](./media/api-management-azure-monitor/apim-monitor-activity-logs.png)

Para ver los registros de actividad:

1. Seleccione la instancia del servicio APIM.
2. Haga clic en **Registro de actividad**.

    ![registro de actividad](./media/api-management-azure-monitor/api-management-activity-logs-blade.png)

3. Seleccione el ámbito de filtrado que desee y haga clic en **Aplicar**.

## <a name="resource-logs"></a>Registros de recurso

Los registros de recurso proporcionan información valiosa acerca de las operaciones y los errores que son importantes para la auditoría, así como para solucionar problemas. Los registros de recurso son diferentes de los registros de actividad. Los registros de actividad proporcionan información detallada sobre las operaciones llevadas a cabo en los recursos de Azure. Los registros de recurso proporcionan información detallada sobre las operaciones que realiza el recurso.

Para configurar los registros de recurso:

1. Seleccione la instancia del servicio APIM.
2. Haga clic en **Configuración de diagnóstico**.

    ![registros de recurso](./media/api-management-azure-monitor/api-management-diagnostic-logs-blade.png)

3. Haga clic en **Activar diagnóstico**. Los registros de recurso se pueden archivar junto con las métricas en una cuenta de almacenamiento, transmitirlos en secuencias a un centro de eventos o enviarlos a los registros de Azure Monitor. 

Actualmente, API Management proporciona registros de recurso (de los que se crean lotes cada hora) de una solicitud de API individual con cada entrada con el esquema siguiente:

```json
{  
    "isRequestSuccess" : "",
    "time": "",
    "operationName": "",
    "category": "",
    "durationMs": ,
    "callerIpAddress": "",
    "correlationId": "",
    "location": "",
    "httpStatusCodeCategory": "",
    "resourceId": "",
    "properties": {   
        "method": "", 
        "url": "", 
        "clientProtocol": "", 
        "responseCode": , 
        "backendMethod": "", 
        "backendUrl": "", 
        "backendResponseCode": ,
        "backendProtocol": "",  
        "requestSize": , 
        "responseSize": , 
        "cache": "", 
        "cacheTime": "", 
        "backendTime": , 
        "clientTime": , 
        "apiId": "",
        "operationId": "", 
        "productId": "", 
        "userId": "", 
        "apimSubscriptionId": "", 
        "backendId": "",
        "lastError": { 
            "elapsed" : "", 
            "source" : "", 
            "scope" : "", 
            "section" : "" ,
            "reason" : "", 
            "message" : ""
        } 
    }      
}  
```

| Propiedad  | Tipo | Descripción |
| ------------- | ------------- | ------------- |
| isRequestSuccess | boolean | True si la solicitud HTTP se completó con el código de estado de respuesta dentro del intervalo 2xx o 3xx |
| time | date-time | Marca de tiempo del momento en que la puerta de enlace comienza a procesar la solicitud |
| operationName | string | Valor constante "Microsoft.ApiManagement/GatewayLogs" |
| category | string | Valor constante "GatewayLogs" |
| durationMs | integer | Número de milisegundos transcurridos desde el momento en que la puerta de enlace recibió la solicitud hasta que se envió toda la respuesta. Incluye clienTime, cacheTime y backendTime. |
| callerIpAddress | string | Dirección IP del llamador inmediato de la puerta de enlace (puede ser un intermediario) |
| correlationId | string | Identificador único de la solicitud HTTP asignado por API Management |
| ubicación | string | Nombre de la región de Azure donde se encontraba la puerta de enlace que procesó la solicitud |
| httpStatusCodeCategory | string | Categoría del código de estado de respuesta HTTP: Correcto (301 o menos, 304 o 307), No autorizado (401, 403, 429), Erróneo (400, entre 500 y 600), Otro |
| resourceId | string | Identificador del recurso de API Management /SUBSCRIPTIONS/\<subscription>/RESOURCEGROUPS/\<resource-group>/PROVIDERS/MICROSOFT.APIMANAGEMENT/SERVICE/\<name> |
| properties | object | Propiedades de la solicitud actual |
| method | string | Método HTTP de la solicitud entrante |
| url | string | Dirección URL de la solicitud entrante |
| clientProtocol | string | Versión del protocolo HTTP de la solicitud entrante |
| responseCode | integer | Código de estado de la respuesta HTTP enviada a un cliente |
| backendMethod | string | Método HTTP de la solicitud enviada a un back-end |
| backendUrl | string | Dirección URL de la solicitud enviada a un back-end |
| backendResponseCode | integer | Código de la respuesta HTTP recibida de un back-end |
| backendProtocol | string | Versión del protocolo HTTP de la solicitud enviada a un back-end | 
| requestSize | integer | Número de bytes recibidos de un cliente durante el procesamiento de solicitudes | 
| responseSize | integer | Número de bytes enviados a un cliente durante el procesamiento de solicitudes | 
| caché | string | Estado de participación de la caché de API Management en el procesamiento de la solicitud (es decir, acierto, error, ninguno) | 
| cacheTime | integer | Número de milisegundos empleados en la E/S de la caché global de API Management (conexión, envío y recepción de bytes) | 
| backendTime | integer | Número de milisegundos empleados en la E/S global de back-end (conexión, envío y recepción de bytes) | 
| clientTime | integer | Número de milisegundos empleados en la E/S global del cliente (conexión, envío y recepción de bytes) | 
| apiId | string | Identificador de la entidad de la API de la solicitud actual | 
| operationId | string | Identificador de la entidad de la operación de la solicitud actual | 
| productId | string | Identificador de la entidad del producto de la solicitud actual | 
| userId | string | Identificador de la entidad del usuario de la solicitud actual | 
| apimSubscriptionId | string | Identificador de la entidad de la suscripción de la solicitud actual | 
| backendId | string | Identificador de la entidad del back-end de la solicitud actual | 
| lastError | object | Último error de procesamiento de la solicitud | 
| elapsed | integer | Número de milisegundos transcurridos desde que la puerta de enlace recibió la solicitud hasta el momento en que se produjo el error | 
| source | string | Nombre del controlador interno del procesamiento o de la directiva que produjo el error | 
| scope | string | Ámbito del documento de directiva que contiene la directiva que produjo el error | 
| section | string | Sección del documento de directiva que contiene la directiva que produjo el error | 
| reason | string | Motivo del error | 
| message | string | Mensaje de error | 

## <a name="next-steps"></a>Pasos siguientes

En este tutorial, ha aprendido a:

> [!div class="checklist"]
> * Visualización de registros de actividad
> * Visualización de registros de recursos
> * Visualización de las métricas de las API
> * Configurar una regla de alerta cuando la API recibe llamadas no autorizadas

Avance hasta el siguiente tutorial:

> [!div class="nextstepaction"]
> [Seguimiento de llamadas](api-management-howto-api-inspector.md)
