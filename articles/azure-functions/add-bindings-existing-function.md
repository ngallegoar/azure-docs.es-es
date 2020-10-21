---
title: Incorporación de enlaces a una función existente de Azure Functions
description: Obtenga información sobre cómo incorporar enlaces a una función existente en el proyecto de Azure Functions.
ms.topic: how-to
ms.date: 04/29/2020
ms.openlocfilehash: 23b7b05f9a9f9da5a48511ee555e3b6184a74179
ms.sourcegitcommit: 829d951d5c90442a38012daaf77e86046018e5b9
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/09/2020
ms.locfileid: "91654073"
---
# <a name="add-bindings-to-an-existing-function-in-azure-functions"></a>Incorporación de enlaces a una función existente de Azure Functions

Al crear una función, se agrega código del desencadenador específico del idioma en el proyecto desde un conjunto de plantillas de desencadenador. Si desea conectar la función a otros servicios mediante enlaces de entrada o salida, debe agregar definiciones de enlace específicas en la función. Para más información acerca de los enlaces, consulte [Conceptos básicos sobre los enlaces y desencadenadores de Azure Functions](functions-triggers-bindings.md).

## <a name="local-development"></a>Desarrollo local       

Al desarrollar funciones localmente, es necesario que actualice el código de función para agregar enlaces. El uso de Visual Studio Code puede facilitar la adición de enlaces a una función.  

### <a name="visual-studio-code"></a>Visual Studio Code

Al usar Visual Studio Code para desarrollar la función y utilizar esta un archivo function.json, la extensión de Azure Functions puede agregar automáticamente un enlace a un archivo function.json existente. Para obtener más información, consulte [Incorporación de enlaces de entrada y de salida](functions-develop-vs-code.md#add-input-and-output-bindings).   

### <a name="manually-add-bindings-based-on-examples"></a>Incorporación manual de enlaces basados en ejemplos

Al agregar un enlace a una función existente, tendrá que actualizar tanto el código de función como el archivo de configuración function.json, si su lenguaje los usa. Tanto la biblioteca de clases .NET como las funciones de Java usan atributos en lugar de function.json, de modo que tendrá que actualizarlo en su lugar.

Use la tabla siguiente para buscar ejemplos de tipos de enlace específicos que puede usar para que le guíen al actualizar una función existente. En primer lugar, elija la pestaña de lenguaje que corresponde al proyecto. 

[!INCLUDE [functions-bindings-code-example-chooser](../../includes/functions-bindings-code-example-chooser.md)]

## <a name="azure-portal"></a>Azure portal

Al desarrollar las funciones en [Azure Portal](https://portal.azure.com), se agregan enlaces de entrada y salida en la pestaña **Integrar** para una función determinada. Los nuevos enlaces se agregan al archivo function.json o a los atributos de método, dependiendo del lenguaje. En los siguientes artículos se muestran ejemplos de cómo agregar enlaces a una función existente en el portal:

+ [Enlace de salida de Queue Storage](functions-integrate-storage-queue-output-binding.md)
+ [Enlace de salida de Azure Cosmos DB](functions-integrate-store-unstructured-data-cosmosdb.md)

## <a name="next-steps"></a>Pasos siguientes

+ [Conceptos básicos sobre los enlaces y desencadenadores de Azure Functions](functions-triggers-bindings.md)
