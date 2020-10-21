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
ms.openlocfilehash: 6bb50e427fa85a170c5ad23a63d67c01e898a17d
ms.sourcegitcommit: 829d951d5c90442a38012daaf77e86046018e5b9
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/09/2020
ms.locfileid: "91665691"
---
# <a name="swagger-documentation"></a>Documentación de Swagger

El servicio de voz ofrece una especificación de Swagger para interactuar con varias de las API REST que se usan para importar datos, crear modelos, probar la precisión de los modelos, crear puntos de conexión personalizados, poner en cola las transcripciones en lote y administrar las suscripciones. La mayoría de las operaciones disponibles a través del portal de Habla personalizada se pueden completar mediante programación con estas API.

> [!NOTE]
> Se admiten las operaciones tanto de voz a texto como de texto a voz y están disponibles como API REST, que a su vez se documentan en la especificación de Swagger.

## <a name="generating-code-from-the-swagger-specification"></a>Generación de código desde la especificación Swagger

La [especificación Swagger](https://westus.dev.cognitive.microsoft.com/docs/services/speech-to-text-api-v3-0) tiene opciones que permiten probar rápidamente varias rutas de acceso. Sin embargo, a veces es conveniente generar código para todas las rutas de acceso, lo que crea una sola biblioteca de llamadas en la que se puedan basar las soluciones futuras. Echemos un vistazo al proceso para generar una biblioteca Python.

Tendrá que establecer Swagger en la misma región que su suscripción al servicio de voz. Puede confirmar la región en Azure Portal en el recurso de servicio de voz. Para ver una lista completa de las regiones admitidas, consulte [Regiones](regions.md).

1. En un explorador, vaya a la especificación de Swagger de la región:  
       `https://<your-region>.dev.cognitive.microsoft.com/docs/services/speech-to-text-api-v3-0`
1. En esa página, haga clic en **Definición de API** y en **Swagger**. Copie la dirección URL de la página que aparece.
1. En un nuevo explorador, vaya a https://editor.swagger.io
1. Haga clic en **Archivo**, en **Import URL** (URL de importación), pegue la dirección URL y haga clic en **Aceptar**.
1. Haga clic en **Generate Client** (Generar cliente) y seleccione **Python**. La biblioteca cliente se descarga en el equipo en un archivo `.zip`.
1. Extraiga todo de la descarga. Puede usar `tar -xf` para extraer todo.
1. Instale el módulo extraído en el entorno de Python:  
       `pip install path/to/package/python-client`
1. El nombre del paquete instalado es `swagger_client`. Compruebe que la instalación ha funcionado:  
       `python -c "import swagger_client"`

Puede usar la biblioteca Python que generó con los [ejemplos del servicio de voz en GitHub](https://aka.ms/csspeech/samples).

## <a name="reference-docs"></a>Documentos de referencia

* [REST (Swagger): Batch transcription and customization](https://westus.dev.cognitive.microsoft.com/docs/services/speech-to-text-api-v3-0) (API de REST: Transcripción y personalización de Azure Batch)
* [API REST: Speech-to-text](rest-speech-to-text.md) (API de REST: Voz a texto)
* [API REST: Text-to-speech](rest-text-to-speech.md) (API de REST: Texto a voz)

## <a name="next-steps"></a>Pasos siguientes

* [Ejemplos del servicio de voz en GitHub](https://aka.ms/csspeech/samples).
* [Obtenga una clave de suscripción gratuita a los servicios de Voz](overview.md#try-the-speech-service-for-free)
