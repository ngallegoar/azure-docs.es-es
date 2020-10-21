---
title: Cifrado del registro con una clave administrada por el cliente
description: Obtenga información sobre el cifrado en reposo de una instancia de Azure Container Registry y sobre cómo cifrar el registro Premium con una clave administrada por el cliente almacenada en Azure Key Vault
ms.topic: article
ms.date: 09/30/2020
ms.custom: ''
ms.openlocfilehash: 6eaae5266277a6a65c7cecaa761b75e3a41ebe87
ms.sourcegitcommit: 541bb46e38ce21829a056da880c1619954678586
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/11/2020
ms.locfileid: "91940674"
---
# <a name="encrypt-registry-using-a-customer-managed-key"></a>Cifrado del registro con una clave administrada por el cliente

Cuando se almacenan imágenes y otros artefactos en una instancia de Azure Container Registry, Azure cifra automáticamente el contenido del registro en reposo con [claves administradas por el servicio](../security/fundamentals/encryption-models.md). Puede complementar el cifrado predeterminado con una capa de cifrado adicional mediante una clave que se crea y administra en Azure Key Vault (una clave administrada por el cliente). Este artículo le guía a través del proceso mediante la CLI de Azure y Azure Portal.

Se admite el cifrado de lado servidor con claves administradas por el cliente por medio de la integración con [Azure Key Vault](../key-vault/general/overview.md). Puede crear claves de cifrado propias y almacenarlas en un almacén de claves, o puede usar API de Azure Key Vault para generar claves. Con Azure Key Vault, también puede auditar el uso de claves.

Esta característica está disponible en el nivel de servicio de un registro de contenedor **Premium**. Para obtener información sobre los límites y los niveles de servicio de los registros, vea [Niveles de servicio de Azure Container Registry](container-registry-skus.md).


## <a name="things-to-know"></a>Cosas que debe saber

