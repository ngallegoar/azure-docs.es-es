---
title: Directrices para la implementación responsable de la tecnología de voz sintética
titleSuffix: Azure Cognitive Services
description: Directrices generales de diseño de Microsoft para usar la tecnología de voz sintética. Se desarrollaron en estudios que Microsoft llevó a cabo con actores de voz, consumidores y personas con trastornos del lenguaje para guiar el desarrollo responsable de la voz sintética.
services: cognitive-services
author: benoah
manager: nitinme
ms.service: cognitive-services
ms.subservice: speech-service
ms.topic: conceptual
ms.date: 10/11/2019
ms.author: benoah
ms.openlocfilehash: 7d80ffb575c6aa15695279584b58288cbc16be43
ms.sourcegitcommit: 10d00006fec1f4b69289ce18fdd0452c3458eca5
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 11/21/2020
ms.locfileid: "95024983"
---
# <a name="guidelines-for-responsible-deployment-of-synthetic-voice-technology"></a>Directrices para la implementación responsable de la tecnología de voz sintética
A continuación se presentan las directrices generales de diseño de Microsoft para usar la tecnología de voz sintética. Se desarrollaron en estudios que Microsoft llevó a cabo con actores de voz, consumidores e incluso personas con trastornos del lenguaje para guiar el desarrollo responsable de la voz sintética.

## <a name="general-considerations"></a>Consideraciones generales
En la implementación de la tecnología de síntesis de voz, se aplican las siguientes directrices en la mayoría de los escenarios.

### <a name="disclose-when-the-voice-is-synthetic"></a>Divulgación de cuando la voz es sintética
Divulgar el hecho de que la voz se generó por ordenador no solo minimiza el riesgo de resultados perjudiciales derivados de una decepción, sino que también aumenta la confianza en la organización que presenta la voz. Más información sobre [cómo divulgar información](concepts-disclosure-guidelines.md).

### <a name="select-appropriate-voice-types-for-your-scenario"></a>Selección de los tipos de voz adecuados para su escenario
Considere cuidadosamente el contexto de uso y los posibles daños asociados al uso de la voz sintética. Por ejemplo, es posible que las voces sintéticas de alta fidelidad no sean adecuadas en situaciones de alto riesgo, como la mensajería personal, las transacciones financieras o situaciones complejas que requieran de la adaptación humana o la empatía. Los usuarios también pueden tener expectativas diferentes para cada tipo de voz. Por ejemplo, al escuchar noticias confidenciales leídas por una voz sintética, algunos usuarios prefieren una lectura más empática y similar a los seres humanos, mientras que otros prefieren una voz más imparcial y monótona. Considere la posibilidad de probar la aplicación para comprender mejor las preferencias del usuario.

### <a name="be-transparent-about-capabilities-and-limitations"></a>Transparencia en cuanto a funcionalidades y limitaciones
Es más probable que los usuarios tengan mayores expectativas al interactuar con agentes de voz sintéticos de alta fidelidad. Por consiguiente, cuando las funcionalidades del sistema no cumplen esas expectativas, la confianza puede verse afectada y puede dar lugar a experiencias desagradables o incluso dañinas.

### <a name="provide-optional-human-support"></a>Ofrecimiento de soporte técnico humano opcional
En escenarios transaccionales ambiguos (por ejemplo, un centro telefónico de soporte técnico), los usuarios no siempre confían en que un agente artificial responderá correctamente a sus solicitudes. El soporte técnico humano puede ser necesario en estas situaciones, independientemente de la calidad realista de la voz o la funcionalidad del sistema.

## <a name="considerations-for-voice-talent"></a>Consideraciones sobre los actores de voz
Al trabajar con actores de voz, como actores de doblaje, para crear voces sintéticas, se aplica la directriz siguiente.

### <a name="obtain-meaningful-consent-from-voice-talent"></a>Obtención del consentimiento significativo del actor de voz
Los actores de voz esperan tener control sobre su fuente de voz (cómo y dónde se usará), así como ser compensados siempre que esta se use. Por lo tanto, los propietarios del sistema deben obtener el permiso explícito por escrito del actor de voz y tener claras especificaciones contractuales sobre los casos de uso, la duración de uso, la compensación, etc. Algunos actores de voz no saben cuáles son los posibles usos malintencionados de la tecnología y los propietarios del sistema deben instruirlos sobre las funcionalidades de la tecnología. Para obtener más información sobre los actores de voz y el consentimiento, consulte [Divulgación de talento de voz](/legal/cognitive-services/speech-service/disclosure-voice-talent).


## <a name="considerations-for-those-with-speech-disorders"></a>Consideraciones para las personas con trastornos del habla
Al trabajar con personas con trastornos del habla para crear o implementar la tecnología de voz sintética, se aplican las siguientes directrices.

### <a name="provide-guidelines-to-establish-contracts"></a>Ofrecimiento de directrices para celebrar contratos
Proporcione directrices para celebrar contratos con personas que usan la voz sintética como ayuda para hablar. El contrato debería especificar las partes que poseen la voz, la duración de uso, los criterios de transferencia de propiedad, los procedimientos para eliminar la fuente de voz y cómo evitar el acceso no autorizado. Además, habilite la transferencia contractual de la propiedad de la fuente de voz después de la muerte a los familiares si esa persona ha dado su permiso.

### <a name="account-for-inconsistencies-in-speech-patterns"></a>Consideración de las incoherencias en los patrones de habla
En el caso de las personas con trastornos del habla que registran sus propias fuentes de voz, las incoherencias en su patrón de habla (mala pronunciación o incapacidad de pronunciar ciertas palabras) pueden complicar el proceso de grabación. En estos casos, la tecnología de voz sintética y las sesiones de grabación deberían adaptarse a ellos (es decir, proporcionar descansos y un número adicional de sesiones de grabación).

### <a name="allow-modification-over-time"></a>Permitir la modificación a lo largo del tiempo
Las personas con trastornos del habla quieren hacer actualizaciones en su voz sintética para reflejar su edad (por ejemplo, un niño que alcanza la pubertad). Las personas también pueden tener preferencias estilísticas que cambian con el tiempo y puede que quieran realizar cambios en las características de tono, acento u otras.


## <a name="reference-docs"></a>Documentos de referencia

* [Divulgación de talento de voz](/legal/cognitive-services/speech-service/disclosure-voice-talent)
* [Información general sobre control](concepts-gating-overview.md)
* [Procedimiento de divulgación](concepts-disclosure-guidelines.md)
* [Modelos de diseño de divulgación](concepts-disclosure-patterns.md)

## <a name="next-steps"></a>Pasos siguientes

* [Divulgación de talento de voz](/legal/cognitive-services/speech-service/disclosure-voice-talent)
* [Procedimiento de divulgación](concepts-disclosure-guidelines.md)
* [Modelos de diseño de divulgación](concepts-disclosure-patterns.md)