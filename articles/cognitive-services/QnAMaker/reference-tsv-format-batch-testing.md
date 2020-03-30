---
title: 'Pruebas por lotes del formato TSV: QnA Maker'
titleSuffix: Azure Cognitive Services
description: Descripción del formato TSV para las pruebas por lotes
services: cognitive-services
author: diberry
manager: nitinme
ms.service: cognitive-services
ms.subservice: qna-maker
ms.topic: reference
ms.date: 10/24/2019
ms.author: diberry
ms.openlocfilehash: dccf034b6497651db70b6cc09fff3f1b1d645a1b
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/28/2020
ms.locfileid: "73506803"
---
# <a name="batch-testing-tsv-format"></a>Pruebas por lotes de formato TSV

Las pruebas por lotes están disponibles desde [código fuente](https://github.com/Azure-Samples/cognitive-services-qnamaker-csharp/tree/master/documentation-samples/batchtesting) o como un [ejecutable descargable comprimido](https://aka.ms/qna_btzip). El formato del comando para ejecutar la prueba por lotes es:

```console
batchtesting.exe input.tsv https://YOUR-HOST.azurewebsites.net ENDPOINT-KEY out.tsv
```

|Parámetro|Valor esperado|
|--|--|
|1|Nombre del archivo TSV formateado con [campos de entrada TSV](#tsv-input-fields).|
|2|URI del punto de conexión, con YOUR-HOST de la página de publicación del portal de QnA Maker.|
|3|ENDPOINT-KEY, que se encuentra en la página de publicación del portal de QnA Maker.|
|4|Nombre del archivo TSV creado por la prueba por lotes para los resultados.|

Utilice la siguiente información para comprender e implementar el formato TSV para las pruebas por lotes. 

## <a name="tsv-input-fields"></a>Campos de entrada TSV

|Campos del archivo de entrada TSV|Notas|
|--|--|
|KBID|Identificador de KB que se encuentra en la página de publicación.|
|Pregunta|Pregunta que un usuario escribiría.|
|Etiquetas de metadatos|opcional|
|Parámetro principal|opcional| 
|Identificador de respuesta esperado|opcional|

![Formato de entrada del archivo TSV para las pruebas por lotes.](media/batch-test/input-tsv-format-batch-test.png)

## <a name="tsv-output-fields"></a>Campos de salida TSV 

|Parámetros del archivo de salida TSV|Notas|
|--|--|
|KBID|Identificador de KB que se encuentra en la página de publicación.|
|Pregunta|Pregunta tal como se especificó en el archivo de entrada.|
|Respuesta|Respuesta principal de Knowledge Base.|
|Identificador de la respuesta.|Identificador de la respuesta.|
|Score|Puntuación de predicción para la respuesta. |
|Etiquetas de metadatos|Asociadas a la respuesta devuelta.|
|Identificador de respuesta esperado|Opcional (solo cuando se proporciona el identificador de respuesta esperado).|
|Etiqueta de resolución|Opcional; los valores pueden ser: correcto o incorrecto (solo cuando se proporciona la respuesta esperada).|
