---
title: ¿Qué es Azure Cognitive Services?
titleSuffix: Azure Cognitive Services
description: Cognitive Services hace la AI accesible a todos los desarrolladores, sin necesidad de tener conocimientos de aprendizaje automático y de ciencia de datos. Solo tiene que realizar una llamada de API desde su aplicación para agregar la capacidad de ver (búsqueda y reconocimiento avanzados de imágenes), oír, hablar, buscar y tomar decisiones a las aplicaciones.
services: cognitive-services
author: nitinme
manager: nitinme
keywords: cognitive services, cognitive intelligence, cognitive solutions, ai services, cognitive understanding, cognitive features
ms.service: cognitive-services
ms.subservice: ''
ms.topic: overview
ms.date: 10/22/2020
ms.author: nitinme
ms.custom: cog-serv-seo-aug-2020
ms.openlocfilehash: 82531466c80917087ca007900ca79b3485f38a21
ms.sourcegitcommit: d767156543e16e816fc8a0c3777f033d649ffd3c
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/26/2020
ms.locfileid: "92546269"
---
# <a name="what-are-azure-cognitive-services"></a>¿Qué es Azure Cognitive Services?

Azure Cognitive Services son servicios basados en la nube con API REST y SDK de biblioteca cliente disponibles para ayudarle a integrar inteligencia cognitiva en sus aplicaciones. Puede agregar características cognitivas a sus aplicaciones sin tener conocimientos sobre inteligencia artificial (IA) o ciencia de datos. Azure Cognitive Services incluye varios servicios de inteligencia artificial que le permiten desarrollar soluciones cognitivas con capacidad de ver, oír, hablar, comprender e incluso tomar decisiones.

## <a name="categories-of-cognitive-services"></a>Categorías de Cognitive Services

El catálogo de Cognitive Services, que facilita la comprensión cognitiva, consta de cinco pilares básicos:

* Visión
* Voz
* Idioma
* Decisión
* Search

Las siguientes secciones de este artículo ofrecen una lista de los servicios que forman parte de estos cinco pilares.

## <a name="vision-apis"></a>API de visión

