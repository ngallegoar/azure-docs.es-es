---
author: ggailey777
ms.service: azure-functions
ms.topic: include
ms.date: 03/18/2020
ms.author: glenga
ms.openlocfilehash: eb54439f89cc2443eeed2d3b63dfbe7fedb4bf17
ms.sourcegitcommit: eb6bef1274b9e6390c7a77ff69bf6a3b94e827fc
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/05/2020
ms.locfileid: "80673493"
---
## <a name="update-the-tests"></a>Actualización de las pruebas

Dado que el arquetipo también crea un conjunto de pruebas, debe actualizar estas pruebas para controlar el nuevo parámetro `msg` en la signatura del método `run`.  

Vaya a la ubicación del código de prueba en _src/test/java_, abra el archivo de proyecto *Function.Java* y reemplace la línea de código debajo de `//Invoke` por el código siguiente.

:::code language="java" source="~/functions-quickstart-java/functions-add-output-binding-storage-queue/src/test/java/com/function/FunctionTest.java" range="48-50":::
