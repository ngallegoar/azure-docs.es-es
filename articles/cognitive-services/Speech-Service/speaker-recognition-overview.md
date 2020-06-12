---
title: Servicio Azure Speaker Recognition
titleSuffix: Azure Cognitive Services
description: Speaker Recognition de Azure Cognitive Services proporciona algoritmos que comprueban e identifican a los hablantes por sus características de voz únicas. Speaker Recognition se usa para responder a la pregunta "¿quién está hablando?".
services: cognitive-services
author: trevorbye
manager: nitinme
ms.service: cognitive-services
ms.subservice: speech-service
ms.topic: conceptual
ms.date: 05/27/2020
ms.author: trbye
ms.openlocfilehash: 2d4ce6f274efbd4d8afe2ac48856b0fc312f0a09
ms.sourcegitcommit: 223cea58a527270fe60f5e2235f4146aea27af32
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 06/01/2020
ms.locfileid: "84261772"
---
# <a name="what-is-the-azure-speaker-recognition-service"></a>¿Qué es el servicio Azure Speaker Recognition?

El servicio Speaker Recognition proporciona algoritmos que comprueban e identifican a los hablantes por sus características de voz únicas. Speaker Recognition se usa para responder a la pregunta "¿quién está hablando?". Proporciona datos de entrenamiento de audio para un solo hablante, que crea un perfil de inscripción basado en las características únicas de la voz del hablante. Después, puede realizar una comprobación cruzada de los ejemplos de voz de audio con respecto a este perfil para comprobar que el hablante es la misma persona (verificación del hablante) o muestras de voz de audio de una consulta cruzada en un *grupo* de perfiles de altavoz inscritos, para ver si coincide con algún perfil del grupo (identificación del hablante). Por el contrario, [Speaker Diarization](batch-transcription.md#speaker-separation-diarization) agrupa segmentos de audio por hablante en una operación por lotes.

## <a name="speaker-verification"></a>Verificación del hablante

Speaker Verification simplifica el proceso de verificación de la identidad de un hablante inscrito con frases de contraseña o con una entrada de voz de forma libre. Se puede usar para comprobar que los usuarios tienen compromisos de cliente seguros y sin problemas en una amplia gama de soluciones, desde la verificación de la identidad del cliente en los centros de llamadas hasta el acceso a la instalación sin conexión.

### <a name="how-does-speaker-verification-work"></a>¿Cómo funciona Speaker Verification?

![Cómo funciona la verificación del hablante](media/speaker-recognition/speaker-rec.png)

La verificación del hablante puede ser dependiente del texto o independiente del texto. En la verificación **dependiente del texto**, los hablantes deben elegir la misma frase de contraseña para usarla durante las fases de inscripción y comprobación. En la verificación **independiente del texto**, los hablantes pueden hablar en el lenguaje cotidiano en las frases de inscripción y comprobación.

En la **verificación dependiente del texto**, la voz del hablante se inscribe indicando una frase de contraseña de un conjunto de frases predefinidas. Las características de voz se extraen de la grabación de audio para formar una firma de voz única, mientras que también se reconoce la frase de contraseña seleccionada. Juntos, la firma de voz y la frase de contraseña se usan para verificar el hablante. 

La **verificación independiente del texto** no tiene restricciones en lo que el hablante indica durante la inscripción o en la muestra de audio que se va a verificar, ya que solo extrae características de voz para puntuar la similitud. 

Las API no están pensadas para determinar si el audio proviene de una persona de carne y hueso, una imitación o una grabación de un hablante inscrito. 

## <a name="speaker-identification"></a>Identificación del hablante

Speaker Identification se usa para determinar la identidad de un hablante desconocido dentro de un grupo de hablantes inscritos. Speaker Identification permite atribuir voz a hablantes individuales y aprovechar el valor de escenarios con varios hablantes, como:

* Soluciones de soporte técnico para la productividad en reuniones remotas 
* Desarrollo de personalización de dispositivos multiusuario

### <a name="how-does-speaker-identification-work"></a>¿Cómo funciona Speaker Identification?

La inscripción para la identificación del hablante **no depende del texto**, lo que significa que no hay restricciones con respecto a lo que el hablante dice en el audio. De forma similar a Speaker Verification, en la fase de inscripción, se graba la voz del hablante y se extraen las características de voz para formar una firma de voz única. En la fase de identificación, la muestra de voz de entrada se compara con una lista especificada de voces inscritas (hasta 50 en cada solicitud).

## <a name="data-security-and-privacy"></a>Privacidad y seguridad de los datos

Los datos de inscripción de hablantes se almacenan en un sistema protegido, incluido el audio de voz para las características de inscripción y firma de voz. El audio de voz para la inscripción solo se utiliza cuando se actualiza el algoritmo y las características deben volver a extraerse. El servicio no conserva la grabación de voz ni las características de voz extraídas que se le envían durante la fase de reconocimiento. 

Puede controlar cuánto tiempo se deben conservar los datos. Los clientes pueden crear, actualizar y quitar los datos de inscripción de un hablante mediante las llamadas API. Cuando se elimine la suscripción, todos los datos de inscripción del hablante asociados a la suscripción también se eliminarán. 

Al igual que sucede con todos los recursos de Cognitive Services, los desarrolladores que usan el servicio Speaker Recognition deben estar al tanto de las directivas de Microsoft sobre los datos de los clientes. Debe asegurarse de haber recibido los permisos adecuados de los usuarios para Speaker Recognition. Para obtener más información, vea la  [página de Cognitive Services](https://azure.microsoft.com/support/legal/cognitive-services-compliance-and-privacy/)  en Microsoft Trust Center. 

## <a name="next-steps"></a>Pasos siguientes

> [!div class="nextstepaction"]
> * Consulte el [tutorial de vídeo](https://azure.microsoft.com/resources/videos/speaker-recognition-text-independent-verification-developer-tutorial/) para la verificación del hablante independiente del texto.
