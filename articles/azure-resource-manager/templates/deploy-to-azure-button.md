---
title: Botón Implementación en Azure
description: Use el botón para implementar plantillas de Azure Resource Manager desde un repositorio de GitHub.
ms.topic: conceptual
ms.date: 02/07/2020
ms.openlocfilehash: 88436eac970b252d7b0bc7bccee4131e06e9e0cf
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/27/2020
ms.locfileid: "77109046"
---
# <a name="use-a-deployment-button-to-deploy-templates-from-github-repository"></a>Usar un botón de implementación para implementar plantillas desde el repositorio de GitHub

En este artículo se describe cómo usar el botón **Implementar en Azure** para implementar plantillas desde un repositorio de GitHub. Puede agregar el botón directamente al archivo README.md en el repositorio de GitHub o a una página web que haga referencia al repositorio.

## <a name="use-common-image"></a>Uso de una imagen común

Para agregar el botón a su página web o repositorio, use la siguiente imagen:

```html
<img src="https://aka.ms/deploytoazurebutton"/>
```

La imagen aparece como:

![Botón Implementación en Azure](https://aka.ms/deploytoazurebutton)

## <a name="create-url-for-deploying-template"></a>Crear dirección URL para implementar una plantilla

Para crear la dirección URL de la plantilla, comience con la dirección URL original de la plantilla en el repositorio:

```html
https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/101-storage-account-create/azuredeploy.json
```

A continuación, codifíquela como una URL. Puede usar un codificador en línea o ejecutar un comando. En el siguiente ejemplo de PowerShell se muestra cómo codificar un valor como una dirección URL.

```powershell
[uri]::EscapeDataString($url)
```

La dirección URL de ejemplo tiene el siguiente valor cuando se codifica como una URL.

```html
https%3A%2F%2Fraw.githubusercontent.com%2FAzure%2Fazure-quickstart-templates%2Fmaster%2F101-storage-account-create%2Fazuredeploy.json
```

Cada vínculo comienza con la misma dirección URL base:

```html
https://portal.azure.com/#create/Microsoft.Template/uri/
```

Agregue el vínculo de la plantilla con codificación URL al final de la dirección URL base.

```html
https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2FAzure%2Fazure-quickstart-templates%2Fmaster%2F101-storage-account-create%2Fazuredeploy.json
```

Ya tiene la dirección URL completa del vínculo.

## <a name="create-deploy-to-azure-button"></a>Crear el botón Implementar en Azure

Por último, coloque el vínculo y la imagen juntos.

Para agregar el botón con Markdown en el archivo README.md en el repositorio de GitHub o en una página web, use:

```markdown
[![Deploy to Azure](https://aka.ms/deploytoazurebutton)](https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2FAzure%2Fazure-quickstart-templates%2Fmaster%2F101-storage-account-create%2Fazuredeploy.json)
```

Para HTML, use:

```html
<a href="https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2FAzure%2Fazure-quickstart-templates%2Fmaster%2F101-storage-account-create%2Fazuredeploy.json" target="_blank">
  <img src="https://aka.ms/deploytoazurebutton"/>
</a>
```

## <a name="deploy-the-template"></a>Implementación de la plantilla

Para probar la solución completa, seleccione el botón siguiente:

[![Implementación en Azure](https://aka.ms/deploytoazurebutton)](https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2FAzure%2Fazure-quickstart-templates%2Fmaster%2F101-storage-account-create%2Fazuredeploy.json)

El portal muestra un panel que le permite proporcionar fácilmente los valores de parámetros. Los parámetros se rellenan previamente con los valores predeterminados de la plantilla.

![Usar el portal para realizar la implementación](./media/deploy-to-azure-button/portal.png)

## <a name="next-steps"></a>Pasos siguientes

- Para obtener más información sobre las plantillas, consulte [Nociones sobre la estructura y la sintaxis de las plantillas de Azure Resource Manager](template-syntax.md).
