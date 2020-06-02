---
title: Configuración de Azure Key Vault con la auditoría y la rotación de claves de un extremo a otro | Microsoft Docs
description: Use esta guía paso a paso para configurar la rotación de claves y supervisar los registros del almacén de claves.
services: key-vault
author: msmbaldwin
manager: rkarlin
tags: ''
ms.service: key-vault
ms.subservice: secrets
ms.topic: conceptual
ms.date: 01/07/2019
ms.author: mbaldwin
ms.openlocfilehash: a5aaef50f12bfec89cf5e883ed6b1c85fa984ad6
ms.sourcegitcommit: 309a9d26f94ab775673fd4c9a0ffc6caa571f598
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 05/09/2020
ms.locfileid: "82995963"
---
# <a name="set-up-azure-key-vault-with-key-rotation-and-auditing"></a>Configuración de Azure Key Vault con la auditoría y la rotación de claves

## <a name="introduction"></a>Introducción

Cuando disponga de un almacén de claves, puede empezar a usarlo para almacenar claves y secretos. Ya no será necesario que las claves y los secretos se guarden en las aplicaciones, sino que estas los solicitarán al almacén cuando sea preciso. Un almacén de claves permite actualizar las claves y los secretos sin que esto afecte al rendimiento de la aplicación, lo que brinda un amplio abanico de posibilidades en lo que respecta a la administración de las claves y los secretos.

Este artículo le guía por el proceso de implementación de una rotación programada de claves de cuenta de almacenamiento, la supervisión de los registros de auditoría del almacén de claves y la generación de alertas cuando se realizan solicitudes inesperadas. 

Primero tiene que crear un almacén de claves mediante el método que elija:

- [Establecimiento y recuperación de un secreto desde Azure Key Vault mediante la CLI de Azure](quick-create-cli.md)
- [Establecimiento y recuperación de un secreto de Azure Key Vault mediante Azure PowerShell](quick-create-powershell.md)
- [Establecimiento y recuperación de un secreto de Azure Key Vault mediante Azure Portal](quick-create-portal.md)


## <a name="store-a-secret"></a>Almacenamiento de un secreto

Para que una aplicación pueda recuperar un secreto de Key Vault, primero debe crear el secreto y guardarlo en el almacén.

Inicie una sesión de PowerShell de Azure e inicie sesión en su cuenta de Azure con el siguiente comando:

```powershell
Connect-AzAccount
```

En la ventana emergente del explorador, escriba el nombre de usuario y la contraseña de la cuenta de Azure. PowerShell obtendrá todas las suscripciones asociadas a esta cuenta. PowerShell utiliza la primera de forma predeterminada.

Si tiene varias suscripciones, es posible que deba especificar la que se usó para crear el almacén de claves. Escriba lo siguiente para ver las suscripciones de su cuenta:

```powershell
Get-AzSubscription
```

Para especificar la suscripción asociada al almacén de claves que se va registrar, escriba:

```powershell
Set-AzContext -SubscriptionId <subscriptionID>
```

Dado que en este artículo se explica cómo se guarda una clave de cuenta de almacenamiento como un secreto, debe obtener dicha clave.

```powershell
Get-AzStorageAccountKey -ResourceGroupName <resourceGroupName> -Name <storageAccountName>
```

Una vez recuperado el secreto (en este caso, la clave de la cuenta de almacenamiento), debe convertir esa clave en una cadena segura y después crear un secreto con ese valor en el almacén de claves.

```powershell
$secretvalue = ConvertTo-SecureString <storageAccountKey> -AsPlainText -Force

Set-AzKeyVaultSecret -VaultName <vaultName> -Name <secretName> -SecretValue $secretvalue
```

A continuación, obtenga el identificador URI para el secreto que creó. Necesitará este URI en un paso posterior para llamar al almacén de claves y recuperar el secreto. Ejecute el siguiente comando de PowerShell y anote el valor de identificador, que es el URI del secreto:

```powershell
Get-AzKeyVaultSecret –VaultName <vaultName>
```

## <a name="set-up-the-application"></a>Configuración de la aplicación

Ahora que tiene un secreto almacenado, puede usar código para recuperarlo y utilizarlo después de realizar varios pasos más.

