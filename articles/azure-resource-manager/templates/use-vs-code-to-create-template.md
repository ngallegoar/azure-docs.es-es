---
title: Uso de Visual Studio Code para la creación de plantillas
description: Aprenda a instalar y usar Visual Studio Code y la extensión de herramientas de Azure Resource Manager.
author: mumian
ms.topic: conceptual
ms.date: 08/30/2019
ms.author: jgao
ms.openlocfilehash: 585e2773ea5d6af184f85e65b63d39b60d632146
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/28/2020
ms.locfileid: "79233980"
---
# <a name="use-visual-studio-code-to-create-azure-resource-manager-templates"></a>Uso de Visual Studio Code para crear plantillas de Azure Resource Manager

Visual Studio Code es un editor ligero, multiplataforma y de código abierto. La extensión de herramientas de plantilla de Azure Resource Manager es un complemento para el desarrollo de plantillas de Resource Manager. La extensión agrega compatibilidad con lenguajes en las plantillas a fin de proporcionarle funciones de lenguaje, como IntelliSense, resaltado de sintaxis, ayuda en línea y muchas otras. Juntos, proporcionan la experiencia de desarrollo de plantillas recomendada.

## <a name="install-visual-studio-code"></a>Instalación de Visual Studio Code

Visual Studio Code es compatible con MacOS, Windows y Linux.  Se puede instalar desde [Visual Studio Code.](https://code.visualstudio.com/)

## <a name="install-resource-manager-tools-extension"></a>Instalación de la extensión Herramientas de Resource Manager

1. Abra Visual Studio Code.
1. Seleccione **Extensiones** en el menú izquierdo. O bien, en el menú **Ver**, seleccione **Extensiones** para abrir el panel de extensiones.

    ![Instalación de Visual Studio Code con la extensión Herramientas de Resource Manager](./media/use-vs-code-to-create-template/resource-manager-visual-studio-code-tools-extension.png)
1. Busque **Resource Manager**.
1. Seleccione **Instalar** en **Herramientas de Azure Resource Manager**.

## <a name="the-extension-features"></a>Características de la extensión

### <a name="colorization-for-template-language-expressions"></a>Coloreado de las expresiones de lenguaje de plantilla

Los parámetros, las variables, las funciones, los nombres y las expresiones están codificados por colores, tal como se muestra en la siguiente captura de pantalla:

![Extensiones de herramientas de Resource Manager de Visual Studio Code: coloreado](./media/use-vs-code-to-create-template/resource-manager-tools-extension-colorization.png)

La vista del esquema de plantilla facilita la navegación por plantillas de gran tamaño.

### <a name="intellisense"></a>IntelliSense

La extensión de plantilla de Resource Manager conoce las posibles finalizaciones de nombres de función, parámetros, variables y referencias. Las sugerencias de IntelliSense emergen a medida que escribe. Si sigue escribiendo caracteres, la lista de miembros (variables, métodos, etc.) se filtra para incluir solo los miembros que contienen los caracteres escritos. Al presionar la tecla **Tab** o **Entrar** se inserta el miembro seleccionado.

- Nombres de funciones integradas

    ![Extensiones de herramientas de Resource Manager de Visual Studio Code: funciones de IntelliSense](./media/use-vs-code-to-create-template/resource-manager-tools-extension-intellisense-functions.png)

- Referencias de parámetros

    ![Extensiones de herramientas de Resource Manager de Visual Studio Code: parámetros de IntelliSense](./media/use-vs-code-to-create-template/resource-manager-tools-extension-intellisense-parameters.png)

- Referencias de variables

    ![Extensiones de herramientas de Resource Manager de Visual Studio Code: variables de IntelliSense](./media/use-vs-code-to-create-template/resource-manager-tools-extension-intellisense-variables.png)

- Propiedades resourceGroup()

    ![Extensiones de herramientas de Resource Manager de Visual Studio Code: funciones de IntelliSense](./media/use-vs-code-to-create-template/resource-manager-tools-extension-intellisense-resourcegroup.png)

Además, IntelliSense también funciona con propiedades suscription() y las propiedades de referencias a variables que son objetos.

### <a name="signature-help-for-function-parameters"></a>Ayuda para la signatura de los parámetros de función

Al mantener el mouse sobre los nombres de función, la extensión muestra la ayuda de la signatura de los parámetros de función.

![Extensiones de herramientas de Resource Manager de Visual Studio Code: función de signatura](./media/use-vs-code-to-create-template/resource-manager-tools-extension-signature-function.png)

### <a name="go-to-definition-for-variable-and-parameter-references"></a>Ir a la definición para referencias de variables y parámetros

Puede saltar a la definición con **Ctrl+clic** o mediante el menú contextual, tal como se muestra en la captura de pantalla: ![Extensiones de herramientas de Resource Manager de Visual Studio Code: ir a la definición](./media/use-vs-code-to-create-template/resource-manager-tools-extension-context-menu.png)

Puede abrir la definición al lado con **Ctrl+Alt+clic.**

### <a name="peek-for-variable-and-parameter-definitions"></a>Inspeccionar las definiciones de variables y parámetros

Para abrir el editor interactivo, use el menú contextual como se muestra en la captura de pantalla anterior.

En la captura de pantalla siguiente se muestra el editor interactivo:

![Extensiones de herramientas de Resource Manager de Visual Studio Code: editor interactivo](./media/use-vs-code-to-create-template/resource-manager-tools-extension-peek-editor.png)

### <a name="find-all-references-for-variables-and-parameters"></a>Buscar todas las referencias de variables y parámetros

Para buscar todas las referencias, use el menú contextual como se muestra en la captura de pantalla anterior.

En la captura de pantalla siguiente se muestra cómo se resaltan las referencias:

![Extensiones de herramientas de Resource Manager de Visual Studio Code: búsqueda de todas las referencias](./media/use-vs-code-to-create-template/resource-manager-tools-extension-find-all-references.png)

### <a name="rename-all-references-for-variables-and-parameters"></a>Cambiar el nombre de todas las referencias de variables y parámetros

Para cambiar el nombre de todas las referencias de variables y parámetros, use el menú contextual como se muestra en la captura de pantalla anterior.

### <a name="hover-for-parameter-description"></a>Mantener el puntero sobre la descripción del parámetro

![Extensiones de herramientas de Resource Manager de Visual Studio Code: mantener el puntero sobre la definición](./media/use-vs-code-to-create-template/resource-manager-tools-extension-hover-parameters.png)

### <a name="brace-matching"></a>Coincidencia de llaves

Las llaves coincidentes se resaltan en cuanto el cursor está cerca de una de ellas. Al hacer clic en una llave, la llave coincidente también se resalta como se muestra en la siguiente captura de pantalla:

![Extensiones de herramientas de Resource Manager de Visual Studio Code: coincidencia de llaves](./media/use-vs-code-to-create-template/resource-manager-tools-extension-brace-matching.png)

### <a name="show-errors-and-warnings"></a>Mostrar errores y advertencias

Los errores que se identifican mediante la extensión incluyen:

- Referencias a parámetros no definidos
- Referencias a variables no definidas
- Nombres de función no reconocidos
- Uso de la función reference() en la definición de la variable
- Número incorrecto de argumentos en las funciones

Las advertencias incluyen:

- Parámetro sin utilizar
- Variables sin utilizar

## <a name="next-steps"></a>Pasos siguientes

- Para más información sobre la creación de plantillas de Azure Resource Manager, consulte [Tutorial: Creación e implementación de la primera plantilla de Azure Resource Manager](template-tutorial-create-first-template.md).
- Para seguir una guía de inicio rápido con Visual Studio Code, consulte [Inicio rápido: Creación de plantillas de Azure Resource Manager mediante Visual Studio Code](quickstart-create-templates-use-visual-studio-code.md).
