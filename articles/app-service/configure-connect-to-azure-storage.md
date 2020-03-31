---
title: Adición de almacenamiento personalizado (contenedor de Windows)
description: Aprenda a conectar un recurso compartido de red personalizado en un contenedor de Windows personalizado en Azure App Service. Comparta archivos entre aplicaciones, administre el contenido estático de forma remota y acceda a él de forma local, etc.
author: msangapu-msft
ms.topic: article
ms.date: 7/01/2019
ms.author: msangapu
ms.openlocfilehash: 64ef4dfe81e6415f1285a74962e2123507715119
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/27/2020
ms.locfileid: "77120669"
---
# <a name="configure-azure-files-in-a-windows-container-on-app-service"></a>Configuración de Azure Files en un contenedor de Windows en App Service

> [!NOTE]
> Este artículo se aplica a contenedores de Windows personalizados. Para implementar App Service en _Linux_, consulte [Servicio de contenido desde Azure Storage](./containers/how-to-serve-content-from-azure-storage.md).
>

Esta guía muestra cómo acceder a Azure Storage en contenedores de Windows. Solo se admiten [recursos compartidos de archivos de Azure Files](https://docs.microsoft.com/azure/storage/files/storage-how-to-use-files-cli) y [recursos compartidos de archivos Premium](https://docs.microsoft.com/azure/storage/files/storage-how-to-create-premium-fileshare). Puede usar recursos compartidos de archivos de Azure Files en esta guía. Entre las ventajas se incluye protección y portabilidad del contenido, acceso a varias aplicaciones y varios métodos de transferencia.

## <a name="prerequisites"></a>Prerrequisitos

- [CLI de Azure](/cli/azure/install-azure-cli) (2.0.46 o posterior).
- [Una aplicación de contenedor de Windows existente en Azure App Service](https://docs.microsoft.com/azure/app-service/app-service-web-get-started-windows-container)
- [Creación de un recurso compartido de Azure Files](https://docs.microsoft.com/azure/storage/files/storage-how-to-use-files-cli)
- [Carga de archivos en un recurso compartido de Azure Files](https://docs.microsoft.com/azure/storage/files/storage-files-deployment-guide)

> [!NOTE]
> Azure Files es almacenamiento no predeterminado y se factura por separado, es decir, no se incluye con la aplicación web. No admite el uso de una configuración de firewall debido a limitaciones de infraestructura.
>

## <a name="limitations"></a>Limitaciones

- Azure Storage en los contenedores de Windows se encuentra **en versión preliminar** y **no es compatible** con los **escenarios de producción**.
- Azure Storage en los contenedores de Windows solo admite el montaje de **contenedores de Azure Files** (lectura y escritura).
- Actualmente, **no se admite** Azure Storage en los contenedores de Windows para incluir escenarios de código propios en los planes de Windows App Service.
- Azure Storage en los contenedores de Windows **no admite** el uso de la configuración del **firewall de Storage** debido a limitaciones de la infraestructura.
- Azure Storage en los contenedores de Windows le permite especificar **hasta cinco** puntos de montaje por aplicación.
- No se pude acceder a Azure Storage montado en una aplicación mediante los puntos de conexión de FTP/FTP de App Service. Use [Explorador de Azure Storage](https://azure.microsoft.com/features/storage-explorer/).
- Azure Storage se factura por separado y **no se incluye** con la aplicación web. Obtenga más información sobre los [precios de Azure Storage](https://azure.microsoft.com/pricing/details/storage).

## <a name="link-storage-to-your-web-app-preview"></a>Vinculación del almacenamiento a la aplicación web (versión preliminar)

 Para montar un recurso compartido de Azure Files en un directorio en la aplicación de App Service, use el comando [`az webapp config storage-account add`](https://docs.microsoft.com/cli/azure/webapp/config/storage-account?view=azure-cli-latest#az-webapp-config-storage-account-add). El tipo de almacenamiento debe ser AzureFiles.

```azurecli
az webapp config storage-account add --resource-group <group_name> --name <app_name> --custom-id <custom_id> --storage-type AzureFiles --share-name <share_name> --account-name <storage_account_name> --access-key "<access_key>" --mount-path <mount_path_directory of form c:<directory name> >
```

Este paso debe realizarlo con cualquier otro directorio que quiera vincular a un recurso compartido de Azure Files.

## <a name="verify"></a>Verify

Una vez que un recurso compartido de Azure Files se vincula a una aplicación web, puede ejecutar el siguiente comando para comprobarlo:

```azurecli
az webapp config storage-account list --resource-group <resource_group> --name <app_name>
```

## <a name="next-steps"></a>Pasos siguientes

- [Migración de una aplicación de ASP.NET a Azure App Service con un contenedor de Windows (versión preliminar)](app-service-web-tutorial-windows-containers-custom-fonts.md).
