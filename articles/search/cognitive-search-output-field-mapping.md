---
title: Asignación de entrada a campos de salida
titleSuffix: Azure Cognitive Search
description: Extraiga y enriquezca los campos de datos de origen y asígnelos a los campos de salida de un índice de Búsqueda cognitiva de Azure.
manager: nitinme
author: luiscabrer
ms.author: luisca
ms.service: cognitive-search
ms.topic: conceptual
ms.date: 11/04/2019
ms.openlocfilehash: c9b0b34202f35babcaa3dce37331d31edf641254
ms.sourcegitcommit: 877491bd46921c11dd478bd25fc718ceee2dcc08
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 07/02/2020
ms.locfileid: "85557262"
---
# <a name="how-to-map-ai-enriched-fields-to-a-searchable-index"></a>Cómo asignar campos enriquecidos con IA a un índice de búsqueda

En este artículo, aprenderá a asignar campos de entrada enriquecidos a campos de salida en un índice de búsqueda. Una vez que haya [definido un conjunto de aptitudes](cognitive-search-defining-skillset.md), debe asignar los campos de salida de cualquier aptitud que aporte directamente valores a un campo dado del índice de búsqueda. 

Las asignaciones de campos de salida son necesarias para mover contenido de documentos enriquecidos al índice.  El documento enriquecido es en realidad un árbol de información y, aunque hay compatibilidad con tipos complejos en el índice, a veces puede interesarle transformar la información del árbol enriquecido en un tipo más simple (por ejemplo, una matriz de cadenas). Las asignaciones de campos de salida permiten realizar transformaciones en la forma de los datos mediante la reducción de información.

> [!NOTE]
> Recientemente se ha habilitado la funcionalidad de asignación de funciones en las asignaciones de campos de salida. Para obtener más información sobre las funciones de asignación, consulte [Funciones de asignación de campos](https://docs.microsoft.com/azure/search/search-indexer-field-mappings#field-mapping-functions)

## <a name="use-outputfieldmappings"></a>Usar outputFieldMappings
Para asignar los campos, agregue `outputFieldMappings` a la definición del indexador tal como se muestra a continuación:

```http
PUT https://[servicename].search.windows.net/indexers/[indexer name]?api-version=2020-06-30
api-key: [admin key]
Content-Type: application/json
```

El cuerpo de la solicitud está estructurado de la siguiente manera:

```json
{
    "name": "myIndexer",
    "dataSourceName": "myDataSource",
    "targetIndexName": "myIndex",
    "skillsetName": "myFirstSkillSet",
    "fieldMappings": [
        {
            "sourceFieldName": "metadata_storage_path",
            "targetFieldName": "id",
            "mappingFunction": {
                "name": "base64Encode"
            }
        }
    ],
    "outputFieldMappings": [
        {
            "sourceFieldName": "/document/content/organizations/*/description",
            "targetFieldName": "descriptions",
            "mappingFunction": {
                "name": "base64Decode"
            }
        },
        {
            "sourceFieldName": "/document/content/organizations",
            "targetFieldName": "orgNames"
        },
        {
            "sourceFieldName": "/document/content/sentiment",
            "targetFieldName": "sentiment"
        }
    ]
}
```

En cada asignación de campo de salida, establezca la ubicación de los datos del árbol de documentos enriquecidos (sourceFieldName) y el nombre del campo al que se hace referencia en el índice (targetFieldName).

## <a name="flattening-information-from-complex-types"></a>Reducción de la información de tipos complejos 

La ruta de acceso en un campo sourceFieldName puede representar un elemento o varios elementos. En el ejemplo anterior, ```/document/content/sentiment``` representa un único valor numérico, mientras que ```/document/content/organizations/*/description``` representa varias descripciones de la organización. 

En casos donde hay varios elementos, estos son "reducidos" en una matriz que contiene cada uno de los elementos. 

Más concretamente, en el ejemplo ```/document/content/organizations/*/description```, los datos del campo *Descripciones* tienen el aspecto de una matriz plana de descripciones antes de que se indexe:

```
 ["Microsoft is a company in Seattle","LinkedIn's office is in San Francisco"]
```

Este es un principio importante, así que se proporcionará otro ejemplo. Imagine que tiene una matriz de tipos complejos como parte del árbol de enriquecimiento. Supongamos que hay un miembro denominado customEntities que tiene una matriz de tipos complejos como la que se describe a continuación.

```json
"document/customEntities": 
[
    {
        "name": "heart failure",
        "matches": [
            {
                "text": "heart failure",
                "offset": 10,
                "length": 12,
                "matchDistance": 0.0
            }
        ]
    },
    {
        "name": "morquio",
        "matches": [
            {
                "text": "morquio",
                "offset": 25,
                "length": 7,
                "matchDistance": 0.0
            }
        ]
    }
    //...
]
```

Supongamos que el índice tiene un campo denominado "diseases" de tipo Collection(Edm.String), donde quiere almacenar cada uno de los nombres de las entidades. 

Esto se puede hacer fácilmente mediante el símbolo "\*", como se indica a continuación:

```json
    "outputFieldMappings": [
        {
            "sourceFieldName": "/document/customEntities/*/name",
            "targetFieldName": "diseases"
        }
    ]
```

Esta operación simplemente "reduce" cada uno de los nombres de los elementos customEntities a una sola matriz de cadenas como esta:

```json
  "diseases" : ["heart failure","morquio"]
```

## <a name="next-steps"></a>Pasos siguientes
Una vez haya asignado los campos enriquecidos a los campos de búsqueda, puede establecer los atributos de campo de cada uno de los campos de búsqueda [como parte de la definición del índice](search-what-is-an-index.md).

Para obtener más información acerca de las asignaciones de campos, consulte [Field mappings in Azure Cognitive Search indexers](search-indexer-field-mappings.md) (Asignaciones de campos en los indexadores de Búsqueda cognitiva de Azure).
