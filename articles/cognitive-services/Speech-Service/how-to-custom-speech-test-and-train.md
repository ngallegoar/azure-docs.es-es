---
title: 'Preparación de los datos para Habla personalizada: servicio de Voz'
titleSuffix: Azure Cognitive Services
description: Al probar la precisión del reconocimiento de voz de Microsoft o entrenar sus modelos personalizados, necesitará datos de texto y de audio. En esta página, veremos los tipos de datos, cómo se usan y administran.
services: cognitive-services
author: trevorbye
manager: nitinme
ms.service: cognitive-services
ms.subservice: speech-service
ms.topic: conceptual
ms.date: 03/27/2020
ms.author: trbye
ms.openlocfilehash: f43f7894c46a75894eb648f02ec378f3a8b2633d
ms.sourcegitcommit: 829d951d5c90442a38012daaf77e86046018e5b9
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/09/2020
ms.locfileid: "84628049"
---
# <a name="prepare-data-for-custom-speech"></a>Preparación de los datos para Habla personalizada

Al probar la precisión del reconocimiento de voz de Microsoft o entrenar sus modelos personalizados, necesitará datos de texto y de audio. En esta página, tratamos los tipos de datos que necesita un modelo de voz personalizado.

## <a name="data-diversity"></a>Diversidad de datos

El texto y el audio usados para probar y entrenar un modelo personalizado deben incluir ejemplos de un conjunto diverso de oradores y escenarios que es preciso que el modelo reconozca.
Tenga en cuenta estos factores al recopilar datos para las pruebas y el entrenamiento de modelos personalizados:

* Los datos de texto y de audio de voz deben cubrir los tipos de instrucciones verbales que los usuarios realizarán al interactuar con el modelo. Por ejemplo, un modelo que eleva y reduce la temperatura necesita entrenamiento en las instrucciones que las personas pueden dar para solicitar tales cambios.
* Los datos deben incluir todas las variantes de la voz que el modelo tendrá que reconocer. Muchos factores pueden variar la voz, entre los que se incluyen los acentos, los dialectos, la combinación de idiomas, la edad, el género, el tono de voz, el nivel de estrés y la hora del día.
* Debe incluir ejemplos de diferentes entornos (en interior, en exteriores, con ruido de carretera) en los que se utilizará el modelo.
* El audio se debe recoger mediante dispositivos de hardware que utilizará el sistema de producción. Si el modelo necesita identificar la voz grabada en dispositivos de grabación de calidad variable, los datos de audio que proporcione para entrenar el modelo también deben representar los distintos escenarios.
* Puede agregar más datos al modelo más adelante, pero procure que el conjunto de datos sea diverso y representativo de las necesidades del proyecto.
* La inclusión de datos que *no* estén dentro de las necesidades de reconocimiento del modelo personalizado puede perjudicar la calidad general del reconocimiento, por lo que se recomienda no incluir datos que el modelo no necesite transcribir.

Un modelo entrenado en un subconjunto de escenarios solo puede funcionar correctamente en esos escenarios. Por consiguiente, es conveniente que elija minuciosamente los datos que representan el ámbito completo de los escenarios que necesita que el modelo personalizado reconozca.

> [!TIP]
> Comience con pequeños conjuntos de datos de ejemplo que coincidan con el idioma y la acústica con los que se encontrará el modelo.
> Por ejemplo, grabe un pequeño pero representativo ejemplo de audio en el mismo hardware y en el mismo entorno acústico que el modelo encontrará en escenarios de producción.
> Los pequeños conjuntos de datos representativos pueden sacar a la luz problemas antes de que haya invertido en la recopilación de un conjunto de datos mucho mayor para el entrenamiento.

## <a name="data-types"></a>Tipos de datos

En esta tabla se enumeran los tipos de datos aceptados, cuándo se debe utilizar cada tipo de datos y la cantidad recomendada. No todos los tipos de datos son necesarios para crear un modelo. Los requisitos de datos variarán dependiendo de si crea una prueba o entrena un modelo.

