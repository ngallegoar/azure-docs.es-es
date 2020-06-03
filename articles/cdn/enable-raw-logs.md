---
title: Registros sin procesar de HTTP de Azure CDN
description: En este artículo se describen los registros sin procesar de HTTP de Azure CDN.
services: cdn
author: sohamnchatterjee
manager: danielgi
ms.service: azure-cdn
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 03/23/2020
ms.author: sohamnc
ms.openlocfilehash: f4693cdac2d5d3a57987cc692797756314ded11e
ms.sourcegitcommit: 493b27fbfd7917c3823a1e4c313d07331d1b732f
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 05/21/2020
ms.locfileid: "83747313"
---
# <a name="azure-cdn-http-raw-logs"></a>Registros sin procesar de HTTP de Azure CDN
Los registros sin procesar proporcionan información valiosa acerca de las operaciones y los errores que son importantes para la auditoría, así como para solucionar problemas. Los registros sin procesar son diferentes de los registros de actividad. Los registros de actividad proporcionan visibilidad sobre las operaciones llevadas a cabo en los recursos de Azure. Los registros sin procesar proporcionan un registro de las operaciones del recurso.

> [!IMPORTANT]
> La característica Registros sin procesar de HTTP está disponible para Azure CDN de Microsoft.

Si no tiene una suscripción a Azure, cree una [cuenta gratuita](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) antes de empezar. 

## <a name="sign-in-to-azure"></a>Inicio de sesión en Azure

Inicie sesión en Azure Portal en [https://portal.azure.com](https://portal.azure.com).

## <a name="configuration"></a>Configuración

Para configurar los registros sin procesar para Azure CDN desde el perfil de Microsoft: 

1. En el menú de Azure Portal, seleccione **Todos los recursos** >>  **\<su-perfil-de-CDN>** .

2. En **Supervisión**, seleccione **Configuración de diagnóstico**.

3. Seleccione **+ Agregar configuración de diagnóstico**.

    ![Configuración de diagnóstico de CDN](./media/cdn-raw-logs/raw-logs-01.png)

    > [!IMPORTANT]
    > Los registros sin procesar solo están disponibles en el nivel de perfil, mientras que los registros de código de estado HTTP agregados están disponibles en el nivel de punto de conexión.

4. En **Configuración de diagnóstico**, escriba un nombre para la configuración de diagnóstico en **Nombre de la configuración de diagnóstico**.

5. Seleccione el **registro** y establezca la retención en días.

6. Seleccione **Detalles del destino**. Las opciones de destino son:
    * **Enviar a Log Analytics**
        * Seleccione la **Suscripción** y el **Área de trabajo de Log Analytics**.
    * **Archivar en una cuenta de almacenamiento**
        * Seleccione la **Suscripción** y la **Cuenta de almacenamiento**.
    * **Transmisión a un centro de eventos**
        * Seleccione la **Suscripción**, el **Espacio de nombres del centro de eventos**, el **Nombre del centro de eventos (opcional)** y el **Nombre de la directiva del centro de eventos**.

    ![Configuración de diagnóstico de CDN](./media/cdn-raw-logs/raw-logs-02.png)

7. Seleccione **Guardar**.

## <a name="raw-logs-properties"></a>Propiedades de los registros sin procesar

En la actualidad, Azure CDN del servicio de Microsoft proporciona los registros sin procesar. Los registros sin procesar proporcionan las solicitudes de API individuales con el siguiente esquema para cada entrada: 

| Propiedad              | Descripción                                                                                                                                                                                          |
|-----------------------|------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| TrackingReference     | La cadena de referencia exclusiva que identifica una solicitud atendida por Front Door, que también se envía como encabezado X-Azure-Ref al cliente. Se requiere para buscar los detalles en los registros de acceso para una solicitud específica. |
| HttpMethod            | Método HTTP utilizado por la solicitud.                                                                                                                                                                     |
| HttpVersion           | Tipo de la solicitud o conexión.                                                                                                                                                                   |
| RequestUri            | URI de la solicitud recibida.                                                                                                                                                                         |
| RequestBytes          | El tamaño del mensaje de solicitud HTTP en bytes, incluidos los encabezados de solicitud y el cuerpo de solicitud.                                                                                                   |
| ResponseBytes         | Bytes enviados por el servidor back-end como respuesta.                                                                                                                                                    |
| UserAgent             | Tipo de explorador utilizado por el cliente.                                                                                                                                                               |
| ClientIp              | Dirección IP del cliente que realizó la solicitud.                                                                                                                                                  |
| TimeTaken             | El período de tiempo que tardó la acción, en milisegundos.                                                                                                                                            |
| SecurityProtocol      | La versión del protocolo TLS/SSL utilizada por la solicitud o null si no hay cifrado.                                                                                                                           |
| Punto de conexión              | Host del punto de conexión de la red CDN que se ha configurado en el perfil de CDN primario.                                                                                                                                   |
| Nombre de host de back-end     | Nombre del host u origen de back-end al que se envían las solicitudes.                                                                                                                                |
| Enviado al escudo de origen | Si es True, significa que la solicitud se respondió desde la memoria caché del escudo de origen en lugar del PoP perimetral. El escudo de origen es una memoria caché primaria que se usa para mejorar la proporción de aciertos de caché.                                       |
| HttpStatusCode        | El código de estado HTTP devuelto desde el servidor proxy.                                                                                                                                                        |
| HttpStatusDetails     | Estado resultante en la solicitud. El significado de este valor de cadena puede encontrarse en una tabla de referencia de estado.                                                                                              |
| PoP                   | El servidor PoP perimetral, que respondió a la solicitud del usuario. Las abreviaturas de los PoP son los códigos de aeropuerto de sus correspondientes áreas metropolitanas.                                                                                   |
| Estado de la caché          | Indica si el objeto se devolvió desde la memoria caché o procede del origen.                                                                                                             |
> [!IMPORTANT]
> La característica de registros sin formato HTTP está disponible automáticamente para los perfiles creados o actualizados después del **25 de febrero de 2020**. En el caso de los perfiles de CDN creados anteriormente, debe actualizar el punto de conexión de CDN después de configurar el registro. Por ejemplo, puede navegar al filtrado geográfico en puntos de conexión de CDN y bloquear cualquier país o región que no sea relevante para su carga de trabajo y pulsar Guardar. 

> [!NOTE]
> Para ver los registros en el perfil de Log Analytics, ejecute una consulta. Una consulta de ejemplo tendría este aspecto:              AzureDiagnostics | where Category == "AzureCdnAccessLog"

## <a name="next-steps"></a>Pasos siguientes
En este artículo, ha habilitado los registros sin procesar de HTTP para el servicio CDN de Microsoft.

Para más información sobre Azure CDN y los otros servicios de Azure que se mencionan en este artículo, consulte:

* [Análisis](cdn-log-analysis.md) de patrones de uso de Azure CDN.

* Más información sobre [Azure Monitor](https://docs.microsoft.com/azure/azure-monitor/overview).

* Configuración de [Log Analytics en Azure Monitor](https://docs.microsoft.com/azure/azure-monitor/log-query/get-started-portal).
