---
title: Utilización de la referencia de la plantilla
description: Use la referencia de plantilla de Azure Resource Manager para crear una plantilla.
author: mumian
ms.date: 03/27/2020
ms.topic: tutorial
ms.author: jgao
ms.custom: seodec18
ms.openlocfilehash: b742982121a20a2b057eba4211584b0386dde411
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/27/2020
ms.locfileid: "80373181"
---
# <a name="tutorial-utilize-the-arm-template-reference"></a>Tutorial: Utilización de la referencia de la plantilla de Resource Manager

Aprenda a encontrar la información del esquema de la plantilla y a usar esa información para crear plantillas de Azure Resource Manager.

En este tutorial se usa una plantilla base de las plantillas de inicio rápido de Azure. Use la documentación de referencia de la plantilla para personalizar la plantilla.

![Cuenta de almacenamiento de la implementación de referencia de plantilla de Resource Manager](./media/template-tutorial-use-template-reference/resource-manager-template-tutorial-deploy-storage-account.png)

En este tutorial se describen las tareas siguientes:

> [!div class="checklist"]
> * Abra una plantilla de inicio rápido.
> * Descripción de la plantilla
> * Búsqueda de la referencia de la plantilla
> * Edición de la plantilla
> * Implementación de la plantilla

Si no tiene una suscripción a Azure, cree una [cuenta gratuita](https://azure.microsoft.com/free/) antes de empezar.

## <a name="prerequisites"></a>Prerrequisitos

Para completar este artículo, necesitará lo siguiente:

* Visual Studio Code con la extensión Resource Manager Tools. Consulte [Uso de Visual Studio Code para la creación de plantillas de Resource Manager](use-vs-code-to-create-template.md).

## <a name="open-a-quickstart-template"></a>Abra una plantilla de inicio rápido.

[Plantillas de inicio rápido de Azure](https://azure.microsoft.com/resources/templates/) es un repositorio de plantillas de Azure Resource Manager. En lugar de crear una plantilla desde cero, puede buscar una plantilla de ejemplo y personalizarla. La plantilla usada en esta guía de inicio rápido se denomina [Crear una cuenta de almacenamiento estándar](https://azure.microsoft.com/resources/templates/101-storage-account-create/). La plantilla define un recurso de la cuenta de almacenamiento de Azure.

1. En Visual Studio Code, seleccione **Archivo**>**Abrir archivo**.
1. En **Nombre de archivo**, pegue el código URL siguiente:

    ```url
    https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/101-storage-account-create/azuredeploy.json
    ```

1. Seleccione **Abrir** para abrir el archivo.
1. Seleccione **Archivo**>**Guardar como** para guardar el archivo como **azuredeploy.json** en el equipo local.

## <a name="understand-the-schema"></a>Información sobre el esquema

1. En VS Code, contraiga la plantilla al nivel raíz. Tiene la estructura más sencilla con los siguientes elementos:

    ![La estructura más sencilla de la plantilla de Resource Manager](./media/template-tutorial-use-template-reference/resource-manager-template-simplest-structure.png)

    * **$schema**: especifique la ubicación del archivo de esquema JSON que describe la versión del lenguaje de plantilla.
    * **contentVersion**: especifique cualquier valor para este elemento para documentar cambios importantes en la plantilla.
    * **parámetros**: especifique los valores que se proporcionan cuando se ejecuta la implementación para personalizar la implementación de recursos.
    * **variables**: especifique los valores que se usan como fragmentos JSON en la plantilla para simplificar las expresiones de lenguaje de plantilla.
    * **recursos**: especifique los tipos de recursos que se implementan o actualizan en un grupo de recursos.
    * **salidas**: especifique los valores que se devuelven después de la implementación.

1. Expanda **recursos**. Hay un recurso `Microsoft.Storage/storageAccounts` definido.

    ![Definición de cuenta de almacenamiento de la plantilla de Resource Manager](./media/template-tutorial-use-template-reference/resource-manager-template-storage-resource.png)

## <a name="find-the-template-reference"></a>Búsqueda de la referencia de la plantilla

1. Vaya a la [referencia sobre las plantillas de Azure](https://docs.microsoft.com/azure/templates/).
1. En el cuadro**Filter by title** (Filtrar por título), escriba **storage accounts** (cuentas de almacenamiento) y seleccione la primera **cuenta de almacenamiento** en **Reference > Storage** (Referencia > Almacenamiento).

    ![Cuenta de almacenamiento de referencia de plantilla de Resource Manager](./media/template-tutorial-use-template-reference/resource-manager-template-resources-reference-storage-accounts.png)

    Lo habitual es que un proveedor de recursos tenga varias versiones de la API:

    ![Versiones de la cuenta de almacenamiento de la referencia de la plantilla de Resource Manager](./media/template-tutorial-use-template-reference/resource-manager-template-resources-reference-storage-accounts-versions.png)

1. Seleccione **Todos los recursos** en **Almacenamiento** en el panel izquierdo. En esta página se enumeran los tipos de recursos y las versiones del proveedor de recursos de almacenamiento. Se recomienda usar las versiones más recientes de las API para los tipos de recursos definidos en la plantilla.

    ![Versiones de los tipos de cuenta de almacenamiento de referencia de la plantilla de Resource Manager](./media/template-tutorial-use-template-reference/resource-manager-template-resources-reference-storage-accounts-types-versions.png)

1. Seleccione la versión más reciente del tipo de recurso **storageAccount**.  En el momento en que se ha escrito el artículo, la versión más reciente es la **2019-06-01**.

1. En esta página se enumeran los detalles del tipo de recurso storageAccount.  Por ejemplo, enumera los valores permitidos para el objeto **Sku**. Hay más sku de las que aparecen en la lista de la plantilla de inicio rápido que abrió antes. La plantilla de inicio rápido se puede personalizar para incluir todos los tipos de almacenamiento disponibles.

    ![SKU de la cuenta de almacenamiento de referencia de la plantilla de Resource Manager](./media/template-tutorial-use-template-reference/resource-manager-template-resources-reference-storage-accounts-skus.png)

## <a name="edit-the-template"></a>Edición de la plantilla

En Visual Studio Code, agregue los tipos de cuenta de almacenamiento adicionales como se muestra en la siguiente captura:

![Recursos de la cuenta de almacenamiento de la plantilla de Resource Manager](./media/template-tutorial-use-template-reference/resource-manager-template-storage-resources-skus.png)

## <a name="deploy-the-template"></a>Implementación de la plantilla

Consulte la sección [Implementación de la plantilla](quickstart-create-templates-use-visual-studio-code.md#deploy-the-template) de la guía de inicio rápido de Visual Studio Code para el procedimiento de implementación. Al implementar la plantilla, especifique el parámetro **storageAccountType** con un valor recién agregado, por ejemplo, **Premium_ZRS**. La implementación no se realizaría si se usa la plantilla de inicio rápido original, ya que **Premium_ZRS** no era un valor permitido.

## <a name="clean-up-resources"></a>Limpieza de recursos

Cuando los recursos de Azure ya no sean necesarios, limpie los recursos que implementó eliminando el grupo de recursos.

1. En Azure Portal, seleccione **Grupos de recursos** en el menú de la izquierda.
2. Escriba el nombre del grupo de recursos en el campo **Filtrar por nombre**.
3. Seleccione el nombre del grupo de recursos.  Verá un total de seis recursos en el grupo de recursos.
4. Seleccione **Eliminar grupo de recursos** del menú superior.

## <a name="next-steps"></a>Pasos siguientes

En este tutorial, aprendió a usar la referencia de plantilla para personalizar una plantilla existente. Para información sobre cómo crear varias instancias de cuenta de almacenamiento, consulte:

> [!div class="nextstepaction"]
> [Creación de varias instancias](./template-tutorial-create-multiple-instances.md)
