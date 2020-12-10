---
title: Tutorial de rotación para recursos con dos conjuntos de credenciales
description: Use este tutorial para aprender a automatizar la rotación de secretos para los recursos que usan dos conjuntos de credenciales de autenticación.
services: key-vault
author: msmbaldwin
manager: rkarlin
tags: rotation
ms.service: key-vault
ms.subservice: secrets
ms.topic: tutorial
ms.date: 06/22/2020
ms.author: jalichwa
ms.openlocfilehash: 72541b8d8f8d8865c680c36f7f84cd91a4ce8ba2
ms.sourcegitcommit: 80c1056113a9d65b6db69c06ca79fa531b9e3a00
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 12/09/2020
ms.locfileid: "96903333"
---
# <a name="automate-the-rotation-of-a-secret-for-resources-that-have-two-sets-of-authentication-credentials"></a>Automatización de la rotación de un secreto para recursos que tienen dos conjuntos de credenciales de autenticación

Aunque la mejor forma de realizar la autenticación en los servicios de Azure es mediante una [identidad administrada](../general/authentication.md), hay algunos escenarios en los que no es posible. En esos casos, se usan claves de acceso o contraseñas. Debe rotar las claves de acceso y las contraseñas con frecuencia.

En este tutorial se muestra cómo automatizar la rotación periódica de secretos de bases de datos y servicios que usan dos conjuntos de credenciales de autenticación. En concreto, en este tutorial se muestra cómo rotar las claves de cuentas de Azure Storage almacenadas en Azure Key Vault como secretos. Usará una función desencadenada por notificaciones de Azure Event Grid. 

> [!NOTE]
> Puede hacer que las claves de la cuenta de almacenamiento se administren automáticamente en Key Vault si proporciona tokens de firma de acceso compartido para el acceso delegado a la cuenta de almacenamiento. Hay servicios que requieren cadenas de conexión de la cuenta de almacenamiento con claves de acceso. Esta es la solución recomendada para ese escenario.

Esta es la solución de rotación que se describe en este tutorial: 

![Diagrama que muestra la solución de rotación.](../media/secrets/rotation-dual/rotation-diagram.png)

En esta solución, Azure Key Vault almacena las claves de acceso individuales de la cuenta de almacenamiento como versiones del mismo secreto; la clave principal y la secundaria se alteran en versiones posteriores. Cuando se almacena una clave de acceso en la versión más reciente del secreto, se regenera la clave alternativa y se agrega a Key Vault como la versión nueva más reciente del secreto. La solución proporciona a la aplicación el ciclo completo de rotación para actualizar a la clave regenerada más reciente. 

1. Treinta días antes de la fecha de expiración de un secreto, Key Vault publica el evento de expiración próxima en Event Grid.
1. Event Grid comprueba las suscripciones del evento y usa HTTP POST para llamar al punto de conexión de la aplicación de funciones que se ha suscrito al evento.
1. La aplicación de funciones identifica la clave alternativa (no la más reciente) y llama a la cuenta de almacenamiento para regenerarla.
1. La aplicación de funciones agrega la nueva clave regenerada a Azure Key Vault como la nueva versión del secreto.

