---
title: ¿Qué es Translator Text API? Translator Text API
titlesuffix: Azure Cognitive Services
description: Integre Translator Text API en aplicaciones, sitios web, herramientas u otras soluciones para proporcionar experiencias de usuario en varios idiomas.
services: cognitive-services
author: swmachan
manager: nitinme
ms.service: cognitive-services
ms.subservice: translator-text
ms.topic: overview
ms.date: 12/09/2019
ms.author: swmachan
ms.custom: seodec18
ms.openlocfilehash: f6b55163440b25bf69c76e9b1217c439b0884d5d
ms.sourcegitcommit: 9ee0cbaf3a67f9c7442b79f5ae2e97a4dfc8227b
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/27/2020
ms.locfileid: "78331049"
---
# <a name="what-is-the-translator-text-api"></a>¿Qué es Translator Text API?

Translator Text API es fácil de integrar en sus aplicaciones, sitios web, herramientas y soluciones. Permite agregar experiencias de usuario multilingüe en [más de 60 idiomas](languages.md) y se puede usar en cualquier plataforma de hardware con cualquier sistema operativo para la traducción de texto a texto.

Translator Text API forma parte de [Azure Cognitive Services API](https://docs.microsoft.com/azure/?pivot=products&panel=ai), una colección de algoritmos de aprendizaje automático y de inteligencia artificial en la nube, que se pueden consumir fácilmente en los proyectos de desarrollo.

## <a name="about-microsoft-translator"></a>Acerca de Microsoft Translator

Microsoft Translator es un servicio de traducción automática basado en la nube. En el centro de este servicio está Translator Text API, que da servicio a diversos productos y servicios de Microsoft y que utilizan miles de empresas en todo el mundo en sus aplicaciones y flujos de trabajo para que su contenido llegue a una audiencia mundial.

La traducción de voz, con la tecnología de Translator Text API, también está disponible mediante [Microsoft Speech Service](https://docs.microsoft.com/azure/cognitive-services/speech-service/). Combina la funcionalidad de Translator Speech API y Custom Speech Service en un servicio totalmente personalizable y unificado. Speech Service reemplaza a Translator Speech API, que se retirará el 15 de octubre de 2019.

## <a name="language-support"></a>Compatibilidad con idiomas

Microsoft Translator proporciona compatibilidad con varios idiomas de traducción, transliteración, detección de idioma y diccionarios. Consulte [Compatibilidad con idiomas](language-support.md) para obtener una lista completa o puede acceder a la lista mediante programación con la [API REST](https://docs.microsoft.com/azure/cognitive-services/translator/reference/v3-0-languages).  

## <a name="microsoft-translator-neural-machine-translation"></a>Traducción automática neuronal de Microsoft Translator

La traducción automática neuronal (NMT) es el nuevo estándar para las traducciones automáticas de alta calidad basadas en inteligencia artificial. Este estándar reemplaza la traducción automática estadística (SMT) que alcanzó un nivel estable a mediados de 2010.

NMT proporciona mejores traducciones que SMT, no solo a la hora de puntuar la calidad de la traducción bruta, sino también porque suenan más fluidas y humanas. El motivo principal de esta fluidez es que NMT usa el contexto completo de una frase para traducir las palabras. SMT solo tomaba el contexto inmediato de unas cuantas palabras antes y después de cada palabra.

Los modelos NMT se sitúan en el centro de la API y no son visibles para los usuarios finales. La única diferencia perceptible es una mejor calidad de la traducción, en especial en los idiomas chino, japonés y árabe.

Más información sobre [el funcionamiento de NMT](https://www.microsoft.com/en-us/translator/mt.aspx#nnt)

## <a name="language-customization"></a>Personalización de lenguaje

Custom Translator, una extensión del servicio Microsoft Translator, se puede usar en combinación con Translator Text API para ayudar a personalizar el sistema de traducción neuronal y mejorar la traducción para su terminología y estilo específicos.

Con Custom Translator puede crear sistemas de traducción que administran la terminología usada en su propio negocio o sector. Luego, los sistemas de traducción personalizados se pueden integrar fácilmente en las aplicaciones, flujos de trabajo y sitios web ya existentes, en varios tipos de dispositivos, mediante el componente normal Microsoft Translator Text API, por medio del parámetro de categoría.

Más información sobre la [personalización de idioma](customization.md)

## <a name="next-steps"></a>Pasos siguientes

- [Regístrese](translator-text-how-to-signup.md) para obtener una clave de acceso.
- [Referencia de API](https://docs.microsoft.com/azure/cognitive-services/Translator/reference/v3-0-reference) proporciona la documentación técnica de las API.
- [Detalles de precios](https://azure.microsoft.com/pricing/details/cognitive-services/translator-text-api/)
