---
title: 'Creación de plantilla: Visual Studio Code'
description: Use Visual Studio Code y la extensión de herramientas de Azure Resource Manager para trabajar con plantillas de Resource Manager.
author: mumian
ms.date: 03/04/2019
ms.topic: quickstart
ms.author: jgao
ms.openlocfilehash: a0c80f18e9cd09b765804aaddbd178b4b3e32a9d
ms.sourcegitcommit: 7d8158fcdcc25107dfda98a355bf4ee6343c0f5c
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 04/09/2020
ms.locfileid: "80984459"
---
# <a name="quickstart-create-arm-templates-by-using-visual-studio-code"></a>Inicio rápido: Creación de plantillas de ARM mediante Visual Studio Code

Obtenga información sobre cómo usar el código de Visual Studio y la extensión de herramientas de Azure Resource Manager para crear y modificar las plantillas de Azure Resource Manager. Puede crear plantillas de Azure Resource Manager en Visual Studio Code sin la extensión, pero la extensión proporciona opciones de autocompletar que simplifican el desarrollo de la plantilla. Para entender los conceptos asociados a la implementación y administración de sus soluciones de Azure, consulte la [introducción a la implementación de plantillas](overview.md).

En este inicio rápido, implementará una cuenta de almacenamiento:

![resource manager template quickstart visual studio code diagram](./media/quickstart-create-templates-use-visual-studio-code/resource-manager-template-quickstart-vscode-diagram.png)

