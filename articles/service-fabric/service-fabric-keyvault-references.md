---
title: 'Azure Service Fabric: uso de las referencias de KeyVault de la aplicación de Service Fabric'
description: En este artículo se explica cómo usar la compatibilidad con KeyVaultReference de Service Fabrica para los secretos de aplicación.
ms.topic: article
ms.date: 09/20/2019
ms.openlocfilehash: f2221bb3e8e3ee3181b2cff70107dccc203954cf
ms.sourcegitcommit: ce8eecb3e966c08ae368fafb69eaeb00e76da57e
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/21/2020
ms.locfileid: "92313792"
---
# <a name="keyvaultreference-support-for-service-fabric-applications-preview"></a>Compatibilidad de KeyVaultReference para aplicaciones de Service Fabric (versión preliminar)

Un desafío común al compilar aplicaciones en la nube es cómo almacenar de manera segura los secretos que requiere la aplicación. Por ejemplo, es posible que quiera almacenar las credenciales del repositorio del contenedor en keyvault y hacer referencia a él en el manifiesto de aplicación. KeyVaultReference de Service Fabric usa la identidad administrada de Service Fabric y facilitar la referencia a los secretos de keyvault. En el resto de este artículo se detalla cómo usar KeyVaultReference de Service Fabric e incluye cierto uso típico.

> [!IMPORTANT]
> No se recomienda utilizar esta característica en versión preliminar en entorno de producción.

