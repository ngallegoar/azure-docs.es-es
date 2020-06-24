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
ms.date: 05/27/2020
ms.author: dbradish
ms.reviewer: sethm
ms.lastreviewed: 03/18/2020
ms.openlocfilehash: 3014a66c633a4293de8cd6eb325e962366c103b9
ms.sourcegitcommit: 6fd28c1e5cf6872fb28691c7dd307a5e4bc71228
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 06/23/2020
ms.locfileid: "85208270"
---
# <a name="quickstart-create-an-azure-notification-hub-using-the-azure-cli"></a>Inicio rápido: Creación de un centro de notificaciones de Azure con la CLI de Azure

Azure Notification Hubs proporciona un motor de inserción de escalabilidad horizontal y fácil de usar que le permite enviar notificaciones a cualquier plataforma (iOS, Android, Windows, Kindle, Baidu, etc.) desde cualquier back-end (en la nube o local). Para más información sobre el servicio, consulte [¿Qué es Azure Notification Hubs?](notification-hubs-push-notification-overview.md).

En este inicio rápido, creará un centro de notificaciones mediante la CLI de Azure. La primera sección proporciona los pasos para crear un espacio de nombres de un centro de notificaciones.  La segunda sección proporciona los pasos para crear un centro de notificaciones en un espacio de nombres existente.  También aprenderá a crear una directiva de acceso personalizado.  

Si no tiene una suscripción a Azure, cree una [cuenta gratuita](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) antes de empezar.

[!INCLUDE [cloud-shell-try-it.md](../../includes/cloud-shell-try-it.md)]

Notification Hubs requiere la versión 2.0.67 o posterior de la CLI de Azure. Ejecute `az --version` para buscar cuál es la versión y las bibliotecas dependientes que están instaladas. Para la instalación o la actualización, consulte [Instalación de la CLI de Azure](/cli/azure/install-azure-cli).

## <a name="prepare-your-environment"></a>Preparación del entorno

