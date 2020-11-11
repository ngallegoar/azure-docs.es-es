---
title: Integración de Azure API Management con Azure Application Insights
titleSuffix: Azure API Management
description: Obtenga información acerca de cómo registrar y ver los eventos de Azure API Management en Azure Application Insights.
services: api-management
documentationcenter: ''
author: mikebudzynski
manager: erikre
editor: ''
ms.service: api-management
ms.workload: mobile
ms.tgt_pltfrm: na
ms.topic: article
ms.date: 06/20/2018
ms.author: apimpm
ms.openlocfilehash: adb221c12af436135b1e740fdef7c5c0a0a7f0cb
ms.sourcegitcommit: 3bdeb546890a740384a8ef383cf915e84bd7e91e
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/30/2020
ms.locfileid: "93096043"
---
# <a name="how-to-integrate-azure-api-management-with-azure-application-insights"></a>Cómo integrar Azure API Management con Azure Application Insights

Azure API Management le permite una integración sencilla con Azure Application Insights, un servicio extensible para desarrolladores web que compilan y administran aplicaciones en varias plataformas. En esta guía, verá todos los pasos para dicha integración; además, se describen las estrategias para reducir el impacto en el rendimiento en la instancia de servicio de API Management.

## <a name="prerequisites"></a>Prerrequisitos

Para seguir esta guía, debe tener una instancia de Azure API Management. Si no tiene una, complete antes el [tutorial](get-started-create-service-instance.md).

## <a name="create-an-azure-application-insights-instance"></a>Creación de una instancia de Azure Application Insights

Antes de poder usar Azure Application Insights, debe crear una instancia del servicio.

1. Abra **Azure Portal** y vaya a **Application Insights**.  
    ![Captura de pantalla en la que se muestra cómo navegar a Application Insights](media/api-management-howto-app-insights/apim-app-insights-instance-1.png).  
2. Haga clic en **+ Agregar**.  
    ![Creación de App Insights](media/api-management-howto-app-insights/apim-app-insights-instance-2.png)  
3. Rellene el formulario. Seleccione **General** como **Tipo de aplicación**.
4. Haga clic en **Crear**.

## <a name="create-a-connection-between-azure-application-insights-and-azure-api-management-service-instance"></a>Creación de una conexión entreAzure Application Insights y la instancia de servicio de Azure API Management

1. Vaya a la **instancia del servicio Azure API Management** en **Azure Portal**.
2. Seleccione **Application Insights** en el menú de la izquierda.
3. Haga clic en **+ Agregar**.  
    ![Captura de pantalla en la que se muestra dónde agregar una conexión nueva](media/api-management-howto-app-insights/apim-app-insights-logger-1.png).  
4. Seleccione la instancia de **Application Insights** creada anteriormente y escriba una descripción breve.
5. Haga clic en **Crear**.
6. Acaba de crear un registrador de Azure Application Insights con una clave de instrumentación. Ahora debería aparecer en la lista.  
    ![Captura de pantalla en la que se muestra dónde ver el registrador de Azure Application Insights recién creado con la clave de instrumentación.](media/api-management-howto-app-insights/apim-app-insights-logger-2.png)  

> [!NOTE]
> En segundo plano, se crea una entidad [Logger](/rest/api/apimanagement/2019-12-01/logger/createorupdate) en la instancia de API Management, que contiene la clave de instrumentación de la instancia de Application Insights.

## <a name="enable-application-insights-logging-for-your-api"></a>Habilitación del registro Application Insights para la API

1. Vaya a la **instancia del servicio Azure API Management** en **Azure Portal**.
2. Seleccione **API** del menú de la izquierda.
3. Haga clic en la API, en este caso, **Demo Conference API**.
4. Vaya a la pestaña **Configuración** de la barra superior.
5. Desplácese hacia abajo hasta la sección **Diagnostics Logs** (Registros de diagnóstico).  
    ![Registrador de App Insights](media/api-management-howto-app-insights/apim-app-insights-api-1.png)  
6. Marque la casilla **Habilitar**.
7. Seleccione el registrador adjunto en el menú desplegable **Destino**.
8. Escriba **100** como **Sampling (%)** [Muestreo (%)] y marque la casilla **Always log errors** (Registrar errores siempre).
9. Haga clic en **Save** (Guardar).

> [!WARNING]
> Reemplazar el valor predeterminado **0** en el campo **First bytes of body** (Primeros bytes del cuerpo) puede disminuir considerablemente el rendimiento de las API.

> [!NOTE]
> En segundo plano, se crea una entidad de [diagnóstico](/rest/api/apimanagement/2019-12-01/diagnostic/createorupdate) denominada "applicationinsights" en el nivel de API.

