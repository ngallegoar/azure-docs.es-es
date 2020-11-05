---
title: Registro de Azure Key Vault | Microsoft Docs
description: Aprenda a supervisar el acceso a los almacenes de claves habilitando el registro para Azure Key Vault, con lo que se guarda la información en una cuenta de almacenamiento de Azure proporcionada por el usuario.
services: key-vault
author: msmbaldwin
manager: rkarlin
tags: azure-resource-manager
ms.service: key-vault
ms.subservice: general
ms.topic: how-to
ms.date: 08/12/2019
ms.author: mbaldwin
ms.openlocfilehash: 5423fc27ecc58bcd79b36a845e4b7569f342f712
ms.sourcegitcommit: 7863fcea618b0342b7c91ae345aa099114205b03
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 11/03/2020
ms.locfileid: "93286705"
---
# <a name="azure-key-vault-logging"></a>Registro de Azure Key Vault

Después de crear uno o varios almacenes de claves, es probable que desee supervisar cómo y cuándo se accede a los almacenes de claves y quién lo hace. Puede hacerlo al habilitar el registro de Azure Key Vault, que guarda la información en una cuenta de almacenamiento de Azure proporcionada por el usuario. Para obtener instrucciones paso a paso sobre cómo configurar esta opción, consulte [Habilitación del registro de Key Vault](howto-logging.md).

Puede acceder a la información de registro 10 minutos (como máximo) después de la operación del almacén de claves. En la mayoría de los casos, será más rápido que esto.  Es decisión suya administrar los registros en la cuenta de almacenamiento:

* Utilice los métodos de control de acceso de Azure estándar para proteger los registros mediante la restricción de quién puede tener acceso a ellos.
* Elimine los registros que ya no desee mantener en la cuenta de almacenamiento.

