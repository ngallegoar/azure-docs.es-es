---
title: Uso de claves administradas por el cliente o API REST BYOK
description: En este tutorial, usará claves administradas por el cliente o Bring Your Own Key (BYOK) con una cuenta de almacenamiento de Azure Media Services.
author: IngridAtMicrosoft
ms.author: inhenkel
ms.service: media-services
ms.topic: tutorial
ms.date: 10/18/2020
ms.openlocfilehash: 93bbd84c25f02ac1653a46ebb4a70c1dfa90c744
ms.sourcegitcommit: c157b830430f9937a7fa7a3a6666dcb66caa338b
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 11/17/2020
ms.locfileid: "94686393"
---
# <a name="tutorial-use-customer-managed-keys-or-byok-with-media-services-rest-api"></a>Tutorial: Uso de claves administradas por el cliente o BYOK con la API REST de Media Services

Con la API 2020-05-01, puede usar una clave RSA administrada por el cliente con una cuenta Azure Media Services que tenga una identidad administrada por el sistema. En este tutorial se incluye una colección y un entorno de Postman para enviar solicitudes REST a los servicios de Azure. Los servicios utilizados son:

- registro de aplicaciones de Azure Active Directory (Azure AD) para Postman
- Microsoft Graph API
- Azure Storage
- Azure Key Vault
- Azure Media Services

En este tutorial, aprenderá a usar Postman para:

> [!div class="checklist"]
> - Obtener tokens para su uso con los servicios de Azure.
> - Crear un grupo de recursos y una cuenta de almacenamiento.
> - Crear una cuenta de Media Services con una identidad administrada por el sistema.
> - Crear un almacén de claves para almacenar una clave RSA administrada por el cliente.
> - Actualizar la cuenta de Media Services para usar la clave RSA con la cuenta de almacenamiento.
> - Usar variables en Postman.

