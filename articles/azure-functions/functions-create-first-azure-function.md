---
title: Creación de su primera función en Azure Portal
description: Obtenga información sobre cómo crear su primera función de Azure para su ejecución sin servidor mediante Azure Portal.
ms.assetid: 96cf87b9-8db6-41a8-863a-abb828e3d06d
ms.topic: how-to
ms.date: 03/26/2020
ms.custom: mvc, devcenter, cc996988-fb4f-47
ms.openlocfilehash: 030af8a289daaf03d17f8402e8d603e893657853
ms.sourcegitcommit: a8ee9717531050115916dfe427f84bd531a92341
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 05/12/2020
ms.locfileid: "83123635"
---
# <a name="create-your-first-function-in-the-azure-portal"></a>Creación de su primera función en Azure Portal

Azure Functions permite ejecutar el código en un entorno sin servidor y sin necesidad de crear una máquina virtual (VM) ni publicar una aplicación web. En este artículo, aprenderá a usar Azure Functions para crear una función de desencadenador de HTTP "Hola mundo" en Azure Portal.

[!INCLUDE [quickstarts-free-trial-note](../../includes/quickstarts-free-trial-note.md)]

Si es desarrollador de C#, debe considerar la [creación de la primera función en Visual Studio 2019](functions-create-your-first-function-visual-studio.md) en lugar de en el portal. 

## <a name="sign-in-to-azure"></a>Inicio de sesión en Azure

Inicie sesión en [Azure Portal](https://portal.azure.com) con su cuenta de Azure.

## <a name="create-a-function-app"></a>Creación de una aplicación de función

Debe tener una Function App para hospedar la ejecución de las funciones. Una aplicación de función permite agrupar funciones como una unidad lógica para facilitar la administración, la implementación, el escalado y el uso compartido de recursos.

[!INCLUDE [Create function app Azure portal](../../includes/functions-create-function-app-portal.md)]

Después, cree una función en la nueva aplicación de funciones.

## <a name="create-an-http-trigger-function"></a><a name="create-function"></a>Creación de una función de desencadenador de HTTP

1. En el menú de la izquierda de la ventana **Funciones**, seleccione **Funciones** y, a continuación, seleccione **Agregar** en el menú superior. 
 
1. En la ventana **Nueva función**, seleccione **Desencadenador de HTTP**.

    ![Elección de la función de desencadenador de HTTP](./media/functions-create-first-azure-function/function-app-select-http-trigger.png)

1. En la ventana **Nueva función**, acepte el nombre predeterminado para la **nueva función** o escriba un nombre nuevo. 

1. En la lista desplegable **Nivel de autorización**, elija **Anónimo** y, a continuación, seleccione **Crear función**.

    Azure crea la función de desencadenador de HTTP. Ahora, puede ejecutar la nueva función mediante el envío de una solicitud HTTP.

## <a name="test-the-function"></a>Prueba de la función

1. En la nueva función de desencadenador de HTTP, seleccione **Código y prueba** en el menú de la izquierda y, a continuación, seleccione **Obtener la dirección URL de la función** en el menú superior.

    ![Selección de la opción Obtener la dirección URL de la función](./media/functions-create-first-azure-function/function-app-select-get-function-url.png)

1. En el cuadro de diálogo **Obtener la dirección URL de la función**, seleccione **valor predeterminado** en la lista desplegable y, a continuación, seleccione el icono **Copiar al Portapapeles**. 

    ![Copiar la dirección URL de la función desde Azure Portal](./media/functions-create-first-azure-function/function-app-develop-tab-testing.png)

1. Pegue la dirección URL de la función en la barra de direcciones de su explorador. Anexe el valor `?name=<your_name>` de la cadena de consulta al final de esta dirección URL y presione Entrar para ejecutar la solicitud. 

    El ejemplo siguiente muestra la respuesta en el explorador:

    ![Respuesta de la función en el explorador.](./media/functions-create-first-azure-function/function-app-browser-testing.png)

    La dirección URL de la solicitud incluye una clave que, de forma predeterminada, es necesaria para tener acceso a la función a través de HTTP.

1. Cuando se ejecuta la función, se escribe información de seguimiento en los registros. Para ver los resultados del seguimiento, vuelva a la página **Código y prueba** en el portal y expanda la flecha **Registros** en la parte inferior de la página.

   ![Visor de registros de las funciones en Azure Portal.](./media/functions-create-first-azure-function/function-view-logs.png)

## <a name="clean-up-resources"></a>Limpieza de recursos

[!INCLUDE [Clean-up resources](../../includes/functions-quickstart-cleanup.md)]

## <a name="next-steps"></a>Pasos siguientes

[!INCLUDE [Next steps note](../../includes/functions-quickstart-next-steps.md)]

