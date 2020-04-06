---
title: 'Importación de orígenes de datos: QnA Maker'
description: Una base de conocimiento de QnA Maker consta de una serie de conjuntos de preguntas y respuestas, así como metadatos opcionales asociados a cada par de pregunta y respuesta.
ms.topic: conceptual
ms.date: 03/16/2020
ms.openlocfilehash: e7772b2c76dfb92ced4d2c0d4b8dae97a59d1b83
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/28/2020
ms.locfileid: "79457828"
---
# <a name="importing-from-data-sources"></a>Importación de orígenes de datos

Una base de conocimiento consta de conjuntos de preguntas y respuestas incorporados mediante archivos y direcciones URL públicos.

## <a name="data-source-locations"></a>Ubicaciones de orígenes de datos

El contenido se incluye en una base de conocimiento desde un origen de datos. Las ubicaciones de origen de datos son **direcciones URL o archivos públicos**, que no requieren autenticación.

[Los archivos de SharePoint](../how-to/add-sharepoint-datasources.md), que están protegidos con autenticación, son la excepción. Los recursos de SharePoint deben ser archivos, no páginas web. Si la dirección URL finaliza con una extensión web, como .ASPX, no se importará en QnA Maker desde SharePoint.

## <a name="chit-chat-content"></a>Contenido de la charla de prueba

El conjunto de contenido de preguntas y respuestas de la charla se ofrece como un origen de datos de contenido completo en varios idiomas y estilos de conversación. Esto puede ser un punto de partida para la personalidad del bot y permite ahorrar el tiempo y el costo que supone escribirlas desde cero. Aprenda [cómo agregar](../how-to/chit-chat-knowledge-base.md) este conjunto de contenido a su base de conocimiento.

## <a name="structured-data-format-through-import"></a>Formato de datos estructurado mediante importación

Al importar una base de conocimiento, se reemplaza el contenido de la base de conocimiento existente. La importación requiere un archivo `.tsv` estructurado que contenga preguntas y una respuesta. Esta información ayuda a QnA Maker a agrupar los conjuntos pregunta-respuesta y a atribuirlos a un origen de datos particular.

| Pregunta  | Respuesta  | Source| Metadatos (1 clave: 1 valor) |
|-----------|---------|----|---------------------|
| Pregunta1 | Respuesta1 | URL1 | <code>Key1:Value1 &#124; Key2:Value2</code> |
| Pregunta2 | Respuesta2 | Editorial|    `Key:Value`       |

## <a name="structured-multi-turn-format-through-import"></a>Formato multiturno estructurado mediante importación

Puede crear conversaciones multiturno en un formato de archivo `.tsv`. Este formato le permite crear conversaciones multiturno mediante el análisis de los registros de chat anteriores (con otros procesos, sin usar QnA Maker). A continuación, puede crear un archivo `.tsv` mediante automatización. Importe el archivo para reemplazar la base de conocimiento existente.

> [!div class="mx-imgBorder"]
> ![Modelo conceptual de preguntas de varios turnos de tres niveles](../media/qnamaker-concepts-knowledgebase/nested-multi-turn.png)

La columna de un archivo `.tsv` de varios turnos que refiere específicamente a dicha característica es **Prompts**. El siguiente es un `.tsv` de ejemplo (mostrado en Excel) que muestra la información que se va a incluir para definir los elementos secundarios multiturno:

```JSON
[
    {"displayOrder":0,"qnaId":2,"displayText":"Level 2 Question A"},
    {"displayOrder":0,"qnaId":3,"displayText":"Level 2 - Question B"}
]
```

La propiedad **displayOrder** es numérica, y **displayText** es texto que no debe incluir Markdown.

> [!div class="mx-imgBorder"]
> ![Ejemplo de pregunta multiturno como se ve en Excel](../media/qnamaker-concepts-knowledgebase/multi-turn-tsv-columns-excel-example.png)

## <a name="export-as-example"></a>Exportación como ejemplo

Si no está seguro de cómo representar el conjunto de QnA en el archivo `.tsv`:
* Use este [ejemplo descargable de GitHub](https://github.com/Azure-Samples/cognitive-services-sample-data-files/blob/master/qna-maker/data-source-formats/Structured-multi-turn-format.xlsx?raw=true).
* O bien, cree el conjunto en el portal de QnA Maker, guárdelo y, luego, exporte la base de conocimiento para ver un ejemplo de cómo representar el conjunto.

## <a name="next-steps"></a>Pasos siguientes

> [!div class="nextstepaction"]
> [Ciclo de vida de desarrollo de una base de conocimiento](./development-lifecycle-knowledge-base.md)

## <a name="see-also"></a>Consulte también

Use la [referencia de Markdown](../reference-markdown-format.md) de QnA Maker como guía para dar formato a las respuestas.

[Introducción de QnA Maker](../Overview/overview.md)

Cree y edite la base de conocimiento con:
* [REST API](https://docs.microsoft.com/rest/api/cognitiveservices/qnamaker/knowledgebase)
* [SDK de .NET](https://docs.microsoft.com/dotnet/api/microsoft.azure.cognitiveservices.knowledge.qnamaker.knowledgebase?view=azure-dotnet)

Genere una respuesta con:
* [REST API](https://docs.microsoft.com/rest/api/cognitiveservices/qnamakerruntime/runtime/generateanswer)
* [SDK de .NET](https://docs.microsoft.com/dotnet/api/microsoft.azure.cognitiveservices.knowledge.qnamaker.runtime?view=azure-dotnet)
