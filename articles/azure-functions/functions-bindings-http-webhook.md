---
title: Enlaces y desencadenadores HTTP de Azure Functions
description: Aprenda a usar desencadenadores y enlaces HTTP en Azure Functions.
author: craigshoemaker
ms.topic: reference
ms.date: 02/14/2020
ms.author: cshoe
ms.openlocfilehash: 29b5e9c7673b4a730a41bf7cf2b1c4a2a86209ed
ms.sourcegitcommit: 829d951d5c90442a38012daaf77e86046018e5b9
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/09/2020
ms.locfileid: "77462112"
---
# <a name="azure-functions-http-triggers-and-bindings-overview"></a>Introducción a los enlaces y desencadenadores HTTP de Azure Functions

Puede invocar Azure Functions mediante solicitudes HTTP para crear API sin servidor y responder a [webhooks](https://en.wikipedia.org/wiki/Webhook).

| Acción | Tipo |
|---------|---------|
| Ejecución de una función desde una solicitud HTTP | [Desencadenador](./functions-bindings-http-webhook-trigger.md) |
| Devolución de una respuesta HTTP desde una función |[Enlace de salida](./functions-bindings-http-webhook-output.md) |

El código de este artículo tiene como valor predeterminado la sintaxis de .NET Core, que se usa en Functions versión 2.x y superiores. Para obtener información sobre la sintaxis de 1.x, consulte las [plantillas de Functions 1.x](https://github.com/Azure/azure-functions-templates/tree/v1.x/Functions.Templates/Templates).

## <a name="add-to-your-functions-app"></a>Adición a la aplicación de Functions

### <a name="functions-2x-and-higher"></a>Functions 2.x y superiores

Para trabajar con el desencadenador y los enlaces, es necesario hacer referencia al paquete adecuado. En las bibliotecas de clases de .NET se usa el paquete NuGet, mientras que en los demás tipos de aplicaciones se emplea el conjunto de extensiones.

| Idioma                                        | Agregar mediante...                                   | Observaciones 
|-------------------------------------------------|---------------------------------------------|-------------|
| C#                                              | Instalación del [paquete NuGet], versión 3.x | |
| Script de C#, Java, JavaScript, Python, PowerShell | Registro de [conjunto de extensiones]          | Se recomienda usar la [extensión Azure Tools](https://marketplace.visualstudio.com/items?itemName=ms-vscode.vscode-node-azure-pack) con Visual Studio Code. |
| Script de C# (solo en línea en Azure Portal)         | Adición de un enlace                            | Para actualizar extensiones de enlace existentes sin tener que volver a publicar la aplicación de funciones, consulte [Actualización de las extensiones]. |

[core tools]: ./functions-run-local.md
[conjunto de extensiones]: ./functions-bindings-register.md#extension-bundles
[Paquete NuGet]: https://www.nuget.org/packages/Microsoft.Azure.WebJobs.Extensions.Http
[Actualización de las extensiones]: ./install-update-binding-extensions-manual.md
[Azure Tools extension]: https://marketplace.visualstudio.com/items?itemName=ms-vscode.vscode-node-azure-pack

### <a name="functions-1x"></a>Functions 1.x

Las aplicaciones de Functions 1.x tienen automáticamente una referencia al paquete NuGet [Microsoft.Azure.WebJobs](https://www.nuget.org/packages/Microsoft.Azure.WebJobs), versión 2.x.

## <a name="next-steps"></a>Pasos siguientes

- [Ejecución de una función desde una solicitud HTTP](./functions-bindings-http-webhook-trigger.md)
- [Devolución de una respuesta HTTP desde una función](./functions-bindings-http-webhook-output.md)
