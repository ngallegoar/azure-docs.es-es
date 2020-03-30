---
title: Actualización de un proyecto a la API 3.0
titleSuffix: Azure Cognitive Services
description: Aprenda a actualizar proyectos de Custom Vision de la versión anterior de la API a la API 3.0.
services: cognitive-services
author: areddish
manager: nitinme
ms.service: cognitive-services
ms.subservice: custom-vision
ms.topic: conceptual
ms.date: 04/04/2019
ms.author: areddish
ms.openlocfilehash: c134f30b124113a23df0e73cd1bbc8209e335183
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/28/2020
ms.locfileid: "73647499"
---
# <a name="update-to-the-30-api"></a>Actualización a la API 3.0

Custom Vision ha alcanzado ahora la disponibilidad general y ha pasado por una actualización de la API.
Esta actualización incluye algunas características nuevas y, lo que es más importante, algunos cambios importantes:

* Prediction API ahora se divide en dos, según el tipo de proyecto.
* La opción de exportación del kit para desarrolladores de Vision AI (VAIDK) requiere la creación de un proyecto de una manera específica.
* Las iteraciones predeterminadas se han eliminado en favor de una publicación o cancelación de publicación de una iteración con nombre.

En esta guía se muestra cómo actualizar los proyectos para que funcionen con la nueva versión de API. Consulte las [notas de la versión](release-notes.md) para obtener una lista completa de los cambios.

## <a name="use-the-updated-prediction-api"></a>Uso de la versión de Prediction API actualizada

Las API 2.0 usaban la misma llamada de predicción tanto para los clasificadores de imagen como para los proyectos de detección de objetos. Ambos tipos de proyecto eran aceptables para las llamadas **PredictImage** y **PredictImageUrl**. A partir de la versión 3.0, hemos dividimos esta API para que deba relacionar el tipo de proyecto con la llamada:

* Use **[ClassifyImage](https://southcentralus.dev.cognitive.microsoft.com/docs/services/Custom_Vision_Prediction_3.0/operations/5c82db60bf6a2b11a8247c15)** y **[ClassifyImageUrl](https://southcentralus.dev.cognitive.microsoft.com/docs/services/Custom_Vision_Prediction_3.0/operations/5c82db60bf6a2b11a8247c14)** para obtener predicciones para los proyectos de clasificación de imágenes.
* Use **[DetectImage](https://southcentralus.dev.cognitive.microsoft.com/docs/services/Custom_Vision_Prediction_3.0/operations/5c82db60bf6a2b11a8247c19)** y **[DetectImageUrl](https://southcentralus.dev.cognitive.microsoft.com/docs/services/Custom_Vision_Prediction_3.0/operations/5c82db60bf6a2b11a8247c18)** para obtener predicciones para los proyectos de detección de objetos.

## <a name="use-the-new-iteration-publishing-workflow"></a>Uso del nuevo flujo de trabajo de publicación de iteraciones

Las API 2.0 usaban la iteración predeterminada o un identificador de iteración especificado para elegir qué iteración emplear con la predicción. A partir de la versión 3.0, hemos adoptado un flujo de publicación mediante el cual primero se publica una iteración con un nombre especificado desde la API de entrenamiento. A continuación, se pasa el nombre a los métodos de predicción para especificar qué iteración usar.

> [!IMPORTANT]
> Las API 3.0 no usan la característica de iteración predeterminada. Hasta que se pongan en desuso las API anteriores, puede seguir usando las API 2.0 para activar o desactivar una iteración como predeterminada. Estas API se mantendrá durante un tiempo, y puede llamar al método **[UpdateIteration](https://southcentralus.dev.cognitive.microsoft.com/docs/services/Custom_Vision_Training_3.0/operations/5c771cdcbf6a2b18a0c3b818)** para marcar una iteración como predeterminada.

### <a name="publish-an-iteration"></a>Publicación de una iteración

Después de entrenar una iteración, puede hacer que esté disponible para la predicción mediante el método **[PublishIteration](https://southcentralus.dev.cognitive.microsoft.com/docs/services/Custom_Vision_Training_3.0/operations/5c82db28bf6a2b11a8247bbc)** . Para publicar una iteración, necesitará el identificador de recurso de predicción, que está disponible en la página de configuración del sitio web de Custom Vision.

![La página de configuración del sitio web de Custom Vision con el identificador de recurso de predicción subrayado.](./media/update-application-to-3.0-sdk/prediction-id.png)

> [!TIP]
> También puede obtener esta información de [Azure Portal](https://portal.azure.com); para ello, vaya a recurso de predicción de Custom Vision y seleccione **Propiedades**.

Después de publicar la iteración, las aplicaciones pueden usarla en la predicción mediante la especificación del nombre en su llamada a la API de predicción. Para que una iteración deje de estar disponible para las llamadas de predicción, use la API **[UnpublishIteration](https://southcentralus.dev.cognitive.microsoft.com/docs/services/Custom_Vision_Training_3.0/operations/5c771cdcbf6a2b18a0c3b81a)** .

## <a name="next-steps"></a>Pasos siguientes

* [Documentación de referencia de API de entrenamiento (REST)](https://go.microsoft.com/fwlink/?linkid=865446)
* [Documentación de referencia de Prediction API (REST)](https://go.microsoft.com/fwlink/?linkid=865445)