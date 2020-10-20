---
author: v-jaswel
ms.service: cognitive-services
ms.topic: include
ms.date: 09/28/2020
ms.author: v-jawe
ms.custom: references_regions
ms.openlocfilehash: dd23bf0528a27f599058271decbf1820084c9a43
ms.sourcegitcommit: fbb620e0c47f49a8cf0a568ba704edefd0e30f81
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/09/2020
ms.locfileid: "91875472"
---
En este inicio rápido, aprenderá los patrones de diseño básicos de Speaker Recognition mediante el SDK de Voz, que incluyen:

* Comprobación dependiente e independiente del texto.
* Identificación del hablante para identificar una muestra de voz entre un grupo de voces.
* Eliminación de perfiles de voz.

Para obtener una visión general de los conceptos de reconocimiento de voz, consulte el artículo de [información general](../../../speaker-recognition-overview.md).

## <a name="skip-to-samples-on-github"></a>Pasar a los ejemplos en GitHub

Si quiere pasar directamente al código de ejemplo, consulte los [ejemplos del inicio rápido de C++](https://github.com/Azure-Samples/cognitive-services-speech-sdk/tree/master/quickstart/cpp/windows) en GitHub.

## <a name="prerequisites"></a>Requisitos previos

En este artículo se da por sentado que tiene una cuenta de Azure y una suscripción al servicio de voz. Si no dispone de una cuenta y una suscripción, [pruebe el servicio de voz de forma gratuita](../../../overview.md#try-the-speech-service-for-free).

> [!IMPORTANT]
> Speaker Recognition *solo* se admite actualmente en los recursos de Voz de Azure creados en la región de `westus`.

## <a name="install-the-speech-sdk"></a>Instalación de Speech SDK

En primer lugar, deberá instalar Speech SDK. Utilice las siguientes instrucciones en función de la plataforma:

* <a href="https://docs.microsoft.com/azure/cognitive-services/speech-service/quickstarts/setup-platform?tabs=linux&pivots=programming-language-cpp" target="_blank">Linux <span class="docon docon-navigate-external x-hidden-focus"></span></a>
* <a href="https://docs.microsoft.com/azure/cognitive-services/speech-service/quickstarts/setup-platform?tabs=macos&pivots=programming-language-cpp" target="_blank">macOS <span class="docon docon-navigate-external x-hidden-focus"></span></a>
* <a href="https://docs.microsoft.com/azure/cognitive-services/speech-service/quickstarts/setup-platform?tabs=windows&pivots=programming-language-cpp" target="_blank">Windows <span class="docon docon-navigate-external x-hidden-focus"></span></a>

## <a name="import-dependencies"></a>Dependencias de importación

Para ejecutar los ejemplos de este artículo, agregue las siguientes instrucciones al principio del archivo .cpp.

:::code language="cpp" source="~/cognitive-services-quickstart-code/cpp/speech/speaker-recognition.cpp" id="dependencies":::

## <a name="create-a-speech-configuration"></a>Creación de una configuración de voz

Para llamar al servicio de voz con Speech SDK, debe crear un elemento [`SpeechConfig`](https://docs.microsoft.com/cpp/cognitive-services/speech/speechconfig). Esta clase incluye información sobre la suscripción, como la clave, la región asociada, el punto de conexión, el host o el token de autorización.

:::code language="cpp" source="~/cognitive-services-quickstart-code/cpp/speech/speaker-recognition.cpp" id="get_speech_config":::

## <a name="text-dependent-verification"></a>Comprobación dependiente del texto

Speaker Verification es el acto de confirmar que un hablante coincide con una voz conocida o **inscrita**. El primer paso es **inscribir** un perfil de voz, de modo que el servicio tenga algo para comparar futuras muestras de voz. En este ejemplo, inscribirá el perfil mediante una estrategia **dependiente del texto**, que requiere que se use una frase de contraseña específica para la inscripción y la comprobación. Consulte los [documentos de referencia](https://docs.microsoft.com/rest/api/speakerrecognition/) para ver una lista de frases de contraseña admitidas.

### <a name="textdependentverification-function"></a>Función TextDependentVerification

Empiece por crear la función `TextDependentVerification`.

:::code language="cpp" source="~/cognitive-services-quickstart-code/cpp/speech/speaker-recognition.cpp" id="text_dependent_verification":::

Esta función crea un objeto [VoiceProfile](https://docs.microsoft.com/cpp/cognitive-services/speech/voiceprofile) con el método [CreateProfileAsync](https://docs.microsoft.com/cpp/cognitive-services/speech/voiceprofileclient#createprofileasync). Observe que hay tres [tipos](https://docs.microsoft.com/cpp/cognitive-services/speech/microsoft-cognitiveservices-speech-namespace#enum-voiceprofiletype) de `VoiceProfile`:

- TextIndependentIdentification
- TextDependentVerification
- TextIndependentVerification

En este caso, pasará `VoiceProfileType::TextDependentVerification` a `CreateProfileAsync`.

Luego, llamará a dos funciones auxiliares que definirá a continuación: `AddEnrollmentsToTextDependentProfile` y `SpeakerVerify`. Por último, llame a [DeleteProfileAsync](https://docs.microsoft.com/cpp/cognitive-services/speech/voiceprofileclient#deleteprofileasync) para limpiar el perfil.

### <a name="addenrollmentstotextdependentprofile-function"></a>Función AddEnrollmentsToTextDependentProfile

Defina la función siguiente para inscribir un perfil de voz.

:::code language="cpp" source="~/cognitive-services-quickstart-code/cpp/speech/speaker-recognition.cpp" id="add_enrollments_dependent":::

Luego, inscribirá muestras de audio en un bucle `while` que realiza un seguimiento del número de muestras restantes y necesarias para la inscripción. En cada iteración, [EnrollProfileAsync](https://docs.microsoft.com/cpp/cognitive-services/speech/voiceprofileclient#enrollprofileasync) le pide que diga la frase de contraseña por el micrófono, y la muestra se agrega al perfil de voz.

### <a name="speakerverify-function"></a>Función SpeakerVerify

Defina `SpeakerVerify` como se indica a continuación.

:::code language="cpp" source="~/cognitive-services-quickstart-code/cpp/speech/speaker-recognition.cpp" id="speaker_verify":::

En esta función, creará un objeto [SpeakerVerificationModel](https://docs.microsoft.com/cpp/cognitive-services/speech/speakerverificationmodel) con el método [SpeakerVerificationModel::FromProfile](https://docs.microsoft.com/cpp/cognitive-services/speech/speakerverificationmodel#fromprofile) y pasará el objeto [VoiceProfile](https://docs.microsoft.com/cpp/cognitive-services/speech/voiceprofile) que creó anteriormente.

Luego, [SpeechRecognizer::RecognizeOnceAsync](https://docs.microsoft.com/cpp/cognitive-services/speech/speechrecognizer#recognizeonceasync) le pide que diga de nuevo la frase de contraseña, pero esta vez la validará con el perfil de voz y devolverá una puntuación de similitud comprendida entre 0,0 y 1,0. El objeto [SpeakerRecognitionResult](https://docs.microsoft.com/cpp/cognitive-services/speech/speakerrecognitionresult) también devuelve `Accept` o `Reject`, según coincida o no la frase de contraseña.

## <a name="text-independent-verification"></a>Comprobación independiente del texto

A diferencia de la comprobación **dependiente del texto**, la comprobación **independiente del texto**:

* No precisa que se diga una frase de contraseña determinada; se puede decir cualquier cosa.
* No precisa tres muestras de audio, pero *sí* se necesitan 20 segundos de audio total.

### <a name="textindependentverification-function"></a>Función TextIndependentVerification

Empiece por crear la función `TextIndependentVerification`.

:::code language="cpp" source="~/cognitive-services-quickstart-code/cpp/speech/speaker-recognition.cpp" id="text_independent_verification":::

Al igual que la función `TextDependentVerification`, esta función crea un objeto [VoiceProfile](https://docs.microsoft.com/cpp/cognitive-services/speech/voiceprofile) con el método [CreateProfileAsync](https://docs.microsoft.com/cpp/cognitive-services/speech/voiceprofileclient#createprofileasync).

En este caso, pasará `VoiceProfileType::TextIndependentVerification` a `CreateProfileAsync`.

Luego, llamará a dos funciones auxiliares: `AddEnrollmentsToTextIndependentProfile`, que definirá a continuación y `SpeakerVerify`, que ya ha definido. Por último, llame a [DeleteProfileAsync](https://docs.microsoft.com/cpp/cognitive-services/speech/voiceprofileclient#deleteprofileasync) para limpiar el perfil.

### <a name="addenrollmentstotextindependentprofile"></a>AddEnrollmentsToTextIndependentProfile

Defina la función siguiente para inscribir un perfil de voz.

:::code language="cpp" source="~/cognitive-services-quickstart-code/cpp/speech/speaker-recognition.cpp" id="add_enrollments_independent":::

En esta función, inscribirá las muestras de audio en un bucle `while` que realiza un seguimiento del número de segundos de audio restantes y necesarios para la inscripción. En cada iteración, [EnrollProfileAsync](https://docs.microsoft.com/cpp/cognitive-services/speech/voiceprofileclient#enrollprofileasync) le pide que hable por el micrófono, y la muestra se agrega al perfil de voz.

## <a name="speaker-identification"></a>Identificación del hablante

Speaker Identification se utiliza para determinar **quién** está hablando de un grupo determinado de voces inscritas. El proceso es muy similar al de **comprobación independiente del texto**. La diferencia principal es que se puede realizar la comprobación con varios perfiles de voz a la vez, en lugar de con uno solo.

### <a name="textindependentidentification-function"></a>Función TextIndependentIdentification

Empiece por crear la función `TextIndependentIdentification`.

:::code language="cpp" source="~/cognitive-services-quickstart-code/cpp/speech/speaker-recognition.cpp" id="text_independent_indentification":::

Al igual que las funciones `TextDependentVerification` y `TextIndependentVerification`, esta función crea un objeto [VoiceProfile](https://docs.microsoft.com/cpp/cognitive-services/speech/voiceprofile) con el método [CreateProfileAsync](https://docs.microsoft.com/cpp/cognitive-services/speech/voiceprofileclient#createprofileasync).

En este caso, pasará `VoiceProfileType::TextIndependentIdentification` a `CreateProfileAsync`.

Luego, llamará a dos funciones auxiliares: `AddEnrollmentsToTextIndependentProfile`, que ya ha definido y `SpeakerIdentify`, que definirá a continuación. Por último, llame a [DeleteProfileAsync](https://docs.microsoft.com/cpp/cognitive-services/speech/voiceprofileclient#deleteprofileasync) para limpiar el perfil.

### <a name="speakeridentify-function"></a>Función SpeakerIdentify

defina la función `SpeakerIdentify` de la siguiente manera.

:::code language="cpp" source="~/cognitive-services-quickstart-code/cpp/speech/speaker-recognition.cpp" id="speaker_identify":::

En esta función, creará un objeto [SpeakerIdentificationModel](https://docs.microsoft.com/cpp/cognitive-services/speech/speakeridentificationmodel) con el método [SpeakerIdentificationModel:: FromProfiles](https://docs.microsoft.com/cpp/cognitive-services/speech/speakeridentificationmodel#fromprofiles). `SpeakerIdentificationModel::FromProfiles` acepta una lista de objetos [VoiceProfile](https://docs.microsoft.com/cpp/cognitive-services/speech/voiceprofile). En este caso, simplemente pasará el objeto `VoiceProfile` que creó anteriormente. Sin embargo, si lo desea, puede pasar varios objetos `VoiceProfile`, cada uno inscrito con muestras de audio de una voz diferente.

Luego, [SpeechRecognizer::RecognizeOnceAsync](https://docs.microsoft.com/cpp/cognitive-services/speech/speechrecognizer#recognizeonceasync) le pide que hable de nuevo. Esta vez se compara su voz con los perfiles de voz inscritos y se devuelve el perfil de voz más parecido.

## <a name="main-function"></a>Función main

Por último, defina la función `main` como de la siguiente manera.

:::code language="cpp" source="~/cognitive-services-quickstart-code/cpp/speech/speaker-recognition.cpp" id="main":::

Esta función simplemente llama a las funciones que definió anteriormente. Sin embargo, primero crea un objeto [VoiceProfileClient](https://docs.microsoft.com/cpp/cognitive-services/speech/voiceprofileclient) y un objeto [SpeakerRecognizer](https://docs.microsoft.com/cpp/cognitive-services/speech/speakerrecognizer).

```
auto speech_config = GetSpeechConfig();
auto client = VoiceProfileClient::FromConfig(speech_config);
auto recognizer = SpeakerRecognizer::FromConfig(speech_config, audio_config);
```

`VoiceProfileClient` se usa para crear, inscribir y eliminar perfiles de voz. `SpeakerRecognizer` se utiliza para validar muestras de voz con uno o varios perfiles de voz inscritos.

## <a name="changing-audio-input-type"></a>Cambio del tipo de entrada de audio

En los ejemplos de este artículo se usa el micrófono del dispositivo predeterminado como entrada para las muestras de audio. Sin embargo, en escenarios en los que necesite usar archivos de audio en lugar de la entrada de micrófono, solo tiene que cambiar la línea siguiente:

```
auto audio_config = Audio::AudioConfig::FromDefaultMicrophoneInput();
```

a:

```
auto audio_config = Audio::AudioConfig::FromWavFileInput(path/to/your/file.wav);
```

También puede reemplazar el uso de `audio_config` por [Audio::AudioConfig::FromWavFileInput](https://docs.microsoft.com/cpp/cognitive-services/speech/audio-audioconfig#fromwavfileinput). También puede tener entradas mixtas, si usa un micrófono para la inscripción y archivos para la comprobación, por ejemplo.