| Tipo de datos | Se usa para pruebas | Cantidad recomendada | Se utiliza para el entrenamiento | Cantidad recomendada |
|-----------|-----------------|----------|-------------------|----------|
| [Audio](#audio-data-for-testing) | Sí<br>Se utiliza para la inspección visual | Más de cinco archivos de audio | No | N/D |
| [Transcripciones de audio con etiqueta humana](#audio--human-labeled-transcript-data-for-testingtraining) | Sí<br>Se utiliza para evaluar la precisión | De 0,5 a 5 horas de audio | Sí | De 1 a 1000 horas de audio |
| [Texto relacionado](#related-text-data-for-training) | No | N/a | Sí | De 1 a 200 MB de texto relacionado |

Los archivos deben agruparse por tipo en un conjunto de datos y cargarse como archivo zip. Cada conjunto de datos solo puede contener un tipo de datos.

> [!TIP]
> Para empezar a trabajar rápidamente, considere la posibilidad de usar datos de ejemplo. Consulte este repositorio de GitHub para obtener <a href="https://github.com/Azure-Samples/cognitive-services-speech-sdk/tree/master/sampledata/customspeech" target="_target">datos de Habla personalizada de ejemplo <span class="docon docon-navigate-external x-hidden-focus"></span></a>

## <a name="upload-data"></a>Carga de datos

Para cargar los datos, navegue al portal de <a href="https://speech.microsoft.com/customspeech" target="_blank">Habla personalizada <span class="docon docon-navigate-external x-hidden-focus"></span></a>. En el portal, haga clic en **Cargar datos** para iniciar el asistente y crear el primer conjunto de datos. Se le pedirá que seleccione un tipo de datos de voz para el conjunto de datos, antes de permitirle cargar los datos.

![Selección del audio desde el portal de Voz](./media/custom-speech/custom-speech-select-audio.png)

Cada conjunto de datos que cargue debe cumplir los requisitos del tipo de datos elegido. Los datos deben tener el formato correcto para poder cargarse. Los datos con el formato correcto garantizan que el servicio Habla personalizada procesará los datos con precisión. Los requisitos se muestran en las secciones siguientes.

Una vez cargado el conjunto de datos, tiene algunas opciones:

* Puede dirigirse a la pestaña **Testing** (Pruebas) e inspeccionar visualmente datos de transcripción solo de audio o de audio y con etiqueta humana.
* Puede dirigirse a la pestaña **Training** (Entrenamiento) y utilizar datos de transcripción de audio y humana o datos de texto relacionados para entrenar un modelo personalizado.

## <a name="audio-data-for-testing"></a>Datos de audio para pruebas

Los datos de audio son óptimos para probar la precisión del modelo de línea de base de Microsoft de voz a texto o de un modelo personalizado. Tenga en cuenta que los datos de audio se utilizan para inspeccionar la precisión de la voz con respecto al rendimiento de un modelo específico. Si busca cuantificar la precisión de un modelo, utilice los [datos de transcripción de audio y con etiqueta humana](#audio--human-labeled-transcript-data-for-testingtraining).

Utilice esta tabla para asegurarse de que los archivos de audio están formateados correctamente para su uso con Habla personalizada:

| Propiedad                 | Value                 |
|--------------------------|-----------------------|
| Formato de archivo              | RIFF (WAV)            |
| Velocidad de muestreo              | 8000 o 16 000 Hz |
| Canales                 | 1 (mono)              |
| Longitud máxima por audio | 2 horas               |
| Formato de ejemplo            | PCM, 16 bits           |
| Formato de archivo           | .zip                  |
| Tamaño de archivo máximo     | 2 GB                  |

[!INCLUDE [supported-audio-formats](includes/supported-audio-formats.md)]

> [!TIP]
> Al cargar los datos del entrenamiento y de las pruebas, el archivo ZIP no puede superar los 2 GB. Si requiere más datos para el entrenamiento, divídalos en varios archivos ZIP y cárguelos por separado. Más adelante, puede optar por entrenar en *varios* conjuntos de datos. Aunque solo puede realizar pruebas desde un *único* conjunto de datos.

Use <a href="http://sox.sourceforge.net" target="_blank" rel="noopener">SoX <span class="docon docon-navigate-external x-hidden-focus"></span></a> para comprobar las propiedades de audio o convertir el audio existente en los formatos adecuados. A continuación se muestran algunos ejemplos de cómo se puede realizar cada una de estas actividades mediante la línea de comandos de SoX:

| Actividad | Descripción | Comando de SoX |
|----------|-------------|-------------|
| Comprobar el formato de audio | Use este comando para comprobar<br>el formato de archivo de audio. | `sox --i <filename>` |
| Convertir formato de audio | Use este comando para convertir<br>el archivo de audio en un canal único, de 16 bits y 16 KHz. | `sox <input> -b 16 -e signed-integer -c 1 -r 16k -t wav <output>.wav` |

## <a name="audio--human-labeled-transcript-data-for-testingtraining"></a>Datos de transcripción de audio y con etiqueta humanos para pruebas y entrenamiento

Para medir la precisión de la precisión de la conversión de voz a texto de Microsoft al procesar sus archivos de audio, debe proporcionar transcripciones con etiqueta humana (palabra por palabra) para su comparación. Aunque la transcripción con etiqueta humana a menudo requiere mucho tiempo, es necesario evaluar la precisión y entrenar al modelo para los casos de uso. Tenga en cuenta que las mejoras en el reconocimiento solo serán tan buenas como los datos proporcionados. Por ese motivo, es importante que solo se carguen las transcripciones de alta calidad.

Los archivos de audio pueden tener silencio al principio y al final de la grabación. Si es posible, incluya al menos medio segundo de silencio antes y después de la voz en cada archivo de ejemplo. Aunque el audio con un volumen de grabación bajo o con ruido de fondo molesto no resulta útil, no debería perjudicar al modelo personalizado. Considere siempre la posibilidad de actualizar los micrófonos y el hardware de procesamiento de la señal antes de recopilar muestras de audio.

| Propiedad                 | Value                               |
|--------------------------|-------------------------------------|
| Formato de archivo              | RIFF (WAV)                          |
| Velocidad de muestreo              | 8000 o 16 000 Hz               |
| Canales                 | 1 (mono)                            |
| Longitud máxima por audio | 2 horas (pruebas)/60 s (entrenamiento) |
| Formato de ejemplo            | PCM, 16 bits                         |
| Formato de archivo           | .zip                                |
| Tamaño máximo de archivo zip         | 2 GB                                |

[!INCLUDE [supported-audio-formats](includes/supported-audio-formats.md)]

> [!NOTE]
> Al cargar los datos del entrenamiento y de las pruebas, el archivo ZIP no puede superar los 2 GB. Solo puede realizar pruebas desde un *único* conjunto de datos; asegúrese de mantenerlo dentro del tamaño de archivo adecuado. Además, cada archivo de entrenamiento no puede superar los 60 segundos, ya que, si lo hace, se producirá un error.

Para abordar problemas como la eliminación o sustitución de palabras, se necesita una cantidad significativa de datos para mejorar el reconocimiento. Por lo general, se recomienda proporcionar transcripciones palabra por palabra para aproximadamente de 10 a 1000 horas de audio. Las transcripciones para todos los archivos WAV deben incluirse en un único archivo de texto sin formato. Cada línea del archivo de transcripción debe contener el nombre de uno de los archivos de audio, seguido de la transcripción correspondiente. El nombre de archivo y la transcripción deben estar separados por un carácter de tabulación (\t).

  Por ejemplo:
```
  speech01.wav  speech recognition is awesome
  speech02.wav  the quick brown fox jumped all over the place
  speech03.wav  the lazy dog was not amused
```

> [!IMPORTANT]
> La transcripción debería estar codificada como una marca BOM UTF-8.

Las transcripciones son texto normalizado para que el sistema pueda procesarlas. Aunque hay algunas normalizaciones importantes que debe hacer el usuario antes de cargar los datos en Speech Studio. Para conocer el idioma que debe usar al preparar las transcripciones, consulte [How to create a human-labeled transcription](how-to-custom-speech-human-labeled-transcriptions.md) (Creación de una transcripción con etiqueta humana).

Cuando haya reunido los archivos de audio y las transcripciones correspondientes, debe empaquetarlos como un solo archivo .zip antes de cargarlos en el <a href="https://speech.microsoft.com/customspeech" target="_blank">portal de Habla personalizada<span class="docon docon-navigate-external x-hidden-focus"></span></a>. A continuación se muestra un conjunto de datos de ejemplo con tres archivos de audio y un archivo de transcripción con etiqueta humana:

> [!div class="mx-imgBorder"]
> ![Selección del audio desde el portal de Voz](./media/custom-speech/custom-speech-audio-transcript-pairs.png)

## <a name="related-text-data-for-training"></a>Datos de texto relacionados para el entrenamiento

Los nombres de producto o las características que son únicas deben incluir datos de texto relacionados para el entrenamiento. El texto relacionado ayuda a garantizar el reconocimiento correcto. Se pueden proporcionar dos tipos de datos de texto relacionados para mejorar el reconocimiento:

| Tipo de datos | Mejora del reconocimiento por estos datos |
|-----------|------------------------------------|
| Frases (expresiones) | Mejore la precisión al reconocer nombres de productos o vocabulario específico del sector dentro del contexto de una frase. |
| Pronunciaciones | Mejore la pronunciación de términos poco comunes, acrónimos u otras palabras de pronunciación indefinida. |

Las frases se pueden proporcionar como un solo archivo de texto o como varios archivos de texto. Para mejorar la precisión, use datos de texto que estén más cerca de las expresiones orales esperadas. Las pronunciaciones deben proporcionarse como un único archivo de texto. Todo se puede empaquetar como un único archivo zip y cargarse en el <a href="https://speech.microsoft.com/customspeech" target="_blank">portal de Habla personalizada<span class="docon docon-navigate-external x-hidden-focus"></span></a>.

### <a name="guidelines-to-create-a-sentences-file"></a>Directrices para crear un archivo de frases

Para crear un modelo personalizado con frases, tendrá que proporcionar una lista de expresiones de ejemplo. Las expresiones _no_ tienen que ser frases completas o gramaticalmente correctas, pero deben reflejar con precisión la entrada oral que se espera en producción. Si quiere que ciertos términos tengan mayor peso, puede agregar varias frases que incluyan estos términos específicos.

Como guía general, la adaptación de modelos es más eficaz cuando el texto de entrenamiento es lo más parecido posible al texto real esperado en producción. La jerga y las frases específicas del dominio que va a mejorar deben incluirse en el texto de entrenamiento. Cuando sea posible, intente que una frase o una palabra clave se controle en una línea independiente. Para las palabras clave y frases que son importantes para usted (por ejemplo, nombres de producto), puede copiarlas varias veces. Pero tenga en cuenta que no debe copiar demasiado; podría afectar a la tasa de reconocimiento general.

Utilice esta tabla para asegurarse de que el archivo de datos relacionado con las expresiones esté formateado correctamente:

| Propiedad | Value |
|----------|-------|
| Codificación de texto | BOM UTF-8 |
| Número de expresiones por línea | 1 |
| Tamaño de archivo máximo | 200 MB |

Además, querrá tener en cuenta las siguientes restricciones:

* Evite repetir los caracteres más de cuatro veces. Por ejemplo: "aaaa" o "uuuu".
* No utilice caracteres especiales ni caracteres UTF-8 por encima de `U+00A1`.
* Se rechazarán los identificadores URI.

### <a name="guidelines-to-create-a-pronunciation-file"></a>Directrices para crear un archivo de pronunciación

Si hay términos poco comunes sin pronunciaciones estándar que los usuarios van a encontrar o utilizar, puede proporcionar un archivo de pronunciación personalizado para mejorar el reconocimiento.

> [!IMPORTANT]
> No se recomienda utilizar archivos de pronunciación personalizados para modificar la pronunciación de palabras comunes.

Esto incluye ejemplos de una expresión hablada y una pronunciación personalizada para cada uno:

| Formulario reconocido o mostrado | Formato hablado |
|--------------|--------------------------|
| 3CPO | CI-TRI-PI-OU |
| CNTK | CI EN TI KEI |
| IEEE | AI TRIPLE I |

La forma hablada es la secuencia fonética deletreada. Puede estar compuesta de letras, palabras, sílabas o una combinación de las tres.

La pronunciación personalizada está disponible en inglés (`en-US`) y en alemán (`de-DE`). En esta tabla se muestran los caracteres admitidos por idioma:

| Idioma | Configuración regional | Characters |
|----------|--------|------------|
| Inglés | `en-US` | `a, b, c, d, e, f, g, h, i, j, k, l, m, n, o, p, q, r, s, t, u, v, w, x, y, z` |
| Alemán | `de-DE` | `ä, ö, ü, a, b, c, d, e, f, g, h, i, j, k, l, m, n, o, p, q, r, s, t, u, v, w, x, y, z` |

Utilice la tabla siguiente para asegurarse de que el formato del archivo de datos relacionado con las pronunciaciones sea correcto. Los archivos de pronunciación son pequeños y no deberían superar unos pocos KB.

| Propiedad | Value |
|----------|-------|
| Codificación de texto | BOM UTF-8 (también se admite ANSI con el inglés) |
| Número de pronunciaciones por línea | 1 |
| Tamaño de archivo máximo | 1 MB (1 KB por cada nivel gratis) |

## <a name="next-steps"></a>Pasos siguientes

* [Inspección de los datos](how-to-custom-speech-inspect-data.md)
* [Evaluación de los datos](how-to-custom-speech-evaluate-data.md)
* [Entrenamiento del modelo](how-to-custom-speech-train-model.md)
* [Implementación del modelo](how-to-custom-speech-deploy-model.md)
