---
title: Uso de referencias de Key Vault
description: Aprenda a configurar Azure App Service y Azure Functions para que usen referencias de Azure Key Vault. Haga que los secretos de Key Vault estén disponibles para el código de aplicación.
author: mattchenderson
ms.topic: article
ms.date: 10/09/2019
ms.author: mahender
ms.custom: seodec18
ms.openlocfilehash: dd0a03ea76d517486bb9bda6d9628fb529166dd8
ms.sourcegitcommit: b55d7c87dc645d8e5eb1e8f05f5afa38d7574846
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 04/16/2020
ms.locfileid: "81453734"
---
# <a name="use-key-vault-references-for-app-service-and-azure-functions"></a>Uso de referencias de Key Vault para App Service y Azure Functions

En este tema se muestra cómo trabajar con secretos de Azure Key Vault en la aplicación de App Service o Azure Functions sin necesidad de realizar cambios en el código. [Azure Key Vault](../key-vault/general/overview.md) es un servicio que proporciona administración centralizada de los secretos, con control total sobre las directivas de acceso y el historial de auditoría.

## <a name="granting-your-app-access-to-key-vault"></a>Concesión a la aplicación de acceso a Key Vault

Para leer secretos desde Key Vault, debe tener creado un almacén y proporcionar a la aplicación permiso para acceder a él.

1. Para crear un almacén de claves, siga la [guía de inicio rápido de Key Vault](../key-vault/secrets/quick-create-cli.md).

1. Cree una [identidad administrada asignada por el sistema](overview-managed-identity.md) para la aplicación.

   > [!NOTE] 
   > Las referencias de Key Vault solo admiten actualmente identidades administradas asignadas por el sistema. No se pueden usar identidades asignadas por el usuario.

