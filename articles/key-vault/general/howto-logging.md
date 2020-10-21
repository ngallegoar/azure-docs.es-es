---
title: Habilitación del registro de Azure Key Vault
description: Se explica cómo habilitar el registro de Azure Key Vault, que guarda la información en una cuenta de almacenamiento de Azure proporcionada por el usuario.
services: key-vault
author: msmbaldwin
tags: azure-resource-manager
ms.service: key-vault
ms.subservice: general
ms.topic: how-to
ms.date: 10/01/2020
ms.author: mbaldwin
ms.openlocfilehash: 8a975673bec3b3579eaa699f873fe8c4b1481d38
ms.sourcegitcommit: 829d951d5c90442a38012daaf77e86046018e5b9
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/09/2020
ms.locfileid: "91744699"
---
# <a name="how-to-enable-key-vault-logging"></a>Habilitación del registro de Key Vault

Después de crear uno o varios almacenes de claves, es probable que desee supervisar cómo y cuándo se accede a los almacenes de claves y quién lo hace. Para obtener detalles completos sobre la característica, consulte [Registro de Azure Key Vault](logging.md).

## <a name="prerequisites"></a>Requisitos previos

Para realizar este tutorial, debe disponer de lo siguiente:

* Un Almacén de claves existente que ha utilizado.  
* La CLI de Azure o Azure PowerShell
* Suficiente almacenamiento en Azure para sus registros de Key Vault.

Si decide instalar y usar la CLI localmente, necesitará la versión 2.0.4 o posterior de la CLI de Azure. Ejecute `az --version` para encontrar la versión. Si necesita instalarla o actualizarla, consulte [Instalación de la CLI de Azure](/cli/azure/install-azure-cli). Para iniciar sesión en Azure mediante la CLI puede escribir:

```azurecli-interactive
az login
```

Si decide instalar y usar PowerShell localmente, necesitará la versión 1.0.0 o posterior del módulo de Azure PowerShell. Escriba `$PSVersionTable.PSVersion` para encontrar la versión. Si necesita actualizarla, consulte [Instalación del módulo de Azure PowerShell](/powershell/azure/install-az-ps). Si PowerShell se ejecuta localmente, también debe ejecutar `Connect-AzAccount` para crear una conexión con Azure.

```powershell-interactive
Connect-AzAccount
```

## <a name="connect-to-your-key-vault-subscription"></a>Conexión a su suscripción de Key Vault

El primer paso para configurar el registro de claves es conectarse a la suscripción que contiene el almacén de claves. Este es un paso especialmente importante si tiene varias suscripciones asociadas a su cuenta.

