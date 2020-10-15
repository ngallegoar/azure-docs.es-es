---
title: Enlaces de SignalR Service con Azure Functions
description: Uso de enlaces de SignalR Service con Azure Functions.
author: craigshoemaker
ms.topic: reference
ms.date: 02/28/2019
ms.author: cshoe
ms.openlocfilehash: 863620ce6f0af33b05ef290ae95ccdc99a53a54d
ms.sourcegitcommit: 829d951d5c90442a38012daaf77e86046018e5b9
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/09/2020
ms.locfileid: "77523043"
---
# <a name="signalr-service-bindings-for-azure-functions"></a>Enlaces de SignalR Service para Azure Functions

En este conjunto de artículos se explica cómo autenticar y enviar mensajes en tiempo real a los clientes conectados a [Azure SignalR Service](https://azure.microsoft.com/services/signalr-service/) mediante enlaces de SignalR Service en Azure Functions. Azure Functions admite enlaces de entrada y salida para SignalR Service.

| Acción | Tipo |
|---------|---------|
| Devolver la dirección URL del punto de conexión de servicio y el token de acceso | [Enlace de entrada](./functions-bindings-signalr-service-input.md) |
| Enviar mensajes de SignalR Service |[Enlace de salida](./functions-bindings-signalr-service-output.md) |

## <a name="add-to-your-functions-app"></a>Adición a la aplicación de Functions

### <a name="functions-2x-and-higher"></a>Functions 2.x y superiores

Para trabajar con el desencadenador y los enlaces, es necesario hacer referencia al paquete adecuado. En las bibliotecas de clases de .NET se usa el paquete NuGet, mientras que en los demás tipos de aplicaciones se emplea el conjunto de extensiones.

| Idioma                                        | Agregar mediante...                                   | Observaciones 
|-------------------------------------------------|---------------------------------------------|-------------|
| C#                                              | Instalación del [paquete NuGet], versión 3.x | |
| Script de C#, Java, JavaScript, Python, PowerShell | Registro de [conjunto de extensiones]          | Se recomienda usar la [extensión Azure Tools] con Visual Studio Code. |
| Script de C# (solo en línea en Azure Portal)         | Adición de un enlace                            | Para actualizar extensiones de enlace existentes sin tener que volver a publicar la aplicación de funciones, consulte [Actualización de las extensiones]. |

[Paquete NuGet]: https://www.nuget.org/packages/Microsoft.Azure.WebJobs.Extensions.SignalRService
[core tools]: ./functions-run-local.md
[conjunto de extensiones]: ./functions-bindings-register.md#extension-bundles
[Actualización de las extensiones]: ./install-update-binding-extensions-manual.md
[Extensión Azure Tools]: https://marketplace.visualstudio.com/items?itemName=ms-vscode.vscode-node-azure-pack

Para más información sobre cómo configurar y usar SignalR Service y Azure Functions juntos, consulte [Desarrollo y configuración de Azure Functions con Azure SignalR Service](../azure-signalr/signalr-concept-serverless-development-config.md).

### <a name="annotations-library-java-only"></a>Biblioteca de anotaciones (solo Java)

Para usar las anotaciones de SignalR Service en las funciones de Java, debe agregar una dependencia al artefacto *azure-functions-java-library-signalr* (versión 1.0 o superior) en el archivo *pom.xml*.

```xml
<dependency>
    <groupId>com.microsoft.azure.functions</groupId>
    <artifactId>azure-functions-java-library-signalr</artifactId>
    <version>1.0.0</version>
</dependency>
```

## <a name="next-steps"></a>Pasos siguientes

- [Devolución de la dirección URL del punto de conexión de servicio y el token de acceso (enlace de entrada)](./functions-bindings-signalr-service-input.md)
- [Envío de mensajes de SignalR Service (enlace de salida)](./functions-bindings-signalr-service-output.md) 
