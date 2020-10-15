---
title: 'Instrucciones de formato de documentos importados: QnA Maker'
description: Siga estas instrucciones para importar documentos a fin de obtener los mejores resultados de su contenido.
ms.service: cognitive-services
ms.subservice: qna-maker
ms.topic: reference
ms.date: 04/06/2020
ms.openlocfilehash: d35ea57a68e500deffa99033b83114ec518dd5e3
ms.sourcegitcommit: 829d951d5c90442a38012daaf77e86046018e5b9
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/09/2020
ms.locfileid: "91776738"
---
# <a name="format-guidelines-for-imported-documents-and-urls"></a>Instrucciones de formato para las direcciones URL y los documentos importados

Revise estas instrucciones de formato para obtener los mejores resultados de su contenido.

## <a name="formatting-considerations"></a>Consideraciones de formato

Después de importar un archivo o una dirección URL, QnA Maker convierte y almacena el contenido en [formato de Markdown](https://en.wikipedia.org/wiki/Markdown). El proceso de conversión agrega nuevas líneas al texto, como `\n\n`. Conocer el formato de Markdown le ayuda a comprender el contenido convertido y a administrar el contenido de la base de conocimiento.

Si agrega o edita el contenido directamente en la base de conocimiento, use el **formato de Markdown** para crear contenido de texto enriquecido o cambiar el contenido de dicho formato que ya está en la respuesta. QnA Maker admite gran parte del formato de Markdown para aportar funcionalidades de texto enriquecido al contenido. Sin embargo, es posible que la aplicación cliente, como un bot de chat, no admita el mismo conjunto de formatos de Markdown. Es importante probar la presentación de las respuestas de la aplicación cliente.

Vea una lista completa de [tipos de contenido y ejemplos](./Concepts/content-types.md#file-and-url-data-types).

## <a name="basic-document-formatting"></a>Formato básico de documentos

QnA Maker identifica las secciones y subsecciones, así como las relaciones, en el archivo según las indicaciones visuales, como:

* tamaño de fuente
* estilo de fuente
* numeración
* colores

## <a name="product-manuals"></a>Manuales de productos

Un manual suele ser material de orientación que acompaña a un producto. Ayuda al usuario a configurar, usar y mantener el producto y a solucionar problemas con el mismo. Cuando QnA Maker procesa un manual, extrae los títulos y subtítulos como preguntas y el contenido ulterior como respuestas. Consulte un ejemplo [aquí](https://download.microsoft.com/download/2/9/B/29B20383-302C-4517-A006-B0186F04BE28/surface-pro-4-user-guide-EN.pdf).

A continuación se muestra un ejemplo de un manual con una página de índice y contenido jerárquico

 ![Ejemplo de manual del producto para una base de conocimiento](./media/qnamaker-concepts-datasources/product-manual.png)

> [!NOTE]
> La extracción funciona mejor en manuales que tienen una tabla de contenido o una página de índice y una estructura clara con títulos jerárquicos.

## <a name="brochures-guidelines-papers-and-other-files"></a>Folletos, instrucciones, documentos y otros archivos

También se pueden procesar muchos otros tipos de documentos para generar pares de preguntas y respuestas, siempre que tengan una estructura y un diseño claros. Entre ellas se incluyen las siguientes: folletos, instrucciones, informes, documentos técnicos, documentos científicos, directivas, libros, etc. Consulte un ejemplo [aquí](https://qnamakerstore.blob.core.windows.net/qnamakerdata/docs/Manage%20Azure%20Blob%20Storage.docx).

A continuación se muestra un ejemplo de documento semiestructurado, sin un índice:

 ![Documento semiestructurado de Azure Blob Storage](./media/qnamaker-concepts-datasources/semi-structured-doc.png)

## <a name="structured-qna-document"></a>Documento de preguntas y respuestas estructurado

El formato de tipo pregunta-respuestas estructurado en los archivos de documento (DOC) consiste en alternar preguntas y respuestas por línea, una pregunta por línea seguida de su respuesta en la línea siguiente, tal como se muestra a continuación:

```text
Question1

Answer1

Question2

Answer2
```

A continuación se muestra un ejemplo de un documento de Word de preguntas y respuestas estructurado:

 ![Ejemplo de documento de QnA estructurado para una base de conocimiento](./media/qnamaker-concepts-datasources/structured-qna-doc.png)

## <a name="structured-txt-tsv-and-xls-files"></a>Archivos *TXT*, *TSV* y *XLS* estructurados

Las preguntas y respuestas en forma de archivos *.txt*, *.tsv* o *.xls* estructurados también se pueden cargar en QnA Maker para crear o ampliar una base de conocimiento.  Pueden ser texto sin formato o pueden tener contenido en formato RTF o HTML.

| Pregunta  | Respuesta  | Metadatos (1 clave: 1 valor) |
|-----------|---------|-------------------------|
| Pregunta1 | Respuesta1 | <code>Key1:Value1 &#124; Key2:Value2</code> |
| Pregunta2 | Respuesta2 |      `Key:Value`           |

Se ignoran las columnas adicionales del archivo de origen.

### <a name="example-of-structured-excel-file"></a>Ejemplo de archivo de Excel estructurado

A continuación se muestra un ejemplo de archivo *.xls* de preguntas y respuestas estructurado, con contenido HTML:

 ![Ejemplo de libro Excel de QnA estructurado para una base de conocimiento](./media/qnamaker-concepts-datasources/structured-qna-xls.png)

### <a name="example-of-alternate-questions-for-single-answer-in-excel-file"></a>Ejemplo de preguntas alternativas para una sola respuesta en un archivo de Excel

A continuación se muestra un ejemplo de un archivo *.xls* de QnA estructurado, con varias preguntas alternativas para una sola respuesta:

 ![Ejemplo de preguntas alternativas para una sola respuesta en un archivo de Excel](./media/qnamaker-concepts-datasources/xls-alternate-question-example.png)

Una vez importado el archivo, el par pregunta-respuesta se encuentra en la base de conocimiento, como se muestra a continuación:

 ![Captura de pantalla de preguntas alternativas para una sola respuesta importada en la base de conocimiento](./media/qnamaker-concepts-datasources/xls-alternate-question-example-after-import.png)

## <a name="structured-data-format-through-import"></a>Formato de datos estructurado mediante importación

Al importar una base de conocimiento, se reemplaza el contenido de la base de conocimiento existente. La importación requiere un archivo .tsv estructurado que contiene información del origen de datos. Esta información ayuda a QnA Maker a agrupar los pares pregunta-respuesta y a atribuirlos a un origen de datos particular.

| Pregunta  | Respuesta  | Source| Metadatos (1 clave: 1 valor) |
|-----------|---------|----|---------------------|
| Pregunta1 | Respuesta1 | URL1 | <code>Key1:Value1 &#124; Key2:Value2</code> |
| Pregunta2 | Respuesta2 | Editorial|    `Key:Value`       |

<a href="#formatting-considerations"></a>

## <a name="multi-turn-document-formatting"></a>Formato de documento de varios turnos

* Use encabezados y subtítulos para denotar la jerarquía. Por ejemplo, puede agregar h1 para indicar las preguntas y respuestas principales y h2 para las que se deben tomar como aviso. Use el tamaño de encabezado pequeño para indicar la jerarquía subsiguiente. No utilice el estilo, el color u otro mecanismo para inferir la estructura en el documento, QnA Maker no extraerá las indicaciones multiturno.
* El primer carácter del encabezado debe escribirse en mayúsculas.
* No termine un encabezado con un signo de interrogación, `?`.

**Documentos de muestra**:<br>[Surface Pro (docx)](https://github.com/Azure-Samples/cognitive-services-sample-data-files/blob/master/qna-maker/data-source-formats/multi-turn.docx)<br>[Ventajas de Contoso (docx)](https://github.com/Azure-Samples/cognitive-services-sample-data-files/blob/master/qna-maker/data-source-formats/Multiturn-ContosoBenefits.docx)<br>[Ventajas de Contoso (pdf)](https://github.com/Azure-Samples/cognitive-services-sample-data-files/blob/master/qna-maker/data-source-formats/Multiturn-ContosoBenefits.pdf)

## <a name="next-steps"></a>Pasos siguientes

Vea una lista completa de [tipos de contenido y ejemplos](./Concepts/content-types.md#file-and-url-data-types).