---
title: Uso de la CLI de Azure con archivos y listas de control de acceso (ACL) en Azure Data Lake Storage Gen2 (versión preliminar)
description: Use la CLI de Azure para administrar directorios y listas de control de acceso (ACL) de archivos y directorios en cuentas de almacenamiento que tengan un espacio de nombres jerárquico.
services: storage
author: normesta
ms.service: storage
ms.subservice: data-lake-storage-gen2
ms.topic: conceptual
ms.date: 11/24/2019
ms.author: normesta
ms.reviewer: prishet
ms.openlocfilehash: ce2b4200496938e6cffb935207df8c7027eaf37a
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/27/2020
ms.locfileid: "77486141"
---
# <a name="use-azure-cli-to-manage-directories-files-and-acls-in-azure-data-lake-storage-gen2-preview"></a>Uso de la CLI de Azure para administrar directorios, archivos y ACL en Azure Data Lake Storage Gen2 (versión preliminar)

En este artículo se muestra cómo usar la [Interfaz de la línea de comandos de Azure (CLI)](https://docs.microsoft.com/cli/azure/?view=azure-cli-latest) para crear y administrar directorios, archivos y permisos en cuentas de almacenamiento que tengan un espacio de nombres jerárquico. 

> [!IMPORTANT]
> La extensión `storage-preview` que se incluye en este artículo se encuentra actualmente en versión preliminar pública.

[Muestra](https://github.com/Azure/azure-cli-extensions/tree/master/src/storage-preview#adls-gen2-support) | [Asignación de Gen1 a Gen2](https://github.com/Azure/azure-cli-extensions/tree/master/src/storage-preview#mapping-from-adls-gen1-to-adls-gen2) | [Envíenos sus comentarios](https://github.com/Azure/azure-cli-extensions/issues)
## <a name="prerequisites"></a>Prerequisites

> [!div class="checklist"]
> * Suscripción a Azure. Consulte [Obtención de una versión de evaluación gratuita](https://azure.microsoft.com/pricing/free-trial/).
> * Una cuenta de almacenamiento que tenga habilitado el espacio de nombres jerárquico (HNS). Siga [estas](data-lake-storage-quickstart-create-account.md) instrucciones para crear uno.
> * CLI de Azure versión `2.0.67` o posterior.

## <a name="install-the-storage-cli-extension"></a>Instalación de la extensión de la CLI de Storage

1. Abra [Azure Cloud Shell](https://docs.microsoft.com/azure/cloud-shell/overview?view=azure-cli-latest) o, si ha [instalado](https://docs.microsoft.com/cli/azure/install-azure-cli?view=azure-cli-latest) la CLI de Azure localmente, abra una aplicación de consola de comandos como Windows PowerShell.

2. Use el comando siguiente para verificar que la versión de la CLI de Azure que ha instalado sea `2.0.67` o posterior.

   ```azurecli
    az --version
   ```
   Si la versión de la CLI de Azure es anterior a `2.0.67`, instale una versión posterior. Consulte [Instalación de la CLI de Azure](https://docs.microsoft.com/cli/azure/install-azure-cli?view=azure-cli-latest).

3. Instale la extensión `storage-preview`.

   ```azurecli
   az extension add -n storage-preview
   ```

## <a name="connect-to-the-account"></a>Conexión con la cuenta

1. Si usa la CLI de Azure localmente, ejecute el comando de inicio de sesión.

   ```azurecli
   az login
   ```

   Si la CLI puede abrir el explorador predeterminado, lo hará y cargará una página de inicio de sesión de Azure.

   En caso contrario, abra una página del explorador en [https://aka.ms/devicelogin](https://aka.ms/devicelogin) y escriba el código de autorización que se muestra en el terminal. Inicie sesión con las credenciales de su cuenta en el explorador.

   Para más información sobre los distintos métodos de autenticación, consulte Inicio de sesión con la CLI de Azure.

2. Si su identidad se asocia a más de una suscripción, establezca su suscripción activa en la suscripción de la cuenta de almacenamiento que hospedará el sitio web estático.

   ```azurecli
   az account set --subscription <subscription-id>
   ```

   Reemplace el valor de marcador de posición `<subscription-id>` por el identificador de la suscripción.

## <a name="create-a-file-system"></a>Creación de un sistema de archivos

Un sistema de archivos actúa como contenedor de los archivos. Puede crear uno mediante el comando `az storage container create`. 

En este ejemplo se crea un sistema de archivos llamado `my-file-system`.

```azurecli
az storage container create --name my-file-system --account-name mystorageaccount
```

## <a name="create-a-directory"></a>Creación de un directorio

Cree una referencia de directorio mediante el comando `az storage blob directory create`. 

En este ejemplo se agrega un directorio denominado `my-directory` a un sistema de archivos denominado `my-file-system` que se encuentra en una cuenta denominada `mystorageaccount`.

```azurecli
az storage blob directory create -c my-file-system -d my-directory --account-name mystorageaccount
```

## <a name="show-directory-properties"></a>Visualización de las propiedades de directorio

Puede imprimir las propiedades de un directorio en la consola mediante el comando `az storage blob show`.

```azurecli
az storage blob directory show -c my-file-system -d my-directory --account-name mystorageaccount
```

## <a name="rename-or-move-a-directory"></a>Cambio de nombre o traslado de un directorio

Cambie el nombre de un directorio o muévalo mediante el comando `az storage blob directory move`.

En este ejemplo se cambia el nombre de un subdirectorio de `my-directory` a `my-new-directory`.

```azurecli
az storage blob directory move -c my-file-system -d my-new-directory -s my-directory --account-name mystorageaccount
```

## <a name="delete-a-directory"></a>Eliminación de un directorio

Elimine un directorio mediante el comando `az storage blob directory delete`.

En este ejemplo se elimina un directorio denominado `my-directory`. 

```azurecli
az storage blob directory delete -c my-file-system -d my-directory --account-name mystorageaccount 
```

## <a name="check-if-a-directory-exists"></a>Comprobación de si existe un directorio

Determine si existe un directorio concreto en el sistema de archivos mediante el comando `az storage blob directory exist`.

En este ejemplo se ve si existe un directorio denominado `my-directory` en el sistema de archivos `my-file-system`. 

```azurecli
az storage blob directory exists -c my-file-system -d my-directory --account-name mystorageaccount 
```

## <a name="download-from-a-directory"></a>Descarga de un directorio

Descargue un archivo de un directorio mediante el comando `az storage blob directory download`.

En este ejemplo se descarga un archivo denominado `upload.txt` de un directorio denominado `my-directory`. 

```azurecli
az storage blob directory download -c my-file-system --account-name mystorageaccount -s "my-directory/upload.txt" -d "C:\mylocalfolder\download.txt"
```

En este ejemplo se descarga un directorio completo.

```azurecli
az storage blob directory download -c my-file-system --account-name mystorageaccount -s "my-directory/" -d "C:\mylocalfolder" --recursive
```

## <a name="list-directory-contents"></a>Lista del contenido del directorio

Muestre el contenido de un directorio mediante el comando `az storage blob directory list`.

En este ejemplo se muestra el contenido de un directorio denominado `my-directory` que se encuentra en el sistema de archivos `my-file-system` de una cuenta de almacenamiento denominada `mystorageaccount`. 

```azurecli
az storage blob directory list -c my-file-system -d my-directory --account-name mystorageaccount
```

## <a name="upload-a-file-to-a-directory"></a>Carga de un archivo en un directorio

Cargue un archivo en un directorio mediante el comando `az storage blob directory upload`.

En este ejemplo se carga un archivo denominado `upload.txt` en un directorio denominado `my-directory`. 

```azurecli
az storage blob directory upload -c my-file-system --account-name mystorageaccount -s "C:\mylocaldirectory\upload.txt" -d my-directory
```

En este ejemplo se carga un directorio completo.

```azurecli
az storage blob directory upload -c my-file-system --account-name mystorageaccount -s "C:\mylocaldirectory\" -d my-directory --recursive 
```

## <a name="show-file-properties"></a>Visualización de las propiedades del archivo

Puede imprimir las propiedades de un archivo en la consola mediante el comando `az storage blob show`.

```azurecli
az storage blob show -c my-file-system -b my-file.txt --account-name mystorageaccount
```

## <a name="rename-or-move-a-file"></a>Cambio de nombre o traslado de un archivo

Cambie el nombre de un archivo o muévalo mediante el comando `az storage blob move`.

En este ejemplo se cambia el nombre de un archivo de `my-file.txt` a `my-file-renamed.txt`.

```azurecli
az storage blob move -c my-file-system -d my-file-renamed.txt -s my-file.txt --account-name mystorageaccount
```

## <a name="delete-a-file"></a>Eliminación de un archivo

Elimine un archivo mediante el comando `az storage blob delete`.

En este ejemplo se elimina un archivo denominado `my-file.txt`.

```azurecli
az storage blob delete -c my-file-system -b my-file.txt --account-name mystorageaccount 
```

## <a name="manage-permissions"></a>Administración de permisos

Puede obtener, establecer y actualizar los permisos de acceso de los directorios y archivos.

> [!NOTE]
> Si usa Azure Active Directory (Azure AD) para autorizar comandos, asegúrese de que la entidad de seguridad tiene asignado el [rol Propietario de datos de blobs de almacenamiento](https://docs.microsoft.com/azure/role-based-access-control/built-in-roles#storage-blob-data-owner). Para obtener más información acerca de cómo se aplican los permisos de ACL y las consecuencias de cambiarlos, consulte [Control de acceso en Azure Data Lake Storage Gen2](https://docs.microsoft.com/azure/storage/blobs/data-lake-storage-access-control).

### <a name="get-directory-and-file-permissions"></a>Obtención de permisos de archivo y directorio

Obtenga la ACL de un **directorio** mediante el comando `az storage blob directory access show`.

En este ejemplo se obtiene la ACL de un directorio y luego se imprime la ACL en la consola.

```azurecli
az storage blob directory access show -d my-directory -c my-file-system --account-name mystorageaccount
```

Obtenga los permisos de acceso de un **archivo** mediante el comando `az storage blob access show`. 

En este ejemplo se obtiene la ACL de un archivo y luego se imprime la ACL en la consola.

```azurecli
az storage blob access show -b my-directory/upload.txt -c my-file-system --account-name mystorageaccount
```

En la siguiente imagen se muestra la salida después de obtener la ACL de un directorio.

![Obtención de una salida de ACL](./media/data-lake-storage-directory-file-acl-cli/get-acl.png)

En este ejemplo, el usuario propietario tiene permisos de lectura, escritura y ejecución. El grupo propietario tiene permisos de solo lectura y ejecución. Para más información sobre las listas de control de acceso, vea [Control de acceso en Azure Data Lake Storage Gen2](data-lake-storage-access-control.md).

### <a name="set-directory-and-file-permissions"></a>Establecimiento de permisos de archivo y directorio

Use el comando `az storage blob directory access set` para establecer la ACL de un **directorio**. 

En este ejemplo se establece la ACL en un directorio del usuario propietario, el grupo propietario o de otros usuarios, y luego se imprime la ACL en la consola.

```azurecli
az storage blob directory access set -a "user::rw-,group::rw-,other::-wx" -d my-directory -c my-file-system --account-name mystorageaccount
```

En este ejemplo, se establece la ACL *predeterminada* en un directorio del usuario propietario, el grupo propietario o de otros usuarios y, luego, se imprime la ACL en la consola.

```azurecli
az storage blob directory access set -a "default:user::rw-,group::rw-,other::-wx" -d my-directory -c my-file-system --account-name mystorageaccount
```

Use el comando `az storage blob access set` para establecer la ACL de un **archivo**. 

En este ejemplo se establece la ACL en un archivo del usuario propietario, el grupo propietario o de otros usuarios, y luego se imprime la ACL en la consola.

```azurecli
az storage blob access set -a "user::rw-,group::rw-,other::-wx" -b my-directory/upload.txt -c my-file-system --account-name mystorageaccount
```
En la siguiente imagen se muestra la salida después de establecer la ACL de un archivo.

![Obtención de una salida de ACL](./media/data-lake-storage-directory-file-acl-cli/set-acl-file.png)

En este ejemplo, el usuario propietario y el grupo propietario tienen permisos de solo lectura y escritura. Los demás usuarios tienen permisos de escritura y ejecución. Para más información sobre las listas de control de acceso, vea [Control de acceso en Azure Data Lake Storage Gen2](data-lake-storage-access-control.md).

### <a name="update-directory-and-file-permissions"></a>Actualización de permisos de archivo y directorio

Otra manera de establecer este permiso es usar el comando `az storage blob directory access update` o `az storage blob access update`. 

Actualice la ACL de un directorio o archivo estableciendo el parámetro `-permissions` en el formato abreviado de una ACL.

En este ejemplo se actualiza la ACL de un **directorio**.

```azurecli
az storage blob directory access update --permissions "rwxrwxrwx" -d my-directory -c my-file-system --account-name mystorageaccount
```

En este ejemplo se actualiza la ACL de un **archivo**.

```azurecli
az storage blob access update --permissions "rwxrwxrwx" -b my-directory/upload.txt -c my-file-system --account-name mystorageaccount
```

También puede actualizar el usuario propietario y el grupo de un directorio o archivo estableciendo los parámetros `--owner` o `group` en el identificador de entidad o en el nombre principal de usuario (UPN) de un usuario. 

En este ejemplo se cambia el propietario de un directorio. 

```azurecli
az storage blob directory access update --owner xxxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx -d my-directory -c my-file-system --account-name mystorageaccount
```

En este ejemplo se cambia el propietario de un archivo. 

```azurecli
az storage blob access update --owner xxxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx -b my-directory/upload.txt -c my-file-system --account-name mystorageaccount
```
## <a name="manage-user-defined-metadata"></a>Administración de metadatos definidos por el usuario

Puede agregar metadatos definidos por el usuario a un archivo o un directorio mediante el comando `az storage blob directory metadata update` con uno o más pares nombre-valor.

En este ejemplo se agregan metadatos definidos por el usuario para un directorio denominado `my-directory`.

```azurecli
az storage blob directory metadata update --metadata tag1=value1 tag2=value2 -c my-file-system -d my-directory --account-name mystorageaccount
```

En este ejemplo se muestran todos los metadatos definidos por el usuario para el directorio denominado `my-directory`.

```azurecli
az storage blob directory metadata show -c my-file-system -d my-directory --account-name mystorageaccount
```

## <a name="see-also"></a>Consulte también

* [Ejemplo](https://github.com/Azure/azure-cli-extensions/tree/master/src/storage-preview)
* [Asignación de Gen1 a Gen2](https://github.com/Azure/azure-cli-extensions/tree/master/src/storage-preview#mapping-from-adls-gen1-to-adls-gen2)
* [Envíenos sus comentarios](https://github.com/Azure/azure-cli-extensions/issues)
* [Problemas conocidos](data-lake-storage-known-issues.md#api-scope-data-lake-client-library)
* [Código fuente](https://github.com/Azure/azure-cli-extensions/tree/master/src)

