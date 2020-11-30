---
title: Uso de Text Analytics para el estado
titleSuffix: Azure Cognitive Services
description: Aprenda a extraer y etiquetar la información médica de textos clínicos no estructurados con Text Analytics para el estado.
services: cognitive-services
author: aahill
manager: nitinme
ms.service: cognitive-services
ms.subservice: text-analytics
ms.topic: conceptual
ms.date: 11/19/2020
ms.author: aahi
ms.custom: references_regions
ms.openlocfilehash: e7f017c1f3dc189af2b0fc053912decca3459478
ms.sourcegitcommit: cd9754373576d6767c06baccfd500ae88ea733e4
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 11/20/2020
ms.locfileid: "94952767"
---
# <a name="how-to-use-text-analytics-for-health-preview"></a>Procedimientos: Uso de Text Analytics para el estado (versión preliminar)

> [!IMPORTANT] 
> Text Analytics para el estado es una funcionalidad de versión preliminar que se proporciona "tal cual" y "con todos los errores". Como tal, no se debe implementar **Text Analytics para el estado (versión preliminar) en ningún uso de producción.** Text Analytics para el estado no está previsto ni está disponible para su uso como dispositivo médico, soporte clínico, herramienta de diagnóstico u otra tecnología que se pretenda usar en el diagnóstico, la cura, la mitigación, el tratamiento o la prevención de la enfermedad u otras condiciones, y Microsoft no concede ninguna licencia o derecho para usar esta funcionalidad con este fin. Esta funcionalidad no está diseñada o prevista para implementarse como un sustituto del asesoramiento médico profesional o la opinión de la atención sanitaria, el diagnóstico, el tratamiento o la opinión clínica de un profesional sanitario y no debe usarse como tal. El cliente es el único responsable de cualquier uso de Text Analytics para el estado. Microsoft no garantiza que Text Analytics para el estado o los materiales que se proporcionen en relación con la capacidad sean suficientes para fines médicos o que de otro modo cumplan los requisitos sanitarios o médicos de cualquier persona. 


Text Analytics for Health es una característica de la API Text Analytics, que extrae y etiqueta información médica pertinente de textos no estructurados, como notas del doctor, resúmenes de altas médicas, historiales clínicos y registros electrónicos de salud.  Hay dos maneras de utilizar este servicio: 

* La API basada en web (asincrónica) 
* Un contenedor de Docker (sincrónica)   

## <a name="features"></a>Características

Text Analytics for Health realiza el reconocimiento de entidades con nombre (NER), la extracción de relaciones, la negación de entidades y la vinculación de entidades en el texto en inglés para revelar información en texto clínico y biomédico sin estructurar.

### <a name="named-entity-recognition"></a>[Reconocimiento de entidades con nombre](#tab/ner)

El reconocimiento de entidades con nombre detecta palabras y frases mencionadas en texto no estructurado que se pueden asociar a uno o varios tipos semánticos, como el diagnóstico, el nombre de los medicamentos, el síntoma, el signo o la edad.

> [!div class="mx-imgBorder"]
> ![Health NER](../media/ta-for-health/health-named-entity-recognition.png)

### <a name="relation-extraction"></a>[Extracción de relaciones](#tab/relation-extraction)

La extracción de relaciones identifica conexiones significativas entre los conceptos mencionados en el texto. Por ejemplo, una relación "tiempo de condición" se encuentra asociando un nombre de condición con una hora. 

> [!div class="mx-imgBorder"]
> ![Health RE](../media/ta-for-health/health-relation-extraction.png)


### <a name="entity-linking"></a>[Entity Linking](#tab/entity-linking)

La vinculación de entidad anula las distintas entidades al asociar las entidades con nombre mencionadas en el texto a los conceptos que se encuentran en una base de datos de conceptos predefinida. Por ejemplo, el sistema de lenguaje unificado de modelado (UMLS).

> [!div class="mx-imgBorder"]
> ![Health EL](../media/ta-for-health/health-entity-linking.png)

