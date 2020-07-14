---
title: Implementación de plantillas de Resource Manager mediante Acciones de GitHub
description: Se describe cómo implementar plantillas de Resource Manager mediante Acciones de GitHub.
ms.topic: conceptual
ms.date: 07/02/2020
ms.openlocfilehash: 313354499901bc69ec6e00f0ba7c385065cae615
ms.sourcegitcommit: 9b5c20fb5e904684dc6dd9059d62429b52cb39bc
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 07/02/2020
ms.locfileid: "85854747"
---
# <a name="deploy-azure-resource-manager-templates-by-using-github-actions"></a>Implementación de plantillas de Azure Resource Manager mediante Acciones de GitHub

[Acciones de GitHub](https://help.github.com/en/actions) permite crear flujos de trabajo personalizados del ciclo de vida de desarrollo de software personalizados directamente en el repositorio de GitHub donde se almacenan las plantillas de Azure Resource Manager (ARM). Un [flujo de trabajo](https://help.github.com/actions/reference/workflow-syntax-for-github-actions) se define mediante un archivo YAML. Los flujos de trabajo tienen uno o más trabajos, y cada uno contiene un conjunto de pasos que realizan tareas individuales. Los pasos pueden ejecutar comandos o usar una acción. Puede crear acciones propias o usar acciones compartidas por la [comunidad de GitHub](https://github.com/marketplace?type=actions) y personalizarlas según sea necesario. En este artículo se muestra cómo usar [la acción de la CLI de Azure](https://github.com/marketplace/actions/azure-cli-action) para implementar plantillas de Resource Manager.

La acción de la CLI de Azure tiene dos acciones dependientes:

- **[Desprotección](https://github.com/marketplace/actions/checkout)** : desproteja el repositorio para que el flujo de trabajo pueda acceder a cualquier plantilla de Resource Manager especificada.
- **[Inicio de sesión de Azure](https://github.com/marketplace/actions/azure-login)** : inicie sesión con las credenciales de Azure.

Un flujo de trabajo básico para implementar una plantilla de Resource Manager puede tener tres pasos:

1. Extraer del repositorio un archivo de plantilla
2. Inicie sesión en Azure.
3. Implementar la plantilla de Resource Manager

## <a name="prerequisites"></a>Requisitos previos

Necesita un repositorio de GitHub para almacenar las plantillas de Resource Manager y los archivos del flujo de trabajo. Para crear uno, vea [Creación de un repositorio](https://help.github.com/en/enterprise/2.14/user/articles/creating-a-new-repository).

## <a name="configure-deployment-credentials"></a>Configuración de las credenciales de implementación

La acción de inicio de sesión de Azure usa una entidad de servicio para autenticarse en Azure. La entidad de seguridad de un flujo de trabajo de CI/CD normalmente necesita el derecho de colaborador integrado para implementar recursos de Azure.

En el siguiente script de la CLI de Azure se muestra cómo generar una entidad de servicio de Azure con permisos de colaborador en un grupo de recursos de Azure. Este grupo de recursos es donde el flujo de trabajo implementa los recursos definidos en la plantilla de Resource Manager.

```azurecli
$projectName="[EnterAProjectName]"
$location="centralus"
$resourceGroupName="${projectName}rg"
$appName="http://${projectName}"
$scope=$(az group create --name $resourceGroupName --location $location --query 'id')
az ad sp create-for-rbac --name $appName --role Contributor --scopes $scope --sdk-auth
```

Personalice el valor de **$projectName** y **$location** en el script. El nombre del grupo de recursos es el nombre del proyecto con **rg** anexado. Tendrá que especificar el nombre del grupo de recursos en el flujo de trabajo.

El script genera un objeto JSON similar al siguiente:

```json
{
   "clientId": "<GUID>",
   "clientSecret": "<GUID>",
   "subscriptionId": "<GUID>",
   "tenantId": "<GUID>",
   (...)
}
```

Copie la salida JSON y almacénela como un secreto de GitHub en el repositorio de GitHub. Vea [Requisito previo](#prerequisites) si todavía no tiene un repositorio.

1. Desde el repositorio de GitHub, seleccione la pestaña **Settings** (Configuración).
1. Seleccione **Secrets** (Secretos) en el panel de la izquierda.
1. Escriba los siguientes valores:

    - **Name**: AZURE_CREDENTIALS
    - **Valor**: (Pegue la salida JSON)
1. Seleccione **Add secret** (Agregar secreto).

Debe especificar el nombre del secreto en el flujo de trabajo.

## <a name="add-resource-manager-template"></a>Adición de la plantilla de Resource Manager

Agregue una plantilla de Resource Manager al repositorio de GitHub. Si no tiene una plantilla, puede usar la siguiente. La plantilla crea una cuenta de almacenamiento.

```url
https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/101-storage-account-create/azuredeploy.json
```

Puede colocar el archivo en cualquier parte del repositorio. En el ejemplo de flujo de trabajo de la sección siguiente se supone que el archivo de plantilla se denomina **azuredeploy.json** y se almacena en una carpeta denominada **templates** en la raíz del repositorio.

## <a name="create-workflow"></a>Creación del flujo de trabajo

El archivo de flujo de trabajo se debe almacenar en la carpeta **.github/workflows** en la raíz del repositorio. La extensión de archivo de flujo de trabajo puede ser **.yml** o **.yaml**.

Puede crear un archivo de flujo de trabajo y, después, insertarlo o cargarlo en el repositorio, o bien usar el procedimiento siguiente:

1. En el repositorio de GitHub, seleccione **Actions** (Acciones) en el menú superior.
1. Seleccione **New workflow** (Nuevo flujo de trabajo).
1. Seleccione **Set up a workflow yourself** (Configurar un flujo de trabajo personalmente).
1. Cambie el nombre del archivo de flujo de trabajo si prefiere otro distinto a **main.yml**. Por ejemplo: **deployStorageAccount.yml**.
1. Reemplace el contenido del archivo .yml por lo siguiente:

    ```yml
    name: Deploy ARM Template

    on:
      push:
        branches:
          - master
        paths:
          - ".github/workflows/deployStorageAccount.yml"
          - "templates/azuredeploy.json"

    jobs:
      deploy-storage-account-template:
        runs-on: ubuntu-latest
        steps:
          - name: Checkout source code
            uses: actions/checkout@master

          - name: Login to Azure
            uses: azure/login@v1
            with:
              creds: ${{ secrets.AZURE_CREDENTIALS }}


          - name: Deploy ARM Template
            uses: azure/CLI@v1
            with:
              inlineScript: |
                az deployment group create --resource-group myResourceGroup --template-file ./templates/azuredeploy.json
    ```

    El archivo de flujo de trabajo tiene tres secciones:

    - **name**: El nombre del flujo de trabajo.
    - **on**: el nombre de los eventos de GitHub que desencadenan el flujo de trabajo. El flujo de trabajo se desencadena cuando hay un evento de envío en la rama principal, que modifica al menos uno de los dos archivos especificados. Los dos archivos son el de trabajo y el de plantilla.

        > [!IMPORTANT]
        > Compruebe que los dos archivos y sus rutas de acceso coinciden con los suyos.
    - **jobs**: una ejecución de flujo de trabajo se compone de uno o varios trabajos. Solo hay un trabajo denominado **deploy-storage-account-template**.  Este trabajo consta de tres pasos:

        - **Desprotección del código fuente**.
        - **Inicio de sesión en Azure**.

            > [!IMPORTANT]
            > Compruebe que el nombre del secreto coincide con el que ha guardado en el repositorio. Vea [Configuración de las credenciales de implementación](#configure-deployment-credentials).
        - **Implementación de la plantilla ARM**. Reemplace el valor de **resourceGroupName**.  Si ha usado el script de la CLI de Azure de [Configuración de las credenciales de implementación](#configure-deployment-credentials), el nombre del grupo de recursos generado es el nombre del proyecto al que se le ha anexado **rg**. Compruebe el valor de **templateLocation**.

1. Seleccione **Start commit** (Iniciar confirmación).
1. Seleccione **Commit directly to the master branch** (Confirmar directamente en la rama principal).
1. Seleccione **Commit new file** (Confirmar nuevo archivo) (o bien **Commit changes** (Confirmar cambios)).

Como el flujo de trabajo está configurado para que lo desencadene el archivo de flujo de trabajo o el de plantilla que se va a actualizar, el flujo de trabajo se inicia inmediatamente después de confirmar los cambios.

## <a name="check-workflow-status"></a>Comprobación del estado del flujo de trabajo

1. Seleccione la pestaña **Actions** (Acciones). Debería ver un flujo de trabajo **Create deployStorageAccount.yml** (Crear deployStorageAccount.yml) en la lista. El flujo de trabajo tarda un par de minutos en ejecutarse.
1. Seleccione el flujo de trabajo para abrirlo.
1. Seleccione **deploy-storage-account-template** (nombre del trabajo) en el menú de la izquierda. El nombre del trabajo se define en el flujo de trabajo.
1. Seleccione **Deploy ARM Template** (Implementar plantilla de ARM) (nombre del paso) para expandirla. Puede ver la respuesta de la API REST.

## <a name="next-steps"></a>Pasos siguientes

Para obtener un tutorial paso a paso que le guíe en el proceso de creación de una plantilla, consulte [Tutorial: Creación e implementación de su primera plantilla de Resource Manager](template-tutorial-create-first-template.md).
