---
author: ggailey777
ms.service: azure-functions
ms.topic: include
ms.date: 07/05/2019
ms.author: glenga
ms.openlocfilehash: 5e1a2622df0038141dd5cb05237f93d5e33e0bfb
ms.sourcegitcommit: c2065e6f0ee0919d36554116432241760de43ec8
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/26/2020
ms.locfileid: "78190929"
---
En un proyecto de biblioteca de clases de C#, los enlaces se definen como atributos de enlace en el método de función. Después se genera automáticamente el archivo *function.json*, que necesita Functions, en función de estos atributos.

Abra el archivo de proyecto *HttpExample.cs* y agregue el parámetro siguiente a la definición del método `Run`:

:::code language="csharp" source="~/functions-docs-csharp/functions-add-output-binding-storage-queue-cli/HttpExample.cs" range="17":::

El parámetro `msg` es de tipo `ICollector<T>`, que representa una colección de mensajes escritos en un enlace de salida cuando se completa la función. En este caso, la salida es una cola de almacenamiento denominada `outqueue`. La cadena de conexión de la cuenta de Storage la establece `StorageAccountAttribute`. Este atributo indica la configuración que contiene la cadena de conexión de la cuenta de Storage y se puede aplicar en el nivel de clase, método o parámetro. En este caso, puede omitir `StorageAccountAttribute`, puesto que ya está usando la cuenta de almacenamiento predeterminada.

La definición del método Run debe ahora parecerse a la siguiente:  

:::code language="csharp" source="~/functions-docs-csharp/functions-add-output-binding-storage-queue-cli/HttpExample.cs" range="14-18":::
