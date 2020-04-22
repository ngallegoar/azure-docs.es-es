---
title: Tutorial de rotación de usuario único y contraseña única
description: Use este tutorial para aprender a automatizar la rotación de secretos para los recursos que usan la autenticación de usuario único y contraseña única.
services: key-vault
author: msmbaldwin
manager: rkarlin
tags: rotation
ms.service: key-vault
ms.subservice: general
ms.topic: tutorial
ms.date: 01/26/2020
ms.author: mbaldwin
ms.openlocfilehash: 70eb2449c5c54750831c30ff7d5c948173a38594
ms.sourcegitcommit: b80aafd2c71d7366838811e92bd234ddbab507b6
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 04/16/2020
ms.locfileid: "81426354"
---
# <a name="automate-the-rotation-of-a-secret-for-resources-that-use-single-usersingle-password-authentication"></a>Automatización de la rotación de secretos para recursos que usan la autenticación de usuario único y contraseña única

Aunque la mejor forma de realizar la autenticación en los servicios de Azure es mediante una [identidad administrada](../general/managed-identity.md), hay algunos escenarios en los que no es posible. En estos casos, se usan claves de acceso o secretos. Tanto las claves de acceso como los secretos deben rotarse periódicamente.

En este tutorial se muestra cómo automatizar la rotación periódica de secretos de bases de datos y servicios que usan la autenticación de usuario único y contraseña única. Específicamente, en este tutorial se rotan las contraseñas de SQL Server almacenadas en Azure Key Vault mediante el uso de una función desencadenada por una notificación de Azure Event Grid:

![Diagrama de la solución de rotación](../media/rotate1.png)

1. Treinta días antes de la fecha de expiración de un secreto, Key Vault publica el evento de "expiración cercana" en Event Grid.
1. Event Grid comprueba las suscripciones del evento y usa HTTP POST para llamar al punto de conexión de la aplicación de funciones suscrita al evento.
1. La aplicación de funciones recibe la información del secreto, genera una nueva contraseña aleatoria y crea una versión del secreto con la contraseña nueva en Key Vault.
1. La aplicación de funciones actualiza SQL Server con la nueva contraseña.

> [!NOTE]
> Podría haber un retraso entre los pasos 3 y 4. Durante ese tiempo, el secreto de Key Vault no podrá autenticarse en SQL Server. En caso de que se produzca un error en cualquiera de los pasos, Event Grid hace reintentos durante dos horas.

## <a name="create-a-key-vault-and-sql-server-instance"></a>Creación de un almacén de claves y una instancia de SQL Server

El primer paso consiste en crear un almacén de claves, una instancia de SQL Server y una base de datos y almacenar la contraseña de administrador de SQL Server en Key Vault.

