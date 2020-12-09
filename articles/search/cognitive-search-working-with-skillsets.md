---
title: Conceptos de conjunto de aptitudes y flujo de trabajo
titleSuffix: Azure Cognitive Search
description: Los conjuntos de aptitudes es donde se crea una canalización de enriquecimiento con inteligencia artificial en Azure Cognitive Search. Aprenda conceptos importantes y detalles sobre la composición del conjunto de aptitudes.
manager: nitinme
author: vkurpad
ms.author: vikurpad
ms.service: cognitive-search
ms.topic: conceptual
ms.date: 06/15/2020
ms.openlocfilehash: b5a893ee1923ba4b2bec53b20fb164337bd65902
ms.sourcegitcommit: 65a4f2a297639811426a4f27c918ac8b10750d81
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 12/03/2020
ms.locfileid: "96558120"
---
# <a name="skillset-concepts-in-azure-cognitive-search"></a>Conceptos de conjunto de aptitudes en Azure Cognitive Search

Este artículo está destinado a los desarrolladores que necesitan una mejor comprensión de los conceptos y la composición del conjunto de aptitudes, y se supone que están familiarizados con el proceso de enriquecimiento con IA. Si no está familiarizado con este concepto, comience por [Enriquecimiento con IA en Azure Cognitive Search](cognitive-search-concept-intro.md).

## <a name="introducing-skillsets"></a>Introducción a los conjuntos de aptitudes

Un conjunto de aptitudes es un recurso reutilizable en Azure Cognitive Search que está asociado con un indizador y especifica una colección de aptitudes que se usan para analizar, transformar y enriquecer el contenido de texto o imagen durante la indexación. Las aptitudes tienen entradas y salidas y, a menudo, la salida de una aptitud se convierte en la entrada de otra en una cadena o secuencia de procesos.

Un conjunto de aptitudes tiene tres propiedades principales:

+ `skills`, una colección sin ordenar de aptitudes para la que la plataforma determina la secuencia de ejecución en función de las entradas necesarias para cada aptitud.
+ `cognitiveServices`, la clave de un recurso de Cognitive Services que realiza el procesamiento de imágenes y texto para conjuntos de aptitudes que incluyen aptitudes integradas.
+ `knowledgeStore`, (opcional) la cuenta de Azure Storage en la que se proyectarán los documentos enriquecidos. Los índices de búsqueda también consumen a los documentos enriquecidos.

