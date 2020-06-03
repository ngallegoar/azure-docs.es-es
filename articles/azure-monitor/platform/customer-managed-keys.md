---
title: Clave administrada por el cliente de Azure Monitor
description: Información y pasos para configurar la clave administrada por el cliente (CMK) para cifrar los datos en las áreas de trabajo de Log Analytics mediante una clave de Azure Key Vault.
ms.subservice: logs
ms.topic: conceptual
author: yossi-y
ms.author: yossiy
ms.date: 05/20/2020
ms.openlocfilehash: 037edb8af6e04a2ff65977a92a66482c9f4f880f
ms.sourcegitcommit: 1f25aa993c38b37472cf8a0359bc6f0bf97b6784
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 05/26/2020
ms.locfileid: "83845105"
---
# <a name="azure-monitor-customer-managed-key"></a>Clave administrada por el cliente de Azure Monitor 

En este artículo se proporcionan información general y los pasos para configurar las claves administradas por el cliente (CMK) para las áreas de trabajo de Log Analytics. Una vez configuradas, los datos enviados a sus áreas de trabajo se cifran con su clave de Azure Key Vault.

Se recomienda revisar la sección [Limitaciones y restricciones](#limitationsandconstraints) que aparece más abajo antes de proceder con la configuración.

## <a name="customer-managed-key-cmk-overview"></a>Información general de la clave administrada por el cliente (CMK)

[El cifrado en reposo](https://docs.microsoft.com/azure/security/fundamentals/encryption-atrest)  es un requisito común de privacidad y seguridad en las organizaciones. Puede dejar que Azure administre completamente el cifrado en reposo, mientras dispone de varias opciones para administrar minuciosamente el cifrado o las claves de cifrado.

Azure Monitor garantiza que todos los datos se cifren en reposo mediante claves administradas por Azure. Azure Monitor también proporciona una opción para el cifrado de datos mediante su propia clave almacenada en  [Azure Key Vault](https://docs.microsoft.com/azure/key-vault/key-vault-overview) , al que se tiene acceso mediante la autenticación de la  [identidad administrada](https://docs.microsoft.com/azure/active-directory/managed-identities-azure-resources/overview)  asignada por el sistema. Esta clave se puede  [proteger mediante software o con módulos de seguridad de hardware (HSM)](https://docs.microsoft.com/azure/key-vault/key-vault-overview).

El uso que hace Azure Monitor del cifrado es idéntico al del  [cifrado de Azure Storage](https://docs.microsoft.com/azure/storage/common/storage-service-encryption#about-azure-storage-encryption) .

CMK le permite controlar el acceso a los datos y revocarlos en cualquier momento. Azure Monitor Storage siempre respeta los cambios en los permisos de las claves en el plazo de una hora. Los datos ingeridos en los últimos 14 días también se conservan en la memoria caché activa (respaldada por SSD) para un funcionamiento eficaz del motor de consultas. Estos datos permanecen cifrados con las claves de Microsoft, independientemente de la configuración de CMK, pero el control sobre los datos de SSD se adhiere a [revocación de claves](#cmk-kek-revocation). Estamos trabajando para que los datos de SSD se cifren con CMK en la segunda mitad de 2020.

La funcionalidad de CMK se proporciona en clústeres de Log Analytics dedicados. Para comprobar que tenemos la capacidad necesaria en su región, es necesario que la suscripción esté en la lista de permitidos de antemano. Use su contacto de Microsoft para obtener la suscripción de la lista de permitidos antes de empezar a configurar CMK.

El  [modelo de precios de los clústeres de Log Analytics](https://docs.microsoft.com/azure/azure-monitor/platform/manage-cost-storage#log-analytics-dedicated-clusters)  usa reservas de capacidad a partir de un nivel de 1000 GB/día.

## <a name="how-cmk-works-in-azure-monitor"></a>Cómo funciona CMK en Azure Monitor

Azure Monitor aprovecha la identidad administrada asignada por el sistema para conceder acceso a su instancia de Azure Key Vault. La identidad administrada asignada por el sistema solo se puede asociar a un único recurso de Azure, mientras que la identidad del clúster de Log Analytics se admite en el nivel de clúster, lo que indica que la capacidad de CMK se entrega en un clúster de Log Analytics dedicado. Para admitir CMK en varias áreas de trabajo, un nuevo recurso de *Clúster* de Log Analytics debe funcionar como una conexión de identidad intermedia entre la instancia de Key Vault y las áreas de trabajo de Log Analytics. El almacenamiento de clúster de Log Analytics usa la identidad administrada que \'está asociada al recurso de *clúster* para autenticar el acceso y acceder a Azure Key Vault mediante Azure Active Directory. 

Después de la configuración de CMK, los datos ingeridos en las áreas de trabajo asociadas a su *clúster* recurso se cifran con la clave en Key Vault. Puede desasociar las áreas de trabajo del recurso de *clúster* en cualquier momento. Los nuevos datos se introducen en el almacenamiento de Log Analytics y se cifran con la clave de Microsoft, mientras que puede consultar los datos nuevos y antiguos sin problemas.


![Información general de CMK](media/customer-managed-keys/cmk-overview-8bit.png)

1. Key Vault
2. Recurso *Clúster* de Log Analytics que tiene una identidad administrada con permisos para Key Vault: la identidad se admite en el nivel del almacenamiento de clúster dedicado de Log Analytics
3. Clúster dedicado de Log Analytics
4. Áreas de trabajo del cliente asociadas al recurso *Clúster* para el cifrado de CMK

## <a name="encryption-keys-operation"></a>Operación de claves de cifrado

Hay tres tipos de claves que se usan en el cifrado de datos de Storage:

- **KEK**: clave de cifrado de claves (CMK)
- **AEK**: clave de cifrado de la cuenta
- **DEK**: clave de cifrado de datos

Se aplican las reglas siguientes:

- Las cuentas de almacenamiento de clúster de Log Analytics generan una clave de cifrado única para cada cuenta de almacenamiento, lo que se conoce como AEK.

- La AEK se usa para derivar DEK, que son las claves que se usa para cifrar cada bloque de datos escritos en el disco.

- Al configurar la clave en Key Vault y hacer referencia a ella en el recurso *Cluster*, Azure Storage envía solicitudes a Azure Key Vault para ajustar y desajustar AEK para realizar operaciones de cifrado y descifrado de datos.

- Esta KEK nunca abandona su instancia de Key Vault y, en el caso de una clave de HSM, nunca abandona el hardware.

- Azure Storage usa la identidad administrada que está asociada al recurso de *clúster* para autenticar el acceso y acceder a Azure Key Vault mediante Azure Active Directory.

## <a name="cmk-provisioning-procedure"></a>Procedimiento de aprovisionamiento de CMK

1. Suscripción permitida: la funcionalidad de CMK se proporciona en clústeres de Log Analytics dedicados. Para comprobar que tenemos la capacidad necesaria en la región, es necesario que la suscripción esté en la lista de permitidos de antemano. Use su contacto de Microsoft para obtener la suscripción de la lista de permitidos
2. Creación de una instancia de Azure Key Vault y almacenamiento de la clave
3. Creación de un recurso de *Cluster*
4. Concesión de permisos a la instancia de Key Vault
5. Asociación de áreas de trabajo de Log Analytics

Este procedimiento no se admite actualmente en la interfaz de usuario y el proceso de aprovisionamiento se realiza a través de la API REST.

> [!IMPORTANT]
> Cualquier solicitud de API debe incluir un token de autorización de portador en el encabezado.

Por ejemplo:

```rst
GET https://management.azure.com/subscriptions/<subscription-id>/resourcegroups/<resource-group-name>/providers/Microsoft.OperationalInsights/workspaces/<workspace-name>?api-version=2020-03-01-preview
Authorization: Bearer eyJ0eXAiO....
```

Donde *eyJ0eXAiO....* representa el token de autorización completo. 

Puede adquirir el token de las siguientes formas:

1. Use el método [Registros de aplicaciones](https://docs.microsoft.com/graph/auth/auth-concepts#access-tokens).
2. En el Portal de Azure
    1. Navegue hasta Azure Portal en la "herramienta para desarrolladores" (F12).
    1. Busque la cadena de autorización en "Encabezados de solicitud", en una de las instancias de "batch?api-version". Tiene el siguiente formato: "authorization: Bearer eyJ0eXAiO....". 
    1. Cópielo y agréguelo a su llamada API siguiendo los ejemplos que se muestran más abajo.
3. Vaya al sitio de documentación de REST de Azure. Presione "Probar" en cualquier API y copie el token de portador.

### <a name="asynchronous-operations-and-status-check"></a>Operaciones asincrónicas y comprobación de estado

Algunas de las operaciones de este procedimiento de configuración se ejecutan de forma asincrónica porque no se pueden completar rápidamente. La respuesta para la operación asincrónica devuelve inicialmente un código de estado HTTP 200 (Correcto) y un encabezado con la propiedad *Azure-AsyncOperation* cuando se acepta:
```json
"Azure-AsyncOperation": "https://management.azure.com/subscriptions/subscription-id/providers/Microsoft.OperationalInsights/locations/region-name/operationStatuses/operation-id?api-version=2020-03-01-preview"
```

Puede obtener el estado de la operación asincrónica si envía una solicitud GET al valor del encabezado *Azure-AsyncOperation*:
```rst
GET https://management.azure.com/subscriptions/subscription-id/providers/microsoft.operationalInsights/locations/region-name/operationstatuses/operation-id?api-version=2020-03-01-preview
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

La eliminación de recursos del *clúster* está en curso: cuando se elimina un recurso de *clúster* que tiene áreas de trabajo asociadas a áreas de trabajo, se realiza una operación de desasociación para cada una de las áreas de trabajo en operaciones asincrónicas que pueden tardar unos minutos.
Esto no es relevante cuando se elimina un *Clúster* sin ningún área de trabajo asociada; en este caso, el recurso de *Clúster*se elimina inmediatamente.
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

### <a name="subscription-whitelisting"></a>Inclusión de suscripciones en una lista blanca

La funcionalidad de CMK se proporciona en clústeres de Log Analytics dedicados. Para comprobar que tenemos la capacidad necesaria en su región, es necesario que la suscripción esté en la lista de permitidos de antemano. Use sus contactos de Microsoft para proporcionar los identificadores de las suscripciones.

> [!IMPORTANT]
> La función de CMK es regional. La instancia de Azure Key Vault, el recurso *Cluster* y las áreas de trabajo de Log Analytics asociadas deben estar en la misma región, pero pueden estar en distintas suscripciones.

### <a name="storing-encryption-key-kek"></a>Almacenamiento de la clave de cifrado de claves (KEK)

Cree un recurso de Azure Key Vault, o use uno que ya tenga, para generar o importar una clave que se usará para el cifrado de datos. Azure Key Vault se debe configurar como recuperable para proteger su clave y el acceso a los datos en Azure Monitor. Puede comprobar esta configuración en las propiedades de Key Vault: tanto la *Eliminación temporal* como la *Protección de purga* deben estar habilitadas.

![Configuración de la eliminación temporal y la protección de purga](media/customer-managed-keys/soft-purge-protection.png)

Esta configuración está disponible a través de la CLI y mediante PowerShell:
- [eliminación temporal](https://docs.microsoft.com/azure/key-vault/key-vault-ovw-soft-delete)
- La [Protección de purga](https://docs.microsoft.com/azure/key-vault/key-vault-ovw-soft-delete#purge-protection) protege contra la eliminación forzada del secreto o almacén incluso después de la eliminación temporal.

### <a name="create-cluster-resource"></a>Creación de un recurso de *clúster*

Este recurso se usa como conexión de identidad intermedia entre su instancia de Key Vault y sus áreas de trabajo de Log Analytics. Después de recibir la confirmación de que las suscripciones se han incluido en la lista blanca, cree un recurso de *clúster* de Log Analytics en la región donde se encuentran las áreas de trabajo.

Debe especificar el nivel (sku) de *capacidad de reserva* al crear un recurso de *Clúster*. El nivel de *capacidad de reserva* puede estar en el rango de 1000 a 2000 GB por día y se puede actualizar más adelante en etapas de 100. Si necesita un nivel de reserva de capacidad superior a 2000 GB por día, comuníquese con nosotros en LAIngestionRate@microsoft.com. [Más información](https://docs.microsoft.com/azure/azure-monitor/platform/manage-cost-storage#log-analytics-clusters)

La propiedad *billingType* determina la atribución de facturación para el recurso de *Clúster*y sus datos:
- *Clúster* (valor predeterminado): la facturación se atribuye a la suscripción que hospeda el recurso de *clúster*
- *Áreas de trabajo*: la facturación se atribuye a las suscripciones que hospedan las áreas de trabajo de forma proporcional

> [!NOTE]
> Después de crear el recurso de *clúster*, puede actualizarlo con *SKU*, *keyVaultProperties* o *billingType* mediante la solicitud de REST de PATCH.

**Creación**

Esta solicitud de Resource Manager es una operación asincrónica.

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

La identidad se asigna al recurso de *clúster* en el momento de su creación.

**Respuesta**

200 Correcto y encabezado.

Si bien el aprovisionamiento del clúster de Log Analytics subyacente tarda un poco en completarse, puede comprobar el estado de aprovisionamiento de dos maneras:

1. Copie el valor de la dirección URL de Azure-AsyncOperation de la respuesta y siga la [comprobación del estado de operaciones asincrónicas](#asynchronous-operations-and-status-check).
2. Envíe una solicitud GET en el recurso *Clúster* y examine el valor *provisioningState*. Es *ProvisioningAccount* mientras se realiza el aprovisionamiento y *Succeeded*, una vez que se completa.

```rst
GET https://management.azure.com/subscriptions/<subscription-id>/resourceGroups/<resource-group-name>/providers/Microsoft.OperationalInsights/clusters/<cluster-name>?api-version=2020-03-01-preview
Authorization: Bearer <token>
```

> [!IMPORTANT]
> Copie y guarde la respuesta, ya que necesitará los detalles en los pasos siguientes.

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
    "clusterType": "LogAnalytics",
    "billingType": "cluster",
    "clusterId": "cluster-id"
    },
  "id": "/subscriptions/subscription-id/resourceGroups/resource-group-name/providers/Microsoft.OperationalInsights/clusters/cluster-name",
  "name": "cluster-name",
  "type": "Microsoft.OperationalInsights/clusters",
  "location": "region-name"
}
```

El GUID "principalId" se genera desde el servicio de identidad administrada para el recurso *Cluster*.

### <a name="grant-key-vault-permissions"></a>Concesión de permisos a Key Vault

Actualice Key Vault con una nueva directiva de acceso que conceda permisos al recurso de *clúster*. Estos permisos los utiliza el almacenamiento de Azure Monitor subyacente para el cifrado de datos. Abra su instancia de Key Vault en Azure Portal y haga clic en "Directivas de acceso" y luego en "+ Agregar directiva de acceso" para crear una nueva directiva con esta configuración:

- Permisos de clave: seleccione los permisos "Obtener", "Encapsular clave" y "Desencapsular clave".
- Seleccionar entidad de seguridad: escriba el identificador de la entidad de seguridad, que se devolvió en la respuesta del paso anterior.

![Concesión de permisos a Key Vault](media/customer-managed-keys/grant-key-vault-permissions-8bit.png)

El permiso *Obtener* es necesario para comprobar que su instancia de Azure Key Vault está configurada como recuperable con el fin de proteger su clave y el acceso a sus datos de Azure Monitor.

### <a name="update-cluster-resource-with-key-identifier-details"></a>Actualización del recurso de clúster con detalles del identificador de clave

Este paso se realiza durante las actualizaciones iniciales y futuras de la versión de la clave en Key Vault. Informa al almacenamiento de Azure Monitor sobre la versión de la clave que se va a usar para el cifrado de los datos. Cuando se actualiza, la clave nueva se usa para ajustar y desajustar la clave de almacenamiento (AEK).

Para actualizar el recurso *Cluster* con los detalles del *identificador de clave* de Key Vault, seleccione la versión actual de la clave en Azure Key Vault para obtener detalles del identificador de clave.

![Concesión de permisos a Key Vault](media/customer-managed-keys/key-identifier-8bit.png)

Actualice el recurso de *clúster* "KeyVaultProperties" con los detalles del identificador de clave.

**Actualizar**

Esta solicitud de Resource Manager es una operación asincrónica cuando se actualizan los detalles del identificador de clave, mientras que es sincrónica al actualizar el valor de la capacidad.

> [!NOTE]
> Puede proporcionar un cuerpo parcial en recurso de *clúster* para actualizar un *SKU*, *keyVaultProperties* o *billingType*.

```rst
PATCH https://management.azure.com/subscriptions/<subscription-id>/resourceGroups/<resource-group-name>/providers/Microsoft.OperationalInsights/clusters/<cluster-name>?api-version=2020-03-01-preview
Authorization: Bearer <token>
Content-type: application/json

{
   "identity": { 
     "type": "systemAssigned" 
     },
   "sku": {
     "name": "capacityReservation",
     "capacity": 1000
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

**Respuesta**

200 Correcto y encabezado.
La propagación del identificador de clave tarda unos minutos en completarse. Puede comprobar el estado de actualización de dos maneras:
1. Copie el valor de la dirección URL de Azure-AsyncOperation de la respuesta y siga la [comprobación del estado de operaciones asincrónicas](#asynchronous-operations-and-status-check).
2. Envíe una solicitud GET en el recurso *Clúster* y examine las propiedades *KeyVaultProperties*. Los detalles del identificador de clave que actualizó recientemente se deben devolver en la respuesta.

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
    "clusterType": "LogAnalytics", 
    "billingType": "cluster",
    "clusterId": "cluster-id"
  },
  "id": "/subscriptions/subscription-id/resourceGroups/resource-group-name/providers/Microsoft.OperationalInsights/clusters/cluster-name",
  "name": "cluster-name",
  "type": "Microsoft.OperationalInsights/clusters",
  "location": "region-name"
}
```

### <a name="workspace-association-to-cluster-resource"></a>Asociación del área de trabajo con el recurso de *clúster*

Debe tener permisos de "escritura" en el área de trabajo y en el recurso *Cluster* para realizar esta operación, que incluye estas acciones:

- En el área de trabajo: Microsoft.OperationalInsights/workspaces/write
- En el recurso de *clúster*: Microsoft.OperationalInsights/clusters/write

> [!IMPORTANT]
> Este paso solo debe realizarse una vez completada la Log Analytics el aprovisionamiento del clúster. Si asocia áreas de trabajo e ingiere datos antes del aprovisionamiento, los datos ingeridos se quitarán y no se podrán recuperar.

**Asociación de un área de trabajo**

Esta solicitud de Resource Manager es una operación asincrónica.

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

**Respuesta**

200 Correcto y encabezado.

Los datos ingeridos se almacenan cifrados con la clave administrada después de la operación de asociación, que puede tardar hasta 90 minutos en completarse. Puede comprobar el estado de asociación del área de trabajo de dos maneras:

1. Copie el valor de la dirección URL de Azure-AsyncOperation de la respuesta y siga la [comprobación del estado de operaciones asincrónicas](#asynchronous-operations-and-status-check).
2. Envíe una solicitud [Áreas de trabajo: Get](https://docs.microsoft.com/rest/api/loganalytics/workspaces/get) y observe la respuesta, el área de trabajo asociada tendrá un elemento clusterResourceId en "características".

```rest
GET https://management.azure.com/subscriptions/<subscription-id>/resourcegroups/<resource-group-name>/providers/microsoft.operationalInsights/workspaces/<workspace-name>?api-version=2020-03-01-preview
Authorization: Bearer <token>
```

**Respuesta**

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

## <a name="cmk-kek-revocation"></a>Revocación de CMK (KEK)

Para revocar el acceso a los datos, deshabilite la clave o elimine la directiva de acceso del recurso *Clúster* en Key Vault. El almacenamiento de clúster de Log Analytics siempre respetará los cambios en los permisos de las claves en el plazo de una hora (normalmente antes), y Storage dejará de estar disponible. Los datos nuevos que se ingestan en áreas de trabajo asociadas al recurso del  *clúster*  se quitan y no se pueden recuperar, no se puede acceder a los datos y las consultas a estas áreas de trabajo generan un error. Los datos ingeridos anteriormente permanecerán en el almacenamiento siempre que se no se elimine el recurso *Clúster* ni las áreas de trabajo. Los datos inaccesibles se rigen por la directiva de retención de datos y se purgarán cuando se alcance la retención. 

Los datos ingeridos en los últimos 14 días también se conservan en la memoria caché activa (respaldada por SSD) para un funcionamiento eficaz del motor de consultas. Esto se elimina en la operación de revocación de claves y también se convierte en inaccesible.

Storage sondeará periódicamente su instancia de Key Vault para intentar desencapsular la clave de cifrado y, una vez haya obtenido el acceso, procederá con la ingesta de datos y la reanudación de la consulta en un plazo de 30 minutos.

## <a name="cmk-kek-rotation"></a>Rotación de CMK (KEK)

La rotación de CMK necesita una actualización explícita del recurso *Clúster* con la nueva versión de la clave en Azure Key Vault. Siga las instrucciones que aparecen en el paso "Actualización del recurso de *clúster* con detalles del identificador de clave". Si no actualiza los detalles del identificador de clave en el recurso de *clúster*, el almacenamiento de clúster de Log Analytics seguirá usando la clave anterior para cifrado. Si deshabilita o elimina la clave anterior antes de actualizar la nueva clave en el recurso de *clúster*, obtendrá el estado de [revocación de clave](#cmk-kek-revocation).

Se puede acceder a todos los datos después de la operación de rotación de claves, incluidos los datos ingeridos antes y después de la rotación, ya que todos los datos permanecen cifrados mediante la clave de cifrado de cuenta (AEK), mientras que la AEK ahora se cifra con la nueva versión de la clave de cifrado de claves (KEK).

## <a name="cmk-manage"></a>Administrar CMK

- **Obtenga todos los recursos *Cluster* para un grupo de recursos**

  ```rst
  GET https://management.azure.com/subscriptions/<subscription-id>/resourcegroups/<resource-group-name>/providers/Microsoft.OperationalInsights/clusters?api-version=2020-03-01-preview
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
          "clusterType": "LogAnalytics", 
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

- **Obtenga todos los recursos *Cluster* para una suscripción**

  ```rst
  GET https://management.azure.com/subscriptions/<subscription-id>/providers/Microsoft.OperationalInsights/clusters?api-version=2020-03-01-preview
  Authorization: Bearer <token>
  ```
    
  **Respuesta**
    
  Se obtiene la misma respuesta que para los recursos de *clúster* de un grupo de recursos, pero en el ámbito de la suscripción.

- **Actualización de la *reserva de capacidad* en el *recurso* de clúster**

  Cuando cambie el volumen de datos en las áreas de trabajo asociadas con el tiempo y desee actualizar el nivel de reserva de capacidad adecuadamente. Siga la [actualización *del recurso* de clúster](#update-cluster-resource-with-key-identifier-details) y proporcione el valor de la nueva capacidad. Puede estar en el rango de 1000 a 2000 GB por día y en pasos de 100. Para un nivel superior a 2000 GB por día, comuníquese con su contacto de Microsoft para habilitarlo. Tenga en cuenta que no tiene que proporcionar el cuerpo completo de la solicitud de REST y debe incluir la SKU:

  **body**
  ```json
  {
    "sku": {
      "name": "capacityReservation",
      "Capacity": 1000
    }
  }
  ```

- **Actualizar *billingType* en el *recurso* de clúster**

  La propiedad *billingType* determina la atribución de facturación para el recurso de *Clúster*y sus datos:
  - *Clúster* (valor predeterminado): la facturación se atribuye a la suscripción que hospeda los recursos de Clúster
  - *Áreas de trabajo*: la facturación se atribuye a las suscripciones que hospedan las áreas de trabajo de forma proporcional
  
  Siga las [actualización *del recurso* de clúster](#update-cluster-resource-with-key-identifier-details) y proporcione el valor de billingType. Tenga en cuenta que no tiene que proporcionar el cuerpo completo de la solicitud de REST y debe incluir el *billingType*:

  **body**
  ```json
  {
    "properties": {
      "billingType": "cluster",
      }  
  }
  ``` 

- **Desasociar área de trabajo**

  Necesita permisos de escritura en el área de trabajo y en el *Clúster* para realizar esta operación. Puede desasociar las áreas de trabajo del recurso de *clúster* en cualquier momento. Los nuevos datos ingeridos se almacenan en el almacenamiento compartido de Log Analytics y se cifran con la clave de Microsoft. Puede consultar los datos ingeridos en el área de trabajo antes y después de la desasociación sin problemas, si el recurso *Clúster* se aprovisiona y se configura con una clave de Key Vault válida.

  Esta solicitud de Resource Manager es una operación asincrónica.

  ```rest
  DELETE https://management.azure.com/subscriptions/<subscription-id>/resourcegroups/<resource-group-name>/providers/microsoft.operationalinsights/workspaces/<workspace-name>/linkedservices/cluster?api-version=2020-03-01-preview
  Authorization: Bearer <token>
  ```

  **Respuesta**

  200 Correcto y encabezado.

  Los datos ingeridos después de que la operación de desasociación se almacena en el almacenamiento de Log Analytics, esto puede tardar 90 minutos en completarse. Puede comprobar el estado de asociación del área de trabajo de dos maneras:

  1. Copie el valor de la dirección URL de Azure-AsyncOperation de la respuesta y siga la [comprobación del estado de operaciones asincrónicas](#asynchronous-operations-and-status-check).
  2. Envíe una solicitud [Áreas de trabajo: ](https://docs.microsoft.com/rest/api/loganalytics/workspaces/get)observe la respuesta, el área de trabajo asociada no tendrá un elemento *clusterResourceId* en *características*.

- **Elimine el *recurso* de clúster**

  Necesita permisos de escritura en el recurso *Clúster* para realizar esta operación. Se realiza una operación de eliminación temporal para permitir la recuperación del recurso *Cluster*, incluidos los datos de un período de catorce días, tanto si la eliminación fue accidental como si fue intencionada. El nombre del recurso *Cluster* permanece reservado durante el período de eliminación temporal y no se puede crear un nuevo clúster con ese nombre. Después del período de eliminación temporal, se libera el nombre del recurso *Clúster*. El recurso *Clúster* y los datos se eliminan de forma permanente y no se pueden recuperar. Cualquier área de trabajo asociada se desasocia del recurso *Clúster* en la operación de eliminación. Los nuevos datos ingeridos se almacenan en el almacenamiento compartido de Log Analytics y se cifran con la clave de Microsoft. La operación de desasociar áreas de trabajo es asincrónica y puede tardar hasta 90 minutos en completarse.

  ```rst
  DELETE https://management.azure.com/subscriptions/<subscription-id>/resourceGroups/<resource-group-name>/providers/Microsoft.OperationalInsights/clusters/<cluster-name>?api-version=2020-03-01-preview
  Authorization: Bearer <token>
  ```

  **Respuesta**

  200 OK

- **Recuperar el recurso *clúster* y los datos** 
  
  Un recurso *Clúster* que se eliminó durante los últimos 14 días se encuentra en estado de eliminación temporal y se puede recuperar. Dado que todas las áreas de trabajo se han desasociado del recurso de *Clúster*con eliminación de recursos de *Clúster*, debe volver a asociar las áreas de trabajo después de la recuperación para el cifrado de CMK. Actualmente, el grupo de productos lo realiza de forma manual. Use el canal de Microsoft para las solicitudes de recuperación.

## <a name="limitationsandconstraints"></a>Limitaciones y restricciones

-CMK es compatible con un clúster de Log Analytics dedicado y adecuado para los clientes que envían 1 TB al día o más.

-El número máximo de los recursos de  *clúster*  por región y suscripción es 2

-Puede asociar un área de trabajo al recurso de  *Clúster*  y, a continuación, desasociarla si CMK no es necesario para el área de trabajo. El número de asociaciones del área de trabajo en un área de trabajo determinada en un período de 30 días se limita a 2

La asociación de los recursos  *Clúster*  al área de trabajo SOLO se debe llevar a cabo después de comprobar que se completó el aprovisionamiento del clúster de Log Analytics. Los datos que se envíen al área de trabajo antes de que se complete el aprovisionamiento se eliminarán y no se podrán recuperar.

El cifrado de CMK se aplica a los datos ingeridos después de la     configuración de CMK. Los datos que se ingieren antes de la     configuración de CMK permanecen cifrados con la clave de Microsoft. Puede consultar fácilmente     los datos ingeridos antes y después de la configuración de CMK.

\- La instancia de Azure Key Vault debe configurarse como recuperable. Las siguientes propiedades no están habilitadas de forma predeterminada y deben configurarse mediante la CLI o PowerShell:   - [La eliminación temporal](https://docs.microsoft.com/azure/key-vault/key-vault-ovw-soft-delete)
    debe estar activada  - [. Protección de purga](https://docs.microsoft.com/azure/key-vault/key-vault-ovw-soft-delete#purge-protection) debe estar activada para protegerse frente a la eliminación forzada del secreto o almacén incluso después de la eliminación temporal.

Actualmente     no se admite el traslado de un recurso de - *clúster*  a otro grupo de recursos o a otra suscripción.

\- La instancia de Azure Key Vault, el recurso del  *clúster*  y las áreas de trabajo asociadas deben estar en la misma región y en el mismo inquilino de Azure Active Directory (Azure AD), pero pueden estar en distintas suscripciones.

\- Se producirá un error al asociar el área de trabajo a un recurso de  *clúster*  si está     asociada a otro *recurso de* clúster

## <a name="troubleshooting"></a>Solución de problemas

- Comportamiento con la disponibilidad de Key Vault
  - En el funcionamiento normal, Storage almacena en caché la AEK durante breves períodos de tiempo y vuelve regularmente a Key Vault para desajustarla.
    
  - En el caso de los errores de conexión transitorios (tiempos de espera, errores de conexión, problemas de DNS), Storage permite que las claves permanezcan en la memoria caché durante un período de tiempo más prolongado; así es más fácil resolver pequeños problemas de disponibilidad. Las funciones de consulta e ingesta continúan sin interrupción.
    
  - La falta de disponibilidad de un sitio activo durante unos 30 minutos hará que la cuenta de Storage deje de estar disponible. La función de consulta no está disponible y los datos ingeridos se copian en caché durante varias horas mediante la clave de Microsoft para evitar la pérdida de datos. Cuando se restaura el acceso a Key Vault, la consulta está disponible y los datos copiados en caché temporalmente se ingieren en el almacén de datos y se cifran con CMK.

  - Frecuencia de acceso a Key Vault: la frecuencia con la que Azure Monitor Storage accede a Key Vault para las operaciones de encapsulado y desencapsulado es de entre 6 y 60 segundos.

- Si crea un recurso de *clúster* y especifica el valor de KeyVaultProperties inmediatamente, puede producirse un error en la operación, ya que la directiva de acceso no se puede definir hasta que se asigne la identidad del sistema al recurso de *clúster*.

- Si actualiza el recurso existente de *clúster* con KeyVaultProperties y la directiva de acceso de clave "Obtener" no se encuentra en Key Vault, se producirá un error en la operación.

- Si intenta eliminar un recurso de *clúster* que está asociado a un área de trabajo, se producirá un error en la operación de eliminación.

- Si se produce un error de conflicto al crear un recurso *Clúster*, es posible que haya eliminado el recurso *Clúster* en los últimos 14 días y que esté en un período de eliminación temporal. El nombre del recurso *Cluster* permanece reservado durante el período de eliminación temporal y no se puede crear un nuevo clúster con ese nombre. El nombre se libera después del período de eliminación temporal cuando el recurso *Clúster* se elimine de forma permanente.

- Si actualiza su recurso *Cluster* mientras una operación está en curso, se producirá un error en dicha operación.

- Si no puede implementar el recurso de *clúster*, compruebe que el recurso de  *clúster*  de Azure Key Vault y las áreas de trabajo de Log Analytics asociadas se encuentran en la misma región. Puede estar en distintas suscripciones.

- Si actualiza la versión de la clave en Key Vault y no actualiza los nuevos detalles del identificador de clave en el recurso *Clúster*, el clúster de Log Analytics seguirá usando la clave anterior y los datos serán inaccesibles. Actualice los nuevos detalles de identificador de clave del recurso de *Clúster* para reanudar la ingesta de datos y la capacidad de consultar los datos.

- Para obtener soporte técnico y ayuda relacionada con la clave administrada por el cliente, use sus contactos en Microsoft.
