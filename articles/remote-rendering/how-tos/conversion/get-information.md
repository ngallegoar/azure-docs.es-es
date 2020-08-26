---
title: Obtención de información acerca de un modelo convertido
description: Descripción de todos los parámetros de conversión de modelos
author: malcolmtyrrell
ms.author: matyrr
ms.date: 03/05/2020
ms.topic: how-to
ms.openlocfilehash: f5c38ac88503416b37b720a091c9e46d819a3146
ms.sourcegitcommit: 54d8052c09e847a6565ec978f352769e8955aead
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 08/18/2020
ms.locfileid: "88509304"
---
# <a name="get-information-about-a-converted-model"></a>Obtención de información acerca de un modelo convertido

El archivo arrAsset generado por el servicio de conversión está diseñado exclusivamente para el consumo por parte del servicio de representación. Sin embargo, puede haber ocasiones en las que quiera acceder a información sobre un modelo sin iniciar una sesión de representación. Por lo tanto, el servicio de conversión coloca un archivo JSON junto al archivo arrAsset en el contenedor de salida. Por ejemplo, si se convierte un archivo `buggy.gltf`, el contenedor de salida incluirá un archivo denominado `buggy.info.json` junto al archivo convertido `buggy.arrAsset`. Este archivo contiene información sobre el modelo de origen, el modelo convertido y sobre la propia conversión.

## <a name="example-info-file"></a>Archivo de *información* de ejemplo

Este es un archivo de *información* de ejemplo generado al convertir un archivo llamado `buggy.gltf`:

```JSON
{
    "files": {
        "input": "Buggy.gltf"
    },
    "conversionSettings": {
        "recenterToOrigin": true
    },
    "inputInfo": {
        "sourceAssetExtension": ".gltf",
        "sourceAssetFormat": "glTF2 Importer",
        "sourceAssetFormatVersion": "2.0",
        "sourceAssetGenerator": "COLLADA2GLTF"
    },
    "inputStatistics": {
        "numMeshes": 148,
        "numFaces": 308306,
        "numVertices": 245673,
        "numMaterial": 149,
        "numFacesSmallestMesh": 2,
        "numFacesBiggestMesh": 8764,
        "numNodes": 206,
        "numMeshUsagesInScene": 236,
        "maxNodeDepth": 3
    },
    "outputInfo": {
        "conversionToolVersion": "3b28d840de9916f9d628342f474d38c3ab949590",
        "conversionHash": "CCDB1F7A4C09F565"
    },
    "outputStatistics": {
        "numMeshPartsCreated": 236,
        "numMeshPartsInstanced": 88,
        "recenteringOffset": [
            -24.1,
            -50.9,
            -16.5974
        ],
        "boundingBox": {
            "min": [
                -43.52,
                -61.775,
                -79.6416
            ],
            "max": [
                43.52,
                61.775,
                79.6416
            ]
        }
    }
}
```

## <a name="information-in-the-info-file"></a>Información en el archivo de información

### <a name="the-files-section"></a>La sección *files*

Esta sección contiene los nombres de archivo proporcionados.

* `input`: nombre del archivo de origen.
* `output`: nombre del archivo de salida, cuando el usuario ha especificado un nombre no predeterminado.

### <a name="the-conversionsettings-section"></a>La sección *conversionSettings*

En esta sección se incluye una copia de la sección [ConversionSettings](configure-model-conversion.md#settings-file) especificada cuando se convirtió el modelo.

### <a name="the-inputinfo-section"></a>La sección *inputInfo*

En esta sección se registra información sobre el formato de archivo de origen.

* `sourceAssetExtension`: la extensión de archivo del archivo de origen.
* `sourceAssetFormat`: descripción del formato del archivo de origen.
* `sourceAssetFormatVersion`: versión del formato del archivo de origen.
* `sourceAssetGenerator`: nombre de la herramienta que generó el archivo de origen, si está disponible.

### <a name="the-inputstatistics-section"></a>La sección *inputStatistics*

Esta sección proporciona información sobre la escena de origen. A menudo habrá discrepancias entre los valores de esta sección y los valores equivalentes de la herramienta que creó el modelo de origen. Tales diferencias están previstas, ya que el modelo se modifica durante los pasos de exportación y conversión.

* `numMeshes`: número de partes de la malla, donde cada parte puede hacer referencia a un único material.
* `numFaces`: número total de _triángulos_ en todo el modelo. Tenga en cuenta que la malla se triangula durante la conversión. Este número contribuye al límite de polígonos en el [tamaño del servidor de representación estándar](../../reference/vm-sizes.md#how-the-renderer-evaluates-the-number-of-polygons).
* `numVertices`: número total de vértices en todo el modelo.
* `numMaterial`: número total de materiales en todo el modelo.
* `numFacesSmallestMesh`: número de triángulos en la malla más pequeña del modelo.
* `numFacesBiggestMesh`: número de triángulos en la malla más grande del modelo.
* `numNodes`: número de nodos del gráfico de escenas del modelo.
* `numMeshUsagesInScene`: número de veces que los nodos hacen referencia a las mallas. Más de un nodo puede hacer referencia a la misma malla.
* `maxNodeDepth`: profundidad máxima de los nodos dentro del gráfico de escenas.

### <a name="the-outputinfo-section"></a>La sección *outputInfo*

En esta sección se registra información general sobre la salida generada.

* `conversionToolVersion`: versión del convertidor de modelos.
* `conversionHash`: hash de los datos dentro de arrAsset que pueden contribuir a la representación. Se puede usar para saber si el servicio de conversión ha producido un resultado diferente cuando se vuelve a ejecutar en el mismo archivo.

### <a name="the-outputstatistics-section"></a>La sección *outputStatistics*

En esta sección se registra la información calculada a partir del recurso convertido.

* `numMeshPartsCreated`: número de mallas en el arrAsset. Puede variar con respecto a `numMeshes` de la sección `inputStatistics`, porque la creación de instancias se ve afectada por el proceso de conversión.
* `numMeshPartsInstanced`: número de mallas que se reutilizan en arrAsset.
* `recenteringOffset`: cuando está habilitada la opción `recenterToOrigin` en [ConversionSettings](configure-model-conversion.md), este valor es el traslado que movería el modelo convertido de vuelta a su posición original.
* `boundingBox`: límites del modelo.

## <a name="next-steps"></a>Pasos siguientes

* [Conversión de datos](model-conversion.md)
* [Configuración de la conversión de modelos](configure-model-conversion.md)
