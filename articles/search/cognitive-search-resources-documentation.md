---
title: Vínculos de la documentación del enriquecimiento con inteligencia artificial
titleSuffix: Azure Cognitive Search
description: Una lista anotada de artículos, tutoriales, ejemplos y entradas de blog relacionados con cargas de trabajo de enriquecimiento con inteligencia artificial en Azure Cognitive Search.
manager: nitinme
author: HeidiSteen
ms.author: heidist
ms.service: cognitive-search
ms.topic: conceptual
ms.date: 02/25/2020
ms.openlocfilehash: d2b25fb93a1e35ffa82cf49c60d181b841b1692d
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/28/2020
ms.locfileid: "77616193"
---
# <a name="documentation-resources-for-ai-enrichment-in-azure-cognitive-search"></a>Recursos de documentación para el enriquecimiento con inteligencia artificial en Azure Cognitive Search

El enriquecimiento con inteligencia artificial es una funcionalidad de indexación de Azure Cognitive Search que busca información latente en orígenes no textuales y texto sin diferenciar, y la transforma en contenido que permite búsquedas de texto completo en Azure Cognitive Search.

Los artículos siguientes son la documentación completa que trata sobre el enriquecimiento con inteligencia artificial.

## <a name="getting-started"></a>Introducción
+ [Introducción a la inteligencia artificial en Azure Cognitive Search](cognitive-search-concept-intro.md)
+ [Inicio rápido: Creación de un conjunto de aptitudes cognitivo en Azure Portal](cognitive-search-quickstart-blob.md)
+ [Tutorial: Enriched indexing with AI](cognitive-search-tutorial-blob.md) (Tutorial: Indexación enriquecida con inteligencia artificial)
+ [Ejemplo: Creación de una aptitud personalizada para el enriquecimiento con inteligencia artificial](cognitive-search-create-custom-skill-example.md)

## <a name="how-to-guidance"></a>Guía de procedimientos
+ [Definición de un conjunto de aptitudes](cognitive-search-defining-skillset.md)
+ [How to reference annotations in a skillset](cognitive-search-concept-annotations-syntax.md) (Referencia a las anotaciones de un conjunto de aptitudes)
+ [How to map fields to an index](cognitive-search-output-field-mapping.md) (Asignación de campos enriquecidos a un índice)
+ [How to process and extract information from images](cognitive-search-concept-image-scenarios.md) (Procesamiento y extracción de información de imágenes)
+ [How to rebuild an Azure Cognitive Search index](search-howto-reindex.md) (Reconstrucción de un índice de Azure Cognitive Search)
+ [How to define a custom skills interface](cognitive-search-custom-skill-interface.md) (Definición de una interfaz de aptitudes personalizada)
+ [Sugerencias para la solución de problemas](cognitive-search-concept-troubleshooting.md)

## <a name="reference"></a>Referencia

+ [Aptitudes integradas](cognitive-search-predefined-skills.md)
  + [Microsoft.Skills.Text.KeyPhraseExtractionSkill](cognitive-search-skill-keyphrases.md)
  + [Microsoft.Skills.Text.LanguageDetectionSkill](cognitive-search-skill-language-detection.md)
  + [Microsoft.Skills.Text.EntityRecognitionSkill](cognitive-search-skill-entity-recognition.md)
  + [Microsoft.Skills.Text.MergeSkill](cognitive-search-skill-textmerger.md)
  + [Microsoft.Skills.Text.PIIDetectionSkill](cognitive-search-skill-pii-detection.md)
  + [Microsoft.Skills.Text.SplitSkill](cognitive-search-skill-textsplit.md)
  + [Microsoft.Skills.Text.SentimentSkill](cognitive-search-skill-sentiment.md)
  + [Microsoft.Skills.Text.TranslationSkill](cognitive-search-skill-text-translation.md)
  + [Microsoft.Skills.Vision.ImageAnalysisSkill](cognitive-search-skill-image-analysis.md)
  + [Microsoft.Skills.Vision.OcrSkill](cognitive-search-skill-ocr.md)
  + [Microsoft.Skills.Util.ConditionalSkill](cognitive-search-skill-conditional.md)
  + [Microsoft.Skills.Util.DocumentExtractionSkill](cognitive-search-skill-document-extraction.md)
  + [Microsoft.Skills.Util.ShaperSkill](cognitive-search-skill-shaper.md)

+ Aptitudes personalizadas
  + [Microsoft.Skills.Custom.WebApiSkill](cognitive-search-custom-skill-web-api.md)

+ [Aptitudes en desuso](cognitive-search-skill-deprecated.md)
  + [Microsoft.Skills.Text.NamedEntityRecognitionSkill](cognitive-search-skill-named-entity-recognition.md)

+ [REST API](https://docs.microsoft.com/rest/api/searchservice/)
  + [Create Skillset (api-version=2019-05-06)](https://docs.microsoft.com/rest/api/searchservice/create-skillset)
  + [Create Indexer (api-version=2019-05-06)](https://docs.microsoft.com/rest/api/searchservice/create-indexer)

## <a name="see-also"></a>Consulte también

+ [API REST de Azure Cognitive Search](https://docs.microsoft.com/rest/api/searchservice/)
+ [Indexadores de Azure Cognitive Search](search-indexer-overview.md)
+ [¿Qué es Azure Cognitive Search?](search-what-is-azure-search.md)
