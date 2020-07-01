---
title: 'Inicio rápido: Entrenamiento de un modelo y extracción de datos de formularios mediante cURL (Form Recognizer)'
titleSuffix: Azure Cognitive Services
description: En este inicio rápido, usará la API de REST de Form Recognizer con cURL para entrenar un modelo y extraer datos de formularios.
author: PatrickFarley
manager: nitinme
ms.service: cognitive-services
ms.subservice: forms-recognizer
ms.topic: quickstart
ms.date: 05/27/2020
ms.author: pafarley
ms.openlocfilehash: 0abc98c95c03e3dd2e12a601188d9c5f7cb4523d
ms.sourcegitcommit: 73ac360f37053a3321e8be23236b32d4f8fb30cf
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 06/30/2020
ms.locfileid: "85558984"
---
# <a name="quickstart-train-a-form-recognizer-model-and-extract-form-data-by-using-the-rest-api-with-curl"></a>Inicio rápido: Entrenamiento de un modelo de Form Recognizer y extracción de los datos del formulario mediante la API de REST con cURL

En este inicio rápido, usará la API de REST de Azure Form Recognizer con cURL para entrenar y puntuar formularios para extraer tablas y pares clave-valor.

Si no tiene una suscripción a Azure, cree una [cuenta gratuita](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) antes de empezar.

## <a name="prerequisites"></a>Prerrequisitos