Para obtener información general sobre Key Vault, consulte [¿Qué es Azure Key Vault?](overview.md) Para obtener información acerca de dónde está disponible Key Vault, consulte la [página de precios](https://azure.microsoft.com/pricing/details/key-vault/). Para obtener información acerca del uso de [Azure Monitor para Key Vault](../../azure-monitor/insights/key-vault-insights-overview.md).

## <a name="interpret-your-key-vault-logs"></a>Interpretación de los registros de Key Vault

Al habilitar el registro, un nuevo contenedor denominado **insights-logs-auditevent** se crea automáticamente para su cuenta de almacenamiento especificada. Puede usar esta misma cuenta de almacenamiento para recopilar registros de varios almacenes de claves.

Los blobs individuales se almacenan como texto, con formato de blob JSON. Veamos una entrada de registro como ejemplo. 

```json
    {
        "records":
        [
            {
                "time": "2016-01-05T01:32:01.2691226Z",
                "resourceId": "/SUBSCRIPTIONS/361DA5D4-A47A-4C79-AFDD-XXXXXXXXXXXX/RESOURCEGROUPS/CONTOSOGROUP/PROVIDERS/MICROSOFT.KEYVAULT/VAULTS/CONTOSOKEYVAULT",
                "operationName": "VaultGet",
                "operationVersion": "2015-06-01",
                "category": "AuditEvent",
                "resultType": "Success",
                "resultSignature": "OK",
                "resultDescription": "",
                "durationMs": "78",
                "callerIpAddress": "104.40.82.76",
                "correlationId": "",
                "identity": {"claim":{"http://schemas.microsoft.com/identity/claims/objectidentifier":"d9da5048-2737-4770-bd64-XXXXXXXXXXXX","http://schemas.xmlsoap.org/ws/2005/05/identity/claims/upn":"live.com#username@outlook.com","appid":"1950a258-227b-4e31-a9cf-XXXXXXXXXXXX"}},
                "properties": {"clientInfo":"azure-resource-manager/2.0","requestUri":"https://control-prod-wus.vaultcore.azure.net/subscriptions/361da5d4-a47a-4c79-afdd-XXXXXXXXXXXX/resourcegroups/contosoresourcegroup/providers/Microsoft.KeyVault/vaults/contosokeyvault?api-version=2015-06-01","id":"https://contosokeyvault.vault.azure.net/","httpStatusCode":200}
            }
        ]
    }
```

En la tabla siguiente se muestran los nombres y las descripciones de los campos:

| Nombre del campo | Descripción |
| --- | --- |
| **time** |Fecha y hora en UTC. |
| **resourceId** |Identificador de recursos de Azure Resource Manager. Para los registros de Key Vault, siempre es el identificador de recurso de Key Vault. |
| **operationName** |Nombre de la operación, como se documenta en la tabla siguiente. |
| **operationVersion** |Versión de la API REST solicitada por el cliente. |
| **category** |Tipo de resultado. Para los registros de Key Vault, `AuditEvent` es el único valor disponible. |
| **resultType** |Resultado de la solicitud de la API REST. |
| **resultSignature** |Estado de HTTP |
| **resultDescription** |Descripción adicional acerca del resultado, cuando esté disponible. |
| **durationMs** |Tiempo que tardó en atender la solicitud de API de REST, en milisegundos. Esto no incluye la latencia de red, por lo que el tiempo que se mide en el cliente podría no coincidir con este tiempo. |
| **callerIpAddress** |Dirección IP del cliente que realizó la solicitud. |
| **correlationId** |Un GUID opcional que el cliente puede pasar para correlacionar los registros del lado cliente con los registros del lado servicio (Key Vault). |
| **identity** |Identidad del token que se ha presentado al realizar la solicitud de la API REST. Suele ser un "usuario", una "entidad de servicio" o una combinación de "usuario + appId", como en el caso de una solicitud procedente de un cmdlet de Azure PowerShell. |
| **properties** |Información que varía en función de la operación ( **operationName** ). En la mayoría de los casos, este campo contiene información del cliente (la cadena del agente de usuario pasada por el cliente), el URI de la solicitud de la API REST exacta y el código de estado HTTP. Además, cuando se devuelve un objeto como resultado de una solicitud (por ejemplo, **KeyCreate** o **VaultGet** ) también contiene el URI de la clave (como `id`), el URI del almacén o el URI del secreto. |

Los valores del campo **operationName** tienen el formato *ObjectVerb*. Por ejemplo:

* Todas las operaciones del almacén de claves tienen el formato `Vault<action>`, como `VaultGet` y `VaultCreate`.
* Todas las operaciones de claves tienen el formato `Key<action>`, como `KeySign` y `KeyList`.
* Todas las operaciones de secretos tienen el formato `Secret<action>`, como `SecretGet` y `SecretListVersions`.

En la tabla siguiente se muestran los valores **operationName** y los comandos correspondientes de la API REST:

### <a name="operation-names-table"></a>Tabla de nombres de operaciones

| operationName | Comando de la API REST |
| --- | --- |
| **Autenticación** |Autenticar mediante un punto de conexión de Azure Active Directory |
| **VaultGet** |[Obtener información acerca de un almacén de claves](/rest/api/keyvault/vaults) |
| **VaultPut** |[Crear o actualizar un almacén de claves](/rest/api/keyvault/vaults) |
| **VaultDelete** |[Eliminar un almacén de claves](/rest/api/keyvault/vaults) |
| **VaultPatch** |[Crear o actualizar un almacén de claves](/rest/api/keyvault/vaults) |
| **VaultList** |[Lista de todos los almacenes de claves en un grupo de recursos](/rest/api/keyvault/vaults) |
| **KeyCreate** |[Crear una clave](/rest/api/keyvault/createkey) |
| **KeyGet** |[Obtener información sobre una clave](/rest/api/keyvault/getkey) |
| **KeyImport** |[Importar una clave a un almacén](/rest/api/keyvault/vaults) |
| **KeyBackup** |[Hacer una copia de seguridad de una clave](/rest/api/keyvault/backupkey) |
| **KeyDelete** |[Eliminar una clave](/rest/api/keyvault/deletekey) |
| **KeyRestore** |[Restauración de una clave](/rest/api/keyvault/restorekey) |
| **KeySign** |[Firmar con una clave](/rest/api/keyvault/sign) |
| **KeyVerify** |[Comprobar con una clave](/rest/api/keyvault/vaults) |
| **KeyWrap** |[Encapsular una clave](/rest/api/keyvault/wrapkey) |
| **KeyUnwrap** |[Desencapsular una clave](/rest/api/keyvault/unwrapkey) |
| **KeyEncrypt** |[Cifrar con una clave](/rest/api/keyvault/encrypt) |
| **KeyDecrypt** |[Descifrado con una clave](/rest/api/keyvault/decrypt) |
| **KeyUpdate** |[Actualizar una clave](/rest/api/keyvault/updatekey) |
| **KeyList** |[Enumeración de las claves en un almacén](/rest/api/keyvault/vaults) |
| **KeyListVersions** |[Enumerar las versiones de una clave](/rest/api/keyvault/getkeyversions) |
| **SecretSet** |[Crear un secreto](/rest/api/keyvault/updatecertificate) |
| **SecretGet** |[Obtener un secreto](/rest/api/keyvault/getsecret) |
| **SecretUpdate** |[Actualizar un secreto](/rest/api/keyvault/updatesecret) |
| **SecretDelete** |[Eliminar un secreto](/rest/api/keyvault/deletesecret) |
| **SecretList** |[Enumerar secretos en un almacén](/rest/api/keyvault/vaults) |
| **SecretListVersions** |[Enumerar versiones de un secreto](/rest/api/keyvault/getsecretversions) |
| **VaultAccessPolicyChangedEventGridNotification** | La directiva de acceso del almacén ha cambiado el evento publicado. |
| **SecretNearExpiryEventGridNotification** |Se ha publicado el evento de expiración cercana del secreto. |
| **SecretExpiredEventGridNotification** |Se ha publicado el evento de expiración del secreto. |
| **KeyNearExpiryEventGridNotification** |Se ha publicado el evento de expiración cercana de la clave. |
| **KeyExpiredEventGridNotification** |Se ha publicado el evento de expiración de la clave. |
| **CertificateNearExpiryEventGridNotification** |Se ha publicado el evento de expiración cercana del certificado. |
| **CertificateExpiredEventGridNotification** |Se ha publicado el evento de expiración del certificado. |

## <a name="use-azure-monitor-logs"></a>Uso de registros de Azure Monitor

Puede utilizar la solución Key Vault en registros de Azure Monitor para revisar los registros `AuditEvent` de Key Vault. En los registros de Azure Monitor, las consultas de los registros se usan para analizar los datos y obtener la información que necesita. 

Para más información, incluido cómo configurar esta opción, consulte [Azure Key Vault en Azure Monitor](../../azure-monitor/insights/key-vault-insights-overview.md).

## <a name="next-steps"></a>Pasos siguientes

- [Habilitación del registro de Key Vault](howto-logging.md)
- Para ver un tutorial que use Azure Key Vault en una aplicación web .NET, consulte [Uso de Azure Key Vault desde una aplicación web](tutorial-net-create-vault-azure-web-app.md).
- Para conocer las referencias de programación, consulte la [Guía del desarrollador de Azure Key Vault](developers-guide.md).
- Para obtener una lista de los cmdlets de Azure PowerShell 1.0 para Azure Key Vault, consulte [Azure Key Vault Cmdlets](/powershell/module/az.keyvault/?view=azps-1.2.0#key_vault)(Cmdlets de Azure Key Vault).