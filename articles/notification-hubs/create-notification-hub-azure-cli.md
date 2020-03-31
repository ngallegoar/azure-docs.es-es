---
title: 'Inicio rápido: Creación de un centro de notificaciones de Azure con la CLI de Azure | Microsoft Docs'
description: En este tutorial, aprenderá a crear un centro de notificaciones de Azure mediante la CLI de Azure.
services: notification-hubs
author: dbradish-microsoft
manager: barbkess
editor: sethmanheim
ms.service: notification-hubs
ms.devlang: azurecli
ms.workload: mobile
ms.topic: quickstart
ms.date: 03/17/2020
ms.author: dbradish
ms.reviewer: sethm
ms.lastreviewed: 03/18/2020
ms.openlocfilehash: 830fd33e19a10ec6472650e3d26fec677b82c3d7
ms.sourcegitcommit: c2065e6f0ee0919d36554116432241760de43ec8
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/26/2020
ms.locfileid: "80069488"
---
# <a name="quickstart-create-an-azure-notification-hub-using-the-azure-cli"></a>Inicio rápido: Creación de un centro de notificaciones de Azure con la CLI de Azure

Azure Notification Hubs proporciona un motor de inserción de escalabilidad horizontal y fácil de usar que le permite enviar notificaciones a cualquier plataforma (iOS, Android, Windows, Kindle, Baidu, etc.) desde cualquier back-end (en la nube o local). Para más información sobre el servicio, consulte [¿Qué es Azure Notification Hubs?](notification-hubs-push-notification-overview.md).

En este inicio rápido, creará un centro de notificaciones mediante la CLI de Azure. La primera sección proporciona los pasos necesarios para crear un espacio de nombres del centro de notificaciones y consultar la información de directivas de acceso para ese espacio de nombres. La segunda sección proporciona los pasos para crear un centro de notificaciones en un espacio de nombres existente.  También aprenderá a crear una directiva de acceso personalizado.

Si no tiene una suscripción a Azure, cree una [cuenta gratuita](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) antes de empezar.

[!INCLUDE [cloud-shell-try-it.md](../../includes/cloud-shell-try-it.md)]

Notification Hubs requiere la versión 2.0.67 o posterior de la CLI de Azure. Ejecute `az --version` para buscar cuál es la versión y las bibliotecas dependientes que están instaladas. Para la instalación o la actualización, consulte [Instalación de la CLI de Azure](/cli/azure/install-azure-cli).

## <a name="prepare-your-environment"></a>Preparación del entorno

