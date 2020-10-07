---
author: ggailey777
ms.service: azure-functions
ms.topic: include
ms.date: 09/23/2019
ms.author: glenga
ms.openlocfilehash: 8530f4469a0c25f3c32e652e2b0752c51c28ff3f
ms.sourcegitcommit: eb6bef1274b9e6390c7a77ff69bf6a3b94e827fc
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/05/2020
ms.locfileid: "78191078"
---
Los atributos de enlace se definen directamente en el archivo function.json. Según el tipo de enlace, pueden ser necesarias propiedades adicionales. En la [configuración de salida de cola](../articles/azure-functions/functions-bindings-storage-queue-output.md#configuration) se describen los campos necesarios para un enlace de cola de Azure Storage. La extensión facilita la incorporación de enlaces al archivo function.json. 

Para crear un enlace, haga clic con el botón derecho (Ctrl + clic en macOS) en el archivo `function.json` de la carpeta HttpTrigger y elija **Agregar enlace...** Siga las indicaciones para definir las siguientes propiedades de enlace para el nuevo enlace:

| Prompt | Value | Descripción |
| -------- | ----- | ----------- |
| **Select binding direction** (Seleccionar dirección de enlace) | `out` | El enlace es un enlace de salida. |
| **Select binding with direction...** (Seleccionar enlace con dirección...) | `Azure Queue Storage` | El enlace es un enlace de cola de Azure Storage. |
| **The name used to identify this binding in your code** (Nombre identificativo del enlace en el código) | `msg` | Nombre que identifica el parámetro de enlace al que se hace referencia en el código. |
| **The queue to which the message will be sent** (Cola donde se enviará el mensaje) | `outqueue` | El nombre de la cola en la que escribe el enlace. Cuando no existe *queueName*, el enlace lo crea durante el primer uso. |
| **Select setting from "local.setting.json"** (Seleccionar configuración de "local.setting.json") | `AzureWebJobsStorage` | El nombre de una configuración de la aplicación que contiene la cadena de conexión de la cuenta de Storage. El valor `AzureWebJobsStorage` contiene la cadena de conexión de la cuenta de almacenamiento que creó con la aplicación de función. |

Se agrega un enlace a la matriz `bindings` en function.json, que debería ser similar al siguiente:

:::code language="son" source="~/functions-docs-javascript/functions-add-output-binding-storage-queue-cli/HttpExample/function.json" range="18-24":::