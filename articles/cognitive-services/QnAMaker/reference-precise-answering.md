---
title: 'Respuestas precisas mediante la detección del intervalo de respuestas: QnA Maker'
description: Descripción de la característica de respuestas precisas disponible en QnA Maker administrado.
ms.service: cognitive-services
ms.subservice: qna-maker
ms.topic: reference
ms.date: 11/09/2020
ms.openlocfilehash: 4f64bab698cb87e26fa4fd1587c4269acf99fa59
ms.sourcegitcommit: 8a1ba1ebc76635b643b6634cc64e137f74a1e4da
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 11/09/2020
ms.locfileid: "94385201"
---
# <a name="precise-answering"></a>Respuestas precisas

La característica de respuestas precisas permite obtener la respuesta corta precisa del pasaje de la respuesta más adecuado presente en la knowledge base para cualquier consulta de usuario. Esta característica utiliza un modelo de aprendizaje profundo que en tiempo de ejecución, que entiende la intención de la consulta de usuario y detecta la respuesta corta precisa en el pasaje de la respuesta, si hay una respuesta corta presente como hecho en el pasaje de la respuesta. 

Esta característica está activada de forma predeterminada en el panel de pruebas, con el fin de que pueda probar la funcionalidad específica de su escenario. Esta característica es muy útil para tanto para los desarrolladores de contenido como para los usuarios finales. Ahora, no es preciso que los desarrolladores de contenido mantengan manualmente pares de QnA específicos para cada hecho presente en la knowledge base ni que el usuario final tenga que examinar todo el pasaje de la respuesta devuelto por el servicio para encontrar el hecho real que responde a la consulta del usuario. 

## <a name="precise-answering-on-qna-maker-portal"></a>Respuestas precisas en el portal de QnA Maker

En el portal de QnA Maker, al abrir el panel de pruebas, verá en la parte superior una opción para **mostrar la respuesta corta**. Esta opción estará seleccionada de forma predeterminada. Si escribe una consulta en el panel de pruebas, verá una respuesta corta junto con el pasaje de la respuesta, en caso de que haya una respuesta corta en el pasaje de la respuesta. 
 
![Panel de pruebas administrado habilitado](../QnAMaker/media/conversational-context/test-pane-with-managed.png)

Puede anular la selección de la opción **Display short answer** (Mostrar respuesta corta) si solo desea ver el pasaje de la respuesta en el panel de pruebas. 

El servicio también devuelve la puntuación de confianza de la respuesta precisa como una **puntuación del intervalo de respuesta**, que puede comprobar seleccionando la opción **Inspect** (Inspeccionar), que se encuentra inmediatamente debajo de la consulta en el panel de pruebas.

![Puntuación de intervalo de respuesta administrado](../QnAMaker/media/conversational-context/managed-answer-span-score.png)

## <a name="publishing-a-qna-maker-bot"></a>Publicación de un bot de QnA Maker

Cuando se publica un bot, se obtiene de forma predeterminada la experiencia habilitada de la respuesta precisa en la aplicación, donde verá una respuesta corta junto con el pasaje de la respuesta. El usuario tiene la flexibilidad de elegir otras experiencias mediante la actualización de la plantilla a través del servicio de aplicaciones eBot. 

## <a name="language-support"></a>Compatibilidad con idiomas

Actualmente, la característica de respuesta precisa solo se admite en inglés.