1. Inicie sesión.

   Si está usando una instalación local de la CLI, inicie sesión con el comando [az login](/cli/azure/reference-index#az-login).

    ```azurecli
    az login
    ```

    Siga los pasos que se muestran en el terminal para completar el proceso de autenticación.

2. Instale la extensión de la CLI de Azure.

   Para trabajar con referencias de extensión de la CLI de Azure, primero debe instalar la extensión.  Las extensiones de la CLI de Azure le proporcionan acceso a comandos experimentales y en versión preliminar que todavía no se han enviado como parte de la CLI principal.  Para más información acerca de las extensiones, incluida la actualización y la desinstalación, consulte [Uso de extensiones con la CLI de Azure](/cli/azure/azure-cli-extensions-overview).

   Ejecute el siguiente comando para instalar la [extensión de Notification Hubs](/cli/azure/ext/notification-hub/notification-hub):

    ```azurecli
    az extension add --name notification-hub
   ```

3. Cree un grupo de recursos.

   Azure Notification Hubs, como todos los demás recursos de Azure, se tiene que implementar en un grupo de recursos. Los grupos de recursos le permiten organizar y administrar los recursos relacionados de Azure.

   Para este inicio rápido, cree un grupo de recursos denominado _spnhubrg_ en la ubicación _eastus_ con el comando [az group create](/cli/azure/group#az-group-create) siguiente:

   ```azurecli
   az group create --name spnhubrg --location eastus
   ```

## <a name="create-a-notification-hub-namespace"></a>Creación de un espacio de nombres del centro de notificaciones

1. Cree un espacio de nombres para los centros de notificaciones.

   Un espacio de nombres contiene uno o varios centros y el **nombre tiene que ser único dentro de todas las suscripciones de Azure y tener al menos seis caracteres de longitud**.  Para comprobar la disponibilidad de un nombre, use el comando [az notification-hub namespace check-availability](/cli/azure/ext/notification-hub/notification-hub/namespace#ext-notification-hub-az-notification-hub-namespace-check-availability).

   ```azurecli
   az notification-hub namespace check-availability --name spnhubns
   ```

   La CLI de Azure responderá a la solicitud de disponibilidad mostrando la siguiente salida de la consola:

   ```output
   {
   "id": "/subscriptions/yourSubscriptionID/providers/Microsoft.NotificationHubs/checkNamespaceAvailability",
   "isAvailable": true,
   "location": null,
   "name": "spnhubns",
   "properties": false,
   "sku": null,
   "tags": null,
   "type": "Microsoft.NotificationHubs/namespaces/checkNamespaceAvailability"
   }
   ```

   Observe la segunda línea de la respuesta de la CLI de Azure, `"isAvailable": true`.  Esta línea mostrará `false` si el nombre deseado que especificó para el espacio de nombres está disponible.  Una vez que haya confirmado la disponibilidad del nombre, ejecute el comando [az notification-hub namespace create](/cli/azure/ext/notification-hub/notification-hub/namespace#ext-notification-hub-az-notification-hub-namespace-create) para crear el espacio de nombres.  

   ```azurecli
   az notification-hub namespace create --resource-group spnhubrg --name spnhubns  --location eastus --sku Free
   ```

   Si el `--name` que proporcionó al comando `az notification-hub namespace create` no está disponible o no cumple las [reglas y restricciones de nomenclatura para los recursos de Azure](/azure/azure-resource-manager/management/resource-name-rules), la CLI de Azure responderá con la siguiente salida de la consola:

   ```output
   #the name is not available
   The specified name is not available. For more information visit https://aka.ms/eventhubsarmexceptions.

   #the name is invalied
   The specified service namespace is invalid.
   ```

   Si el nombre que ha intentado no es correcto, seleccione otro nombre para el nuevo espacio de nombres y vuelva a ejecutar el comando `az notification-hub namespace create`.

   > [!NOTE]
   > Desde este paso en adelante, tendrá que reemplazar el valor del parámetro `--namespace` en cada comando de la CLI de Azure que copie de este inicio rápido.

2. Obtenga una lista de espacios de nombres.

   Para ver los detalles sobre el nuevo espacio de nombres, use el comando [az notification-hub namespace list](/cli/azure/ext/notification-hub/notification-hub/namespace?view=azure-cli-latest#ext-notification-hub-az-notification-hub-namespace-list).  El parámetro `--resource-group` es opcional si desea ver todos los espacios de nombres de una suscripción.

   ```azurecli
   az notification-hub namespace list --resource-group spnhubrg
   ```

## <a name="create-notification-hubs"></a>Creación de centros de notificaciones

1. Cree su primer centro de notificaciones.

   Ahora se puede crear uno o varios centros de notificaciones en el nuevo espacio de nombres.  Ejecute el comando [az notification-hub create](/cli/azure/ext/notification-hub/notification-hub#ext-notification-hub-az-notification-hub-create) para crear un centro de notificaciones.

   ```azurecli
   az notification-hub create --resource-group spnhubrg --namespace-name spnhubns --name spfcmtutorial1nhub --location eastus --sku Free
   ```

2. Cree un segundo centro de notificaciones.

   Se pueden crear varios centros de notificaciones en un solo espacio de nombres.  Para crear un segundo centro de notificaciones en el mismo espacio de nombres, vuelva a ejecutar el comando `az notification-hub create` con un nombre de centro diferente.

   ```azurecli
   az notification-hub create --resource-group spnhubrg --namespace-name spnhubns --name mysecondnhub --location eastus --sku Free
   ```

3. Obtiene una lista de centros de notificaciones.

   La CLI de Azure devuelve un mensaje de estado correcto o erróneo con cada comando ejecutado; sin embargo, es posible consultar una lista de los centros de notificaciones, lo cual resulta útil.  El comando [az notification-hub list](/cli/azure/ext/notification-hub/notification-hub?view=azure-cli-latest#ext-notification-hub-az-notification-hub-list) se diseñó para este fin.

   ```azurecli
   az notification-hub list --resource-group spnhubrg --namespace-name spnhubns --output table
   ```

## <a name="work-with-notification-hub-access-policies"></a>Uso de las directivas de acceso del centro de notificaciones

1. Enumere las directivas de acceso para un centro de notificaciones.

   Azure Notification Hubs utiliza [seguridad de firma de acceso compartido](/azure/notification-hubs/notification-hubs-push-notification-security) mediante el uso de directivas de acceso.  Cuando se crea un centro de notificaciones, se crean dos directivas automáticamente.  Las cadenas de conexión de estas directivas son necesarias para configurar las notificaciones de envío.  El comando [az notification-hub authorization-rule list](/cli/azure/ext/notification-hub/notification-hub/authorization-rule#ext-notification-hub-az-notification-hub-authorization-rule-list) proporciona una lista de nombres de directivas y sus grupos de recursos respectivos.

   ```azurecli
   az notification-hub authorization-rule list --resource-group spnhubrg --namespace-name spnhubns --notification-hub-name spfcmtutorial1nhub --output table
   ```

   > [!IMPORTANT]
   > No use la directiva _DefaultFullSharedAccessSignature_ en la aplicación. Está pensada para usarse solamente en el back-end.  Use solo directivas de acceso `Listen` en la aplicación cliente.

2. Cree una nueva regla de autorización para un centro de notificaciones.

   Si desea crear reglas de autorización adicionales con nombres significativos, puede crear y personalizar su propia directiva de acceso mediante el comando [az notification-hub authorization-rule create](/cli/azure/ext/notification-hub/notification-hub/authorization-rule#ext-notification-hub-az-notification-hub-authorization-rule-create).  El parámetro `--rights` es una lista delimitada por espacios de los permisos que desea asignar.

   ```azurecli
   az notification-hub authorization-rule create --resource-group spnhubrg --namespace-name spnhubns --notification-hub-name spfcmtutorial1nhub --name spnhub1key --rights Listen Manage Send
   ```

3. Enumere las claves y las cadenas de conexión para una directiva de acceso al centro de notificaciones

   Hay dos conjuntos de claves y cadenas de conexión para cada directiva de acceso.  Las necesitará más adelante para [configurar un centro de notificaciones](/azure/notification-hubs/configure-notification-hub-portal-pns-settings).  Para enumerar las claves y las cadenas de conexión para una directiva de acceso al centro de notificaciones, use el comando [az notification-hub authorization-rule list-keys](/cli/azure/ext/notification-hub/notification-hub/authorization-rule#ext-notification-hub-az-notification-hub-authorization-rule-list-keys).

   ```azurecli
   #query the keys and connection strings for DefaultListenSharedAccessSignature
   az notification-hub authorization-rule list-keys --resource-group spnhubrg --namespace-name spnhubns --notification-hub-name spfcmtutorial1nhub --name DefaultListenSharedAccessSignature --output table
   ```

   ```azurecli
   #query the keys and connection strings for a custom policy
   az notification-hub authorization-rule list-keys --resource-group spnhubrg --namespace-name spnhubns --notification-hub-name spfcmtutorial1nhub --name spnhub1key --output table
   ```

   > [!NOTE]
   > Un [espacio de nombres de centro de notificaciones](/cli/azure/ext/notification-hub/notification-hub/namespace/authorization-rule#ext-notification-hub-az-notification-hub-namespace-authorization-rule-list-keys) y un [centro de notificaciones](/cli/azure/ext/notification-hub/notification-hub/authorization-rule#ext-notification-hub-az-notification-hub-authorization-rule-list-keys) tienen directivas de acceso independientes.  Asegúrese de que está usando la referencia correcta de la CLI de Azure al consultar las claves y las cadenas de conexión.

## <a name="clean-up-resources"></a>Limpieza de recursos

Cuando ya no se necesiten, utilice el comando [az group delete](/cli/azure/group) para quitar el grupo de recursos y todos los recursos relacionados.

```azurecli
az group delete --name spnhubrg
```

## <a name="next-steps"></a>Pasos siguientes

* En este inicio rápido, ha creado un centro de notificaciones. Para aprender a configurar el centro con la configuración del Sistema de notificación de plataforma (PNS), consulte [Configuración de notificaciones push en un centro de notificaciones](configure-notification-hub-portal-pns-settings.md).

* Descubra las amplias funcionalidades de administración de centros de notificaciones con la CLI de Azure.

  [Lista de referencias completa para Notification Hubs](/cli/azure/ext/notification-hub/notification-hub)

  [Lista de referencias para los espacios de nombres de Notification Hubs](/cli/azure/ext/notification-hub/notification-hub/namespace)

  [Lista de referencias para las reglas de autorización de Notification Hubs](/cli/azure/ext/notification-hub/notification-hub/authorization-rule)

  [Lista de referencias para las credenciales de Notification Hubs](/cli/azure/ext/notification-hub/notification-hub/credential)
