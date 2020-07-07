---
title: Actualización de la contraseña de la cuenta de almacenamiento de AD DS
description: Obtenga información sobre cómo actualizar la contraseña de la cuenta de Active Directory Domain Services que representa la cuenta de almacenamiento. Esto impedirá que la cuenta de almacenamiento se limpie cuando expire la contraseña, con lo que se evitarán errores de autenticación.
author: roygara
ms.service: storage
ms.subservice: files
ms.topic: how-to
ms.date: 06/22/2020
ms.author: rogarana
ms.openlocfilehash: d16f11a85c6b370b0187975cce965bf3e1b5ba3f
ms.sourcegitcommit: 877491bd46921c11dd478bd25fc718ceee2dcc08
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 07/02/2020
ms.locfileid: "85510261"
---
# <a name="update-the-password-of-your-storage-account-identity-in-ad-ds"></a>Actualización de la contraseña de la identidad de la cuenta de almacenamiento en AD DS

Si registró la cuenta/identidad de Active Directory Domain Services (AD DS) que representa la cuenta de almacenamiento en una unidad organizativa o un dominio que aplica el tiempo de expiración de la contraseña, debe cambiar la contraseña antes de la vigencia máxima de la contraseña. La organización puede ejecutar scripts de limpieza automatizada que eliminen cuentas una vez que expire su contraseña. Por este motivo, si no cambia la contraseña antes de que expire, podría eliminarse la cuenta, lo que hará que pierda el acceso a los recursos compartidos de archivos de Azure.

Para desencadenar la rotación de contraseñas, puede ejecutar el comando `Update-AzStorageAccountADObjectPassword` desde el [módulo AzFilesHybrid](https://github.com/Azure-Samples/azure-files-samples/releases). Este comando debe ejecutarse en un entorno local unido a AD DS mediante un usuario híbrido con permiso de propietario en la cuenta de almacenamiento y permisos de AD DS para cambiar la contraseña de la identidad que representa la cuenta de almacenamiento. El comando realiza acciones similares a la rotación de claves de la cuenta de almacenamiento. Concretamente, obtiene la segunda clave Kerberos de la cuenta de almacenamiento y la usa para actualizar la contraseña de la cuenta registrada en AD DS. Luego, vuelve a generar la clave Kerberos de destino de la cuenta de almacenamiento y actualiza la contraseña de la cuenta registrada en AD DS. Este comando se debe ejecutar en un entorno local unido a AD DS.

```PowerShell
# Update the password of the AD DS account registered for the storage account
# You may use either kerb1 or kerb2
Update-AzStorageAccountADObjectPassword `
        -RotateToKerbKey kerb2 `
        -ResourceGroupName "<your-resource-group-name-here>" `
        -StorageAccountName "<your-storage-account-name-here>"
```
