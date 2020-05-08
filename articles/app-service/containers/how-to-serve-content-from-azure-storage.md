---
title: Servicio de contenido desde Azure Storage a contenedores de Linux
description: Aprenda a conectar un recurso compartido de red personalizado a un contenedor de Linux en Azure App Service. Comparta archivos entre aplicaciones, administre el contenido estático de forma remota y acceda a él de forma local, etc.
author: msangapu-msft
ms.topic: article
ms.date: 01/02/2020
ms.author: msangapu
ms.openlocfilehash: 9a5a38ea32d927f50fb9ddbebe3e1c3533e6fcc0
ms.sourcegitcommit: 1895459d1c8a592f03326fcb037007b86e2fd22f
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 05/01/2020
ms.locfileid: "82625330"
---
# <a name="serve-content-from-azure-storage-in-app-service-on-linux"></a>Servicio de contenido desde Azure Storage en App Service en Linux

> [!NOTE]
> Este artículo se aplica a los contenedores de Linux. Para implementar en contenedores de Windows personalizados, consulte [Configuración de Azure Files en un contenedor de Windows en App Service](../configure-connect-to-azure-storage.md). Azure Storage en App Service en Linux es una característica en **vista previa**. Esta característica **no se admite para escenarios de producción**.
>

En esta guía se muestra cómo adjuntar Azure Storage a App Service en Linux. Entre las ventajas se incluye protección y portabilidad del contenido, almacenamiento persistente, acceso a varias aplicaciones y varios métodos de transferencia.

## <a name="prerequisites"></a>Prerrequisitos

- [CLI de Azure](/cli/azure/install-azure-cli) (2.0.46 o posterior).
- Una [aplicación de App Service en Linux](https://docs.microsoft.com/azure/app-service/containers/) existente.
- Una [cuenta de Azure Storage](https://docs.microsoft.com/azure/storage/common/storage-quickstart-create-account?tabs=azure-cli).
- Un [recurso compartido y un directorio de Azure](../../storage/files/storage-how-to-use-files-cli.md).


## <a name="limitations-of-azure-storage-with-app-service"></a>Limitaciones de Azure Storage con App Service

- Azure Storage con App Service está en **versión preliminar** para App Service en Linux y Web App for Containers. **No se admite** para **escenarios de producción**.
- Azure Storage con App Service admite el montaje de **contenedores de Azure Files** (lectura y escritura) y **contenedores de blobs de Azure** (solo lectura)
- Azure Storage con App Service **no admite** el uso de la configuración del **firewall de Storage** debido a limitaciones de la infraestructura.
- Azure Storage con App Service le permite especificar **hasta cinco** puntos de montaje por aplicación.
- No se pude acceder a Azure Storage montado en una aplicación mediante los puntos de conexión de FTP/FTP de App Service. Use [Explorador de Azure Storage](https://azure.microsoft.com/features/storage-explorer/).
- Azure Storage **no se incluye** con la aplicación web y se factura por separado. Obtenga más información sobre los [precios de Azure Storage](https://azure.microsoft.com/pricing/details/storage).

> [!WARNING]
> Las configuraciones de App Service que usan Azure Blob Storage serán de solo lectura a partir de febrero de 2020. [Más información](https://github.com/Azure/app-service-linux-docs/blob/master/BringYourOwnStorage/mounting_azure_blob.md)
>

## <a name="configure-your-app-with-azure-storage"></a>Configuración de la aplicación con Azure Storage

Una vez que haya creado la [cuenta de Azure Storage, el recurso compartido y el directorio](#prerequisites), ahora puede configurar la aplicación con Azure Storage.

Para montar una cuenta de almacenamiento en un directorio en la aplicación de App Service, use el comando [`az webapp config storage-account add`](https://docs.microsoft.com/cli/azure/webapp/config/storage-account?view=azure-cli-latest#az-webapp-config-storage-account-add). El tipo de almacenamiento puede ser AzureBlob o AzureFiles. AzureFiles se usa en este ejemplo. La configuración de la ruta de acceso de montaje corresponde a la carpeta que desea montar desde Azure Storage. Al establecerla en '/' se monta la instancia completa de Azure Storage.


> [!CAUTION]
> El directorio especificado como ruta de acceso de montaje en la aplicación web debe estar vacío. Cualquier contenido almacenado en este directorio se eliminará cuando se agregue un montaje externo. Si va a migrar archivos de una aplicación existente, realice una copia de seguridad de la aplicación y de su contenido antes de comenzar.
>

```azurecli
az webapp config storage-account add --resource-group <group_name> --name <app_name> --custom-id <custom_id> --storage-type AzureFiles --share-name <share_name> --account-name <storage_account_name> --access-key "<access_key>" --mount-path <mount_path_directory>
```

Este paso debe realizarlo con cualquier otro directorio que quiera vincular a una cuenta de almacenamiento.

## <a name="verify-azure-storage-link-to-the-web-app"></a>Comprobación del vínculo de Azure Storage a la aplicación web

Una vez que un contenedor de almacenamiento está vinculado a una aplicación web, puede ejecutar el siguiente comando para comprobarlo:

```azurecli
az webapp config storage-account list --resource-group <resource_group> --name <app_name>
```

## <a name="use-azure-storage-in-docker-compose"></a>Uso de Azure Storage en Docker Compose

Azure Storage se puede montar con aplicaciones de varios contenedores mediante custom-id. Para ver el nombre de custom-id, ejecute [`az webapp config storage-account list --name <app_name> --resource-group <resource_group>`](/cli/azure/webapp/config/storage-account?view=azure-cli-latest#az-webapp-config-storage-account-list).

En el archivo *docker-compose.yml*, asigne la opción `volumes` a `custom-id`. Por ejemplo:

```yaml
wordpress:
  image: wordpress:latest
  volumes:
  - <custom-id>:<path_in_container>
```

## <a name="next-steps"></a>Pasos siguientes

- [Configuración de aplicaciones web en Azure App Service](../configure-common.md)

