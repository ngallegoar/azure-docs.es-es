---
title: Creación de una función con Microsoft Azure for Students Starter
description: Obtenga información sobre cómo crear una función de Azure en una suscripción a Microsoft Azure for Students Starter.
Customer intent: As a student, I want to be able to create an HTTP triggered Function App within the Student Starter plan so that I can easily add APIs to any project.
author: alexkarcher-msft
ms.topic: how-to
ms.date: 04/29/2020
ms.author: alkarche
ms.openlocfilehash: ffb6378d3dc4cc3fb23ea62157aad393d8ae6642
ms.sourcegitcommit: a8ee9717531050115916dfe427f84bd531a92341
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 05/12/2020
ms.locfileid: "83122907"
---
# <a name="create-a-function-using-azure-for-students-starter"></a>Creación de una función con Microsoft Azure for Students Starter

En este tutorial, vamos a crear una función HTTP de "Hola mundo" en una suscripción a Microsoft Azure for Students Starter. También se ofrecerá información sobre qué se encuentra disponible en Azure Functions con este tipo de suscripción.

Microsoft *Azure for Students Starter* le inicia en el uso de los productos de Azure que necesita para desarrollar soluciones en la nube sin costo alguno. [Obtenga más información aquí sobre esta oferta](https://azure.microsoft.com/offers/ms-azr-0144p/).

Azure Functions permite ejecutar el código en un entorno [sin servidor](https://azure.microsoft.com/solutions/serverless/) sin necesidad de crear una máquina virtual o publicar una aplicación web. [Obtenga más información aquí sobre Azure Functions](./functions-overview.md).

[!INCLUDE [quickstarts-free-trial-note](../../includes/quickstarts-free-trial-note.md)]

## <a name="create-a-function"></a>Creación de una función

 En este artículo, aprenderá a usar Azure Functions para crear una función de desencadenador HTTP "Hola mundo" en Azure Portal.

![Creación de una aplicación de función en Azure Portal](./media/functions-create-student-starter/function-app-in-portal-editor.png)

## <a name="sign-in-to-azure"></a>Inicio de sesión en Azure

Inicie sesión en [Azure Portal](https://portal.azure.com) con su cuenta de Azure.

## <a name="create-a-function-app"></a>Creación de una aplicación de función

Debe tener una Function App para hospedar la ejecución de las funciones. Una aplicación de función permite agrupar funciones como una unidad lógica para facilitar la administración, la implementación, el escalado y el uso compartido de recursos.

[!INCLUDE [Create function app Azure portal](../../includes/functions-create-function-app-portal.md)]

Después, cree una función en la nueva Function App.

## <a name="create-an-http-trigger-function"></a><a name="create-function"></a>Creación de una función desencadenada mediante HTTP

1. En el menú de la izquierda de la ventana **Funciones**, seleccione **Funciones** y, a continuación, seleccione **Agregar** en el menú superior. 
 
1. En la ventana **Nueva función**, seleccione **Desencadenador HTTP**.

    ![Elección de la función de desencadenador HTTP](./media/functions-create-student-starter/function-app-select-http-trigger.png)

1. En la ventana **Nueva función**, acepte el nombre predeterminado para la **Nueva función** o escriba un nombre nuevo. 

1. En la lista desplegable **Nivel de autorización**, elija **Anónimo** y, a continuación, seleccione **Crear función**.

    Azure crea la función de desencadenador HTTP. Ahora, puede ejecutar la nueva función mediante el envío de una solicitud HTTP.

## <a name="test-the-function"></a>Prueba de la función

1. En la nueva función de desencadenador HTTP, seleccione **Código y prueba** en el menú de la izquierda y, a continuación, seleccione **Obtener la dirección URL de la función** en el menú superior.

    ![Selección de Obtener la dirección URL de la función](./media/functions-create-student-starter/function-app-select-get-function-url.png)

1. En el cuadro de diálogo **Obtener la dirección URL de la función**, seleccione **valor predeterminado** en la lista desplegable y, a continuación, seleccione el icono **Copiar al Portapapeles**. 

    ![Copiar la dirección URL de la función desde Azure Portal](./media/functions-create-student-starter/function-app-develop-tab-testing.png)

1. Pegue la dirección URL de la función en la barra de direcciones de su explorador. Anexe el valor `?name=<your_name>` de la cadena de consulta al final de esta dirección URL y presione Entrar para ejecutar la solicitud. 

    El ejemplo siguiente muestra la respuesta en el explorador:

    ![Respuesta de la función en el explorador.](./media/functions-create-student-starter/function-app-browser-testing.png)

    La dirección URL de la solicitud incluye una clave que, de forma predeterminada, es necesaria para tener acceso a la función a través de HTTP.

1. Cuando se ejecuta la función, se escribe información de seguimiento en los registros. Para ver los resultados del seguimiento, vuelva a la página **Código y prueba** en el portal y expanda la flecha **Registros** en la parte inferior de la página.

   ![Visor de registros de las funciones en Azure Portal.](./media/functions-create-student-starter/function-view-logs.png)

## <a name="clean-up-resources"></a>Limpieza de recursos

[!INCLUDE [Clean-up resources](../../includes/functions-quickstart-cleanup.md)]

## <a name="supported-features-in-azure-for-students-starter"></a>Características admitidas en Microsoft Azure for Students Starter

En Microsoft Azure for Students Starter, puede acceder a la mayoría de las características de Azure Functions en tiempo de ejecución, con varias limitaciones fundamentales que se enumeran a continuación:

* El desencadenador HTTP es el único tipo de desencadenador compatible.
    * Se admiten todos los enlaces de entrada y salida. [Consulte la lista completa aquí](functions-triggers-bindings.md).
* Lenguajes compatibles: 
    * C# (.NET Core 2)
    * Javascript (Node.js 8 y 10)
    * F# (.NET Core 2)
    * [Consulte los lenguajes admitidos en los planes superiores aquí](supported-languages.md).
* Windows es el único sistema operativo compatible.
* El escalado está restringido a [una instancia de nivel Gratis](https://azure.microsoft.com/pricing/details/app-service/windows/) en ejecución durante un máximo de 60 minutos cada día. Podrá escalar sin servidor de 0 a 1 instancia automáticamente a medida que se recibe el tráfico HTTP, pero ninguna otra.
* Solo se admiten la [versión 2.x y las versiones posteriores](functions-versions.md) del runtime de Functions.
* Se admiten todas las herramientas de desarrolladores para editar y publicar funciones. Esto incluye VS Code, Visual Studio, la CLI de Azure y Azure Portal. Si desea usar algo distinto del portal, deberá crear primero una aplicación en el portal y, a continuación, elegirla como un destino de implementación en su herramienta preferida.

## <a name="next-steps"></a>Pasos siguientes

Ya ha terminado de crear una aplicación de funciones con una función simple de desencadenador HTTP. Ahora puede explorar herramientas locales, más lenguajes, supervisión e integraciones.

 * [Creación de la primera función mediante Visual Studio](./functions-create-your-first-function-visual-studio.md)
 * [Creación de la primera función mediante Visual Studio Code](./functions-create-first-function-vs-code.md)
 * [Guía para el desarrollador de JavaScript para Azure Functions](./functions-reference-node.md)
 * [Uso de Azure Functions para conectarse a una base de datos de Azure SQL Database](./functions-scenario-database-table-cleanup.md)
 * [Más información sobre enlaces HTTP en Azure Functions](./functions-bindings-http-webhook.md)
 * [Supervisión de Azure Functions](./functions-monitoring.md)