1. Inicie sesión.

   Si está usando una instalación local de la CLI, inicie sesión con el comando [az login](/cli/azure/reference-index#az-login).

    ```azurecli-interactive
    az login
    ```

    Siga los pasos que se muestran en el terminal para completar el proceso de autenticación.

2. Instale la extensión de la CLI de Azure.

   Para ejecutar los comandos de la CLI de Azure para los centros de notificaciones, instale la [extensión de la CLI de Azure para Notification Hubs](/cli/azure/ext/notification-hub/notification-hub).  

    ```azurecli-interactive
    az extension add --name notification-hub
   ```

3. Cree un grupo de recursos.

   Los centros de notificaciones de Azure, como todos los demás recursos de Azure, se tienen que implementar dentro de un grupo de recursos. Los grupos de recursos le permiten organizar y administrar los recursos relacionados de Azure.

   Para este inicio rápido, cree un grupo de recursos denominado *spnhubrg* en la ubicación *eastus* con el comando [az group create](/cli/azure/group#az-group-create) siguiente:

   ```azurecli-interactive
   az group create --name spnhubrg --location eastus
   ```

## <a name="create-a-notification-hub-namespace"></a>Creación de un espacio de nombres del centro de notificaciones

1. Cree un espacio de nombres para los centros de notificaciones

   Un espacio de nombres contiene uno o varios centros, además el nombre tiene que ser único dentro de todas las suscripciones de Azure.  Para comprobar la disponibilidad del espacio de nombres de servicio determinado, use el comando [az notification-hub namespace check-availability](/cli/azure/ext/notification-hub/notification-hub/namespace#ext-notification-hub-az-notification-hub-namespace-check-availability).  Ejecute el comando [az notification-hub namespace create](/cli/azure/ext/notification-hub/notification-hub/namespace#ext-notification-hub-az-notification-hub-namespace-create) para crear un espacio de nombres.  

   ```azurecli-interactive
   #check availability
   az notification-hub namespace check-availability --name spnhubns

   #create the namespace
   az notification-hub namespace create --resource-group spnhubrg --name spnhubns  --location eastus --sku Free
   ```

2. Enumere las claves y las cadenas de conexión de su directiva de acceso al espacio de nombres.

   Se crea automáticamente una directiva de acceso denominada **RootManageSharedAccessKey** para un espacio de nombres nuevo.  Cada directiva de acceso tiene dos conjuntos de claves y cadenas de conexión.  Para enumerar las claves y las cadenas de conexión del espacio de nombres, ejecute el comando [az notification-hub namespace authorization-rule list-keys](/cli/azure/ext/notification-hub/notification-hub/authorization-rule#ext-notification-hub-az-notification-hub-authorization-rule-list-keys).

   ```azurecli-interactive
   az notification-hub namespace authorization-rule list-keys --resource-group spnhubrg --namespace-name spnhubns --name RootManageSharedAccessKey
   ```

## <a name="create-notification-hubs"></a>Creación de centros de notificaciones

1. Cree su primer centro de notificaciones.

   Ahora se puede crear un centro de notificaciones en el nuevo espacio de nombres.  Ejecute el comando [az notification-hub create](/cli/azure/ext/notification-hub/notification-hub#ext-notification-hub-az-notification-hub-create) para crear un centro de notificaciones.

   ```azurecli-interactive
   az notification-hub create --resource-group spnhubrg --namespace-name spnhubns --name spfcmtutorial1nhub --location eastus --sku Free
   ```

2. Cree un segundo centro de notificaciones.

   Se pueden crear varios centros de notificaciones en un solo espacio de nombres.  Para crear un segundo centro de notificaciones en el mismo espacio de nombres, vuelva a ejecutar el comando `az notification-hub create` con un nombre de centro diferente.

   ```azurecli-interactive
   az notification-hub create --resource-group spnhubrg --namespace-name spnhubns --name mysecondnhub --location eastus --sku Free
   ```

## <a name="work-with-access-policies"></a>Trabajo con directivas de acceso

1. Cree una nueva regla de autorización para un centro de notificaciones.

   Se crea automáticamente una directiva de acceso para cada nuevo centro de notificaciones.  Para crear y personalizar su propia directiva de acceso, use el comando [az notification-hub authorization-rule create](/cli/azure/ext/notification-hub/notification-hub/authorization-rule#ext-notification-hub-az-notification-hub-authorization-rule-create).

   ```azurecli-interactive
   az notification-hub authorization-rule create --resource-group spnhubrg --namespace-name spnhubns --notification-hub-name spfcmtutorial1nhub --name spnhub1key --rights Listen Send
   ```

2. Enumere las directivas de acceso para un centro de notificaciones.

   Para consultar qué directivas de acceso existen para un centro de notificaciones, use el comando [az notification-hub authorization-rule lis](/cli/azure/ext/notification-hub/notification-hub/authorization-rule#ext-notification-hub-az-notification-hub-authorization-rule-list).

   ```azurecli-interactive
   az notification-hub authorization-rule list --resource-group spnhubrg --namespace-name spnhubns --notification-hub-name spfcmtutorial1nhub --output table
   ```

   > [!IMPORTANT]
   > No use la directiva **DefaultFullSharedAccessSignature** en la aplicación. Está pensada para usarse solamente en el back-end.  Use solo directivas de acceso de **escucha** en la aplicación cliente.

3. Enumere las claves y las cadenas de conexión para una directiva de acceso al centro de notificaciones

   Hay dos conjuntos de claves y cadenas de conexión para cada directiva de acceso.  Las necesitará más adelante para gestionar las notificaciones push.  Para enumerar las claves y las cadenas de conexión para una directiva de acceso al centro de notificaciones, use el comando [az notification-hub authorization-rule list-keys](/cli/azure/ext/notification-hub/notification-hub/authorization-rule#ext-notification-hub-az-notification-hub-authorization-rule-list-keys).

   ```azurecli-interactive
   #query the keys and connection strings for DefaultListenSharedAccessSignature
   az notification-hub authorization-rule list-keys --resource-group spnhubrg --namespace-name spnhubns --notification-hub-name spfcmtutorial1nhub --name DefaultListenSharedAccessSignature --output json

   #query the keys and connection strings for the custom policy
   az notification-hub authorization-rule list-keys --resource-group spnhubrg --namespace-name spnhubns --notification-hub-name spfcmtutorial1nhub --name spnhub1key --output table
   ```

   > [!NOTE]
   > Un [espacio de nombres de centro de notificaciones](/cli/azure/ext/notification-hub/notification-hub/namespace/authorization-rule#ext-notification-hub-az-notification-hub-namespace-authorization-rule-list-keys) y un [centro de notificaciones](/cli/azure/ext/notification-hub/notification-hub/authorization-rule#ext-notification-hub-az-notification-hub-authorization-rule-list-keys) tienen directivas de acceso independientes.  Asegúrese de que está usando la referencia correcta de la CLI de Azure al consultar las claves y las cadenas de conexión.

## <a name="clean-up-resources"></a>Limpieza de recursos

Cuando ya no se necesiten, utilice el comando [az group delete](/cli/azure/group) para quitar el grupo de recursos y todos los recursos relacionados.

```azurecli-interactive
az group delete --name spnhubrg
```

## <a name="see-also"></a>Consulte también

Descubra las funcionalidades completas para la administración de los centros de notificaciones con la CLI de Azure.

* [Lista de referencias completa de la CLI de Azure para Notification Hubs](/cli/azure/ext/notification-hub/notification-hub)
* [Lista de referencias de la CLI de Azure para los espacio de nombres de Notification Hubs](/cli/azure/ext/notification-hub/notification-hub/namespace)
* [Lista de referencias de la CLI de Azure para las reglas de autorización de Notification Hubs](/cli/azure/ext/notification-hub/notification-hub/authorization-rule)
* [Lista de referencias de la CLI de Azure para las credenciales de Notification Hubs](/cli/azure/ext/notification-hub/notification-hub/credential)
