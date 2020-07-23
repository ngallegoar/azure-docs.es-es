---
title: Configuración de acceso de lectura público anónimo a contenedores y blobs
titleSuffix: Azure Storage
description: Aprenda a habilitar o deshabilitar el acceso anónimo a los datos de blobs en la cuenta de almacenamiento. Establezca la configuración del acceso público a contenedores para que tanto los contenedores como los blobs estén disponibles para el acceso anónimo.
services: storage
author: tamram
ms.service: storage
ms.topic: how-to
ms.date: 06/29/2020
ms.author: tamram
ms.reviewer: fryu
ms.openlocfilehash: af589874021baaf04a423b7bbaa0e36528eda93c
ms.sourcegitcommit: 3541c9cae8a12bdf457f1383e3557eb85a9b3187
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 07/09/2020
ms.locfileid: "86209160"
---
# <a name="configure-anonymous-public-read-access-for-containers-and-blobs"></a>Configuración de acceso de lectura público anónimo a contenedores y blobs

Azure Storage admite el acceso de lectura público anónimo a contenedores y blobs. De forma predeterminada, todas las solicitudes a un contenedor y a sus blobs se deben autorizar mediante Azure Active Directory (Azure AD) o una autorización de clave compartida. Si se configura el nivel de acceso público de un contenedor para permitir el acceso anónimo, los clientes pueden leer los datos de dicho contenedor sin autorizar la solicitud.

> [!WARNING]
> Cuando se configura un contenedor para el acceso público, cualquier cliente puede leer los datos del mismo. El acceso público presenta un riesgo de seguridad potencial, por lo que si el escenario no lo requiere, Microsoft recomienda deshabilitarlo para la cuenta de almacenamiento. Para más información, consulte el artículo en el que se explica cómo [impedir el acceso de lectura público anónimo a contenedores y blobs](anonymous-read-access-prevent.md).

Para configurar el acceso público a un contenedor, es preciso realizar estos dos pasos:

1. Habilitar el acceso público para la cuenta de almacenamiento.
1. Configurar el acceso público del contenedor.

En este artículo se describe cómo configurar el acceso público anónimo de lectura para un contenedor y sus blobs. Para más información sobre cómo acceder a los datos de blobs de forma anónima desde una aplicación cliente, consulte [Acceso anónimo a contenedores y blobs públicos con .NET](anonymous-read-access-client.md).

## <a name="enable-or-disable-public-read-access-for-a-storage-account"></a>Habilitación o deshabilitación del acceso de lectura público a una cuenta de almacenamiento

El acceso público está habilitado en las cuentas de almacenamiento de forma predeterminada. La deshabilitación de este tipo de acceso impide todo acceso anónimo a los contenedores y blobs de esas cuentas. Para mejorar la seguridad, Microsoft recomienda deshabilitar el acceso público para las cuentas de almacenamiento, salvo que el escenario requiera que los usuarios accedan a los recursos de los blobs de forma anónima.

> [!WARNING]
> Cuando se deshabilita el acceso público a una cuenta de almacenamiento, se invalida la configuración de acceso público en todos los contenedores de esa cuenta de almacenamiento. Además, todas las solicitudes anónimas futuras a esa cuenta generarán un error.

Para habilitar o deshabilitar el acceso público en una cuenta de almacenamiento, use Azure Portal o la CLI de Azure para configurar la propiedad **blobPublicAccess** de la cuenta. Esta propiedad está disponible para todas las cuentas de almacenamiento que se crean con el modelo de implementación de Azure Resource Manager. Para más información, consulte [Introducción a las cuentas de almacenamiento](../common/storage-account-overview.md).

# <a name="azure-portal"></a>[Azure Portal](#tab/portal)

Para habilitar o deshabilitar el acceso público a una cuenta de almacenamiento en Azure Portal, siga estos pasos:

