---
title: 'Creación de un proyecto de Voz personalizada: servicio de voz'
titleSuffix: Azure Cognitive Services
description: Cuando esté listo para cargar los datos, vaya al portal de voz personalizada. Cree o seleccione un proyecto de voz personalizada. El proyecto debe compartir el idioma/configuración regional y las propiedades de género correctos con los datos que pretende usar para el entrenamiento de voz.
services: cognitive-services
author: erhopf
manager: nitinme
ms.service: cognitive-services
ms.subservice: speech-service
ms.topic: conceptual
ms.date: 11/04/2019
ms.author: erhopf
ms.openlocfilehash: bbe1d651a7d2d2cac1b1aa78b815b2797ad185c5
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/28/2020
ms.locfileid: "76717316"
---
# <a name="create-a-custom-voice"></a>Creación de una voz personalizada

En [Preparar los datos para crear una voz personalizada](how-to-custom-voice-prepare-data.md), se describen los tipos de datos diferentes que puede usar para entrenar una voz personalizada y los distintos requisitos de formato. Cuando haya preparado los datos, puede empezar a cargarlos en el [portal de voz personalizada](https://aka.ms/custom-voice-portal), o a través de la API de entrenamiento de voz personalizada. Aquí se describen los pasos del entrenamiento de una voz personalizada mediante el portal.

> [!NOTE]
> En esta página se supone que ha leído [Empezar a trabajar con la voz personalizada](how-to-custom-voice.md) y [Preparar los datos para crear una voz personalizado](how-to-custom-voice-prepare-data.md) y que ha creado un proyecto de voz personalizada.

Compruebe los idiomas admitidos para la voz personalizada: [idioma para la personalización](language-support.md#customization).

## <a name="upload-your-datasets"></a>Cargar los conjuntos de datos

Cuando esté listo para cargar los datos, vaya al [portal de voz personalizada](https://aka.ms/custom-voice-portal). Cree o seleccione un proyecto de voz personalizada. El proyecto debe compartir el idioma/configuración regional y las propiedades de género correctos con los datos que pretende usar para el entrenamiento de voz. Por ejemplo, seleccione `en-GB` si las grabaciones de audio se han realizado en inglés con acento británico.

Vaya a la pestaña **Data** (Datos) y haga clic en **Upload data** (Cargar datos). En el asistente, seleccione el tipo de datos correcto que coincida con lo que ha preparado.

Cada conjunto de datos que cargue debe cumplir los requisitos del tipo de datos elegido. Es importante dar formato correctamente a los datos antes de cargarlos. Esto garantiza que el servicio de voz personalizada procesa los datos con precisión. Vaya a [Preparar los datos para crear una voz personalizada](how-to-custom-voice-prepare-data.md) y asegúrese de que los datos tienen el formato correcto.

> [!NOTE]
> Los usuarios de suscripción gratuita (F0) pueden cargar dos conjuntos de datos al mismo tiempo. En cambio, los usuarios con una suscripción estándar (S0) pueden cargar cinco conjuntos de datos a la vez. Si alcanza el límite, espere hasta que al menos uno de los conjuntos de datos finalice la importación. A continuación, inténtelo de nuevo.

> [!NOTE]
> El número máximo de conjuntos de datos que se pueden importar por suscripción es de 10 archivos ZIP para usuarios de la suscripción gratuita (F0) y 500 para usuarios para la suscripción estándar (S0).

Los conjuntos de datos se validan automáticamente una vez que pulsa el botón de carga. La validación de datos incluye una serie de comprobaciones en los archivos de audio para comprobar su formato de archivo, el tamaño y la frecuencia de muestreo. Corrija los errores si los hay y vuelva a realizarla. Cuando se inicia correctamente la solicitud de importación de datos, debería ver una entrada en la tabla de datos que se corresponde con el conjunto de datos que acaba de cargar.

En la siguiente tabla se muestran los estados de procesamiento de los conjuntos de datos importados:

| State | Significado |
| ----- | ------- |
| Processing | El conjunto de datos se ha recibido y se está procesando. |
| Correcto | El conjunto de datos se ha validado y se puede usar para compilar un modelo de voz. |
| Con error | El conjunto de datos ha dado error durante el procesamiento debido a muchas razones, por ejemplo, errores de archivo, problemas de datos o problemas de red. |

Una vez completada la validación, puede ver el número total de expresiones coincidentes para cada uno de los conjuntos de datos en la columna **Utterances** (Expresiones). Si el tipo de datos que ha seleccionado requiere una segmentación de audio de larga duración, esta columna solo refleja las expresiones que se han segmentado automáticamente en función de las transcripciones o mediante el servicio de transcripción de voz. Puede seguir descargando el conjunto de datos validado para ver los resultados detallados de las expresiones importadas correctamente y sus transcripciones de asignación. Sugerencia: la segmentación de audio de larga duración puede tardar más de una hora en completar el procesamiento de datos.

Para los conjuntos de datos de en-US y zh-CN, puede seguir descargando un informe para comprobar las puntuaciones de la pronunciación y el nivel de ruido de cada una de las grabaciones. La puntuación de las pronunciaciones abarca un rango del 0 al 100. Una puntuación por debajo de 70 normalmente indica un error en el discurso o que el guion no coincide. Un acento marcado puede reducir la puntuación de las pronunciaciones, y afectar a la voz digital que se ha creado.

Una relación de la señal y el ruido (SNR) más alta indica un ruido más bajo en el audio. Por lo general, el audio puede alcanzar una SNR de más de 50 puntos si se graba en estudios profesionales. Un audio que tenga un valor de SNR por debajo de 20 puntos puede provocar un ruido obvio en la voz generada.

Puede volver a grabar cualquier expresión que tenga una puntuación baja debido a la pronunciación o a la pobre relación entre el ruido y la señal. Si no es posible volver a realizar la grabación, puede excluir esas expresiones de su conjunto de datos.

## <a name="build-your-custom-voice-model"></a>Compilación del modelo de voz personalizada

Una vez que el conjunto de datos se haya validado, podrá usarlo para compilar su modelo de voz personalizado.

1.  Vaya a **Text-to-Speech > Custom Voice > Training** (Texto a voz > Voz personalizada > Entrenamiento).

2.  Haga clic en **Train model** (Entrenar modelo).

3.  A continuación, escriba un **nombre** y una **descripción** que le ayuden a identificar este modelo.

    Elija un nombre con cuidado. El nombre que escriba aquí será el nombre que use para especificar la voz en su solicitud de síntesis de voz como parte de la entrada de SSML. Solo se permiten letras, números y algunos signos de puntuación, como -, \_ y (","). Use nombres diferentes paras modelos de voz diferentes.

    Un uso habitual del campo **Descripción** es registrar los nombres de los conjuntos de datos que se usaron para crear el modelo.

4.  En la página **Select training data** (Seleccionar datos de entrenamiento), elija uno o varios conjuntos de datos que quiera usar para el entrenamiento. Compruebe el número de expresiones antes de enviarlas. Puede comenzar con cualquier número de expresiones para los modelos de voz de en-US y zh-CN. En el caso de otras configuraciones regionales, debe seleccionar más de 2000 expresiones para poder entrenar una voz.

    > [!NOTE]
    > Los nombres de audio duplicados se quitarán del entrenamiento. Asegúrese de que los conjuntos de datos que seleccione no contengan los mismos nombres de audio en varios archivos ZIP.

    > [!TIP]
    > Para obtener unos resultados de calidad, es necesario usar los conjuntos de datos del mismo altavoz. Cuando los conjuntos de datos que ha enviado para el entrenamiento contengan un número total de menos de 6000 expresiones distintas, entrenará el modelo de voz mediante la técnica de síntesis paramétrica estadística. En el caso de que los datos de entrenamiento excedan un número total de 6000 expresiones diferentes, se iniciará un proceso de entrenamiento con la técnica de síntesis de concatenación. Normalmente la tecnología de concatenación puede producir resultados más naturales y aumenta la fidelidad de la voz. [Póngase en contacto con el equipo de Voz personalizada](https://go.microsoft.com/fwlink/?linkid=2108737) si quiere entrenar un modelo con la tecnología TTS neuronal más reciente, la cual puede producir una voz digital equivalente a las [voces neuronales](language-support.md#neural-voices) disponibles públicamente.

5.  Haga clic en **Train** (Entrenar) para empezar a crear el modelo de voz.

En la tabla de entrenamiento se muestra una nueva entrada que corresponde a este modelo recién creado. En la tabla también se muestra el estado: Procesando, Correcto, Error.

El estado que se muestra refleja el proceso de convertir el conjunto de datos en un modelo de voz, como se muestra aquí.

| State | Significado |
| ----- | ------- |
| Processing | Se está creando el modelo de voz. |
| Correcto | El modelo de voz se ha creado y se puede implementar. |
| Con error | El modelo de voz ha dado error en el entrenamiento por muchas razones, por ejemplo, problemas desapercibidos con los datos o problemas de red. |

El tiempo de aprendizaje varía según el volumen de datos de audio procesados. El intervalo de tiempo típicos varía, aproximadamente, desde los 30 minutos para unos cientos de expresiones, hasta 40 horas para 20.000 expresiones. Cuando finalice correctamente el entrenamiento del modelo, puede empezar a probarlo.

> [!NOTE]
> Los usuarios con una suscripción gratuita (F0) pueden entrenar una fuente de voz al mismo tiempo. En cambio, los usuarios que tengan una suscripción estándar (S0) pueden entrenar tres voces simultáneamente. Si alcanza el límite, espere hasta que al menos una de las fuentes de voz finalice el aprendizaje e inténtelo de nuevo.

> [!NOTE]
> El número máximo de modelos de voz que se pueden entrenar por suscripción es de 10 modelos para los usuarios con una suscripción gratuita (F0) y de 100 para los usuarios con una suscripción estándar (S0).

Si usa la funcionalidad de entrenamiento de voz neuronal, puede optar por entrenar un modelo optimizado para escenarios de streaming en tiempo real o un modelo neuronal de alta definición optimizado para la [síntesis de audio de larga duración](long-audio-api.md) de manera asincrónica.  

## <a name="test-your-voice-model"></a>Prueba del modelo de voz

Una vez que la fuente de voz se haya creado correctamente, podrá probarla antes de implementarla para su uso.

1.  Vaya a **Text-to-Speech > Custom Voice > Testing** (Texto a voz > Voz personalizada > Prueba).

2.  Haga clic en **Add test** (Agregar prueba).

3.  Seleccione uno o varios modelos que le gustaría probar.

4.  Proporcione el texto que quiere que hable la voz o las voces. Si ha seleccionado probar varios modelos al mismo tiempo, se usará el mismo texto para las pruebas en distintos modelos.

    > [!NOTE]
    > Recuerde que el idioma del texto debe ser el mismo que el de la fuente de voz. Solo se pueden probar los modelos entrenados correctamente. En este paso, solo se admite texto sin formato.

5.  Haga clic en **Crear**.

Cuando haya enviado la solicitud de prueba, volverá a la página de prueba. La tabla ahora incluye una entrada que corresponde a su nueva solicitud y la columna de estado. Es posible que se tarden unos minutos en sintetizar la voz. Cuando la columna de estado muestre el valor **Succeeded** (Correcto), puede reproducir el audio, o descargar la entrada de texto (un archivo .txt) y la salida de audio (un archivo .wav) y escuchar esta última para comprobar la calidad.

También puede encontrar los resultados de la prueba en la página de detalles de cada modelo seleccionado para prueba. Vaya a la pestaña **Training** (Entrenamiento) y haga clic en el nombre del modelo para especificar su página de detalles.

## <a name="create-and-use-a-custom-voice-endpoint"></a>Creación y uso de un punto de conexión de voz personalizado

Una vez que haya creado y probado con éxito su modelo de voz, impleméntelo en un punto de conexión personalizado de Text to Speech. A continuación, use ese punto de conexión en lugar del punto de conexión habitual al realizar solicitudes de Text to Speech a través de la API de REST. Recuerde que solo se puede llamar al punto de conexión personalizado mediante la suscripción que utilizó para implementar la fuente.

Para crear un nuevo punto de conexión personalizado, vaya a **Text-to-Speech > Custom Voice > Deployment** (Texto a voz > Voz personalizada > Implementación). Seleccione **Add endpoint** (Agregar punto de conexión) y escriba un **nombre** y una **descripción** para el punto de conexión personalizado. A continuación, seleccione el modelo de voz personalizada que le gustaría asociar a este punto de conexión.

Después de haber hecho clic en el botón **Add** (Agregar), en la tabla de puntos de conexión, verá una entrada para el nuevo punto de conexión. El proceso para crear instancias del nuevo punto de conexión puede llevar unos minutos. Cuando el estado de la implementación muestra el valor **Completado**, quiere decir que el punto de conexión está listo para su uso.

> [!NOTE]
> Los usuarios con una suscripción gratuita (F0) solo pueden tener un modelo implementado. En cambio, los usuarios que tengan una suscripción estándar (S0) pueden crear hasta 50 puntos de conexión, cada uno de ellos con su propia voz personalizada.

> [!NOTE]
> Para usar su voz personalizada, debe especificar el nombre del modelo de voz, utilizar el URI personalizado directamente en una solicitud HTTP y emplear la misma suscripción para pasar por la autenticación del servicio TTS.

Una vez implementado el punto de conexión, su nombre aparece como un vínculo. Haga clic en el vínculo para mostrar información específica del punto de conexión, como la clave o la dirección URL, y código de ejemplo.

Las pruebas en línea del punto de conexión también están disponibles a través del portal de voz personalizada. Para probar el punto de conexión, elija **Check endpoint** (Comprobar el punto de conexión) en la página **Endpoint detail** (Detalle de punto de conexión). Aparecerá la página para probar los puntos de conexión. Escriba el texto que se hablará (en texto sin formato o [formato SSML](speech-synthesis-markup.md) en el cuadro de texto. Seleccione **Reproducir** para escuchar el texto que se habla en su fuente de voz personalizada. Esta característica de pruebas se les cobrará con el uso de síntesis de voz personalizada.

El punto de conexión personalizado es técnicamente idéntico al punto de conexión estándar que se usa en las solicitudes de texto a voz. Consulte la [API REST](rest-text-to-speech.md) para obtener más información.

## <a name="next-steps"></a>Pasos siguientes

* [Guía: Grabación de muestras de voz](record-custom-voice-samples.md)
* [Referencia de Text-to-Speech API](rest-text-to-speech.md)
* [Long Audio API](long-audio-api.md)