Si no tiene ninguna suscripción a Azure, [cree una cuenta de evaluación gratuita](https://azure.microsoft.com/free/).

## <a name="prerequisites"></a>Requisitos previos

1. Registre una entidad de servicio con los permisos adecuados.
1. Instale [Postman](https://www.postman.com).
1. Descargue la colección de Postman de este tutorial en [Azure Samples: media-services-customer-managed-keys-byok](https://github.com/Azure-Samples/media-services-customer-managed-keys-byok).

### <a name="register-a-service-principal-with-the-needed-permissions"></a>Registro de una entidad de servicio con los permisos necesarios

1. [Cree una entidad de servicio](https://docs.microsoft.com/azure/active-directory/develop/howto-create-service-principal-portal).
1. Vaya a [Opción 2: Creación de un secreto de aplicación](https://docs.microsoft.com/azure/active-directory/develop/howto-create-service-principal-portal#authentication-two-options) para obtener el secreto de la entidad de servicio.

   > [!IMPORTANT]
   >Copie y guarde el secreto para usarlo más tarde. Después de salir de la página del secreto en el portal, no se podrá acceder al secreto.

1. Asigne permisos a la entidad de servicio, tal como se muestra en la siguiente captura de pantalla:

   :::image type="complex" source="./media/tutorial-byok/service-principal-permissions-1.png" alt-text="Captura de pantalla que muestra los permisos necesarios para la entidad de servicio.":::
   Los permisos se enumeran por servicio, nombre de permiso, tipo y, luego, descripción. Azure Key Vault: suplantación de usuarios, delegado, acceso completo a Azure Key Vault. Administración de servicios de Azure: suplantación de usuarios, delegado, acceso a Azure Service Management como usuario de la organización. Azure Storage: suplantación de usuarios, delegado, acceso a Azure Storage. Media Services: suplantación de usuarios, delegado, acceso a Media Services. Microsoft Graph: user.read, delegado, inicio de sesión y lectura de perfiles de usuario.
   :::image-end:::

### <a name="install-postman"></a>Instalación de Postman

Si aún no ha instalado Postman para utilizarlo con Azure, puede obtenerlo en [postman.com](https://www.postman.com/).

### <a name="download-and-import-the-collection"></a>Descarga e importación de la colección

Descargue la colección de Postman de este tutorial en [Azure Samples: media-services-customer-managed-keys-byok](https://github.com/Azure-Samples/media-services-customer-managed-keys-byok).

## <a name="install-the-postman-collection-and-environment"></a>Instalación de la colección y del entorno de Postman

1. Ejecute Postman.
1. Seleccione **Import** (Importar).
1. Seleccione **Upload files** (Cargar archivos).
1. Vaya a la ubicación donde guardó los archivos de la colección y del entorno.
1. Seleccione los archivos de la colección y del entorno.
1. Seleccione **Abrir**. Aparece una advertencia que indica que los archivos no se importarán como API, sino como colecciones. Esta advertencia es normal. Es lo que desea.

La colección ahora se muestra en las colecciones como BYOK. Además, las variables de entorno aparecerán en sus entornos.

### <a name="understand-the-rest-api-requests-in-the-collection"></a>Descripción de las solicitudes de API REST de la colección

La colección proporciona las siguientes solicitudes de API REST.

> [!NOTE]
>
>- Las solicitudes se deben enviar en la secuencia proporcionada.
>- La mayoría de las solicitudes tienen scripts de prueba que crean dinámicamente variables globales para la siguiente solicitud de la secuencia.
>- No es necesario crear manualmente variables globales.

En Postman, verá estas variables incluidas entre corchetes. Por ejemplo, `{{bearerToken}}`.

1. Obtener un token de Azure AD: la prueba establece la variable global **bearerToken**.
2. Obtener un token de Microsoft Graph: la prueba establece la variable global **graphToken**.
3. Obtener los detalles de la entidad de servicio: la prueba establece la variable global **servicePrincipalObjectId**.
4. Cree una cuenta de almacenamiento: la prueba establece la variable global **storageAccountId**.
5. Crear una cuenta de Media Services con una identidad administrada por el sistema: la prueba establece la variable global **principalId**.
6. Crear un almacén de claves para conceder acceso a la entidad de servicio: la prueba establece la variable global **keyVaultId**.
7. Obtener un token de Key Vault: la prueba establece la variable global **keyVaultToken**.
8. Crear la clave RSA en el almacén de claves: la prueba establece la variable global **keyId**.
9. Actualizar la cuenta de Media Services para usar la clave con la cuenta de almacenamiento: no hay ningún script de prueba para esta solicitud.

## <a name="define-environment-variables"></a>Definición de variables de entorno

1. Seleccione la lista desplegable del entorno para cambiar al entorno que descargó.
1. Establezca las variables de entorno en Postman. También se usan como variables incluidas entre corchetes. Por ejemplo, `{{tenantId}}`.

    - **tenantId**: El identificador de inquilino.
    - **servicePrincipalId**: el identificador de la entidad de servicio que se establece con el método favorito, como el portal o la CLI.
    - **servicePrincipalSecret**: el secreto creado para la entidad de servicio.
    - **Suscripción**: El identificador de suscripción.
    - **storageName**: el nombre que quiere dar a su almacenamiento.
    - **accountName**: el nombre de la cuenta de Media Services que quiere usar.
    - **keyVaultName**: el nombre del almacén de claves que quiere usar.
    - **resourceLocation**: la ubicación **CentralUs** o donde quiera colocar los recursos. Esta colección solo se ha probado con **CentralUs**.
    - **resourceGroup**: El nombre del grupo de recursos.

    Las siguientes variables son estándar para trabajar con recursos de Azure. Por lo tanto, no es necesario cambiarlas.

    - **armResource**: `https://management.core.windows.net`
    - **graphResource**: `https://graph.windows.net/`
    - **keyVaultResource**: `https://vault.azure.net`
    - **armEndpoint**: `management.azure.com`
    - **graphEndpoint**: `graph.windows.net`
    - **aadEndpoint**: `login.microsoftonline.com`
    - **keyVaultDomainSuffix**: `vault.azure.net`

## <a name="send-the-requests"></a>Envío de las solicitudes

Después de definir las variables de entorno, puede ejecutar las solicitudes de una en una en la secuencia anterior. O bien, puede usar el ejecutor de Postman para ejecutar la colección.

## <a name="change-the-key"></a>Cambio de la clave

Media Services detecta automáticamente cuándo se cambia la clave. Para probar este proceso, cree otra versión de clave para la misma clave. Media Services debe detectar esta clave en menos de 15 minutos.

## <a name="clean-up-resources"></a>Limpieza de recursos

Si no va a seguir usando los recursos que ha creado y *no desea que se le siga facturando*, elimínelos.

## <a name="next-steps"></a>Pasos siguientes

Avance al siguiente artículo para obtener información sobre:
> [!div class="nextstepaction"]
> [Tutorial: Codificación de un archivo remoto según una dirección URL y transmisión del vídeo: REST](stream-files-tutorial-with-rest.md)
