---
title: ¿Qué es Speaker Recognition API?
titleSuffix: Azure Cognitive Services
description: Verificación e identificación del hablante con Speaker Recognition API en Cognitive Services.
services: cognitive-services
author: dwlin
manager: nitinme
ms.service: cognitive-services
ms.subservice: speaker-recognition
ms.topic: overview
ms.date: 10/01/2018
ms.author: nitinme
ROBOTS: NOINDEX
ms.openlocfilehash: f354793e3779f4d9f7be9bae8a21545a15ed1c4c
ms.sourcegitcommit: 34a6fa5fc66b1cfdfbf8178ef5cdb151c97c721c
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 04/28/2020
ms.locfileid: "73464920"
---
# <a name="speaker-recognition-api---preview"></a>Speaker Recognition API (versión preliminar)

Las Speaker Recognition API son API basadas en la nube que proporcionan los algoritmos de inteligencia artificial más avanzados para la verificación y la identificación del hablante. Speaker Recognition se divide en dos categorías: verificación del hablante e identificación del hablante.

## <a name="speaker-verification"></a>Verificación del hablante

La voz tiene características únicas que se pueden asociar con una persona.  Las aplicaciones pueden usar la voz como factor de verificación adicional en escenarios como centros de llamadas y servicios web.

Las Speaker Verification API sirven como herramienta inteligente para ayudar en la verificación de los usuarios tanto con su voz como con frases de contraseña habladas.

### <a name="enrollment"></a>Inscripción

La inscripción para la verificación del hablante depende del texto, lo que significa que los hablantes deben elegir una frase de contraseña específica que usarán durante las fases de inscripción y verificación.

En la fase de inscripción, se graba la voz del hablante con una frase determinada. Se extraen las características de voz para formar una firma de voz única mientras se reconoce la frase elegida. Estos datos de inscripción del hablante se usan juntos para verificar su identidad. Los datos de inscripción del hablante se almacenan en un sistema protegido. El cliente controla cuánto tiempo se deben conservar. Los clientes pueden crear, actualizar y quitar los datos de inscripción de un hablante mediante las llamadas API.  Cuando se elimine la suscripción, todos los datos de inscripción del hablante asociados a la suscripción también se eliminarán.

Los clientes deben asegurarse de haber recibido los permisos adecuados de los usuarios para la verificación del hablante.

### <a name="verification"></a>Comprobación

En la fase de verificación, el cliente debe llamar a la API de verificación del hablante con el identificador asociado al individuo que se va a verificar.  El servicio extrae las características de voz y la frase de contraseña de la grabación de voz de entrada. Luego, compara las características con los elementos correspondientes de los datos de inscripción del hablante que quiere verificar el cliente y determina las posibles coincidencias.  La respuesta devuelve "aceptar" o "rechazar" con distintos niveles de confianza.  Después, el cliente determina cómo usar los resultados para decidir si esta persona es el hablante inscrito.

El umbral de nivel de confianza debe establecerse según el escenario y otros factores de verificación que use. Se recomienda experimentar con el nivel de confianza y tener en cuenta la configuración adecuada para cada aplicación. Las API no están pensadas para determinar si el audio proviene de una persona de carne y hueso, una imitación o una grabación de un hablante inscrito.

El servicio no conserva la grabación de voz ni las características de voz extraídas que se le envían durante la fase de verificación.

Para obtener más información sobre la verificación del hablante, consulte la API [Speaker - Verification](https://westus.dev.cognitive.microsoft.com/docs/services/563309b6778daf02acc0a508/operations/563309b7778daf06340c9652).

## <a name="speaker-identification"></a>Identificación del hablante

Las aplicaciones pueden usar la voz para identificar "quién habla" dado un grupo de hablantes inscritos. Las Speaker Identification API se pueden usar en escenarios como productividad de reuniones, personalización y transcripción del centro de llamadas.

### <a name="enrollment"></a>Inscripción

La inscripción para la identificación del hablante depende del texto, lo que significa que no hay restricciones con respecto a lo que el hablante dice en el audio. No se requiere ninguna frase de contraseña.

En la fase de inscripción, se graba la voz del hablante y se extraen las características de voz para formar una firma de voz única. Las características y el audio de voz extraídos se almacenan en un sistema protegido. El cliente controla cuánto tiempo se deben conservar. Los clientes pueden crear, actualizar y quitar estos datos de inscripción de un hablante mediante las llamadas API. Cuando se elimine la suscripción, todos los datos de inscripción del hablante asociados a la suscripción también se eliminarán.

Los clientes deben asegurarse de haber recibido los permisos adecuados de los usuarios para la identificación del hablante.

### <a name="identification"></a>Identificación

En la fase de identificación, el servicio de identificación del hablante extrae las características de voz de la grabación de voz de entrada. Luego, las compara con los datos de inscripción de la lista de hablantes especificada. Cuando encuentra una coincidencia con un hablante inscrito, la respuesta devuelve el identificador del hablante con un nivel de confianza.  Cuando ninguno de los hablantes coincide con uno de los inscritos, la respuesta devuelve "rechazar".

El umbral de nivel de confianza debe establecerse en función del escenario. Se recomienda experimentar con el nivel de confianza y tener en cuenta la configuración adecuada para cada aplicación. Las API no están pensadas para determinar si el audio proviene de una persona de carne y hueso, una imitación o una grabación de un hablante inscrito.

El servicio no conserva la grabación de voz ni las características de voz extraídas que se le envían durante la fase de identificación.

Para más información acerca de la identificación del hablante, consulte  [Speaker API: identificación](https://westus.dev.cognitive.microsoft.com/docs/services/563309b6778daf02acc0a508/operations/5645c068e597ed22ec38f42e).
