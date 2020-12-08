---
title: 'Introducción a Custom Speech: Servicio de voz'
titleSuffix: Azure Cognitive Services
description: Custom Speech es un conjunto de herramientas en línea que permiten evaluar y mejorar la precisión de la conversión de voz a texto de Microsoft para las aplicaciones, herramientas y productos.
services: cognitive-services
author: trevorbye
manager: nitinme
ms.service: cognitive-services
ms.subservice: speech-service
ms.topic: conceptual
ms.date: 11/11/2020
ms.author: trbye
ms.openlocfilehash: 96b8e4031520e1a0540fca7d84b3f9ae1cca9c22
ms.sourcegitcommit: d60976768dec91724d94430fb6fc9498fdc1db37
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 12/02/2020
ms.locfileid: "96490353"
---
# <a name="what-is-custom-speech"></a>¿Qué es Custom Speech?

[Custom Speech](https://aka.ms/customspeech) es un conjunto de herramientas basadas en interfaz de usuario que permiten evaluar y mejorar la precisión de la conversión de voz en texto de Microsoft para las aplicaciones y productos. Para empezar a usarlo solo se necesita una serie de archivos de audio de prueba. Siga los vínculos de este artículo para empezar a crear una experiencia personalizada de conversión de voz a texto.

## <a name="whats-in-custom-speech"></a>¿Qué incluye Custom Speech?

Para utilizar Custom Speech, necesitará una cuenta de Azure y una suscripción al Servicio de voz. Una vez que tenga una cuenta, podrá preparar los datos, entrenar y probar sus modelos, inspeccionar la calidad del reconocimiento, evaluar la precisión y, en última instancia, implementar y utilizar el modelo de conversión de voz a texto personalizado.

Este diagrama resalta las partes que componen el [portal de Habla personalizada](https://aka.ms/customspeech). Use los siguientes vínculos para obtener más información sobre cada paso.

![Diagrama que resalta los componentes que constituyen el portal de Custom Speech.](./media/custom-speech/custom-speech-overview.png)

1. [Suscripción y creación de un proyecto](#set-up-your-azure-account). Cree una cuenta de Azure y suscríbase al servicio de voz. Esta suscripción unificada proporciona acceso a la conversión de voz a texto, la conversión de texto a voz, la traducción de voz y el [portal de Habla personalizada](https://speech.microsoft.com/customspeech). Luego, utilice la suscripción al Servicio de voz para crear su primer proyecto de Custom Speech.

1. [Carga de datos de prueba](./how-to-custom-speech-test-and-train.md). Cargue datos de prueba (archivos de audio) para evaluar la oferta de conversión de voz a texto de Microsoft para sus aplicaciones, herramientas y productos.

1. [Inspección de la calidad del reconocimiento](how-to-custom-speech-inspect-data.md). Use el [portal de Custom Speech](https://speech.microsoft.com/customspeech) para reproducir el audio cargado e inspeccionar la calidad del reconocimiento de voz de los datos de prueba. Para conocer las medidas cuantitativas, consulte [Inspección de los datos](how-to-custom-speech-inspect-data.md).

1. [Evaluación y mejora de la precisión](how-to-custom-speech-evaluate-data.md). Evalúe y mejore la precisión del modelo de conversión de voz a texto. El [portal de Custom Speech](https://speech.microsoft.com/customspeech) proporcionará una *tasa de errores de palabras*, que se puede usar para determinar si se necesita más entrenamiento. Si está satisfecho con la precisión, puede usar directamente las API del servicio de voz. Si desea mejorar la precisión en una media relativa entre el 5 y el 20 %, use la pestaña **Entrenamiento** del portal para cargar datos de entrenamiento adicionales, como transcripciones con etiqueta humana y texto relacionado.

1. [Entrenamiento e implementación de un modelo](how-to-custom-speech-train-model.md). Mejore la precisión del modelo de conversión de voz a texto incorporando transcripciones escritas (entre 10 y 1000 horas) y texto relacionado (menos de 200 MB) junto con los datos de prueba de audio. Estos datos ayudan a entrenar el modelo de conversión de voz a texto. Después del entrenamiento, vuelva a realizar la prueba. Si el resultado es satisfactorio, puede implementar el modelo en un punto de conexión personalizado.

## <a name="set-up-your-azure-account"></a>Configuración de la cuenta de Azure

Es preciso tener una cuenta de Azure y una suscripción al servicio de voz para poder usar el [portal de Custom Speech](https://speech.microsoft.com/customspeech) para crear un modelo personalizado. Si no dispone de una cuenta y una suscripción, [pruebe el servicio de voz de forma gratuita](overview.md#try-the-speech-service-for-free).

> [!NOTE]
> Asegúrese de crear una suscripción estándar (S0). No se admiten las suscripciones Gratis (F0).

Tras crear una cuenta de Azure y una suscripción al servicio de voz, deberá iniciar sesión en el [portal de Custom Speech](https://speech.microsoft.com/customspeech) y conectarse a su suscripción.

1. Inicie sesión en el [portal de Habla personalizada](https://aka.ms/custom-speech).
1. Seleccione la suscripción que necesita para trabajar y crear un proyecto de voz.
1. Si desea modificarla, seleccione el botón del engranaje en el menú superior.

## <a name="how-to-create-a-project"></a>Creación de un proyecto

El contenido, como datos, modelos, pruebas y puntos de conexión, se organiza en *proyectos* en el [portal de Custom Speech](https://speech.microsoft.com/customspeech). Cada proyecto es específico de un dominio y un país o idioma. Por ejemplo, puede crear un proyecto para centros de llamadas que usan el inglés en Estados Unidos.

Para crear el primer proyecto, seleccione **Speech-to-text/Custom speech** (Conversión de voz a texto/Conversión de voz personalizada) y, después, haga clic en **New project** (Nuevo proyecto). Siga las instrucciones del asistente para crear el proyecto. Después de crear el proyecto, debería ver cuatro pestañas: **Datos**, **Pruebas**, **Entrenamiento** e **Implementación**. Use los vínculos incluidos en [Pasos siguientes](#next-steps) para aprender a usar cada pestaña.

> [!IMPORTANT]
> ¡El [portal de Custom Speech](https://aka.ms/custom-speech) se ha actualizado recientemente! Si creó datos, modelos y pruebas anteriores y publicó puntos de conexión en el portal CRIS.ai o con API, debe crear un nuevo proyecto en el nuevo portal para conectarse a estas entidades antiguas.

## <a name="model-lifecycle"></a>Ciclo de vida del modelo

Custom Speech usa tanto *modelos base* como *modelos personalizados*. Cada idioma tiene uno o varios modelos base. Por lo general, cuando se publica un nuevo modelo de voz en el servicio de voz normal, también se importa en el servicio Custom Speech como nuevo modelo base. Se actualizan cada 3 a 6 meses. Los modelos anteriores normalmente dejan pierden utilidad con el tiempo, ya que el modelo más reciente suele tener mayor precisión.

En cambio, los modelos personalizados se crean mediante la adaptación de un modelo base elegido a un escenario de cliente determinado. Una vez que tenga un modelo personalizado que cubra sus necesidades, puede seguir usándolo durante mucho tiempo. Pero se recomienda actualizarlo periódicamente al modelo base más reciente y volver a entrenarlo con datos adicionales. 

Otros términos clave relacionados con el ciclo de vida del modelo incluyen:

* **Adaptación**: tomar un modelo base y personalizarlo para el dominio o escenario propios mediante datos de texto o datos de audio.
* **Descodificación**: usar un modelo y realizar el reconocimiento de voz (descodificar audio en texto).
* **Punto de conexión**: una implementación específica del usuario, bien de un modelo base, bien de un modelo personalizado al que *solo* puede acceder un usuario dado.

### <a name="expiration-timeline"></a>Escala de tiempo de expiración

A medida que los nuevos modelos y la nueva funcionalidad pasan a estar disponibles, se retiran los modelos más antiguos y menos precisos. Consulte las siguientes escalas de tiempo de expiración del modelo y el punto de conexión:

**Modelos base** 

* Adaptación: disponibles durante un año. Una vez que se importa el modelo, está disponible durante un año para crear modelos personalizados. Después de un año, deben crearse nuevos modelos personalizados a partir de una versión más reciente del modelo base.  
* Descodificación: disponibles durante dos años después de la importación. Por consiguiente, puede crear un punto de conexión y usar la transcripción por lotes durante dos años con este modelo. 
* Puntos de conexión: disponibles según la misma escala de tiempo que la descodificación.

**Modelos personalizados**

* Descodificación: disponible durante dos años después de la creación del modelo. Por consiguiente, el modelo personalizado se puede usar durante dos años (lote, tiempo real y pruebas) una vez creado. Después de dos años, *debe volver a entrenar el modelo*, ya que la mayoría de las veces el modelo base habrá quedado en desuso para la adaptación.  
* Puntos de conexión: disponibles según la misma escala de tiempo que la descodificación.

Cuando expira un modelo base o un modelo personalizado, siempre se revierte a la *versión más reciente del modelo base*. Por tanto, la implementación nunca se interrumpirá, pero puede ser menos precisa para *sus datos específicos* si los modelos personalizados llegan a expirar. Puede ver la expiración de un modelo en los siguientes lugares del portal de Habla personalizada:

* Resumen del entrenamiento del modelo
* Detalles del entrenamiento del modelo
* Resumen de implementación
* Detalles de la implementación

También puede comprobar las fechas de expiración mediante las API [`GetModel`](https://westus.dev.cognitive.microsoft.com/docs/services/speech-to-text-api-v3-0/operations/GetModel) y [`GetBaseModel`](https://westus.dev.cognitive.microsoft.com/docs/services/speech-to-text-api-v3-0/operations/GetBaseModel) de habla personalizada, en la propiedad `deprecationDates` de la respuesta JSON.

Tenga en cuenta que puede actualizar el modelo en un punto de conexión de habla personalizada sin tiempo de inactividad si cambia el modelo usado por el punto de conexión en la sección de implementación del portal de Habla personalizada o por medio de la API de habla personalizada.

## <a name="next-steps"></a>Pasos siguientes

* [Preparación y prueba de los datos](./how-to-custom-speech-test-and-train.md)
* [Inspección de los datos](how-to-custom-speech-inspect-data.md)
* [Evaluación y mejora de la precisión del modelo](how-to-custom-speech-evaluate-data.md)
* [Entrenamiento e implementación de un modelo](how-to-custom-speech-train-model.md)