Text Analytics para el estado admite la vinculación a los vocabularios biomédicos y de salud que se encuentran en el origen de la información del metadiccionario de sinónimos del Sistema unificado de lenguaje médico ([UMLS](https://www.nlm.nih.gov/research/umls/sourcereleasedocs/index.html)).

### <a name="negation-detection"></a>[Detección de negación](#tab/negation-detection) 

El significado del contenido médico se ve muy afectado por los modificadores como, por ejemplo, la negación, que puede tener una implicación crítica si se ha diagnosticado erróneamente. Text Analytics para el estado admite la detección de negación para las distintas entidades mencionadas en el texto. 

> [!div class="mx-imgBorder"]
> ![Health NEG](../media/ta-for-health/health-negation.png)

---

Consulte las [categorías de entidad](../named-entity-types.md?tabs=health) devueltas por Text Analytics para el estado a fin de obtener una lista completa de las entidades admitidas.

### <a name="supported-languages-and-regions"></a>Idiomas y regiones admitidos

Text Analytics para el estado solo admite documentos en inglés. 

Actualmente, la API web hospedada de Text Analytics for Health solo está disponible en estas regiones: Oeste de EE. UU. 2, este de EE. UU. 2, centro de EE. UU., Norte de Europa y Oeste de Europa.

## <a name="request-access-to-the-public-preview"></a>Solicitud de acceso a la versión preliminar pública

Rellene y envíe el [formulario de solicitud de Cognitive Services](https://aka.ms/csgate) para solicitar acceso a la versión preliminar pública de Text Analytics for Health. El uso de Text Analytics for Health no se facturará. 

El formulario solicita información acerca del usuario y de su empresa, así como del escenario de usuario para el que se va a usar el contenedor. Después de enviar el formulario, el equipo de Azure Cognitive Services lo revisará y le informará la decisión por correo electrónico.

> [!IMPORTANT]
> * En el formulario, debe usar una dirección de correo electrónico asociada a un identificador de suscripción de Azure.
> * El recurso de Azure que se usa se debe haber creado con el identificador de la suscripción de Azure aprobada. 
> * Compruebe el correo electrónico (bandeja de entrada y carpetas de correo no deseado) para obtener las actualizaciones del estado de la aplicación por parte de Microsoft.

## <a name="using-the-docker-container"></a>Uso del contenedor de Docker 

Para ejecutar el contenedor de Text Analytics for Heath en su propio entorno, siga estas [instrucciones para descargar e instalar el contenedor](../how-tos/text-analytics-how-to-install-containers.md?tabs=healthcare).

## <a name="using-the-client-library"></a>Uso de la biblioteca cliente

La versión preliminar más reciente de la biblioteca cliente de Text Analytics permite llamar a Text Analytics for Health mediante un objeto de cliente. Consulte la documentación de referencia y vea los ejemplos en GitHub:
* [C#](https://github.com/Azure/azure-sdk-for-net/tree/master/sdk/textanalytics/Azure.AI.TextAnalytics)
* [Python](https://github.com/Azure/azure-sdk-for-python/tree/master/sdk/textanalytics/azure-ai-textanalytics/)
* [Java](https://github.com/Azure/azure-sdk-for-java/tree/master/sdk/textanalytics/azure-ai-textanalytics)



## <a name="sending-a-rest-api-request"></a>Envío de una solicitud de API REST 

### <a name="preparation"></a>Preparación

Text Analytics for Health genera un resultado de mayor calidad si se le proporcionan fragmentos de texto más pequeños en los que trabajar, algo que es totalmente opuesto a lo que sucede con otras características de Text Analytics, como la extracción de frases clave, que funciona mejor en bloques de texto más grandes. Para obtener los mejores resultados posibles en ambas operaciones, considere la posibilidad de reestructurar las entradas en consecuencia.

Debe tener documentos JSON en este formato: identificador, texto e idioma. 

El tamaño del documento debe ser inferior a 5120 caracteres por documento. Para conocer el número máximo de documentos permitidos en una colección, consulte el artículo sobre [límites de datos](../concepts/data-limits.md?tabs=version-3) en Conceptos. La colección se envía en el cuerpo de la solicitud.

### <a name="structure-the-api-request-for-the-hosted-asynchronous-web-api"></a>Estructura de la solicitud de API para la API web asincrónica hospedada

Tanto para el contenedor como para la API web hospedada, debe crear una solicitud POST. Puede [usar Postman](text-analytics-how-to-call-api.md), un comando de cURL o la **consola de prueba de API** en la [referencia de la API hospedada Text Analytics for Health](https://westus2.dev.cognitive.microsoft.com/docs/services/TextAnalytics-v3-1-preview-3/operations/Health) para construir y enviar rápidamente una solicitud POST a la API web hospedada en la región deseada. 

A continuación hay un ejemplo de un archivo JSON adjunto al cuerpo de POST de la solicitud de la API Text Analytics for Health:

```json
example.json

{
  "documents": [
    {
      "language": "en",
      "id": "1",
      "text": "Subject was administered 100mg remdesivir intravenously over a period of 120 min"
    }
  ]
}
```

### <a name="hosted-asynchronous-web-api-response"></a>Respuesta de API web asincrónica hospedada 

Como esta solicitud POST se usa para enviar un trabajo para la operación asincrónica, no hay texto en el objeto de la respuesta.  Sin embargo, necesita el valor de KEY de operation-location en los encabezados de respuesta para realizar una solicitud GET para comprobar el estado del trabajo y la salida.  A continuación se muestra un ejemplo del valor de la KEY de operation-location en el encabezado de respuesta de la solicitud POST:

`https://<your-custom-subdomain>.cognitiveservices.azure.com/text/analytics/v3.1-preview.3/entities/health/jobs/<jobID>`

Para comprobar el estado del trabajo, realice una solicitud GET a la dirección URL en el valor del encabezado KEY DE operation-Location de la respuesta POST.  Los siguientes estados se usan para reflejar el estado de un trabajo: `NotStarted`, `running`, `succeeded`, `failed`, `rejected`, `cancelling` y `cancelled`.  

Puede cancelar un trabajo con los estados `NotStarted` o `running` con una llamada HTTP DELETE a la misma dirección URL que la solicitud GET.  Puede encontrar más información sobre la llamada de eliminación en la [referencia de la API hospedada Text Analytics for Health](https://westus2.dev.cognitive.microsoft.com/docs/services/TextAnalytics-v3-1-preview-3/operations/CancelHealthJob).

El siguiente es un ejemplo de la respuesta de una solicitud GET.  Tenga en cuenta que la salida está disponible para su recuperación hasta que ha transcurrido el valor de `expirationDateTime` (24 horas desde el momento en que se creó el trabajo), y después se purga la salida.

```json
{
    "jobId": "b672c6f5-7c0d-4783-ba8c-4d0c47213454",
    "lastUpdateDateTime": "2020-11-18T01:45:00Z",
    "createdDateTime": "2020-11-18T01:44:55Z",
    "expirationDateTime": "2020-11-19T01:44:55Z",
    "status": "succeeded",
    "errors": [],
    "results": {
        "documents": [
            {
                "id": "1",
                "entities": [
                    {
                        "offset": 25,
                        "length": 5,
                        "text": "100mg",
                        "category": "Dosage",
                        "confidenceScore": 1.0,
                        "isNegated": false
                    },
                    {
                        "offset": 31,
                        "length": 10,
                        "text": "remdesivir",
                        "category": "MedicationName",
                        "confidenceScore": 1.0,
                        "isNegated": false,
                        "links": [
                            {
                                "dataSource": "UMLS",
                                "id": "C4726677"
                            },
                            {
                                "dataSource": "MSH",
                                "id": "C000606551"
                            },
                            {
                                "dataSource": "NCI",
                                "id": "C152185"
                            },
                            {
                                "dataSource": "NCI_FDA",
                                "id": "3QKI37EEHE"
                            }
                        ]
                    },
                    {
                        "offset": 42,
                        "length": 13,
                        "text": "intravenously",
                        "category": "MedicationRoute",
                        "confidenceScore": 1.0,
                        "isNegated": false
                    },
                    {
                        "offset": 56,
                        "length": 4,
                        "text": "over",
                        "category": "Time",
                        "confidenceScore": 0.87,
                        "isNegated": false
                    },
                    {
                        "offset": 73,
                        "length": 7,
                        "text": "120 min",
                        "category": "Time",
                        "confidenceScore": 0.99,
                        "isNegated": false
                    }
                ],
                "relations": [
                    {
                        "relationType": "DosageOfMedication",
                        "bidirectional": false,
                        "source": "#/results/documents/0/entities/0",
                        "target": "#/results/documents/0/entities/1"
                    },
                    {
                        "relationType": "RouteOfMedication",
                        "bidirectional": false,
                        "source": "#/results/documents/0/entities/2",
                        "target": "#/results/documents/0/entities/1"
                    },
                    {
                        "relationType": "TimeOfMedication",
                        "bidirectional": false,
                        "source": "#/results/documents/0/entities/3",
                        "target": "#/results/documents/0/entities/1"
                    },
                    {
                        "relationType": "TimeOfMedication",
                        "bidirectional": false,
                        "source": "#/results/documents/0/entities/4",
                        "target": "#/results/documents/0/entities/1"
                    }
                ],
                "warnings": []
            }
        ],
        "errors": [],
        "modelVersion": "2020-09-03"
    }
}
```


### <a name="structure-the-api-request-for-the-container"></a>Estructuración de la solicitud de API para el contenedor

Puede [usar Postman](text-analytics-how-to-call-api.md) o la siguiente solicitud de cURL de ejemplo para enviar una consulta al contenedor que ha implementado, pero reemplace la variable `serverURL` por el valor adecuado.  Tenga en cuenta que la versión de la API en la dirección URL del contenedor no es la misma que la de la API hospedada.

```bash
curl -X POST 'http://<serverURL>:5000/text/analytics/v3.2-preview.1/entities/health' --header 'Content-Type: application/json' --header 'accept: application/json' --data-binary @example.json

```

El siguiente JSON es un ejemplo de un archivo JSON adjunto al cuerpo POST de la solicitud de la API de Text Analytics para el estado:

```json
example.json

{
  "documents": [
    {
      "language": "en",
      "id": "1",
      "text": "Patient reported itchy sores after swimming in the lake."
    },
    {
      "language": "en",
      "id": "2",
      "text": "Prescribed 50mg benadryl, taken twice daily."
    }
  ]
}
```

### <a name="container-response-body"></a>Cuerpo de respuesta del contenedor

El siguiente JSON es un ejemplo de cuerpo de respuesta de la API Text Analytics for Health desde la llamada sincrónica con contenedores:

```json
{
    "documents": [
        {
            "id": "1",
            "entities": [
                {
                    "id": "0",
                    "offset": 25,
                    "length": 5,
                    "text": "100mg",
                    "category": "Dosage",
                    "confidenceScore": 1.0,
                    "isNegated": false
                },
                {
                    "id": "1",
                    "offset": 31,
                    "length": 10,
                    "text": "remdesivir",
                    "category": "MedicationName",
                    "confidenceScore": 1.0,
                    "isNegated": false,
                    "links": [
                        {
                            "dataSource": "UMLS",
                            "id": "C4726677"
                        },
                        {
                            "dataSource": "MSH",
                            "id": "C000606551"
                        },
                        {
                            "dataSource": "NCI",
                            "id": "C152185"
                        },
                        {
                            "dataSource": "NCI_FDA",
                            "id": "3QKI37EEHE"
                        }
                    ]
                },
                {
                    "id": "2",
                    "offset": 42,
                    "length": 13,
                    "text": "intravenously",
                    "category": "MedicationRoute",
                    "confidenceScore": 1.0,
                    "isNegated": false
                },
                {
                    "id": "3",
                    "offset": 56,
                    "length": 4,
                    "text": "over",
                    "category": "Time",
                    "confidenceScore": 0.87,
                    "isNegated": false
                },
                {
                    "id": "4",
                    "offset": 73,
                    "length": 7,
                    "text": "120 min",
                    "category": "Time",
                    "confidenceScore": 0.99,
                    "isNegated": false
                }
            ],
            "relations": [
                {
                    "relationType": "DosageOfMedication",
                    "bidirectional": false,
                    "source": "#/documents/0/entities/0",
                    "target": "#/documents/0/entities/1"
                },
                {
                    "relationType": "RouteOfMedication",
                    "bidirectional": false,
                    "source": "#/documents/0/entities/2",
                    "target": "#/documents/0/entities/1"
                },
                {
                    "relationType": "TimeOfMedication",
                    "bidirectional": false,
                    "source": "#/documents/0/entities/3",
                    "target": "#/documents/0/entities/1"
                },
                {
                    "relationType": "TimeOfMedication",
                    "bidirectional": false,
                    "source": "#/documents/0/entities/4",
                    "target": "#/documents/0/entities/1"
                }
            ]
        }
    ],
    "errors": [],
    "modelVersion": "2020-09-03"
}
```

### <a name="negation-detection-output"></a>Salida de la detección de negación

Al usar la detección de negación, en algunos casos un solo término de negación puede abordar varios términos a la vez. La negación de una entidad reconocida se representa en la salida JSON mediante el valor booleano de la marca `isNegated`, por ejemplo:

```json
{
  "id": "2",
  "offset": 90,
  "length": 10,
  "text": "chest pain",
  "category": "SymptomOrSign",
  "score": 0.9972,
  "isNegated": true,
  "links": [
    {
      "dataSource": "UMLS",
      "id": "C0008031"
    },
    {
      "dataSource": "CHV",
      "id": "0000023593"
    },
    ...
```

### <a name="relation-extraction-output"></a>Salida de la extracción de relaciones

La salida de la extracción de relaciones contiene referencias de URI al *origen* de la relación y su *destino*. A las entidades con el rol de relación de `ENTITY` se les asigna el campo `target`. A las entidades con el rol de relación de `ATTRIBUTE` se les asigna el campo `source`. Las relaciones de abreviatura contienen campos bidireccionales `source` y `target`, y `bidirectional` se establecerá en `true`. 

```json
"relations": [
                {
                    "relationType": "DosageOfMedication",
                    "bidirectional": false,
                    "source": "#/documents/1/entities/0",
                    "target": "#/documents/1/entities/1"
                },
                {
                    "relationType": "FrequencyOfMedication",
                    "bidirectional": false,
                    "source": "#/documents/1/entities/2",
                    "target": "#/documents/1/entities/1"
                }
            ]
  },
...
]
```

## <a name="see-also"></a>Consulte también

* [Información general de Text Analytics](../overview.md)
* [Categorías de entidad con nombres](../named-entity-types.md)
* [Novedades](../whats-new.md)