Los conjuntos de aptitudes se crean en JSON. El siguiente ejemplo es una versión ligeramente simplificada de este [conjunto de aptitudes hotel-reviews](https://github.com/Azure-Samples/azure-search-sample-data/blob/master/hotelreviews/HotelReviews_skillset.json), que se usa para ilustrar los conceptos de este artículo. 

A continuación se muestran las dos primeras aptitudes:

+ La aptitud n.º 1 es una [aptitud de división del texto](cognitive-search-skill-textsplit.md) que acepta el contenido del campo "reviews_text" como entrada y divide el contenido en "pages" de 5000 caracteres como salida.
+ La aptitud n.º 2 es una [aptitud de detección de opinión](cognitive-search-skill-sentiment.md) que acepta "pages" como entrada y genera un nuevo campo denominado "sentiment" como salida que contiene los resultados del análisis de opiniones.


```json
{
    "skills": [
        {
            "@odata.type": "#Microsoft.Skills.Text.SplitSkill",
            "name": "#1",
            "description": null,
            "context": "/document/reviews_text",
            "defaultLanguageCode": "en",
            "textSplitMode": "pages",
            "maximumPageLength": 5000,
            "inputs": [
                {
                    "name": "text",
                    "source": "/document/reviews_text"
                }
            ],
            "outputs": [
                {
                    "name": "textItems",
                    "targetName": "pages"
                }
            ]
        },
        {
            "@odata.type": "#Microsoft.Skills.Text.SentimentSkill",
            "name": "#2",
            "description": null,
            "context": "/document/reviews_text/pages/*",
            "defaultLanguageCode": "en",
            "inputs": [
                {
                    "name": "text",
                    "source": "/document/reviews_text/pages/*",
                }
            ],
            "outputs": [
                {
                    "name": "score",
                    "targetName": "Sentiment"
                }
            ]
        },
  "cognitiveServices": null,
  "knowledgeStore": {  }
}
```
> [!NOTE]
> Puede crear conjuntos de aptitudes complejos con bucles y ramificaciones mediante la [aptitud condicional](cognitive-search-skill-conditional.md) para crear las expresiones. La sintaxis está basada en la notación de ruta del [puntero JSON](https://tools.ietf.org/html/rfc6901) con algunas modificaciones para identificar los nodos del árbol de enriquecimiento. `"/"` atraviesa un nivel inferior en el árbol y `"*"` actúa como un operador for-each en el contexto. En los numerosos ejemplos de este artículo se muestra la sintaxis. 

### <a name="enrichment-tree"></a>Árbol de enriquecimiento

En la progresión de los [pasos de una canalización de enriquecimiento](cognitive-search-concept-intro.md#enrichment-steps), el procesamiento de contenido sigue la fase de *descifrado de documentos* en la que se extraen el texto y las imágenes del origen. Después, el contenido de la imagen se puede enrutar a las aptitudes que especifican el procesamiento de imágenes, mientras que el contenido de texto se pone en cola para su procesamiento. En el caso de los documentos de origen que contienen grandes cantidades de texto, puede configurar un *modo de análisis* en el indizador para segmentar el texto en fragmentos más pequeños para un procesamiento más óptimo. 

![Almacén de conocimiento en un diagrama de canalización](./media/knowledge-store-concept-intro/knowledge-store-concept-intro.svg "Almacén de conocimiento en un diagrama de canalización")

Una vez que un documento se encuentra en la canalización de enriquecimiento, se representa como un árbol de contenido y enriquecimientos asociados. Como salida del descifrado de documentos, se crea una instancia de este árbol.  El formato de árbol de enriquecimiento permite que la canalización de enriquecimiento adjunte metadatos incluso a tipos de datos primitivos; no es un objeto JSON válido, pero se puede proyectar en un formato JSON válido. En la tabla siguiente se muestra el estado de un documento que entra en la canalización de enriquecimiento:

|Origen de datos\Modo de análisis|Valor predeterminado|JSON, líneas JSON y CSV|
|---|---|---|
|Blob Storage|/document/content<br>/document/normalized_images/*<br>…|/document/{key1}<br>/document/{key2}<br>…|
|SQL|/document/{column1}<br>/document/{column2}<br>…|N/D |
|Cosmos DB|/document/{key1}<br>/document/{key2}<br>…|N/D|

 A medida que se ejecutan las aptitudes, agregan nuevos nodos al árbol de enriquecimiento. Estos nuevos nodos se pueden usar como entradas para aptitudes de bajada, proyectándose en el almacén de conocimiento o asignándose a campos de índice. Los enriquecimientos no son mutables: una vez creados, los nodos no se pueden editar. A medida que los conjuntos de aptitudes se vuelven más complejos, también lo hará el árbol de enriquecimiento, pero no todos los nodos del árbol de enriquecimiento deben asignarse al índice o al almacén de conocimiento. 

Puede conservar de forma selectiva solo un subconjunto de enriquecimientos en el índice o el almacén de conocimiento.

### <a name="context"></a>Context

Cada aptitud requiere un contexto. Un contexto determina:

+ El número de veces que se ejecuta la aptitud, basado en los nodos seleccionados. En el caso de valores de contexto de colección de tipos, agregar `/*` al final hará que se invoque la aptitud una vez para cada instancia de la colección. 

+ Dónde se agregan las salidas de la aptitud en el árbol de enriquecimiento. Las salidas siempre se agregan al árbol como elementos secundarios del nodo de contexto. 

+ La forma de las entradas. En el caso de colecciones de varios niveles, el establecimiento del contexto en la colección primaria afectará a la forma de la entrada de la aptitud. Por ejemplo, si tiene un árbol de enriquecimiento con una lista de países o regiones, cada uno de ellos enriquecido con una lista de estados que contiene una lista de códigos postales.

|Context|Entrada|Forma de la entrada|Invocación de la aptitud|
|-------|-----|--------------|----------------|
|`/document/countries/*` |`/document/countries/*/states/*/zipcodes/*` |Lista de todos los códigos postales del país o región |Una vez por país o región |
|`/document/countries/*/states/*` |`/document/countries/ */states/* /zipcodes/*`` |Lista de todos los códigos postales del estado | Una vez por combinación de país o región y estado|

## <a name="generate-enriched-data"></a>Generación de datos enriquecidos 

Con el [conjunto de aptitudes de reseñas de hoteles](https://github.com/Azure-Samples/azure-search-sample-data/blob/master/hotelreviews/HotelReviews_skillset.json) como punto de referencia, examinaremos lo siguiente:

+ Cómo evoluciona el árbol de enriquecimiento con la ejecución de cada aptitud.
+ Cómo funcionan el contexto y las entradas para determinar el número de veces que se ejecuta una aptitud.
+ Qué forma tiene la entrada según el contexto.

Un elemento "document" dentro del proceso de enriquecimiento representa una sola fila (una reseña de hotel) dentro del archivo de código fuente hotel_reviews.csv.

### <a name="skill-1-split-skill"></a>Aptitud 1: aptitud de división

Cuando el contenido de origen se compone de grandes fragmentos de texto, resulta útil dividirlo en componentes más pequeños para una mayor precisión en la detección del idioma, la opinión y las frases clave. Hay dos intervalos disponibles: páginas y oraciones. Una página consta de aproximadamente 5000 caracteres.

Una aptitud de división de texto suele ir primero en un conjunto de aptitudes.

```json
      "@odata.type": "#Microsoft.Skills.Text.SplitSkill",
      "name": "#1",
      "description": null,
      "context": "/document/reviews_text",
      "defaultLanguageCode": "en",
      "textSplitMode": "pages",
      "maximumPageLength": 5000,
      "inputs": [
        {
          "name": "text",
          "source": "/document/reviews_text"
        }
      ],
      "outputs": [
        {
          "name": "textItems",
          "targetName": "pages"
        }
```

Con el contexto de aptitud de `"/document/reviews_text"`, la aptitud de división se ejecutará una vez para `reviews_text`. La salida de la aptitud es una lista donde `reviews_text` se fragmenta en segmentos de 5000 caracteres. La salida de la aptitud de división se denomina `pages` y se agrega al árbol de enriquecimiento. La característica `targetName` permite cambiar el nombre de una salida de aptitud antes de agregarla al árbol de enriquecimiento.

El árbol enriquecimiento tiene ahora un nuevo nodo colocado en el contexto de la aptitud. Este nodo está disponible para cualquier aptitud, proyección o asignación de campos de salida. Conceptualmente, el árbol tiene el siguiente aspecto:

![Árbol de enriquecimiento después del descifrado de documentos](media/cognitive-search-working-with-skillsets/enrichment-tree-doc-cracking.png "Árbol de enriquecimiento después del descifrado de documentos y antes de la ejecución de la aptitud")

El nodo raíz de todos los enriquecimientos es `"/document"`. Al trabajar con indexadores de blobs, el nodo `"/document"` tendrá nodos secundarios de `"/document/content"` y `"/document/normalized_images"`. Cuando se trabaja con datos CSV, como en este ejemplo, los nombres de columna se asignan a los nodos situados debajo de `"/document"`. 

Para acceder a cualquiera de los enriquecimientos agregados a un nodo mediante una aptitud, se necesita la ruta de acceso completa para el enriquecimiento. Por ejemplo, si desea utilizar el texto del nodo ```pages``` como una entrada a otra aptitud, deberá especificarlo como ```"/document/reviews_text/pages/*"```.
 
 ![Árbol de enriquecimiento después de la aptitud 1](media/cognitive-search-working-with-skillsets/enrichment-tree-skill1.png "Árbol de enriquecimiento después de la ejecución de la aptitud 1")

### <a name="skill-2-language-detection"></a>Aptitud 2: detección de idioma

Los documentos de reseñas de hoteles incluyen comentarios de los clientes realizados en varios idiomas. La aptitud de detección de idiomas determina el idioma que se usó. A continuación, el resultado se pasa a la extracción de frases clave y a la detección de opiniones, que toma en cuenta el idioma durante la detección de la opinión y las frases.

Aunque la aptitud de detección de idioma es la tercera (aptitud 3) definida en el conjunto de aptitudes, es la siguiente aptitud que se ejecuta. Dado que no se bloquea al no requerir entradas, se ejecutará en paralelo con la aptitud anterior. Al igual que la aptitud de división que la precedía, la aptitud de detección de idioma también se invoca una vez para cada documento. El árbol de enriquecimiento tiene ahora un nuevo nodo para Language.

 ![Árbol de enriquecimiento después de la aptitud 2](media/cognitive-search-working-with-skillsets/enrichment-tree-skill2.png "Árbol de enriquecimiento después de la ejecución de la aptitud 2")
 
 ### <a name="skill-3-key-phrases-skill"></a>Aptitud 3: aptitud de frases clave 

Dado el contexto de `/document/reviews_text/pages/*`, la aptitud de frases clave se invocará una vez para cada uno de los elementos de la colección `pages`. La salida de la aptitud será un nodo bajo el elemento de página asociado. 

 Ahora deberá poder observar el resto de las aptitudes del conjunto y visualizar cómo seguirá creciendo el árbol de enriquecimientos con la ejecución de cada una de las aptitudes. Algunas aptitudes, como la aptitud de combinación y la aptitud de conformador, también crean nuevos nodos, pero solo usan datos de los nodos existentes y no crean nuevos enriquecimientos netos.

![Árbol de enriquecimiento después de todas las aptitudes](media/cognitive-search-working-with-skillsets/enrichment-tree-final.png "Árbol de enriquecimiento después de todas las aptitudes")

Los colores de los conectores en el árbol anterior indican que los enriquecimientos se crearon mediante distintas aptitudes y que los nodos deben abordarse de forma individual y no formarán parte del objeto devuelto al seleccionar el nodo primario.

## <a name="save-enrichments"></a>Guardado de enriquecimientos

En Azure Cognitive Search, un indizador guarda el resultado que crea. Una de las salidas es siempre un [índice de búsqueda](search-what-is-an-index.md). La especificación de un índice es un requisito y, al adjuntar un conjunto de aptitudes, los datos ingeridos por un índice incluyen el contenido de los enriquecimientos. Normalmente, las salidas de aptitudes específicas, como frases clave o puntuaciones de opinión, se ingieren en el índice de un campo creado para ese fin.

Un indizador también puede enviar la salida a un [almacén de conocimiento](knowledge-store-concept-intro.md) para su consumo en otras herramientas o procesos. Un almacén de conocimientos se define como parte del conjunto de aptitudes. Su definición determina si los documentos enriquecidos se proyectan como tablas u objetos (archivos o blobs). Las proyecciones tabulares son adecuadas para el análisis interactivo en herramientas como Power BI, mientras que los archivos y los blobs se usan normalmente en la ciencia de datos o en procesos similares. En esta sección, aprenderá la forma en que la composición de un conjunto de aptitudes puede dar forma a las tablas u objetos que quiere proyectar.

### <a name="projections"></a>Proyecciones

En el caso de contenido dirigido a un almacén de información, debería preguntarse cómo se estructura el contenido. La *proyección* es el proceso de seleccionar nodos del árbol de enriquecimiento y crear una expresión física de estos en el almacén de conocimiento. Las proyecciones son formas personalizadas del documento (contenido y enriquecimientos) que se pueden generar como proyecciones de tabla u objeto. Para más información sobre cómo trabajar con proyecciones, consulte [Trabajar con proyecciones](knowledge-store-projection-overview.md).

![Opciones de asignación de campos](./media/cognitive-search-working-with-skillsets/field-mapping-options.png "Opciones de asignación de campos para la canalización de enriquecimiento")

### <a name="sourcecontext"></a>SourceContext

El elemento `sourceContext` solo se usa en proyecciones y entradas de aptitudes. Se utiliza para construir objetos anidados de varios niveles. Es posible que tenga que crear un nuevo objeto para pasarlo como una entrada a una aptitud o proyecto en el almacén de conocimiento. Dado que los nodos de enriquecimiento pueden no ser un objeto JSON válido en el árbol de enriquecimiento y que la referencia a un nodo del árbol solo devuelve el estado del nodo cuando se creó, el uso de enriquecimientos como proyecciones o entradas de aptitudes requiere la creación de un objeto JSON bien formado. `sourceContext` permite construir un objeto de tipo anónimo y jerárquico, que requeriría varias aptitudes si solo usara el contexto. 

En el ejemplo siguiente se muestra el uso de `sourceContext`. Observe la salida de aptitud que generó un enriquecimiento para determinar si es un objeto JSON válido y no un tipo primitivo.

### <a name="slicing-projections"></a>Segmentación de proyecciones

Al definir un grupo de proyecciones de tabla, se puede segmentar un solo nodo del árbol de enriquecimiento en varias tablas relacionadas. Si agrega una tabla con una ruta de acceso de origen que sea un elemento secundario de una proyección de tabla existente, el nodo secundario resultante no será un elemento secundario de la proyección de tabla existente, sino que se proyectará en la nueva tabla relacionada. Esta técnica de segmentación permite definir un único nodo en una aptitud de conformador que puede ser el origen de todas sus proyecciones de tabla. 

### <a name="shaping-projections"></a>Modelado de proyecciones

Hay dos maneras de definir una proyección:

+ Use la aptitud de conformador de texto para crear un nuevo nodo que sea el nodo raíz para todos los enriquecimientos que proyecta. Después, en las proyecciones, solo haría referencia a la salida de la aptitud de conformador.

+ Use una forma en línea para crear una proyección en la propia definición de la proyección.

El método del conformador es más detallado que la forma en línea, pero garantiza que todas las mutaciones del árbol de enriquecimiento estén contenidas en las aptitudes y que la salida sea un objeto que se pueda volver a usar. Por el contrario, la forma en línea le permite crear la forma que necesita, pero es un objeto anónimo y solo está disponible para la proyección para la que se define. Estos enfoques se pueden usar juntos o por separado. El conjunto de aptitudes creado en el flujo de trabajo del portal contiene ambos. Usa una aptitud de conformador para las proyecciones de tabla, pero también utiliza la forma en línea para proyectar la tabla de frases clave.

Para ampliar el ejemplo, puede optar por quitar la forma en línea y usar una aptitud de conformador para crear un nuevo nodo para las frases clave. Para crear una forma proyectada en tres tablas, es decir `hotelReviewsDocument`, `hotelReviewsPages`, y `hotelReviewsKeyPhrases`, se describen las dos opciones en las secciones siguientes.

#### <a name="shaper-skill-and-projection"></a>Aptitud de conformador y proyección

> [!Note]
> Algunas de las columnas de la tabla de documentos se han quitado de este ejemplo para mayor simplicidad.
>
```json
{
    "@odata.type": "#Microsoft.Skills.Util.ShaperSkill",
    "name": "#5",
    "description": null,
    "context": "/document",
    "inputs": [        
        {
            "name": "reviews_text",
            "source": "/document/reviews_text",
            "sourceContext": null,
            "inputs": []
        },
        {
            "name": "reviews_title",
            "source": "/document/reviews_title",
            "sourceContext": null,
            "inputs": []
        },
        {
            "name": "AzureSearch_DocumentKey",
            "source": "/document/AzureSearch_DocumentKey",
            "sourceContext": null,
            "inputs": []
        },  
        {
            "name": "pages",
            "source": null,
            "sourceContext": "/document/reviews_text/pages/*",
            "inputs": [
                {
                    "name": "SentimentScore",
                    "source": "/document/reviews_text/pages/*/Sentiment",
                    "sourceContext": null,
                    "inputs": []
                },
                {
                    "name": "LanguageCode",
                    "source": "/document/Language",
                    "sourceContext": null,
                    "inputs": []
                },
                {
                    "name": "Page",
                    "source": "/document/reviews_text/pages/*",
                    "sourceContext": null,
                    "inputs": []
                },
                {
                    "name": "keyphrase",
                    "sourceContext": "/document/reviews_text/pages/*/Keyphrases/*",
                    "inputs": [
                        {
                            "source": "/document/reviews_text/pages/*/Keyphrases/*",
                            "name": "Keyphrases"
                        }
                    ]
                }
            ]
        }
    ],
    "outputs": [
        {
            "name": "output",
            "targetName": "tableprojection"
        }
    ]
}
```

Con el nodo `tableprojection` definido en la sección `outputs` anterior, ahora podemos usar la característica de segmentación para proyectar partes del nodo `tableprojection` en tablas diferentes:

> [!Note]
> Este es solo un fragmento de la proyección dentro de la configuración del almacén de conocimiento.
>
```json
"projections": [
    {
        "tables": [
            {
                "tableName": "hotelReviewsDocument",
                "generatedKeyName": "Documentid",
                "source": "/document/tableprojection"
            },
            {
                "tableName": "hotelReviewsPages",
                "generatedKeyName": "Pagesid",
                "source": "/document/tableprojection/pages/*"
            },
            {
                "tableName": "hotelReviewsKeyPhrases",
                "generatedKeyName": "KeyPhrasesid",
                "source": "/document/tableprojection/pages/*/keyphrase/*"
            }
        ]
    }
]
```

#### <a name="inline-shaping-projections"></a>Modelado de proyecciones en línea

El método de modelado en línea no requiere una aptitud de conformador, ya que todas las formas necesarias para las proyecciones se crean en el momento en que se necesitan. Para proyectar los mismos datos que en el ejemplo anterior, la opción de proyección en línea tendría el siguiente aspecto:

```json
"projections": [
    {
        "tables": [
            {
                "tableName": "hotelReviewsInlineDocument",
                "generatedKeyName": "Documentid",
                "sourceContext": "/document",     
                "inputs": [
                    {
                        "name": "reviews_text",
                        "source": "/document/reviews_text"
                    },
                    {
                        "name": "reviews_title",
                        "source": "/document/reviews_title"
                    },
                    {
                        "name": "AzureSearch_DocumentKey",
                        "source": "/document/AzureSearch_DocumentKey"
                    }                             
                ]
            },
            {
                "tableName": "hotelReviewsInlinePages",
                "generatedKeyName": "Pagesid",
                "sourceContext": "/document/reviews_text/pages/*",
                "inputs": [
                        {
                    "name": "SentimentScore",
                    "source": "/document/reviews_text/pages/*/Sentiment"
                    },
                    {
                        "name": "LanguageCode",
                        "source": "/document/Language"
                    },
                    {
                        "name": "Page",
                        "source": "/document/reviews_text/pages/*"
                    }
                ]
            },
            {
                "tableName": "hotelReviewsInlineKeyPhrases",
                "generatedKeyName": "KeyPhraseId",
                "sourceContext": "/document/reviews_text/pages/*/Keyphrases/*",
                "inputs": [
                    {
                        "name": "Keyphrases",
                        "source": "/document/reviews_text/pages/*/Keyphrases/*"
                    }
                ]
            }
        ]
    }
]
```
  
Un ejemplo de ambos enfoques es la proyección de los valores de `"Keyphrases"` mediante `"sourceContext"`. El nodo `"Keyphrases"`, que contiene una colección de cadenas, es en sí mismo un elemento secundario del texto de la página. Sin embargo, dado que las proyecciones requieren un objeto JSON y la página es primitiva (cadena), `"sourceContext"` se usa para encapsular la frase clave en un objeto con una propiedad con nombre. Esta técnica permite que incluso se proyecten primitivas de forma independiente.

## <a name="next-steps"></a>Pasos siguientes

Como paso siguiente, cree su primer conjunto de aptitudes con aptitudes cognitivas.

> [!div class="nextstepaction"]
> [Cree su primer conjunto de aptitudes](cognitive-search-defining-skillset.md).
