---
author: baanders
description: 'archivo de inclusión para Azure Digital Twins: formas de administrar la instancia'
ms.service: digital-twins
ms.topic: include
ms.date: 11/11/2020
ms.author: baanders
ms.openlocfilehash: 887d185249f96b5d3be4aab6a96aa3c6c4a85690
ms.sourcegitcommit: d22a86a1329be8fd1913ce4d1bfbd2a125b2bcae
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 11/26/2020
ms.locfileid: "96231419"
---
En este artículo se describe cómo completar diferentes operaciones de administración con el [**SDK** para .NET de Azure Digital Twins (C#)](/dotnet/api/overview/azure/digitaltwins/management?view=azure-dotnet&preserve-view=true). También puede crear estas mismas llamadas de administración con los otros SDK de lenguaje descritos en [*Paso a paso: las API y los SDK de Azure Digital Twins*](../articles/digital-twins/how-to-use-apis-sdks.md).

> [!TIP] 
> Recuerde que todos los métodos del SDK cuentan con versiones sincrónicas y asincrónicas. En las llamadas de paginación, los métodos asincrónicos devuelven `AsyncPageable<T>`, mientras que las versiones sincrónicas devuelven `Pageable<T>`.

Otra opción de administración es llamar a las [**API REST**](/rest/api/azure-digitaltwins/) de Azure Digital Twins para esta cuestión directamente a través de un cliente de REST como Postman. Para obtener instrucciones sobre cómo hacerlo, consulte [*Procedimiento: Realización de solicitudes con Postman*](../articles/digital-twins/how-to-use-postman.md).

Por último, puede completar las mismas operaciones de administración mediante la **CLI** de Azure Digital Twins. Para obtener más información sobre el uso de la CLI, consulte [*Paso a paso: Uso de la CLI de Azure Digital Twins*](../articles/digital-twins/how-to-use-cli.md).