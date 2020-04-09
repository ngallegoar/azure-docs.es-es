---
author: ggailey777
ms.service: azure-functions
ms.topic: include
ms.date: 03/25/2020
ms.author: glenga
ms.openlocfilehash: 121f6ffa5c1a7c903e59be8a5bc3e1e1db0834fc
ms.sourcegitcommit: b129186667a696134d3b93363f8f92d175d51475
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 04/06/2020
ms.locfileid: "80673348"
---
## <a name="add-an-output-binding-definition-to-the-function"></a>Incorporación de una definición de enlace de salida a la función

Aunque ninguna de las funciones puede tener más de un desencadenador, puede tener varios enlaces de entrada y salida que le permiten conectarse a otros servicios y recursos de Azure sin escribir código de integración personalizado. 

::: zone pivot="programming-language-python,programming-language-javascript,programming-language-powershell,programming-language-typescript"  
Estos enlaces se declaran en el archivo *function.json* en la carpeta de la función. En el inicio rápido anterior, el archivo *function.json* de la carpeta *HttpExample* contiene dos enlaces en la colección `bindings`:  
::: zone-end

::: zone pivot="programming-language-javascript,programming-language-typescript"  
:::code language="json" source="~/functions-quickstart-templates/Functions.Templates/Templates/HttpTrigger-JavaScript/function.json" range="2-18":::  
::: zone-end

::: zone pivot="programming-language-python"  
:::code language="json" source="~/functions-quickstart-templates/Functions.Templates/Templates/HttpTrigger-Python/function.json" range="2-18":::  
::: zone-end

::: zone pivot="programming-language-powershell"  
:::code language="json" source="~/functions-quickstart-templates/Functions.Templates/Templates/HttpTrigger-PowerShell/function.json" range="2-18":::
::: zone-end  

::: zone pivot="programming-language-python,programming-language-javascript, programming-language-powershell, programming-language-typescript"  
Cada enlace tiene al menos un tipo, una dirección y un nombre. En el ejemplo anterior, el primer enlace es del tipo `httpTrigger` con la dirección `in`. En el caso de la dirección `in`, `name` especifica el nombre de un parámetro de entrada que se envía a la función cuando la invoca el desencadenador.  
::: zone-end

::: zone pivot="programming-language-javascript,programming-language-typescript"  
El segundo enlace de la colección se denomina `res`. Este enlace `http` es un enlace de salida (`out`) que se usa para escribir la respuesta HTTP. 

Para escribir en una cola de Azure Storage desde esta función, agregue un enlace `out` del tipo `queue` con el nombre `msg`, como se muestra en el código siguiente:

:::code language="json" source="~/functions-docs-javascript/functions-add-output-binding-storage-queue-cli/HttpExample/function.json" range="3-26":::
::: zone-end  

::: zone pivot="programming-language-python"  
El segundo enlace de la colección es de tipo `http` con la dirección `out`, en cuyo caso la variable `name` especial de `$return` indica que este enlace usa el valor devuelto de la función, en lugar de proporcionar un parámetro de entrada.

Para escribir en una cola de Azure Storage desde esta función, agregue un enlace `out` del tipo `queue` con el nombre `msg`, como se muestra en el código siguiente:

:::code language="json" source="~/functions-docs-python/functions-add-output-binding-storage-queue-cli/HttpExample/function.json" range="3-26":::
::: zone-end  

::: zone pivot="programming-language-powershell"  
El segundo enlace de la colección se denomina `res`. Este enlace `http` es un enlace de salida (`out`) que se usa para escribir la respuesta HTTP. 

Para escribir en una cola de Azure Storage desde esta función, agregue un enlace `out` del tipo `queue` con el nombre `msg`, como se muestra en el código siguiente:

:::code language="json" source="~/functions-docs-powershell/functions-add-output-binding-storage-queue-cli/HttpExample/function.json" range="3-26":::
::: zone-end  

::: zone pivot="programming-language-python,programming-language-javascript,programming-language-powershell,programming-language-typescript"  
En este caso, `msg` se asigna a la función como argumento de salida. En el caso de un tipo `queue`, también debe especificar el nombre de la cola en `queueName` y proporcionar el *nombre* de la conexión Azure Storage (desde *local.settings.json*) en `connection`. 
::: zone-end  
