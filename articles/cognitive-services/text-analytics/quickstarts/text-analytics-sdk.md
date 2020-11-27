---
title: 'Inicio rápido: Minería de datos mediante la biblioteca cliente de Text Analytics'
titleSuffix: Azure Cognitive Services
description: Use este inicio rápido para realizar un análisis de sentimiento, entre otras funciones, mediante la API Text Analytics desde Azure Cognitive Services.
services: cognitive-services
author: aahill
manager: nitinme
ms.service: cognitive-services
ms.subservice: text-analytics
ms.topic: quickstart
ms.date: 10/07/2020
ms.author: aahi
keywords: text mining, sentiment analysis, text analytics
ms.custom: devx-track-python, devx-track-js, devx-track-csharp, cog-serv-seo-aug-2020
zone_pivot_groups: programming-languages-text-analytics
ms.openlocfilehash: 5a0856df71f87e49c1a7d627ba92419352c796d5
ms.sourcegitcommit: f311f112c9ca711d88a096bed43040fcdad24433
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 11/20/2020
ms.locfileid: "94980950"
---
# <a name="quickstart-use-the-text-analytics-client-library"></a>Inicio rápido: Uso de la biblioteca cliente de Text Analytics

Use este artículo para empezar a trabajar con la biblioteca cliente de Text Analytics. Siga estos pasos para instalar el paquete y probar el código de ejemplo en el texto de la minería.

La biblioteca cliente de Text Analytics se puede usar para realizar las siguientes operaciones:

* análisis de opiniones
* Detección de idiomas
* Reconocimiento de entidades
* Extracción de la frase clave
* Minería de opiniones

::: zone pivot="programming-language-csharp"

> [!IMPORTANT]
> * La versión estable más reciente de Text Analytics API es `3.0`.
>    * Asegúrese de seguir únicamente las instrucciones de la versión que está usando.
> * Por motivos de simplicidad, en el código de este artículo se usan métodos sincrónicos y almacenamiento de credenciales no protegidas. En escenarios de producción, se recomienda usar los métodos asincrónicos por lotes para mayor rendimiento y escalabilidad. Para más información, consulte la siguiente documentación de referencia.
> * Si desea usar Text Analytics for Health u operaciones asincrónicas, consulte en GitHub los ejemplos para [C#](https://github.com/Azure/azure-sdk-for-net/tree/master/sdk/textanalytics/Azure.AI.TextAnalytics), [Python](https://github.com/Azure/azure-sdk-for-python/tree/master/sdk/textanalytics/azure-ai-textanalytics/) o [Java](https://github.com/Azure/azure-sdk-for-java/tree/master/sdk/textanalytics/azure-ai-textanalytics)

[!INCLUDE [v3 region availability](../includes/v3-region-availability.md)]

[!INCLUDE [C# quickstart](../includes/quickstarts/csharp-sdk.md)]

::: zone-end

::: zone pivot="programming-language-java"

> [!IMPORTANT]
> * La versión estable más reciente de Text Analytics API es `3.0`.
> * Por motivos de simplicidad, en el código de este artículo se usan métodos sincrónicos y almacenamiento de credenciales no protegidas. En escenarios de producción, se recomienda usar los métodos asincrónicos por lotes para mayor rendimiento y escalabilidad. Para más información, consulte la siguiente documentación de referencia.
Si desea usar Text Analytics for Health u operaciones asincrónicas, consulte en GitHub los ejemplos para [C#](https://github.com/Azure/azure-sdk-for-net/tree/master/sdk/textanalytics/Azure.AI.TextAnalytics), [Python](https://github.com/Azure/azure-sdk-for-python/tree/master/sdk/textanalytics/azure-ai-textanalytics/) o [Java](https://github.com/Azure/azure-sdk-for-java/tree/master/sdk/textanalytics/azure-ai-textanalytics)

[!INCLUDE [Java quickstart](../includes/quickstarts/java-sdk.md)]

::: zone-end

::: zone pivot="programming-language-javascript"

> [!IMPORTANT]
> * La versión estable más reciente de Text Analytics API es `3.0`.
>    * Asegúrese de seguir únicamente las instrucciones de la versión que está usando.
> * Por motivos de simplicidad, en el código de este artículo se usan métodos sincrónicos y almacenamiento de credenciales no protegidas. En escenarios de producción, se recomienda usar los métodos asincrónicos por lotes para mayor rendimiento y escalabilidad. Para más información, consulte la siguiente documentación de referencia.
> * Esta versión de la biblioteca cliente de Text Analytics también se puede ejecutar en el [explorador](https://github.com/Azure/azure-sdk-for-js/blob/master/documentation/Bundling.md).

[!INCLUDE [NodeJS quickstart](../includes/quickstarts/nodejs-sdk.md)]

::: zone-end

::: zone pivot="programming-language-python"

> [!IMPORTANT]
> * La versión estable más reciente de Text Analytics API es `3.0`.
>    * Asegúrese de seguir únicamente las instrucciones de la versión que está usando.
> * Por motivos de simplicidad, en el código de este artículo se usan métodos sincrónicos y almacenamiento de credenciales no protegidas. En escenarios de producción, se recomienda usar los métodos asincrónicos por lotes para mayor rendimiento y escalabilidad. Para más información, consulte la siguiente documentación de referencia. Si desea usar Text Analytics for Health u operaciones asincrónicas, consulte en GitHub los ejemplos para [C#](https://github.com/Azure/azure-sdk-for-net/tree/master/sdk/textanalytics/Azure.AI.TextAnalytics), [Python](https://github.com/Azure/azure-sdk-for-python/tree/master/sdk/textanalytics/azure-ai-textanalytics/) o [Java](https://github.com/Azure/azure-sdk-for-java/tree/master/sdk/textanalytics/azure-ai-textanalytics)

[!INCLUDE [Python quickstart](../includes/quickstarts/python-sdk.md)]

::: zone-end

::: zone pivot="programming-language-other"

## <a name="additional-language-support"></a>Compatibilidad para lenguajes adicionales

Si ha hecho clic en esta pestaña, es probable que no vea un inicio rápido en su lenguaje de programación favorito. No se preocupe, tenemos inicios rápidos adicionales disponibles. Use la tabla para encontrar el ejemplo correcto para su lenguaje de programación.

| Idioma | Versión disponible | 
|----------|------------------------|
| Ruby     | [Versión 2.1](ruby-sdk.md) | 
| Go       | [Versión 2.1](go-sdk.md) | 

::: zone-end

## <a name="clean-up-resources"></a>Limpieza de recursos

Si quiere limpiar y eliminar una suscripción a Cognitive Services, puede eliminar el recurso o grupo de recursos. Al eliminar el grupo de recursos, también se elimina cualquier otro recurso que esté asociado a él.

* [Portal](../../cognitive-services-apis-create-account.md#clean-up-resources)
* [CLI de Azure](../../cognitive-services-apis-create-account-cli.md#clean-up-resources)

## <a name="next-steps"></a>Pasos siguientes

> [!div class="nextstepaction"]
> [Exploración de una solución](../text-analytics-user-scenarios.md#analyze-recorded-inbound-customer-calls)

* [Información general de Text Analytics](../overview.md)
* [Análisis de opiniones](../how-tos/text-analytics-how-to-sentiment-analysis.md)
* [Reconocimiento de entidades](../how-tos/text-analytics-how-to-entity-linking.md)
* [Detección de idioma](../how-tos/text-analytics-how-to-keyword-extraction.md)
* [Reconocimiento de idioma](../how-tos/text-analytics-how-to-language-detection.md)
