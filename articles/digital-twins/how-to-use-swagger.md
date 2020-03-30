---
title: 'Uso de la documentación de referencia de Swagger: Azure Digital Twins | Microsoft Docs'
description: Descripción de cómo utilizar la documentación de referencia de Swagger de Azure Digital Twins | Microsoft Docs
ms.author: alinast
author: alinamstanciu
manager: bertvanhoof
ms.service: digital-twins
services: digital-twins
ms.topic: conceptual
ms.date: 02/03/2020
ms.custom: seodec18
ms.openlocfilehash: 27874e5731bd6fb9821e7aeda9333adbdbb79099
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/27/2020
ms.locfileid: "77023300"
---
# <a name="azure-digital-twins-swagger-reference-documentation"></a>Documentación de referencia de Swagger de Azure Digital Twins

Cada instancia aprovisionada de Azure Digital Twins incluye su propia documentación de referencia de Swagger generada automáticamente.

[Swagger](https://swagger.io/) (u [OpenAPI](https://www.openapis.org/)) reúne información compleja de las API en un recurso de referencia interactivo e independiente del lenguaje. Swagger proporciona material de referencia crítico sobre qué cargas JSON, métodos HTTP y puntos de conexión concretos se deben usar para realizar operaciones en una API.

## <a name="swagger-summary"></a>Resumen de Swagger

Swagger proporciona un resumen interactivo de la API, en el que se incluye:

* Información de la API y del modelo de objetos.
* Los puntos de conexión de API REST con especificación de las cargas de solicitud necesarias, encabezados, parámetros, las rutas de acceso de contexto y los métodos HTTP.
* Pruebas de las funcionalidades de la API.
* Información de respuesta de ejemplo usada para comprobar y confirmar las respuestas HTTP.
* Información de código de error.

Swagger es una herramienta muy útil para ayudar con el desarrollo y las pruebas de las llamadas realizadas a Management API de Azure Digital Twins.

[!INCLUDE [Digital Twins Swagger](../../includes/digital-twins-swagger.md)]

## <a name="reference-material"></a>Material de referencia

El material de referencia de Swagger generado automáticamente proporciona una introducción rápida a los conceptos importantes, puntos de conexión de API de administración disponibles, así como una descripción de cada modelo de objetos para ayudar en el desarrollo y las pruebas.

Un breve resumen describe la API.

[![Resumen de Swagger e información general de la API](media/how-to-use-swagger/swagger-management-top-img.png)](media/how-to-use-swagger/swagger-management-top-img.png#lightbox)

También se muestran los modelos de objetos de la API de administración.

[![Modelos de Swagger enumerados en la parte inferior de la interfaz de usuario de Swagger](media/how-to-use-swagger/swagger-management-models-img.png)](media/how-to-use-swagger/swagger-management-models-img.png#lightbox)

Puede seleccionar cada modelo de objetos de la lista para obtener un resumen más detallado de los atributos clave.

[![Modelos de Swagger expandidos para leer el contenido de los modelos](media/how-to-use-swagger/swagger-management-model-img.png)](media/how-to-use-swagger/swagger-management-model-img.png#lightbox)

Los modelos de objetos de Swagger generados resultan prácticos para leer todos [los objetos y las API](./concepts-objectmodel-spatialgraph.md) de Azure Digital Twins que están disponibles. Los desarrolladores utilizar recurso al crear soluciones en Azure Digital Twins.

## <a name="endpoint-summary"></a>Resumen de puntos de conexión

Swagger también proporciona un resumen exhaustivo de todos los puntos de conexión que componen las API de administración.

Cada punto de conexión de la lista también incluye la información de solicitud necesaria, como:

* Parámetros obligatorios.
* Tipos de datos de parámetro obligatorios.
* Método HTTP para acceder al recurso.

[![Puntos de conexión de Swagger mostrados en la interfaz de usuario de Swagger](media/how-to-use-swagger/swagger-management-endpoints-img.png)](media/how-to-use-swagger/swagger-management-endpoints-img.png#lightbox)

Seleccione cada uno de los recursos para mostrar su contenido adicional y obtener información general más detallada.

## <a name="use-swagger-to-test-endpoints"></a>Uso de Swagger para probar los puntos de conexión

Una de las funcionalidades más eficaces de Swagger proporciona la capacidad de probar un punto de conexión de la API directamente mediante la interfaz de usuario de la documentación.

Después de seleccionar un punto de conexión específico, se mostrará el botón **Probar**.

[![Botón de prueba de Swagger](media/how-to-use-swagger/swagger-management-try-img.png)](media/how-to-use-swagger/swagger-management-try-img.png#lightbox)

Al expandir esa sección se muestran los campos de entrada para cada parámetro obligatorio y opcional. Escriba los valores correctos y seleccione **Ejecución**.

[![Ejemplo de resultado de la prueba de Swagger](media/how-to-use-swagger/swagger-management-tried-img.png)](media/how-to-use-swagger/swagger-management-tried-img.png#lightbox)

Después de ejecutar la prueba, puede comprobar los datos de respuesta.

## <a name="swagger-response-data"></a>Datos de respuesta de Swagger

Cada punto de conexión en la lista también incluye datos del cuerpo de respuesta para comprobar desarrollo y pruebas. Estos ejemplos incluyen los códigos de estado y JSON para las solicitudes correctas de HTTP.

[![Ejemplo de respuesta JSON de Swagger](media/how-to-use-swagger/swagger-management-response-img.png)](media/how-to-use-swagger/swagger-management-response-img.png#lightbox)

Los ejemplos incluyen también los códigos de error para ayudar a depurar o mejorar los errores de las pruebas.

## <a name="swagger-oauth-20-authorization"></a>Autorización de Swagger OAuth 2.0

> [!NOTE]
> * La entidad de seguridad de usuario que creó el recurso de Azure Digital Twins tendrá una asignación del rol Administrador de espacio y podrá crear asignaciones de roles adicionales para otros usuarios. Estos usuarios y sus roles se pueden autorizar para llamar a las API.

1. Siga los pasos de [la guía de inicio rápido](quickstart-view-occupancy-dotnet.md#set-permissions-for-your-app) para crear y configurar una aplicación de Azure Active Directory. También puede volver a usar un registro de aplicación existente.

1. Agregue el siguiente **URI de redirección** al registro de la aplicación de Azure Active Directory:

    [![Registro de la dirección URL de redireccionamiento de Swagger en AAD](media/how-to-use-swagger/swagger-aad-redirect-url-registration.png)](media/how-to-use-swagger/swagger-aad-redirect-url-registration.png#lightbox)

    ```plaintext
    https://YOUR_SWAGGER_URL/ui/oauth2-redirect-html
    ```
    | Nombre  | Reemplazar por | Ejemplo |
    |---------|---------|---------|
    | YOUR_SWAGGER_URL | La dirección URL de documentación de API REST de administración que se encuentra en el portal  | `https://yourDigitalTwinsName.yourLocation.azuresmartspaces.net/management/swagger` |

1. Active la casilla **Concesión implícita** > **Tokens de acceso** para permitir el uso del flujo de concesión implícita de OAuth 2.0. Seleccione **Configurar** y, a continuación, **Guardar**.

1. Copie el **Id. del inquilino** de la aplicación de Azure Active Directory.

Después de completar el registro de Azure Active Directory:

1. Seleccione el botón **Autorizar** botón en la página de Swagger.

    [![Selección del botón Autorizar de Swagger](media/how-to-use-swagger/swagger-select-authorize-btn.png)](media/how-to-use-swagger/swagger-select-authorize-btn.png#lightbox)

1. Pegue el identificador de la aplicación en el campo **client_id**.

    [![Campo client_id de Swagger](media/how-to-use-swagger/swagger-auth-form.png)](media/how-to-use-swagger/swagger-auth-form.png#lightbox)

1. Se le redirigirá al siguiente cuadro de diálogo modal de operación correcta.

    [![Cuadro de diálogo modal de redirección de Swagger](media/how-to-use-swagger/swagger-auth-redirect-img.png)](media/how-to-use-swagger/swagger-auth-redirect-img.png#lightbox)

Para obtener más información sobre las solicitudes de pruebas interactivas protegidas por OAuth 2.0, lea la [documentación oficial](https://swagger.io/docs/specification/authentication/oauth2/).

## <a name="next-steps"></a>Pasos siguientes

- Para más información sobre los modelos de objeto de Azure Digital Twins y el grafo de inteligencia espacial, lea [Descripción de los modelos de objetos de Azure Digital Twins](./concepts-objectmodel-spatialgraph.md).

- Para aprender cómo autenticar con Management API, lea [Autenticación con las API](./security-authenticating-apis.md).
