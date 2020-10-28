---
title: Creación de un almacén de Azure App Configuration mediante una plantilla de Azure Resource Manager
titleSuffix: Azure App Configuration
description: Aprenda a crear un almacén de Azure App Configuration mediante una plantilla de Azure Resource Manager.
author: ZhijunZhao
ms.author: zhijzhao
ms.date: 10/16/2020
ms.service: azure-resource-manager
ms.topic: quickstart
ms.custom: subject-armqs
ms.openlocfilehash: feabac62564729338e41bf30eaf8d9f5a6317126
ms.sourcegitcommit: dbe434f45f9d0f9d298076bf8c08672ceca416c6
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/17/2020
ms.locfileid: "92149011"
---
# <a name="quickstart-create-an-azure-app-configuration-store-by-using-an-arm-template"></a>Inicio rápido: Creación de un almacén de Azure App Configuration mediante una plantilla de Resource Manager

En este inicio rápido se describe cómo:

- Implementar un almacén de App Configuration mediante una plantilla de Azure Resource Manager (ARM).
- Crear pares clave-valor en un almacén de App Configuration mediante la plantilla de ARM.
- Leer los pares clave-valor de un almacén de App Configuration en la plantilla de ARM.

[!INCLUDE [About Azure Resource Manager](../../includes/resource-manager-quickstart-introduction.md)]

Si su entorno cumple los requisitos previos y está familiarizado con el uso de plantillas de Resource Manager, seleccione el botón **Implementar en Azure** . La plantilla se abrirá en Azure Portal.

[![Implementación en Azure](../media/template-deployments/deploy-to-azure.svg)](https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2FAzure%2Fazure-quickstart-templates%2Fmaster%2F101-app-configuration-store-kv%2Fazuredeploy.json)

## <a name="prerequisites"></a>Requisitos previos

Si no tiene una suscripción a Azure, cree una [cuenta gratuita](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) antes de empezar.

## <a name="review-the-template"></a>Revisión de la plantilla

La plantilla usada en este inicio rápido forma parte de las [plantillas de inicio rápido de Azure](https://azure.microsoft.com/resources/templates/101-app-configuration-store-kv/). Crea un almacén de App Configuration con dos pares clave-valor dentro. Luego, utiliza la función `reference` para generar los valores de los dos recursos de clave-valor. La lectura del valor de la clave de esta manera permite que se use en otros lugares de la plantilla.

En el inicio rápido se usa el elemento `copy` para crear varias instancias del recurso de clave-valor. Para más información sobre el elemento `copy`, consulte [Iteración de recursos en las plantillas de Resource Manager](../azure-resource-manager/templates/copy-resources.md).

> [!IMPORTANT]
> Esta plantilla requiere la versión `2020-07-01-preview` o posterior del proveedor de recursos de App Configuration. Esta versión utiliza la función `reference` para leer los pares clave-valor. La función `listKeyValue` que se usó para leer los pares clave-valor en la versión anterior no está disponible a partir de la versión `2020-07-01-preview`.

:::code language="json" source="~/quickstart-templates/101-app-configuration-store-kv/azuredeploy.json":::

En la plantilla se definen dos recursos de Azure:

- [Microsoft.AppConfiguration/configurationStores](/azure/templates/microsoft.appconfiguration/2020-06-01/configurationstores): cree un almacén de App Configuration.
- Microsoft.AppConfiguration/configurationStores/keyValues: crea un valor de clave dentro del almacén de App Configuration.

> [!NOTE]
> El nombre del recurso de `keyValues` es una combinación de clave y etiqueta. La clave y la etiqueta están unidas por el delimitador `$`. La etiqueta es opcional. En el ejemplo anterior, el recurso `keyValues` con el nombre `myKey` crea un valor de clave sin etiqueta.
>
> La codificación porcentual, también conocida como codificación de direcciones URL, permite que las claves o etiquetas incluyan caracteres que no se permiten en los nombres de recursos de plantilla de Resource Manager. `%` tampoco es un carácter permitido, por lo que se usa `~` en su lugar. Para codificar correctamente un nombre, siga estos pasos:
>
> 1. Aplicación de la codificación de direcciones URL
> 2. Reemplazar `~` con `~7E`
> 3. Reemplazar `%` con `~`
>
> Por ejemplo, para crear un par clave-valor con el nombre de clave `AppName:DbEndpoint` y el nombre de etiqueta `Test`, el nombre del recurso debe ser `AppName~3ADbEndpoint$Test`.

## <a name="deploy-the-template"></a>Implementación de la plantilla

Seleccione la imagen siguiente para iniciar sesión en Azure y abrir una plantilla. La plantilla crea un almacén de App Configuration con dos pares clave-valor dentro.

[![Implementación en Azure](../media/template-deployments/deploy-to-azure.svg)](https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2FAzure%2Fazure-quickstart-templates%2Fmaster%2F101-app-configuration-store-kv%2Fazuredeploy.json)

También puede implementar la plantilla con el siguiente cmdlet de PowerShell. Los pares clave-valor estarán en la salida de la consola de PowerShell.

```azurepowershell-interactive
$projectName = Read-Host -Prompt "Enter a project name that is used for generating resource names"
$location = Read-Host -Prompt "Enter the location (i.e. centralus)"
$templateUri = "https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/101-app-configuration-store-kv/azuredeploy.json"

$resourceGroupName = "${projectName}rg"

New-AzResourceGroup -Name $resourceGroupName -Location "$location"
New-AzResourceGroupDeployment -ResourceGroupName $resourceGroupName -TemplateUri $templateUri

Read-Host -Prompt "Press [ENTER] to continue ..."
```

## <a name="review-deployed-resources"></a>Revisión de los recursos implementados

1. Inicie sesión en [Azure Portal](https://portal.azure.com).
1. En el cuadro de búsqueda de Azure Portal, escriba **App Configuration** . En la lista, seleccione **App Configuration** .
1. Seleccione el recurso de App Configuration recién creado.
1. En **Operaciones** , haga clic en **Explorador de configuración** .
1. Compruebe que existen dos pares clave-valor.

## <a name="clean-up-resources"></a>Limpieza de recursos

Cuando ya no los necesite, elimine el grupo de recursos, el almacén de App Configuration y todos los recursos relacionados. Si tiene pensado usar el almacén de App Configuration en el futuro, puede conservarlo. Si no va a seguir usando este almacén, ejecute el siguiente cmdlet para eliminar todos los recursos creados en este inicio rápido:

```azurepowershell-interactive
$resourceGroupName = Read-Host -Prompt "Enter the Resource Group name"
Remove-AzResourceGroup -Name $resourceGroupName
Write-Host "Press [ENTER] to continue..."
```

## <a name="next-steps"></a>Pasos siguientes

Para obtener información acerca de cómo crear otras aplicaciones con Azure App Configuration, continúe con el siguiente artículo:

> [!div class="nextstepaction"]
> [Inicio rápido: Creación de una aplicación ASP.NET Core con Azure App Configuration](quickstart-aspnet-core-app.md)
