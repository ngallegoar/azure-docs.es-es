---
title: 'Referencia de API: Content Moderator'
titleSuffix: Azure Cognitive Services
description: Aprenda sobre las distintas API de moderación y revisión de contenido de Content Moderator.
services: cognitive-services
author: PatrickFarley
manager: nitinme
ms.service: cognitive-services
ms.subservice: content-moderator
ms.topic: reference
ms.date: 05/29/2019
ms.author: pafarley
ms.openlocfilehash: 7fc46d06b68dca074da060b4866186a6242ffad2
ms.sourcegitcommit: 58faa9fcbd62f3ac37ff0a65ab9357a01051a64f
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 04/29/2020
ms.locfileid: "72757377"
---
# <a name="content-moderator-api-reference"></a>Referencia de API de Content Moderator

Puede empezar a trabajar con las API de Azure Content Moderator de las siguientes maneras:

- En Azure Portal, [suscríbase a Content Moderator API](https://ms.portal.azure.com/#create/Microsoft.CognitiveServicesContentModerator).
- Consulte [Prueba de Content Moderator en la web](quick-start.md) para registrarse en la [herramienta de revisión de Content Moderator](https://contentmoderator.cognitive.microsoft.com/).

## <a name="moderation-apis"></a>API de moderación

Puede usar las siguientes API de Content Moderator para configurar los flujos de trabajo posteriores a la moderación.

| Descripción | Referencia |
| -------------------- |-------------|
| **Image Moderation API**<br /><br />Examine imágenes y detecte posible contenido para adultos o explícito mediante etiquetas, puntuaciones de confianza y otra información extraída. <br /><br />Use esta información para publicar, rechazar o revisar el contenido del flujo de trabajo posterior a la moderación. <br /><br />| [Referencia de Image Moderation API](https://westus.dev.cognitive.microsoft.com/docs/services/57cf753a3f9b070c105bd2c1/operations/57cf753a3f9b070868a1f66c "Referencia de Image Moderation API")   |
| **Text Moderation API**<br /><br />Examine contenido de texto. Las blasfemias y los datos personales se devuelven. <br /><br />Use esta información para publicar, rechazar o revisar el contenido del flujo de trabajo posterior a la moderación.<br /><br /> | [Referencia de Text Moderation API](https://westus.dev.cognitive.microsoft.com/docs/services/57cf753a3f9b070c105bd2c1/operations/57cf753a3f9b070868a1f66f "Referencia de Text Moderation API")   |
| **Video Moderation API**<br /><br />Examine vídeos y detecte posible contenido para adultos y explícito. <br /><br />Use esta información para publicar, rechazar o revisar el contenido del flujo de trabajo posterior a la moderación.<br /><br /> | [Introducción a Video Moderation API](video-moderation-api.md "Introducción a Video Moderation API")   |
| **List Management API**<br /><br />Cree y administre listas personalizadas de exclusión o inclusión de texto e imágenes. Si están habilitadas, las operaciones **Image - Match** (Imagen: coincidir) y **Text - Screen** (Texto: pantalla) buscan coincidencias parciales del contenido enviado en las listas personalizadas. <br /><br />Para mejorar la eficacia, puede omitir el paso de moderación basado en el aprendizaje automático.<br /><br /> | [Referencia de List Management API](https://westus.dev.cognitive.microsoft.com/docs/services/57cf755e3f9b070c105bd2c2/operations/57cf755e3f9b070868a1f675 "Referencia de List Management API")   |

## <a name="review-apis"></a>Revisión de las API

Review APIs tienen los siguientes componentes:

| Descripción | Referencia |
| -------------------- |-------------|
| **Trabajos**<br /><br /> Inicie flujos de trabajo de examen y revisión para el contenido de imagen y texto. Un trabajo de moderación examina el contenido mediante Image Moderation API y Text Moderation API. Los trabajos de moderación usan los flujos de trabajo definidos y predeterminados para generar revisiones. <br /><br />Después de que un moderador humano ha revisado las etiquetas asignadas automáticamente y los datos de predicción y de que ha enviado una decisión de moderación de contenido, Review API envía toda la información al punto de conexión de API.<br /><br /> | [Referencia de trabajos](https://westus.dev.cognitive.microsoft.com/docs/services/580519463f9b070e5c591178/operations/580519483f9b0709fc47f9c5 "Referencia de trabajos")   |
| **Revisiones**<br /><br />Use la herramienta de revisión para crear directamente revisiones de texto o imagen para moderadores humanos.<br /><br /> Después de que un moderador humano ha revisado las etiquetas asignadas automáticamente y los datos de predicción y de que ha enviado una decisión de moderación de contenido, Review API envía toda la información al punto de conexión de API.<br /><br /> | [Referencia de revisiones](https://westus.dev.cognitive.microsoft.com/docs/services/580519463f9b070e5c591178/operations/580519483f9b0709fc47f9c4 "Referencia de revisiones")   |
| **Flujos de trabajo**<br /><br />Cree, actualice y obtenga información detallada sobre los flujos de trabajo personalizados que crea el equipo. Los flujos de trabajo se definen mediante la herramienta de revisión. <br /> <br />Normalmente, los flujos de trabajo usan Content Moderator, pero también pueden usar algunas otras API que están disponibles como conectores en la herramienta de revisión.<br /><br /> | [Referencia de flujos de trabajo](https://westus.dev.cognitive.microsoft.com/docs/services/580519463f9b070e5c591178/operations/5813b46b3f9b0711b43c4c59 "Referencia de flujos de trabajo")   |