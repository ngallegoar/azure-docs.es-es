---
title: Claves administradas por el cliente o Bring Your Own Key (BYOK) con Media Services
description: Este es un tutorial sobre el uso de claves administradas por el cliente con una cuenta de almacenamiento de Media Services.
author: IngridAtMicrosoft
ms.author: inhenkel
ms.service: media-services
ms.topic: tutorial
ms.date: 10/18/2020
ms.openlocfilehash: c26da9d888bbcdf72c052fa4bd7fcdf443a2d8f7
ms.sourcegitcommit: ce8eecb3e966c08ae368fafb69eaeb00e76da57e
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/21/2020
ms.locfileid: "92325893"
---
# <a name="tutorial-use-customer-managed-keys-or-bring-your-own-key-byok-with-media-services"></a>Tutorial: Uso de claves administradas por el cliente o Bring Your Own Key (BYOK) con Media Services

Con la API 2020-05-01, puede usar una clave RSA administrada por el cliente con una cuenta de Media Services que tenga una identidad administrada por el sistema.  En este tutorial se incluyen una colección y un entorno de Postman para enviar solicitudes REST a los servicios de Azure.  Servicios utilizados:

- Registro de aplicación de Azure Active Directory para Postman
- Microsoft Graph API
- Almacenamiento de Azure
- Azure Key Vault
- Media Services

En este tutorial, aprenderá a usar Postman para:

> [!div class="checklist"]
> * Obtener tokens para su uso con los servicios de Azure.
> * Crear un grupo de recursos y una cuenta de almacenamiento.
> * Crear una cuenta de Media Services con una identidad administrada por el sistema.
> * Crear una instancia de Key Vault para almacenar una clave RSA (administrada por el cliente) para su uso con la cuenta de almacenamiento.
> * Actualizar la cuenta de Media Services para usar la clave RSA con la cuenta de almacenamiento.
> * Usar variables en Postman.

