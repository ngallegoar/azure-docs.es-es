---
title: Indexación de blobs que contienen varios documentos
titleSuffix: Azure Cognitive Search
description: Rastree blobs de Azure para buscar contenido de texto mediante el indexador de blobs de Azure Cognitive Search, en el que cada blob podría producir uno o más documentos de índice de búsqueda.
manager: nitinme
author: arv100kri
ms.author: arjagann
ms.devlang: rest-api
ms.service: cognitive-search
ms.topic: conceptual
ms.date: 11/04/2019
ms.openlocfilehash: 1840bda0ecc9462a5d8f796b616d728d0bb412f7
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/27/2020
ms.locfileid: "74112266"
---
# <a name="indexing-blobs-to-produce-multiple-search-documents"></a>Indexación de blobs para producir varios documentos de búsqueda
De manera predeterminada, un indexador de blobs tratará el contenido de un blob como un único documento de búsqueda. Determinados valores de **parsingMode** admiten escenarios donde un blob individual puede dar lugar a varios documentos de búsqueda. A continuación, se muestran los diferentes tipos de **parsingMode** que permiten que un indexador extraiga más de un documento de búsqueda de un blob:
+ `delimitedText`
+ `jsonArray`
+ `jsonLines`

## <a name="one-to-many-document-key"></a>Clave de documento de uno a varios
Cada documento que se muestra en un índice de Azure Cognitive Search se identifica de forma única mediante una clave de documento. 

Cuando no se especifica ningún modo de análisis y si no hay ninguna asignación explícita para el campo de clave en el índice, Azure Cognitive Search [asigna](search-indexer-field-mappings.md) automáticamente la propiedad `metadata_storage_path` como clave. Esta asignación garantiza que cada blob aparezca como un documento de búsqueda distinto.

Al usar cualquiera de los modos de análisis mencionados anteriormente, un blob se asigna a "varios" documentos de búsqueda, lo que provoca que una clave de documento que se basa únicamente en los metadatos del blob no sea adecuada. Para superar esta restricción, Azure Cognitive Search es capaz de generar una clave de documento de "uno a varios" para cada entidad individual extraída de un blob. Esta propiedad se denomina `AzureSearch_DocumentKey` y se agrega a cada entidad individual extraída del blob. Se garantiza que el valor de esta propiedad sea único para cada entidad individual _entre blobs_ y las entidades se mostrarán como documentos de búsqueda independientes.

De manera predeterminada, cuando no se especifica ninguna asignación de campo explícita para el campo de índice de la clave, se le asigna `AzureSearch_DocumentKey` mediante la función `base64Encode` de asignación de campos.

## <a name="example"></a>Ejemplo
Supongamos que hay una definición de índice con los siguientes campos:
+ `id`
+ `temperature`
+ `pressure`
+ `timestamp`

Y que el contenedor de blobs tiene blobs con la estructura siguiente:

_Blob1.json_

    { "temperature": 100, "pressure": 100, "timestamp": "2019-02-13T00:00:00Z" }
    { "temperature" : 33, "pressure" : 30, "timestamp": "2019-02-14T00:00:00Z" }

_Blob2.json_

    { "temperature": 1, "pressure": 1, "timestamp": "2018-01-12T00:00:00Z" }
    { "temperature" : 120, "pressure" : 3, "timestamp": "2013-05-11T00:00:00Z" }

Al crear un indexador y establecer el valor de **parsingMode** en `jsonLines`: sin especificar ninguna asignación de campo explícita para el campo de clave, se aplicará la siguiente asignación de forma implícita.
    
    {
        "sourceFieldName" : "AzureSearch_DocumentKey",
        "targetFieldName": "id",
        "mappingFunction": { "name" : "base64Encode" }
    }

Esta configuración dará como resultado el índice de Azure Cognitive Search que contiene la información siguiente (el id. codificado en base64 se ha reducido para abreviar).

| id | temperatura | presión | timestamp |
|----|-------------|----------|-----------|
| aHR0 ... YjEuanNvbjsx | 100 | 100 | 2019-02-13T00:00:00Z |
| aHR0 ... YjEuanNvbjsy | 33 | 30 | 2019-02-14T00:00:00Z |
| aHR0 ... YjIuanNvbjsx | 1 | 1 | 2018-01-12T00:00:00Z |
| aHR0 ... YjIuanNvbjsy | 120 | 3 | 2013-05-11T00:00:00Z |

## <a name="custom-field-mapping-for-index-key-field"></a>Asignación de campos personalizados para el campo de clave de índice

Adoptando la misma definición de índice que en el ejemplo anterior, supongamos que el contenedor de blobs tiene blobs con la estructura siguiente:

_Blob1.json_

    recordid, temperature, pressure, timestamp
    1, 100, 100,"2019-02-13T00:00:00Z" 
    2, 33, 30,"2019-02-14T00:00:00Z" 

_Blob2.json_

    recordid, temperature, pressure, timestamp
    1, 1, 1,"2018-01-12T00:00:00Z" 
    2, 120, 3,"2013-05-11T00:00:00Z" 

Cuando se crea un indexador con **parsingMode** `delimitedText`, puede resultar natural configurar una función de asignación de campos para el campo de clave como se indica a continuación:

    {
        "sourceFieldName" : "recordid",
        "targetFieldName": "id"
    }

Sin embargo, esta asignación _no_ tendrá como resultado cuatro documentos que se muestran en el índice, porque el campo `recordid` no es único _entre blobs_. Por lo tanto, se recomienda hacer uso de la asignación de campos implícita que se aplica desde la propiedad `AzureSearch_DocumentKey` en el campo de índice de la clave para los modos de análisis de "uno a varios".

Si quiere configurar la asignación de campos explícita, asegúrese de que _sourceField_ sea diferente para cada entidad individual **entre todos los blobs**.

> [!NOTE]
> El enfoque usado por `AzureSearch_DocumentKey` para garantizar la unicidad por entidad extraída está sujeto a cambios y, por tanto, no debe confiar en su valor para las necesidades de la aplicación.

## <a name="next-steps"></a>Pasos siguientes

Si aún no está familiarizado con la estructura y el flujo de trabajo básicos de la indexación de blobs, debería consultar primero [Indexación de Azure Blob Storage con Azure Cognitive Search](search-howto-index-json-blobs.md). Para más información sobre los modos de análisis de los distintos tipos de contenido de blobs, revise los artículos siguientes.

> [!div class="nextstepaction"]
> [Indexación de blobs CSV](search-howto-index-csv-blobs.md)
> [Indexación de blobs JSON](search-howto-index-json-blobs.md)
