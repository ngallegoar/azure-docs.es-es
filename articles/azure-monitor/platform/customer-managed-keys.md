---
title: Clave administrada por el cliente de Azure Monitor
description: Información y pasos para configurar la clave administrada por el cliente para cifrar los datos en las áreas de trabajo de Log Analytics mediante una clave de Azure Key Vault.
ms.subservice: logs
ms.topic: conceptual
author: yossi-y
ms.author: yossiy
ms.date: 11/09/2020
ms.openlocfilehash: 62621a36955808ec3f2c796681fe660e6e8524bc
ms.sourcegitcommit: 6109f1d9f0acd8e5d1c1775bc9aa7c61ca076c45
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 11/10/2020
ms.locfileid: "94443388"
---
# <a name="azure-monitor-customer-managed-key"></a>Clave administrada por el cliente de Azure Monitor 

En este artículo se proporciona información general y los pasos para configurar las claves administradas por el cliente para las áreas de trabajo de Log Analytics. Una vez configuradas, los datos enviados a sus áreas de trabajo se cifran con su clave de Azure Key Vault.

Se recomienda revisar la sección [Limitaciones y restricciones](#limitationsandconstraints) que aparece más abajo antes de proceder con la configuración.

## <a name="customer-managed-key-overview"></a>Información general de la clave administrada por el cliente

[El cifrado en reposo](../../security/fundamentals/encryption-atrest.md) es un requisito común de privacidad y seguridad en las organizaciones. Puede dejar que Azure administre por completo el cifrado en reposo, mientras dispone de varias opciones para administrar minuciosamente el cifrado o las claves de cifrado.

Azure Monitor garantiza que todos los datos y las consultas guardadas se cifren en reposo mediante claves administradas por Microsoft (MMK). Azure Monitor también proporciona una opción para el cifrado mediante su propia clave que está almacenada en [Azure Key Vault](../../key-vault/general/overview.md) y Azure Storage usa para el cifrado de datos. La clave se puede [proteger mediante software o con módulos de seguridad de hardware (HSM)](../../key-vault/general/overview.md). El uso del cifrado de Azure Monitor es idéntico a la forma en que funciona el [cifrado de Azure Storage](../../storage/common/storage-service-encryption.md#about-azure-storage-encryption).

La capacidad de clave administrada por el cliente se proporciona en clústeres de Log Analytics dedicados. Permite proteger los datos con el control de la [Caja de seguridad](#customer-lockbox-preview) y proporciona el control para revocar el acceso a los datos en cualquier momento. Los datos ingeridos en los últimos 14 días también se conservan en la memoria caché activa (respaldada por SSD) para un funcionamiento eficaz del motor de consultas. Estos datos permanecen cifrados con las claves de Microsoft, independientemente de la configuración de clave administrada por el cliente, pero el control sobre los datos de SSD se adhiere a la [revocación de claves](#key-revocation). Estamos trabajando para que los datos de SSD se cifren con una clave administrada por el cliente en la primera mitad de 2021.

El [modelo de precios de los clústeres de Log Analytics](./manage-cost-storage.md#log-analytics-dedicated-clusters) usa reservas de capacidad a partir de un nivel de 1000 GB/día.

> [!IMPORTANT]
> Debido a las restricciones de capacidad temporales, se exige el registro previo a crear un clúster. Use sus contactos en Microsoft o abra la solicitud de soporte técnico para registrar los id. de las suscripciones.

## <a name="how-customer-managed-key-works-in-azure-monitor"></a>Funcionamiento de las claves administradas por el cliente en Azure Monitor

Azure Monitor aprovecha la identidad administrada asignada por el sistema para conceder acceso a su instancia de Azure Key Vault. La identidad administrada asignada por el sistema solo se puede asociar a un único recurso de Azure, mientras que la identidad del clúster de Log Analytics se admite en el nivel de clúster, lo que indica que la capacidad de la clave administrada por el cliente se entrega en un clúster de Log Analytics dedicado. Para admitir la clave administrada por el cliente en varias áreas de trabajo, un nuevo recurso de *clúster* de Log Analytics debe funcionar como una conexión de identidad intermedia entre la instancia de Key Vault y las áreas de trabajo de Log Analytics. El almacenamiento de clúster de Log Analytics usa la identidad administrada que \'está asociada al recurso de *clúster* para autenticar el acceso y acceder a Azure Key Vault mediante Azure Active Directory. 

Después de la configuración, los datos ingeridos en las áreas de trabajo vinculadas asociadas a su clúster dedicado se cifran con la clave en Key Vault. Puede desvincular las áreas de trabajo del clúster en cualquier momento. Después, los nuevos datos se introducen en el almacenamiento de Log Analytics y se cifran con la clave de Microsoft, mientras que puede consultar los datos nuevos y antiguos sin problemas.


![Información general de la clave administrada por el cliente](media/customer-managed-keys/cmk-overview.png)

1. Key Vault
2. Recurso *Clúster* de Log Analytics que tiene una identidad administrada con permisos para Key Vault: la identidad se admite en el nivel del almacenamiento de clúster dedicado de Log Analytics
3. Clúster dedicado de Log Analytics
4. Áreas de trabajo vinculadas al recurso de *clúster* 

## <a name="encryption-keys-operation"></a>Operación de claves de cifrado

Hay tres tipos de claves que se usan en el cifrado de datos de Storage:

- **KEK**: clave de cifrado de claves (su clave administrada por el cliente)
- **AEK**: clave de cifrado de la cuenta
- **DEK**: clave de cifrado de datos

Se aplican las reglas siguientes:

- Las cuentas de almacenamiento de clúster de Log Analytics generan una clave de cifrado única para cada cuenta de almacenamiento, lo que se conoce como AEK.
- La AEK se usa para derivar DEK, que son las claves que se usan para cifrar cada bloque de datos escritos en el disco.
- Al configurar la clave en Key Vault y hacer referencia a ella en el clúster, Azure Storage envía solicitudes a Azure Key Vault para ajustar y desajustar AEK para realizar operaciones de cifrado y descifrado de datos.
- Esta KEK nunca abandona su instancia de Key Vault y, en el caso de una clave de HSM, nunca abandona el hardware.
- Azure Storage usa la identidad administrada que está asociada al recurso *Clúster* para autenticar el acceso y acceder a Azure Key Vault mediante Azure Active Directory.

## <a name="customer-managed-key-provisioning-procedure"></a>Procedimiento de aprovisionamiento de claves administradas por el cliente

1. Registro de la suscripción para permitir la creación de clústeres
1. Creación de una instancia de Azure Key Vault y almacenamiento de la clave
1. Creación de un clúster
1. Concesión de permisos a la instancia de Key Vault
1. Vinculación de áreas de trabajo de Log Analytics

La configuración de las claves administradas por el cliente no se admite en Azure Portal y el aprovisionamiento se realiza mediante [PowerShell](https://docs.microsoft.com/powershell/module/az.operationalinsights/), la [CLI](https://docs.microsoft.com/cli/azure/monitor/log-analytics) o solicitudes [REST](https://docs.microsoft.com/rest/api/loganalytics/).

### <a name="asynchronous-operations-and-status-check"></a>Operaciones asincrónicas y comprobación de estado

Algunos de los pasos de configuración se ejecutan de forma asincrónica porque no se pueden completar rápidamente. Cuando se utilizan solicitudes REST en la configuración, la respuesta devuelve inicialmente un código de estado HTTP 200 (Correcto) y un encabezado con la propiedad *Azure-AsyncOperation* cuando se acepta:
```json
"Azure-AsyncOperation": "https://management.azure.com/subscriptions/subscription-id/providers/Microsoft.OperationalInsights/locations/region-name/operationStatuses/operation-id?api-version=2020-08-01"
```

A continuación, puede comprobar el estado de la operación asincrónica mediante el envío de una solicitud GET al valor del encabezado *Azure-AsyncOperation*:
```rst
GET https://management.azure.com/subscriptions/subscription-id/providers/microsoft.operationalInsights/locations/region-name/operationstatuses/operation-id?api-version=2020-08-01
Authorization: Bearer <token>
```

La respuesta contiene información sobre la operación y su *Estado*. Puede tener uno de los valores siguientes:

La operación está en curso.
```json
{
    "id": "Azure-AsyncOperation URL value from the GET operation",
    "name": "operation-id", 
    "status" : "InProgress", 
    "startTime": "2017-01-06T20:56:36.002812+00:00",
}
```

Operación de actualización de identificador de clave en curso
```json
{
    "id": "Azure-AsyncOperation URL value from the GET operation",
    "name": "operation-id", 
    "status" : "Updating", 
    "startTime": "2017-01-06T20:56:36.002812+00:00",
    "endTime": "2017-01-06T20:56:56.002812+00:00",
}
```

La eliminación del clúster está en curso: cuando se elimina un clúster que tiene áreas de trabajo vinculadas, la operación de desvinculación se realiza para cada una de las áreas de trabajo de forma asincrónica y la operación puede tardar cierto tiempo.
Esto no es relevante cuando se elimina un clúster sin ningún área de trabajo asociada; en este caso, el clúster se elimina inmediatamente.
```json
{
    "id": "Azure-AsyncOperation URL value from the GET operation",
    "name": "operation-id", 
    "status" : "Deleting", 
    "startTime": "2017-01-06T20:56:36.002812+00:00",
    "endTime": "2017-01-06T20:56:56.002812+00:00",
}
```

Operación completada.
```json
{
    "id": "Azure-AsyncOperation URL value from the GET operation",
    "name": "operation-id", 
    "status" : "Succeeded", 
    "startTime": "2017-01-06T20:56:36.002812+00:00",
    "endTime": "2017-01-06T20:56:56.002812+00:00",
}
```

Error en la operación.
```json
{
    "id": "Azure-AsyncOperation URL value from the GET operation",
    "name": "operation-id", 
    "status" : "Failed", 
    "startTime": "2017-01-06T20:56:36.002812+00:00",
    "endTime": "2017-01-06T20:56:56.002812+00:00",
    "error" : { 
        "code": "error-code",  
        "message": "error-message" 
    }
}
```

### <a name="allowing-subscription"></a>Habilitación de la suscripción

> [!IMPORTANT]
> La capacidad de claves administradas por el cliente es regional. La instancia de Azure Key Vault, el clúster y las áreas de trabajo de Log Analytics vinculadas deben estar en la misma región, pero pueden estar en distintas suscripciones.

### <a name="storing-encryption-key-kek"></a>Almacenamiento de la clave de cifrado de claves (KEK)

Cree un recurso de Azure Key Vault, o use uno que ya tenga, para generar o importar una clave que se usará para el cifrado de datos. Azure Key Vault se debe configurar como recuperable para proteger su clave y el acceso a los datos en Azure Monitor. Puede comprobar esta configuración en las propiedades de Key Vault: tanto la *Eliminación temporal* como la *Protección de purga* deben estar habilitadas.

![Configuración de la eliminación temporal y la protección de purga](media/customer-managed-keys/soft-purge-protection.png)

Esta configuración puede actualizarse en Key Vault a través de la CLI y PowerShell:

- [eliminación temporal](../../key-vault/general/soft-delete-overview.md)
- La [Protección de purga](../../key-vault/general/soft-delete-overview.md#purge-protection) protege contra la eliminación forzada del secreto o almacén incluso después de la eliminación temporal.

### <a name="create-cluster"></a>Crear clúster

Siga el procedimiento que se muestra en el [artículo Clústeres dedicados](https://docs.microsoft.com/azure/azure-monitor/log-query/logs-dedicated-clusters#creating-a-cluster). 

> [!IMPORTANT]
> Copie y guarde la respuesta, ya que necesitará los detalles en los pasos siguientes.

### <a name="grant-key-vault-permissions"></a>Concesión de permisos a Key Vault

Cree una directiva de acceso en Key Vault para conceder permisos al clúster. Estos permisos los utiliza el almacenamiento de Azure Monitor subyacente para el cifrado de datos. Abra su instancia de Key Vault en Azure Portal y haga clic en "Directivas de acceso" y luego en "+ Agregar directiva de acceso" para crear una nueva directiva con esta configuración:

- Permisos de clave: seleccione los permisos "Obtener", "Encapsular clave" y "Desencapsular clave".
- Seleccionar entidad de seguridad: escriba el nombre del clúster o el identificador de la entidad de seguridad que se devolvió en la respuesta del paso anterior.

![Concesión de permisos a Key Vault](media/customer-managed-keys/grant-key-vault-permissions-8bit.png)

El permiso *Obtener* es necesario para comprobar que su instancia de Azure Key Vault está configurada como recuperable con el fin de proteger su clave y el acceso a sus datos de Azure Monitor.

### <a name="update-cluster-with-key-identifier-details"></a>Actualización del clúster con detalles del identificador de clave

Todas las operaciones en el clúster requieren el permiso Microsoft.OperationalInsights/clusters/write action. Este permiso se puede conceder a través del rol Propietario o Colaborador que contiene la acción */write action o a través del rol Colaborador de Log Analytics que contiene la acción Microsoft.OperationalInsights/* .

En este paso se actualiza el almacenamiento de Azure Monitor con la clave y la versión que se van a usar para el cifrado de los datos. Cuando se actualiza, la clave nueva se usa para ajustar y desajustar la clave de almacenamiento (AEK).

Seleccione la versión actual de la clave en Azure Key Vault para obtener los detalles del identificador de clave.

![Concesión de permisos a Key Vault](media/customer-managed-keys/key-identifier-8bit.png)

Actualice KeyVaultProperties en el clúster con los detalles del identificador de clave.

Esta operación es asincrónica y puede tardar un tiempo en completarse.

```azurecli
az monitor log-analytics cluster update --name "cluster-name" --resource-group "resource-group-name" --key-name "key-name" --key-vault-uri "key-uri" --key-version "key-version"
```

```powershell
Update-AzOperationalInsightsCluster -ResourceGroupName "resource-group-name" -ClusterName "cluster-name" -KeyVaultUri "key-uri" -KeyName "key-name" -KeyVersion "key-version"
```

```rst
PATCH https://management.azure.com/subscriptions/<subscription-id>/resourcegroups/<resource-group-name>/providers/Microsoft.OperationalInsights/clusters/cluster-name"?api-version=2020-08-01
Authorization: Bearer <token> 
Content-type: application/json
 
{
  "properties": {
    "keyVaultProperties": {
      "keyVaultUri": "https://key-vault-name.vault.azure.net",
      "kyName": "key-name",
      "keyVersion": "current-version"
  },
  "sku": {
    "name": "CapacityReservation",
    "capacity": 1000
  }
}
```

**Respuesta**

La propagación del identificador de clave tarda unos minutos en completarse. Puede comprobar el estado de actualización de dos maneras:
1. Copie el valor de la dirección URL de Azure-AsyncOperation de la respuesta y siga la [comprobación del estado de operaciones asincrónicas](#asynchronous-operations-and-status-check).
2. Envíe una solicitud GET en el clúster y examine las propiedades de *KeyVaultProperties*. Los detalles del identificador de clave que actualizó recientemente se deben devolver en la respuesta.

Una respuesta a la solicitud GET debe ser similar a la siguiente cuando se complete la actualización del identificador de clave: 200 Correcto y encabezado
```json
{
  "identity": {
    "type": "SystemAssigned",
    "tenantId": "tenant-id",
    "principalId": "principle-id"
    },
  "sku": {
    "name": "capacityReservation",
    "capacity": 1000,
    "lastSkuUpdate": "Sun, 22 Mar 2020 15:39:29 GMT"
    },
  "properties": {
    "keyVaultProperties": {
      "keyVaultUri": "https://key-vault-name.vault.azure.net",
      "kyName": "key-name",
      "keyVersion": "current-version"
      },
    "provisioningState": "Succeeded",
    "billingType": "cluster",
    "clusterId": "cluster-id"
  },
  "id": "/subscriptions/subscription-id/resourceGroups/resource-group-name/providers/Microsoft.OperationalInsights/clusters/cluster-name",
  "name": "cluster-name",
  "type": "Microsoft.OperationalInsights/clusters",
  "location": "region-name"
}
```

### <a name="link-workspace-to-cluster"></a>Vinculación del área de trabajo al clúster

Debe tener permisos de "escritura" en el área de trabajo y en el clúster para realizar esta operación, que incluye estas acciones:

- En el área de trabajo: Microsoft.OperationalInsights/workspaces/write
- En el clúster: Microsoft.OperationalInsights/clusters/write

> [!IMPORTANT]
> Este paso solo debe realizarse una vez completada la Log Analytics el aprovisionamiento del clúster. Si vincula áreas de trabajo e ingiere datos antes del aprovisionamiento, los datos ingeridos se quitarán y no se podrán recuperar.

Esta operación es asincrónica y puede tardar un tiempo en completarse.

Siga el procedimiento que se muestra en el [artículo Clústeres dedicados](https://docs.microsoft.com/azure/azure-monitor/log-query/logs-dedicated-clusters#link-a-workspace-to-the-cluster).

## <a name="key-revocation"></a>Revocación de claves

Para revocar el acceso a los datos, puede deshabilitar la clave o eliminar la directiva de acceso del clúster en Key Vault. El almacenamiento de clúster de Log Analytics siempre respetará los cambios en los permisos de las claves en el plazo de una hora (normalmente antes), y Storage dejará de estar disponible. Los datos nuevos que se ingieren en áreas de trabajo vinculadas al clúster se quitan y no se podrán recuperar, no se puede acceder a los datos y las consultas a estas áreas de trabajo generan un error. Los datos ingeridos anteriormente permanecerán en el almacenamiento siempre que se no se eliminen el clúster ni las áreas de trabajo. Los datos inaccesibles se rigen por la directiva de retención de datos y se purgarán cuando se alcance la retención. 

Los datos ingeridos en los últimos 14 días también se conservan en la memoria caché activa (respaldada por SSD) para un funcionamiento eficaz del motor de consultas. Esto se elimina en la operación de revocación de claves y también se convierte en inaccesible.

Storage sondeará periódicamente su instancia de Key Vault para intentar desencapsular la clave de cifrado y, una vez haya obtenido el acceso, procederá con la ingesta de datos y la reanudación de la consulta en un plazo de 30 minutos.

## <a name="key-rotation"></a>Rotación de claves

La rotación de claves administradas por el cliente necesita una actualización explícita del clúster con la nueva versión de la clave en Azure Key Vault. Siga las instrucciones que aparecen en el paso "Actualización del clúster con detalles del identificador de clave". Si no actualiza los detalles del identificador de clave en el clúster, el almacenamiento de clúster de Log Analytics seguirá usando la clave anterior para cifrado. Si deshabilita o elimina la clave anterior antes de actualizar la nueva clave en el clúster, obtendrá el estado de [revocación de clave](#key-revocation).

Se puede acceder a todos los datos después de la operación de rotación de claves, incluidos los datos ingeridos antes y después de la rotación, ya que todos los datos permanecen cifrados mediante la clave de cifrado de cuenta (AEK), mientras que la AEK ahora se cifra con la nueva versión de la clave de cifrado de claves (KEK).

## <a name="customer-managed-key-for-queries"></a>Clave administrada para las consultas

El lenguaje de consulta utilizado en Log Analytics es expresivo y puede contener información confidencial en los comentarios que se agregan a las consultas o en la sintaxis de la consulta. Algunas organizaciones requieren que dicha información se mantenga protegida como parte de la directiva de claves administradas por el cliente y debe guardar las consultas cifradas con su clave. Azure Monitor le permite almacenar consultas de *búsquedas guardadas* y de *alertas del registro* cifradas con su clave en su propia cuenta de almacenamiento cuando se conecta al área de trabajo. 

> [!NOTE]
> Las consultas de Log Analytics se pueden guardar en varios almacenes según el escenario usado. Las consultas permanecen cifradas con la clave de Microsoft (MMK) en los escenarios siguientes, independientemente de la configuración de claves administradas por el cliente: Libros en Azure Monitor, paneles de Azure, Azure Logic Apps, Azure Notebooks y runbooks de automatización.

Cuando traiga su propio almacenamiento (BYOS) y lo asocie a su área de trabajo, el servicio carga consultas de *búsquedas guardadas* y de *alertas del registro* en la cuenta de almacenamiento. Esto significa que puede controlar la cuenta de almacenamiento y la [directiva de cifrado en reposo](../../storage/common/customer-managed-keys-overview.md) con la misma clave que se usa para cifrar los datos en el clúster de Log Analytics o con una clave diferente. Sin embargo, será responsable de los costos asociados a esa cuenta de almacenamiento. 

**Consideraciones antes de establecer la clave administrada por el cliente para las consultas**
* Debe tener permisos de "escritura" en el área de trabajo y la cuenta de almacenamiento.
* Asegúrese de crear la cuenta de almacenamiento en la misma región en la que se encuentra el área de trabajo de Log Analytics.
* Las *búsquedas guardadas* en el almacenamiento se consideran artefactos de servicio y su formato puede cambiar.
* Las *búsquedas guardadas* existentes se eliminan del área de trabajo. Copie las *búsquedas guardadas* que necesite antes de la configuración. Puede ver las *búsquedas guardadas* mediante [PowerShell](/powershell/module/az.operationalinsights/get-azoperationalinsightssavedsearch).
* No se admite el historial de consultas y no podrá ver las consultas que se han ejecutado.
* Puede vincular una sola cuenta de almacenamiento al área de trabajo con el fin de guardar las consultas, pero se puede usar tanto para las consultas de *búsquedas guardadas* como para las de *alertas del registro*.
* No se admite el anclaje a un panel.

**Configuración de BYOS para las consultas de búsquedas guardadas**

Vincule una cuenta de almacenamiento de *Consulta* con el área de trabajo. Las consultas de *búsquedas guardadas* se guardan en la cuenta de almacenamiento. 

```azurecli
$storageAccountId = '/subscriptions/<subscription-id>/resourceGroups/<resource-group-name>/providers/Microsoft.Storage/storageAccounts/<storage name>'
az monitor log-analytics workspace linked-storage create --type Query --resource-group "resource-group-name" --workspace-name "workspace-name" --storage-accounts $storageAccountId
```

```powershell
$storageAccount.Id = Get-AzStorageAccount -ResourceGroupName "resource-group-name" -Name "storage-account-name"
New-AzOperationalInsightsLinkedStorageAccount -ResourceGroupName "resource-group-name" -WorkspaceName "workspace-name" -DataSourceType Query -StorageAccountIds $storageAccount.Id
```

```rst
PUT https://management.azure.com/subscriptions/<subscription-id>/resourcegroups/<resource-group-name>/providers/Microsoft.OperationalInsights/workspaces/<workspace-name>/linkedStorageAccounts/Query?api-version=2020-08-01
Authorization: Bearer <token> 
Content-type: application/json
 
{
  "properties": {
    "dataSourceType": "Query", 
    "storageAccountIds": 
    [
      "/subscriptions/<subscription-id>/resourceGroups/<resource-group-name>/providers/Microsoft.Storage/storageAccounts/<storage-account-name>"
    ]
  }
}
```

Después de la configuración, se guardará en el almacenamiento cualquier nueva consulta de *búsqueda guardada*.

**Configuración de BYOS para las consultas de alertas del registro**

Vincule una cuenta de almacenamiento de *Alertas* con el área de trabajo. Las consultas de *alertas del registro* se guardan en la cuenta de almacenamiento. 

```azurecli
$storageAccountId = '/subscriptions/<subscription-id>/resourceGroups/<resource-group-name>/providers/Microsoft.Storage/storageAccounts/<storage name>'
az monitor log-analytics workspace linked-storage create --type ALerts --resource-group "resource-group-name" --workspace-name "workspace-name" --storage-accounts $storageAccountId
```

```powershell
$storageAccount.Id = Get-AzStorageAccount -ResourceGroupName "resource-group-name" -Name "storage-account-name"
New-AzOperationalInsightsLinkedStorageAccount -ResourceGroupName "resource-group-name" -WorkspaceName "workspace-name" -DataSourceType Alerts -StorageAccountIds $storageAccount.Id
```

```rst
PUT https://management.azure.com/subscriptions/<subscription-id>/resourcegroups/<resource-group-name>/providers/Microsoft.OperationalInsights/workspaces/<workspace-name>/linkedStorageAccounts/Alerts?api-version=2020-08-01
Authorization: Bearer <token> 
Content-type: application/json
 
{
  "properties": {
    "dataSourceType": "Alerts", 
    "storageAccountIds": 
    [
      "/subscriptions/<subscription-id>/resourceGroups/<resource-group-name>/providers/Microsoft.Storage/storageAccounts/<storage-account-name>"
    ]
  }
}
```

Después de la configuración, se guardará en el almacenamiento cualquier nueva consulta de alertas.

## <a name="customer-lockbox-preview"></a>Caja de seguridad del cliente (versión preliminar)
Caja de seguridad le proporciona el control para aprobar o rechazar la solicitud de ingeniero de Microsoft para acceder a los datos durante una solicitud de soporte técnico.

En Azure Monitor, tiene este control sobre los datos en las áreas de trabajo vinculadas al clúster dedicado de Log Analytics. El control Caja de seguridad se aplica a los datos almacenados en un clúster dedicado de Log Analytics en el que se mantiene aislado en las cuentas de almacenamiento del clúster en su suscripción protegida de Caja de seguridad.  

Más información sobre [Caja de seguridad del cliente de Microsoft Azure](../../security/fundamentals/customer-lockbox-overview.md)

## <a name="customer-managed-key-operations"></a>Operaciones de claves administradas por el cliente

- **Obtención de todos los clústeres de un grupo de recursos**
  
  ```azurecli
  az monitor log-analytics cluster list --resource-group "resource-group-name"
  ```

  ```powershell
  Get-AzOperationalInsightsCluster -ResourceGroupName "resource-group-name"
  ```

  ```rst
  GET https://management.azure.com/subscriptions/<subscription-id>/resourcegroups/<resource-group-name>/providers/Microsoft.OperationalInsights/clusters?api-version=2020-08-01
  Authorization: Bearer <token>
  ```

  **Respuesta**
  
  ```json
  {
    "value": [
      {
        "identity": {
          "type": "SystemAssigned",
          "tenantId": "tenant-id",
          "principalId": "principal-Id"
        },
        "sku": {
          "name": "capacityReservation",
          "capacity": 1000,
          "lastSkuUpdate": "Sun, 22 Mar 2020 15:39:29 GMT"
          },
        "properties": {
           "keyVaultProperties": {
              "keyVaultUri": "https://key-vault-name.vault.azure.net",
              "keyName": "key-name",
              "keyVersion": "current-version"
              },
          "provisioningState": "Succeeded",
          "billingType": "cluster",
          "clusterId": "cluster-id"
        },
        "id": "/subscriptions/subscription-id/resourcegroups/resource-group-name/providers/microsoft.operationalinsights/workspaces/workspace-name",
        "name": "cluster-name",
        "type": "Microsoft.OperationalInsights/clusters",
        "location": "region-name"
      }
    ]
  }
  ```

- **Obtención de todos los clústeres de una suscripción**

  ```azurecli
  az monitor log-analytics cluster list
  ```

  ```powershell
  Get-AzOperationalInsightsCluster
  ```

  ```rst
  GET https://management.azure.com/subscriptions/<subscription-id>/providers/Microsoft.OperationalInsights/clusters?api-version=2020-08-01
  Authorization: Bearer <token>
  ```
    
  **Respuesta**
    
  La misma respuesta que para el "clúster de un grupo de recursos", pero en el ámbito de la suscripción.

- **Actualización de la *reserva de capacidad* en el clúster**

  Cuando cambie el volumen de datos en las áreas de trabajo vinculadas con el tiempo y desee actualizar el nivel de reserva de capacidad adecuadamente. Siga la [actualización del clúster](#update-cluster-with-key-identifier-details) y proporcione el valor de la nueva capacidad. Puede estar en el rango de 1000 a 3000 GB por día y en pasos de 100. Para un nivel superior a 3000 GB por día, comuníquese con su contacto de Microsoft para habilitarlo. Tenga en cuenta que no tiene que proporcionar el cuerpo completo de la solicitud de REST, pero debe incluir la SKU:

  ```azurecli
  az monitor log-analytics cluster update --name "cluster-name" --resource-group "resource-group-name" --sku-capacity daily-ingestion-gigabyte
  ```

  ```powershell
  Update-AzOperationalInsightsCluster -ResourceGroupName "resource-group-name" -ClusterName "cluster-name" -SkuCapacity daily-ingestion-gigabyte
  ```

  ```rst
  PATCH https://management.azure.com/subscriptions/<subscription-id>/resourceGroups/<resource-group-name>/providers/Microsoft.OperationalInsights/clusters/<cluster-name>?api-version=2020-08-01
  Authorization: Bearer <token>
  Content-type: application/json

  {
    "sku": {
      "name": "capacityReservation",
      "Capacity": daily-ingestion-gigabyte
    }
  }
  ```

- **Actualización de *billingType* en el clúster**

  La propiedad *billingType* determina la atribución de facturación para el clúster y sus datos:
  - *Clúster* (valor predeterminado): la facturación se atribuye a la suscripción que hospeda los recursos de Clúster
  - *Áreas de trabajo*: la facturación se atribuye a las suscripciones que hospedan las áreas de trabajo de forma proporcional
  
  Siga la [actualización del clúster](#update-cluster-with-key-identifier-details) y proporcione el nuevo valor de billingType. Tenga en cuenta que no tiene que proporcionar el cuerpo completo de la solicitud de REST y debe incluir el *billingType*:

  ```rst
  PATCH https://management.azure.com/subscriptions/<subscription-id>/resourceGroups/<resource-group-name>/providers/Microsoft.OperationalInsights/clusters/<cluster-name>?api-version=2020-08-01
  Authorization: Bearer <token>
  Content-type: application/json

  {
    "properties": {
      "billingType": "cluster",
      }  
  }
  ``` 

- **Desvinculación de un área de trabajo**

  Necesita permisos de "escritura" en el área de trabajo y en el clúster para realizar esta operación. Puede desvincular un área de trabajo del clúster en cualquier momento. Los nuevos datos ingeridos después de la operación de desvinculación se almacenan en el almacenamiento de Log Analytics y se cifran con la clave de Microsoft. Puede consultar los datos ingeridos en el área de trabajo antes y después de la desvinculación sin problemas, siempre que el clúster esté aprovisionado y configurado con la clave de Key Vault válida.

  Esta operación es asincrónica y puede tardar un tiempo en completarse.

  ```azurecli
  az monitor log-analytics workspace linked-service delete --resource-group "resource-group-name" --name "cluster-name" --workspace-name "workspace-name"
  ```

  ```powershell
  Remove-AzOperationalInsightsLinkedService -ResourceGroupName "resource-group-name" -Name "workspace-name" -LinkedServiceName cluster
  ```

  ```rest
  DELETE https://management.azure.com/subscriptions/<subscription-id>/resourcegroups/<resource-group-name>/providers/microsoft.operationalinsights/workspaces/<workspace-name>/linkedservices/cluster?api-version=2020-08-01
  Authorization: Bearer <token>
  ```

  - **Comprobación del estado de vinculación del área de trabajo**
  
  Realice una operación Get en el área de trabajo y observe si la propiedad *clusterResourceId* presente en la respuesta en la sección *features*. Un área de trabajo vinculada tendrá la propiedad *clusterResourceId*.

  ```azurecli
  az monitor log-analytics cluster show --resource-group "resource-group-name" --name "cluster-name"
  ```

  ```powershell
  Get-AzOperationalInsightsWorkspace -ResourceGroupName "resource-group-name" -Name "workspace-name"
  ```

- **Eliminación del clúster**

  Necesita permisos de "escritura" en el clúster para realizar esta operación. Se realiza una operación de eliminación temporal para permitir la recuperación del clúster, incluidos los datos de un período de catorce días, tanto si la eliminación fue accidental como si fue intencionada. El nombre del clúster permanece reservado durante el período de eliminación temporal y no se puede crear un nuevo clúster con ese nombre. Después del período de eliminación temporal, se libera el nombre del clúster. El clúster y sus datos se eliminan de forma permanente y no se pueden recuperar. Cualquier área de trabajo vinculada se desvincula del clúster en la operación de eliminación. Los nuevos datos ingeridos se almacenan en el almacenamiento compartido de Log Analytics y se cifran con la clave de Microsoft. 
  
  La operación de desvinculación es asincrónica y puede tardar hasta 90 minutos en completarse.

  ```azurecli
  az monitor log-analytics cluster delete --resource-group "resource-group-name" --name "cluster-name"
  ```
 
  ```powershell
  Remove-AzOperationalInsightsCluster -ResourceGroupName "resource-group-name" -ClusterName "cluster-name"
  ```

  ```rst
  DELETE https://management.azure.com/subscriptions/<subscription-id>/resourceGroups/<resource-group-name>/providers/Microsoft.OperationalInsights/clusters/<cluster-name>?api-version=2020-08-01
  Authorization: Bearer <token>
  ```
  
- **Recuperación del clúster y los datos** 
  
  Un clúster que se eliminó durante los últimos 14 días se encuentra en estado de eliminación temporal y se puede recuperar con sus datos. Dado que todas las áreas de trabajo se han desasociado de la eliminación del clúster, debe volver a vincular las áreas de trabajo después de recuperar el clúster. El grupo de productos actualmente lo realiza de manera manual. Use el canal de Microsoft o abra una solicitud de soporte técnico para la recuperación del clúster eliminado.

## <a name="limitations-and-constraints"></a>Limitaciones y restricciones

- La clave administrada por el cliente es compatible con un clúster de Log Analytics dedicado y es adecuada para los clientes que envían 1 TB al día o más.

- El número máximo de clústeres por región y suscripción es 2.

- El número máximo de áreas de trabajo vinculadas al clúster es 1000.

- Puede vincular un área de trabajo al clúster y desvincularla después. El número de operaciones de vinculación de área de trabajo en un área de trabajo determinada en un período de 30 días se limita a 2.

- La vinculación de área de trabajo al clúster SOLO se debe llevar a cabo después de comprobar que se completó el aprovisionamiento del clúster de Log Analytics. Los datos que se envíen al área de trabajo antes de que se complete el aprovisionamiento se eliminarán y no se podrán recuperar.

- El cifrado de claves administradas por el cliente se aplica a los datos ingeridos después del momento de la configuración. Los datos que se ingieren antes de la configuración, permanecen cifrados con la clave de Microsoft. Puede consultar fácilmente los datos ingeridos antes y después de la configuración de claves administradas por el cliente.

- La instancia de Azure Key Vault debe configurarse como recuperable. Las siguientes propiedades no están habilitadas de forma predeterminada y deben configurarse mediante la CLI o PowerShell:<br>
  - [eliminación temporal](../../key-vault/general/soft-delete-overview.md)
  - La [protección de purgas](../../key-vault/general/soft-delete-overview.md#purge-protection) debe estar activada si quiere tener protección frente a posibles eliminaciones forzadas de secretos o del almacén, incluso después de su eliminación temporal.

- Actualmente no se admite el traslado de un clúster a otro grupo de recursos o a otra suscripción.

- La instancia de Azure Key Vault, el clúster y las áreas de trabajo vinculadas deben estar en la misma región y en el mismo inquilino de Azure Active Directory (Azure AD), pero pueden estar en distintas suscripciones.

- Se producirá un error en la vinculación del área de trabajo al clúster si está vinculada a otro clúster.

## <a name="troubleshooting"></a>Solución de problemas

- Comportamiento con la disponibilidad de Key Vault
  - En el funcionamiento normal, Storage almacena en caché la AEK durante breves períodos de tiempo y vuelve regularmente a Key Vault para desajustarla.
    
  - En el caso de los errores de conexión transitorios (tiempos de espera, errores de conexión, problemas de DNS), Storage permite que las claves permanezcan en la memoria caché durante un período de tiempo más prolongado; así es más fácil resolver pequeños problemas de disponibilidad. Las funciones de consulta e ingesta continúan sin interrupción.
    
  - La falta de disponibilidad de un sitio activo durante unos 30 minutos hará que la cuenta de Storage deje de estar disponible. La función de consulta no está disponible y los datos ingeridos se copian en caché durante varias horas mediante la clave de Microsoft para evitar la pérdida de datos. Cuando se restaura el acceso a Key Vault, la consulta está disponible y los datos copiados en caché temporalmente se ingieren en el almacén de datos y se cifran con la clave administrada por el cliente.

  - Frecuencia de acceso a Key Vault: la frecuencia con la que Azure Monitor Storage accede a Key Vault para las operaciones de encapsulado y desencapsulado es de entre 6 y 60 segundos.

- Si crea un clúster y especifica el valor de KeyVaultProperties inmediatamente, puede producirse un error en la operación, ya que la directiva de acceso no se puede definir hasta que se asigne la identidad del sistema al clúster.

- Si actualiza un clúster existente con KeyVaultProperties y la directiva de acceso de clave "Get" no se encuentra en Key Vault, se producirá un error en la operación.

- Si se produce un error de conflicto al crear un clúster, es posible que haya eliminado el clúster en los últimos 14 días y que esté en un período de eliminación temporal. El nombre del clúster permanece reservado durante el período de eliminación temporal y no se puede crear un nuevo clúster con ese nombre. El nombre se libera después del período de eliminación temporal cuando el clúster se elimina de forma permanente.

- Si actualiza su clúster mientras una operación está en curso, se producirá un error en dicha operación.

- Si no puede implementar el clúster, compruebe que la instancia de Azure Key Vault, el clúster y las áreas de trabajo de Log Analytics vinculadas se encuentran en la misma región. Puede estar en distintas suscripciones.

- Si actualiza la versión de la clave en Key Vault y no actualiza los nuevos detalles del identificador de clave en el clúster, el clúster de Log Analytics seguirá usando la clave anterior y los datos serán inaccesibles. Actualice los nuevos detalles de identificador de clave del clúster para reanudar la ingesta de datos y la capacidad de consultar los datos.

- Algunas operaciones son largas y pueden tardar un tiempo en completarse. Estas operaciones son las de creación de clúster, actualización de la clave de clúster y eliminación de clúster. Puede comprobar el estado de la operación de dos maneras:
  1. Cuando se utiliza REST, copie el valor de la dirección URL de Azure-AsyncOperation de la respuesta y siga la [comprobación del estado de operaciones asincrónicas](#asynchronous-operations-and-status-check).
  2. Envíe una solicitud GET al clúster o al área de trabajo y observe la respuesta. Por ejemplo, un área de trabajo desvinculada no tendrá el elemento *clusterResourceId* en la sección *features*.

- Para obtener soporte técnico y ayuda relacionada con la clave administrada por el cliente, use sus contactos en Microsoft.

- Mensajes de error
  
  Creación de un clúster:
  -  400: El nombre del clúster no es válido. El nombre del clúster puede contener los caracteres a-z, A-Z, 0-9 y una longitud de 3 a 63.
  -  400: El cuerpo de la solicitud es NULL o tiene un formato incorrecto.
  -  400: El nombre de SKU no es válido. Establezca el nombre de SKU en capacityReservation.
  -  400: Se proporcionó Capacity, pero la SKU no es capacityReservation. Establezca el nombre de la SKU en capacityReservation.
  -  400: Falta Capacity en la SKU. Establezca el valor de Capacity en 1000 o más en incrementos de 100 (GB).
  -  400: Capacity en la SKU no está en el rango. Debe ser 1000 como mínimo y hasta la capacidad máxima permitida que está disponible en "Uso y costos estimados" en el área de trabajo.
  -  400: Capacity está bloqueado durante 30 días. Se permite la reducción de la capacidad 30 días después de la actualización.
  -  400: No se estableció ninguna SKU. Establezca el nombre de la SKU en capacityReservation y el valor de Capacity en 1000 o más en incrementos de 100 (GB).
  -  400: Identity es NULL o está vacío. Establezca Identity en el tipo systemAssigned.
  -  400: KeyVaultProperties se estableció en la creación. Actualice KeyVaultProperties después de la creación del clúster.
  -  400: No se puede ejecutar la operación ahora. La operación asincrónica está en un estado distinto del correcto. El clúster debe completar su operación antes de realizar cualquier operación de actualización.

  Actualización del clúster:
  -  400: El clúster está en estado de eliminación. La operación asincrónica está en curso. El clúster debe completar su operación antes de realizar cualquier operación de actualización.
  -  400: KeyVaultProperties no está vacío, pero tiene un formato incorrecto. Consulte [actualización de identificador de clave](#update-cluster-with-key-identifier-details).
  -  400: No se pudo validar la clave en Key Vault. Podría deberse a la falta de permisos o a que la clave no existe. Verifique que [estableció la clave y la directiva de acceso](#grant-key-vault-permissions) en Key Vault.
  -  400: No se puede recuperar la clave. Key Vault debe establecerse para la eliminación temporal y protección de purga. Consulte la [documentación de Key Vault](../../key-vault/general/soft-delete-overview.md).
  -  400: No se puede ejecutar la operación ahora. Espere a que se complete la operación asincrónica e inténtelo de nuevo.
  -  400: El clúster está en estado de eliminación. Espere a que se complete la operación asincrónica e inténtelo de nuevo.

  Obtención del clúster:
    -  404: No se encontró el clúster, es posible que se haya eliminado. Si intenta crear un clúster con ese nombre y obtiene un conflicto, el clúster se encuentra en eliminación temporal durante 14 días. Puede ponerse en contacto con soporte técnico para recuperarlo o usar otro nombre para crear un nuevo clúster. 

  Eliminación del clúster:
    -  409: No se puede eliminar un clúster mientras está en estado de aprovisionamiento. Espere a que se complete la operación asincrónica e inténtelo de nuevo.

  Vínculo del área de trabajo:
  -  404: No se encuentra el área de trabajo. El área de trabajo que especificó no existe o se eliminó.
  -  409: Operación en curso de vinculación o desvinculación del área de trabajo.
  -  400: No se encontró el clúster, el clúster que especificó no existe o se eliminó. Si intenta crear un clúster con ese nombre y obtiene un conflicto, el clúster se encuentra en eliminación temporal durante 14 días. Puede ponerse en contacto con el soporte técnico para recuperarlo.

  Desvinculación del área de trabajo:
  -  404: No se encuentra el área de trabajo. El área de trabajo que especificó no existe o se eliminó.
  -  409: Operación en curso de vinculación o desvinculación del área de trabajo.
