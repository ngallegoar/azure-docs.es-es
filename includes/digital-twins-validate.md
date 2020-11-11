---
author: baanders
description: Archivo de inclusión que describe cómo validar los modelos de Azure Digital Twins
ms.service: digital-twins
ms.topic: include
ms.date: 8/7/2020
ms.author: baanders
ms.openlocfilehash: bccf92c6c912747e28b3075f12ac1558cced30c9
ms.sourcegitcommit: 3bdeb546890a740384a8ef383cf915e84bd7e91e
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/30/2020
ms.locfileid: "93130388"
---
> [!TIP]
> Después de crear un modelo, se recomienda validar los modelos sin conexión antes de cargarlos en la instancia de Azure Digital Twins.

Hay un ejemplo disponible para todos los lenguajes que sirve para validar los documentos del modelo para garantizar que DTDL sea correcto antes de cargarlo en la instancia. Se encuentra aquí: [**Ejemplo de validador de DTDL**](https://docs.microsoft.com/samples/azure-samples/dtdl-validator/dtdl-validator).

* El ejemplo de validador de DTDL se basa en una biblioteca de analizador de DTDL de .NET, que está disponible en NuGet como una biblioteca de cliente: [**Microsoft.Azure.DigitalTwins.Parser**](https://nuget.org/packages/Microsoft.Azure.DigitalTwins.Parser/). También puede usar la biblioteca directamente para diseñar su propia solución de validación. Al usar la biblioteca del analizador, asegúrese de usar una versión que sea compatible con la versión de Azure Digital Twins que se está ejecutando. Actualmente, se trata de la versión *3.12.4*.

Puede obtener más información sobre el ejemplo del validador y la biblioteca del analizador, incluidos ejemplos de uso, en [*Procedimientos: Análisis y validación de modelos*](../articles/digital-twins/how-to-parse-models.md).