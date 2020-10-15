---
title: 'Conceptos de revisiones, flujos de trabajo y trabajos: Content Moderator'
titleSuffix: Azure Cognitive Services
description: 'En este artículo, aprenderá los conceptos básicos de la herramienta de revisión: revisiones, flujos de trabajo y trabajos.'
services: cognitive-services
author: PatrickFarley
manager: nitinme
ms.service: cognitive-services
ms.subservice: content-moderator
ms.topic: conceptual
ms.date: 03/14/2019
ms.author: pafarley
ms.openlocfilehash: 1aba86efb9ea76fbf060e80b47f9f2f6cdf8ee71
ms.sourcegitcommit: 829d951d5c90442a38012daaf77e86046018e5b9
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/09/2020
ms.locfileid: "91872058"
---
# <a name="content-moderation-reviews-workflows-and-jobs"></a>Revisiones, flujos de trabajo y trabajos de moderación de contenido

Content Moderator combina la moderación automática con funcionalidades de intervención humana en bucle para crear un proceso de moderación óptimo para escenarios del mundo real. Lo hace mediante la [herramienta de revisión](https://contentmoderator.cognitive.microsoft.com) basada en a nube. En esta guía, aprenderá los conceptos básicos de la herramienta de revisión: revisiones, flujos de trabajo y trabajos.

## <a name="reviews"></a>Revisiones

En una revisión, el contenido se carga en la herramienta de revisión y aparece en la pestaña **Revisar**. A partir de aquí, los usuarios pueden modificar las etiquetas aplicadas y aplicar sus propias etiquetas personalizadas según corresponda. Cuando un usuario envía una revisión, los resultados se envían a un punto de conexión de devolución de llamada especificado y el contenido se quita del sitio.

![El sitio web de la herramienta de revisión se abre en un explorador web, en la pestaña Revisar](./Review-Tool-user-Guide/images/image-workflow-review.png)

Consulte la [guía de la herramienta de revisión](./review-tool-user-guide/review-moderated-images.md) para empezar a crear revisiones o la [guía de la API REST](./try-review-api-review.md) si quiere aprender a hacerlo mediante programación.

## <a name="workflows"></a>Workflows

Un flujo de trabajo es un filtro personalizado basado en la nube para el contenido. Los flujos de trabajo pueden conectarse a diversos servicios para filtrar el contenido de maneras diferentes y luego realizar la acción apropiada. Con el conector de Content Moderator, un flujo de trabajo puede aplicar automáticamente etiquetas de moderación y crear revisiones con el contenido enviado.

### <a name="view-workflows"></a>Vista de flujos de trabajo

Para ver los flujos de trabajo existentes, vaya a la [herramienta de revisión](https://contentmoderator.cognitive.microsoft.com/) y seleccione **Configuración** > **Flujos de trabajo**.

![Flujo de trabajo predeterminado](images/default-workflow-listed.PNG)

Los flujos de trabajo se pueden describir por completo como cadenas JSON, lo que hace que sean accesibles mediante programación. Si selecciona la opción **Editar** del flujo de trabajo y luego selecciona la pestaña **JSON**, verá una expresión JSON similar a la siguiente:

```json
{
    "Type": "Logic",
    "If": {
        "ConnectorName": "moderator",
        "OutputName": "isAdult",
        "Operator": "eq",
        "Value": "true",
        "Type": "Condition"
        },
    "Then": {
    "Perform": [
    {
        "Name": "createreview",
        "CallbackEndpoint": null,
        "Tags": []
    }
    ],
    "Type": "Actions"
    }
}
```

Consulte la [guía de la herramienta de revisión](./review-tool-user-guide/workflows.md) para empezar a crear y usar flujos de trabajo o la [guía de la API REST](./try-review-api-workflow.md) si quiere aprender a hacerlo mediante programación.

## <a name="jobs"></a>Trabajos

Un trabajo de moderación actúa como un tipo de contenedor para la funcionalidad de moderación de contenido, los flujos de trabajo y las revisiones. El trabajo de moderación examina el contenido con Content Moderator Image Moderation API o Text Moderation API y luego lo compara con el flujo de trabajo designado. En función de los resultados del flujo de trabajo, tal vez pueda crear una revisión del contenido en la [herramienta de revisión](./review-tool-user-guide/human-in-the-loop.md). Mientras que tanto las revisiones como los flujos de trabajo pueden crearse y configurarse con sus respectivas API, la API de trabajos le permite obtener un informe detallado de todo el proceso (que puede enviarse a un punto final de devolución de llamada especificado).

Consulte la [guía de la API REST](./try-review-api-job.md) para empezar a usar los trabajos.

## <a name="next-steps"></a>Pasos siguientes

* Pruebe la [consola de Job API](try-review-api-job.md) y use los ejemplos de código de la API REST. Consulte también el [inicio rápido de .NET de trabajos](moderation-jobs-quickstart-dotnet.md) si está familiarizado con C# y Visual Studio. 
* Para las revisiones, empiece a trabajar con la [consola de Review API](try-review-api-review.md) y use los ejemplos de código de la API REST. A continuación, consulte la sección de revisiones de [Guía de inicio rápido de .NET](dotnet-sdk-quickstart.md).
* Para revisiones de vídeos, use [Video review quickstart](video-reviews-quickstart-dotnet.md) (Guía de inicio rápido de revisión de vídeos) y aprenda a [agregar transcripciones a la revisión del vídeo](video-transcript-reviews-quickstart-dotnet.md).
