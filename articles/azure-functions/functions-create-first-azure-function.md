---
title: Creación de su primera función en Azure Portal
description: Obtenga información sobre cómo crear su primera función de Azure para su ejecución sin servidor mediante Azure Portal.
ms.assetid: 96cf87b9-8db6-41a8-863a-abb828e3d06d
ms.topic: how-to
ms.date: 03/26/2020
ms.custom: mvc, devcenter, cc996988-fb4f-47
ms.openlocfilehash: a96d2ede80b4c57e7b85048379a4bfb66cacfd52
ms.sourcegitcommit: 441db70765ff9042db87c60f4aa3c51df2afae2d
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 04/06/2020
ms.locfileid: "80754853"
---
# <a name="create-your-first-function-in-the-azure-portal"></a>Creación de su primera función en Azure Portal

Azure Functions permite ejecutar el código en un entorno sin servidor y sin necesidad de crear una máquina virtual (VM) ni publicar una aplicación web. En este artículo, aprenderá a usar Azure Functions para crear una función desencadenada por HTTP "Hola mundo" en Azure Portal.

[!INCLUDE [quickstarts-free-trial-note](../../includes/quickstarts-free-trial-note.md)]

Si es desarrollador de C#, debe considerar la [creación de la primera función en Visual Studio 2019](functions-create-your-first-function-visual-studio.md) en lugar de en el portal. 

## <a name="sign-in-to-azure"></a>Inicio de sesión en Azure

Inicie sesión en [Azure Portal](https://portal.azure.com) con su cuenta de Azure.

## <a name="create-a-function-app"></a>Creación de una aplicación de función

Debe tener una Function App para hospedar la ejecución de las funciones. Una aplicación de función permite agrupar funciones como una unidad lógica para facilitar la administración, la implementación, el escalado y el uso compartido de recursos.

[!INCLUDE [Create function app Azure portal](../../includes/functions-create-function-app-portal.md)]

Después, cree una función en la nueva aplicación de funciones.

## <a name="create-an-http-triggered-function"></a><a name="create-function"></a>Crear una función desencadenada por HTTP

1. Expanda la nueva aplicación de funciones, seleccione el botón **+** situado junto a **Funciones**, elija **En el portal** y luego seleccione **Continuar**.

    ![Inicio rápido de Functions para elegir una plataforma.](./media/functions-create-first-azure-function/function-app-quickstart-choose-portal.png)

1. Elija **WebHook y API** y, a continuación, seleccione **Crear**.

    ![Inicio rápido de funciones en Azure Portal.](./media/functions-create-first-azure-function/function-app-quickstart-node-webhook.png)

   Se crea una función mediante una plantilla específica del idioma para una función desencadenada por HTTP.

Ahora, puede ejecutar la nueva función mediante el envío de una solicitud HTTP.

## <a name="test-the-function"></a>Prueba de la función

1. En la nueva función, seleccione **</> Obtener la dirección URL de la función** en la parte superior. 

1. En el cuadro de diálogo **Obtener la dirección URL de la función**, seleccione **default (Function key)** [predeterminada (tecla de función)] en la lista desplegable y, a continuación, **Copiar**. 

    ![Copiar la dirección URL de la función desde Azure Portal](./media/functions-create-first-azure-function/function-app-develop-tab-testing.png)

1. Pegue la dirección URL de la función en la barra de direcciones de su explorador. Anexe el valor `&name=<your_name>` de la cadena de consulta al final de esta dirección URL y presione Entrar para ejecutar la solicitud. 

    El ejemplo siguiente muestra la respuesta en el explorador:

    ![Respuesta de la función en el explorador.](./media/functions-create-first-azure-function/function-app-browser-testing.png)

    La dirección URL de la solicitud incluye una clave que, de forma predeterminada, es necesaria para tener acceso a la función a través de HTTP.

1. Cuando se ejecuta la función, se escribe información de seguimiento en los registros. Para ver el resultado del seguimiento de la ejecución anterior, vuelva a la función en el portal y seleccione la flecha que encontrará en la parte inferior de la pantalla para expandir **Registros**.

   ![Visor de registros de las funciones en Azure Portal.](./media/functions-create-first-azure-function/function-view-logs.png)

## <a name="clean-up-resources"></a>Limpieza de recursos

[!INCLUDE [Clean-up resources](../../includes/functions-quickstart-cleanup.md)]

## <a name="next-steps"></a>Pasos siguientes

[!INCLUDE [Next steps note](../../includes/functions-quickstart-next-steps.md)]