> [!NOTE]
> La característica en versión preliminar de referencia de KeyVault solo admite secretos [con versión](../key-vault/general/about-keys-secrets-certificates.md#objects-identifiers-and-versioning). No se admiten secretos sin versión.

## <a name="prerequisites"></a>Requisitos previos

- Identidad administrada para la aplicación (MIT)
    
    La compatibilidad de KeyVaultReference de Service Fabric usa Identidad administrada de la aplicación y, por lo tanto, las aplicaciones que planean usar KeyVaultReferences deberían usar Identidad administrada. Siga este [documento](concepts-managed-identity.md) para habilitar la identidad administrada para la aplicación.

- Almacén central de secretos (CSS).

    El Almacén central de secretos (CSS) es la caché de secretos local cifrada de Service Fabric. CSS es una caché local de almacén de secretos que conserva datos confidenciales, como contraseñas, tokens y claves, cifrados en memoria. Los secretos de KeyVaultReference, una vez capturados, se almacenan en la memoria caché en CSS.

    Agregue lo siguiente a la configuración del clúster en `fabricSettings` para habilitar todas las características necesarias para la compatibilidad con KeyVaultReference.

    ```json
    "fabricSettings": 
    [
        ...
    {
                "name":  "CentralSecretService",
                "parameters":  [
                {
                    "name":  "IsEnabled",
                    "value":  "true"
                },
                {
                    "name":  "MinReplicaSetSize",
                    "value":  "3"
                },
                {
                    "name":  "TargetReplicaSetSize",
                    "value":  "3"
                }
                ]
            },
            {
                "name":  "ManagedIdentityTokenService",
                "parameters":  [
                {
                    "name":  "IsEnabled",
                    "value":  "true"
                }
                ]
            }
            ]
    ```

    > [!NOTE] 
    > Se recomienda usar un certificado de cifrado independiente para CSS. Puede agregarlo en la sección "CentralSecretService".
    

    ```json
        {
            "name": "EncryptionCertificateThumbprint",
            "value": "<EncryptionCertificateThumbprint for CSS>"
        }
    ```
Para que los cambios surtan efecto, también tendrá que cambiar la directiva de actualización para especificar un reinicio forzado del runtime de Service Fabric en cada nodo a medida que la actualización avanza a través del clúster. Con este reinicio se garantiza que el servicio de sistema recién habilitado se inicia y se ejecuta en cada uno de los nodos. En el siguiente fragmento de código, forceRestart es la opción esencial; use los valores existentes para el resto de la configuración.
```json
"upgradeDescription": {
    "forceRestart": true,
    "healthCheckRetryTimeout": "00:45:00",
    "healthCheckStableDuration": "00:05:00",
    "healthCheckWaitDuration": "00:05:00",
    "upgradeDomainTimeout": "02:00:00",
    "upgradeReplicaSetCheckTimeout": "1.00:00:00",
    "upgradeTimeout": "12:00:00"
}
```
- Conceda permiso de acceso a keyvault a la identidad administrada de la aplicación

    Consulte este [documento](how-to-grant-access-other-resources.md) para ver cómo conceder acceso a keyvault a la identidad administrada. Además, tenga en cuenta que si usa una identidad administrada asignada por el sistema, la identidad administrada solo se crea después de la implementación de la aplicación.

## <a name="keyvault-secret-as-application-parameter"></a>Secreto de keyvault como parámetro de aplicación
Supongamos que la aplicación necesita leer la contraseña de la base de datos de back-end almacenada en keyvault: la compatibilidad de KeyVaultReference de Service Fabric lo hace fácil. En el ejemplo siguiente, se lee el secreto `DBPassword` desde keyvault mediante la compatibilidad de KeyVaultReference de Service Fabric.

- Incorporación de una sección a settings.xml

    Defina el parámetro `DBPassword` con el tipo `KeyVaultReference` y el valor `<KeyVaultURL>`.

    ```xml
    <Section Name="dbsecrets">
        <Parameter Name="DBPassword" Type="KeyVaultReference" Value="https://vault200.vault.azure.net/secrets/dbpassword/8ec042bbe0ea4356b9b171588a8a1f32"/>
    </Section>
    ```
- Consulte la sección nueva en ApplicationManifest.xml en `<ConfigPackagePolicies>`.

    ```xml
    <ServiceManifestImport>
        <Policies>
        <IdentityBindingPolicy ServiceIdentityRef="WebAdmin" ApplicationIdentityRef="ttkappuser" />
        <ConfigPackagePolicies CodePackageRef="Code">
            <!--Linux container example-->
            <ConfigPackage Name="Config" SectionName="dbsecrets" EnvironmentVariableName="SecretPath" MountPoint="/var/secrets"/>
            <!--Windows container example-->
            <!-- <ConfigPackage Name="Config" SectionName="dbsecrets" EnvironmentVariableName="SecretPath" MountPoint="C:\secrets"/> -->
        </ConfigPackagePolicies>
        </Policies>
    </ServiceManifestImport>
    ```

- Uso de KeyVaultReference en la aplicación

    Service Fabric en la creación de instancias del servicio resolverá el parámetro KeyVaultReference con la identidad administrada de la aplicación. Cada parámetro incluido en `<Section  Name=dbsecrets>` será un archivo en la carpeta a la que apunta EnvironmentVariable SecretPath. A continuación, el fragmento de código de C# muestra cómo leer DBPassword en la aplicación.

    ```C#
    string secretPath = Environment.GetEnvironmentVariable("SecretPath");
    using (StreamReader sr = new StreamReader(Path.Combine(secretPath, "DBPassword"))) 
    {
        string dbPassword =  sr.ReadToEnd();
        // dbPassword to connect to DB
    }
    ```
    > [!NOTE] 
    > En el escenario de contenedor, puede usar MountPoint para controlar dónde se montará `secrets`.

## <a name="keyvault-secret-as-environment-variable"></a>Secreto de Keyvault como variable de entorno

Las variables de entorno de Service Fabric ahora admiten el tipo KeyVaultReference. A continuación, el ejemplo muestra cómo enlazar una variable de entorno a un secreto almacenado en KeyVault.

```xml
<EnvironmentVariables>
      <EnvironmentVariable Name="EventStorePassword" Type="KeyVaultReference" Value="https://ttkvault.vault.azure.net/secrets/clustercert/e225bd97e203430d809740b47736b9b8"/>
</EnvironmentVariables>
```

```C#
string eventStorePassword =  Environment.GetEnvironmentVariable("EventStorePassword");
```
## <a name="keyvault-secret-as-container-repository-password"></a>Secreto de Keyvault como contraseña del repositorio de contenedor
KeyVaultReference es un tipo compatible con el contenedor RepositoryCredentials. A continuación, el ejemplo muestra cómo usar una referencia de keyvault como contraseña del repositorio de contenedor.
```xml
 <Policies>
      <ContainerHostPolicies CodePackageRef="Code">
        <RepositoryCredentials AccountName="user1" Type="KeyVaultReference" Password="https://ttkvault.vault.azure.net/secrets/containerpwd/e225bd97e203430d809740b47736b9b8"/>
      </ContainerHostPolicies>
```
## <a name="faq"></a>Preguntas más frecuentes
- La identidad administrada debe estar habilitada para la compatibilidad con KeyVaultReference. Se producirá un error en la activación de la aplicación si se usa KeyVaultReference sin habilitar la identidad administrada.

- Si usa una identidad asignada por el sistema, se crea solo después de implementar la aplicación y esto crea una dependencia circular. Una vez implementada la aplicación, puede conceder el permiso de acceso a la identidad asignada al sistema a keyvault. Puede encontrar la identidad asignada por el sistema por nombre {clúster}/{nombre de aplicación}/{nombre de servicio}

- El keyvault debe estar en la misma suscripción que el clúster de Service Fabric. 

## <a name="next-steps"></a>Pasos siguientes

* [Documentación de Azure KeyVault](../key-vault/index.yml)