---
title: archivo de inclusión
description: archivo de inclusión
services: functions
author: craigshoemaker
manager: gwallace
ms.service: azure-functions
ms.topic: include
ms.date: 08/02/2019
ms.author: cshoe
ms.custom: include file
ms.openlocfilehash: fa3888d28c52625684676036def7e2920b77b5ca
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/28/2020
ms.locfileid: "77201973"
---
Los siguientes tipos de parámetros se pueden usar en el blob de desencadenamiento:

* `Stream`
* `TextReader`
* `string`
* `Byte[]`
* Un objeto POCO serializable como JSON
* `ICloudBlob`<sup>1</sup>
* `CloudBlockBlob`<sup>1</sup>
* `CloudPageBlob`<sup>1</sup>
* `CloudAppendBlob`<sup>1</sup>

<sup>1</sup> requiere un enlace "inout" `direction` en *function.json* o `FileAccess.ReadWrite` en una biblioteca de clases de C#.

Si intenta enlazar a uno de los tipos de SDK de Storage y recibe un mensaje de error, asegúrese de que hace referencia a [la versión de SDK de Storage correcta](../articles/azure-functions/functions-bindings-storage-blob.md#azure-storage-sdk-version-in-functions-1x).

El enlace a `string`, `Byte[]` o POCO solo se recomienda si el tamaño de blob es pequeño, ya que todo el contenido del blob se carga en memoria. Por lo general, es preferible usar un tipo `Stream` o `CloudBlockBlob`. Para obtener más información, consulte [Uso de simultaneidad y memoria](../articles/azure-functions/functions-bindings-storage-blob-trigger.md#concurrency-and-memory-usage) más adelante en este artículo.