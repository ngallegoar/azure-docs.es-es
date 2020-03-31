---
title: Tutorial de rotación de usuario único y contraseña
description: Use este tutorial para automatizar la rotación de usuario único y contraseña
services: key-vault
author: msmbaldwin
manager: rkarlin
tags: rotation
ms.service: key-vault
ms.subservice: general
ms.topic: tutorial
ms.date: 01/26/2020
ms.author: mbaldwin
ms.openlocfilehash: 890932f7c0e46a2c9c0b6e1cf1461e4d7d25b409
ms.sourcegitcommit: 0947111b263015136bca0e6ec5a8c570b3f700ff
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/24/2020
ms.locfileid: "79223362"
---
# <a name="automate-the-rotation-of-a-secret-for-resources-with-single-userpassword-authentication"></a>Automatización de la rotación de un secreto para recursos con la autenticación de usuario único y contraseña

Aunque la mejor forma de realizar la autenticación en los servicios de Azure es mediante una [identidad administrada](managed-identity.md), hay varios escenarios en que no es posible. En estos casos, se usan claves de acceso o secretos. Tanto las claves de acceso como los secretos deben rotarse periódicamente.

En este tutorial se muestra cómo automatizar la rotación periódica de secretos en bases de datos y servicios con la autenticación de usuario único y contraseña. En concreto, este escenario rota las contraseñas de SQL Server que se encuentran en Key Vault, para lo cual usa una función desencadenada por una notificación de Event Grid:

![Diagrama de la rotación](./media/rotate1.png)

1. Treinta días antes de la fecha de expiración de un secreto, Key Vault publica el evento de "expiración cercana" en Event Grid.
1. Event Grid comprueba las suscripciones del evento y usa HTTP POST para llamar al punto de conexión de la aplicación de funciones suscrita al evento.
1. La aplicación de funciones recibe la información del secreto, genera una contraseña aleatoria y crea una versión del secreto con una contraseña nueva en Key Vault.
1. La aplicación de funciones actualiza SQL con la nueva contraseña.

> [!NOTE]
> Puede haber un intervalo entre los pasos 3 y 4; durante ese tiempo, el secreto de Key Vault no sería válido para realizar la autenticación en SQL. Si no se puede realizar cualquiera de los pasos, Event Grid vuelve a intentarlo durante dos horas.

## <a name="setup"></a>Configurar

## <a name="create-a-key-vault-and-sql-server"></a>Creación de un almacén de claves y un servidor con SQL Server

Antes de empezar, debemos crear un almacén de claves, un servidor con SQL Server y una base de datos SQL, y almacenar la contraseña de administrador de SQL Server en el almacén de claves.