1. Cree una [directiva de acceso en Key Vault](../key-vault/general/secure-your-key-vault.md#key-vault-access-policies) para la identidad de aplicación que creó anteriormente. Habilite el permiso secreto "Get" en esta directiva. No configure la "aplicación autorizada" o la configuración `applicationId`, ya que no es compatible con una identidad administrada.

    > [!NOTE]
    > Actualmente, las referencias de Key Vault no pueden resolver los secretos almacenados en un almacén de claves con [restricciones de red](../key-vault/general/overview-vnet-service-endpoints.md).

## <a name="reference-syntax"></a>Sintaxis de referencia

Una referencia de Key Vault tiene el formato `@Microsoft.KeyVault({referenceString})`, donde `{referenceString}` se sustituye por una de las opciones siguientes:

> [!div class="mx-tdBreakAll"]
> | Cadena de referencia                                                            | Descripción                                                                                                                                                                                 |
> |-----------------------------------------------------------------------------|---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
> | SecretUri=_secretUri_                                                       | **SecretUri** debe ser el URI completo del plano de datos de un secreto en Key Vault, incluida una versión, por ejemplo, https://myvault.vault.azure.net/secrets/mysecret/ec96f02080254f109c51a1f14cdb1931.  |
> | VaultName=_vaultName_;SecretName=_secretName_;SecretVersion=_secretVersion_ | **VaultName** debe ser el nombre del recurso de Key Vault. **SecretName** debe ser el nombre del secreto de destino. **SecretVersion** debe ser la versión del secreto que se va a usar. |

Por ejemplo, una referencia completa con versión se parecería a la siguiente:

```
@Microsoft.KeyVault(SecretUri=https://myvault.vault.azure.net/secrets/mysecret/ec96f02080254f109c51a1f14cdb1931)
```
O bien:

```
@Microsoft.KeyVault(VaultName=myvault;SecretName=mysecret;SecretVersion=ec96f02080254f109c51a1f14cdb1931)
```


## <a name="source-application-settings-from-key-vault"></a>Configuración de la aplicación de origen desde Key Vault

Las referencias de Key Vault pueden usarse como valores en [Configuración de la aplicación](configure-common.md#configure-app-settings), lo que le permite conservar los secretos en Key Vault en lugar de en la configuración del sitio. La configuración de la aplicación se cifra de forma segura en reposo, pero si necesita funcionalidades de administración de secretos, debe incluirse en Key Vault.

Para usar una referencia de Key Vault para la configuración de la aplicación, establezca la referencia como el valor de la configuración. La aplicación puede hacer referencia al secreto mediante su clave de la forma habitual. No se requiere ningún cambio de código.

> [!TIP]
> La mayoría de los valores de configuración de la aplicación que usan referencias de Key Vault se deben marcar como configuración de ranuras, así que debe tener distintos almacenes para cada entorno.

### <a name="azure-resource-manager-deployment"></a>Implementación de Azure Resource Manager

Al automatizar las implementaciones de recursos mediante plantillas de Azure Resource Manager, puede que deba secuenciar sus dependencias en un orden determinado para que esta característica funcione. Cabe señalar que deberá definir la configuración de la aplicación como su propio recurso en lugar de usar una propiedad `siteConfig` en la definición del sitio. El motivo es que primero es necesario definir el sitio para que la identidad asignada por el sistema se cree con él y se pueda usar en la directiva de acceso.

Una seudoplantilla de ejemplo para una aplicación de función podría tener este aspecto:

```json
{
    //...
    "resources": [
        {
            "type": "Microsoft.Storage/storageAccounts",
            "name": "[variables('storageAccountName')]",
            //...
        },
        {
            "type": "Microsoft.Insights/components",
            "name": "[variables('appInsightsName')]",
            //...
        },
        {
            "type": "Microsoft.Web/sites",
            "name": "[variables('functionAppName')]",
            "identity": {
                "type": "SystemAssigned"
            },
            //...
            "resources": [
                {
                    "type": "config",
                    "name": "appsettings",
                    //...
                    "dependsOn": [
                        "[resourceId('Microsoft.Web/sites', variables('functionAppName'))]",
                        "[resourceId('Microsoft.KeyVault/vaults/', variables('keyVaultName'))]",
                        "[resourceId('Microsoft.KeyVault/vaults/secrets', variables('keyVaultName'), variables('storageConnectionStringName'))]",
                        "[resourceId('Microsoft.KeyVault/vaults/secrets', variables('keyVaultName'), variables('appInsightsKeyName'))]"
                    ],
                    "properties": {
                        "AzureWebJobsStorage": "[concat('@Microsoft.KeyVault(SecretUri=', reference(variables('storageConnectionStringResourceId')).secretUriWithVersion, ')')]",
                        "WEBSITE_CONTENTAZUREFILECONNECTIONSTRING": "[concat('@Microsoft.KeyVault(SecretUri=', reference(variables('storageConnectionStringResourceId')).secretUriWithVersion, ')')]",
                        "APPINSIGHTS_INSTRUMENTATIONKEY": "[concat('@Microsoft.KeyVault(SecretUri=', reference(variables('appInsightsKeyResourceId')).secretUriWithVersion, ')')]",
                        "WEBSITE_ENABLE_SYNC_UPDATE_SITE": "true"
                        //...
                    }
                },
                {
                    "type": "sourcecontrols",
                    "name": "web",
                    //...
                    "dependsOn": [
                        "[resourceId('Microsoft.Web/sites', variables('functionAppName'))]",
                        "[resourceId('Microsoft.Web/sites/config', variables('functionAppName'), 'appsettings')]"
                    ],
                }
            ]
        },
        {
            "type": "Microsoft.KeyVault/vaults",
            "name": "[variables('keyVaultName')]",
            //...
            "dependsOn": [
                "[resourceId('Microsoft.Web/sites', variables('functionAppName'))]"
            ],
            "properties": {
                //...
                "accessPolicies": [
                    {
                        "tenantId": "[reference(concat('Microsoft.Web/sites/',  variables('functionAppName'), '/providers/Microsoft.ManagedIdentity/Identities/default'), '2015-08-31-PREVIEW').tenantId]",
                        "objectId": "[reference(concat('Microsoft.Web/sites/',  variables('functionAppName'), '/providers/Microsoft.ManagedIdentity/Identities/default'), '2015-08-31-PREVIEW').principalId]",
                        "permissions": {
                            "secrets": [ "get" ]
                        }
                    }
                ]
            },
            "resources": [
                {
                    "type": "secrets",
                    "name": "[variables('storageConnectionStringName')]",
                    //...
                    "dependsOn": [
                        "[resourceId('Microsoft.KeyVault/vaults/', variables('keyVaultName'))]",
                        "[resourceId('Microsoft.Storage/storageAccounts', variables('storageAccountName'))]"
                    ],
                    "properties": {
                        "value": "[concat('DefaultEndpointsProtocol=https;AccountName=', variables('storageAccountName'), ';AccountKey=', listKeys(variables('storageAccountResourceId'),'2015-05-01-preview').key1)]"
                    }
                },
                {
                    "type": "secrets",
                    "name": "[variables('appInsightsKeyName')]",
                    //...
                    "dependsOn": [
                        "[resourceId('Microsoft.KeyVault/vaults/', variables('keyVaultName'))]",
                        "[resourceId('Microsoft.Insights/components', variables('appInsightsName'))]"
                    ],
                    "properties": {
                        "value": "[reference(resourceId('microsoft.insights/components/', variables('appInsightsName')), '2015-05-01').InstrumentationKey]"
                    }
                }
            ]
        }
    ]
}
```

> [!NOTE] 
> En este ejemplo, la implementación del control de código fuente depende de la configuración de la aplicación. Este comportamiento es normalmente poco seguro, ya que la actualización de la configuración de la aplicación se comporta de forma asincrónica. Sin embargo, como hemos incluido la configuración de la aplicación `WEBSITE_ENABLE_SYNC_UPDATE_SITE`, la actualización es sincrónica. Esto significa que la implementación del control de código fuente solo comenzará una vez que la configuración de la aplicación se haya actualizado completamente.

## <a name="troubleshooting-key-vault-references"></a>Solución de problemas de las referencias de Key Vault

Si alguna referencia no se resuelve correctamente, se usará el valor de la referencia, lo que significa que en la configuración de la aplicación se creará una variable de entorno cuyo valor tiene la sintaxis `@Microsoft.KeyVault(...)`. Esto puede provocar que la aplicación genere errores, ya que esperaba un secreto con una estructura determinada.

Habitualmente, esto se debe a una mala configuración de la [directiva de acceso de Key Vault](#granting-your-app-access-to-key-vault). Sin embargo, también puede deberse a que el secreto ya no existe o a un error de sintaxis de la propia referencia.

Si la sintaxis es correcta, compruebe el estado de resolución actual en el portal para ver otras posibles causas del error. Vaya a Configuración de la aplicación y seleccione "Editar" para la referencia en cuestión. En la configuración de opciones, debe ver la información sobre el estado, incluido cualquier error. La ausencia de estos implica que la sintaxis de referencia no es válida.

También puede usar uno de los detectores integrados para obtener información adicional.

### <a name="using-the-detector-for-app-service"></a>Uso del detector en App Service

1. En el portal, vaya a la aplicación.
2. Seleccione **Diagnóstico y solución de problemas**.
3. Elija **Availability and Performance** (Disponibilidad y rendimiento) y seleccione **Web app down** (La aplicación web no funciona).
4. Busque **Key Vault Application Settings Diagnostics** (Diagnóstico de configuración de la aplicación Key Vault) y haga clic en **Más información**.


### <a name="using-the-detector-for-azure-functions"></a>Uso del detector en Azure Functions

1. En el portal, vaya a la aplicación.
2. Seleccione **Características de la plataforma**.
3. Seleccione **Diagnóstico y solución de problemas**.
4. Elija **Disponibilidad y rendimiento** y seleccione **Function app down or reporting errors** (La aplicación de funciones no funciona o presenta errores).
5. Haga clic en **Key Vault Application Settings Diagnostics** (Diagnóstico de configuración de la aplicación Key Vault).
