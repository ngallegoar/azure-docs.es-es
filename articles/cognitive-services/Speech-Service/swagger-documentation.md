---
title: 'Documentación de Swagger: servicio de voz'
titleSuffix: Azure Cognitive Services
description: La documentación de Swagger se puede utilizar para generar automáticamente SDK para varios lenguajes de programación. Todas las operaciones de nuestro servicio son compatibles con Swagger
services: cognitive-services
author: PanosPeriorellis
manager: nitinme
ms.service: cognitive-services
ms.subservice: speech-service
ms.topic: reference
ms.date: 07/05/2019
ms.author: erhopf
ms.openlocfilehash: fcc43caf895dadfaf832a47c3254f9b828bcb71a
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/28/2020
ms.locfileid: "77430812"
---
# <a name="swagger-documentation"></a>Documentación de Swagger

El servicio de voz ofrece una especificación de Swagger para interactuar con varias de las API REST que se usan para importar datos, crear modelos, probar la precisión de los modelos, crear puntos de conexión personalizados, poner en cola las transcripciones en lote y administrar las suscripciones. La mayoría de las operaciones disponibles a través del portal de Habla personalizada se pueden completar mediante programación con estas API.

> [!NOTE]
> Se admiten las operaciones tanto de voz a texto como de texto a voz y están disponibles como API REST, que a su vez se documentan en la especificación de Swagger.

## <a name="generating-code-from-the-swagger-specification"></a>Generación de código desde la especificación Swagger

La [especificación Swagger](https://cris.ai/swagger/ui/index) tiene opciones que permiten probar rápidamente varias rutas de acceso. Sin embargo, a veces es conveniente generar código para todas las rutas de acceso, lo que crea una sola biblioteca de llamadas en la que se puedan basar las soluciones futuras. Echemos un vistazo al proceso para generar una biblioteca Python.

Tendrá que establecer Swagger en la misma región que su suscripción al servicio de voz. Puede confirmar la región en Azure Portal en el recurso de servicio de voz. Para ver una lista completa de las regiones admitidas, consulte [Regiones](regions.md).

1. Vaya a https://editor.swagger.io.
2. Haga clic en **File** (Archivo) y, luego, en **Import** (Importar)
3. Escriba la dirección URL de Swagger, incluida la región de la suscripción del servicio de voz `https://<your-region>.cris.ai/docs/v2.0/swagger`.
4. Haga clic en **Generate Client** (Generar cliente) y seleccione Python
5. Guarde la biblioteca cliente

Puede usar la biblioteca Python que generó con los [ejemplos del servicio de voz en GitHub](https://aka.ms/csspeech/samples).

## <a name="reference-docs"></a>Documentos de referencia

* [REST (Swagger): Batch transcription and customization](https://westus.cris.ai/swagger/ui/index) (API de REST: Transcripción y personalización de Azure Batch)
* [API REST: Speech-to-text](rest-speech-to-text.md) (API de REST: Voz a texto)
* [API REST: Text-to-speech](rest-text-to-speech.md) (API de REST: Texto a voz)

## <a name="next-steps"></a>Pasos siguientes

* [Ejemplos del servicio de voz en GitHub](https://aka.ms/csspeech/samples).
* [Obtenga una clave de suscripción gratuita a los servicios de Voz](get-started.md)
