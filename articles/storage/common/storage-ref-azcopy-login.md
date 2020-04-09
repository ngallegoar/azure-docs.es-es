---
title: azcopy login | Microsoft Docs
description: En este artículo se proporciona información de referencia del comando azcopy login.
author: normesta
ms.service: storage
ms.topic: reference
ms.date: 10/16/2019
ms.author: normesta
ms.subservice: common
ms.reviewer: zezha-msft
ms.openlocfilehash: d07d1a706635a7f269a9a51769ae6f8bbf57df3d
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/28/2020
ms.locfileid: "80295403"
---
# <a name="azcopy-login"></a>azcopy login

Inicia sesión en Azure Active Directory para acceder a recursos de Azure Storage.

## <a name="synopsis"></a>Sinopsis

Inicie sesión en Azure Active Directory para acceder a recursos de Azure Storage.

Para que se le autorice en su cuenta de Azure Storage, debe asignar el rol **Colaborador de datos de blobs de almacenamiento** a su cuenta de usuario en el contexto de la cuenta de almacenamiento, el grupo de recursos primario o la suscripción primaria.

Este comando almacenará en caché la información de inicio de sesión cifrada para el usuario actual que está utilizando los mecanismos integrados del sistema operativo.

Consulte los ejemplos para más información.

> [!IMPORTANT]
> Si establece una variable de entorno mediante la línea de comandos, esa variable se podrá leer en el historial de la línea de comandos. Considere la posibilidad de borrar las variables que contengan credenciales del historial de la línea de comandos. Para evitar que aparezcan variables en el historial, puede usar un script para pedir al usuario sus credenciales y establecer la variable de entorno.

```azcopy
azcopy login [flags]
```

## <a name="related-conceptual-articles"></a>Artículos conceptuales relacionados

- [Introducción a AzCopy](storage-use-azcopy-v10.md)
- [Transferencia de datos con AzCopy y Blob Storage](storage-use-azcopy-blobs.md)
- [Transferencia de datos con AzCopy y File Storage](storage-use-azcopy-files.md)
- [Configurar, optimizar y solucionar problemas de AzCopy](storage-use-azcopy-configure.md)

## <a name="examples"></a>Ejemplos

Inicie sesión de forma interactiva con el identificador de inquilino de AAD predeterminado establecido en "common" (común):

```azcopy
azcopy login
```

Inicie sesión de forma interactiva con un identificador de inquilino especificado:

```azcopy
azcopy login --tenant-id "[TenantID]"
```

Inicie sesión con la identidad asignada por el sistema de una máquina virtual (VM):

```azcopy
azcopy login --identity
```

Inicie sesión con la identidad asignada por el usuario de una máquina virtual y un identificador de cliente de la identidad de servicio:

```azcopy
azcopy login --identity --identity-client-id "[ServiceIdentityClientID]"
```

Inicie sesión con la identidad asignada por el usuario de una máquina virtual y un identificador de objeto de la identidad de servicio:

```azcopy
azcopy login --identity --identity-object-id "[ServiceIdentityObjectID]"
```

Inicie sesión con la identidad asignada por el usuario de una máquina virtual y un identificador de recurso de la identidad de servicio:

```azcopy
azcopy login --identity --identity-resource-id "/subscriptions/<subscriptionId>/resourcegroups/myRG/providers/Microsoft.ManagedIdentity/userAssignedIdentities/myID"
```

Inicie sesión como una entidad de servicio mediante un secreto de cliente. Establezca la variable de entorno AZCOPY_SPA_CLIENT_SECRET en el secreto de cliente para la autenticación de entidades de servicio basada en secretos.

```azcopy
azcopy login --service-principal
```

Inicie sesión como una entidad de servicio mediante un certificado y una contraseña. Establezca la variable de entorno AZCOPY_SPA_CERT_PASSWORD en la contraseña del certificado para la autorización de entidades de servicio basada en certificados.

```azcopy
azcopy login --service-principal --certificate-path /path/to/my/cert
```

Asegúrese de tratar/path/to/my/cert como una ruta de acceso a un archivo PEM o PKCS12. AzCopy no llega al almacén de certificados del sistema para obtener el certificado.

--certificate-path es obligatorio en la autenticación de entidades de servicio basada en certificados.

## <a name="options"></a>Opciones

|Opción|Descripción|
|--|--|
|--aad-endpoint|El punto de conexión de Azure Active Directory que se usará. El valor predeterminado (`https://login.microsoftonline.com`) es correcto para la nube pública de Azure. Establezca este parámetro al autenticarse en una nube nacional. Consulte [Puntos de conexión de autenticación de Azure AD](https://docs.microsoft.com/azure/active-directory/develop/authentication-national-cloud#azure-ad-authentication-endpoints).
Esta marca no es necesaria para Managed Service Identity.|
|--application-id string|Identificador de aplicación de la identidad asignada por el usuario. Se requiere para la autenticación de entidades de servicio.|
|--certificate-path string|Ruta de acceso al certificado para la autenticación de SPN. Se requiere para la autenticación de entidades de servicio basada en certificados.|
|-h, --help|Muestra el contenido de la ayuda para el comando login.|
|--identity|Inicia sesión con la identidad de la máquina virtual, también conocida como Managed Service Identity (MSI).|
|--identity-client-id string|Identificador de cliente de la identidad asignada por el usuario.|
|--identity-object-id string|Identificador de objeto de la identidad asignada por el usuario.|
|--identity-resource-id string|Identificador de recurso de la identidad asignada por el usuario.|
|--service-principal|Inicia sesión a través de SPN (nombre de entidad de seguridad de servicio) con un certificado o un secreto. El secreto de cliente o la contraseña del certificado deben colocarse en la variable de entorno adecuada. Escriba `AzCopy env` para ver los nombres y descripciones de las variables de entorno.|
|--tenant-id string| El identificador de inquilino de Azure Active Directory que se usará para el inicio de sesión interactivo de dispositivos con OAuth.|

## <a name="options-inherited-from-parent-commands"></a>Opciones heredadas de comandos primarios

|Opción|Descripción|
|---|---|
|--cap-mbps uint32|Limita la velocidad de transferencia, en megabits por segundo. El rendimiento en un momento dado puede variar ligeramente del límite. Si esta opción se establece en cero o se omite, el rendimiento no se limita.|
|--output-type string|Formato de la salida del comando. Las opciones incluyen: text, json. El valor predeterminado es "text".|

## <a name="see-also"></a>Consulte también

- [azcopy](storage-ref-azcopy.md)
