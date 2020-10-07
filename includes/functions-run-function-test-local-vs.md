---
author: ggailey777
ms.service: azure-functions
ms.topic: include
ms.date: 03/06/2020
ms.author: glenga
ms.openlocfilehash: b4b2409928b6a4196738c7cc6c7040e781d34686
ms.sourcegitcommit: eb6bef1274b9e6390c7a77ff69bf6a3b94e827fc
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/05/2020
ms.locfileid: "80056605"
---
1. Para ejecutar la función, presione F5 en Visual Studio. Es preciso habilitar una excepción de firewall para que las herramientas para controlen las solicitudes de HTTP. Los niveles de autorización nunca se aplican cuando se ejecuta una función localmente.

2. Copie la dirección URL de la función de los resultados del runtime de Azure Functions.

    ![Runtime local de Azure](./media/functions-run-function-test-local-vs/functions-debug-local-vs.png)

3. Pegue la dirección URL de la solicitud HTTP en la barra de direcciones del explorador. Agregue la cadena de consulta `?name=<YOUR_NAME>` a esta dirección URL y ejecute la solicitud. En la siguiente imagen se muestra la respuesta en el explorador para la solicitud GET local devuelta por la función: 

    ![Respuesta de localhost de la función en el explorador](./media/functions-run-function-test-local-vs/functions-run-browser-local-vs.png)

4. Para detener la depuración, presione Mayús+F5 en Visual Studio.