Si no tiene ninguna suscripción a Azure, [cree una cuenta de evaluación gratuita](https://azure.microsoft.com/free/).

## <a name="prerequisites"></a>Requisitos previos

- Registre una entidad de servicio con los permisos adecuados.
- Instale [Postman](https://www.postman.com).
- Descargue la colección de Postman para este tutorial en [Ejemplos de Azure: media-services-customer-managed-keys-byok](https://github.com/Azure-Samples/media-services-customer-managed-keys-byok).

### <a name="register-a-service-principal-with-the-needed-permissions"></a>Registro de una entidad de servicio con los permisos necesarios

[Cree una entidad de servicio](https://docs.microsoft.com/azure/active-directory/develop/howto-create-service-principal-portal).  Consulte la [opción dos](https://docs.microsoft.com/azure/active-directory/develop/howto-create-service-principal-portal#authentication-two-options) para obtener el secreto de la entidad de servicio.  Anote el secreto en algún lugar, ya que después de salir de la página del secreto en el portal no será accesible.

La entidad de servicio necesita los siguientes permisos para realizar las tareas de este tutorial.

![Permisos necesarios para la entidad de servicio](./media/tutorial-byok/service-principal-permissions-1.png)

### <a name="install-postman"></a>Instalación de Postman

Si aún no ha instalado Postman para utilizarlo con Azure, puede obtenerlo en [postman.com](https://www.postman.com/).

### <a name="download-and-import-the-collection"></a>Descarga e importación de la colección

Descargue la colección de Postman para este tutorial en [Ejemplos de Azure: media-services-customer-managed-keys-byok](https://github.com/Azure-Samples/media-services-customer-managed-keys-byok).

## <a name="installation-of-collection-and-environment"></a>Instalación de la colección y el entorno

1. Ejecute Postman.
1. Seleccione **Import** (Importar).
1. Seleccione **Upload files** (Cargar archivos).
1. Vaya a la ubicación donde guardó los archivos de la colección y del entorno.
1. Seleccione el archivo de entorno y la colección.
1. Seleccione **Abrir** .  (Verá una advertencia que indica que los archivos no se importarán como una API, sino como colecciones.  Esto es correcto.  Es lo que desea).
1. Esta colección se mostrará ahora en las colecciones como BYOK.
1. Las variables de entorno aparecerán ahora en sus entornos.

### <a name="understand-the-rest-api-requests-in-the-collection"></a>Descripción de las solicitudes de API REST de la colección

La colección proporciona las siguientes solicitudes de API REST. Las solicitudes se deben enviar en la secuencia proporcionada, ya que la mayoría de ellas tienen scripts de prueba que crean dinámicamente variables globales para la siguiente solicitud (o siguientes) de la secuencia. No es necesario crear manualmente las variables globales.

En Postman, verá estas variables contenidas entre corchetes `{{ }}`.  Por ejemplo, `{{bearerToken}}`.

1. Obtener token de AAD: la prueba establece la variable global *bearerToken* .
2. Obtener token de Graph: la prueba establece la variable global *graphToken* .
3. Obtener detalles de la entidad de servicio: la prueba establece la variable global *servicePrincipalObjectId* .
4. Crear una cuenta de almacenamiento: la prueba establece la variable global *storageAccountId* .
5. Crear una cuenta de Media Services con una identidad administrada por el sistema: la prueba establece la variable global *principalId* .
6. Crear una instancia de Key Vault y conceder acceso a la entidad de servicio: la prueba establece la variable global *keyVaultId* .
7. Obtener un token de Key Vault: la prueba establece la variable global *keyVaultToken* .
8. Crear una clave RSA en el almacén de claves: la prueba establece la variable global *keyId* .
9. Actualice la cuenta de Media Services para usar la clave con la cuenta de almacenamiento: no hay un script de prueba para esta solicitud.

## <a name="define-environment-variables"></a>Definición de variables de entorno

1. Cambie al entorno que descargó; para ello, seleccione la lista desplegable de entornos.
1. Establezca las variables de entorno en Postman. También se usan como variables contenidas entre corchetes `{{ }}`.  Por ejemplo, `{{tenantId}}`.

    * *tenantId* = identificador del inquilino
    * *servicePrincipalId* = identificador de la entidad de servicio que estableció con su método favorito: el portal, la CLI, etc.
    * *servicePrincipalSecret* = secreto creado para la entidad de servicio
    * *subscription* = identificador de la suscripción
    * *storageName* = nombre que desea dar al almacenamiento
    * *accountName* = nombre de la cuenta de Media Services que quiere usar
    * *keyVaultName* = nombre del almacén de claves que desea usar
    * *resourceLocation* = centralus (o la ubicación donde desea colocar los recursos.  Esta colección solo se ha probado con centralus).
    * *resourceGroup* = nombre del grupo de recursos

    Las siguientes variables son estándar para trabajar con recursos de Azure, por lo que no es necesario cambiarlas.

    * *armResource* = `https://management.core.windows.net`
    * *graphResource* = `https://graph.windows.net/`
    * *keyVaultResource* = `https://vault.azure.net`
    * *armEndpoint* = `management.azure.com`
    * *graphEndpoint* = `graph.windows.net`
    * *aadEndpoint* = `login.microsoftonline.com`
    * *keyVaultDomainSuffix* = `vault.azure.net`

## <a name="send-the-requests"></a>Envío de las solicitudes

Una vez definidas las variables de entorno, puede ejecutar las solicitudes de una en una en la secuencia anterior o bien usar el ejecutor de Postman para ejecutar la colección.

## <a name="change-the-key"></a>Cambio de la clave

Media Services detectará automáticamente cuando se haya cambiado la clave.  Para probarlo, cree otra versión de clave para la misma clave. Media Services debe detectar esta clave en menos de 15 minutos.

## <a name="clean-up-resources"></a>Limpieza de recursos

Si no va a seguir usando los recursos que ha creado y **no desea que se le siga facturando** , elimínelos.

## <a name="next-steps"></a>Pasos siguientes

Avance al siguiente artículo para obtener información sobre...
> [!div class="nextstepaction"]
> [Tutorial: Codificación de un archivo remoto según una dirección URL y transmisión del vídeo: REST](stream-files-tutorial-with-rest.md)