Con la CLI de Azure, puede ver todas las suscripciones mediante el comando [az account list](/cli/azure/account?view=azure-cli-latest#az_account_list); después puede conectarse a una de ellas mediante [az account set](/cli/azure/account?view=azure-cli-latest#az_account_set):

```azurecli-interactive
az account list

az account set --subscription "<subscriptionID>"
```

Con Azure PowerShell, primero puede enumerar las suscripciones mediante el cmdlet [Get-AzSubscription](/powershell/module/az.accounts/get-azsubscription?view=azps-4.7.0) y, después, conectarse a una de ellas mediante el cmdlet [Set-AzContext](/powershell/module/az.accounts/set-azcontext?view=azps-4.7.0): 

```powershell-interactive
Get-AzSubscription

Set-AzContext -SubscriptionId "<subscriptionID>"
```

## <a name="create-a-storage-account-for-your-logs"></a>Creación de una cuenta de almacenamiento para sus registros

Aunque puede usar una cuenta de almacenamiento existente para sus registros, crearemos una cuenta de almacenamiento dedicada a los registros de Key Vault. 

Para una mayor facilidad de administración, también usaremos el grupo de recursos que contiene el almacén de claves. En el [inicio rápido de la CLI de Azure](quick-create-cli.md) y el [inicio rápido de Azure PowerShell](quick-create-powershell.md), este grupo de recursos se denomina **myResourceGroup** y la ubicación es *eastus*. Sustituya estos valores por los suyos propios, según corresponda. 

También será necesario indicar un nombre de cuenta de almacenamiento. Los nombres de cuentas de almacenamiento deben ser únicos, tener entre 3 y 24 caracteres, y usar solo números y letras minúsculas.  Por último, vamos a crear una cuenta de almacenamiento de la SKU "Standard_LRS".

Con la CLI de Azure, use el comando [az storage account create](/cli/azure/storage/account?view=azure-cli-latest#az_storage_account_create).

```azurecli-interactive
az storage account create --name "<your-unique-storage-account-name>" -g "myResourceGroup" --sku "Standard_LRS"
```

Con Azure PowerShell, use el cmdlet [New-AzStorageAccount](/powershell/module/az.storage/new-azstorageaccount?view=azps-4.7.0). Tendrá que proporcionar la ubicación correspondiente al grupo de recursos.

```powershell
 New-AzStorageAccount -ResourceGroupName myResourceGroup -Name "<your-unique-storage-account-name>" -Type "Standard_LRS" -Location "eastus"
```

En cualquier caso, anote el valor "id" de la cuenta de almacenamiento. La operación con la CLI de Azure devuelve el valor de "id" en la salida. Para obtener el valor de "id" con Azure PowerShell, use [Get-AzStorageAccount](/powershell/module/az.storage/get-azstorageaccount?view=azps-4.7.0) y asigne la salida a la variable $sa. Así, puede ver la cuenta de almacenamiento con $sa.id. Más adelante en este artículo también se usará la propiedad "$sa.Context".

```powershell-interactive
$sa = Get-AzStorageAccount -Name "<your-unique-storage-account-name>" -ResourceGroup "myResourceGroup"
$sa.id
```

El valor "id" de la cuenta de almacenamiento tendrá el formato "/subscriptions/<su-Id.-de-suscripción>/resourceGroups/myResourceGroup/providers/Microsoft.Storage/storageAccounts/<su-nombre-único-de-cuenta-de-almacenamiento>".

> [!NOTE]
> Si decide usar una cuenta de almacenamiento existente, esta debe usar la misma suscripción que el almacén de claves; también debe usar el modelo de implementación de Azure Resource Manager, en lugar del modelo de implementación clásica.

## <a name="obtain-your-key-vault-resource-id"></a>Obtención del identificador de recurso del almacén de claves

En el [inicio rápido de la CLI](quick-create-cli.md) y el [inicio rápido de PowerShell](quick-create-powershell.md), creó una clave con un nombre único.  Vuelva a usar ese nombre en los pasos siguientes.  Si no recuerda el nombre del almacén de claves, puede usar el comando [az keyvault list](/cli/azure/keyvault?view=azure-cli-latest#az_keyvault_list) de la CLI de Azure o el cmdlet [Get-AzKeyVault](/powershell/module/az.keyvault/get-azkeyvault?view=azps-4.7.0) de Azure PowerShell para obtenerlo.

Use el nombre del almacén de claves para encontrar su identificador de recurso.  Con la CLI de Azure, use el comando [az keyvault show](/cli/azure/keyvault?view=azure-cli-latest#az_keyvault_show).

```azurecli-interactive
az keyvault show --name "<your-unique-keyvault-name>"
```

Con Azure PowerShell, use el cmdlet [Get-AzKeyVault](/powershell/module/az.keyvault/get-azkeyvault?view=azps-4.7.0).

```powershell-interactive
Get-AzKeyVault -VaultName "<your-unique-keyvault-name>"
```

El identificador de recurso del almacén de claves tendrá el formato "/subscriptions/<su-id.-de-suscripción>/resourceGroups/myResourceGroup/providers/Microsoft.KeyVault/vaults/<su-nombre-único-de-almacén-de-claves>". Guárdelo para el siguiente paso.

## <a name="enable-logging-using-azure-powershell"></a>Habilitación del registro con Azure PowerShell

Para habilitar el registro de Key Vault, usaremos el comando [az monitor diagnostic-settings create](/cli/azure/monitor/diagnostic-settings?view=azure-cli-latest) de la CLI de Azure, o el cmdlet [Set-AzDiagnosticSetting](/powershell/module/az.monitor/set-azdiagnosticsetting?view=azps-4.7.0), junto con el identificador de la cuenta de almacenamiento y el identificador de recurso del almacén de claves.

```azurecli-interactive
az monitor diagnostic-settings create --storage-account "<storage-account-id>" --resource "<key-vault-resource-id>" --name "Key vault logs" --logs '[{"category": "AuditEvent","enabled": true}]' --metrics '[{"category": "AllMetrics","enabled": true}]'
```

Con Azure PowerShell, usaremos el cmdlet [Set-AzDiagnosticSetting](/powershell/module/az.monitor/set-azdiagnosticsetting?view=azps-4.7.0), con la marca **-Enabled** establecida en **$true** y la categoría establecida en `AuditEvent` (la única categoría para el registro de Key Vault):

```powershell-interactive
Set-AzDiagnosticSetting -ResourceId "<key-vault-resource-id>" -StorageAccountId $sa.id -Enabled $true -Category "AuditEvent"
```

Si lo desea, puede establecer una directiva de retención para los registros, de forma que los registros más antiguos se eliminen automáticamente tras un período de tiempo especificado. Por ejemplo, podría establecer una directiva de retención que elimine automáticamente los registros que tengan más de 90 días.

<!-- With the Azure CLI, use the [az monitor diagnostic-settings update](/cli/azure/monitor/diagnostic-settings?view=azure-cli-latest#az_monitor_diagnostic_settings_update) command. 

```azurecli-interactive
az monitor diagnostic-settings update 
```
-->

Con Azure PowerShell, use el cmdlet [Set-AzDiagnosticSetting](/powershell/module/az.monitor/set-azdiagnosticsetting?view=azps-4.7.0). 

```powershell-interactive
Set-AzDiagnosticSetting "<key-vault-resource-id>" -StorageAccountId $sa.id -Enabled $true -Category AuditEvent -RetentionEnabled $true -RetentionInDays 90
```

Esto es lo que se registra:

* Todas las solicitudes de la API REST autenticadas, incluidas las solicitudes con error debido a permisos de acceso, errores del sistema o solicitudes incorrectas.
* Las operaciones en el almacén de claves, incluidas la creación, la eliminación, la definición de políticas de acceso del almacén de claves y la actualización de los atributos del almacén de claves, como las etiquetas.
* Las operaciones en claves y secretos del almacén de claves, incluido lo siguiente:
  * Crear, modificar o eliminar estas claves o secretos.
  * Firmar, comprobar, cifrar, descifrar, encapsular y desencapsular claves, obtener secretos y elaborar listados de claves y secretos (y sus versiones).
* Solicitudes no autenticadas que dan como resultado una respuesta 401. Por ejemplo, las solicitudes que no tienen un token de portador, cuyo formato es incorrecto o está caducado o que tienen un token no válido.  
* Los eventos de notificación de Event Grid para la expiración cercana, ya expirado y la directiva de acceso al almacén han cambiado (no se registra el evento de la nueva versión). Los eventos se registran independientemente de si hay una suscripción de eventos creada en el almacén de claves. Para más información, consulte [Esquema de eventos de Event Grid](https://docs.microsoft.com/azure/event-grid/event-schema-key-vault).

## <a name="access-your-logs"></a>Acceso a los registros

Los registros de Key Vault se almacenan en el contenedor "insights-logs-auditevent" de la cuenta de almacenamiento que ha proporcionado. Para ver los registros, tendrá que descargar blobs.

En primer lugar, enumere todos los blobs del contenedor.  Con la CLI de Azure, use el comando [az storage blob list](/cli/azure/storage/blob?view=azure-cli-latest#az_storage_blob_list).

```azurecli-interactive
az storage blob list --account-name "<your-unique-storage-account-name>" --container-name "insights-logs-auditevent"
```

Con Azure PowerShell, use el cmdlet [Get-AzStorageBlob](/powershell/module/az.storage/get-azstorageblob?view=azps-4.7.0). Para obtener la lista de todos los blobs de este contenedor, escriba:

```powershell
Get-AzStorageBlob -Container $container -Context $sa.Context
```

Como podrá ver en la salida del comando de la CLI de Azure o del cmdlet de Azure PowerShell, el nombre de los blobs tiene el formato `resourceId=<ARM resource ID>/y=<year>/m=<month>/d=<day of month>/h=<hour>/m=<minute>/filename.json`. Los valores de fecha y hora usan UTC.

Puesto que la misma cuenta de almacenamiento puede usarse para recopilar registros de varios recursos, el identificador de recurso completo en el nombre del blob es útil para acceder o descargar solo los blobs que necesita. Pero antes de hacerlo, primero explicaremos cómo descargar todos los blobs.

Con la CLI de Azure, use el comando [az storage blob download](/cli/azure/storage/blob?view=azure-cli-latest#az_storage_blob_download); pásele los nombres de los blobs y la ruta de acceso al archivo en el que desea guardar los resultados.

```azurecli-interactive
az storage blob download --container-name "insights-logs-auditevent" --file <path-to-file> --name "<blob-name>" --account-name "<your-unique-storage-account-name>"
```

Con Azure PowerShell, use el cmdlet [Gt-AzStorageBlobs](/powershell/module/az.storage/get-azstorageblob?view=azps-4.7.0) para obtener una lista de los blobs y, después, canalícela al cmdlet [Get-AzStorageBlobContent](/powershell/module/az.storage/get-azstorageblobcontent?view=azps-4.7.0) para descargar los registros en la ruta de acceso elegida.

```powershell-interactive
$blobs = Get-AzStorageBlob -Container $container -Context $sa.Context | Get-AzStorageBlobContent -Destination "<path-to-file>"
```

Al ejecutar este segundo comando en PowerShell, el delimitador **/** de los nombres de blobs crea una estructura de carpetas completa en la carpeta de destino. Usará esta estructura para descargar y almacenar los blobs como archivos.

Para descargar blobs de forma selectiva, utilice caracteres comodín. Por ejemplo:

* Si tiene varios almacenes de claves y desea descargar los registros solo para un Almacén de claves, denominado CONTOSOKEYVAULT3:

  ```powershell
  Get-AzStorageBlob -Container $container -Context $sa.Context -Blob '*/VAULTS/CONTOSOKEYVAULT3
  ```

* Si tiene varios grupos de recursos y desea descargar los registros solo de un grupo de recursos específico, use `-Blob '*/RESOURCEGROUPS/<resource group name>/*'`:

  ```powershell
  Get-AzStorageBlob -Container $container -Context $sa.Context -Blob '*/RESOURCEGROUPS/CONTOSORESOURCEGROUP3/*'
  ```

* Si desea descargar todos los registros del mes de enero de 2019, use `-Blob '*/year=2019/m=01/*'`:

  ```powershell
  Get-AzStorageBlob -Container $container -Context $sa.Context -Blob '*/year=2016/m=01/*'
  ```

Ahora está listo para comenzar a ver lo que está en los registros. Pero antes de pasar a ello, debe conocer dos comandos más:

Para obtener información detallada sobre cómo leer los registros, consulte [Registro de Azure Key Vault: Interpretación de los registros de Key Vault](logging.md#interpret-your-key-vault-logs).

## <a name="use-azure-monitor-logs"></a>Uso de registros de Azure Monitor

Puede utilizar la solución Key Vault en registros de Azure Monitor para revisar los registros `AuditEvent` de Key Vault. En los registros de Azure Monitor, las consultas de los registros se usan para analizar los datos y obtener la información que necesita.

Para más información, incluido cómo configurar esta opción, consulte [Azure Key Vault en Azure Monitor](../../azure-monitor/insights/key-vault-insights-overview.md).

## <a name="next-steps"></a>Pasos siguientes

- Para obtener información conceptual, incluido cómo interpretar los registros de Key Vault, consulte [Registro de Azure Key Vault](logging.md).
- Para ver un tutorial que use Azure Key Vault en una aplicación web .NET, consulte [Uso de Azure Key Vault desde una aplicación web](tutorial-net-create-vault-azure-web-app.md).
- Para conocer las referencias de programación, consulte la [Guía del desarrollador de Azure Key Vault](developers-guide.md).