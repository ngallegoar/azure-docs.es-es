---
author: IngridAtMicrosoft
ms.service: media-services
ms.topic: include
ms.date: 11/04/2020
ms.author: inhenkel
ms.custom: portal
ms.openlocfilehash: 72857564a6b195353e7bf2a27c8369004d52d3f2
ms.sourcegitcommit: a43a59e44c14d349d597c3d2fd2bc779989c71d7
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 11/25/2020
ms.locfileid: "95971370"
---
<!-- Use the portal to create a media services account. -->

## <a name="create-a-media-services-account"></a>Creación de una cuenta de Media Services

1. Inicie sesión en [Azure Portal](https://portal.azure.com/).
1. Haga clic en **+ Crear un recurso** > **Multimedia** > **Media Services**.
1. En la sección **Creación de una cuenta de Media Services** escriba los valores necesarios.

    | Nombre | Descripción |
    | ---|---|
    |**Nombre de cuenta**|Escriba el nombre de la nueva cuenta de Media Services. El nombre de la cuenta de Media Services debe estar compuesto totalmente de minúsculas o de números, sin espacios, y con una longitud de entre 3 y 24 caracteres.|
    |**Suscripción**|Si tiene más de una suscripción, en la lista de suscripciones de Azure seleccione una de entre aquellas a las que tiene acceso.|
    |**Grupo de recursos**|Seleccione el recurso nuevo o existente. Un grupo de recursos es una colección de recursos que comparten ciclos de vida, permisos y directivas. Obtenga más información [aquí](../../../azure-resource-manager/management/overview.md#resource-groups).|
    |**Ubicación**|Seleccione la región geográfica que se usará para almacenar los registros de elementos multimedia y de metadatos para la cuenta de Media Services. Esta región se utilizará para procesar y transmitir contenido multimedia. Solo las regiones de Media Services disponibles aparecen en la lista desplegable. |
    |**Storage Account**|Seleccione una cuenta de almacenamiento para proporcionar almacenamiento de blobs del contenido multimedia desde la cuenta de Media Services. Puede seleccionar una cuenta de almacenamiento existente en la misma región geográfica que la cuenta de Media Services o crear una nueva cuenta de almacenamiento. Se crea una nueva cuenta de almacenamiento en la misma región. Las reglas para los nombres de cuenta de almacenamiento son las mismas que para las cuentas de Media Services.<br/><br/>Debe tener una cuenta de almacenamiento **Principal** y puede tener cualquier número de cuentas de almacenamiento **Secundarias** asociadas a su cuenta de Media Services. Puede usar Azure Portal para agregar cuentas de almacenamiento secundarias. Para más información, consulte [Cuentas de Azure Storage con cuentas de Azure Media Services](../storage-account-concept.md).<br/><br/>La cuenta de Media Services y todas las cuentas de almacenamiento asociadas deben estar en la misma suscripción de Azure. Se recomienda encarecidamente usar cuentas de almacenamiento que se encuentren en la misma ubicación que la cuenta de Media Services para evitar costos adicionales debidos a la latencia y a la salida de datos.|
    |**Configuración avanzada**| Puede crear una cuenta mediante una identidad administrada por el sistema; para ello, seleccione el botón de radio **Administrada por el sistema**.  Al hacer esta selección, podrá usar las claves administradas por el cliente o aportar su propia clave (BYOK) y Media Services para habilitar el almacenamiento de confianza.  Para más información acerca de las claves administradas por el cliente, consulte [Bring Your Own Key (claves administradas por el cliente) con Media Services](../concept-use-customer-managed-keys-byok.md). Además, también se habilitarán las [identidades administradas](../concept-managed-identities.md).

1. Seleccione **Anclar al panel** para ver el progreso de la implementación de la cuenta.
1. Haga clic en **Crear** en la parte inferior del formulario.
