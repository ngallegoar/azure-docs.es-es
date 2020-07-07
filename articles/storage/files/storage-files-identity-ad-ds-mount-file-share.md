---
title: Montaje de un recurso compartido de archivos de Azure en una máquina virtual unida a AD DS
description: Obtenga información sobre cómo montar un recurso compartido de archivos en máquinas locales unidas a Active Directory Domain Services.
author: roygara
ms.service: storage
ms.subservice: files
ms.topic: how-to
ms.date: 06/22/2020
ms.author: rogarana
ms.openlocfilehash: 9a8805666e1e162f76cf5fa6f7d828833c573bed
ms.sourcegitcommit: 877491bd46921c11dd478bd25fc718ceee2dcc08
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 07/02/2020
ms.locfileid: "85510440"
---
# <a name="part-four-mount-a-file-share-from-a-domain-joined-vm"></a>Parte 4: Montaje de un recurso compartido de archivos desde una VM unida al dominio

Antes de comenzar este artículo, asegúrese de completar el anterior, [Configuración de los permisos de nivel de archivo y directorio en SMB](storage-files-identity-ad-ds-configure-permissions.md).

El proceso descrito en este artículo comprueba que el recurso compartido de archivos y los permisos de acceso se han configurado correctamente y que puede acceder a un recurso compartido de archivos de Azure desde una máquina virtual unida a un dominio. La asignación de roles RBAC de nivel de recurso compartido puede tardar un tiempo en surtir efecto. 

Inicie sesión en el cliente con las credenciales a las que ha concedido permisos, como se muestra en la imagen siguiente.

![Captura de pantalla que muestra la pantalla Inicio de sesión de Azure AD para la autenticación de usuario](media/storage-files-aad-permissions-and-mounting/azure-active-directory-authentication-dialog.png)

## <a name="mounting-prerequisites"></a>Requisitos previos de montaje

Antes de poder montar el recurso compartido de archivos, asegúrese de que ha superado los requisitos previos siguientes:

- Si va a montar el recurso compartido de archivos desde un cliente que lo haya montado previamente con la clave de la cuenta de almacenamiento, asegúrese de que ha desconectado el recurso compartido, que ha quitado las credenciales persistentes de la clave de la cuenta de almacenamiento y que actualmente usa credenciales de AD DS para la autenticación.
- El cliente debe tener una línea de visión en la instancia de AD DS. Si el equipo o la máquina virtual está fuera de la red administrada por AD DS, tendrá que habilitar la VPN a fin de acceder a AD DS para la autenticación.

Reemplace los valores de marcador de posición con valores propios y, después, use el comando siguiente para montar el recurso compartido de archivos de Azure:

```cli
# Always mount your share using.file.core.windows.net, even if you setup a private endpoint for your share.
net use <desired-drive-letter>: \\<storage-account-name>.file.core.windows.net\<share-name>
```

Si tiene problemas de montaje con las credenciales de AD DS, consulte [No se puede montar Azure Files con las credenciales de AD](storage-troubleshoot-windows-file-connection-problems.md#unable-to-mount-azure-files-with-ad-credentials) para obtener instrucciones.

Si el recurso compartido de archivos se ha montado correctamente, ha habilitado y configurado sin problemas la autenticación de AD DS local para los recursos compartidos de archivos de Azure.

## <a name="next-steps"></a>Pasos siguientes

Si la identidad que ha creado en AD DS para representar la cuenta de almacenamiento se encuentra en un dominio o una unidad organizativa que aplica la rotación de contraseñas, continúe con el artículo siguiente para obtener instrucciones sobre cómo actualizar la contraseña:

[Actualice la contraseña de la identidad de la cuenta de almacenamiento en AD DS.](storage-files-identity-ad-ds-update-password.md)