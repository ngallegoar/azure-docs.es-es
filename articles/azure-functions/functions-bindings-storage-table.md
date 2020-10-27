---
title: Enlaces de Azure Table Storage para Azure Functions
description: Descubra cómo usar los enlaces de Azure Table Storage en Azure Functions.
author: craigshoemaker
ms.topic: reference
ms.date: 09/03/2018
ms.author: cshoe
ms.custom: devx-track-csharp, devx-track-python
ms.openlocfilehash: 4f2b890dc60cd50b5fcaefabe8d418268b738c20
ms.sourcegitcommit: 93329b2fcdb9b4091dbd632ee031801f74beb05b
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/15/2020
ms.locfileid: "92096732"
---
# <a name="azure-table-storage-bindings-for-azure-functions"></a>Enlaces de Azure Table Storage para Azure Functions

Azure Functions se integra con [Azure Storage](../storage/index.yml) mediante [desencadenadores y enlaces](./functions-triggers-bindings.md). La integración con Table Storage le permite crear funciones que leen y escriben datos de Table Storage.

| Acción | Tipo |
|---------|---------|
| Lectura de datos de Table Storage en una función | [Enlace de entrada](./functions-bindings-storage-table-input.md) |
| Escritura de datos de Table Storage desde una función |[Enlace de salida](./functions-bindings-storage-table-output.md) |

## <a name="packages---functions-2x-and-higher"></a>Paquetes: Functions 2.x y versiones posteriores

Los enlaces de almacenamiento de Table se proporcionan en el paquete NuGet [Microsoft.Azure.WebJobs.Extensions.Storage](https://www.nuget.org/packages/Microsoft.Azure.WebJobs.Extensions.Storage), versión 3.x. El código fuente del paquete se encuentra en el repositorio [azure-webjobs-sdk](https://github.com/Azure/azure-webjobs-sdk/tree/dev/src/Microsoft.Azure.WebJobs.Extensions.Storage/Tables) de GitHub.

[!INCLUDE [functions-package-v2](../../includes/functions-package-v2.md)]

## <a name="packages---functions-1x"></a>Paquetes: Functions 1.x

Los enlaces de Table Storage se proporcionan en el paquete NuGet [Microsoft.Azure.WebJobs](https://www.nuget.org/packages/Microsoft.Azure.WebJobs), versión 2.x. El código fuente del paquete se encuentra en el repositorio [azure-webjobs-sdk](https://github.com/Azure/azure-webjobs-sdk/tree/v2.x/src/Microsoft.Azure.WebJobs.Storage/Table) de GitHub.

[!INCLUDE [functions-package-auto](../../includes/functions-package-auto.md)]

[!INCLUDE [functions-storage-sdk-version](../../includes/functions-storage-sdk-version.md)]

## <a name="next-steps"></a>Pasos siguientes

- [Lectura de datos de Table Storage cuando se ejecuta una función](./functions-bindings-storage-table-input.md)
- [Escritura de datos de Table Storage desde una función](./functions-bindings-storage-table-output.md)