* De momento, solo se puede habilitar una clave administrada por el cliente al crear un registro. Al habilitar la clave, se configura una identidad administrada *asignada por el usuario* para acceder al almacén de claves.
* Después de habilitar el cifrado con una clave administrada por el cliente en un registro, no es posible deshabilitarlo.  
* [Confianza de contenido](container-registry-content-trust.md) no se admite actualmente en un registro cifrado con una clave administrada por el cliente.
* En un registro cifrado con una clave administrada por el cliente, los registros de ejecución de [ACR Tasks](container-registry-tasks-overview.md) solo se conservan durante 24 horas. Si necesita conservar los registros durante un período más largo, vea la guía para [exportar y almacenar registros de ejecución de tareas](container-registry-tasks-logs.md#alternative-log-storage).


> [!NOTE]
> Si se restringe el acceso a Azure Key Vault mediante una red virtual con un [firewall de Key Vault](../key-vault/general/network-security.md), se necesitan pasos de configuración adicionales. Después de crear el registro y habilitar la clave administrada por el cliente, configure el acceso a la clave mediante la identidad administrada *asignada por el sistema* del registro y configure este para omitir el firewall de Key Vault. Siga los pasos de este artículo para habilitar el cifrado con una clave administrada por el cliente y, a continuación, consulte la guía de [Escenario avanzado: Firewall de Key Vault](#advanced-scenario-key-vault-firewall) más adelante en este artículo.

## <a name="prerequisites"></a>Requisitos previos

Para aplicar los pasos de la CLI de Azure de este artículo, se necesita la versión 2.2.0 o posterior de la CLI de Azure. Si necesita instalarla o actualizarla, vea [Instalación de la CLI de Azure](/cli/azure/install-azure-cli).

## <a name="enable-customer-managed-key---cli"></a>Habilitación de una clave administrada por el cliente: CLI

### <a name="create-a-resource-group"></a>Crear un grupo de recursos

Si es necesario, ejecute el comando [az group create][az-group-create] a fin de crear un grupo de recursos para la creación del almacén de claves, el registro de contenedor y otros recursos necesarios.

```azurecli
az group create --name <resource-group-name> --location <location>
```

### <a name="create-a-user-assigned-managed-identity"></a>Crear una identidad administrada asignada por el usuario

Cree una [identidad administrada para recursos de Azure](../active-directory/managed-identities-azure-resources/overview.md) asignada por el usuario con el comando [az identity create][az-identity-create]. El registro usa esta identidad para acceder al servicio Key Vault.

```azurecli
az identity create \
  --resource-group <resource-group-name> \
  --name <managed-identity-name>
```

En el resultado del comando, anote los siguientes valores: `id` y `principalId`. Los va a necesitar en pasos posteriores para configurar el acceso del registro al almacén de claves.

```JSON
{
  "clientId": "xxxx2bac-xxxx-xxxx-xxxx-192cxxxx6273",
  "clientSecretUrl": "https://control-eastus.identity.azure.net/subscriptions/xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx/resourcegroups/myresourcegroup/providers/Microsoft.ManagedIdentity/userAssignedIdentities/myidentityname/credentials?tid=xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx&oid=xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx&aid=xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx",
  "id": "/subscriptions/xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx/resourcegroups/myresourcegroup/providers/Microsoft.ManagedIdentity/userAssignedIdentities/myresourcegroup",
  "location": "eastus",
  "name": "myidentityname",
  "principalId": "xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx",
  "resourceGroup": "myresourcegroup",
  "tags": {},
  "tenantId": "xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx",
  "type": "Microsoft.ManagedIdentity/userAssignedIdentities"
}
```

Por comodidad, almacene estos valores en variables de entorno:

```azurecli
identityID=$(az identity show --resource-group <resource-group-name> --name <managed-identity-name> --query 'id' --output tsv)

identityPrincipalID=$(az identity show --resource-group <resource-group-name> --name <managed-identity-name> --query 'principalId' --output tsv)
```

### <a name="create-a-key-vault"></a>Creación de un Almacén de claves

Cree un almacén de claves con [az keyvault create][az-keyvault-create] para almacenar una clave administrada por el cliente para el cifrado del registro.

Para evitar la pérdida de datos causada por eliminaciones accidentales de la clave o el almacén de claves, habilite los valores siguientes: **Eliminación temporal** y **Protección de purga**. En el ejemplo siguiente se incluyen parámetros para estos valores:

```azurecli
az keyvault create --name <key-vault-name> \
  --resource-group <resource-group-name> \
  --enable-soft-delete \
  --enable-purge-protection
```

> [!NOTE]
> A partir de la versión 2.2 de la CLI de Azure, `az keyvault create` habilita la eliminación temporal de manera predeterminada.

Para su uso en pasos posteriores, obtenga el identificador de recurso del almacén de claves:

```azurecli
keyvaultID=$(az keyvault show --resource-group <resource-group-name> --name <key-vault-name> --query 'id' --output tsv)
```

### <a name="enable-key-vault-access"></a>Habilitación del acceso al almacén de claves

Configure una directiva para el almacén de claves de modo que la identidad pueda acceder a él. En el siguiente comando [az keyvault set-policy][az-keyvault-set-policy], se pasa el identificador de la entidad de seguridad de la identidad administrada creada, almacenada previamente en una variable de entorno. Establezca los permisos de la clave en **get**, **unwrapKey** y **wrapKey**.  

```azurecli
az keyvault set-policy \
  --resource-group <resource-group-name> \
  --name <key-vault-name> \
  --object-id $identityPrincipalID \
  --key-permissions get unwrapKey wrapKey
```

También puede usar [Azure RBAC para Key Vault](../key-vault/general/rbac-guide.md) (versión preliminar) para asignar permisos a la identidad para obtener acceso al almacén de claves. Por ejemplo, asigne el rol de cifrado de servicio criptográfico de Key Vault a la identidad mediante el comando [az role assignment create](/cli/azure/role/assignment?view=azure-cli-latest#az-role-assignment-create):

```azurecli 
az role assignment create --assignee $identityPrincipalID \
  --role "Key Vault Crypto Service Encryption (preview)" \
  --scope $keyvaultID
```

### <a name="create-key-and-get-key-id"></a>Creación de clave y obtención de su identificador

Ejecute el comando [az keyvault key create][az-keyvault-key-create] para crear una clave en el almacén de claves.

```azurecli
az keyvault key create \
  --name <key-name> \
  --vault-name <key-vault-name>
```

En el resultado del comando, anote el identificador de la clave, `kid`. Este identificador se usa en el paso siguiente:

```JSON
[...]
  "key": {
    "crv": null,
    "d": null,
    "dp": null,
    "dq": null,
    "e": "AQAB",
    "k": null,
    "keyOps": [
      "encrypt",
      "decrypt",
      "sign",
      "verify",
      "wrapKey",
      "unwrapKey"
    ],
    "kid": "https://mykeyvault.vault.azure.net/keys/mykey/xxxxxxxxxxxxxxxxxxxxxxxx",
    "kty": "RSA",
[...]
```
Por comodidad, almacene este valor en una variable de entorno:

```azurecli
keyID=$(az keyvault key show \
  --name <keyname> \
  --vault-name <key-vault-name> \
  --query 'key.kid' --output tsv)
```

### <a name="create-a-registry-with-customer-managed-key"></a>Creación de un registro con una clave administrada por el cliente

Ejecute el comando [az acr create][az-acr-create] para crear un registro en el nivel de servicio Premium y habilitar la clave administrada por el cliente. Pase el identificador de la entidad de seguridad de la identidad administrada y el identificador de clave almacenados previamente en variables de entorno:

```azurecli
az acr create \
  --resource-group <resource-group-name> \
  --name <container-registry-name> \
  --identity $identityID \
  --key-encryption-key $keyID \
  --sku Premium
```

### <a name="show-encryption-status"></a>Presentación del estado de cifrado

Para mostrar si está habilitado el cifrado del registro con una clave administrada por el cliente, ejecute el comando [az acr encryption show][az-acr-encryption-show]:

```azurecli
az acr encryption show --name <registry-name>
```

La salida es parecida a esta:

```console
{
  "keyVaultProperties": {
    "identity": "xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx",
    "keyIdentifier": "https://myvault.vault.azure.net/keys/myresourcegroup/abcdefg123456789...",
    "versionedKeyIdentifier": "https://myvault.vault.azure.net/keys/myresourcegroup/abcdefg123456789..."
  },
  "status": "enabled"
}
```

## <a name="enable-customer-managed-key---portal"></a>Habilitación de una clave administrada por el cliente: portal

### <a name="create-a-managed-identity"></a>Creación de una entidad administrada

Cree una [identidad administrada para recursos de Azure](../active-directory/managed-identities-azure-resources/overview.md) asignada por el usuario en Azure Portal. Para conocer los pasos, vea [Creación de una identidad asignada por el usuario](../active-directory/managed-identities-azure-resources/how-to-manage-ua-identity-portal.md#create-a-user-assigned-managed-identity).

En los pasos posteriores, use el nombre de la identidad.

![Creación de una identidad administrada asignada por el usuario en Azure Portal](./media/container-registry-customer-managed-keys/create-managed-identity.png)

### <a name="create-a-key-vault"></a>Creación de un Almacén de claves

Para conocer los pasos para crear un almacén de claves, vea [Inicio rápido: Establecimiento y recuperación de un secreto de Azure Key Vault mediante Azure Portal](../key-vault/secrets/quick-create-portal.md).

Al crear un almacén de claves para una clave administrada por el cliente, en la pestaña **Datos básicos**, habilite los siguientes valores de protección: **Eliminación temporal** y **Protección de purga**. Estos valores ayudan a evitar la pérdida de datos causada por eliminaciones accidentales de la clave o el almacén de claves.

![Creación de un almacén de claves en Azure Portal](./media/container-registry-customer-managed-keys/create-key-vault.png)

### <a name="enable-key-vault-access"></a>Habilitación del acceso al almacén de claves

Configure una directiva para el almacén de claves de modo que la identidad pueda acceder a él.

1. Vaya al almacén de claves.
1. Seleccione **Configuración** > **Directivas de acceso > +Agregar directiva de acceso**.
1. Seleccione **Permisos de clave** y **Obtener**, **Desencapsular clave** y **Encapsular clave**.
1. Seleccione **Seleccionar la entidad de seguridad** y luego el nombre de recurso de la identidad administrada asignada por el usuario.  
1. Seleccione **Agregar** y después **Guardar**.

![Creación de directiva de acceso del almacén de claves](./media/container-registry-customer-managed-keys/add-key-vault-access-policy.png)

 También puede usar [Azure RBAC para Key Vault](../key-vault/general/rbac-guide.md) (versión preliminar) para asignar permisos a la identidad para obtener acceso al almacén de claves. Por ejemplo, asigne el rol de cifrado de servicio criptográfico de Key Vault a la identidad.

1. Vaya al almacén de claves.
1. Seleccione **Control de acceso (IAM)**  >  **+Agregar** > **Agregar asignación de roles**.
1. En la ventana **Agregar asignación de roles**:
    1. Seleccione el rol de **Cifrado de servicio criptográfico de Key Vault (versión preliminar)** . 
    1. Asigne acceso a la **identidad administrada asignada por el usuario**.
    1. Seleccione el nombre de recurso de la identidad administrada asignada por el usuario y luego **Guardar**.

### <a name="create-key"></a>Crear clave

1. Vaya al almacén de claves.
1. Seleccione **Configuración** > **Claves**.
1. Seleccione **+Generar o importar** y escriba un nombre único para la clave.
1. Acepte los restantes valores predeterminados y seleccione **Crear**.
1. Tras la creación, seleccione la clave y anote la versión de la clave actual.

### <a name="create-azure-container-registry"></a>Creación de una instancia de Azure Container Registry

1. Seleccione **Crear un recurso** > **Contenedores** > **Registro de contenedor**.
1. En la pestaña **Datos básicos**, seleccione o cree un grupo de recursos y escriba un nombre de registro. En **SKU**, seleccione **Premium**.
1. En la pestaña **Cifrado**, en **Clave administrada por el cliente**, seleccione **Habilitado**.
1. En **Identidad**, seleccione la identidad administrada que ha creado.
1. En **Cifrado**, elija **Seleccionar de Key Vault**.
1. En la ventana **Seleccione clave de Azure Key Vault**, seleccione el almacén de claves, la clave y la versión que ha creado en la sección anterior.
1. En la pestaña **Cifrado**, seleccione **Revisar y crear**.
1. Seleccione **Crear** para implementar la instancia del registro.

![Creación de un registro de contenedor en Azure Portal](./media/container-registry-customer-managed-keys/create-encrypted-registry.png)

Para ver el estado de cifrado del registro en el portal, vaya al registro. En **Configuración**, seleccione **Cifrado**.

## <a name="enable-customer-managed-key---template"></a>Habilitación de una clave administrada por el cliente: plantilla

También puede usar una plantilla de Resource Manager para crear un registro y habilitar el cifrado con una clave administrada por el cliente.

La siguiente plantilla crea un nuevo registro de contenedor y una identidad administrada asignada por el usuario. Copie el siguiente contenido en un archivo nuevo y guárdelo con un nombre de archivo como `CMKtemplate.json`.

```JSON
{
  "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
  "contentVersion": "1.0.0.0",
  "parameters": {
    "vault_name": {
      "defaultValue": "",
      "type": "String"
    },
    "registry_name": {
      "defaultValue": "",
      "type": "String"
    },
    "identity_name": {
      "defaultValue": "",
      "type": "String"
    },
    "kek_id": {
      "type": "String"
    }
  },
  "variables": {},
  "resources": [
    {
      "type": "Microsoft.ContainerRegistry/registries",
      "apiVersion": "2019-12-01-preview",
      "name": "[parameters('registry_name')]",
      "location": "[resourceGroup().location]",
      "sku": {
        "name": "Premium",
        "tier": "Premium"
      },
      "identity": {
        "type": "UserAssigned",
        "userAssignedIdentities": {
          "[resourceID('Microsoft.ManagedIdentity/userAssignedIdentities', parameters('identity_name'))]": {}
        }
      },
      "dependsOn": [
        "[resourceId('Microsoft.ManagedIdentity/userAssignedIdentities', parameters('identity_name'))]"
      ],
      "properties": {
        "adminUserEnabled": false,
        "encryption": {
          "status": "enabled",
          "keyVaultProperties": {
            "identity": "[reference(resourceId('Microsoft.ManagedIdentity/userAssignedIdentities', parameters('identity_name')), '2018-11-30').clientId]",
            "KeyIdentifier": "[parameters('kek_id')]"
          }
        },
        "networkRuleSet": {
          "defaultAction": "Allow",
          "virtualNetworkRules": [],
          "ipRules": []
        },
        "policies": {
          "quarantinePolicy": {
            "status": "disabled"
          },
          "trustPolicy": {
            "type": "Notary",
            "status": "disabled"
          },
          "retentionPolicy": {
            "days": 7,
            "status": "disabled"
          }
        }
      }
    },
    {
      "type": "Microsoft.KeyVault/vaults/accessPolicies",
      "apiVersion": "2018-02-14",
      "name": "[concat(parameters('vault_name'), '/add')]",
      "dependsOn": [
        "[resourceId('Microsoft.ManagedIdentity/userAssignedIdentities', parameters('identity_name'))]"
      ],
      "properties": {
        "accessPolicies": [
          {
            "tenantId": "[subscription().tenantId]",
            "objectId": "[reference(resourceId('Microsoft.ManagedIdentity/userAssignedIdentities', parameters('identity_name')), '2018-11-30').principalId]",
            "permissions": {
              "keys": [
                "get",
                "unwrapKey",
                "wrapKey"
              ]
            }
          }
        ]
      }
    },
    {
      "type": "Microsoft.ManagedIdentity/userAssignedIdentities",
      "apiVersion": "2018-11-30",
      "name": "[parameters('identity_name')]",
      "location": "[resourceGroup().location]"
    }
  ]
}

```

Siga los pasos de las secciones anteriores para crear los siguientes recursos:

* Almacén de claves, identificado por nombre
* Clave del almacén de claves, identificada por identificador de clave

Ejecute el siguiente comando [az group deployment create][az-group-deployment-create] para crear el registro con el archivo de plantilla anterior. Donde se indique, proporcione un nuevo nombre de registro y un nombre de identidad administrada, así como el nombre del almacén de claves y el identificador de clave que ha creado.

```bash
az group deployment create \
  --resource-group <resource-group-name> \
  --template-file CMKtemplate.json \
  --parameters \
    registry_name=<registry-name> \
    identity_name=<managed-identity> \
    vault_name=<key-vault-name> \
    kek_id=<key-vault-key-id>
```

### <a name="show-encryption-status"></a>Presentación del estado de cifrado

Para mostrar el estado de cifrado del registro, ejecute el comando [az acr encryption show][az-acr-encryption-show]:

```azurecli
az acr encryption show --name <registry-name>
```

## <a name="use-the-registry"></a>Uso del registro

Después de habilitar una clave administrada por el cliente en un registro, puede realizar las mismas operaciones de registro que en un registro que no está cifrado con una clave administrada por el cliente. Por ejemplo, puede autenticarse en el registro e insertar imágenes de Docker. Vea comandos de ejemplo en [Inserción y extracción de una imagen](container-registry-get-started-docker-cli.md).

## <a name="rotate-key"></a>Rotación de clave

Gire una clave administrada por el cliente que se use para el cifrado del registro según las directivas de cumplimiento. Cree una nueva clave, o actualice la versión de una clave, y actualice el registro para cifrar los datos mediante la clave. Puede realizar estos pasos con la CLI de Azure o en el portal.

Al girar una clave, normalmente se especifica la misma identidad usada al crear el registro. Opcionalmente, configure una nueva identidad asignada por el usuario para el acceso a la clave o habilite y especifique la identidad asignada por el sistema del registro.

> [!NOTE]
> Establezca el [acceso al almacén de claves](#enable-key-vault-access) necesario para la identidad que configure para el acceso a claves.

### <a name="azure-cli"></a>Azure CLI

Use comandos [az keyvault key][az-keyvault-key] para crear o administrar las claves del almacén de claves. Por ejemplo, para crear una clave o versión de clave, ejecute el comando [az keyvault key create][az-keyvault-key-create]:

```azurecli
# Create new version of existing key
az keyvault key create \
  –-name <key-name> \
  --vault-name <key-vault-name>

# Create new key
az keyvault key create \
  –-name <new-key-name> \
  --vault-name <key-vault-name>
```

Luego, ejecute el comando [az acr encryption rotate-key][az-acr-encryption-rotate-key] y pase el nuevo identificador de la clave y la identidad que quiere configurar:

```azurecli
# Rotate key and use user-assigned identity
az acr encryption rotate-key \
  --name <registry-name> \
  --key-encryption-key <new-key-id> \
  --identity <principal-id-user-assigned-identity>

# Rotate key and use system-assigned identity
az acr encryption rotate-key \
  --name <registry-name> \
  --key-encryption-key <new-key-id> \
  --identity [system]
```

### <a name="portal"></a>Portal

Use la configuración **Cifrado** del registro para actualizar la versión de la clave, la clave, el almacén de claves o la configuración de identidad usados para la clave administrada por el cliente.

Por ejemplo, para generar una versión de clave y configurarla:

1. En el portal, vaya al registro.
1. En **Configuración**, seleccione **Cifrado** > **Cambiar clave**.
1. Seleccione **Seleccionar clave**.

    ![Giro de clave en Azure Portal](./media/container-registry-customer-managed-keys/rotate-key.png)
1. En la ventana **Seleccione clave de Azure Key Vault**, seleccione el almacén de claves y la clave configurados anteriormente y, en **Versión**, seleccione **Crear nueva**.
1. En la ventana **Crear una clave**, seleccione **Generar** y luego **Crear**.
1. Complete la selección de claves y seleccione **Guardar**.

## <a name="revoke-key"></a>Revocación de clave

Para revocar la clave de cifrado administrada por el cliente, cambie la directiva de acceso o los permisos en el almacén de claves o elimine la clave. Por ejemplo, use el comando [az keyvault delete-policy][az-keyvault-delete-policy] para cambiar la directiva de acceso de la identidad administrada usada por el registro:

```azurecli
az keyvault delete-policy \
  --resource-group <resource-group-name> \
  --name <key-vault-name> \
  --object-id $identityPrincipalID
```

Al revocar la clave realmente, se bloquea el acceso a todos los datos del registro, ya que este no puede acceder a la clave de cifrado. Si se habilita el acceso a la clave o se restaura la clave eliminada, el registro toma la clave para que se pueda acceder de nuevo a los datos cifrados del registro.

## <a name="advanced-scenario-key-vault-firewall"></a>Escenario avanzado: Firewall de Key Vault

Si la instancia de Azure Key Vault se implementa en una red virtual con un firewall de Key Vault, siga estos pasos adicionales después de habilitar el cifrado de claves administradas por el cliente en el registro.

1. Configurar el cifrado del registro para usar la identidad asignada por el sistema del registro
1. Habilitar el registro para omitir el firewall de Key Vault
1. Rotar la clave administrada por el cliente

### <a name="configure-system-assigned-identity"></a>Configuración de la identidad asignada por el sistema

Puede configurar una identidad administrada asignada por el sistema del registro para acceder al almacén de claves de cifrado. Si no está familiarizado con las diferentes identidades administradas de los recursos de Azure, vea la [Introducción](../active-directory/managed-identities-azure-resources/overview.md).

Para habilitar la identidad asignada por el sistema del registro en el portal:

1. En el portal, vaya al registro.
1. Seleccione **Configuración** >  **Identidad**.
1. En **Asignado por el sistema**, establezca **Estado** en **Activado**. Seleccione **Guardar**.
1. Copie el **Identificador de objeto** de la identidad.

Para conceder acceso a la identidad al almacén de claves:

1. Vaya al almacén de claves.
1. Seleccione **Configuración** > **Directivas de acceso > +Agregar directiva de acceso**.
1. Seleccione **Permisos de clave** y **Obtener**, **Desencapsular clave** y **Encapsular clave**.
1. Seleccione **Seleccionar la entidad de seguridad** y busque el identificador de objeto de la identidad administrada asignada por el sistema, o el nombre del registro.  
1. Seleccione **Agregar** y después **Guardar**.

Para actualizar la configuración de cifrado del registro a fin de usar la identidad:

1. En el portal, vaya al registro.
1. En **Configuración**, seleccione **Cifrado** > **Cambiar clave**.
1. En **Identidad**, seleccione **Asignado por el sistema** y luego **Guardar**.

### <a name="enable-key-vault-bypass"></a>Habilitación de la omisión del almacén de claves

Para acceder a un almacén de claves configurado con un firewall de Key Vault, el registro debe omitir el firewall. Configure el almacén de claves para permitir el acceso de cualquier [servicio de confianza](../key-vault/general/overview-vnet-service-endpoints.md#trusted-services). Azure Container Registry es uno de los servicios de confianza.

1. En el portal, vaya al almacén de claves.
1. Seleccione **Configuración** > **Redes**.
1. Confirme, actualice o agregue la configuración de la red virtual. Para conocer los pasos detallados, vea [Configuración de firewalls y redes virtuales de Azure Key Vault](../key-vault/general/network-security.md).
1. En **¿Quiere permitir que los servicios de confianza de Microsoft puedan omitir este firewall?** , seleccione **Sí**. 

### <a name="rotate-the-customer-managed-key"></a>Rotar la clave administrada por el cliente

Después de completar los pasos anteriores, gire la clave a una nueva en el almacén de claves detrás de un firewall. Para conocer los pasos, consulte [Rotación de clave](#rotate-key) en este artículo.

## <a name="troubleshoot"></a>Solución de problemas

### <a name="removing-user-assigned-identity"></a>Eliminación de una identidad asignada por el usuario

Si intenta quitar una identidad asignada por el usuario de un registro que se usa para el cifrado, es posible que vea un mensaje de error similar al siguiente:
 
```
Azure resource '/subscriptions/xxxx/resourcegroups/myGroup/providers/Microsoft.ContainerRegistry/registries/myRegistry' does not have access to identity 'xxxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxx' Try forcibly adding the identity to the registry <registry name>. For more information on bring your own key, please visit 'https://aka.ms/acr/cmk'.
```
 
Tampoco podrá cambiar (girar) la clave de cifrado. Si se produce este problema, reasigne primero la identidad con el GUID que se muestra en el mensaje de error. Por ejemplo:

```azurecli
az acr identity assign -n myRegistry --identities xxxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxx
```
        
Después, tras cambiar la clave y asignar otra identidad, puede quitar la identidad asignada por el usuario original.

## <a name="next-steps"></a>Pasos siguientes

* Obtenga más información sobre el [cifrado en reposo en Azure](../security/fundamentals/encryption-atrest.md).
* Obtenga más información sobre las directivas de acceso y la [Protección del acceso a un almacén de claves](../key-vault/general/secure-your-key-vault.md).


<!-- LINKS - external -->

<!-- LINKS - internal -->

[az-feature-register]: /cli/azure/feature#az-feature-register
[az-feature-show]: /cli/azure/feature#az-feature-show
[az-group-create]: /cli/azure/group#az-group-create
[az-identity-create]: /cli/azure/identity#az-identity-create
[az-feature-register]: /cli/azure/feature#az-feature-register
[az-group-deployment-create]: /cli/azure/group/deployment#az-group-deployment-create
[az-keyvault-create]: /cli/azure/keyvault#az-keyvault-create
[az-keyvault-key-create]: /cli/azure/keyvault/key#az-keyvault-key-create
[az-keyvault-key]: /cli/azure/keyvault/key
[az-keyvault-set-policy]: /cli/azure/keyvault#az-keyvault-set-policy
[az-keyvault-delete-policy]: /cli/azure/keyvault#az-keyvault-delete-policy
[az-resource-show]: /cli/azure/resource#az-resource-show
[az-acr-create]: /cli/azure/acr#az-acr-create
[az-acr-show]: /cli/azure/acr#az-acr-show
[az-acr-encryption-rotate-key]: /cli/azure/acr/encryption#az-acr-encryption-rotate-key
[az-acr-encryption-show]: /cli/azure/acr/encryption#az-acr-encryption-show