|Nombre de servicio|Descripción del servicio|
|:-----------|:------------------|
|[Computer Vision](https://docs.microsoft.com/azure/cognitive-services/computer-vision/ "Computer Vision")|El servicio Computer Vision proporciona acceso a algoritmos cognitivos avanzados para procesar imágenes y devolver información.|
|[Custom Vision Service](https://docs.microsoft.com/azure/cognitive-services/Custom-Vision-Service/home "Custom Vision Service")|Custom Vision Service permite crear clasificadores personalizados de imágenes.|
|[Face](https://docs.microsoft.com/azure/cognitive-services/face/ "Caras")| El servicio Face proporciona acceso a algoritmos faciales avanzados, lo que permite la detección y el reconocimiento de atributos faciales.|
|[Form Recognizer](https://docs.microsoft.com/azure/cognitive-services/form-recognizer/ "Form Recognizer")|Form Recognizer identifica y extrae pares clave-valor y datos de tabla de documentos de formulario; luego, genera datos estructurados, incluidas las relaciones en el archivo original.|
|[Ink Recognizer](https://docs.microsoft.com/azure/cognitive-services/ink-recognizer/ "Ink Recognizer") (retirada)|Ink Recognizer le permite reconocer y analizar los datos del trazo de tinta digital, las formas y el contenido escrito a mano, y genera una estructura de documento con todas las entidades reconocidas.|
|[Video Indexer](https://docs.microsoft.com/azure/cognitive-services/video-indexer/video-indexer-overview "Video Indexer")|Video Indexer permite extraer información de un vídeo.|

## <a name="speech-apis"></a>API de voz

|Nombre de servicio|Descripción del servicio|
|:-----------|:------------------|
|[Servicio Voz](https://docs.microsoft.com/azure/cognitive-services/speech-service/ "Servicio de voz")|El servicio de voz agrega a las aplicaciones características habilitadas por voz. El servicio Voz incluye varias funciones, entre otras: voz a texto, texto a voz y traducción de voz.|
<!--
|[Speaker Recognition API](https://docs.microsoft.com/azure/cognitive-services/speaker-recognition/home "Speaker Recognition API") (Preview)|The Speaker Recognition API provides algorithms for speaker identification and verification.|
|[Bing Speech](https://docs.microsoft.com/azure/cognitive-services/speech/home "Bing Speech") (Retiring)|The Bing Speech API provides you with an easy way to create speech-enabled features in your applications.|
|[Translator Speech](https://docs.microsoft.com/azure/cognitive-services/translator-speech/ "Translator Speech") (Retiring)|Translator Speech is a machine translation service.|
-->
## <a name="language-apis"></a>API de idioma

|Nombre de servicio|Descripción del servicio|
|:-----------|:------------------|
|[Language Understanding: LUIS](https://docs.microsoft.com/azure/cognitive-services/luis/ "Language Understanding")|Aprenda a usar el servicio Language Understanding para permitir que la aplicación entienda lo que una persona quiere en sus propias palabras.|
|[QnA Maker](https://docs.microsoft.com/azure/cognitive-services/qnamaker/index "QnA Maker")|QnA Maker permite generar un servicio de preguntas y respuestas a partir de contenido semiestructurado.|
|[Text Analytics](https://docs.microsoft.com/azure/cognitive-services/text-analytics/ "Text Analytics")| Proporciona procesamiento de lenguaje natural en texto sin formato para el análisis de opiniones, la extracción de frases clave y la detección del idioma.|
|[Traductor](https://docs.microsoft.com/azure/cognitive-services/translator/ "Traductor")|Traductor proporciona la traducción de texto automática casi en tiempo real.|
| [Lector inmersivo](https://docs.microsoft.com/azure/cognitive-services/immersive-reader/ "Lector inmersivo") | El Lector inmersivo agrega a las aplicaciones funcionalidad de comprensión y lectura en pantalla. |

## <a name="decision-apis"></a>API de Decision

|Nombre de servicio|Descripción del servicio|
|:-----------|:------------------|
|[Anomaly Detector](https://docs.microsoft.com/azure/cognitive-services/anomaly-detector/ "Anomaly Detector") |Anomaly Detector permite supervisar y detectar anomalías en datos de series temporales.|
|[Content Moderator](https://docs.microsoft.com/azure/cognitive-services/content-moderator/overview "Content Moderator")|Content Moderator proporciona la supervisión de posibles contenidos ofensivos, indeseables y peligrosos.|
|[Metrics Advisor](https://docs.microsoft.com/azure/cognitive-services/metrics-advisor) (versión preliminar) | Metrics Advisor proporciona una detección de anomalías personalizable en los datos de serie temporal de varios variados y un portal web con todas las características para ayudarle a usar el servicio.|
|[Personalizer](https://docs.microsoft.com/azure/cognitive-services/personalizer/ "Personalizer")|Personalizer permite elegir la mejor experiencia para mostrar a los usuarios y aprender de su comportamiento en tiempo real.|

## <a name="search-apis"></a>API de búsqueda

> [!NOTE]
> ¿Busca [Azure Cognitive Search](https://docs.microsoft.com/azure/search/)? Aunque usa Cognitive Services para algunas tareas, es una tecnología de búsqueda diferente que admite otros escenarios.

|Nombre de servicio|Descripción del servicio|
|:-----------|:------------------|
|[Bing News Search](https://docs.microsoft.com/azure/cognitive-services/bing-news-search/ "Bing News Search")|Bing News Search devuelve una lista de artículos de noticias cuya relevancia se ha determinado para la consulta del usuario.|
|[Bing Video Search](https://docs.microsoft.com/azure/cognitive-services/Bing-Video-Search/ "Bing Video Search")|Bing Video Search devuelve una lista de vídeos cuya relevancia se ha determinado para la consulta del usuario.|
|[Bing Web Search](https://docs.microsoft.com/azure/cognitive-services/bing-web-search/ "Bing Web Search")|Bing Web Search devuelve una lista de resultados de la búsqueda cuya relevancia se ha determinado para la consulta del usuario.|
|[Bing Autosuggest](https://docs.microsoft.com/azure/cognitive-services/Bing-Autosuggest "Bing Autosuggest")|Bing Autosuggest permite enviar un término de consulta de búsqueda parcial a Bing y obtener una lista de consultas sugeridas.|
|[Bing Custom Search](https://docs.microsoft.com/azure/cognitive-services/bing-custom-search "Bing Custom Search")|Bing Custom Search permite crear experiencias de búsqueda a medida de los temas que le interesan.|
|[Bing Entity Search](https://docs.microsoft.com/azure/cognitive-services/bing-entities-search/ "Bing Entity Search")|Bing Entity Search devuelve información sobre las entidades que Bing determina que están relacionadas con la consulta del usuario.|
|[Bing Image Search](https://docs.microsoft.com/azure/cognitive-services/bing-image-search "Bing Image Search")|Bing Image Search devuelve una lista de imágenes cuya relevancia se ha determinado para la consulta del usuario.|
|[Bing Visual Search](https://docs.microsoft.com/azure/cognitive-services/bing-visual-search "Bing Visual Search")|Bing Visual Search devuelve información sobre una imagen, como imágenes visualmente similares, orígenes de compra de productos encontrados en la imagen y búsquedas relacionadas.|
|[Bing Local Business Search](https://docs.microsoft.com/azure/cognitive-services/bing-local-business-search/ "Bing Local Business Search")| La API Bing Local Business Search permite que las aplicaciones busquen información de ubicación y contacto sobre las empresas locales en función de las consultas de búsqueda.|
|[Bing Spell Check](https://docs.microsoft.com/azure/cognitive-services/bing-spell-check/ "Bing Spell Check")|Bing Spell Check permite realizar correcciones de gramática y ortografía en contexto.|

## <a name="development-options"></a>Opciones de desarrollo 

Con Azure y Cognitive Services, dispone de varias opciones de desarrollo, como las siguientes:

* Herramientas de automatización e integración, como Logic Apps y Power Automate.
* Opciones de implementación, como Azure Functions y App Service. 
* Contenedores de Cognitive Services para el acceso seguro.
* Herramientas como Apache Spark, Azure Databricks, Azure Synapse Analytics y Azure Kubernetes Service para escenarios de macrodatos. 

Para más información, consulte [Opciones de desarrollo de Cognitive Services](./cognitive-services-development-options.md).

## <a name="learn-with-the-quickstarts"></a>Aprenda con las guías de inicio rápido

Aprenda a crear un recurso de Cognitive Services con inicios rápidos prácticos mediante el uso de:

* [Azure Portal](cognitive-services-apis-create-account.md?tabs=multiservice%2Cwindows "Azure portal")
* [CLI de Azure](cognitive-services-apis-create-account-cli.md?tabs=windows "CLI de Azure")
* [Bibliotecas cliente de Azure SDK](cognitive-services-apis-create-account-cli.md?tabs=windows "cognitive-services-apis-create-account-client-library?pivots=programming-language-csharp")
* [Plantillas de Azure Resource Manager (ARM)](resource-manager-template.md?tabs=portal "Plantillas de Azure Resource Manager (ARM)")

<!--
## Subscription management

Once you are signed in with your Microsoft Account, you can access [My subscriptions](https://www.microsoft.com/cognitive-services/subscriptions "My subscriptions") to show the products you are using, the quota remaining, and the ability to add additional products to your subscription.

## Upgrade to unlock higher limits

All APIs have a free tier, which has usage and throughput limits.  You can increase these limits by using a paid offering and selecting the appropriate pricing tier option when deploying the service in the Azure portal. [Learn more about the offerings and pricing](https://azure.microsoft.com/pricing/details/cognitive-services/ "offerings and pricing"). You'll need to set up an Azure subscriber account with a credit card and a phone number. If you have a special requirement or simply want to talk to sales, click "Contact us" button at the top the pricing page.
-->

## <a name="using-cognitive-services-securely"></a>Uso de Cognitive Services de forma segura

Azure Cognitive Services proporciona un modelo de seguridad por niveles, lo que incluye la [autenticación ](authentication.md "autenticación") a través de credenciales de Azure Active Directory, una clave de recurso válida e [instancias de Azure Virtual Network](cognitive-services-virtual-networks.md "Instancias de Azure Virtual Network").

## <a name="containers-for-cognitive-services"></a>Contenedores para Cognitive Services

 Cognitive Services proporciona contenedores para la implementación en la nube de Azure o localmente. Obtenga más información sobre [contenedores de Cognitive Services](cognitive-services-container-support.md "Contenedores de Cognitive Services").

## <a name="regional-availability"></a>Disponibilidad regional

Las API de Cognitive Services se hospedan en una red cada vez mayor de centros de datos administrados por Microsoft. Encontrará la disponibilidad regional de cada API en la [lista de regiones de Azure](https://azure.microsoft.com/regions "Lista de regiones de Azure").

¿Busca una región que aún no se admite? Díganoslo. Envíe una solicitud de característica en nuestro [foro de UserVoice](https://cognitive.uservoice.com/ "Foro de UserVoice").

## <a name="supported-cultural-languages"></a>Idiomas culturales admitidos

Cognitivas Services admite una amplia gama de idiomas culturales en el nivel de servicio. Puede encontrar la disponibilidad de idiomas para cada API en la [lista de idiomas admitidos](language-support.md "lista de idiomas que se admiten").

## <a name="certifications-and-compliance"></a>Certificaciones y cumplimiento

Cognitivas Services ha recibido certificaciones como la certificación CSA STAR, FedRAMP Moderate y HIPAA BAA. Puede [descargar](https://gallery.technet.microsoft.com/Overview-of-Azure-c1be3942 "descarga") certificaciones para sus propias auditorías y revisiones de seguridad.

Para comprender la administración de los datos y la privacidad, vaya al [Centro de confianza](https://servicetrust.microsoft.com/ "Centro de confianza").

## <a name="support"></a>Soporte técnico

Cognitive Services proporciona varias opciones de soporte técnico para ayudarle a avanzar en el desarrollo de aplicaciones inteligentes. Cognitive Services también cuenta con una sólida comunidad de desarrolladores que pueden ayudarle a responder preguntas específicas. Para obtener una lista completa de las opciones disponibles, consulte [Opciones de ayuda y de soporte técnico de Azure Cognitive Services](cognitive-services-support-options.md "Opciones de ayuda y de soporte técnico de Azure Cognitive Services").

## <a name="next-steps"></a>Pasos siguientes

* [Creación de una cuenta de Cognitive Services](cognitive-services-apis-create-account.md "Creación de una cuenta de Cognitive Services")
* [Novedades en la documentación de Cognitive Services](whats-new-docs.md "Novedades en la documentación de Cognitive Services")