## <a name="prerequisites"></a>Requisitos previos
* Suscripción a Azure. [Cree una cuenta gratuita](https://azure.microsoft.com/free/?WT.mc_id=A261C142F).
* Azure Key Vault.
* Dos cuentas de Azure Storage.

Puede usar este vínculo de implementación si no tiene un almacén de claves ni cuentas de almacenamiento:

[![Vínculo con la etiqueta de implementación en Azure.](https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/1-CONTRIBUTION-GUIDE/images/deploytoazure.png)](https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2Fjlichwa%2FKeyVault-Rotation-StorageAccountKey-PowerShell%2Fmaster%2Farm-templates%2FInitial-Setup%2Fazuredeploy.json)

1. En **Grupo de recursos**, seleccione **Crear nuevo**. Asigne al grupo el nombre **akvrotation** y, a continuación, seleccione **Aceptar**.
1. Seleccione **Revisar + crear**.
1. Seleccione **Crear**.

    ![Captura de pantalla que muestra cómo se crea un grupo de recursos.](../media/secrets/rotation-dual/dual-rotation-1.png)

Ahora tendrá un almacén de claves y dos cuentas de almacenamiento. Puede comprobar esta configuración en la CLI de Azure mediante la ejecución de este comando:

```azurecli
az resource list -o table -g akvrotation
```

El resultado deberá tener un aspecto similar a esta salida:

```console
Name                     ResourceGroup         Location    Type                               Status
-----------------------  --------------------  ----------  ---------------------------------  --------
akvrotation-kv         akvrotation      eastus      Microsoft.KeyVault/vaults
akvrotationstorage     akvrotation      eastus      Microsoft.Storage/storageAccounts
akvrotationstorage2    akvrotation      eastus      Microsoft.Storage/storageAccounts
```

## <a name="create-and-deploy-the-key-rotation-function"></a>Creación e implementación de la función de rotación de claves

A continuación, va a crear una aplicación de funciones con una identidad administrada por el sistema, además de otros componentes necesarios. También va a implementar la función de rotación de las claves de la cuenta de almacenamiento.

La función de rotación de la aplicación de funciones requiere los siguientes componentes y configuración:
- Un plan de Azure App Service
- Una cuenta de almacenamiento para administrar los desencadenadores de la aplicación de funciones
- Una directiva de acceso para acceder a los secretos de Key Vault
- El rol de servicio del operador de claves de cuentas de almacenamiento asignado a la aplicación de funciones para que pueda acceder a las claves de acceso de la cuenta de almacenamiento
- Una función de rotación de claves con un desencadenador de eventos y un desencadenador HTTP (rotación a petición)
- Una suscripción a eventos de Event Grid para el evento **SecretNearExpiry**

1. Seleccione el vínculo de implementación de la plantilla de Azure: 

   [![Vínculo de implementación de la plantilla de Azure.](https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/1-CONTRIBUTION-GUIDE/images/deploytoazure.png)](https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2Fjlichwa%2FKeyVault-Rotation-StorageAccountKey-PowerShell%2Fmaster%2Farm-templates%2FFunction%2Fazuredeploy.json)

1. En la lista **Grupo de recursos**, seleccione **akvrotation**.
1. En el cuadro **Storage Account RG** (Grupo de recursos de la cuenta de almacenamiento), escriba el nombre del grupo de recursos en el que se encuentra la cuenta de almacenamiento. Conserve el valor predeterminado **[resourceGroup().name]** si la cuenta de almacenamiento ya se encuentra en el mismo grupo de recursos donde va a implementar la función de rotación de claves.
1. En el cuadro **Nombre de la cuenta de almacenamiento**, escriba el nombre de la cuenta de almacenamiento que contiene las claves de acceso que se van a rotar.
1. En el cuadro **Key Vault RG** (Grupo de recursos del almacén de claves), escriba el nombre del grupo de recursos donde se encuentra el almacén de claves. Conserve el valor predeterminado **[resourceGroup().name]** si el almacén de claves ya existe en el mismo grupo de recursos donde va a implementar la función de rotación de claves.
1. En el cuadro **Nombre del almacén de claves**, escriba el nombre del almacén de claves.
1. En el cuadro **Nombre de la aplicación de funciones**, escriba el nombre de la aplicación de funciones.
1. En el cuadro **Nombre del secreto**, escriba el nombre del secreto en el que almacenará las claves de acceso.
1. En el cuadro **Repo URL** (Dirección URL del repositorio), escriba la ubicación en GitHub del código de la función: **https://github.com/jlichwa/KeyVault-Rotation-StorageAccountKey-PowerShell.git** .
1. Seleccione **Revisar + crear**.
1. Seleccione **Crear**.

   ![Captura de pantalla que muestra cómo se crea la primera cuenta de almacenamiento.](../media/secrets/rotation-dual/dual-rotation-2.png)

Tras completar los pasos anteriores tendrá una cuenta de almacenamiento, una granja de servidores, una aplicación de funciones y Application Insights. Una vez completada la implementación, verá esta página: ![Captura de pantalla que muestra la página Se completó la implementación.](../media/secrets/rotation-dual/dual-rotation-3.png)
> [!NOTE]
> Si se produce un error, puede seleccionar **Volver a implementar** para finalizar la implementación de los componentes.


Puede encontrar plantillas de implementación y código para la función de rotación en [GitHub](https://github.com/jlichwa/KeyVault-Rotation-StorageAccountKey-PowerShell).

## <a name="add-the-storage-account-access-keys-to-key-vault"></a>Incorporación de las claves de acceso de la cuenta de almacenamiento a Key Vault

En primer lugar, establezca una directiva de acceso para conceder a los usuarios el permiso para **administrar secretos**:

```azurecli
az keyvault set-policy --upn <email-address-of-user> --name akvrotation-kv --secret-permissions set delete get list
```

Ahora puede crear un nuevo secreto con una clave de acceso de la cuenta de almacenamiento como valor. También necesitará el identificador de recurso de la cuenta de almacenamiento, el período de validez del secreto y el identificador de clave para agregarlos al secreto, para que la función de rotación pueda volver a generar la clave en la cuenta de almacenamiento.

Determine el identificador de recurso de la cuenta de almacenamiento. Puede encontrar este valor en la propiedad `id`.
```azurecli
az storage account show -n akvrotationstorage
```

Enumere las claves de acceso de la cuenta de almacenamiento para que pueda obtener los valores de clave:

```azurecli
az storage account keys list -n akvrotationstorage 
```

Ejecute este comando con los valores recuperados para `key1Value` y `storageAccountResourceId`:

```azurecli
$tomorrowDate = (get-date).AddDays(+1).ToString("yyy-MM-ddThh:mm:ssZ")
az keyvault secret set --name storageKey --vault-name akvrotation-kv --value <key1Value> --tags "CredentialId=key1" "ProviderAddress=<storageAccountResourceId>" "ValidityPeriodDays=60" --expires $tomorrowDate
```

Si crea un secreto con una fecha de expiración corta, en pocos minutos se publicará un evento `SecretNearExpiry`. Este evento, a su vez, desencadenará la función para rotar el secreto.

Para comprobar que las claves de acceso se han regenerado, recupere la clave de la cuenta de almacenamiento y el secreto de Key Vault y compárelos.

Use este comando para obtener la información del secreto:
```azurecli
az keyvault secret show --vault-name akvrotation-kv --name storageKey
```
Observe que el valor de `CredentialId` se actualiza al valor de `keyName` alternativo y que se regenera `value`: ![Captura de pantalla que muestra la salida del comando a z keyvault secret show para la primera cuenta de almacenamiento.](../media/secrets/rotation-dual/dual-rotation-4.png)

Recupere las claves de acceso para comparar los valores:
```azurecli
az storage account keys list -n akvrotationstorage 
```
![Captura de pantalla que muestra la salida del comando a z storage account keys list para la primera cuenta de almacenamiento.](../media/secrets/rotation-dual/dual-rotation-5.png)

## <a name="add-storage-accounts-for-rotation"></a>Incorporación de cuentas de almacenamiento para la rotación

Puede reutilizar la misma aplicación de funciones para rotar las claves de varias cuentas de almacenamiento. 

Para agregar claves de cuenta de almacenamiento a una función existente para la rotación, necesita:
- El rol de servicio del operador de claves de cuentas de almacenamiento asignado a la aplicación de funciones para que pueda acceder a las claves de acceso de la cuenta de almacenamiento.
- Una suscripción a eventos de Event Grid para el evento **SecretNearExpiry**.

1. Seleccione el vínculo de implementación de la plantilla de Azure: 

   [![Vínculo de implementación de la plantilla de Azure.](https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/1-CONTRIBUTION-GUIDE/images/deploytoazure.png)](https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2Fjlichwa%2FKeyVault-Rotation-StorageAccountKey-PowerShell%2Fmaster%2Farm-templates%2FAdd-Event-Subscriptions%2Fazuredeploy.json)

1. En la lista **Grupo de recursos**, seleccione **akvrotation**.
1. En el cuadro **Nombre de la cuenta de almacenamiento**, escriba el nombre de la cuenta de almacenamiento que contiene las claves de acceso que se van a rotar.
1. En el cuadro **Nombre del almacén de claves**, escriba el nombre del almacén de claves.
1. En el cuadro **Nombre de la aplicación de funciones**, escriba el nombre de la aplicación de funciones.
1. En el cuadro **Nombre del secreto**, escriba el nombre del secreto en el que almacenará las claves de acceso.
1. Seleccione **Revisar + crear**.
1. Seleccione **Crear**.

   ![Captura de pantalla que muestra cómo se crea una cuenta de almacenamiento adicional.](../media/secrets/rotation-dual/dual-rotation-7.png)

### <a name="add-another-storage-account-access-key-to-key-vault"></a>Incorporación de otra clave de acceso de cuenta de almacenamiento a Key Vault

Determine el identificador de recurso de la cuenta de almacenamiento. Puede encontrar este valor en la propiedad `id`.
```azurecli
az storage account show -n akvrotationstorage2
```

Enumere las claves de acceso de la cuenta de almacenamiento para que pueda obtener el valor de key2:

```azurecli
az storage account keys list -n akvrotationstorage2 
```

Ejecute este comando con los valores recuperados para `key2Value` y `storageAccountResourceId`:

```azurecli
tomorrowDate=`date -d tomorrow -Iseconds -u | awk -F'+' '{print $1"Z"}'`
az keyvault secret set --name storageKey2 --vault-name akvrotation-kv --value <key2Value> --tags "CredentialId=key2" "ProviderAddress=<storageAccountResourceId>" "ValidityPeriodDays=60" --expires $tomorrowDate
```

Use este comando para obtener la información del secreto:
```azurecli
az keyvault secret show --vault-name akvrotation-kv --name storageKey2
```
Observe que el valor de `CredentialId` se actualiza al valor de `keyName` alternativo y que se regenera `value`: ![Captura de pantalla que muestra la salida del comando a z keyvault secret show para la segunda cuenta de almacenamiento.](../media/secrets/rotation-dual/dual-rotation-8.png)

Recupere las claves de acceso para comparar los valores:
```azurecli
az storage account keys list -n akvrotationstorage 
```
![Captura de pantalla que muestra la salida del comando a z storage account keys list para la segunda cuenta de almacenamiento.](../media/secrets/rotation-dual/dual-rotation-9.png)

## <a name="key-vault-dual-credential-rotation-functions"></a>Funciones de rotación de credenciales duales de Key Vault

- [Cuenta de almacenamiento](https://github.com/jlichwa/KeyVault-Rotation-StorageAccountKey-PowerShell)
- [Redis Cache](https://github.com/jlichwa/KeyVault-Rotation-RedisCacheKey-PowerShell)

## <a name="next-steps"></a>Pasos siguientes
- Introducción: [Supervisión de Key Vault con Azure Event Grid](../general/event-grid-overview.md)
- Procedimientos: [Creación de su primera función en Azure Portal](../../azure-functions/functions-create-first-azure-function.md)
- Procedimiento: [Recibir un correo electrónico al cambiar un secreto de Key Vault](../general/event-grid-logicapps.md)
- Referencia: [Esquema de eventos de Azure Event Grid para Azure Key Vault](../../event-grid/event-schema-key-vault.md)
