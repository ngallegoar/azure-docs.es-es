---
title: 'Códigos de respuesta HTTP de API: LUIS'
titleSuffix: Azure Cognitive Services
description: Comprender qué códigos de respuesta HTTP se devuelven de las API de punto de conexión y creación de LUIS
services: cognitive-services
manager: nitinme
ms.custom: seodec18
ms.service: cognitive-services
ms.subservice: language-understanding
ms.topic: reference
ms.date: 03/04/2019
ms.openlocfilehash: 46a7fff86d3a8c4539b77a0f271179d68e7a26d2
ms.sourcegitcommit: 829d951d5c90442a38012daaf77e86046018e5b9
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/09/2020
ms.locfileid: "91541686"
---
# <a name="common-api-response-codes-and-their-meaning"></a>Códigos de respuesta de Common API y su significado

Las API de [creación](https://go.microsoft.com/fwlink/?linkid=2092087) y [punto de conexión](https://go.microsoft.com/fwlink/?linkid=2092356) devuelven códigos de respuesta HTTP. Mientras que los mensajes de respuesta incluyen información específica de una solicitud, el código de estado de respuesta HTTP es general.

## <a name="common-status-codes"></a>Códigos de estado comunes
En la tabla siguiente se muestran algunos de los códigos de estado de respuesta HTTP más comunes para las API de [creación](https://go.microsoft.com/fwlink/?linkid=2092087) y [punto de conexión](https://go.microsoft.com/fwlink/?linkid=2092356):

|Código|API|Explicación|
|:--|--|--|
|400|Creación, punto de conexión|Los parámetros de la solicitud no son correctos, lo que significa que faltan parámetros necesarios, o bien que tienen un formato incorrecto o son demasiado grandes.|
|400|Creación, punto de conexión|El cuerpo de la solicitud es incorrecto, lo que significa que falta el código JSON, o bien que tiene un formato incorrecto o es demasiado grande.|
|401|Creación|Se usó la clave de la suscripción de punto de conexión en lugar de la clave de creación.|
|401|Creación, punto de conexión|La clave no es válida, tiene un formato incorrecto o está vacía.|
|401|Creación, punto de conexión| La clave no coincide con la región.|
|401|Creación|No es el propietario o un colaborador.|
|401|Creación|El orden de las llamadas API no es válido.|
|403|Creación, punto de conexión|Se superó el límite de cuota de claves mensual total.|
|409|Punto de conexión|La aplicación aún se está cargando.|
|410|Punto de conexión|La aplicación se debe entrenar y publicar de nuevo.|
|414|Punto de conexión|La consulta supera el límite máximo de caracteres.|
|429|Creación, punto de conexión|Se superó el límite de velocidad (solicitudes/segundo)|

## <a name="next-steps"></a>Pasos siguientes

* Documentación de [creación](https://westus.dev.cognitive.microsoft.com/docs/services/5890b47c39e2bb17b84a55ff/operations/5890b47c39e2bb052c5b9c2f) y [punto de conexión](https://westus.dev.cognitive.microsoft.com/docs/services/5819c76f40a6350ce09de1ac/operations/5819c77140a63516d81aee78) de API REST