Para crear los componentes, en este tutorial se usa una plantilla de Azure Resource Manager creada previamente. Aquí puede encontrar todo el código: [Ejemplo de plantilla básica para la rotación de secretos](https://github.com/jlichwa/azure-keyvault-basicrotation-tutorial/tree/master/arm-templates).

1. Haga clic en el vínculo de implementación de la plantilla de Azure:
<br><a href="https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2Fjlichwa%2Fazure-keyvault-basicrotation-tutorial%2Fmaster%2Farm-templates%2Finitial-setup%2Fazuredeploy.json" target="_blank"> <img src="https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/1-CONTRIBUTION-GUIDE/images/deploytoazure.png"/></a>
1. En "Grupo de recursos", seleccione "Crear nuevo" y asígnele el nombre "simplerotation".
1. Seleccione "Comprar".

    ![Crear un grupo de recursos](./media/rotate2.png)

Cuando complete estos pasos, tendrá un almacén de claves, un servidor con SQL Server y una base de datos SQL. Esto es algo que se puede comprobar en el terminal de la CLI de Azure mediante la ejecución de:

```azurecli
az resource list -o table
```

El resultado será similar a este:

```console
Name                     ResourceGroup         Location    Type                               Status
-----------------------  --------------------  ----------  ---------------------------------  --------
simplerotation-kv             simplerotation             eastus      Microsoft.KeyVault/vaults
simplerotation-sql            simplerotation             eastus      Microsoft.Sql/servers
simplerotation-sql/master     simplerotation             eastus      Microsoft.Sql/servers/databases
```

## <a name="create-function-app"></a>Creación de una aplicación de funciones

Cree una aplicación de funciones con una identidad administrada por el sistema, así como los componentes necesarios adicionales: 

La aplicación de funciones requiere los componentes y la configuración siguientes:
- Plan de servicio de aplicación
- Cuenta de almacenamiento
- Directiva de acceso para acceder a los secretos de Key Vault mediante la identidad administrada de la aplicación de funciones

1. Haga clic en el vínculo de implementación de la plantilla de Azure:
<br><a href="https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2Fjlichwa%2Fazure-keyvault-basicrotation-tutorial%2Fmaster%2Farm-templates%2Ffunction-app%2Fazuredeploy.json" target="_blank"><img src="https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/1-CONTRIBUTION-GUIDE/images/deploytoazure.png"/></a>
1. En "Grupo de recursos", seleccione "simplerotation".
1. Seleccione "Comprar".

   ![Pantalla de compra](./media/rotate3.png)

Tras completar los pasos anteriores tendrá una cuenta de almacenamiento, una granja de servidores y una aplicación de funciones.  Esto es algo que se puede comprobar en el terminal de la CLI de Azure mediante la ejecución de:

```azurecli
az resource list -o table
```

El resultado será similar a este:

```console
Name                     ResourceGroup         Location    Type                               Status
-----------------------  --------------------  ----------  ---------------------------------  --------
simplerotation-kv             simplerotation             eastus      Microsoft.KeyVault/vaults
simplerotation-sql            simplerotation             eastus      Microsoft.Sql/servers
simplerotation-sql/master     simplerotation             eastus      Microsoft.Sql/servers/databases
simplerotationstrg            simplerotation             eastus      Microsoft.Storage/storageAccounts
simplerotation-plan           simplerotation             eastus      Microsoft.Web/serverFarms
simplerotation-fn             simplerotation             eastus      Microsoft.Web/sites
```

Para obtener información sobre cómo crear una aplicación de funciones y usar Identidad administrada para acceder a Key Vault, consulte [Creación de una instancia de Function App desde Azure Portal](../azure-functions/functions-create-function-app-portal.md) y [Autenticación de Key Vault con una identidad administrada](managed-identity.md)

### <a name="rotation-function-and-deployment"></a>Implementación y función de la rotación
La función usa un evento como desencadenador y realiza la rotación de un secreto que actualiza Key Vault y la base de datos SQL.

#### <a name="function-event-trigger-handler"></a>Controlador de desencadenador de eventos de la función

A continuación, la función lee los datos del evento y ejecuta la lógica de la rotación

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
Este método de rotación lee la información de la base de datos del secreto, crea una nueva versión del secreto y actualiza la base de datos con un nuevo secreto.

```csharp
public class SecretRotator
    {
       private const string UserIdTagName = "UserID";
       private const string DataSourceTagName = "DataSource";
       private const int SecretExpirationDays = 31;

    public static void RotateSecret(ILogger log, string secretName, string secretVersion, string keyVaultName)
    {
           //Retrieve Current Secret
           var kvUri = "https://" + keyVaultName + ".vault.azure.net";
           var client = new SecretClient(new Uri(kvUri), new DefaultAzureCredential());
           KeyVaultSecret secret = client.GetSecret(secretName, secretVersion);
           log.LogInformation("Secret Info Retrieved");
        
           //Retrieve Secret Info
           var userId = secret.Properties.Tags.ContainsKey(UserIdTagName) ?  
                        secret.Properties.Tags[UserIdTagName] : "";
           var datasource = secret.Properties.Tags.ContainsKey(DataSourceTagName) ? 
                            secret.Properties.Tags[DataSourceTagName] : "";
           log.LogInformation($"Data Source Name: {datasource}");
           log.LogInformation($"User Id Name: {userId}");
        
           //create new password
           var randomPassword = CreateRandomPassword();
           log.LogInformation("New Password Generated");
        
           //Check db connection using existing secret
           CheckServiceConnection(secret);
           log.LogInformation("Service Connection Validated");
                    
           //Create new secret with generated password
           CreateNewSecretVersion(client, secret, randomPassword);
           log.LogInformation("New Secret Version Generated");
        
           //Update db password
           UpdateServicePassword(secret, randomPassword);
           log.LogInformation("Password Changed");
           log.LogInformation($"Secret Rotated Succesffuly");
    }
}
```
Aquí puede encontrar todo el código: https://github.com/jlichwa/azure-keyvault-basicrotation-tutorial/tree/master/rotation-function

#### <a name="function-deployment"></a>Implementación de la función

1. Descargue el archivo zip de la aplicación de funciones: https://github.com/jlichwa/azure-keyvault-basicrotation-tutorial/raw/master/simplerotationsample-fn.zip

1. Cargue el archivo simplerotationsample-fn.zip en Azure Cloud Shell.
 
1. Use el comando de la CLI siguiente para implementar el archivo zip en una aplicación de funciones:

```azurecli
az functionapp deployment source config-zip -g simplerotation -n simplerotation-fn --src /home/{firstname e.g jack}/simplerotationsample-fn.zip
```
![Pantalla de compra](./media/rotate4.png)

Después de la implementación debería ver dos funciones en simplerotation-fn:

![Azure Cloud Shell](./media/rotate5.png)

### <a name="add-event-subscription-for-secretnearexpiry-event"></a>Adición de una suscripción para el evento "SecretNearExpiry"

Copie la clave eventgrid_extension de la aplicación de funciones.

![Azure Cloud Shell](./media/rotate6.png)

![Prueba y comprobación](./media/rotate7.png)

Use la clave de extensión eventgrid copiada y el identificador de la suscripción en el comando siguiente para crear una suscripción de Event Grid para los eventos SecretNearExpiry.

```azurecli
az eventgrid event-subscription create --name simplerotation-eventsubscription --source-resource-id "/subscriptions/<subscription-id>/resourceGroups/simplerotation/providers/Microsoft.KeyVault/vaults/simplerotation-kv" --endpoint "https://simplerotation-fn.azurewebsites.net/runtime/webhooks/EventGrid?functionName=SimpleRotation&code=<extension-key>" --endpoint-type WebHook --included-event-types "Microsoft.KeyVault.SecretNearExpiry"
```

### <a name="add-secret-to-key-vault"></a>Incorporación de un secreto a Key Vault
Establezca una directiva de acceso para dar a los usuarios el permiso "administrar secretos".

```azurecli
az keyvault set-policy --upn <email-address-of-user> --name simplerotation-kv --secret-permissions set delete get list
```

Ahora cree un secreto con etiquetas que contengan el origen de datos de la base de datos SQL y el identificador de usuario, y con la fecha de expiración establecida para mañana.

```azurecli
$tomorrowDate = (get-date).AddDays(+1).ToString("yyy-MM-ddThh:mm:ssZ")
az keyvault secret set --name sqluser --vault-name simplerotation-kv --value "Simple123" --tags "UserID=azureuser" "DataSource=simplerotation-sql.database.windows.net" --expires $tomorrowDate
```

La creación de un secreto con una fecha de expiración corta publicará inmediatamente un evento SecretNearExpiry, que a su vez desencadenará la función para rotar el secreto.

### <a name="test-and-verify"></a>Prueba y comprobación
Pocos minutos después el secreto sqluser debería rotar automáticamente.

Para comprobar que el secreto ha rotado, vaya a Key Vault > Secretos

![Prueba y comprobación](./media/rotate8.png)

Abra el secreto "sqluser" y vea la versión original y la rotada

![Prueba y comprobación](./media/rotate9.png)

## <a name="create-web-app"></a>Crear aplicación web

Para comprobar las credenciales de SQL, cree una aplicación web, que obtendrá el secreto del almacén de claves de claves, extraerá la información y las credenciales de la base de datos SQL del secreto, y probará la conexión con SQL.

La aplicación web requiere los componentes y la configuración siguientes:
- Aplicación web con identidad administrada por el sistema
- Directiva de acceso para acceder a los secretos de Key Vault mediante la identidad administrada de la aplicación web

1. Haga clic en el vínculo de implementación de la plantilla de Azure:
<br><a href="https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2Fjlichwa%2Fazure-keyvault-basicrotation-tutorial%2Fmaster%2Farm-templates%2Fweb-app%2Fazuredeploy.json" target="_blank"> <img src="https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/1-CONTRIBUTION-GUIDE/images/deploytoazure.png"/></a>
1. Seleccione el grupo de recursos **simplerotation**.
1. Haga clic en Comprar.

### <a name="deploy-web-app"></a>Implementación de una aplicación web

El código fuente de la aplicación web se puede encontrar en https://github.com/jlichwa/azure-keyvault-basicrotation-tutorial/tree/master/test-webapp. Para la implementación de la aplicación web, siga estos pasos:

1. Descargue el archivo zip de la aplicación de funciones de https://github.com/jlichwa/azure-keyvault-basicrotation-tutorial/raw/master/simplerotationsample-app.zip.
1. Cargue el archivo `simplerotationsample-app.zip` en Azure Cloud Shell.
1. Use este comando de la CLI de Azure para implementar el archivo zip en la aplicación de funciones:

   ```azurecli
   az webapp deployment source config-zip -g simplerotation -n simplerotation-app --src /home/{firstname e.g jack}/simplerotationsample-app.zip
   ```

#### <a name="open-web-application"></a>Apertura de la aplicación web

Vaya a la aplicación implementada y haga clic en "URL":
 
![Prueba y comprobación](./media/rotate10.png)

El valor del secreto generado se debe mostrar con el valor true en Database Connected (Base de datos conectada).

## <a name="learn-more"></a>Más información:

- Introducción: [Supervisión de Key Vault con Azure Event Grid (versión preliminar)](event-grid-overview.md)
- Procedimientos: [Recibir un correo electrónico al cambiar un secreto del almacén de claves](event-grid-logicapps.md)
- [Esquema de eventos de Azure Event Grid para Azure Key Vault (versión preliminar)](../event-grid/event-schema-key-vault.md)
