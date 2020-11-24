---
author: baanders
description: 'archivo de inclusión para Azure Digital Twins: formas de administrar la instancia'
ms.service: digital-twins
ms.topic: include
ms.date: 11/11/2020
ms.author: baanders
ms.openlocfilehash: 47de00780fb259e7476c16dea9b7c63bc512a211
ms.sourcegitcommit: 6ab718e1be2767db2605eeebe974ee9e2c07022b
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 11/12/2020
ms.locfileid: "94533912"
---
En este artículo se describe cómo completar diferentes operaciones de administración con el [**SDK** para .NET de Azure Digital Twins (C#)](/dotnet/api/overview/azure/digitaltwins/management?view=azure-dotnet&preserve-view=true). También puede crear estas mismas llamadas de administración con los otros SDK de lenguaje descritos en [*Paso a paso: las API y los SDK de Azure Digital Twins*](../articles/digital-twins/how-to-use-apis-sdks.md).

> [!TIP] 
> Recuerde que todos los métodos del SDK cuentan con versiones sincrónicas y asincrónicas. En las llamadas de paginación, los métodos asincrónicos devuelven `AsyncPageable<T>`, mientras que las versiones sincrónicas devuelven `Pageable<T>`.

Otra opción de administración es llamar a las [**API REST**](/rest/api/azure-digitaltwins/) de Azure Digital Twins para esta cuestión directamente.

Por último, puede completar las mismas operaciones de administración mediante la **CLI** de Azure Digital Twins. Para obtener más información sobre el uso de la CLI, consulte [*Paso a paso: Uso de la CLI de Azure Digital Twins*](../articles/digital-twins/how-to-use-cli.md).