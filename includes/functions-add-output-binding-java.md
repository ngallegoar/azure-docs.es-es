---
author: ggailey777
ms.service: azure-functions
ms.topic: include
ms.date: 03/18/2020
ms.author: glenga
ms.openlocfilehash: c27f3fea6d2dd8b891220ba06b375b33e6cd950b
ms.sourcegitcommit: b129186667a696134d3b93363f8f92d175d51475
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 04/06/2020
ms.locfileid: "80673485"
---
En un proyecto de Java, los enlaces se definen como anotaciones de enlace en el método de función. Entonces, el archivo *function.json* se genera automáticamente en función de estas anotaciones.

Vaya a la ubicación del código de función en _src/main/java_, abra el archivo de proyecto *Function.java* y agregue el parámetro siguiente a la definición del método `run`:

:::code language="java" source="~/functions-quickstart-java/functions-add-output-binding-storage-queue/src/main/java/com/function/Function.java" range="20-21":::

El parámetro `msg` es de tipo [`OutputBinding<T>`](/java/api/com.microsoft.azure.functions.outputbinding), que representa una colección de cadenas escritas como mensajes en un enlace de salida cuando se completa la función. En este caso, la salida es una cola de almacenamiento denominada `outqueue`. La cadena de conexión de la cuenta de Storage la establece el método `connection`. En lugar de la propia cadena de conexión, el usuario se encarga de pasar la configuración de la aplicación que contiene la cadena de conexión de la cuenta de Storage.

La definición del método `run` debe ahora parecerse a la del siguiente ejemplo:  

:::code language="java" source="~/functions-quickstart-java/functions-add-output-binding-storage-queue/src/main/java/com/function/Function.java" range="16-22":::