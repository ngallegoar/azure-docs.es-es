---
title: Clústeres dedicados de registros de Azure Monitor
description: Los clientes que ingieren más de 1 TB al día de datos de supervisión pueden usar clústeres dedicados en lugar de compartidos.
ms.subservice: logs
ms.topic: conceptual
author: rboucher
ms.author: robb
ms.date: 09/16/2020
ms.openlocfilehash: d261640dfdb59b2b06cfe3066fca26640a0bed54
ms.sourcegitcommit: 642988f1ac17cfd7a72ad38ce38ed7a5c2926b6c
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 11/18/2020
ms.locfileid: "94874651"
---
# <a name="azure-monitor-logs-dedicated-clusters"></a>Clústeres dedicados de registros de Azure Monitor

Los clústeres dedicados de registros de Azure Monitor son una opción de implementación que está disponible para ofrecer mejor servicio a los clientes de gran volumen. Los clientes que ingieren más de 4 TB de datos al día usarán clústeres dedicados. Los clientes con clústeres dedicados pueden elegir las áreas de trabajo que se van a hospedar en estos clústeres.

Además de la posibilidad de un gran volumen, existen otras ventajas al usar clústeres dedicados:

- **Límite de frecuencia**: Un cliente puede tener [límites de tasa de ingesta](../service-limits.md#data-ingestion-volume-rate) superiores solo en un clúster dedicado.
- **Características**: Algunas características empresariales solo están disponibles en clústeres dedicados, en concreto, las claves administradas por el cliente (CMK) y la compatibilidad con Caja de seguridad. 
- **Coherencia**: Los clientes tienen sus propios recursos dedicados y, por lo tanto, no hay influencia de otros clientes que funcionen en la misma infraestructura compartida.
- **Rentabilidad**: Puede ser más rentable usar un clúster dedicado, ya que los niveles de reserva de capacidad asignados tienen en cuenta toda la ingesta del clúster y se aplican a todas sus áreas de trabajo, incluso si algunas de ellas son pequeñas y no son válidas para el descuento de la reserva de capacidad.
- Las consultas **entre áreas de trabajo** se ejecutan más rápido si todas las áreas de trabajo están en el mismo clúster.

Los clústeres dedicados requieren que los clientes confirmen el uso de una capacidad de al menos 1 TB de ingesta de datos al día. La migración a un clúster dedicado es sencilla. No se produce ninguna pérdida de datos ni interrupción del servicio. 

> [!IMPORTANT]
> Los clústeres dedicados están aprobados y son totalmente compatibles con las implementaciones de producción. Sin embargo, debido a las restricciones de capacidad temporales, se exige el registro previo para usar esta característica. Use sus contactos de Microsoft para proporcionar los identificadores de las suscripciones.

## <a name="management"></a>Administración 

Los clústeres dedicados se administran a través de un recurso de Azure que representa clústeres de registro de Azure Monitor. Todas las operaciones se realizan en este recurso mediante PowerShell o la API de REST.

Una vez que se crea el clúster, se puede configurar y se le pueden vincular áreas de trabajo. Cuando un área de trabajo está vinculada a un clúster, los nuevos datos que se envían al área de trabajo residen en el clúster. Solo se pueden vincular al clúster las áreas de trabajo que se encuentran en la misma región que el clúster. Las áreas de trabajo se pueden desvincular de un clúster con algunas limitaciones. En este artículo se incluyen más detalles sobre estas limitaciones. 

Los datos ingeridos en clústeres dedicados se cifran dos veces, una vez en el nivel de servicio mediante claves administradas por Microsoft o [claves administradas por el cliente](../platform/customer-managed-keys.md), y una vez en el nivel de infraestructura mediante dos algoritmos de cifrado diferentes y dos claves diferentes. El [doble cifrado](../../storage/common/storage-service-encryption.md#doubly-encrypt-data-with-infrastructure-encryption) sirve de protección en caso de que uno de los algoritmos o claves de cifrado puedan estar en peligro. En este caso, la capa adicional de cifrado también protege los datos. El clúster dedicado también le permite proteger los datos con un control de [caja de seguridad](../platform/customer-managed-keys.md#customer-lockbox-preview).

Todas las operaciones en el nivel de clúster requieren el permiso de acción `Microsoft.OperationalInsights/clusters/write` en el clúster. Este permiso se puede conceder a través del rol Propietario o Colaborador que contiene la acción `*/write` o a través del rol Colaborador de Log Analytics que contiene la acción `Microsoft.OperationalInsights/*`. Para obtener más información sobre los permisos de Log Analytics, consulte [Administración del acceso a los datos de registro y las áreas de trabajo en Azure Monitor](../platform/manage-access.md). 


## <a name="cluster-pricing-model"></a>Modelo de precios de clúster

Los clústeres dedicados de Log Analytics usan un modelo de precios de reserva de capacidad de al menos 1000 GB/día. Cualquier uso por encima del nivel de reserva se facturará según la tarifa de pago por uso.  La información de precios de reserva de capacidad está disponible en la [página de precios de Azure Monitor]( https://azure.microsoft.com/pricing/details/monitor/).  

El nivel de reserva de capacidad del clúster se configura mediante programación con Azure Resource Manager con el parámetro `Capacity` en `Sku`. El parámetro `Capacity` se especifica en unidades de GB y puede tener valores de 1000 GB/día o más en incrementos de 100 GB/día.

Hay dos modos de facturación para el uso en un clúster. Estos pueden especificarse mediante el parámetro `billingType` al configurar el clúster. 

1. **Clúster**: en este caso (que es el modo predeterminado), la facturación de los datos ingeridos se realiza en el nivel de clúster. Las cantidades de datos ingeridas desde cada área de trabajo asociada a un clúster se suman para calcular la factura diaria del clúster. 

2. **Áreas de trabajo**: los costos de reserva de capacidad para el clúster se atribuyen proporcionalmente a las áreas de trabajo del clúster (después de tener en cuenta las asignaciones por nodo de [Azure Security Center](../../security-center/index.yml) para cada área de trabajo).

Tenga en cuenta que, si el área de trabajo usa el plan de tarifa heredado por nodo, cuando esté vinculado a un clúster se le facturará en función de los datos ingeridos en relación con la reserva de capacidad del clúster y ya no por nodo. Se seguirán aplicando las asignaciones de datos por nodo de Azure Security Center.

Encontrará más detalles sobre la facturación de los clústeres dedicados de Log Analytics [aquí]( https://docs.microsoft.com/azure/azure-monitor/platform/manage-cost-storage#log-analytics-dedicated-clusters).


## <a name="creating-a-cluster"></a>Creación de un clúster

Primero debe crear los recursos de clúster para empezar a crear un clúster dedicado.

Deben especificarse las siguientes propiedades:

- **Nombre del clúster**: Se usa con fines administrativos. Los usuarios no se exponen a este nombre.
- **ResourceGroupName**: Como para cualquier recurso de Azure, los clústeres pertenecen a un grupo de recursos. Se recomienda usar un grupo de recursos de TI central, ya que los clústeres suelen compartirse entre muchos equipos dentro de una organización. Para más información sobre el diseño, consulte [Diseño de la implementación de registros de Azure Monitor](../platform/design-logs-deployment.md).
- **Ubicación**: Un clúster se encuentra en una región de Azure específica. Solo las áreas de trabajo que se encuentran en esta región se pueden vincular a este clúster.
- **SkuCapacity**: Debe especificar el nivel (sku) de *capacidad de reserva* al crear un recurso *cluster*. El nivel de *reserva de capacidad* puede estar en el intervalo de 1000 a 3000 GB por día. Puede actualizarlo en incrementos de 100 más adelante si es necesario. Si necesita un nivel de reserva de capacidad superior a 3000 GB por día, comuníquese con nosotros en LAIngestionRate@microsoft.com. Para obtener más información sobre los costos del clúster, consulte [Administración de costos de clústeres de Log Analytics](../platform/manage-cost-storage.md#log-analytics-dedicated-clusters).

Después de crear el recurso *cluster*, puede editar propiedades adicionales, como *sku*, *keyVaultProperties o *billingType*. Vea más detalles a continuación.

> [!WARNING]
> La creación del clúster desencadena la asignación y el aprovisionamiento de recursos. Esta operación puede tardar hasta una hora en completarse. Se recomienda ejecutarla de forma asincrónica.

La cuenta de usuario que crea los clústeres debe tener el permiso de creación de recursos de Azure estándar, `Microsoft.Resources/deployments/*`, y el permiso de escritura de clúster `(Microsoft.OperationalInsights/clusters/write)`.

### <a name="create"></a>Crear 

**PowerShell**

```powershell
New-AzOperationalInsightsCluster -ResourceGroupName {resource-group-name} -ClusterName {cluster-name} -Location {region-name} -SkuCapacity {daily-ingestion-gigabyte} -AsJob

# Check when the job is done
Get-Job -Command "New-AzOperationalInsightsCluster*" | Format-List -Property *
```

**REST**

*Call* 
```rst
PUT https://management.azure.com/subscriptions/<subscription-id>/resourceGroups/<resource-group-name>/providers/Microsoft.OperationalInsights/clusters/<cluster-name>?api-version=2020-03-01-preview
Authorization: Bearer <token>
Content-type: application/json

{
  "identity": {
    "type": "systemAssigned"
    },
  "sku": {
    "name": "capacityReservation",
    "Capacity": 1000
    },
  "properties": {
    "billingType": "cluster",
    },
  "location": "<region-name>",
}
```

*Respuesta*

Debe ser 200 Correcto y un encabezado.

### <a name="check-provisioning-status"></a>Comprobación del estado de aprovisionamiento

El aprovisionamiento del clúster de Log Analytics tarda un tiempo en completarse. Puede comprobar el estado de aprovisionamiento de varias maneras:

- Ejecute el comando de PowerShell Get-AzOperationalInsightsCluster con el nombre del grupo de recursos y compruebe la propiedad ProvisioningState. El valor es *ProvisioningAccount* mientras se realiza el aprovisionamiento y *Succeeded* una vez que se completa.
  ```powershell
  New-AzOperationalInsightsCluster -ResourceGroupName {resource-group-name} 
  ```

- Copie el valor de la dirección URL de Azure-AsyncOperation de la respuesta y siga la comprobación del estado de operaciones asincrónicas.

- Envíe una solicitud GET en el recurso *Clúster* y examine el valor *provisioningState*. El valor es *ProvisioningAccount* mientras se realiza el aprovisionamiento y *Succeeded* una vez que se completa.

   ```rst
   GET https://management.azure.com/subscriptions/<subscription-id>/resourceGroups/<resource-group-name>/providers/Microsoft.OperationalInsights/clusters/<cluster-name>?api-version=2020-03-01-preview
   Authorization: Bearer <token>
   ```

   **Respuesta**

   ```json
   {
     "identity": {
       "type": "SystemAssigned",
       "tenantId": "tenant-id",
       "principalId": "principal-id"
       },
     "sku": {
       "name": "capacityReservation",
       "capacity": 1000,
       "lastSkuUpdate": "Sun, 22 Mar 2020 15:39:29 GMT"
       },
     "properties": {
       "provisioningState": "ProvisioningAccount",
       "billingType": "cluster",
       "clusterId": "cluster-id"
       },
     "id": "/subscriptions/subscription-id/resourceGroups/resource-group-name/providers/Microsoft.OperationalInsights/clusters/cluster-name",
     "name": "cluster-name",
     "type": "Microsoft.OperationalInsights/clusters",
     "location": "region-name"
   }
   ```

El GUID *principalId* se genera desde el servicio de identidad administrada para el recurso *cluster*.

## <a name="change-cluster-properties"></a>Comprobación de las propiedades del clúster

Después de crear el recurso *cluster* y de que esté totalmente aprovisionado, puede editar propiedades adicionales en el nivel de clúster mediante PowerShell o la API de REST. Además de las propiedades que están disponibles durante la creación del clúster, solo se pueden establecer propiedades adicionales después de haber aprovisionado el clúster:

- **keyVaultProperties**: Se usa para configurar la instancia de Azure Key Vault que se usa para aprovisionar una [clave administradas por el cliente de Azure Monitor](../platform/customer-managed-keys.md#customer-managed-key-provisioning-procedure). Contiene los parámetros siguientes:  *KeyVaultUri*, *KeyName*, *KeyVersion*. 
- **billingType**: La propiedad *billingType* determina la atribución de facturación para el recurso *cluster* y sus datos:
  - **Cluster** (valor predeterminado): Los costos de Reserva de capacidad para el clúster se atribuyen al recurso *cluster*.
  - **Workspaces**: Los costos de Reserva de capacidad para el clúster se atribuyen proporcionalmente a las áreas de trabajo del clúster. En este caso, se factura una parte del uso del recurso *cluster* si el total de datos ingeridos del día está por debajo de la Reserva de capacidad. Vea [Clústeres dedicados de Log Analytics](../platform/manage-cost-storage.md#log-analytics-dedicated-clusters) para obtener más información sobre el modelo de precios del clúster. 

> [!NOTE]
> La propiedad *billingType* no se admite en PowerShell.
> Es posible que las actualizaciones de las propiedades del clúster se ejecuten de forma asincrónica y que tarden en completarse.

**PowerShell**

```powershell
Update-AzOperationalInsightsCluster -ResourceGroupName {resource-group-name} -ClusterName {cluster-name} -KeyVaultUri {key-uri} -KeyName {key-name} -KeyVersion {key-version}
```

**REST**

> [!NOTE]
> Puede actualizar la *SKU* del recurso de *clúster* y el valor de *keyVaultProperties* o *billingType* mediante PATCH.

Por ejemplo: 

*Call*

```rst
PATCH https://management.azure.com/subscriptions/<subscription-id>/resourceGroups/<resource-group-name>/providers/Microsoft.OperationalInsights/clusters/<cluster-name>?api-version=2020-03-01-preview
Authorization: Bearer <token>
Content-type: application/json

{
   "sku": {
     "name": "capacityReservation",
     "capacity": <capacity-reservation-amount-in-GB>
     },
   "properties": {
    "billingType": "cluster",
     "KeyVaultProperties": {
       "KeyVaultUri": "https://<key-vault-name>.vault.azure.net",
       "KeyName": "<key-name>",
       "KeyVersion": "<current-version>"
       }
   },
   "location":"<region-name>"
}
```

"KeyVaultProperties" contiene los detalles del identificador de la clave del almacén de claves.

*Respuesta*

200 Correcto y encabezado

### <a name="check-cluster-update-status"></a>Comprobación del estado de actualización del clúster

La propagación del identificador de clave tarda unos minutos en completarse. Puede comprobar el estado de actualización de dos maneras:

- Copie el valor de la dirección URL de Azure-AsyncOperation de la respuesta y siga la comprobación del estado de operaciones asincrónicas. 

   O

- Envíe una solicitud GET en el recurso *Clúster* y examine las propiedades *KeyVaultProperties*. Los detalles del identificador de clave que actualizó recientemente se deben devolver en la respuesta.

   Una respuesta a la solicitud GET en el recurso *Clúster* debe ser similar a esta cuando se complete la actualización del identificador de clave:

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

## <a name="link-a-workspace-to-the-cluster"></a>Vinculación de un área de trabajo al clúster

Cuando un área de trabajo está vinculada a un clúster dedicado, los nuevos datos que se ingieren en el área de trabajo se enrutan al nuevo clúster, mientras que los datos existentes permanecen en el clúster existente. Si el clúster dedicado se cifra mediante claves administradas por el cliente (CMK), solo se cifrarán los datos nuevos con la clave. El sistema abstrae esta diferencia de los usuarios, y estos simplemente consultan el área de trabajo como de costumbre mientras el sistema realiza consultas entre clústeres en el back-end.

Un clúster puede vincularse con hasta 100 áreas de trabajo. Las áreas de trabajo vinculadas se encuentran en la misma región que el clúster. Para proteger el back-end del sistema y evitar la fragmentación de los datos, un área de trabajo no se puede vincular a un clúster más de dos veces al mes.

Para realizar la operación de vinculación, debe tener permisos de "escritura" tanto en el área de trabajo como en el recurso *cluster*:

- En el área de trabajo: *Microsoft.OperationalInsights/workspaces/write*
- En el recurso *cluster*: *Microsoft.OperationalInsights/clusters/write*

Además de los aspectos de facturación, el área de trabajo vinculada mantiene su propia configuración, como la duración de la retención de datos.
El área de trabajo y el clúster pueden estar en distintas suscripciones. Es posible que el área de trabajo y el clúster estén en inquilinos diferentes si se usa Azure Lighthouse para asignar ambos a un único inquilino.

Como con cualquier operación con clústeres, vincular un área de trabajo puede realizarse una vez completado el aprovisionamiento del clúster de Log Analytics.

> [!WARNING]
> Vincular un área de trabajo a un clúster requiere sincronizar varios componentes de back-end y garantizar la hidratación de la memoria caché. Esta operación puede tardar hasta dos horas en completarse. Se recomienda ejecutarla de forma asincrónica.


**PowerShell**

Use el comando de PowerShell siguiente para vincular un clúster:

```powershell
# Find cluster resource ID
$clusterResourceId = (Get-AzOperationalInsightsCluster -ResourceGroupName {resource-group-name} -ClusterName {cluster-name}).id

# Link the workspace to the cluster
Set-AzOperationalInsightsLinkedService -ResourceGroupName {resource-group-name} -WorkspaceName {workspace-name} -LinkedServiceName cluster -WriteAccessResourceId $clusterResourceId -AsJob

# Check when the job is done
Get-Job -Command "Set-AzOperationalInsightsLinkedService" | Format-List -Property *
```


**REST**

Use la llamada de REST siguiente para vincular un clúster:

*Envío*

```rst
PUT https://management.azure.com/subscriptions/<subscription-id>/resourcegroups/<resource-group-name>/providers/microsoft.operationalinsights/workspaces/<workspace-name>/linkedservices/cluster?api-version=2020-03-01-preview 
Authorization: Bearer <token>
Content-type: application/json

{
  "properties": {
    "WriteAccessResourceId": "/subscriptions/<subscription-id>/resourcegroups/<resource-group-name>/providers/microsoft.operationalinsights/clusters/<cluster-name>"
    }
}
```

*Respuesta*

200 Correcto y encabezado.

### <a name="using-customer-managed-keys-with-linking"></a>Uso de claves administradas por el cliente con la vinculación

Si usa claves administrada por el cliente, los datos ingeridos se almacenan cifrados con la clave administrada después de la operación de asociación, que puede tardar hasta 90 minutos en completarse. 

Puede comprobar el estado de asociación del área de trabajo de dos maneras:

- Copie el valor de la dirección URL de Azure-AsyncOperation de la respuesta y siga la comprobación del estado de operaciones asincrónicas.

- Envíe una solicitud [Workspaces - Get](/rest/api/loganalytics/workspaces/get) y observe la respuesta. El área de trabajo asociada tiene clusterResourceId en "features".

Una solicitud de envío será similar a la siguiente:

*Envío*

```rest
GET https://management.azure.com/subscriptions/<subscription-id>/resourcegroups/<resource-group-name>/providers/microsoft.operationalInsights/workspaces/<workspace-name>?api-version=2020-03-01-preview
Authorization: Bearer <token>
```

*Respuesta*

```json
{
  "properties": {
    "source": "Azure",
    "customerId": "workspace-name",
    "provisioningState": "Succeeded",
    "sku": {
      "name": "pricing-tier-name",
      "lastSkuUpdate": "Tue, 28 Jan 2020 12:26:30 GMT"
    },
    "retentionInDays": 31,
    "features": {
      "legacy": 0,
      "searchVersion": 1,
      "enableLogAccessUsingOnlyResourcePermissions": true,
      "clusterResourceId": "/subscriptions/subscription-id/resourceGroups/resource-group-name/providers/Microsoft.OperationalInsights/clusters/cluster-name"
    },
    "workspaceCapping": {
      "dailyQuotaGb": -1.0,
      "quotaNextResetTime": "Tue, 28 Jan 2020 14:00:00 GMT",
      "dataIngestionStatus": "RespectQuota"
    }
  },
  "id": "/subscriptions/subscription-id/resourcegroups/resource-group-name/providers/microsoft.operationalinsights/workspaces/workspace-name",
  "name": "workspace-name",
  "type": "Microsoft.OperationalInsights/workspaces",
  "location": "region-name"
}
```

## <a name="unlink-a-workspace-from-a-dedicated-cluster"></a>Desvinculación de un área de trabajo de un clúster dedicado

Puede desvincular un área de trabajo de un clúster. Después de desvincular un área de trabajo del clúster, los nuevos datos asociados a esta área de trabajo no se envían al clúster dedicado. Además, la facturación del área de trabajo ya no se realiza a través del clúster. Los datos antiguos del área de trabajo desvinculada podrían quedar en el clúster. Si estos datos están cifrados mediante claves administradas por el cliente (CMK), se conservan los secretos de Key Vault. El sistema abstrae este cambio de los usuarios de Log Analytics. Los usuarios pueden simplemente consultar el área de trabajo como de costumbre. El sistema realiza consultas entre clústeres en el back-end según sea necesario sin indicarlo a los usuarios.  

> [!WARNING] 
> Hay un límite de dos operaciones de vinculación por área de trabajo dentro de un mes. Dedique tiempo a analizar y planear las acciones de desvinculación en consecuencia. 

## <a name="delete-a-dedicated-cluster"></a>Eliminación de un clúster dedicado

Se puede eliminar un recurso de clúster dedicado. Debe desvincular todas las áreas de trabajo del clúster antes de eliminarlo. Necesita permisos de escritura en el recurso *Clúster* para realizar esta operación. 

Una vez que se elimina el recurso de clúster, el clúster físico entra en un proceso de purga y eliminación. La eliminación de un clúster elimina todos los datos que se han almacenado en el clúster. Los datos podrían ser de áreas de trabajo que se vincularon al clúster en el pasado.

Un recurso *Clúster* que se eliminó durante los últimos 14 días se encuentra en estado de eliminación temporal y se puede recuperar. Dado que todas las áreas de trabajo se han desasociado del recurso *cluster* con eliminación del recurso *cluster*, debe volver a asociar las áreas de trabajo después de la recuperación. El usuario no puede realizar la operación de recuperación. Póngase en contacto con el canal de Microsoft o con soporte técnico si tiene solicitudes de recuperación.

Dentro de los 14 días posteriores a la eliminación, el nombre del recurso de clúster está reservado y otros recursos no pueden usarlo.

**PowerShell**

Use el comando de PowerShell siguiente para eliminar un clúster:

  ```powershell
  Remove-AzOperationalInsightsCluster -ResourceGroupName "resource-group-name" -ClusterName "cluster-name"
  ```

**REST**

Use la llamada de REST siguiente para eliminar un clúster:

  ```rst
  DELETE https://management.azure.com/subscriptions/<subscription-id>/resourceGroups/<resource-group-name>/providers/Microsoft.OperationalInsights/clusters/<cluster-name>?api-version=2020-08-01
  Authorization: Bearer <token>
  ```

  **Respuesta**

  200 OK



## <a name="next-steps"></a>Pasos siguientes

- Obtenga más información sobre la [facturación de clústeres dedicados de Log Analytics](../platform/manage-cost-storage.md#log-analytics-dedicated-clusters)
- Obtenga más información sobre el [diseño adecuado de áreas de trabajo de Log Analytics](../platform/design-logs-deployment.md)