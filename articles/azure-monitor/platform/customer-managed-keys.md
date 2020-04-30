---
title: Configuración de la clave administrada por el cliente de Azure Monitor
description: Información y pasos para configurar la clave administrada por el cliente (CMK) para cifrar los datos en las áreas de trabajo de Log Analytics mediante una clave de Azure Key Vault.
ms.subservice: logs
ms.topic: conceptual
author: yossi-y
ms.author: yossiy
ms.date: 04/12/2020
ms.openlocfilehash: 25fdb0aefacbdd9c2630a69981a67821ac155786
ms.sourcegitcommit: 31e9f369e5ff4dd4dda6cf05edf71046b33164d3
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 04/22/2020
ms.locfileid: "81758811"
---
# <a name="azure-monitor-customer-managed-key-configuration"></a>Configuración de la clave administrada por el cliente de Azure Monitor 

En este artículo se proporcionan información general y los pasos para configurar las claves administradas por el cliente (CMK) para las áreas de trabajo de Log Analytics y los componentes de Application Insights. Una vez configuradas, los datos enviados a sus áreas de trabajo o a sus componentes se cifran con su clave de Azure Key Vault.

Se recomienda revisar la sección [Limitaciones y restricciones](#limitations-and-constraints) que aparece más abajo antes de proceder con la configuración.

## <a name="disclaimers"></a>Declinación de responsabilidades

- La CMK de Azure Monitor es una característica de acceso anticipado que está habilitada para las suscripciones registradas.

- La implementación de CMK que se describe en este artículo se proporciona en calidad de producción y se admite como tal, aunque es una característica de acceso anticipado.

- La función de CMK se proporciona en un clúster de almacenamiento de datos dedicado, que es un clúster de Azure Data Explorer (ADX) adecuado para los clientes que envían 1 TB al día o más. 

- El modelo de precios de CMK no está disponible actualmente y no se trata en este artículo. Está previsto que se ofrezca un modelo de precios para un clúster de ADX dedicado en el segundo trimestre del año 2020, el cual se aplicará a cualquier implementación de CMK existente.

- En este artículo se describe la configuración de CMK para áreas de trabajo de Log Analytics. También se aplica este artículo a la configuración de CMK para los componentes de Application Insights; las diferencias se enumeran en el apéndice.

> [!NOTE]
> Log Analytics y Application Insights usan la misma plataforma de almacenamiento de datos y el mismo motor de consultas.
> Vamos a unir estos dos almacenes a través de la integración de Application Insights en Log Analytics para crear un único almacén de registros unificados en Azure Monitor. Este cambio está previsto para el segundo trimestre del año 2020. Si no necesita implementar claves CMK para los datos de Application Insights hasta entonces, le recomendamos que espere a que se complete la consolidación, ya que la consolidación interrumpirá dichas implementaciones y tendrá que volver a configurar la CMK después de la migración al área de trabajo de Log Analytics. El mínimo de 1 TB por día se aplica en el nivel del clúster y, hasta que se complete la consolidación durante el segundo trimestre, Application Insights y Log Analytics necesitan clústeres independientes.

## <a name="customer-managed-key-cmk-overview"></a>Información general de la clave administrada por el cliente (CMK)

[El cifrado en reposo](https://docs.microsoft.com/azure/security/fundamentals/encryption-atrest) es un requisito común de privacidad y seguridad en las organizaciones. Puede dejar que Azure administre completamente el cifrado en reposo, mientras dispone de varias opciones para administrar minuciosamente el cifrado o las claves de cifrado.

El almacén de datos de Azure Monitor garantiza que todos los datos se cifran en reposo mediante claves administradas por Azure mientras están almacenados en Azure Storage. Azure Monitor también proporciona una opción para el cifrado de datos mediante su propia clave almacenada en [Azure Key Vault](https://docs.microsoft.com/azure/key-vault/key-vault-overview), al que se tiene acceso mediante la autenticación de la [identidad administrada](https://docs.microsoft.com/azure/active-directory/managed-identities-azure-resources/overview) asignada por el sistema. Esta clave se puede [proteger mediante software o con módulos de seguridad de hardware (HSM)](https://docs.microsoft.com/azure/key-vault/key-vault-overview).
El uso que hace Azure Monitor del cifrado es idéntico al del [cifrado de Azure Storage](https://docs.microsoft.com/azure/storage/common/storage-service-encryption#about-azure-storage-encryption).

La frecuencia con la que Azure Monitor Storage accede a Key Vault para las operaciones de encapsulado y desencapsulado es de entre 6 y 60 segundos. Azure Monitor Storage siempre respeta los cambios en los permisos de las claves en el plazo de una hora.

Los datos ingeridos en los últimos 14 días también se conservan en la memoria caché activa (respaldada por SSD) para un funcionamiento eficaz del motor de consultas. Estos datos permanecen cifrados con las claves de Microsoft independientemente de la configuración de CMK, pero estamos trabajando para que la SSD se cifre con CMK durante el primer semestre de 2020.

## <a name="how-cmk-works-in-azure-monitor"></a>Cómo funciona CMK en Azure Monitor

Azure Monitor aprovecha la identidad administrada asignada por el sistema para conceder acceso a su instancia de Azure Key Vault. La identidad administrada asignada por el sistema solo se puede asociar con un recurso de Azure. La identidad del almacén de datos de Azure Monitor (clúster de ADX) se admite en el nivel del clúster y esto indica que la función de CMK se proporciona en un clúster de ADX dedicado. Para admitir CMK en varias áreas de trabajo, un nuevo recurso de Log Analytics (*clúster*) debe funcionar como una conexión de identidad intermedia entre su instancia de Key Vault y sus áreas de trabajo de Log Analytics. Este concepto se ajusta a la restricción de identidad asignada por el sistema y la identidad se mantiene entre el clúster de ADX y el recurso de *clúster* de Log Analytics; de este modo, los datos de todas las áreas de trabajo asociadas están protegidos con su clave de Key Vault. El almacenamiento de clúster subyacente de ADX usa la identidad administrada que está asociada al recurso de *clúster* para autenticar el acceso y acceder a Azure Key Vault mediante Azure Active Directory.

![Información general de CMK](media/customer-managed-keys/cmk-overview-8bit.png)
1.    Key Vault del cliente.
2.    Recurso *Clúster* de Log Analytics del cliente que tiene una identidad administrada con permisos para Key Vault: la identidad se admite en el nivel del almacén de datos (clúster de ADX).
3.    Clúster de ADX dedicado para Azure Monitor.
4.    Áreas de trabajo del cliente asociadas al recurso *Clúster* para el cifrado de CMK.

## <a name="encryption-keys-management"></a>Administración de las claves de cifrado

Hay tres tipos de claves que se usan en el cifrado de datos de Storage:

- **KEK**: clave de cifrado de claves (CMK)
- **AEK**: clave de cifrado de la cuenta
- **DEK**: clave de cifrado de datos

Se aplican las reglas siguientes:

- Las cuentas de almacenamiento de ADX generan una clave de cifrado única para cada cuenta de almacenamiento, lo que se conoce como AEK.

- La AEK se usa para derivar DEK, que son las claves que se usa para cifrar cada bloque de datos escritos en el disco.

- Al configurar la clave en Key Vault y hacer referencia a ella en el recurso *Cluster*, Azure Storage envía solicitudes a Azure Key Vault para ajustar y desajustar AEK para realizar operaciones de cifrado y descifrado de datos.

- Esta KEK nunca abandona su instancia de Key Vault y, en el caso de una clave de HSM, nunca abandona el hardware.

- Azure Storage usa la identidad administrada que está asociada al recurso de *clúster* para autenticar el acceso y acceder a Azure Key Vault mediante Azure Active Directory.

## <a name="cmk-provisioning-procedure"></a>Procedimiento de aprovisionamiento de CMK

Para la configuración de CMK en Application Insights, en lugar de los pasos 3 y 6, siga el contenido del apéndice.

1. Elaboración de una lista blanca de suscripciones (es necesario para esta característica de acceso anticipado)
2. Creación de una instancia de Azure Key Vault y almacenamiento de la clave
3. Creación de un recurso de *Cluster*
4. Aprovisionamiento del almacén de datos de Azure Monitor (clúster de ADX)
5. Concesión de permisos a la instancia de Key Vault
6. Asociación de áreas de trabajo de Log Analytics

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

La función de CMK es una característica de acceso anticipado. Las suscripciones en las que planea crear recursos de *clúster* deben haber sido incluidas de antemano en una lista blanca por el equipo de productos de Azure. Use sus contactos de Microsoft para proporcionar los identificadores de las suscripciones.

> [!IMPORTANT]
> La función de CMK es regional. La instancia de Azure Key Vault, el recurso *Cluster* y las áreas de trabajo de Log Analytics asociadas deben estar en la misma región, pero pueden estar en distintas suscripciones.

### <a name="storing-encryption-key-kek"></a>Almacenamiento de la clave de cifrado de claves (KEK)

Cree un recurso de Azure Key Vault, o use uno que ya tenga, para generar o importar una clave que se usará para el cifrado de datos. Azure Key Vault se debe configurar como recuperable para proteger su clave y el acceso a los datos en Azure Monitor. Puede comprobar esta configuración en las propiedades de Key Vault: tanto la *Eliminación temporal* como la *Protección de purga* deben estar habilitadas.

![Configuración de la eliminación temporal y la protección de purga](media/customer-managed-keys/soft-purge-protection.png)

Esta configuración está disponible a través de la CLI y mediante PowerShell:
- [eliminación temporal](https://docs.microsoft.com/azure/key-vault/key-vault-ovw-soft-delete)
- La [Protección de purga](https://docs.microsoft.com/azure/key-vault/key-vault-ovw-soft-delete#purge-protection) protege contra la eliminación forzada del secreto o almacén incluso después de la eliminación temporal.

### <a name="create-cluster-resource"></a>Creación de un recurso de *clúster*

Este recurso se usa como conexión de identidad intermedia entre su instancia de Key Vault y sus áreas de trabajo de Log Analytics. Después de recibir la confirmación de que las suscripciones se han incluido en la lista blanca, cree un recurso de *clúster* de Log Analytics en la región donde se encuentran las áreas de trabajo. Application Insights y Log Analytics necesitan tipos de recursos *Cluster* independientes. El tipo del recurso de *clúster* se define en el momento de su creación estableciendo la propiedad *clusterType* en *LogAnalytics* o *ApplicationInsights*. El tipo de recurso de clúster no se puede modificar después.

Para la configuración de CMK en Application Insights, siga el contenido del apéndice.

Debe especificar el nivel (sku) de reserva de capacidad al crear un recurso *Clúster*. El nivel de reserva de capacidad puede estar en el intervalo de 1000 a 2000 GB por día y se puede actualizar más adelante en intervalos de 100. Si necesita un nivel de reserva de capacidad superior a 2000 GB por día, comuníquese con su contacto de Microsoft para habilitarlo. Esta propiedad no afecta actualmente a la facturación: una vez que se introduce el modelo de precios para un clúster dedicado, la facturación se aplicará a las implementaciones de CMK existentes.

**Creación**

Esta solicitud de Resource Manager es una operación asincrónica.

```rst
PUT https://management.azure.com/subscriptions/<subscription-id>/resourceGroups/<resource-group-name>/providers/Microsoft.OperationalInsights/clusters/<cluster-name>?api-version=2019-08-01-preview
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
    "clusterType": "LogAnalytics",
    },
  "location": "<region-name>",
}
```
La identidad se asigna al recurso de *clúster* en el momento de su creación.

**Respuesta**

200 Correcto y encabezado.
Durante el período de acceso temprano de la característica, el clúster de ADX se aprovisiona de forma manual. Si bien el aprovisionamiento del clúster de ADX subyacente tarda un poco en completarse, puede comprobar el estado de aprovisionamiento de dos maneras:
1. Copie el valor de la dirección URL de Azure-AsyncOperation de la respuesta y siga la [comprobación del estado de operaciones asincrónicas](#asynchronous-operations-and-status-check).
2. Envíe una solicitud GET en el recurso *Clúster* y examine el valor *provisioningState*. Es *ProvisioningAccount* mientras se realiza el aprovisionamiento y *Succeeded*, una vez que se completa.

### <a name="azure-monitor-data-store-adx-cluster-provisioning"></a>Aprovisionamiento del almacén de datos de Azure Monitor (clúster de ADX)

Durante el período de acceso anticipado de la característica, el equipo de productos aprovisiona el clúster de ADX manualmente una vez se han completado los pasos anteriores. Use el canal de Microsoft para este paso y proporcione la respuesta del recurso *Cluster*. 

```rst
GET https://management.azure.com/subscriptions/<subscription-id>/resourceGroups/<resource-group-name>/providers/Microsoft.OperationalInsights/clusters/<cluster-name>?api-version=2019-08-01-preview
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

> [!Warning]
> Debe proporcionar un cuerpo completo en la actualización del recurso *Cluster* que incluya *identity*, *sku*, *KeyVaultProperties* y *location*. Si faltan los detalles de *KeyVaultProperties*, se quitará el identificador de clave del recurso *Cluster* y se [revocará la clave](#cmk-kek-revocation).

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
     "capacity": 1000
     },
   "properties": {
     "KeyVaultProperties": {
       KeyVaultUri: "https://<key-vault-name>.vault.azure.net",
       KeyName: "<key-name>",
       KeyVersion: "<current-version>"
       },
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
    "KeyVaultProperties": {
      KeyVaultUri: "https://key-vault-name.vault.azure.net",
      KeyName: "key-name",
      KeyVersion: "current-version"
      },
    "provisioningState": "Succeeded",
    "clusterType": "LogAnalytics", 
    "clusterId": "cluster-id"
  },
  "id": "/subscriptions/subscription-id/resourceGroups/resource-group-name/providers/Microsoft.OperationalInsights/clusters/cluster-name",
  "name": "cluster-name",
  "type": "Microsoft.OperationalInsights/clusters",
  "location": "region-name"
}
```

### <a name="workspace-association-to-cluster-resource"></a>Asociación del área de trabajo con el recurso de *clúster*
Para la configuración de CMK en Application Insights, en lugar de realizar este paso, siga el contenido del apéndice.

Debe tener permisos de "escritura" en el área de trabajo y en el recurso *Cluster* para realizar esta operación, que incluye estas acciones:

- En el área de trabajo: Microsoft.OperationalInsights/workspaces/write
- En el recurso de *clúster*: Microsoft.OperationalInsights/clusters/write

> [!IMPORTANT]
> Este paso solo debe realizarse después del aprovisionamiento de clústeres de ADX. Si asocia áreas de trabajo e ingiere datos antes del aprovisionamiento, los datos ingeridos se quitarán y no se podrán recuperar.

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
    "retentionInDays": days,
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

Para revocar el acceso a los datos, deshabilite la clave o elimine la directiva de acceso del recurso *Cluster* en Key Vault. Azure Monitor Storage siempre respetará los cambios en los permisos de las claves en el plazo de una hora (normalmente antes), y Storage dejará de estar disponible. Se quitarán los datos ingeridos en las áreas de trabajo asociadas a su recurso de *clúster* y se producirá un error en las consultas. Los datos ingeridos anteriormente permanecerán inaccesibles en Azure Monitor Storage siempre que se no se elimine el recurso *Clúster* ni las áreas de trabajo. Los datos inaccesibles se rigen por la directiva de retención de datos y se purgarán cuando se alcance la retención.

Storage sondeará periódicamente su instancia de Key Vault para intentar desencapsular la clave de cifrado y, una vez haya obtenido el acceso, procederá con la ingesta de datos y la reanudación de la consulta en un plazo de 30 minutos.

## <a name="cmk-kek-rotation"></a>Rotación de CMK (KEK)

La rotación de CMK necesita una actualización explícita del recurso *Cluster* con la nueva versión de la clave en Azure Key Vault. Para actualizar Azure Monitor con la nueva versión de la clave, siga las instrucciones que aparecen en el paso "Actualización del recurso de *clúster* con detalles del identificador de clave". Si actualiza la versión de la clave en Key Vault y no actualiza los nuevos detalles del identificador de clave en el recurso *Cluster*, Azure Monitor Storage seguirá usando la clave anterior.
Se puede acceder a todos los datos después de la operación de rotación de claves, incluidos los datos ingeridos antes y después de la rotación, ya que todos los datos permanecen cifrados mediante la clave de cifrado de cuenta (AEK), mientras que la AEK ahora se cifra con la nueva versión de la clave de cifrado de claves (KEK).

## <a name="limitations-and-constraints"></a>Limitaciones y restricciones

- La característica CMK se admite en el nivel de clúster de ADX y requiere un clúster de ADX de Azure Monitor dedicado con el requisito de enviar 1 TB al día o más.

- El número máximo de recursos *Cluster* por suscripción está limitado a dos.

- La asociación de los recursos *Clúster* al área de trabajo SOLO se debe llevar a cabo después de comprobar que se completó el aprovisionamiento del clúster de ADX. Los datos que se envíen al área de trabajo antes de que se complete el aprovisionamiento se eliminarán y no se podrán recuperar.

- El cifrado de CMK se aplica a los datos ingeridos después de la configuración de CMK. Los datos que se ingieren antes de la configuración de CMK permanecen cifrados con la clave de Microsoft. Puede consultar fácilmente los datos ingeridos antes y después de la configuración de CMK.

- Puede desasociar un área de trabajo de un recurso *Clúster* al decidir que CMK no es necesario para un área de trabajo determinada. Los nuevos datos ingeridos después de la operación de desasociación se almacenan en el almacenamiento compartido de Log Analytics, tal como se encontraban antes de asociarse con el recurso *Clúster*. Puede consultar los datos ingeridos antes y después de la desasociación sin problemas, si el recurso *Clúster* se aprovisiona y se configura con una clave de Key Vault válida.

- La instancia de Azure Key Vault debe configurarse como recuperable. Las siguientes propiedades no están habilitadas de forma predeterminada y deben configurarse mediante la CLI o PowerShell:

  - La [eliminación temporal](https://docs.microsoft.com/azure/key-vault/key-vault-ovw-soft-delete) debe estar activada.
  - La [protección de purgas](https://docs.microsoft.com/azure/key-vault/key-vault-ovw-soft-delete#purge-protection) debe estar activada si quiere tener protección frente a posibles eliminaciones forzadas de secretos o del almacén, incluso después de su eliminación temporal.

- Application Insights y Log Analytics necesitan recursos de *clúster* independientes. El tipo del recurso *Clúster* se define en el momento de su creación al establecer la propiedad "clusterType" en "LogAnalytics" o "ApplicationInsights". No se puede modificar el tipo de recurso *Clúster*.

- Actualmente no se admite el traslado de un recurso de *clúster* a otro grupo de recursos o a otra suscripción.

- La instancia de Azure Key Vault, el recurso del *clúster* y las áreas de trabajo asociadas deben estar en la misma región y en el mismo inquilino de Azure Active Directory (Azure AD), pero pueden estar en distintas suscripciones.

- Se producirá un error al asociar el área de trabajo a un recurso de *clúster* si está asociada a otro recurso de *clúster*.

## <a name="troubleshooting-and-management"></a>Solución de problemas y administración de errores

- Disponibilidad de Key Vault
    - En el funcionamiento normal, Storage almacena en caché la AEK durante breves períodos de tiempo y vuelve regularmente a Key Vault para desajustarla.
    
    - En el caso de los errores de conexión transitorios (tiempos de espera, errores de conexión, problemas de DNS), Storage permite que las claves permanezcan en la memoria caché durante un período de tiempo más prolongado; así es más fácil resolver pequeños problemas de disponibilidad. Las funciones de consulta e ingesta continúan sin interrupción.
    
    - La falta de disponibilidad de un sitio activo durante unos 30 minutos hará que la cuenta de Storage deje de estar disponible. La función de consulta no está disponible y los datos ingeridos se copian en caché durante varias horas mediante la clave de Microsoft para evitar la pérdida de datos. Cuando se restaura el acceso a Key Vault, la consulta está disponible y los datos copiados en caché temporalmente se ingieren en el almacén de datos y se cifran con CMK.

- Si crea un recurso de *clúster* y especifica el valor de KeyVaultProperties inmediatamente, puede producirse un error en la operación, ya que la directiva de acceso no se puede definir hasta que se asigne la identidad del sistema al recurso de *clúster*.

- Si actualiza el recurso existente de *clúster* con KeyVaultProperties y la directiva de acceso de clave "Obtener" no se encuentra en Key Vault, se producirá un error en la operación.

- Si intenta eliminar un recurso de *clúster* que está asociado a un área de trabajo, se producirá un error en la operación de eliminación.

- Si se produce un error de conflicto al crear un recurso *Clúster*, es posible que haya eliminado el recurso *Clúster* en los últimos 14 días y que esté en un período de eliminación temporal. El nombre del recurso *Cluster* permanece reservado durante el período de eliminación temporal y no se puede crear un nuevo clúster con ese nombre. El nombre se libera después del período de eliminación temporal cuando el recurso *Clúster* se elimine de forma permanente.

- Obtenga todos los recursos *Cluster* para un grupo de recursos:

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
           "KeyVaultProperties": {
              KeyVaultUri: "https://key-vault-name.vault.azure.net",
              KeyName: "key-name",
              KeyVersion: "current-version"
              },
          "provisioningState": "Succeeded",
          "clusterType": "LogAnalytics", 
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

- Obtenga todos los recursos *Cluster* para una suscripción:

  ```rst
  GET https://management.azure.com/subscriptions/<subscription-id>/providers/Microsoft.OperationalInsights/clusters?api-version=2020-03-01-preview
  Authorization: Bearer <token>
  ```
    
  **Respuesta**
    
  Se obtiene la misma respuesta que para los recursos de *clúster* de un grupo de recursos, pero en el ámbito de la suscripción.

- Actualice la *reserva de capacidad* en el recurso *Clúster*: cuando cambie el volumen de datos para las áreas de trabajo asociadas y desee actualizar el nivel de reserva de capacidad para las consideraciones de facturación, siga la [actualización del recurso *Clúster*](#update-cluster-resource-with-key-identifier-details) y proporcione el nuevo valor de capacidad. El nivel de reserva de capacidad puede estar en el intervalo de 1000 a 2000 GB por día y en pasos de 100. Para un nivel superior a 2000 GB por día, comuníquese con su contacto de Microsoft para habilitarlo.

- Elimine el recurso *Clúster*: se realiza una operación de eliminación temporal para permitir la recuperación del recurso *Clúster*, incluidos los datos de un período de 14 días, tanto si la eliminación fue accidental como si fue intencionada. El nombre del recurso *Cluster* permanece reservado durante el período de eliminación temporal y no se puede crear un nuevo clúster con ese nombre. Después del período de eliminación temporal, se libera el nombre del recurso *Clúster*. El recurso *Clúster* y los datos se eliminan de forma permanente y no se pueden recuperar. Cualquier área de trabajo asociada se desasocia del recurso *Clúster* en la operación de eliminación. Los nuevos datos ingeridos se almacenan en el almacenamiento compartido de Log Analytics y se cifran con la clave de Microsoft. La operación de desasociar áreas de trabajo es asincrónica.

  ```rst
  DELETE https://management.azure.com/subscriptions/<subscription-id>/resourceGroups/<resource-group-name>/providers/Microsoft.OperationalInsights/clusters/<cluster-name>?api-version=2020-03-01-preview
  Authorization: Bearer <token>
  ```

  **Respuesta**

  200 OK

- Recupere el recurso *Clúster* y los datos: un recurso *Clúster* que se eliminó durante los últimos 14 días se encuentra en estado de eliminación temporal y se puede recuperar. Actualmente, el grupo de productos lo realiza de forma manual. Use el canal de Microsoft para las solicitudes de recuperación.

## <a name="appendix"></a>Apéndice

También se admiten claves administradas por el cliente (CMK) en Application Insights, aunque debe tener en cuenta el siguiente cambio para planear la implementación de CMK para los componentes de Application Insights.

Log Analytics y Application Insights usan la misma plataforma de almacenamiento de datos y el mismo motor de consultas. Vamos a unir estos dos almacenes a través de la integración de Application Insights en Log Analytics para ofrecer un único almacén de registros unificados en Azure Monitor durante el segundo trimestre del 2020.
2020. Con este cambio se introducirán varias mejoras, como la posibilidad de realizar consultas y obtener información; además, se incluirán los datos de Application Insights en las áreas de trabajo de Log Analytics, por lo que la configuración de CMK en su área de trabajo también se aplicará a los datos de Application Insights.

> [!NOTE]
> Si no necesita implementar claves CMK para los datos de Application Insights antes de la integración, le recomendamos que siga usando la CMK de Application Insights, ya que esta integración interrumpirá las implementaciones y tendrá que volver a configurar la CMK después de la migración al área de trabajo de Log Analytics. El mínimo de 1 TB por día se aplica en el nivel del clúster y, hasta que se complete la consolidación durante el segundo trimestre, Application Insights y Log Analytics necesitan clústeres independientes.

## <a name="application-insights-cmk-configuration"></a>Configuración de CMK en Application Insights

La configuración de CMK en Application Insights es idéntica al proceso que se muestra en este artículo, incluidas las restricciones y la solución de problemas, excepto por estos pasos:

- Creación de un recurso de *clúster*
- Asociación de un componente a un recurso de *clúster*

A la hora de configurar claves CMK para Application Insights, siga estos pasos en lugar de los enumerados anteriormente en el artículo.

### <a name="create-a-cluster-resource"></a>Creación de un recurso de *clúster*

Este recurso se usa como conexión de identidad intermedia entre su instancia de Key Vault y sus componentes. Después de recibir la confirmación de que las suscripciones se han incluido en la lista blanca, cree un recurso de *clúster* de Log Analytics en la región donde se encuentran sus componentes. El tipo del recurso de *clúster* se define en el momento de su creación estableciendo la propiedad *clusterType* en *LogAnalytics* o *ApplicationInsights*. Debe ser *ApplicationInsights* para la CMK de Application Insights. No se puede modificar el valor de *clusterType* después de la configuración.

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
    "clusterType":"ApplicationInsights"
    },
  "location": "<region-name>"
}
```

**Respuesta**

200 Correcto y encabezado.
Durante el período de acceso temprano de la característica, el clúster de ADX se aprovisiona de forma manual. Si bien el aprovisionamiento del clúster de ADX subyacente tarda un poco en completarse, puede comprobar el estado de aprovisionamiento de dos maneras:
1. Copie el valor de la dirección URL de Azure-AsyncOperation de la respuesta y siga la [comprobación del estado de operaciones asincrónicas](#asynchronous-operations-and-status-check).
2. Envíe una solicitud GET en el recurso *Clúster* y examine el valor *provisioningState*. Es *ProvisioningAccount* mientras se realiza el aprovisionamiento y *Succeeded*, una vez que se completa.

### <a name="associate-a-component-to-a-cluster-resource-using-components---create-or-update-api"></a>Asociación de un componente a un recurso de *clúster* mediante la API [Componentes: crear o actualizar](https://docs.microsoft.com/rest/api/application-insights/components/createorupdate)

Es necesario tener permisos de "escritura" en el componente y en el recurso *Cluster* para realizar esta operación, que incluye estas acciones:

- En el componente: Microsoft.Insights/component/write
- En el recurso de *clúster*: Microsoft.OperationalInsights/clusters/write

> [!IMPORTANT]
> Este paso solo debe realizarse después del aprovisionamiento de clústeres de ADX. Si asocia componentes e ingiere datos antes del aprovisionamiento, los datos ingeridos se quitarán y no se podrán recuperar.
> Para comprobar que el clúster de ADX se aprovisiona, ejecute la API REST Get del recurso *Cluster* y compruebe que el valor de *provisioningState* sea *Succeeded*.

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
    "KeyVaultProperties": {
      KeyVaultUri: "https://key-vault-name.vault.azure.net",
      KeyName: "key-name",
      KeyVersion: "current-version"
      },
    "provisioningState": "Succeeded",
    "clusterType": "ApplicationInsights", 
    "clusterId": "cluster-id"
    },
  "id": "/subscriptions/subscription-id/resourceGroups/resource-group-name/providers/Microsoft.OperationalInsights/clusters/cluster-name",
  "name": "cluster-name",
  "type": "Microsoft.OperationalInsights/clusters",
  "location": "region-name"
  }
```

> [!IMPORTANT]
> Copie y guarde la respuesta, ya que la necesitará en los pasos siguientes.

**Asociación de un componente**

```rst
PUT https://management.azure.com/subscriptions/<subscription-id>/resourceGroups/<resource-group-name>/providers/Microsoft.Insights/components/<component-name>?api-version=2015-05-01
Authorization: Bearer <token>
Content-type: application/json

{
  "properties": {
    "clusterDefinitionId": "cluster-id"
  },
  "location": "<region-name>",
  "kind": "<component-type>"
}
```
"clusterDefinitionId" es el valor "clusterId" proporcionado en la respuesta del paso anterior.
El ejemplo "kind" es "web".

**Respuesta**

```json
{
  "id": "/subscriptions/subscription-id/resourcegroups/resource-group-name/providers/microsoft.insights/components/component-name",
  "name": "component-name",
  "type": "Microsoft.Insights/components",
  "location": "region-name",
  "tags": "",
  "kind": "",
  "properties": {
    "clusterDefinitionId": "cluster-id"
    "ApplicationId": "",
    "AppId": "",
    "Application_Type": "",
    "Flow_Type": "",
    "Request_Source": "",
    "InstrumentationKey": "",
    "CreationDate": "",
    "TenantId": "",
    "HockeyAppId": "",
    "HockeyAppToken": "",
    "provisioningState": "",
    "SamplingPercentage":,
    "RetentionInDays":,
    "ConnectionString": "",
    "DisableIpMasking":,
    "ImmediatePurgeDataOn30Days": 
  }
}
```
"clusterDefinitionId" es el identificador del recurso de *clúster* asociado a este componente.

Después de realizar la asociación, los datos que se envían a sus componentes se almacenan cifrados con su clave administrada.
