---
title: Procedimiento para usar una identidad administrad asignada por el sistema para acceder a datos de Azure Cosmos DB
description: Aprenda a configurar una identidad administrada de Azure AD asignada por el sistema para acceder a claves desde Azure Cosmos DB. msi, identidad de servicio administrado, aad, azure active directory, identidad
author: j-patrick
ms.service: cosmos-db
ms.topic: conceptual
ms.date: 03/20/2020
ms.author: justipat
ms.reviewer: sngun
ms.openlocfilehash: 102efba5d028eef621f392ef1739ea9ebeca0b44
ms.sourcegitcommit: 27bbda320225c2c2a43ac370b604432679a6a7c0
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/31/2020
ms.locfileid: "80416977"
---
# <a name="how-to-use-a-system-assigned-managed-identity-to-access-azure-cosmos-db-data"></a>Procedimiento para usar una identidad administrad asignada por el sistema para acceder a datos de Azure Cosmos DB

En este artículo, configurará una solución **sólida e independiente de la rotación de claves** para acceder a las claves de Azure Cosmos DB mediante [identidades administradas](../active-directory/managed-identities-azure-resources/services-support-managed-identities.md). En el ejemplo de este artículo se usa una función de Azure. Pero puede lograr esta solución mediante cualquier servicio que admita identidades administradas. 

Aprenderá a crear una función de Azure que puede acceder a Azure Cosmos DB sin necesidad de copiar ninguna clave de Azure Cosmos DB. La función se reactivará cada minuto y registrará la temperatura actual de un acuario. Para obtener información sobre cómo configurar una función de Azure desencadenada por un temporizador, vea el artículo [Creación de una función en Azure que se desencadena mediante un temporizador](../azure-functions/functions-create-scheduled-function.md).

Para simplificar el escenario, la limpieza de los documentos de temperatura más antiguos se controla mediante una opción [Período de vida](./time-to-live.md) ya configurada. 

## <a name="assign-a-system-assigned-managed-identity-to-an-azure-function"></a>Asignación de una identidad administrada asignada por el sistema a una función de Azure

En este paso, asignará una identidad administrada asignada por el sistema a la función de Azure.