| Nombre del valor                        | Tipo de valor                        | Descripción                                                                                                                                                                                                                                                                                                                                      |
|-------------------------------------|-----------------------------------|--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| Habilitar                              | boolean                           | Especifica si el registro de esta API está habilitado.                                                                                                                                                                                                                                                                                                |
| Destination                         | Registrador de Azure Application Insights | Especifica el registrador de Azure Application Insights que se usará                                                                                                                                                                                                                                                                                           |
| Sampling (%) [Muestreo (%)]                        | Decimal                           | Valores de 0 a 100 (porcentaje). <br/> Especifica el porcentaje de solicitudes que se registrarán en Azure Application Insights. Un muestreo del 0 % significa que no se registrará ninguna solicitud, mientras que un muestreo del 100 % se traduce en que se registrarán todas las solicitudes. <br/> Este valor se usa para reducir las consecuencias que tiene en el rendimiento registrar solicitudes en Azure Application Insights (consulte la sección siguiente). |
| Always log errors (Registrar errores siempre)                   | boolean                           | Si se selecciona esta opción, se registrarán todos los errores en Azure Application Insights, independientemente del valor de **Sampling**.                                                                                                                                                                                                                  |
| Opciones básicas: encabezados              | list                              | Especifica los encabezados que se registrarán en Azure Application Insights para las solicitudes y las respuestas.  Predeterminado: no se registra ningún encabezado.                                                                                                                                                                                                             |
| Opciones básicas: Primeros bytes del cuerpo  | integer                           | Especifica cuántos primeros bytes del cuerpo se registrarán en Azure Application Insights para las solicitudes y las respuestas.  Predeterminado: el cuerpo no se registra.                                                                                                                                                                                                    |
| Opciones avanzadas: Nivel de detalle         |                                   | Especifica el nivel de detalle. Solo se registrarán los seguimientos personalizados de mayor nivel de gravedad. Valor predeterminado: información.                                                                                                                                                                                                                               |
| Opciones avanzadas: Solicitud de front-end  |                                   | Especifica si las *solicitudes de front-end* se registran en Azure Application Insights y cuántas se registran. *Frontend request* es una solicitud que entra al servicio de Azure API Management.                                                                                                                                                                        |
| Opciones avanzadas: Respuesta de front-end |                                   | Especifica si las *respuestas de front-end* se registran en Azure Application Insights y cuántas se registran. *Frontend response* es una respuesta que sale del servicio de Azure API Management.                                                                                                                                                                   |
| Opciones avanzadas: Solicitud de back-end   |                                   | Especifica si las *solicitudes de back-end* se registran en Azure Application Insights y cuántas se registran. *Backend request* es una solicitud que sale del servicio de Azure API Management.                                                                                                                                                                        |
| Opciones avanzadas: Respuesta de back-end  |                                   | Especifica si las *respuestas de back-end* se registran en Azure Application Insights y cuántas se registran. *Backend response* es una respuesta que entra al servicio de Azure API Management.                                                                                                                                                                       |

> [!NOTE]
> Puede especificar registradores en distintos niveles: registrador para una API o registrador para todas las API.
>  
> Si especifica ambos:
> + si son registradores distintos, se usarán ambos (registros de multiplexación);
> + si son el mismo registrador, pero tienen configuraciones diferentes, el correspondiente a una sola API (nivel más granular) invalidará al que corresponde a todas las API.

## <a name="what-data-is-added-to-azure-application-insights"></a>Qué datos se agregan a Azure Application Insights

Azure Application Insights recibe:

+ elemento de telemetría *Solicitud* , para cada solicitud de entrada ( *solicitud de front-end* , *respuesta de front-end* );
+ elemento de telemetría *Dependencia* , para cada solicitud reenviada a un servicio de back-end ( *solicitud de back-end* , *respuesta de back-end* );
+ elemento de telemetría *Excepción* , para cada solicitud con error.

Una solicitud con error es una solicitud que:

+ no se pudo procesar debido a una conexión cerrada con el cliente; o
+ desencadenó una sección *on-error* (al producirse un error) de las directivas de la API; o
+ tiene un código de estado HTTP de respuesta que coincide con 4xx o 5xx.

## <a name="performance-implications-and-log-sampling"></a>Consecuencias en el rendimiento y muestreo de registros

> [!WARNING]
> Registrar todos los eventos puede tener graves consecuencias en el rendimiento, según la tasa de solicitudes de entrada.

En función de las pruebas de carga internas, habilitar esta característica provocó una reducción del 40 % al 50 % en el rendimiento cuando la tasa de solicitudes superaba las 1000 solicitudes por segundo. Azure Application Insights está diseñada para usar análisis estadísticos para evaluar el rendimiento de las aplicaciones. No está destinado a ser un sistema de auditoría y no es apta para registrar cada solicitud individual para API de gran volumen.

Para manipular el número de solicitudes que se registran, puede ajustar la opción **Sampling** (Muestreo) que se explica anteriormente. Un valor del 100 % significa que se registran todas las solicitudes, mientras que un 0 % indica que no se registra nada en absoluto. **Sampling** (Muestreo) ayuda a reducir el volumen de telemetría, lo que evita de manera eficaz una degradación significativa en el rendimiento, a la vez que brinda las ventajas del registro.

Omitir el registro de encabezados y del cuerpo de las solicitudes y respuestas también tendrá un efecto positivo para evitar problemas de rendimiento.

## <a name="video"></a>Vídeo

> [!VIDEO https://www.microsoft.com/en-us/videoplayer/embed/RE2pkXv]
>
>

## <a name="next-steps"></a>Pasos siguientes

+ Más información sobre [Azure Application Insights](/azure/application-insights/).
+ Tenga en cuenta el [registro con Azure Event Hubs](api-management-howto-log-event-hubs.md).
