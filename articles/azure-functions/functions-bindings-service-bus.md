---
title: Enlaces de Azure Service Bus en Azure Functions
description: Obtenga información sobre cómo enviar desencadenadores y enlaces de Azure Service Bus en Azure Functions.
author: craigshoemaker
ms.assetid: daedacf0-6546-4355-a65c-50873e74f66b
ms.topic: reference
ms.date: 02/19/2020
ms.author: cshoe
ms.openlocfilehash: 46660a0c8d20ab82c994a62b1c781108ea1070c1
ms.sourcegitcommit: d7008edadc9993df960817ad4c5521efa69ffa9f
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 07/08/2020
ms.locfileid: "86111208"
---
# <a name="azure-service-bus-bindings-for-azure-functions"></a>Enlaces de Azure Service Bus en Azure Functions

Azure Functions se integra con [Azure Service Bus](https://azure.microsoft.com/services/service-bus) mediante [desencadenadores y enlaces](./functions-triggers-bindings.md). La integración con Service Bus permite crear funciones que reaccionan ante y envían mensajes de cola o tema.

| Acción | Tipo |
|---------|---------|
| Ejecutar una función cuando se crea un mensaje de cola o tema de Service Bus | [Desencadenador](./functions-bindings-service-bus-trigger.md) |
| Enviar mensajes de Azure Service Bus |[Enlace de salida](./functions-bindings-service-bus-output.md) |

## <a name="add-to-your-functions-app"></a>Adición a la aplicación de Functions

### <a name="functions-2x-and-higher"></a>Functions 2.x y superiores

Para trabajar con el desencadenador y los enlaces, es necesario hacer referencia al paquete adecuado. En las bibliotecas de clases de .NET se usa el paquete NuGet, mientras que en los demás tipos de aplicaciones se emplea el conjunto de extensiones.

| Idioma                                        | Agregar mediante...                                   | Observaciones 
|-------------------------------------------------|---------------------------------------------|-------------|
| C#                                              | Instalación del [paquete NuGet], versión 4.x | |
| Script de C#, Java, JavaScript, Python, PowerShell | Registro de [conjunto de extensiones]          | Se recomienda usar la [extensión Azure Tools] con Visual Studio Code. |
| Script de C# (solo en línea en Azure Portal)         | Adición de un enlace                            | Para actualizar extensiones de enlace existentes sin tener que volver a publicar la aplicación de funciones, consulte [Actualización de las extensiones]. |

[Paquete NuGet]: https://www.nuget.org/packages/Microsoft.Azure.WebJobs.Extensions.ServiceBus/
[core tools]: ./functions-run-local.md
[conjunto de extensiones]: ./functions-bindings-register.md#extension-bundles
[Actualización de las extensiones]: ./install-update-binding-extensions-manual.md
[Extensión Azure Tools]: https://marketplace.visualstudio.com/items?itemName=ms-vscode.vscode-node-azure-pack

### <a name="functions-1x"></a>Functions 1.x

Las aplicaciones de Functions 1.x tienen automáticamente una referencia al paquete NuGet [Microsoft.Azure.WebJobs](https://www.nuget.org/packages/Microsoft.Azure.WebJobs), versión 2.x.

## <a name="next-steps"></a>Pasos siguientes

- [Ejecución de una función cuando se crea un mensaje de cola o tema de Service Bus (Desencadenador)](./functions-bindings-service-bus-trigger.md)
- [Envío de mensajes de Azure Service Bus desde Azure Functions (Enlace de salida)](./functions-bindings-service-bus-output.md)