1. En [Azure Portal](https://portal.azure.com/), abra el panel **Función de Azure** y vaya a la aplicación de funciones. 

1. Abra la pestaña **Características de la plataforma** > **Identidad**: 

   ![Pestaña Identidad](./media/managed-identity-based-authentication/identity-tab-selection.png)

1. En la pestaña **Identidad**, **active** el estado **Identidad del sistema**. Asegúrese de seleccionar **Guardar** y confirme que quiere activar la identidad del sistema. Al final, el panel **Identidad del sistema** debe tener el este aspecto:  

   ![Identidad del sistema activada](./media/managed-identity-based-authentication/identity-tab-system-managed-on.png)

## <a name="grant-the-managed-identity-access-to-your-azure-cosmos-account"></a>Concesión de acceso a la cuenta de Azure Cosmos para la identidad administrada

En este paso, asignará un rol a la identidad administrada asignada por el sistema de la función de Azure. Azure Cosmos DB tiene varios roles integrados que se pueden asignar a la identidad administrada. Para esta solución, usará los dos roles siguientes:

|Rol integrado  |Descripción  |
|---------|---------|
|[Colaborador de cuenta de DocumentDB](../role-based-access-control/built-in-roles.md#documentdb-account-contributor)|Puede administrar cuentas de Azure Cosmos DB. Permite la recuperación de claves de lectura/escritura. |
|[Lector de cuentas de Cosmos DB](../role-based-access-control/built-in-roles.md#cosmos-db-account-reader-role)|Puede leer los datos de cuentas de Azure Cosmos DB. Permite la recuperación de claves de lectura. |

> [!IMPORTANT]
> La compatibilidad con RBAC en Azure Cosmos DB solo se aplica a las operaciones del plano de control. Las operaciones del plano de datos se protegen mediante las claves maestras o tokens de recursos. Para más información, vea el artículo [Protección del acceso a los datos](secure-access-to-data.md).

> [!TIP] 
> Al asignar roles, asigne solo el acceso necesario. Si el servicio solo requiere leer datos, asigne la identidad administrada al rol **Lector de cuentas de Cosmos DB**. Para obtener más información sobre la importancia del acceso con privilegios mínimos, vea el artículo [Menor exposición de las cuentas con privilegios](../security/fundamentals/identity-management-best-practices.md#lower-exposure-of-privileged-accounts).

En este escenario, leerá la temperatura y, luego, reescribirá esos datos en un contenedor de Azure Cosmos DB. Como tiene que escribir los datos, usará el rol **Colaborador de cuenta de DocumentDB**. 

1. Inicie sesión en Azure Portal y vaya a la cuenta de Azure Cosmos DB. Abra el **panel Administración de identidad y acceso (IAM)** y, después, la pestaña **Asignaciones de roles**:

   ![Panel de IAM](./media/managed-identity-based-authentication/cosmos-db-iam-tab.png)

1. Seleccione el botón **+ Agregar** y, después, **Agregar asignación de roles**.

1. El panel **Agregar asignación de roles** se abre a la derecha:

   ![Adición del rol](./media/managed-identity-based-authentication/cosmos-db-iam-tab-add-role-pane.png)

   * **Rol**: seleccione **Colaborador de cuenta de DocumentDB**.
   * **Asignar acceso a**: en la subsección Seleccionar **Identidad administrada asignada por el sistema**, seleccione **Aplicación de funciones**.
   * **Seleccionar**: el panel se rellenará con todas las aplicaciones de funciones de la suscripción que tengan una **Identidad del sistema administrada**. En este caso, seleccione la aplicación de funciones **SummaryService**: 

      ![Selección de la asignación](./media/managed-identity-based-authentication/cosmos-db-iam-tab-add-role-pane-filled.png)

1. Después de seleccionar la identidad de la aplicación de funciones, haga clic en **Guardar**.

## <a name="programmatically-access-the-azure-cosmos-db-keys-from-the-azure-function"></a>Acceso mediante programación a las claves de Azure Cosmos DB desde la función de Azure

Ahora, tiene una aplicación de funciones con una identidad administrada asignada por el sistema. A esa identidad se le ha asignado el rol **Colaborador de cuenta de DocumentDB** en los permisos de Azure Cosmos DB. El código de la aplicación de funciones siguiente obtendrá las claves de Azure Cosmos DB, creará un objeto CosmosClient, obtendrá la temperatura y, después, la guardará en Cosmos DB.

En este ejemplo se usa la [API List Keys](https://docs.microsoft.com/rest/api/cosmos-db-resource-provider/DatabaseAccounts/ListKeys) para acceder a las claves de la cuenta de Azure Cosmos DB.

> [!IMPORTANT] 
> Si quiere [asignar el rol **Lector de cuentas de Cosmos DB**](#grant-the-managed-identity-access-to-your-azure-cosmos-account), tendrá que usar la [API List Keys](https://docs.microsoft.com/rest/api/cosmos-db-resource-provider/DatabaseAccounts/ListReadOnlyKeys) de solo lectura. Esto solamente rellenará las claves de solo lectura.

La API List Keys devuelve el objeto `DatabaseAccountListKeysResult`. Este tipo no está definido en las bibliotecas de C#. En el código siguiente se muestra la implementación de esta clase:  

```csharp 
namespace SummarizationService 
{
  public class DatabaseAccountListKeysResult
  {
      public string primaryMasterKey {get;set;}
      public string primaryReadonlyMasterKey {get; set;}
      public string secondaryMasterKey {get; set;}
      public string secondaryReadonlyMasterKey {get;set;}
  }
}
```

En el ejemplo también se usa un documento simple denominado "TemperatureRecord", que se define de esta forma:

```csharp
using System;

namespace Monitor
{
    public class TemperatureRecord
    {
        public string id { get; set; } = Guid.NewGuid().ToString();
        public DateTime RecordTime { get; set; }
        public int Temperature { get; set; }

    }
}
```

Usará la biblioteca [Microsoft.Azure.Services.AppAuthentication](https://www.nuget.org/packages/Microsoft.Azure.Services.AppAuthentication) para obtener el token de identidad administrada asignada por el sistema. Para aprender otras formas de conseguir el token y obtener más información sobre de la biblioteca `Microsoft.Azure.Service.AppAuthentication`, vea el artículo [Autenticación entre servicios](../key-vault/service-to-service-authentication.md).

```csharp
using System;
using System.Net.Http;
using System.Net.Http.Headers;
using System.Threading.Tasks;
using Microsoft.Azure.Cosmos;
using Microsoft.Azure.Services.AppAuthentication;
using Microsoft.Azure.WebJobs;
using Microsoft.Extensions.Logging;

namespace Monitor
{
    public static class TemperatureMonitor
    {
        private static string subscriptionId =
        "<azure subscription id>";
        private static string resourceGroupName =
        "<name of your azure resource group>";
        private static string accountName =
        "<Azure Cosmos DB account name>";
        private static string cosmosDbEndpoint =
        "<Azure Cosmos DB endpoint>";
        private static string databaseName =
        "<Azure Cosmos DB name>";
        private static string containerName =
        "<container to store the temperature in>";

        [FunctionName("TemperatureMonitor")]
        public static async Task Run([TimerTrigger("0 * * * * *")]TimerInfo myTimer, ILogger log)
        {
            log.LogInformation($"Starting temperature monitoring: {DateTime.Now}");

            // AzureServiceTokenProvider will help us to get the Service Managed token.
            var azureServiceTokenProvider = new AzureServiceTokenProvider();

            // In order to get the Service Managed token we need to authenticate to the Azure Resource Manager.
            string accessToken = await azureServiceTokenProvider.GetAccessTokenAsync("https://management.azure.com/");

            // To get the Azure Cosmos DB keys setup the List Keys API:
            string endpoint = $"https://management.azure.com/subscriptions/{subscriptionId}/resourceGroups/{resourceGroupName}/providers/Microsoft.DocumentDB/databaseAccounts/{accountName}/listKeys?api-version=2019-12-12";

            // setup an HTTP Client and add the access token.
            HttpClient httpClient = new HttpClient();
            httpClient.DefaultRequestHeaders.Authorization = new AuthenticationHeaderValue("Bearer", accessToken);

            // Post to the endpoint to get the keys result.
            var result = await httpClient.PostAsync(endpoint, new StringContent(""));

            // Get the Result back as a DatabaseAccountListKeysResult.
            DatabaseAccountListKeysResult keys = await result.Content.ReadAsAsync<DatabaseAccountListKeysResult>();

            log.LogInformation("Starting to create the client");

            CosmosClient client = new CosmosClient(cosmosDbEndpoint, keys.primaryMasterKey);

            log.LogInformation("Client created");

            var database = client.GetDatabase(databaseName);
            var container = database.GetContainer(containerName);

            log.LogInformation("Get the temperature.");

            var tempRecord = new TemperatureRecord() { RecordTime = DateTime.UtcNow, Temperature = GetTemperature() };

            log.LogInformation("Store temperature");

            await container.CreateItemAsync<TemperatureRecord>(tempRecord);

            log.LogInformation($"Ending temperature monitor: {DateTime.Now}");
        }

        private static int GetTemperature()
        {
            // fake the temperature sensor for this demo
            Random r = new Random(DateTime.UtcNow.Second);
            return r.Next(0, 120);
        }
    }
}
```

Ya está preparado para [implementar la función de Azure](../azure-functions/functions-create-first-function-vs-code.md).

## <a name="next-steps"></a>Pasos siguientes

* [Autenticación basada en certificados con Azure Active Directory y Azure Cosmos DB](certificate-based-authentication.md)
* [Protección de las claves de Azure Cosmos con Azure Key Vault](access-secrets-from-keyvault.md)
* [Línea de base de seguridad para Azure Cosmos DB](security-baseline.md)
