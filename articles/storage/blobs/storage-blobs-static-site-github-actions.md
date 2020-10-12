---
title: Uso de las acciones de GitHub para implementar un sitio estático en Azure Storage
description: Hospedaje de sitios web estáticos de Azure Storage con acciones de GitHub
author: juliakm
ms.service: storage
ms.topic: how-to
ms.author: jukullam
ms.reviewer: dineshm
ms.date: 09/11/2020
ms.subservice: blobs
ms.custom: devx-track-javascript, github-actions-azure
ms.openlocfilehash: 919fa0d7b6dff0361e4439b442bcfe9648ed8677
ms.sourcegitcommit: ef69245ca06aa16775d4232b790b142b53a0c248
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/06/2020
ms.locfileid: "91776398"
---
# <a name="set-up-a-github-actions-workflow-to-deploy-your-static-website-in-azure-storage"></a>Configuración de un flujo de trabajo de acciones de GitHub para implementar el sitio web estático en Azure Storage

Aprenda a usar las [acciones de GitHub](https://docs.github.com/en/actions) mediante un flujo de trabajo para implementar un sitio estático en una instancia de Azure Storage Blob. Una vez que haya configurado el flujo de trabajo de acciones de GitHub, podrá implementar automáticamente el sitio en Azure desde GitHub cuando realice cambios en el código del sitio. 

> [!NOTE]
> Si usa [Azure Static Web Apps](https://docs.microsoft.com/azure/static-web-apps/), no es necesario configurar manualmente un flujo de trabajo de acciones de GitHub.
> Azure Static Web Apps crea automáticamente un flujo de trabajo de GitHub. 

## <a name="prerequisites"></a>Requisitos previos

Una cuenta de GitHub y una suscripción de Azure. 

- Una cuenta de Azure con una suscripción activa. [Cree una cuenta gratuita](https://azure.microsoft.com/free/?WT.mc_id=A261C142F).
- Una cuenta de GitHub con el código del sitio web estático. Si no tiene una cuenta de GitHub, [regístrese para obtener una gratis](https://github.com/join).  
- Un sitio web estático en funcionamiento hospedado en Azure Storage. Aprenda a administrar el [hospedaje de sitios web estáticos en Azure Storage](storage-blob-static-website-how-to.md). El sitio web estático debe incluir una instancia del servicio [Azure CDN](static-website-content-delivery-network.md).

## <a name="generate-deployment-credentials"></a>Genere las credenciales de implementación.

Puede crear una [entidad de servicio](../../active-directory/develop/app-objects-and-service-principals.md#service-principal-object) mediante el comando [az ad sp create-for-rbac](/cli/azure/ad/sp?view=azure-cli-latest#az-ad-sp-create-for-rbac&preserve-view=true) de la [CLI de Azure](/cli/azure/). Puede ejecutar este comando mediante [Azure Cloud Shell](https://shell.azure.com/) en Azure Portal o haciendo clic en el botón **Probar**.

Reemplace el marcador de posición `myStaticSite` por el nombre del sitio hospedado en Azure Storage. 

```azurecli-interactive
   az ad sp create-for-rbac --name {myStaticSite} --role contributor --scopes /subscriptions/{subscription-id}/resourceGroups/{resource-group} --sdk-auth
```

En el ejemplo anterior, reemplace los marcadores de posición por el identificador de la suscripción y el nombre del grupo de recursos. La salida es un objeto JSON con las credenciales de asignación de roles que proporcionan acceso a la aplicación App Service similar al siguiente. Copie este objeto JSON para más adelante.

```output 
  {
    "clientId": "<GUID>",
    "clientSecret": "<GUID>",
    "subscriptionId": "<GUID>",
    "tenantId": "<GUID>",
    (...)
  }
```

> [!IMPORTANT]
> Siempre es recomendable conceder acceso mínimo. El ámbito del ejemplo anterior se limita a la aplicación específica de App Service y no a todo el grupo de recursos.

## <a name="configure-the-github-secret"></a>Configuración del secreto de GitHub

1. En [GitHub](https://github.com/), examine el repositorio.

1. Seleccione **Settings > Secrets > New secret** (Configuración > Secretos > Secreto nuevo).

1. Pegue la salida JSON completa del comando de la CLI de Azure en el campo de valor del secreto. Asigne al secreto un nombre como `AZURE_CREDENTIALS`.

    Cuando configure el archivo de flujo de trabajo más adelante, usará el secreto para la entrada `creds` de la acción de inicio de sesión de Azure. Por ejemplo:

    ```yaml
    - uses: azure/login@v1
    with:
        creds: ${{ secrets.AZURE_CREDENTIALS }}
    ```

## <a name="add-your-workflow"></a>Agregar el flujo de trabajo

1. Vaya a **Acciones** del repositorio de GitHub. 

    :::image type="content" source="media/storage-blob-static-website/storage-blob-github-actions-header.png" alt-text="Elemento de menú de las acciones de GitHub&quot;:::

1. Seleccione **Set up your workflow yourself** (Configure el flujo de trabajo usted mismo). 

1. Elimine todo lo que aparezca después de la sección `on:` del archivo de flujo de trabajo. Por ejemplo, el flujo de trabajo restante puede tener el siguiente aspecto. 

    ```yaml
    name: CI

    on:
    push:
        branches: [ master ]
    pull_request:
        branches: [ master ]
    ```

1. Cambie el nombre del flujo de trabajo `Blob storage website CI` y agregue las acciones de restauración e inicio de sesión. Estas acciones comprobarán el código del sitio y se autenticarán con Azure mediante el secreto de GitHub `AZURE_CREDENTIALS` que creó anteriormente. 

    ```yaml
    name: Blob storage website CI

    on:
    push:
        branches: [ master ]
    pull_request:
        branches: [ master ]

    jobs:
      build:
        runs-on: ubuntu-latest
        steps:            
        - uses: actions/checkout@v2
        - uses: azure/login@v1
          with:
          creds: ${{ secrets.AZURE_CREDENTIALS }}
    ```

1. Use la acción de la CLI de Azure para cargar el código en la instancia de Blob Storage y purgar el punto de conexión de la red CDN. En cuanto a `az storage blob upload-batch`, reemplace el marcador de posición por el nombre de la cuenta de almacenamiento. El script se cargará en el contenedor `$web`. Para `az cdn endpoint purge`, reemplace los marcadores de posición por el nombre del perfil de CDN, el nombre del punto de conexión de CDN y el grupo de recursos.

    ```yaml
        - name: Upload to blob storage
        uses: azure/CLI@v1
        with:
            azcliversion: 2.0.72
            inlineScript: |
                az storage blob upload-batch --account-name <STORAGE_ACCOUNT_NAME> -d '$web' -s .
        - name: Purge CDN endpoint
        uses: azure/CLI@v1
        with:
            azcliversion: 2.0.72
            inlineScript: |
            az cdn endpoint purge --content-paths  &quot;/*&quot; --profile-name &quot;CDN_PROFILE_NAME&quot; --name &quot;CDN_ENDPOINT&quot; --resource-group &quot;RESOURCE_GROUP&quot;
    ``` 

1. Complete el flujo de trabajo agregando una acción al cierre de sesión de Azure. Este es el flujo de trabajo completado. El archivo aparecerá en la carpeta `.github/workflows` del repositorio.

    ```yaml
   name: Blob storage website CI

    on:
    push:
        branches: [ master ]
    pull_request:
        branches: [ master ]

    jobs:
    build:
        runs-on: ubuntu-latest
        steps:
        - uses: actions/checkout@v2
        - name: Azure Login
        uses: azure/login@v1
        with:
            creds: ${{ secrets.AZURE_CREDENTIALS }}    
        - name: Azure CLI script
        uses: azure/CLI@v1
        with:
            azcliversion: 2.0.72
            inlineScript: |
                az storage blob upload-batch --account-name <STORAGE_ACCOUNT_NAME> -d '$web' -s .
        - name: Azure CLI script
        uses: azure/CLI@v1
        with:
            azcliversion: 2.0.72
            inlineScript: |
            az cdn endpoint purge --content-paths  &quot;/*&quot; --profile-name &quot;CDN_PROFILE_NAME&quot; --name &quot;CDN_ENDPOINT&quot; --resource-group &quot;RESOURCE_GROUP"
            # Azure logout 
        - name: logout
          run: |
                az logout
    ```

## <a name="review-your-deployment"></a>Revisar la implementación

1. Vaya a la opción de **Acciones** del repositorio de GitHub. 

1. Abra el primer resultado para ver los registros detallados de la ejecución del flujo de trabajo. 
 
    :::image type="content" source="../media/index/github-actions-run.png" alt-text="Elemento de menú de las acciones de GitHub&quot;:::

1. Seleccione **Set up your workflow yourself** (Configure el flujo de trabajo usted mismo). 

1. Elimine todo lo que aparezca después de la sección `on:` del archivo de flujo de trabajo. Por ejemplo, el flujo de trabajo restante puede tener el siguiente aspecto. 

    ```yaml
    name: CI

    on:
    push:
        branches: [ master ]
    pull_request:
        branches: [ master ]
    ```

1. Cambie el nombre del flujo de trabajo `Blob storage website CI` y agregue las acciones de restauración e inicio de sesión. Estas acciones comprobarán el código del sitio y se autenticarán con Azure mediante el secreto de GitHub `AZURE_CREDENTIALS` que creó anteriormente. 

    ```yaml
    name: Blob storage website CI

    on:
    push:
        branches: [ master ]
    pull_request:
        branches: [ master ]

    jobs:
      build:
        runs-on: ubuntu-latest
        steps:            
        - uses: actions/checkout@v2
        - uses: azure/login@v1
          with:
          creds: ${{ secrets.AZURE_CREDENTIALS }}
    ```

1. Use la acción de la CLI de Azure para cargar el código en la instancia de Blob Storage y purgar el punto de conexión de la red CDN. En cuanto a `az storage blob upload-batch`, reemplace el marcador de posición por el nombre de la cuenta de almacenamiento. El script se cargará en el contenedor `$web`. Para `az cdn endpoint purge`, reemplace los marcadores de posición por el nombre del perfil de CDN, el nombre del punto de conexión de CDN y el grupo de recursos.

    ```yaml
        - name: Upload to blob storage
        uses: azure/CLI@v1
        with:
            azcliversion: 2.0.72
            inlineScript: |
                az storage blob upload-batch --account-name <STORAGE_ACCOUNT_NAME> -d '$web' -s .
        - name: Purge CDN endpoint
        uses: azure/CLI@v1
        with:
            azcliversion: 2.0.72
            inlineScript: |
            az cdn endpoint purge --content-paths  &quot;/*&quot; --profile-name &quot;CDN_PROFILE_NAME&quot; --name &quot;CDN_ENDPOINT&quot; --resource-group &quot;RESOURCE_GROUP&quot;
    ``` 

1. Complete el flujo de trabajo agregando una acción al cierre de sesión de Azure. Este es el flujo de trabajo completado. El archivo aparecerá en la carpeta `.github/workflows` del repositorio.

    ```yaml
   name: Blob storage website CI

    on:
    push:
        branches: [ master ]
    pull_request:
        branches: [ master ]

    jobs:
    build:
        runs-on: ubuntu-latest
        steps:
        - uses: actions/checkout@v2
        - name: Azure Login
        uses: azure/login@v1
        with:
            creds: ${{ secrets.AZURE_CREDENTIALS }}    
        - name: Azure CLI script
        uses: azure/CLI@v1
        with:
            azcliversion: 2.0.72
            inlineScript: |
                az storage blob upload-batch --account-name <STORAGE_ACCOUNT_NAME> -d '$web' -s .
        - name: Azure CLI script
        uses: azure/CLI@v1
        with:
            azcliversion: 2.0.72
            inlineScript: |
            az cdn endpoint purge --content-paths  &quot;/*&quot; --profile-name &quot;CDN_PROFILE_NAME&quot; --name &quot;CDN_ENDPOINT&quot; --resource-group &quot;RESOURCE_GROUP":::

## <a name="clean-up-resources"></a>Limpieza de recursos

Cuando el repositorio y el sitio estático de Azure ya no sean necesarios, limpie los recursos que implementó eliminando el grupo de recursos y el repositorio de GitHub. 

## <a name="next-steps"></a>Pasos siguientes

> [!div class="nextstepaction"]
> [Obtener más información sobre Azure Static Web Apps](https://docs.microsoft.com/azure/static-web-apps/)