En primer lugar, tendrá que registrar la aplicación en Azure Active Directory. Después, indique a Key Vault la información de la aplicación para que pueda permitir solicitudes desde la aplicación.

> [!NOTE]
> La aplicación debe crearse en el mismo inquilino de Azure Active Directory que el del almacén de claves.

1. Inicie sesión en [Azure Portal](https://portal.azure.com) con una cuenta personal, profesional o educativa de Microsoft.
1. Si la cuenta proporciona acceso a más de un inquilino, seleccione su cuenta en la esquina superior derecha. Establezca la sesión del portal en el inquilino de Azure AD que desee.
1. Busque y seleccione **Azure Active Directory**. En **Administrar**, seleccione **Registros de aplicaciones**.
1. Seleccione **Nuevo registro**.
1. En **Registrar una aplicación**, escriba un nombre de aplicación significativo para mostrar a los usuarios.
1. Especifique quién puede usar la aplicación, como se indica a continuación:

    | Tipos de cuenta admitidos | Descripción |
    |-------------------------|-------------|
    | **Solo las cuentas de este directorio organizativo** | Seleccione esta opción si va a crear una aplicación de línea de negocio (LOB). Esta opción no está disponible si no va a registrar la aplicación en un directorio.<br><br>Esta opción se asigna solo a un único inquilino de Azure AD.<br><br>Esta es la opción predeterminada, a menos que registre la aplicación fuera de un directorio. En los casos en los que la aplicación se registra fuera de un directorio, el valor predeterminado son las cuentas personales de Microsoft y cuentas multiinquilino de Azure AD. |
    | **Cuentas en cualquier directorio organizativo** | Seleccione esta opción si desea tener como destino todos los clientes de negocios y del sector educativo.<br><br>Esta opción se asigna solo a un multiinquilino de Azure AD.<br><br>Si ha registrado la aplicación como solo para un único inquilino de Azure AD, puede actualizarla para que sea de multiinquilino de Azure AD y que vuelva a serlo para un solo inquilino mediante la página **Autenticación**. |
    | **Cuentas en cualquier directorio organizativo y cuentas Microsoft personales** | Seleccione esta opción para establecer como destino el mayor conjunto posible de clientes.<br><br>Esta opción asigna cuentas personales de Microsoft y cuentas multiinquilinos de Azure AD.<br><br>Si registró la aplicación como cuentas multiinquilino de Azure AD y cuentas personales de Microsoft, no puede cambiar esta opción en la interfaz de usuario. En su lugar, debe usar el editor de manifiestos de aplicación para cambiar los tipos de cuenta admitidos. |

1. En **URI de redirección (opcional)** , seleccione el tipo de aplicación que va a crear: **Web** o **Cliente público (dispositivos móviles y escritorio)** . A continuación, escriba el identificador URI de redireccionamiento o la dirección URL de respuesta de la aplicación.

    * Para aplicaciones web, proporcione la dirección URL base de la aplicación. Por ejemplo, `https://localhost:31544` podría ser la dirección URL de una aplicación web que se ejecuta en la máquina local. Los usuarios utilizan esta dirección URL para iniciar sesión en una aplicación cliente web.
    * Para aplicaciones cliente públicas, proporcione el identificador URI que utiliza Azure AD para devolver las respuestas de los tokens. Escriba un valor específico para la aplicación, como `myapp://auth`.

1. Cuando termine, seleccione **Registrar**.

    ![Muestra la pantalla para registrar una aplicación nueva en Azure Portal.](../media/new-app-registration.png)

Azure AD asigna un identificador de aplicación o cliente único a la aplicación. El portal abre la página de **información general** de la aplicación. Anote el valor de **Id. de aplicación (cliente)** .

Para agregar funcionalidades a la aplicación, puede seleccionar otras opciones de configuración, como personalización de marca, certificados y secretos, permisos de API y mucho más.

![Ejemplo de una página de introducción de la aplicación recién registrada](../media//new-app-overview-page-expanded.png)

Antes de establecer llamadas desde la aplicación al almacén de claves, tendrá que indicarle al almacén de claves la aplicación y sus permisos. En comando siguiente usa el nombre del almacén y el **Id. de aplicación (cliente)** de la aplicación de Azure Active Directory para conceder a la aplicación acceso **Obtener** al almacén de claves.

```powershell
Set-AzKeyVaultAccessPolicy -VaultName <vaultName> -ServicePrincipalName <clientIDfromAzureAD> -PermissionsToSecrets Get
```

Ya está listo para empezar a crear las llamadas a la aplicación. En la aplicación, tendrá que instalar los paquetes NuGet necesarios para interactuar con Azure Key Vault y Azure Active Directory. En la Consola del Administrador de paquetes de Visual Studio, escriba los siguientes comandos. Al redactar este artículo, la versión más reciente del paquete de Azure Active Directory era la 3.10.305231913, por lo que tendrá que confirmar que se trata de la última versión y actualizarla si es necesario.

```powershell
Install-Package Microsoft.IdentityModel.Clients.ActiveDirectory -Version 3.10.305231913

Install-Package Microsoft.Azure.KeyVault
```

En el código de la aplicación, cree una clase que contenga el método de autenticación de Azure Active Directory. En este ejemplo, la clase se llama **Utils**. Agregue la siguiente instrucción `using` :

```csharp
using Microsoft.IdentityModel.Clients.ActiveDirectory;
```

A continuación, agregue el método siguiente para recuperar el token JWT de Azure Active Directory. Para facilitar el mantenimiento, es posible que quiera trasladar los valores de cadena codificados de forma rígida a la configuración del sitio web o la aplicación.

```csharp
public async static Task<string> GetToken(string authority, string resource, string scope)
{
    var authContext = new AuthenticationContext(authority);

    ClientCredential clientCred = new ClientCredential("<AzureADApplicationClientID>","<AzureADApplicationClientKey>");

    AuthenticationResult result = await authContext.AcquireTokenAsync(resource, clientCred);

    if (result == null)

    throw new InvalidOperationException("Failed to obtain the JWT token");

    return result.AccessToken;
}
```

Agregue el código necesario para llamar a Key Vault y recuperar el valor del secreto. Lo primero que debe hacer es agregar la siguiente instrucción `using`:

```csharp
using Microsoft.Azure.KeyVault;
```

Agregue las llamadas de método para invocar a Key Vault y recuperar el secreto. En este método, proporcione el URI del secreto que guardó en un paso anterior. Observe el uso del método **GetToken** de la clase **Utils** que ha creado antes.

```csharp
var kv = new KeyVaultClient(new KeyVaultClient.AuthenticationCallback(Utils.GetToken));

var sec = kv.GetSecretAsync(<SecretID>).Result.Value;
```

Cuando ejecute la aplicación, debería estar autenticado en Azure Active Directory y recuperar el valor del secreto de Azure Key Vault.

## <a name="key-rotation-using-azure-automation"></a>Rotación de claves mediante Azure Automation

> [!IMPORTANT]
> En los runbooks de Azure Automation sigue siendo necesario usar el módulo `AzureRM`.

Ya está listo para configurar una estrategia de rotación para los valores almacenados como secretos de Key Vault. Los secretos se pueden rotar de varias maneras:

- Como parte de un proceso manual.
- Mediante programación usando llamadas API.
- A través de un script de Azure Automation.

Para los objetivos de este artículo, se usará PowerShell junto a Azure Automation para cambiar la clave de acceso de una cuenta de Azure Storage. Después, actualizará un secreto del almacén de claves con esa clave nueva.

Para que Azure Automation pueda establecer valores de secreto en el almacén de claves, tendrá que obtener el identificador de cliente de la conexión **AzureRunAsConnection**. Esta conexión se ha creado al establecer la instancia de Azure Automation. Para encontrar este identificador, seleccione **Recursos** en la instancia de Azure Automation. Desde ahí, seleccione **Conexiones** y después la entidad de servicio **AzureRunAsConnection**. Anote el valor **ApplicationId**.

![Identificador de cliente de Azure Automation](../media/keyvault-keyrotation/Azure_Automation_ClientID.png)

En **Recursos**, seleccione **Módulos**. Seleccione **Galería** y, después, busque e importe las versiones actualizadas de cada uno de los módulos siguientes:

    Azure
    Azure.Storage
    AzureRM.Profile
    AzureRM.KeyVault
    AzureRM.Automation
    AzureRM.Storage

> [!NOTE]
> Cuando se redactó este artículo, los módulos que aparecen arriba eran los únicos que debían actualizarse para el script que se muestra a continuación. Si se produce un error en el trabajo de automatización, confirme que ha importado todos los módulos necesarios y sus dependencias.

Una vez que haya recuperado el identificador de aplicación de la conexión de Azure Automation, debe notificar al almacén de claves que esta aplicación tiene permiso para actualizar los secretos del almacén. Use el siguiente comando de PowerShell:

```powershell
Set-AzKeyVaultAccessPolicy -VaultName <vaultName> -ServicePrincipalName <applicationIDfromAzureAutomation> -PermissionsToSecrets Set
```

A continuación, seleccione **Runbooks** en la instancia de Azure Automation y, después, seleccione **Agregar Runbook**. Seleccione **Creación rápida**. Asigne un nombre al runbook y seleccione **PowerShell** como tipo del runbook. Puede agregar una descripción. Por último, seleccione **Crear**.

![Creación de runbook](../media/keyvault-keyrotation/Create_Runbook.png)

En el panel del editor del nuevo runbook, pegue el siguiente script de PowerShell:

```powershell
$connectionName = "AzureRunAsConnection"
try
{
    # Get the connection "AzureRunAsConnection"
    $servicePrincipalConnection=Get-AutomationConnection -Name $connectionName         

    "Logging in to Azure..."
    Connect-AzureRmAccount `
        -ServicePrincipal `
        -TenantId $servicePrincipalConnection.TenantId `
        -ApplicationId $servicePrincipalConnection.ApplicationId `
        -CertificateThumbprint $servicePrincipalConnection.CertificateThumbprint
    "Login complete."
}
catch {
    if (!$servicePrincipalConnection)
    {
        $ErrorMessage = "Connection $connectionName not found."
        throw $ErrorMessage
    } else{
        Write-Error -Message $_.Exception
        throw $_.Exception
    }
}

# Optionally you can set the following as parameters
$StorageAccountName = <storageAccountName>
$RGName = <storageAccountResourceGroupName>
$VaultName = <keyVaultName>
$SecretName = <keyVaultSecretName>

#Key name. For example key1 or key2 for the storage account
New-AzureRmStorageAccountKey -ResourceGroupName $RGName -Name $StorageAccountName -KeyName "key2" -Verbose
$SAKeys = Get-AzureRmStorageAccountKey -ResourceGroupName $RGName -Name $StorageAccountName

$secretvalue = ConvertTo-SecureString $SAKeys[1].Value -AsPlainText -Force

$secret = Set-AzureKeyVaultSecret -VaultName $VaultName -Name $SecretName -SecretValue $secretvalue
```

En el panel del editor, seleccione **Panel de prueba** para probar el script. Una vez que el script se ejecute sin errores, puede seleccionar **Publicar** y después aplicar una programación para el runbook desde el panel de configuración de runbooks.

## <a name="key-vault-auditing-pipeline"></a>Canalización de auditorías de Key Vault

Cuando configure un almacén de claves, puede habilitar la auditoría para que se recopilen registros de las solicitudes de acceso a dicho almacén. Estos registros se almacenan en una cuenta de Azure Storage designada y se pueden extraer, supervisar y analizar. En el escenario siguiente se usan funciones de Azure, aplicaciones lógicas de Azure y registros de auditoría del almacén de claves para crear una canalización que envía un correo electrónico cuando una aplicación cuyo identificador no coincide con el de la aplicación web recupera secretos del almacén.

En primer lugar, debe habilitar los registros en el almacén de claves. Use los siguientes comandos de PowerShell. (Puede ver los detalles completos en [este artículo sobre el registro del almacén de claves](../general/logging.md)).

```powershell
$sa = New-AzStorageAccount -ResourceGroupName <resourceGroupName> -Name <storageAccountName> -Type Standard\_LRS -Location 'East US'
$kv = Get-AzKeyVault -VaultName '<vaultName>'
Set-AzDiagnosticSetting -ResourceId $kv.ResourceId -StorageAccountId $sa.Id -Enabled $true -Category AuditEvent
```

Después de habilitar el registro, se empiezan a almacenar registros de auditoría en la cuenta de almacenamiento designada. Estos registros incluirán eventos acerca de qué usuario, cómo y cuándo ha obtenido acceso a los almacenes de claves.

> [!NOTE]
> Puede acceder a la información de registro 10 minutos después de la operación del almacén de claves. A menudo estará disponible antes.

El siguiente paso consiste en la [creación de una cola de Azure Service Bus](../../service-bus-messaging/service-bus-dotnet-get-started-with-queues.md). En esta cola se insertan los registros de auditoría del almacén de claves. Cuando los mensajes del registro de auditoría se encuentran en la cola, la aplicación lógica los recopila y actúa sobre ellos. Siga estos pasos para crear una instancia de Service Bus:

1. Cree un espacio de nombres de Service Bus (si ya tiene uno que quiera usar, puede saltar al paso 2).
2. Busque la instancia de Service Bus en Azure Portal y seleccione el espacio de nombres en el que quiera crear la cola.
3. Seleccione **Crear un recurso** > **Integración empresarial** > **Service Bus** y después escriba los detalles necesarios.
4. Para buscar la información de conexión de Service Bus, seleccione el espacio de nombres y después **Información de conexión**. Necesitará esta información para la sección siguiente.

A continuación, [cree una función de Azure](../../azure-functions/functions-create-first-azure-function.md) para sondear los registros del almacén de claves incluidos en la cuenta de almacenamiento y recopilar nuevos eventos. Esta función se desencadenará según una programación.

Para crear una función de Azure, seleccione **Crear un recurso**, busque **Function App** en Marketplace y después seleccione **Crear**. Durante el proceso de creación, puede usar un plan de hospedaje existente o crear uno nuevo. También puede optar por el hospedaje dinámico. Para más información sobre las opciones de hospedaje para Azure Functions, vea [Escalado de Azure Functions](../../azure-functions/functions-scale.md).

Una vez creada la aplicación de función de Azure, vaya hasta ella y seleccione el escenario **Temporizador** y **C\#** para el lenguaje. Después, seleccione **Crear esta función**.

![Hoja de inicio de Azure Functions](../media/keyvault-keyrotation/Azure_Functions_Start.png)

En la pestaña **Desarrollar**, sustituya el código de run.csx por el siguiente:

```csharp
#r "Newtonsoft.Json"

using System;
using Microsoft.WindowsAzure.Storage;
using Microsoft.WindowsAzure.Storage.Auth;
using Microsoft.WindowsAzure.Storage.Blob;
using Microsoft.ServiceBus.Messaging;
using System.Text;

public static void Run(TimerInfo myTimer, TextReader inputBlob, TextWriter outputBlob, TraceWriter log)
{
    log.Info("Starting");

    CloudStorageAccount sourceStorageAccount = new CloudStorageAccount(new StorageCredentials("<STORAGE_ACCOUNT_NAME>", "<STORAGE_ACCOUNT_KEY>"), true);

    CloudBlobClient sourceCloudBlobClient = sourceStorageAccount.CreateCloudBlobClient();

    var connectionString = "<SERVICE_BUS_CONNECTION_STRING>";
    var queueName = "<SERVICE_BUS_QUEUE_NAME>";

    var sbClient = QueueClient.CreateFromConnectionString(connectionString, queueName);

    DateTime dtPrev = DateTime.UtcNow;
    if(inputBlob != null)
    {
        var txt = inputBlob.ReadToEnd();

        if(!string.IsNullOrEmpty(txt))
        {
            dtPrev = DateTime.Parse(txt);
            log.Verbose($"SyncPoint: {dtPrev.ToString("O")}");
        }
        else
        {
            dtPrev = DateTime.UtcNow;
            log.Verbose($"Sync point file didn't have a date. Setting to now.");
        }
    }

    var now = DateTime.UtcNow;

    string blobPrefix = "insights-logs-auditevent/resourceId=/SUBSCRIPTIONS/<SUBSCRIPTION_ID>/RESOURCEGROUPS/<RESOURCE_GROUP_NAME>/PROVIDERS/MICROSOFT.KEYVAULT/VAULTS/<KEY_VAULT_NAME>/y=" + now.Year +"/m="+now.Month.ToString("D2")+"/d="+ (now.Day).ToString("D2")+"/h="+(now.Hour).ToString("D2")+"/m=00/";

    log.Info($"Scanning:  {blobPrefix}");

    IEnumerable<IListBlobItem> blobs = sourceCloudBlobClient.ListBlobs(blobPrefix, true);

    log.Info($"found {blobs.Count()} blobs");

    foreach(var item in blobs)
    {
        if (item is CloudBlockBlob)
        {
            CloudBlockBlob blockBlob = (CloudBlockBlob)item;

            log.Info($"Syncing: {item.Uri}");

            string sharedAccessUri = GetContainerSasUri(blockBlob);

            CloudBlockBlob sourceBlob = new CloudBlockBlob(new Uri(sharedAccessUri));

            string text;
            using (var memoryStream = new MemoryStream())
            {
                sourceBlob.DownloadToStream(memoryStream);
                text = System.Text.Encoding.UTF8.GetString(memoryStream.ToArray());
            }

            dynamic dynJson = JsonConvert.DeserializeObject(text);

            //Required to order by time as they might not be in the file
            var results = ((IEnumerable<dynamic>) dynJson.records).OrderBy(p => p.time);

            foreach (var jsonItem in results)
            {
                DateTime dt = Convert.ToDateTime(jsonItem.time);

                if(dt>dtPrev){
                    log.Info($"{jsonItem.ToString()}");

                    var payloadStream = new MemoryStream(Encoding.UTF8.GetBytes(jsonItem.ToString()));
                    //When sending to ServiceBus, use the payloadStream and set keeporiginal to true
                    var message = new BrokeredMessage(payloadStream, true);
                    sbClient.Send(message);
                    dtPrev = dt;
                }
            }
        }
    }
    outputBlob.Write(dtPrev.ToString("o"));
}

static string GetContainerSasUri(CloudBlockBlob blob)
{
    SharedAccessBlobPolicy sasConstraints = new SharedAccessBlobPolicy();

    sasConstraints.SharedAccessStartTime = DateTime.UtcNow.AddMinutes(-5);
    sasConstraints.SharedAccessExpiryTime = DateTime.UtcNow.AddHours(24);
    sasConstraints.Permissions = SharedAccessBlobPermissions.Read;

    //Generate the shared access signature on the container, setting the constraints directly on the signature.
    string sasBlobToken = blob.GetSharedAccessSignature(sasConstraints);

    //Return the URI string for the container, including the SAS token.
    return blob.Uri + sasBlobToken;
}
```

> [!NOTE]
> Cambie las variables del código anterior para que apunten a la cuenta de almacenamiento en la que se escriben los registros del almacén de claves, a la instancia de Service Bus que ha creado antes y a la ruta de acceso específica de los registros de almacenamiento del almacén de claves.

La función selecciona el archivo de registro más reciente de la cuenta de almacenamiento en la que se escriben los registros del almacén de claves, recopila los últimos eventos de ese archivo y los inserta en una cola de Service Bus. 

Como un mismo archivo puede tener varios eventos, debe crear un archivo sync.txt que la función también consulte para determinar la marca de tiempo del último evento seleccionado. Al usar este archivo se asegura de que no se inserte varias veces el mismo evento. 

El archivo sync.txt contiene una marca de tiempo del último evento encontrado. Cuando se cargan los registros, se deben ordenar en función de sus marcas de tiempo para asegurarse de que están ordenados de la forma correcta.

En esta función, se hace referencia a un par de bibliotecas adicionales que no están disponibles de forma predeterminada en Azure Functions. Para incluir estas bibliotecas, es necesario que Azure Functions las extraiga mediante NuGet. En el cuadro **Código**, seleccione **Ver archivos**.

![Opción "Ver archivos"](../media/keyvault-keyrotation/Azure_Functions_ViewFiles.png)

Agregue un archivo denominado project.json con el contenido siguiente:

```json
    {
      "frameworks": {
        "net46":{
          "dependencies": {
                "WindowsAzure.Storage": "7.0.0",
                "WindowsAzure.ServiceBus":"3.2.2"
          }
        }
       }
    }
```

Después de seleccionar **Guardar**, Azure Functions descargará los binarios necesarios.

Cambie a la pestaña **Integrar** y asigne al parámetro del temporizador un nombre descriptivo para usarlo en la función. En el código anterior, la función espera que el nombre del temporizador sea *myTimer*. Especifique una [expresión CRON](../../app-service/webjobs-create.md#CreateScheduledCRON) para el temporizador de esta forma: `0 * * * * *`. Esta expresión hará que la función se ejecute una vez por minuto.

En la misma pestaña **Integrar**, agregue una entrada del tipo **Azure Blob Storage**. Esta entrada apuntará al archivo sync.txt que contiene la marca de tiempo del último evento que ha consultado la función. El acceso a esta entrada se realiza desde dentro de la función con el nombre de parámetro. En el código anterior, la entrada de Azure Blob Storage espera que el nombre de parámetro sea *inputBlob*. Seleccione la cuenta de almacenamiento donde residirá el archivo sync.txt (puede ser la misma u otra). En el campo de ruta de acceso, proporcione la ruta de acceso al archivo en el formato `{container-name}/path/to/sync.txt`.

Agregue una salida de tipo **Azure Blob Storage**. Esta salida apuntará al archivo sync.txt que ha definido en la entrada. La función usará esta salida para escribir la marca de tiempo del último evento que se ha consultado. En el código anterior, el nombre esperado para esta parámetro es *outputBlob*.

La función ya está lista. No olvide volver a la pestaña **Desarrollar** y guardar el código. Compruebe en la ventana de salida si hay errores de compilación y corríjalos según sea necesario. Si el código se compila, debería comprobar cada minuto los registros del almacén de claves e insertar los nuevos eventos en la cola de Service Bus definida. Cada vez que la función se desencadena, debería aparecer la información de los registros en la ventana del registro.

### <a name="azure-logic-app"></a>Aplicación lógica de Azure

Ahora, debe crear una aplicación lógica de Azure que seleccione los eventos que la función inserta en la cola de Service Bus, analice el contenido y envíe un correo electrónico si se cumple una determinada condición.

Para [crear una aplicación lógica](../../logic-apps/quickstart-create-first-logic-app-workflow.md), seleccione **Crear un recurso** > **Integración** > **Aplicación lógica**.

Después de crear la aplicación lógica, vaya hasta ella y seleccione **Editar**. En el editor de la aplicación lógica, seleccione **Cola de Service Bus** y escriba las credenciales de Service Bus para conectarla a la cola.

![Service Bus de la aplicación lógica de Azure](../media/keyvault-keyrotation/Azure_LogicApp_ServiceBus.png)

Seleccione **Agregar una condición**. En la condición, cambie al editor avanzado y escriba el código siguiente. Reemplace *APP_ID* por el identificador real de la aplicación web:

```
@equals('<APP_ID>', json(decodeBase64(triggerBody()['ContentData']))['identity']['claim']['appid'])
```

Básicamente, esta expresión devuelve **false** si el valor *appid* del evento entrante (que es el cuerpo del mensaje de Service Bus) no es el valor *appid* de la aplicación.

Ahora, cree una acción en **IF NO, DO NOTHING** (SI NO, NO HACER NADA).

![Selección de una acción de Azure Logic Apps](../media/keyvault-keyrotation/Azure_LogicApp_Condition.png)

Para la acción, seleccione **Office 365 - send email** (Office 365 - enviar correo electrónico). Rellene los campos para crear el correo electrónico que se enviará cuando la condición definida devuelva **false**. Si no tiene Office 365, busque otras alternativas para conseguir los mismos resultados.

Ahora tiene una canalización de un extremo a otro que busca nuevos registros de auditoría del almacén de claves una vez por minuto. Inserta los registros nuevos que encuentra en una cola de Service Bus. La aplicación lógica se activa cuando un nuevo mensaje llega a la cola. Si el valor *appid* dentro del evento no coincide con el identificador de la aplicación que realiza la llamada, envía un correo electrónico.