Para completar este inicio rápido, debe cumplir los siguientes requisitos:
- [cURL](https://curl.haxx.se/windows/) instalado.
- Un conjunto de al menos seis formularios del mismo tipo. Para entrenar el modelo utilizará cinco de ellos, y el sexto formulario le servirá para probarlo. Los formularios pueden ser diferentes tipos de archivo, pero deben ser del mismo tipo de documento. En este inicio rápido puede usar un [conjunto de datos de ejemplo](https://go.microsoft.com/fwlink/?linkid=2090451). Cargue los archivos de entrenamiento en la raíz de un contenedor de almacenamiento de blobs de una cuenta de Azure Storage. Puede colocar los archivos de prueba en una carpeta independiente.

## <a name="create-a-form-recognizer-resource"></a>Creación de un recurso de Form Recognizer

[!INCLUDE [create resource](../includes/create-resource.md)]

## <a name="train-a-form-recognizer-model"></a>Entrenamiento de un modelo de Form Recognizer

En primer lugar, se necesitará un conjunto de datos de entrenamiento de un blob de Azure Storage. Debe tener como mínimo cinco formularios rellenados (documentos PDF o imágenes) de la misma estructura y tipo que los datos de entrada principales. O bien, puede usar un formulario vacío con dos formularios rellenos. El nombre de archivo del primero formulario debe incluir la palabra "empty". Consulte [Build a training data set for a custom model](../build-training-data-set.md) (Creación de un conjunto de datos de aprendizaje para un modelo personalizado) para ver sugerencias y opciones para reunir los datos de entrenamiento.

> [!NOTE]
> Puede usar la característica de datos etiquetados para etiquetar manualmente algunos o todos los datos de entrenamiento de antemano. Se trata de un proceso más complejo, pero el resultado es un modelo mejor entrenado. Consulte la sección [Entrenamiento con etiquetas](../overview.md#train-with-labels) de la introducción para más información acerca de esta característica.

Para entrenar un modelo de Form Recognizer con los documentos del contenedor de blobs de Azure, llame a la API **[Train Custom Model](https://westus2.dev.cognitive.microsoft.com/docs/services/form-recognizer-api-v2-preview/operations/TrainCustomModelAsync)** mediante la ejecución del siguiente comando de cURL. Antes de ejecutar el comando, realice estos cambios:

1. Reemplace `<Endpoint>` por el punto de conexión que obtuvo con la suscripción de Form Recognizer.
1. Reemplace `<subscription key>` con la clave de suscripción que copió en el paso anterior.
1. Reemplace `<SAS URL>` por la dirección URL de la firma de acceso compartido (SAS) del contenedor de almacenamiento de blobs de Azure. Para recuperar la dirección URL de SAS, abra el Explorador de Microsoft Azure Storage, haga clic con el botón derecho en el contenedor y seleccione **Get shared access signature** (Obtener firma de acceso compartido). Asegúrese de que los permisos de **lectura** y **enumeración** están marcados y haga clic en **Create** (Crear). A continuación, copie el valor en la sección **URL**. Debe tener el formato `https://<storage account>.blob.core.windows.net/<container name>?<SAS value>`.

```bash
curl -i -X POST "https://<Endpoint>/formrecognizer/v2.0/custom/models" -H "Content-Type: application/json" -H "Ocp-Apim-Subscription-Key: <subscription key>" --data-ascii "{ \"source\": \""<SAS URL>"\"}"
```

Recibirá una respuesta `201 (Success)` con un encabezado **Location** (Ubicación). El valor de este encabezado es el identificador del nuevo modelo que se va a entrenar. 

## <a name="get-training-results"></a>Obtención de los resultados del entrenamiento

Una vez iniciada la operación de entrenamiento, utilizará una nueva operación, **[Obtención del modelo personalizado](https://westus2.dev.cognitive.microsoft.com/docs/services/form-recognizer-api-v2-preview/operations/GetCustomModel)** , para comprobar el estado del entrenamiento. Pase el identificador de modelo a esta llamada API para comprobar el estado del entrenamiento:

1. Reemplace `<Endpoint>` por el punto de conexión que obtuvo con la clave de suscripción de Form Recognizer.
1. Reemplace `<subscription key>` por la clave de suscripción.
1. Reemplace `<model ID>` por el identificador de modelo que recibió en el paso anterior.

```bash
curl -X GET "https://<Endpoint>/formrecognizer/v2.0/custom/models/<model ID>" -H "Content-Type: application/json" -H "Ocp-Apim-Subscription-Key: <subscription key>"
```

Recibirá una respuesta `200 (Success)` con un cuerpo JSON con el formato siguiente: Observe el campo `"status"`. Este tendrá el valor `"ready"` una vez que se complete el entrenamiento. Si el modelo no ha finalizado el entrenamiento, tendrá que consultar de nuevo el servicio volviendo a ejecutar el comando. Se recomienda un intervalo de uno o varios segundos entre llamadas.

El campo `"modelId"` contiene el identificador del modelo que se está entrenando. Lo necesitará para el siguiente paso.
    
```json
{ 
  "modelInfo":{ 
    "status":"ready",
    "createdDateTime":"2019-10-08T10:20:31.957784",
    "lastUpdatedDateTime":"2019-10-08T14:20:41+00:00",
    "modelId":"1cfb372bab404ba3aa59481ab2c63da5"
  },
  "trainResult":{ 
    "trainingDocuments":[ 
      { 
        "documentName":"invoices\\Invoice_1.pdf",
        "pages":1,
        "errors":[ 

        ],
        "status":"succeeded"
      },
      { 
        "documentName":"invoices\\Invoice_2.pdf",
        "pages":1,
        "errors":[ 

        ],
        "status":"succeeded"
      },
      { 
        "documentName":"invoices\\Invoice_3.pdf",
        "pages":1,
        "errors":[ 

        ],
        "status":"succeeded"
      },
      { 
        "documentName":"invoices\\Invoice_4.pdf",
        "pages":1,
        "errors":[ 

        ],
        "status":"succeeded"
      },
      { 
        "documentName":"invoices\\Invoice_5.pdf",
        "pages":1,
        "errors":[ 

        ],
        "status":"succeeded"
      }
    ],
    "errors":[ 

    ]
  },
  "keys":{ 
    "0":[ 
      "Address:",
      "Invoice For:",
      "Microsoft",
      "Page"
    ]
  }
}
```

## <a name="analyze-forms-for-key-value-pairs-and-tables"></a>Análisis de los formularios para obtener los pares clave-valor y las tablas

A continuación, utilizará el modelo recién entrenado para analizar un documento y extraerá de él tanto los pares clave-valor como las tablas. Llame a la API **[Analyze Form](https://westus2.dev.cognitive.microsoft.com/docs/services/form-recognizer-api-v2-preview/operations/AnalyzeWithCustomForm)** mediante la ejecución del siguiente comando cURL. Antes de ejecutar el comando, realice estos cambios:

1. Reemplace `<Endpoint>` por el punto de conexión que obtuvo de la clave de suscripción de Form Recognizer. Lo encontrará en la pestaña **Información general** del recurso Form Recognizer.
1. Reemplace `<model ID>` por el identificador de modelo que recibió en la sección anterior.
1. Reemplace `<SAS URL>` por una dirección URL de SAS en el archivo de Azure Storage. Siga los pasos de la sección de entrenamiento, pero en lugar de obtener una dirección URL de SAS para todo el contenedor de blobs, obtenga una para el archivo específico que desea analizar.
1. Reemplace `<subscription key>` por la clave de suscripción.

```bash
curl -v "https://<Endpoint>/formrecognizer/v2.0/custom/models/<model ID>/analyze" -H "Content-Type: application/json" -H "Ocp-Apim-Subscription-Key: <subscription key>" -d "{ \"source\": \""<SAS URL>"\" } "
```

Recibirá una respuesta `202 (Success)` con un encabezado **Operation-Location** (Operación-ubicación). El valor de este encabezado incluye un identificador de resultados que se usa para realizar el seguimiento de los resultados de la operación de análisis. Guarde este identificador de resultados para el siguiente paso.

## <a name="get-the-analyze-results"></a>Obtención de los resultados del análisis

Use la siguiente API para consultar los resultados de la operación de análisis.

1. Reemplace `<Endpoint>` por el punto de conexión que obtuvo de la clave de suscripción de Form Recognizer. Lo encontrará en la pestaña **Información general** del recurso Form Recognizer.
1. Reemplace `<result ID>` por el identificador que recibió en la sección anterior.
1. Reemplace `<subscription key>` por la clave de suscripción.

```bash
curl -X GET "https://<Endpoint>/formrecognizer/v2.0/custom/models/<model ID>/analyzeResults/<result ID>" -H "Ocp-Apim-Subscription-Key: <subscription key>"
```

Recibirá una respuesta `200 (Success)` con un cuerpo JSON con el formato siguiente: La salida se ha abreviado por motivos de simplicidad. Observe el campo `"status"` situado cerca de la parte inferior. Este tendrá el valor `"succeeded"` cuando se complete la operación de análisis. Si la operación de análisis no se ha completado, tendrá que consultar de nuevo el servicio volviendo a ejecutar el comando. Se recomienda un intervalo de uno o varios segundos entre llamadas.

Las asociaciones de pares clave-valor principales y tablas se encuentran en el nodo `"pageResults"`. Si también especificó la extracción de texto sin formato mediante el parámetro de URL *includeTextDetails*, el nodo `"readResults"` mostrará el contenido y las posiciones de todo el texto del documento.

```json
{
  "analyzeResult":{ 
    "readResults":[ 
      { 
        "page":1,
        "width":8.5,
        "height":11.0,
        "angle":0,
        "unit":"inch",
        "lines":[ 
          { 
            "text":"Contoso",
            "boundingBox":[ 
              0.5278,
              1.0597,
              1.4569,
              1.0597,
              1.4569,
              1.4347,
              0.5278,
              1.4347
            ],
            "words":[ 
              { 
                "text":"Contoso",
                "boundingBox":[ 
                  0.5278,
                  1.0597,
                  1.4569,
                  1.0597,
                  1.4569,
                  1.4347,
                  0.5278,
                  1.4347
                ]
              }
            ]
          },
          ...
          { 
            "text":"PT",
            "boundingBox":[ 
              6.2181,
              3.3528,
              6.3944,
              3.3528,
              6.3944,
              3.5417,
              6.2181,
              3.5417
            ],
            "words":[ 
              { 
                "text":"PT",
                "boundingBox":[ 
                  6.2181,
                  3.3528,
                  6.3944,
                  3.3528,
                  6.3944,
                  3.5417,
                  6.2181,
                  3.5417
                ]
              }
            ]
          }
        ]
      }
    ],
    "version":"2.0.0",
    "errors":[ 

    ],
    "documentResults":[ 

    ],
    "pageResults":[ 
      { 
        "page":1,
        "clusterId":1,
        "keyValuePairs":[ 
          { 
            "key":{ 
              "text":"Address:",
              "boundingBox":[ 
                0.7972,
                1.5125,
                1.3958,
                1.5125,
                1.3958,
                1.6431,
                0.7972,
                1.6431
              ],
              "elements":[ 
                "#/readResults/0/lines/1/words/0"
              ]
            },
            "value":{ 
              "text":"1 Redmond way Suite 6000 Redmond, WA 99243",
              "boundingBox":[ 
                0.7972,
                1.6764,
                2.15,
                1.6764,
                2.15,
                2.2181,
                0.7972,
                2.2181
              ],
              "elements":[ 
                "#/readResults/0/lines/4/words/0",
                "#/readResults/0/lines/4/words/1",
                "#/readResults/0/lines/4/words/2",
                "#/readResults/0/lines/4/words/3",
                "#/readResults/0/lines/6/words/0",
                "#/readResults/0/lines/6/words/1",
                "#/readResults/0/lines/6/words/2",
                "#/readResults/0/lines/8/words/0"
              ]
            },
            "confidence":0.86
          },
          { 
            "key":{ 
              "text":"Invoice For:",
              "boundingBox":[ 
                4.3903,
                1.5125,
                5.1139,
                1.5125,
                5.1139,
                1.6431,
                4.3903,
                1.6431
              ],
              "elements":[ 
                "#/readResults/0/lines/2/words/0",
                "#/readResults/0/lines/2/words/1"
              ]
            },
            "value":{ 
              "text":"Microsoft 1020 Enterprise Way Sunnayvale, CA 87659",
              "boundingBox":[ 
                5.1917,
                1.4458,
                6.6583,
                1.4458,
                6.6583,
                2.0347,
                5.1917,
                2.0347
              ],
              "elements":[ 
                "#/readResults/0/lines/3/words/0",
                "#/readResults/0/lines/5/words/0",
                "#/readResults/0/lines/5/words/1",
                "#/readResults/0/lines/5/words/2",
                "#/readResults/0/lines/7/words/0",
                "#/readResults/0/lines/7/words/1",
                "#/readResults/0/lines/7/words/2"
              ]
            },
            "confidence":0.86
          },
          ...
        ],
        "tables":[ 
          { 
            "caption":null,
            "rows":2,
            "columns":5,
            "cells":[ 
              { 
                "rowIndex":0,
                "colIndex":0,
                "header":true,
                "text":"Invoice Number",
                "boundingBox":[ 
                  0.5347,
                  2.8722,
                  1.575,
                  2.8722,
                  1.575,
                  3.0028,
                  0.5347,
                  3.0028
                ],
                "elements":[ 
                  "#/readResults/0/lines/9/words/0",
                  "#/readResults/0/lines/9/words/1"
                ]
              },
              { 
                "rowIndex":0,
                "colIndex":1,
                "header":true,
                "text":"Invoice Date",
                "boundingBox":[ 
                  1.9403,
                  2.8722,
                  2.7569,
                  2.8722,
                  2.7569,
                  3.0028,
                  1.9403,
                  3.0028
                ],
                "elements":[ 
                  "#/readResults/0/lines/10/words/0",
                  "#/readResults/0/lines/10/words/1"
                ]
              },
              { 
                "rowIndex":0,
                "colIndex":2,
                "header":true,
                "text":"Invoice Due Date",
                "boundingBox":[ 
                  3.3403,
                  2.8722,
                  4.4583,
                  2.8722,
                  4.4583,
                  3.0028,
                  3.3403,
                  3.0028
                ],
                "elements":[ 
                  "#/readResults/0/lines/11/words/0",
                  "#/readResults/0/lines/11/words/1",
                  "#/readResults/0/lines/11/words/2"
                ]
              },
              ...
            ]
          }
        ]
      }
    ]
  },
  "lastUpdatedDateTime":"2019-10-07T19:32:18+00:00",
  "status":"succeeded",
  "createdDateTime":"2019-10-07T19:32:15+00:00"
}
```

## <a name="improve-results"></a>Mejora de los resultados

[!INCLUDE [improve results](../includes/improve-results-unlabeled.md)]

## <a name="next-steps"></a>Pasos siguientes

En este inicio rápido, ha utilizado la API de REST de Form Recognizer con cURL para entrenar un modelo y ejecutarlo en un escenario de ejemplo. A continuación, consulte la documentación de referencia para explorar la API de Form Recognizer con mayor profundidad.

> [!div class="nextstepaction"]
> [Documentación de referencia de API REST](https://westus2.dev.cognitive.microsoft.com/docs/services/form-recognizer-api-v2-preview/operations/AnalyzeWithCustomForm)