Si no tiene una suscripción a Azure, cree una [cuenta gratuita](https://azure.microsoft.com/free/) antes de empezar.

## <a name="prerequisites"></a>Prerrequisitos

Para completar este artículo, necesitará lo siguiente:

- [Visual Studio Code](https://code.visualstudio.com/).
- Extensión de herramientas de Resource Manager Para instalarla, siga estos pasos:

    1. Abra Visual Studio Code.
    2. Presione **CTRL + MAYÚS + X** para abrir el panel de extensiones.
    3. Busque **Herramientas de Azure Resource Manager** y seleccione **Instalar**.
    4. Seleccione **Recargar** para finalizar la instalación de la extensión.

## <a name="open-a-quickstart-template"></a>Abra una plantilla de inicio rápido.

En lugar de crear una plantilla desde cero, abra una plantilla en las [plantillas de inicio rápido de Azure](https://azure.microsoft.com/resources/templates/). Plantillas de inicio rápido de Azure es un repositorio de plantillas de Azure Resource Manager.

La plantilla usada en esta guía de inicio rápido se denomina [Crear una cuenta de almacenamiento estándar](https://azure.microsoft.com/resources/templates/101-storage-account-create/). La plantilla define un recurso de la cuenta de almacenamiento de Azure.

1. En Visual Studio Code, seleccione **Archivo**>**Abrir archivo**.
2. En **Nombre de archivo**, pegue el código URL siguiente:

    ```url
    https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/101-storage-account-create/azuredeploy.json
    ```

3. Seleccione **Abrir** para abrir el archivo.
4. Seleccione **Archivo**>**Guardar como** para guardar el archivo como **azuredeploy.json** en el equipo local.

## <a name="edit-the-template"></a>Edición de la plantilla

Para experimentar cómo se edita una plantilla mediante Visual Studio Code, agregue un elemento más a la sección `outputs` para mostrar el identificador URI.

1. Agregue una salida más a la plantilla exportada:

    ```json
    "storageUri": {
      "type": "string",
      "value": "[reference(variables('storageAccountName')).primaryEndpoints.blob]"
    }
    ```

    Cuando haya terminado, la sección outputs tendrá este aspecto:

    ```json
    "outputs": {
      "storageAccountName": {
        "type": "string",
        "value": "[variables('storageAccountName')]"
      },
      "storageUri": {
        "type": "string",
        "value": "[reference(variables('storageAccountName')).primaryEndpoints.blob]"
      }
    }
    ```

    Si ha copiado y pegado el código en Visual Studio Code, intente volver a escribir el elemento **value** para probar la funcionalidad IntelliSense de la extensión de herramientas de Resource Manager.

    ![Plantilla de Resource Manager visual studio code IntelliSense](./media/quickstart-create-templates-use-visual-studio-code/resource-manager-templates-visual-studio-code-intellisense.png)

2. Seleccione **File**>**Guardar** para guardar los cambios.

## <a name="deploy-the-template"></a>Implementación de la plantilla

Existen muchos métodos para la implementación de plantillas. En este inicio rápido se usa Azure Cloud Shell. Cloud Shell admite la CLI de Azure y Azure PowerShell. Use el selector de pestañas para elegir entre la CLI y PowerShell.

[!INCLUDE [updated-for-az](../../../includes/updated-for-az.md)]

1. Inicio de sesión en [Azure Cloud Shell](https://shell.azure.com)

2. Elija el entorno que prefiera; para ello, seleccione **PowerShell** o **Bash** en la esquina superior izquierda.  Es necesario reiniciar el shell cuando realiza el cambio.

    # <a name="cli"></a>[CLI](#tab/CLI)

    ![CLI de Cloud Shell de Azure Portal](./media/quickstart-create-templates-use-visual-studio-code/azure-portal-cloud-shell-choose-cli.png)

    # <a name="powershell"></a>[PowerShell](#tab/PowerShell)

    ![Azure portal Cloud shell PowerShell](./media/quickstart-create-templates-use-visual-studio-code/azure-portal-cloud-shell-choose-powershell.png)

    ---

3. Seleccione **Cargar/descargar archivos** y, después, seleccione **Cargar**.

    # <a name="cli"></a>[CLI](#tab/CLI)

    ![Archivo de carga de Cloud Shell de Azure Portal](./media/quickstart-create-templates-use-visual-studio-code/azure-portal-cloud-shell-upload-file.png)

    # <a name="powershell"></a>[PowerShell](#tab/PowerShell)

    ![Archivo de carga de Cloud Shell de Azure Portal](./media/quickstart-create-templates-use-visual-studio-code/azure-portal-cloud-shell-upload-file-powershell.png)

    ---

    Seleccione el archivo que guardó en la sección anterior. El nombre predeterminado es **azuredeploy.json**. Al archivo de plantilla se debe poder acceder desde el shell.

    Si lo desea, puede usar el comando **ls** y el comando **cat** para comprobar que el archivo se ha cargado correctamente.

    # <a name="cli"></a>[CLI](#tab/CLI)

    ![Archivo de lista de Cloud Shell de Azure Portal](./media/quickstart-create-templates-use-visual-studio-code/azure-portal-cloud-shell-list-file.png)

    # <a name="powershell"></a>[PowerShell](#tab/PowerShell)

    ![Archivo de lista de Cloud Shell de Azure Portal](./media/quickstart-create-templates-use-visual-studio-code/azure-portal-cloud-shell-list-file-powershell.png)

    ---
4. En Cloud Shell, ejecute los comandos siguientes. Seleccione la pestaña para mostrar el código de PowerShell o el código de la CLI.

    # <a name="cli"></a>[CLI](#tab/CLI)

    ```azurecli
    echo "Enter a project name that is used to generate resource group name:" &&
    read projectName &&
    echo "Enter the location (i.e. centralus):" &&
    read location &&
    resourceGroupName="${projectName}rg" &&
    az group create --name $resourceGroupName --location "$location" &&
    az deployment group create --resource-group $resourceGroupName --template-file "$HOME/azuredeploy.json"
    ```

    # <a name="powershell"></a>[PowerShell](#tab/PowerShell)

    ```azurepowershell
    $projectName = Read-Host -Prompt "Enter a project name that is used to generate resource group name"
    $location = Read-Host -Prompt "Enter the location (i.e. centralus)"
    $resourceGroupName = "${projectName}rg"

    New-AzResourceGroup -Name $resourceGroupName -Location "$location"
    New-AzResourceGroupDeployment -ResourceGroupName $resourceGroupName -TemplateFile "$HOME/azuredeploy.json"
    ```

    ---

    Actualice el nombre del archivo de plantilla si guarda el archivo en un nombre distinto a **azuredeploy.json**.

    En la captura de pantalla siguiente se muestra una implementación de ejemplo:

    # <a name="cli"></a>[CLI](#tab/CLI)

    ![Plantilla de implementación de Cloud Shell de Azure Portal](./media/quickstart-create-templates-use-visual-studio-code/azure-portal-cloud-shell-deploy-template.png)

    # <a name="powershell"></a>[PowerShell](#tab/PowerShell)

    ![Plantilla de implementación de Cloud Shell de Azure Portal](./media/quickstart-create-templates-use-visual-studio-code/azure-portal-cloud-shell-deploy-template-powershell.png)

    ---

    El nombre de la cuenta de almacenamiento y la dirección URL de almacenamiento en la sección outputs se resaltan en la captura de pantalla. Necesita el nombre de la cuenta de almacenamiento en el paso siguiente.

5. Ejecute el siguiente comando de la CLI o de PowerShell para enumerar la cuenta de almacenamiento recién creada:

    # <a name="cli"></a>[CLI](#tab/CLI)

    ```azurecli
    echo "Enter the Resource Group name:" &&
    read resourceGroupName &&
    echo "Enter the Storage Account name:" &&
    read storageAccountName &&
    az storage account show --resource-group $resourceGroupName --name $storageAccountName
    ```

    # <a name="powershell"></a>[PowerShell](#tab/PowerShell)

    ```azurepowershell
    $resourceGroupName = Read-Host -Prompt "Enter the Resource Group name"
    $storageAccountName = Read-Host -Prompt "Enter the Storage Account name"
    Get-AzStorageAccount -ResourceGroupName $resourceGroupName -Name $storageAccountName
    ```

    ---

Para más información sobre las cuentas de almacenamiento de Azure, consulte [Inicio rápido: Carga, descarga y enumeración de blobs mediante Azure Portal](../../storage/blobs/storage-quickstart-blobs-portal.md).

## <a name="clean-up-resources"></a>Limpieza de recursos

Cuando los recursos de Azure ya no sean necesarios, limpie los recursos que implementó eliminando el grupo de recursos.

1. En Azure Portal, seleccione **Grupos de recursos** en el menú de la izquierda.
2. Escriba el nombre del grupo de recursos en el campo **Filtrar por nombre**.
3. Seleccione el nombre del grupo de recursos.  Verá un total de seis recursos en el grupo de recursos.
4. Seleccione **Eliminar grupo de recursos** del menú superior.

## <a name="next-steps"></a>Pasos siguientes

El objetivo principal de este tutorial consiste en utilizar Visual Studio Code para editar una plantilla existente de las plantillas de inicio rápido de Azure. También ha aprendido a implementar la plantilla mediante la CLI o mediante PowerShell desde Azure Cloud Shell. Es posible que las plantillas de inicio rápido de Azure no le proporcionen todo lo que necesita. Para más información sobre el desarrollo de plantillas, consulte nuestra nueva serie de tutoriales para principiantes:

> [!div class="nextstepaction"]
> [Tutoriales para principiantes](./template-tutorial-create-first-template.md)
