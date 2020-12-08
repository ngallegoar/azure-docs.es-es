---
title: Realización de solicitudes con Postman
titleSuffix: Azure Digital Twins
description: Aprenda a configurar y usar Postman para probar las API de Azure Digital Twins.
ms.author: baanders
author: baanders
ms.service: digital-twins
services: digital-twins
ms.topic: how-to
ms.date: 11/10/2020
ms.openlocfilehash: f9be9dd94aad8c206b562f2c984ec944f70d3957
ms.sourcegitcommit: d22a86a1329be8fd1913ce4d1bfbd2a125b2bcae
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 11/26/2020
ms.locfileid: "96188044"
---
# <a name="how-to-use-postman-to-send-requests-to-the-azure-digital-twins-apis"></a>Uso de Postman para enviar solicitudes a las API de Azure Digital Twins

[Postman](https://www.getpostman.com/) es una herramienta de pruebas REST que proporciona funcionalidades clave de solicitud HTTP en una interfaz gráfica de usuario basada en escritorio y complementos. Puede usarla para elaborar solicitudes HTTP y enviarlas a las [API REST de Azure Digital Twins](how-to-use-apis-sdks.md).

En este artículo se describe cómo configurar el [cliente REST de Postman](https://www.getpostman.com/) para interactuar con las API de Azure Digital Twins. Para ello, es preciso seguir estos pasos:

1. Use la [CLI de Azure](/cli/azure/install-azure-cli?view=azure-cli-latest&preserve-view=true) para obtener un token de portador que usará para realizar solicitudes de API en Postman.
1. Configure una colección de Postman y configure el cliente de REST de Postman para que use el token de portador para la autenticación.
1. Use la versión de Postman configurada para crear y enviar una solicitud a las API de Azure Digital Twins.

## <a name="prerequisites"></a>Prerrequisitos

Para continuar con el uso de Postman para acceder a las API de Azure Digital Twins, es preciso configurar una instancia de Azure Digital Twins y descargar Postman. El resto de esta sección le guía a través de estos pasos.

### <a name="set-up-azure-digital-twins-instance"></a>Configuración de la instancia de Azure Digital Twins

[!INCLUDE [digital-twins-prereq-instance.md](../../includes/digital-twins-prereq-instance.md)]

### <a name="download-postman"></a>Descarga de Postman

A continuación, descargue la versión de escritorio del cliente de Postman. Vaya a [*www.getpostman.com/apps*](https://www.getpostman.com/apps) y siga las indicaciones para descargar la aplicación.

## <a name="get-bearer-token"></a>Obtención de un token de portador

Ahora que ha configurado Postman y su instancia de Azure Digital Twins, deberá obtener un token de portador que las solicitudes de Postman puedan usar para la autorización en las API de Azure Digital Twins.

Este token se puede obtener de varias formas. En este artículo se usa la [CLI de Azure](/cli/azure/install-azure-cli?view=azure-cli-latest&preserve-view=true) para iniciar sesión en su cuenta de Azure y obtener así un token.

Si tiene una CLI de Azure [instalada localmente](/cli/azure/install-azure-cli?view=azure-cli-latest&preserve-view=true), puede iniciar un símbolo del sistema en la máquina para ejecutar los siguientes comandos.
De lo contrario, puede abrir una ventana de [Azure Cloud Shell](https://shell.azure.com) en el explorador y ejecutar los comandos en ella.

1. En primer lugar, asegúrese de que ha iniciado sesión en Azure con las credenciales apropiadas. Para ello, ejecute este comando:

    ```azurecli-interactive
    az login
    ```

1. Luego, use el comando [az account get-access-token](https://docs.microsoft.com/cli/azure/account?view=azure-cli-latest&preserve-view=true#az_account_get_access_token) para obtener un token de portador con acceso al servicio Azure Digital Twins.

    ```azurecli-interactive
    az account get-access-token --resource 0b07f429-9f4b-4714-9392-cc5e8e80c8b0
    ```

1. Copie el valor de `accessToken` en el resultado y guárdelo para usarlo en la siguiente sección. Este es el **valor del token** que proporcionará a Postman para autenticar las solicitudes.

    :::image type="content" source="media/how-to-use-postman/console-access-token.png" alt-text="Vista de una ventana de la consola local que muestra el resultado del comando az account get-access-token. Uno de los campos del resultado se denomina accessToken y su valor de ejemplo (que comienza por ey) está resaltado.":::

>[!TIP]
>Este token es válido durante un mínimo de 5 minutos y un máximo de 60. Si se agota el tiempo asignado al token actual, puede repetir los pasos de esta sección para obtener uno nuevo.

## <a name="set-up-postman-collection-and-authorization"></a>Configuración de la colección y autorización de Postman

A continuación, configure Postman para realizar solicitudes de API.
Estos pasos se producen en la aplicación de Postman local, así que continúe y ábrala en el equipo.

### <a name="create-a-postman-collection"></a>Creación de la colección de Postman

En Postman, las solicitudes se guardan en **colecciones** (grupos de solicitudes). Al crear una colección para agrupar las solicitudes, puede aplicar una configuración común a muchas solicitudes a la vez. Esto puede simplificar considerablemente la autorización si planea crear más de una solicitud con las API de Azure Digital Twins, ya que solo tiene que configurar la autenticación una vez para toda la recopilación.

1. Para crear una colección, presione el botón *+ New Collection* (+ Nueva colección).

    :::image type="content" source="media/how-to-use-postman/postman-new-collection.png" alt-text="Vista de una ventana de Postman recién abierta. El botón &quot;New Collection&quot; (Nueva conexión) está resaltado":::

1. En la ventana *CREATE A NEW COLLECTION* (CREAR UNA NUEVA COLECCIÓN) que aparece a continuación, especifique un **nombre** y una **descripción** opcional para la colección.

Luego, vaya a la sección siguiente para agregar un token de portador a la colección para la autorización.

### <a name="add-authorization-token-and-finish-collection"></a>Incorporación de un token de autorización y finalización de la recopilación

1. En el cuadro de diálogo *CREATE A NEW COLLECTION* (CREAR UNA NUEVA COLECCIÓN), vaya a la pestaña *Authorization* (Autorización). Ahí es donde colocará el **valor del token** que ha recopilado en la sección [Obtención de un token de portador](#get-bearer-token) para usarlo en todas las solicitudes de API de la colección.

    :::image type="content" source="media/how-to-use-postman/postman-authorization.png" alt-text="La ventana &quot;CREATE A NEW COLLECTION&quot; (CREAR UNA NUEVA COLECCIÓN) de Postman en la que se muestra la pestaña &quot;Authorization&quot; (Autorización).":::

1. En *Type* (Tipo), seleccione _**OAuth 2.0**_ y pegue el token de acceso en el cuadro *Access Token* (Token de acceso).

    :::image type="content" source="media/how-to-use-postman/postman-paste-token.png" alt-text="La ventana &quot;CREATE A NEW COLLECTION&quot; (CREAR UNA NUEVA COLECCIÓN) de Postman en la que se muestra la pestaña &quot;Authorization&quot; (Autorización). Está seleccionado un tipo de &quot;OAuth 2.0&quot; y donde está resaltado el cuadro Access Token (Token de acceso), se puede pegar el valor del token de acceso.":::

1. Después de pegar el token de portador, pulse *Create* (Crear) para terminar de crear la colección.

La nueva colección se puede ver ahora en la vista principal de Postman, en *Collections* (Colecciones).

:::image type="content" source="media/how-to-use-postman/postman-post-collection.png" alt-text="Vista de la ventana principal de Postman. La colección recién creada está resaltada en la pestaña &quot;Collections&quot; (Colecciones).":::

## <a name="create-a-request"></a>Creación de una solicitud

Después de completar los pasos anteriores, puede crear solicitudes en las API de Azure Digital Twins.

1. Para crear una solicitud, pulse el botón *+ New* (+ Nuevo).

    :::image type="content" source="media/how-to-use-postman/postman-new-request.png" alt-text="Vista de la ventana principal de Postman. El botón Next (Siguiente) está resaltado":::

1. Elija *Request* (Solicitud).

    :::image type="content" source="media/how-to-use-postman/postman-new-request-2.png" alt-text="Vista de las opciones que puede seleccionar para crear algo nuevo. La opción &quot;Request&quot; (Solicitud) está resaltada":::

1. Esta acción abre la ventana *Save request* (Guardar solicitud), donde puede especificar el nombre de la solicitud, darle una descripción opcional y elegir la colección de la que forma parte. Rellene los detalles y guarde la solicitud en la colección que creó anteriormente.

    :::row:::
        :::column:::
            :::image type="content" source="media/how-to-use-postman/postman-save-request.png" alt-text="Vista de la ventana &quot;Save request&quot; (Guardar solicitud), donde puede rellenar los campos descritos. El botón &quot;Save to Azure Digital Twins collection&quot; (Guardar en colección de Azure Digital Twins) está resaltado":::
        :::column-end:::
        :::column:::
        :::column-end:::
    :::row-end:::

Ahora puede ver la solicitud en la colección y seleccionarla para extraer sus detalles editables.

:::image type="content" source="media/how-to-use-postman/postman-request-details.png" alt-text="Vista de la ventana principal de Postman. La colección de Azure Digital Twins está expandida y se resalta la solicitud &quot;Query twins&quot; (Consultar gemelos). Los detalles de la solicitud se muestran en el centro de la página." lightbox="media/how-to-use-postman/postman-request-details.png":::

### <a name="set-request-details"></a>Establecimiento de los detalles de la solicitud

Para realizar una solicitud de Postman en una de las API de Azure Digital Twins, necesitará la dirección URL de la API e información sobre los detalles que requiere. Esta información se puede encontrar en la [documentación de referencia de la API REST de Azure Digital Twins](/rest/api/azure-digitaltwins/).

Para continuar con una consulta de ejemplo, en este artículo se usará Query API (y su [documentación de referencia](/rest/api/digital-twins/dataplane/query/querytwins)) para consultar todos los gemelos digitales de una instancia.

1. Obtenga la dirección URL y el tipo de la solicitud en la documentación de referencia. En el caso de Query API, es *POST https://digitaltwins-name.digitaltwins.azure.net/query?api-version=2020-10-31* .
1. En Postman, establezca el tipo de la solicitud y escriba la dirección URL de la solicitud, rellenando los marcadores de posición en la dirección URL según sea necesario. Aquí es donde usará el **nombre de host** de la instancia de la sección [*Requisitos previos*](#prerequisites).
    
   :::image type="content" source="media/how-to-use-postman/postman-request-url.png" alt-text="En los detalles de la nueva solicitud, la dirección URL de la consulta de la documentación de referencia se ha rellenado en el cuadro de la dirección URL de la solicitud." lightbox="media/how-to-use-postman/postman-request-url.png":::
    
1. Compruebe que los parámetros que se muestran para la solicitud en la pestaña *Params* (Parámetros) coinciden con los descritos en la documentación de referencia. En el caso de esta solicitud en Postman, el parámetro `api-version` se rellenó automáticamente al escribir la dirección URL de la solicitud en el paso anterior. Para Query API, este es el único parámetro necesario, por lo que este paso ha terminado.
1. En la pestaña *Authorization* (Autorización), en *Type* (Tipo) seleccione *Inherit auth from parent* (Heredar autorización de primario). Esto indica que esta solicitud usará la autenticación que configuró anteriormente para toda la colección.
1. Compruebe que los encabezados que se muestran para la solicitud en la pestaña *Headers* (Encabezados) coinciden con los descritos en la documentación de referencia. Para esta solicitud, se han rellenado automáticamente varios encabezados. Para Query API, no se requiere ninguna de las opciones de encabezado, por lo que este paso ha finalizado.
1. Compruebe que el cuerpo que se muestra para la solicitud en la pestaña *Body* (Cuerpo) coincide con las necesidades descritas en la documentación de referencia. Para Query API, se requiere un cuerpo JSON que proporcione el texto de la consulta. Este es un cuerpo de ejemplo para esta solicitud que consulta todos los gemelos digitales de la instancia:

   :::image type="content" source="media/how-to-use-postman/postman-request-body.png" alt-text="En los detalles de la nueva solicitud, se muestra la pestaña Body (Cuerpo). Contiene un cuerpo JSON sin formato con una consulta de &quot;SELECT * FROM DIGITALTWINS&quot;."lightbox="media/how-to-use-postman/postman-request-body.png":::

   Para más información sobre cómo crear consultas de Azure Digital Twins, consulte [*Procedimiento: Consulta del grafo de gemelos*](how-to-query-graph.md).

1. Consulte en la documentación de referencia otros campos que pueden ser necesarios para su tipo de solicitud. En el caso de Query API, ahora se han cumplido todos los requisitos en la solicitud de Postman, por lo que este paso ha finalizado.
1. Use el botón *Send* (Enviar) para enviar la solicitud completada.
   :::image type="content" source="media/how-to-use-postman/postman-request-send.png" alt-text="Cerca de los detalles de la nueva solicitud, el botón Send (Enviar) aparece resaltado." lightbox="media/how-to-use-postman/postman-request-send.png":::

Después de enviar la solicitud, los detalles de la respuesta aparecerán en la ventana de Postman debajo de la solicitud. Puede ver el código de estado de la respuesta y cualquier texto del cuerpo.

:::image type="content" source="media/how-to-use-postman/postman-request-response.png" alt-text="Debajo de los detalles de la solicitud enviada, se resaltan los detalles de la respuesta. Hay un estado de 200 OK y texto del cuerpo, que describe los gemelos digitales que ha devuelto la consulta." lightbox="media/how-to-use-postman/postman-request-response.png":::

También puede comparar la respuesta con los datos de respuesta esperados que se proporcionan en la documentación de referencia para comprobar el resultado u obtener más información sobre los errores que surjan.

## <a name="next-steps"></a>Pasos siguientes

Para más información sobre las API de Digital Twins, lea [*Procedimiento: Uso de las API y los SDK de Azure Digital Twins*](how-to-use-apis-sdks.md) o vea la [documentación de referencia de las API REST](/rest/api/azure-digitaltwins/).