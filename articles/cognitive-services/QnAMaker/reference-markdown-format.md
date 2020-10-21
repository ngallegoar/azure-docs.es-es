---
title: 'Formato Markdown: QnA Maker'
description: A continuación se muestra la lista de formatos de Markdown que puede usar en el texto de respuesta de QnA Maker.
ms.service: cognitive-services
ms.subservice: qna-maker
ms.topic: reference
ms.date: 03/19/2020
ms.openlocfilehash: f5f640f8895cd6defe31a68b790ce841c4679a6f
ms.sourcegitcommit: 829d951d5c90442a38012daaf77e86046018e5b9
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/09/2020
ms.locfileid: "91777364"
---
# <a name="markdown-format-supported-in-qna-maker-answer-text"></a>Formato Markdown admitido en el texto de respuesta de QnA Maker

QnA Maker almacena el texto de respuesta como Markdown. Hay muchos tipos de Markdown. Para asegurarse de que el texto de respuesta se devuelve y se muestra correctamente, use esta referencia.

Use el tutorial **[CommonMark](https://commonmark.org/help/tutorial/index.html)** para validar su Markdown. El tutorial tiene una característica **Pruébelo** para la validación rápida de copiar y pegar.

## <a name="when-to-use-rich-text-editing-versus-markdown"></a>Cuándo usar la edición de texto enriquecido en lugar de Markdown

La [edición de texto enriquecido](How-To/edit-knowledge-base.md#add-an-editorial-qna-set) de las respuestas le permite, como autor, usar una barra de herramientas de formato para seleccionar y dar formato al texto rápidamente.

Markdown es la mejor herramienta cuando se necesita generar contenido automáticamente para crear bases de conocimiento que se van a importar como parte de una canalización de CI/CD o para [pruebas por lotes](Quickstarts/batch-testing.md).

## <a name="supported-markdown-format"></a>Formato Markdown compatible

A continuación se muestra la lista de formatos de Markdown que puede usar en el texto de respuesta de QnA Maker.

|Propósito|Formato|Markdown de ejemplo|Representación<br>tal como se muestra en el bot de chat|
|--|--|--|--|
Nueva línea entre dos oraciones.|`\n\n`|`How can I create a bot with \n\n QnA Maker?`|![dar formato a una nueva línea entre dos oraciones](./media/qnamaker-concepts-datasources/format-newline.png)|
|Encabezados de h1 a h6, el número de `#` indica el encabezado. 1 `#` es el h1.|`\n# text \n## text \n### text \n####text \n#####text` |`## Creating a bot \n ...text.... \n### Important news\n ...text... \n### Related Information\n ....text...`<br><br>`\n# my h1 \n## my h2\n### my h3 \n#### my h4 \n##### my h5`|![formato con encabezados de Markdown](./media/qnamaker-concepts-datasources/format-headers.png)<br>![formato con encabezados de Markdown H1 a H5](./media/qnamaker-concepts-datasources/format-h1-h5.png)|
|Cursiva |`*text*`|`How do I create a bot with *QnA Maker*?`|![formato con cursiva](./media/qnamaker-concepts-datasources/format-italics.png)|
|Negrita|`**text**`|`How do I create a bot with **QnA Maker**?`|![formato con marcado fuerte para negrita](./media/qnamaker-concepts-datasources/format-strong.png)|
|Dirección URL del vínculo|`[text](https://www.my.com)`|`How do I create a bot with [QnA Maker](https://www.qnamaker.ai)?`|![formato de dirección URL (hipervínculo)](./media/qnamaker-concepts-datasources/format-url.png)|
|*Dirección URL de imagen pública|`![text](https://www.my.com/image.png)`|`How can I create a bot with ![QnAMaker](https://review.docs.microsoft.com/azure/cognitive-services/qnamaker/media/qnamaker-how-to-key-management/qnamaker-resource-list.png)`|![formato de la dirección URL de imagen pública ](./media/qnamaker-concepts-datasources/format-image-url.png)|
|Tachado|`~~text~~`|`some ~~questoins~~ questions need to be asked`|![formato del tachado](./media/qnamaker-concepts-datasources/format-strikethrough.png)|
|Negrita y cursiva|`***text***`|`How can I create a ***QnA Maker*** bot?`|![formato de negrita y cursiva](./media/qnamaker-concepts-datasources/format-bold-italics.png)|
|Dirección URL en negrita del vínculo.|`[**text**](https://www.my.com)`|`How do I create a bot with [**QnA Maker**](https://www.qnamaker.ai)?`|![formato para la dirección URL en negrita](./media/qnamaker-concepts-datasources/format-bold-url.png)|
|Dirección URL en cursiva del vínculo.|`[*text*](https://www.my.com)`|`How do I create a bot with [*QnA Maker*](https://www.qnamaker.ai)?`|![formato de la dirección URL en cursiva](./media/qnamaker-concepts-datasources/format-url-italics.png)|
|Símbolos de escape de Markdown|`\*text\*`|`How do I create a bot with \*QnA Maker\*?`|![formato de símbolos de escape de Markdown](./media/qnamaker-concepts-datasources/format-escape-markdown-symbols.png)|
|Lista ordenada|`\n 1. item1 \n 1. item2`|`This is an ordered list: \n 1. List item 1 \n 1. List item 2`<br>En el ejemplo anterior se utiliza la numeración automática integrada en Markdown.<br>`This is an ordered list: \n 1. List item 1 \n 2. List item 2`<br>En el ejemplo anterior se usa la numeración explícita.|![formato de la lista ordenada](./media/qnamaker-concepts-datasources/format-ordered-list.png)|
|Lista sin ordenar|`\n * item1 \n * item2`<br>or<br>`\n - item1 \n - item2`|`This is an unordered list: \n * List item 1 \n * List item 2`|![formato de la lista sin ordenar](./media/qnamaker-concepts-datasources/format-unordered-list.png)|
|Listas anidadas|`\n * Parent1 \n\t * Child1 \n\t * Child2 \n * Parent2`<br><br>`\n * Parent1 \n\t 1. Child1 \n\t * Child2 \n 1. Parent2`<br><br>Puede anidar listas ordenadas y sin ordenar juntas. La pestaña, `\t`, indica el nivel de sangría del elemento secundario.|`This is an unordered list: \n * List item 1 \n\t * Child1 \n\t * Child2 \n * List item 2`<br><br>`This is an ordered nested list: \n 1. Parent1 \n\t 1. Child1 \n\t 1. Child2 \n 1. Parent2`|![formato de la lista sin ordenar anidada](./media/qnamaker-concepts-datasources/format-nested-unordered-list.png)<br>![formato de la lista ordenada anidada](./media/qnamaker-concepts-datasources/format-nested-ordered-list.png)|

\* QnA Maker no procesa la imagen de ninguna manera. Es el rol de la aplicación cliente para representar la imagen.

Si quiere agregar contenido mediante las API de base de conocimiento de actualización y sustitución y el contenido o el archivo incluyen etiquetas HTML, puede conservar el código HTML en el archivo si se asegura de que la apertura y el cierre de las etiquetas se convierten en el formato codificado.

| Conservar HTML  | Representación en la solicitud de API  | Representación en la base de conocimiento |
|-----------|---------|-------------------------|
| Sí | \&lt;br\&gt; | &lt;br&gt; |
| Sí | \&lt;h3\&gt;header\&lt;/h3\&gt; | &lt;h3&gt;header&lt;/h3&gt; |

Además, CR LF(\r\n) se convierte en \n en la base de conocimiento. LF(\n) se queda como está. Si quiere escapar cualquier secuencia de escape, como \t o \n, puede usar una barra diagonal inversa, por ejemplo: "\\\\r\\\\n" y "\\\\t".

## <a name="next-steps"></a>Pasos siguientes

Revise los [formatos de archivo](reference-tsv-format-batch-testing.md) de las pruebas por lotes.
