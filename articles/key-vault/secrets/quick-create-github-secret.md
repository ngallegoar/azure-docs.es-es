---
title: 'Inicio rápido: Uso de secretos de Azure Key Vault en flujos de trabajo de Acciones de GitHub'
description: Uso de secretos de Key Vault en Acciones de GitHub para automatizar los flujos de trabajo de desarrollo de software
author: juliakm
ms.custom: github-actions-azure
ms.author: jukullam
ms.date: 11/24/2020
ms.service: key-vault
ms.subservice: secrets
ms.topic: quickstart
ms.openlocfilehash: 9509f84b14a42180189a529282b5db348deab279
ms.sourcegitcommit: a43a59e44c14d349d597c3d2fd2bc779989c71d7
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 11/25/2020
ms.locfileid: "95920240"
---
# <a name="use-key-vault-secrets-in-github-actions-workflows"></a>Uso de secretos de Key Vault en flujos de trabajo de Acciones de GitHub

Use secretos de Key Vault en [Acciones de GitHub](https://help.github.com/en/articles/about-github-actions) y almacene de forma segura contraseñas y otros secretos en un almacén de claves de Azure. Más información sobre [Key Vault](../general/overview.md). 

Con Acciones de GitHub y Key Vault, tiene las ventajas de una herramienta de administración de secretos centralizada y todas las ventajas de Acciones de GitHub. Acciones de GitHub es un conjunto de características de GitHub que automatizan los flujos de trabajo de desarrollo de software. Puede implementar flujos de trabajo en el mismo lugar donde almacena el código y colaborar en las solicitudes de incorporación de cambios y los problemas. 


## <a name="prerequisites"></a>Requisitos previos 
- Una cuenta de GitHub. Si no tiene ninguna, regístrese [gratis](https://github.com/join).  
- Una cuenta de Azure con una suscripción activa. [Cree una cuenta gratuita](https://azure.microsoft.com/free/?WT.mc_id=A261C142F).
- Una aplicación de Azure conectada a un repositorio de GitHub. En este ejemplo se usa [Implementación de contenedores en Azure App Service](https://docs.microsoft.com/azure/developer/javascript/tutorial-vscode-docker-node-01). 
- Un almacén de claves de Azure.  Puede crear una instancia de Azure Key Vault mediante Azure Portal, la CLI de Azure o Azure PowerShell.

## <a name="workflow-file-overview"></a>Información general sobre el archivo de flujo de trabajo

Un archivo YAML (.yml) define un flujo de trabajo en la ruta de acceso `/.github/workflows/` de su repositorio. En esta definición se incluyen los diversos pasos y parámetros que componen el flujo de trabajo.

El archivo tiene dos secciones para autenticarse con Acciones de GitHub:

|Sección  |Tareas  |
|---------|---------|
|**Autenticación** | 1. Defina una entidad de servicio. <br /> 2. Cree un secreto de GitHub. <br /> 3. Agregue una asignación de roles. |
|**Key Vault** | 1. Agregue la acción del almacén de claves. <br /> 2. Haga referencia a secreto del almacén de claves. |

## <a name="authentication"></a>Authentication

Puede crear una [entidad de servicio](../../active-directory/develop/app-objects-and-service-principals.md#service-principal-object) mediante el comando [az ad sp create-for-rbac](/cli/azure/ad/sp?view=azure-cli-latest#az-ad-sp-create-for-rbac&preserve-view=true) de la [CLI de Azure](/cli/azure/). Puede ejecutar este comando mediante [Azure Cloud Shell](https://shell.azure.com/) en Azure Portal o haciendo clic en el botón **Probar**.

```azurecli-interactive
   az ad sp create-for-rbac --name {myApp} --role contributor --scopes /subscriptions/{subscription-id}/resourceGroups/{MyResourceGroup} --sdk-auth
```

En el ejemplo anterior, reemplace los marcadores de posición por el identificador de la suscripción y el nombre del grupo de recursos. Sustituya el marcador de posición `myApp` por el nombre de aplicación. La salida es un objeto JSON con las credenciales de asignación de roles que proporcionan acceso a la aplicación App Service similar al siguiente. Copie este objeto JSON para más adelante. Solo necesitará las secciones con los valores `clientId`, `clientSecret`, `subscriptionId` y `tenantId`. 

```output 
  {
    "clientId": "<GUID>",
    "clientSecret": "<GUID>",
    "subscriptionId": "<GUID>",
    "tenantId": "<GUID>",
    (...)
  }
```

### <a name="configure-the-github-secret"></a>Configuración del secreto de GitHub

Cree secretos para las credenciales de Azure, el grupo de recursos y las suscripciones. 

1. En [GitHub](https://github.com/), examine el repositorio.

1. Seleccione **Settings > Secrets > New secret** (Configuración > Secretos > Secreto nuevo).

1. Pegue la salida JSON completa del comando de la CLI de Azure en el campo de valor del secreto. Asigne al secreto el nombre `AZURE_CREDENTIALS`.

1. Copie el valor de `clientId` para usarlo más adelante. 

### <a name="add-a-role-assignment"></a>Adición de una asignación de roles 
 
Debe conceder acceso a la entidad de servicio de Azure para que el usuario pueda acceder al almacén de claves para operaciones `get` y `list`. Si no lo hace, no podrá usar la entidad de servicio. 

Reemplace `keyVaultName` por el nombre del almacén de claves y `clientIdGUID`, por el valor de `clientId`. 

```azurecli-interactive
    az keyvault set-policy -n {keyVaultName} --secret-permissions get list --spn {clientIdGUID}
```

## <a name="use-the-azure-key-vault-action"></a>Uso de la acción de Azure Key Vault

Con la [acción de Azure Key Vault](https://github.com/marketplace/actions/azure-key-vault-get-secrets) puede capturar uno o varios secretos de una instancia de Azure Key Vault y consumirlos en los flujos de trabajo de Acciones de GitHub.

Los secretos capturados se establecen como salidas y también como variables de entorno. Las variables se enmascaran automáticamente cuando se imprimen en la consola o en los registros.

```yaml
    - uses: Azure/get-keyvault-secrets@v1
      with:
        keyvault: "my Vault" # name of key vault in Azure portal
        secrets: 'mySecret'  # comma separated list of secret keys to fetch from key vault 
      id: myGetSecretAction # ID for secrets that you will reference
```

Para usar un almacén de claves en el flujo de trabajo, necesita la acción del almacén de claves y hacer referencia a esa acción. 

En este ejemplo, el almacén de claves se llama `containervault`. Con la acción del almacén de claves se agregan dos secretos del almacén de claves al entorno, `containerPassword` y `containerUsername`. 

Posteriormente, en la tarea de inicio de sesión de Docker con el prefijo `steps.myGetSecretAction.outputs` se hace referencia a los valores del almacén de claves. 

```yaml
name: Example key vault flow

on: [push]

jobs:
  build:
    runs-on: ubuntu-latest
    steps:
    # checkout the repo
    - uses: actions/checkout@v2
    - uses: Azure/login@v1
      with:
        creds: ${{ secrets.AZURE_CREDENTIALS }}
    - uses: Azure/get-keyvault-secrets@v1
      with: 
        keyvault: "containervault"
        secrets: 'containerPassword, containerUsername'
      id: myGetSecretAction
    - uses: azure/docker-login@v1
      with:
        login-server: myregistry.azurecr.io
        username: ${{ steps.myGetSecretAction.outputs.containerUsername }}
        password: ${{ steps.myGetSecretAction.outputs.containerPassword }}
    - run: |
        docker build . -t myregistry.azurecr.io/myapp:${{ github.sha }}
        docker push myregistry.azurecr.io/myapp:${{ github.sha }}     
    - uses: azure/webapps-deploy@v2
      with:
        app-name: 'myapp'
        publish-profile: ${{ secrets.AZURE_WEBAPP_PUBLISH_PROFILE }}
        images: 'myregistry.azurecr.io/myapp:${{ github.sha }}'
```

## <a name="clean-up-resources"></a>Limpieza de recursos

Cuando la aplicación de Azure, el repositorio de GitHub y el almacén de claves ya no sean necesarios, limpie los recursos que implementó; para ello, elimine el grupo de recursos de la aplicación, el repositorio de GitHub y el almacén de claves.

## <a name="next-steps"></a>Pasos siguientes

> [!div class="nextstepaction"]
> [Más información sobre Azure Key Vault](../general/overview.md)
