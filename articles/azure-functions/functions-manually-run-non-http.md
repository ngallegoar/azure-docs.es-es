---
title: Ejecución manual de una instancia de Azure Functions no desencadenada por HTTP
description: Uso de una solicitud HTTP para ejecutar una instancia de Azure Functions no desencadenada por HTTP
author: craigshoemaker
ms.topic: tutorial
ms.date: 12/12/2018
ms.author: cshoe
ms.openlocfilehash: 4ce7b8590e4718585fe841921466e049dc204928
ms.sourcegitcommit: 0947111b263015136bca0e6ec5a8c570b3f700ff
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/24/2020
ms.locfileid: "75769139"
---
# <a name="manually-run-a-non-http-triggered-function"></a>Ejecución manual de una función no desencadenada por HTTP

En este artículo se muestra cómo ejecutar manualmente una función no desencadenada por HTTP a través de una solicitud HTTP con formato especial.

En algunos contexto, es posible que tenga que ejecutar "a petición" una instancia de Azure Functions desencadenada de manera indirecta.  Algunos ejemplos de desencadenadores indirectos son las [funciones según una programación](./functions-create-scheduled-function.md) o las funciones que se ejecutan como resultado de la [acción de otro recurso](./functions-create-storage-blob-triggered-function.md). 

En el ejemplo siguiente se usa [Postman](https://www.getpostman.com/), pero puede usar [cURL](https://curl.haxx.se/), [Fiddler](https://www.telerik.com/fiddler) o cualquier otra herramienta similar para enviar solicitudes HTTP.

## <a name="define-the-request-location"></a>Definición de la ubicación de la solicitud

Para ejecutar una función no desencadenada por HTTP, necesita una manera de enviar una solicitud a Azure para que ejecute la función. La dirección URL que se usa para hacer esta solicitud tiene un formato específico.

![Definición de la ubicación de la solicitud: nombre de host + ruta de acceso a la carpeta + nombre de la función](./media/functions-manually-run-non-http/azure-functions-admin-url-anatomy.png)

- **Nombre de host:** La ubicación pública de la aplicación de función que consta del nombre de la aplicación de función más *azurewebsites.net* o el dominio personalizado.
- **Ruta de acceso a la carpeta:** Para acceso a las funciones no desencadenadas por HTTP a través de una solicitud HTTP, debe enviar la solicitud a través de las carpetas *admin/functions*.
- **Nombre de la función:** El nombre de la función que quiere ejecutar.

Puede usar la ubicación de esta solicitud en Postman junto con la clave maestra de la función en la solicitud para que Azure ejecute la función.

> [!NOTE]
> Cuando se ejecuta localmente, la clave maestra de la función no es necesaria. Puede [llamar a la función](#call-the-function) directamente al omitir el encabezado `x-functions-key`.

## <a name="get-the-functions-master-key"></a>Obtención de la clave maestra de la función

Vaya a la función en Azure Portal, haga clic en **Administrar** y busque la sección **Claves de host**. Haga clic en el botón **Copiar** en la fila *_master* para copiar la clave maestra en el Portapapeles.

![Copia de la clave maestra desde la pantalla de administración de la función](./media/functions-manually-run-non-http/azure-portal-functions-master-key.png)

Después de copiar la clave maestra, haga clic en el nombre de la función para volver a la ventana de archivo de código. Luego, haga clic en la pestaña **Registros**. Aquí verá mensajes de la función que se registraron cuando ejecutó manualmente la función desde Postman.

> [!CAUTION]  
> Debido a los permisos elevados de la aplicación de función otorgados por la clave maestra, no debe compartir esta clave con terceros ni distribuirla en una aplicación.

## <a name="call-the-function"></a>Llamada a la función

Abra Postman y siga estos pasos:

1. Escriba la **ubicación de la solicitud en el cuadro de texto de la dirección URL**.
2. Asegúrese de que el método HTTP está establecido en **POST**.
3. **Haga clic** en la pestaña **Encabezados**.
4. Escriba **x-functions-key** como la primera **clave** y pegue la clave maestra (que está en el Portapapeles) en el cuadro **value**.
5. Escriba **Content-Type** como la segunda **clave** y **application/json** como el **valor**.

    ![Configuración de los encabezados de Postman](./media/functions-manually-run-non-http/functions-manually-run-non-http-headers.png)

6. **Haga clic** en la pestaña **Cuerpo**.
7. Escriba **{ "input": "test" }** como el cuerpo de la solicitud.

    ![Configuración del cuerpo de Postman](./media/functions-manually-run-non-http/functions-manually-run-non-http-body.png)

8. Haga clic en **Enviar**.

    ![Envío de una solicitud con Postman](./media/functions-manually-run-non-http/functions-manually-run-non-http-send.png)

Postman luego notifica un estado de **202 - Aceptado**.

A continuación, vuelva a la función en Azure Portal. Busque la ventana *Registros* y verá mensajes procedentes de la llamada manual a la función.

![Resultados del registro de la función a partir de la llamada local](./media/functions-manually-run-non-http/azure-portal-function-log.png)

## <a name="next-steps"></a>Pasos siguientes

- [Estrategias para probar el código en Azure Functions](./functions-test-a-function.md)
- [Depuración local de funciones de Azure desencadenadas por Event Grid](./functions-debug-event-grid-trigger-local.md)