1. Vaya a la cuenta de almacenamiento en Azure Portal.
1. Busque la opción **Configuración** en **Configuración**.
1. En **Permitir el acceso público a blobs**, seleccione **Deshabilitado** o **Habilitado**.

    :::image type="content" source="media/anonymous-read-access-configure/blob-public-access-portal.png" alt-text="Captura de pantalla que muestra cómo habilitar o deshabilitar el acceso público a blobs en una cuenta":::

# <a name="azure-cli"></a>[CLI de Azure](#tab/azure-cli)

Para habilitar o deshabilitar el acceso público en una cuenta de almacenamiento con la CLI de Azure, primero obtenga el identificador de recurso de la cuenta de almacenamiento mediante una llamada al comando [az resource show](/cli/azure/resource#az-resource-show). Luego, llame al comando [az resource update](/cli/azure/resource#az-resource-update) para establecer la propiedad **allowBlobPublicAccess** de la cuenta de almacenamiento. Para habilitar el acceso público, establezca la propiedad **allowBlobPublicAccess** en true; para deshabilitarlo, establézcalo en **false**.

En el ejemplo siguiente se deshabilita el acceso público a blobs en la cuenta de almacenamiento. No olvide reemplazar los valores del marcador de posición entre corchetes por sus propios valores:

```azurecli-interactive
storage_account_id=$(az resource show \
    --name anonpublicaccess \
    --resource-group storagesamples-rg \
    --resource-type Microsoft.Storage/storageAccounts \
    --query id \
    --output tsv)

az resource update \
    --ids $storage_account_id \
    --set properties.allowBlobPublicAccess=false
    ```
```

Para comprobar si el acceso público está habilitado con la CLI de Azure, llame al comando [az resource show](/cli/azure/resource#az-resource-show) y consulte la propiedad **allowBlobPublicAccess**:

```azurecli-interactive
az resource show \
    --name <storage-account> \
    --resource-group <resource-group> \
    --resource-type Microsoft.Storage/storageAccounts \
    --query properties.allowBlobPublicAccess \
    --output tsv
```

---

> [!NOTE]
> Deshabilitar el acceso público en una cuenta de almacenamiento no afecta a los sitios web estáticos hospedados en dicha cuenta. Al contenedor **$web** siempre se puede acceder de forma pública.

## <a name="check-the-public-access-setting-for-a-storage-account"></a>Comprobación del valor del acceso público en una cuenta de almacenamiento

Para comprobar el valor del acceso público en una cuenta de almacenamiento, obtenga el valor de la propiedad **allowBlobPublicAccess**. Para comprobar esta propiedad en un número elevado de cuentas de almacenamiento a la vez, use Azure Resource Graph Explorer.

La propiedad **allowBlobPublicAccess** no se establece de forma predeterminada y no devuelve un valor hasta que se establece de forma explícita. El valor predeterminado de la cuenta de almacenamiento es permitir el acceso público cuando el valor de la propiedad es NULL.

### <a name="check-the-public-access-setting-for-a-single-storage-account"></a>Comprobación del valor del acceso público en una cuenta de almacenamiento individual

Para comprobar el valor del acceso público en una cuenta de almacenamiento individual mediante la CLI de Azure, llame al comando [az resource show](/cli/azure/resource#az-resource-show) y consulte la propiedad **allowBlobPublicAccess**:

```azurecli-interactive
az resource show \
    --name <storage-account> \
    --resource-group <resource-group> \
    --resource-type Microsoft.Storage/storageAccounts \
    --query properties.allowBlobPublicAccess \
    --output tsv
```

### <a name="check-the-public-access-setting-for-a-set-of-storage-accounts"></a>Comprobación del valor de acceso público en un conjunto de cuentas de almacenamiento

Para comprobar el valor del acceso público en un conjunto de cuentas de almacenamiento con un rendimiento óptimo, puede usar Azure Resource Graph Explorer en Azure Portal. Para más información sobre el uso de Resource Graph Explorer, consulte [Inicio rápido: Ejecución de la primera consulta de Resource Graph mediante Azure Resource Graph Explorer](/azure/governance/resource-graph/first-query-portal).

Si la siguiente consulta se ejecuta en Resource Graph Explorer, este devuelve una lista de cuentas de almacenamiento y muestra el valor de la propiedad **allowBlobPublicAccess** de cada cuenta:

```kusto
resources
| where type =~ 'Microsoft.Storage/storageAccounts'
| extend allowBlobPublicAccess = parse_json(properties).allowBlobPublicAccess
| project subscriptionId, resourceGroup, name, allowBlobPublicAccess
| order by subscriptionId, resourceGroup, name asc
```

## <a name="set-the-public-access-level-for-a-container"></a>Establecimiento del nivel de acceso público en un contenedor

Para conceder a usuarios anónimos acceso de lectura a un contenedor y sus blobs, primero hay que habilitar el acceso público en la cuenta de almacenamiento y, después, establecer el nivel de acceso público del contenedor. Si el acceso público está deshabilitado en la cuenta de almacenamiento, no podrá configurar un acceso público en un contenedor.

Cuando está habilitado el acceso público para una cuenta de almacenamiento, puede configurar un contenedor con los siguientes permisos:

- **Sin acceso de lectura público:** Solo puede acceder al contenedor y a sus blobs con una solicitud autorizada. Esta opción es el valor predeterminado para todos los contenedores nuevos.
- **Acceso de lectura público solo para blobs:** Los blobs del contenedor los puede leer una solicitud anónima, pero los datos del contenedor no están disponibles de forma anónima. Los clientes anónimos no pueden enumerar los blobs dentro del contenedor.
- **Acceso de lectura público para contenedores y blobs:** Los datos de los contenedores y los blobs los puede leer una solicitud anónima, excepto la configuración de los permisos de un contenedor y los metadatos de un contenedor. Los clientes pueden enumerar los blobs del contenedor a través de una solicitud anónima, pero no pueden enumerar los contenedores que están en la cuenta de almacenamiento.

No puede cambiar el nivel de acceso público para un blob individual. El nivel de acceso público se establece en el nivel de contenedor.

Para establecer el nivel de acceso público de un contenedor, use Azure Portal o la CLI de Azure. El nivel de acceso público del contenedor se puede establecer al crear el contenedor o al actualizar este valor en un contenedor existente.

# <a name="azure-portal"></a>[Azure Portal](#tab/portal)

Para actualizar el nivel de acceso público en uno o varios contenedores desde Azure Portal, siga estos pasos:

1. Vaya a la información general de su cuenta de almacenamiento en Azure Portal.
1. En **Blob service** en la hoja de menú, seleccione **Contenedores**.
1. Seleccione los contenedores para el que quiere establecer el nivel de acceso público.
1. Use el botón **Cambiar nivel de acceso** para mostrar la configuración de acceso público.
1. Seleccione el nivel de acceso público deseado en la lista desplegable **Nivel de acceso público** y haga clic en el botón Aceptar para aplicar el cambio a los contenedores seleccionados.

    ![Captura de pantalla que muestra cómo establecer el nivel de acceso público en el portal](./media/anonymous-read-access-configure/configure-public-access-container.png)

Cuando el acceso público está deshabilitado en la cuenta de almacenamiento, no es posible establecer el nivel de acceso público de ningún contenedor. Si intenta establecer el nivel de acceso público del contenedor, verá que el valor está deshabilitado porque el acceso público está prohibido para la cuenta.

:::image type="content" source="media/anonymous-read-access-configure/container-public-access-blocked.png" alt-text="Captura de pantalla que muestra que el valor del nivel de acceso público del contenedor está bloqueado cuando se deshabilita el acceso público":::

# <a name="azure-cli"></a>[CLI de Azure](#tab/azure-cli)

Para actualizar el nivel de acceso público de uno o varios contenedores con la CLI de Azure, llame al comando [az storage container set permission](/cli/azure/storage/container#az-storage-container-set-permission). Para autorizar esta operación, use la clave de su cuenta, una cadena de conexión o una firma de acceso compartido (SAS). La operación [Set Container ACL](/rest/api/storageservices/set-container-acl) (Establecer ACL de contenedor) que establece el nivel de acceso público del contenedor no admite la autorización con Azure AD. Para obtener más información, consulte [Permisos para llamar a operaciones de datos de blobs y colas](/rest/api/storageservices/authorize-with-azure-active-directory#permissions-for-calling-blob-and-queue-data-operations).

El siguiente ejemplo establece el valor del acceso público en un contenedor para habilitar el acceso anónimo tanto al contenedor como a sus blobs. No olvide reemplazar los valores del marcador de posición entre corchetes con sus propios valores:

```azurecli-interactive
az storage container set-permission \
    --name <container-name> \
    --account-name <account-name> \
    --public-access container \
    --account-key <account-key> \
    --auth-mode key
```

Cuando el acceso público está deshabilitado en la cuenta de almacenamiento, no es posible establecer el nivel de acceso público de ningún contenedor. Si intenta establecer el nivel de acceso público del contenedor, se produce un error que indica que no se permite el acceso público en la cuenta de almacenamiento.

---

## <a name="check-the-container-public-access-setting"></a>Comprobación del valor del acceso público del contenedor

Para comprobar la configuración del acceso público de uno o varios contenedores, puede usar Azure Portal, PowerShell, la CLI de Azure, una de las bibliotecas cliente de Azure Storage o el proveedor de recursos de Azure Storage. En las siguientes secciones encontrará algunos ejemplos.  

### <a name="check-the-public-access-setting-for-a-single-container"></a>Comprobación de la configuración del acceso público de un contenedor individual

Para obtener el nivel de acceso público de uno o varios contenedores con la CLI de Azure, llame al comando [az storage container show permission](/cli/azure/storage/container#az-storage-container-show-permission). Para autorizar esta operación, use la clave de su cuenta, una cadena de conexión o una firma de acceso compartido (SAS). La operación [Get Container ACL](/rest/api/storageservices/get-container-acl) (Obtener ACL de contenedor) que devuelve el nivel de acceso público del contenedor no admite la autorización con Azure AD. Para obtener más información, consulte [Permisos para llamar a operaciones de datos de blobs y colas](/rest/api/storageservices/authorize-with-azure-active-directory#permissions-for-calling-blob-and-queue-data-operations).

El siguiente ejemplo lee la configuración del acceso público de un contenedor. No olvide reemplazar los valores del marcador de posición entre corchetes con sus propios valores:

```azurecli-interactive
az storage container show-permission \
    --name <container-name> \
    --account-name <account-name> \
    --account-key <account-key>
    --auth-mode key
```

### <a name="check-the-public-access-setting-for-a-set-of-containers"></a>Comprobación de la configuración del acceso público de un conjunto de contenedores

Para comprobar qué contenedores de una o varias cuentas de almacenamiento están configurados para el acceso público, puede enumerar los contenedores y consultar la configuración del acceso público. Este enfoque es una opción práctica cuando una cuenta de almacenamiento no contiene un elevado número de contenedores, o bien cuando se comprueba la configuración de un pequeño número de cuentas de almacenamiento. Sin embargo, el rendimiento puede empeorar si intenta enumerar un elevado número de contenedores.

En el ejemplo siguiente se usa PowerShell para obtener la configuración del acceso público de todos los contenedores de una cuenta de almacenamiento. No olvide reemplazar los valores del marcador de posición entre corchetes con sus propios valores:

```powershell
$rgName = "<resource-group>"
$accountName = "<storage-account>"

$storageAccount = Get-AzStorageAccount -ResourceGroupName $rgName -Name $accountName
$ctx = $storageAccount.Context

Get-AzStorageContainer -Context $ctx | Select Name, PublicAccess
```

## <a name="next-steps"></a>Pasos siguientes

- [Impedir el acceso de lectura público anónimo a contenedores y blobs](anonymous-read-access-prevent.md)
- [Acceso anónimo a contenedores y blobs públicos con .NET](anonymous-read-access-client.md)
- [Autorización del acceso a Azure Storage](../common/storage-auth.md)