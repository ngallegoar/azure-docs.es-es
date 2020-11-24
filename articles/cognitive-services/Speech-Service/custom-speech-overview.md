---
title: 'Introducción a Habla personalizada: Servicio de voz'
titleSuffix: Azure Cognitive Services
description: Habla personalizada es un conjunto de herramientas en línea que permiten evaluar y mejorar la precisión de la conversión de voz a texto para las aplicaciones, las herramientas y los productos. Para comenzar solo se necesita una serie de archivos de audio de prueba. Siga los vínculos que se incluyen a continuación para empezar a crear una experiencia personalizada de conversión de voz a texto.
services: cognitive-services
author: trevorbye
manager: nitinme
ms.service: cognitive-services
ms.subservice: speech-service
ms.topic: conceptual
ms.date: 11/11/2020
ms.author: trbye
ms.openlocfilehash: 6dbe97d615753f0a90c8ba80aa7afa6dafa15eb2
ms.sourcegitcommit: 8e7316bd4c4991de62ea485adca30065e5b86c67
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 11/17/2020
ms.locfileid: "94658475"
---
# <a name="what-is-custom-speech"></a>¿Qué es Habla personalizada?

[Habla personalizada](https://aka.ms/customspeech) es un conjunto de herramientas basadas en interfaz de usuario que permiten evaluar y mejorar la precisión de la conversión de voz en texto de Microsoft para las aplicaciones y productos. Para comenzar solo se necesita una serie de archivos de audio de prueba. Siga los vínculos que se incluyen a continuación para empezar a crear una experiencia personalizada de conversión de voz a texto.

## <a name="whats-in-custom-speech"></a>¿Qué incluye Habla personalizada?

Para utilizar Habla personalizada, necesitará una cuenta de Azure y una suscripción al Servicio de voz. Cuando ya tenga la cuenta, podrá preparar los datos, entrenar y probar sus modelos, inspeccionar la calidad del reconocimiento, evaluar la precisión y, en última instancia, implementar y utilizar el modelo de conversión de voz a texto personalizado.

Este diagrama resalta las partes que componen el [portal de Habla personalizada](https://aka.ms/customspeech). Use los siguientes vínculos para obtener más información sobre cada paso.

![Se resaltan los distintos componentes que constituyen el portal de Habla personalizada.](./media/custom-speech/custom-speech-overview.png)

1. [Suscríbase y cree un proyecto](#set-up-your-azure-account): cree una cuenta de Azure y suscríbase al servicio de voz. Esta suscripción unificada proporciona acceso a la conversión de voz a texto, la conversión de texto a voz, la traducción de voz y el [portal de Habla personalizada](https://speech.microsoft.com/customspeech). A continuación, mediante la suscripción al Servicio de voz, cree su primer proyecto de Habla personalizada.

1. [Cargue datos de prueba](how-to-custom-speech-test-data.md): cargue datos de prueba (archivos de audio) para evaluar la oferta de conversión de voz a texto para sus aplicaciones, herramientas y productos.

1. [Inspeccione la calidad del reconocimiento](how-to-custom-speech-inspect-data.md): use el [portal de Habla personalizada](https://speech.microsoft.com/customspeech) para reproducir el audio cargado e inspeccionar la calidad del reconocimiento de voz de los datos de prueba. Para conocer las medidas cuantitativas, consulte [Inspección de los datos](how-to-custom-speech-inspect-data.md).

1. [Evalúe y mejore la precisión](how-to-custom-speech-evaluate-data.md): evalúe y mejore la precisión del modelo de conversión de voz en texto. El [portal de Habla personalizada](https://speech.microsoft.com/customspeech) proporcionará una *tasa de errores de palabras*, que puede utilizarse para determinar si se necesita más entrenamiento. Si está satisfecho con la precisión, puede usar directamente las API del servicio de voz. Si desea mejorar la precisión en una media relativa del 5 al 20 %, use la pestaña **Entrenamiento** del portal para cargar datos de entrenamiento adicionales, como transcripciones con etiqueta humana y texto relacionado.

1. [Entrene e implemente el modelo](how-to-custom-speech-train-model.md): mejore la precisión del modelo de conversión de voz en texto uniendo transcripciones escritas (de 10 a 1000 horas) y texto relacionado (< 200 MB) a datos de prueba de audio. Estos datos ayudan a entrenar el modelo de conversión de voz a texto. Después del entrenamiento, vuelva a probar; si le satisface el resultado, ya puede implementar el modelo en un punto de conexión personalizado.

## <a name="set-up-your-azure-account"></a>Configuración de la cuenta de Azure

Para poder usar el [portal de Habla personalizada](https://speech.microsoft.com/customspeech) y crear un modelo personalizado, se necesita una cuenta de Azure y una suscripción al servicio de voz. Si no dispone de una cuenta y una suscripción, [pruebe el servicio de voz de forma gratuita](overview.md#try-the-speech-service-for-free).

> [!NOTE]
> Asegúrese de crear suscripciones estándar (S0); no se admiten las suscripciones gratuitas (F0).

Después de crear la cuenta de Azure y la suscripción al servicio de voz, deberá iniciar sesión en el [portal de Habla personalizada](https://speech.microsoft.com/customspeech) y conectarse a su suscripción.

1. Inicie sesión en el [portal de Habla personalizada](https://aka.ms/custom-speech).
1. Seleccione la suscripción que necesita para trabajar y crear un proyecto de voz.
1. Si desea modificar la suscripción, utilice el icono **engranaje** situado en el panel de navegación superior.

## <a name="how-to-create-a-project"></a>Creación de un proyecto

El contenido, como datos, modelos, pruebas y puntos de conexión, se organiza en **proyectos** en el [portal de Habla personalizada](https://speech.microsoft.com/customspeech). Cada proyecto es específico de un dominio y un país o idioma. Por ejemplo, puede crear un proyecto para centros de llamadas que usan el inglés en Estados Unidos.

Para crear su primer proyecto, seleccione **Speech-to-text/Custom speech** (Conversión de voz a texto/Conversión de voz personalizada) y, a continuación, haga clic en **New project** (Nuevo proyecto). Siga las instrucciones del asistente para crear el proyecto. Después de crear el proyecto, verá cuatro pestañas: **Datos**, **Pruebas**, **Entrenamiento** e **Implementación**. Use los vínculos incluidos en [Pasos siguientes](#next-steps) para aprender a usar cada pestaña.

> [!IMPORTANT]
> ¡El [portal de Custom Speech](https://aka.ms/custom-speech) se ha actualizado recientemente! Si creó datos, modelos y pruebas anteriores y publicó puntos de conexión en el portal CRIS.ai o con API, debe crear un nuevo proyecto en el nuevo portal para conectarse a estas entidades antiguas.

## <a name="model-lifecycle"></a>Ciclo de vida del modelo

Habla personalizada usa tanto **modelos base** como **modelos personalizados**. Cada idioma tiene uno o varios **modelos base**. Por lo general, cuando se publica un nuevo modelo de voz en el servicio de voz normal, también se importa en el servicio Habla personalizada como un nuevo **modelo base**. Normalmente los modelos se actualizan con una frecuencia de 3 a 6 meses; los más antiguos pierden utilidad con el tiempo, ya que el modelo más reciente suele tener mayor precisión.

En cambio, los **modelos personalizados** se crean mediante la adaptación de un modelo base elegido a un escenario de cliente determinado. Puede seguir usando un modelo personalizado determinado durante un período de tiempo prolongado una vez que haya logrado una versión que satisfaga sus necesidades, pero se recomienda actualizar periódicamente al modelo base más reciente y volver a entrenar con datos adicionales.

Otros términos clave relacionados con el ciclo de vida del modelo incluyen:

* **Adaptación**: tomar un modelo base y personalizarlo para el dominio o escenario propios mediante datos de texto o datos de audio.
* **Descodificar**: usar un modelo y realizar el reconocimiento de voz (descodificar audio en texto).
* **Punto de conexión**: una implementación específica del usuario, bien de un modelo base, bien de un modelo personalizado al que *solo* puede acceder un usuario determinado.

### <a name="expiration-timeline"></a>Escala de tiempo de expiración

A medida que los nuevos modelos y la nueva funcionalidad pasan a estar disponibles, se retiran los modelos más antiguos y menos precisos. Consulte las siguientes escalas de tiempo de expiración del modelo y el punto de conexión:

**Modelos base** 

* Adaptación: disponibles durante un año. Una vez que se importa el modelo, está disponible durante un año para crear modelos personalizados. Después de un año, deben crearse nuevos modelos personalizados a partir de una versión más reciente del modelo base.  
* Descodificación: disponibles durante dos años después de la importación. Esto significa que puede crear un punto de conexión y usar la transcripción por lotes durante dos años con este modelo. 
* Puntos de conexión: disponibles según la misma escala de tiempo que la descodificación.

**Modelos personalizados**

* Descodificación: disponibles durante dos años después de la creación del modelo. Esto significa que puede usar el modelo personalizado durante dos años (lotes, tiempo real, pruebas) una vez creado. Después de dos años, **debe volver a entrenar el modelo**, porque la mayoría de las veces el modelo base habrá quedado en desuso para la adaptación.  
* Puntos de conexión: disponibles según la misma escala de tiempo que la descodificación.

Cuando expira un modelo base o un modelo personalizado, siempre se revierte a la **versión más reciente del modelo base**. Por lo tanto, la implementación nunca se interrumpirá, pero puede ser menos precisa para *sus datos específicos* si los modelos personalizados llegan a expirar. Puede ver la expiración de un modelo en los siguientes lugares del portal de Habla personalizada:

* Resumen del entrenamiento del modelo
* Detalles del entrenamiento del modelo
* Resumen de implementación
* Detalles de la implementación

También puede comprobar las fechas de expiración mediante las API [`GetModel`](https://westus.dev.cognitive.microsoft.com/docs/services/speech-to-text-api-v3-0/operations/GetModel) y [`GetBaseModel`](https://westus.dev.cognitive.microsoft.com/docs/services/speech-to-text-api-v3-0/operations/GetBaseModel) de habla personalizada, en la propiedad `deprecationDates` de la respuesta JSON.

Tenga en cuenta que puede actualizar el modelo en un punto de conexión de habla personalizada sin tiempo de inactividad si cambia el modelo usado por el punto de conexión en la sección de implementación del portal de Habla personalizada o por medio de la API de habla personalizada.

## <a name="next-steps"></a>Pasos siguientes

* [Preparación y prueba de los datos](how-to-custom-speech-test-data.md)
* [Inspección de los datos](how-to-custom-speech-inspect-data.md)
* [Evaluación y mejora de la precisión del modelo](how-to-custom-speech-evaluate-data.md)
* [Entrenamiento e implementación de un modelo](how-to-custom-speech-train-model.md)