Para crear los componentes, en este tutorial se usa una plantilla de Azure Resource Manager existente. Puede encontrar el código aquí: [Ejemplo de plantilla básica para la rotación de secretos](https://github.com/jlichwa/azure-keyvault-basicrotation-tutorial/tree/master/arm-templates).

1. Seleccione el vínculo de implementación de la plantilla de Azure:
<br><a href="https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2Fjlichwa%2Fazure-keyvault-basicrotation-tutorial%2Fmaster%2Farm-templates%2Finitial-setup%2Fazuredeploy.json" target="_blank"> <img src="https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/1-CONTRIBUTION-GUIDE/images/deploytoazure.png"/></a>
1. En **Grupo de recursos**, seleccione **Crear nuevo**. Asigne al grupo el nombre **simplerotation**.
1. Seleccione **Comprar**.

    ![Crear un grupo de recursos](../media/rotate2.png)

Ahora tendrá un almacén de claves, una instancia de SQL Server y una base de datos SQL. Puede comprobar esta configuración en la CLI de Azure mediante la ejecución del comando siguiente:

```azurecli
az resource list -o table
```

El resultado tendrá un aspecto similar a la salida siguiente:

```console
Name                     ResourceGroup         Location    Type                               Status
-----------------------  --------------------  ----------  ---------------------------------  --------
simplerotation-kv          simplerotation      eastus      Microsoft.KeyVault/vaults
simplerotation-sql         simplerotation      eastus      Microsoft.Sql/servers
simplerotation-sql/master  simplerotation      eastus      Microsoft.Sql/servers/databases
```

## <a name="create-a-function-app"></a>Creación de una aplicación de función

A continuación, cree una aplicación de funciones con una identidad administrada por el sistema, además de los componentes necesarios adicionales.

La aplicación de funciones requiere estos componentes:
- Un plan de Azure App Service
- Una cuenta de almacenamiento
- Una directiva de acceso para acceder a los secretos de Key Vault mediante la identidad administrada de la aplicación de funciones

1. Seleccione el vínculo de implementación de la plantilla de Azure:
<br><a href="https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2Fjlichwa%2Fazure-keyvault-basicrotation-tutorial%2Fmaster%2Farm-templates%2Ffunction-app%2Fazuredeploy.json" target="_blank"><img src="https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/1-CONTRIBUTION-GUIDE/images/deploytoazure.png"/></a>
1. En la lista **Grupo de recursos**, seleccione **simplerotation**.
1. Seleccione **Comprar**.

   ![Seleccionar Comprar](../media/rotate3.png)

Tras completar los pasos anteriores tendrá una cuenta de almacenamiento, una granja de servidores y una aplicación de funciones. Puede comprobar esta configuración en la CLI de Azure mediante la ejecución del comando siguiente:

```azurecli
az resource list -o table
```

El resultado tendrá un aspecto similar al de la salida siguiente:

```console
Name                     ResourceGroup         Location    Type                               Status
-----------------------  --------------------  ----------  ---------------------------------  --------
simplerotation-kv          simplerotation       eastus      Microsoft.KeyVault/vaults
simplerotation-sql         simplerotation       eastus      Microsoft.Sql/servers
simplerotation-sql/master  simplerotation       eastus      Microsoft.Sql/servers/databases
simplerotationstrg         simplerotation       eastus      Microsoft.Storage/storageAccounts
simplerotation-plan        simplerotation       eastus      Microsoft.Web/serverFarms
simplerotation-fn          simplerotation       eastus      Microsoft.Web/sites
```

Para obtener información sobre cómo crear una aplicación de funciones y usar una identidad administrada para acceder a Key Vault, consulte [Creación de una aplicación de funciones desde Azure Portal](../../azure-functions/functions-create-function-app-portal.md) y [Autenticación de Key Vault con una identidad administrada](../general/managed-identity.md).

### <a name="rotation-function"></a>Función de rotación
La función utiliza un evento para desencadenar la rotación de un secreto mediante la actualización de Key Vault y la base de datos SQL.

#### <a name="function-trigger-event"></a>Evento de desencadenador de la función

Esta función lee los datos del evento y ejecuta la lógica de la rotación:

```csharp
public static class SimpleRotationEventHandler
{
    [FunctionName("SimpleRotation")]
       public static void Run([EventGridTrigger]EventGridEvent eventGridEvent, ILogger log)
       {
            log.LogInformation("C# Event trigger function processed a request.");
            var secretName = eventGridEvent.Subject;
            var secretVersion = Regex.Match(eventGridEvent.Data.ToString(), "Version\":\"([a-z0-9]*)").Groups[1].ToString();
            var keyVaultName = Regex.Match(eventGridEvent.Topic, ".vaults.(.*)").Groups[1].ToString();
            log.LogInformation($"Key Vault Name: {keyVaultName}");
            log.LogInformation($"Secret Name: {secretName}");
            log.LogInformation($"Secret Version: {secretVersion}");

            SeretRotator.RotateSecret(log, secretName, secretVersion, keyVaultName);
        }
}
```

#### <a name="secret-rotation-logic"></a>Lógica de rotación de secretos
Este método de rotación lee la información de la base de datos del secreto, crea una nueva versión del secreto y actualiza la base de datos con el nuevo secreto:

```csharp
public class SecretRotator
    {
       private const string UserIdTagName = "UserID";
       private const string DataSourceTagName = "DataSource";
       private const int SecretExpirationDays = 31;

    public static void RotateSecret(ILogger log, string secretName, string secretVersion, string keyVaultName)
    {
           //Retrieve current secret
           var kvUri = "https://" + keyVaultName + ".vault.azure.net";
           var client = new SecretClient(new Uri(kvUri), new DefaultAzureCredential());
           KeyVaultSecret secret = client.GetSecret(secretName, secretVersion);
           log.LogInformation("Secret Info Retrieved");
        
           //Retrieve secret info
           var userId = secret.Properties.Tags.ContainsKey(UserIdTagName) ?  
                        secret.Properties.Tags[UserIdTagName] : "";
           var datasource = secret.Properties.Tags.ContainsKey(DataSourceTagName) ? 
                            secret.Properties.Tags[DataSourceTagName] : "";
           log.LogInformation($"Data Source Name: {datasource}");
           log.LogInformation($"User Id Name: {userId}");
        
           //Create new password
           var randomPassword = CreateRandomPassword();
           log.LogInformation("New Password Generated");
        
           //Check DB connection using existing secret
           CheckServiceConnection(secret);
           log.LogInformation("Service Connection Validated");
                    
           //Create new secret with generated password
           CreateNewSecretVersion(client, secret, randomPassword);
           log.LogInformation("New Secret Version Generated");
        
           //Update DB password
           UpdateServicePassword(secret, randomPassword);
           log.LogInformation("Password Changed");
           log.LogInformation($"Secret Rotated Succesffuly");
    }
}
```
Puede encontrar el código completo en [GitHub](https://github.com/jlichwa/azure-keyvault-basicrotation-tutorial/tree/master/rotation-function).

#### <a name="function-deployment"></a>Implementación de la función

1. Descargue el archivo ZIP de la aplicación de funciones desde [GitHub](https://github.com/jlichwa/azure-keyvault-basicrotation-tutorial/raw/master/simplerotationsample-fn.zip).

1. Cargue el archivo simplerotationsample-fn.zip en Azure Cloud Shell.

   ![Carga del archivo](../media/rotate4.png)
1. Use este comando de la CLI de Azure para implementar el archivo zip en la aplicación de funciones:

   ```azurecli
   az functionapp deployment source config-zip -g simplerotation -n simplerotation-fn --src /home/{firstname e.g jack}/simplerotationsample-fn.zip
   ```

Una vez implementada la función, debería ver dos funciones en simplerotation-fn:

![Funciones SimpleRotation y SimpleRotationHttpTest](../media/rotate5.png)

## <a name="add-an-event-subscription-for-the-secretnearexpiry-event"></a>Adición de una suscripción a eventos para el evento SecretNearExpiry

Copie la clave `eventgrid_extension` de la aplicación de funciones:

   ![Seleccionar Configuración de Function App](../media/rotate6.png)

   ![clave eventgrid_extension](../media/rotate7.png)

Use la clave `eventgrid_extension` copiada y el identificador de suscripción en el siguiente comando para crear una suscripción a Event Grid para los eventos `SecretNearExpiry`:

```azurecli
az eventgrid event-subscription create --name simplerotation-eventsubscription --source-resource-id "/subscriptions/<subscription-id>/resourceGroups/simplerotation/providers/Microsoft.KeyVault/vaults/simplerotation-kv" --endpoint "https://simplerotation-fn.azurewebsites.net/runtime/webhooks/EventGrid?functionName=SimpleRotation&code=<extension-key>" --endpoint-type WebHook --included-event-types "Microsoft.KeyVault.SecretNearExpiry"
```

## <a name="add-the-secret-to-key-vault"></a>Adición del secreto a Key Vault
Establezca una directiva de acceso para conceder a los usuarios el permiso para *administrar secretos*.

```azurecli
az keyvault set-policy --upn <email-address-of-user> --name simplerotation-kv --secret-permissions set delete get list
```

Cree un nuevo secreto con etiquetas que contengan el origen de datos de la base de datos SQL y el identificador de usuario. Incluya una fecha de expiración establecida para mañana.

```azurecli
$tomorrowDate = (get-date).AddDays(+1).ToString("yyy-MM-ddThh:mm:ssZ")
az keyvault secret set --name sqluser --vault-name simplerotation-kv --value "Simple123" --tags "UserID=azureuser" "DataSource=simplerotation-sql.database.windows.net" --expires $tomorrowDate
```

La creación de un secreto con una fecha de expiración corta publicará inmediatamente un evento `SecretNearExpiry`, que a su vez desencadenará la función para rotar el secreto.

## <a name="test-and-verify"></a>Prueba y comprobación
Pocos minutos después el secreto `sqluser` debería rotar automáticamente.

Para comprobar que el secreto ha rotado, vaya a **Key Vault** > **Secretos**:

![Ir a Secretos](../media/rotate8.png)

Abra el secreto **sqluser** y consulte la versión original y la rotada:

![Abrir el secreto sqluser](../media/rotate9.png)

### <a name="create-a-web-app"></a>Creación de una aplicación web

Para comprobar las credenciales de SQL, cree una aplicación web. Esta aplicación web obtendrá el secreto de Key Vault, extraerá la información y las credenciales de la base de datos SQL del secreto, y probará la conexión con SQL Server.

La aplicación web requiere estos componentes:
- Una aplicación web con una identidad administrada por el sistema.
- Una directiva de acceso para acceder a los secretos de Key Vault mediante la identidad administrada de la aplicación web.

1. Seleccione el vínculo de implementación de la plantilla de Azure:
<br><a href="https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2Fjlichwa%2Fazure-keyvault-basicrotation-tutorial%2Fmaster%2Farm-templates%2Fweb-app%2Fazuredeploy.json" target="_blank"> <img src="https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/1-CONTRIBUTION-GUIDE/images/deploytoazure.png"/></a>
1. Seleccione el grupo de recursos **simplerotation**.
1. Seleccione **Comprar**.

### <a name="deploy-the-web-app"></a>Implementación de la aplicación web

Puede encontrar el código fuente de la aplicación web en [GitHub](https://github.com/jlichwa/azure-keyvault-basicrotation-tutorial/tree/master/test-webapp).

Siga estos pasos para implementar la aplicación web:

1. Descargue el archivo ZIP de la aplicación de funciones desde [GitHub](https://github.com/jlichwa/azure-keyvault-basicrotation-tutorial/raw/master/simplerotationsample-app.zip).
1. Cargue el archivo simplerotationsample-app.zip en Azure Cloud Shell.
1. Use este comando de la CLI de Azure para implementar el archivo zip en la aplicación de funciones:

   ```azurecli
   az webapp deployment source config-zip -g simplerotation -n simplerotation-app --src /home/{firstname e.g jack}/simplerotationsample-app.zip
   ```

### <a name="open-the-web-app"></a>Abrir la aplicación web

Vaya a la aplicación implementada y seleccione la dirección URL:
 
![Selección de la dirección URL](../media/rotate10.png)

Verá el valor del secreto generado con el valor de base de datos conectada establecido en True.

## <a name="learn-more"></a>Más información

- Introducción: [Supervisión de Key Vault con Azure Event Grid (versión preliminar)](../general/event-grid-overview.md)
- Procedimientos: [Recibir un correo electrónico al cambiar un secreto del almacén de claves](../general/event-grid-logicapps.md)
- [Esquema de eventos de Azure Event Grid para Azure Key Vault (versión preliminar)](../../event-grid/event-schema-key-vault.md)