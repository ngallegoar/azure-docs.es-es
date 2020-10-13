---
title: Azure Key Vault como origen de Event Grid
description: Describe las propiedades y el esquema que se proporcionan para los eventos de Azure Key Vault con Azure Event Grid
ms.topic: conceptual
ms.date: 07/07/2020
ms.openlocfilehash: b3d961228429c1f84e45c1df9147fa1687ab5074
ms.sourcegitcommit: 32c521a2ef396d121e71ba682e098092ac673b30
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 09/25/2020
ms.locfileid: "91324083"
---
# <a name="azure-key-vault-as-event-grid-source"></a>Azure Key Vault como origen de Event Grid

En este artículo se proporcionan las propiedades y el esquema de los eventos en [Azure Key Vault](../key-vault/index.yml). Para una introducción a los esquemas de eventos, consulte [Esquema de eventos de Azure Event Grid](event-schema.md).

## <a name="event-grid-event-schema"></a>Esquema de eventos de Event Grid

### <a name="available-event-types"></a>Tipos de eventos disponibles

Una cuenta Azure Key Vault genera los siguientes tipos de eventos:

| Nombre completo del evento | Nombre para mostrar del evento | Descripción |
| ---------- | ----------- |---|
| Microsoft.KeyVault.CertificateNewVersionCreated | Nueva versión de certificado creada | Se desencadena cuando se crea un nuevo certificado o una nueva versión de certificado. |
| Microsoft.KeyVault.CertificateNearExpiry | El certificado está a punto de expirar | Se desencadena cuando la versión actual del certificado está a punto de expirar. (El evento se desencadena 30 días antes de la fecha de expiración). |
| Microsoft.KeyVault.CertificateExpired | Certificado expirado | Se desencadena cuando el certificado ha expirado. |
| Microsoft.KeyVault.KeyNewVersionCreated | Nueva versión de clave creada | Se desencadena cuando se crea una nueva clave o nueva versión de clave. |
| Microsoft.KeyVault.KeyNearExpiry | La clave está a punto de expirar | Se desencadena cuando la versión actual de una clave está a punto de expirar. (El evento se desencadena 30 días antes de la fecha de expiración). |
| Microsoft.KeyVault.KeyExpired | Clave expirada | Se desencadena cuando una clave ha expirado. |
| Microsoft.KeyVault.SecretNewVersionCreated | Secreto nueva versión creada | Se desencadena cuando se crea una nueva versión de secreto o secreto nuevo. |
| Microsoft.KeyVault.SecretNearExpiry | El secreto está a punto de expirar | Se desencadena cuando la versión actual de un secreto está a punto de expirar. (El evento se desencadena 30 días antes de la fecha de expiración). |
| Microsoft.KeyVault.SecretExpired | Secreto expirado | Se desencadena cuando un secreto ha expirado. |
| Microsoft.KeyVault.VaultAccessPolicyChanged | Directiva de acceso de Vault modificada | Se desencadena cuando cambia una directiva de acceso en Key Vault. Incluye un escenario en el que el modelo de permisos de Key Vault se modifica para o desde Azure RBAC.  |

### <a name="event-examples"></a>Ejemplos de eventos

En el ejemplo siguiente se muestra el esquema para **Microsoft.KeyVault.SecretNewVersionCreated**:

```JSON
[
   {
      "id":"00eccf70-95a7-4e7c-8299-2eb17ee9ad64",
      "topic":"/subscriptions/{subscription-id}/resourceGroups/sample-rg/providers/Microsoft.KeyVault/vaults/sample-kv",
      "subject":"newsecret",
      "eventType":"Microsoft.KeyVault.SecretNewVersionCreated",
      "eventTime":"2019-07-25T01:08:33.1036736Z",
      "data":{
         "Id":"https://sample-kv.vault.azure.net/secrets/newsecret/ee059b2bb5bc48398a53b168c6cdcb10",
         "vaultName":"sample-kv",
         "objectType":"Secret",
         "objectName ":"newsecret",
         "version":" ee059b2bb5bc48398a53b168c6cdcb10",
         "nbf":"1559081980",
         "exp":"1559082102"
      },
      "dataVersion":"1",
      "metadataVersion":"1"
   }
]
```

### <a name="event-properties"></a>Propiedades de evento

Un evento tiene los siguientes datos de nivel superior:

| Propiedad | Tipo | Descripción |
| ---------- | ----------- |---|
| id | string | El identificador del objeto que desencadenó este evento |
| vaultName | string | Nombre del almacén de claves del objeto que desencadenó este evento |
| objectType | string | El tipo del objeto que desencadenó este evento |
| objectName | string | El nombre del objeto que desencadenó este evento |
| version | string | La versión del objeto que desencadenó este evento |
| nbf | number | La fecha no antes de segundos desde 1970-01-01T00:00:00Z del objeto que desencadenó este evento |
| exp | number | Fecha de expiración en segundos desde 1970-01-01T00:00:00Z del objeto que desencadenó este evento |

## <a name="tutorials-and-how-tos"></a>Tutoriales y procedimientos
|Título  |Descripción  |
|---------|---------|
| [Supervisión de eventos de Key Vault con Azure Event Grid](../key-vault/general/event-grid-overview.md) | Información general sobre la integración de Key Vault con Event Grid. |
| [Tutorial: Creación y supervisión de eventos de Key Vault con Event Grid](../key-vault/general/event-grid-logicapps.md) | Obtenga información sobre cómo configurar notificaciones de Event Grid para Key Vault. |


## <a name="next-steps"></a>Pasos siguientes

* Para una introducción a Azure Event Grid, consulte el artículo de [introducción a Event Grid](overview.md).
* Para más información sobre cómo crear una suscripción de Azure Event Grid, consulte [Esquema de suscripción de Event Grid](subscription-creation-schema.md).
* Para más información sobre Key Vault, consulte [¿Qué es Azure Key Vault?](../key-vault/general/overview.md)

