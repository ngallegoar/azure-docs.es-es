---
title: Conceptos de diseño con simulación acústica
titlesuffix: Azure Cognitive Services
description: En esta información general conceptual se explica cómo Project Acoustics incorpora la simulación acústica al proceso de diseño de sonido.
services: cognitive-services
author: NoelCross
manager: nitinme
ms.service: cognitive-services
ms.subservice: acoustics
ms.topic: conceptual
ms.date: 03/20/2019
ms.author: noelc
ROBOTS: NOINDEX
ms.openlocfilehash: 11e1e3f45b5198ddedb6c31fcd354185adef445d
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/28/2020
ms.locfileid: "68854307"
---
# <a name="project-acoustics-design-process-concepts"></a>Conceptos del proceso de diseño de Project Acoustics

En esta información general conceptual se explica cómo Project Acoustics incorpora simulación acústica física al proceso de diseño de sonido.

## <a name="sound-design-with-audio-dsp-parameters"></a>Diseño de sonido con parámetros DSP de audio

Los títulos interactivos en 3D logran su sonido particular mediante bloques de procesamiento de señales de audio digitales (DSP) hospedados en un motor de audio. Estos bloques varían en complejidad desde mezcla simple hasta reverberación, eco, retardo, ecualización, compresión y limitación, y otros efectos. La selección, la organización y la configuración de parámetros sobre estos efectos es responsabilidad del diseñador de sonido, quien se ocupa de crear un gráfico de audio que logra los objetivos estéticos y de juego de la experiencia.

En un título interactivo, a medida que los sonidos y el oyente circulan por el espacio 3D, ¿cómo se adaptan estos parámetros a las condiciones cambiantes? Con frecuencia, el diseñador de sonido organiza por todo el espacio volúmenes que están programados para desencadenar cambios en los parámetros y conseguir cambios, por ejemplo, en los efectos de reverberación; o bien para reducir sonidos de la mezcla cuando el oyente se mueve de una parte de la escena a otra. También existen sistemas acústicos que pueden automatizar algunos de estos efectos.

Los títulos en 3D usan sistemas de física de la luz y la cinemática que están motivados por la física, pero que el diseñador ha ajustado para conseguir una mezcla de objetivos de inmersión y juego. Un diseñador visual no establece valores de píxeles individuales, sino que ajusta los modelos 3D, los materiales y los sistemas de transporte de luz, todos con base física, para compensar la estética visual y los costos de CPU. ¿Cuál sería el proceso equivalente para el audio? Project Acoustics es un primer paso en la exploración de esta pregunta. En primer lugar, nos referiremos a lo que significa transportar la energía acústica por un espacio.

![Captura de pantalla de una escena de AltSpace superpuesta con zonas de reverberación](media/reverb-zones-altspace.png)

## <a name="impulse-responses-acoustically-connecting-two-points-in-space"></a>Respuestas al impulso: conexión acústica de dos puntos en el espacio

Si está familiarizado con el diseño de audio, es posible que esté familiarizado con las respuestas al impulso acústico. Una respuesta al impulso acústico modela el transporte de un sonido desde una fuente hasta un oyente. Por lo tanto, una respuesta al impulso puede capturar cada uno de los efectos interesantes de la acústica de la sala, como oclusión y reverberación. Las respuestas al impulso también presentan algunas propiedades eficaces que permiten ampliar los efectos del DSP de audio. Al sumar dos señales de audio y procesarlas con una respuesta al impulso se consigue el mismo resultado que al aplicar la respuesta al impulso por separado a cada señal y sumar los resultados. Además, la propagación acústica y las respuestas al impulso no dependen del audio que se procesa, solo de la escena modelada y de las ubicaciones de fuente y oyente. En resumen, una respuesta al impulso sintetiza el efecto de la escena en la propagación del sonido.

Una respuesta al impulso captura todos los efectos interesantes de la acústica de una sala, y podemos aplicarlos al audio de forma eficiente con un filtro, y las respuestas al impulso podemos obtenerlas de la medición o la simulación. Pero, ¿qué ocurre si no queremos que la acústica coincida exactamente con la física, sino moldearla para que coincida con las exigencias emocionales de una escena? Pero de forma muy parecida a lo que sucede con los valores de píxel, una respuesta al impulso es simplemente una lista de miles de números. Así que, ¿cómo podemos ajustarla para satisfacer las necesidades estéticas? Y, ¿qué pasa si queremos tener oclusión/obstrucción que varíe de manera suave al pasar por puertas o detrás de obstáculos?, ¿cuántas respuestas al impulso necesitamos para obtener un efecto suave? ¿Qué ocurre si la fuente se mueve rápido? ¿Cómo se interpola?

Parece difícil usar simulación y respuestas al impulso con algunos aspectos de la acústica en títulos interactivos. Sin embargo, podemos crear un sistema de transporte de audio que admita los ajustes del diseñador si podemos conectar nuestras respuestas al impulso de la simulación con nuestros conocidos parámetros de efectos de DSP de audio.

## <a name="connecting-simulation-to-audio-dsp-with-parameters"></a>Conexión de la simulación al DSP de audio con parámetros

Una respuesta al impulso contiene todos los efectos acústicos interesantes (y menos interesantes). Los bloques de DSP de audio, cuando sus parámetros se establecen correctamente, pueden representar los efectos acústicos interesantes. El uso de simulación acústica para controlar un bloque de DSP de audio y automatizar el transporte de audio en una escena 3D es solo cuestión de medir los parámetros de DSP de audio de una respuesta al impulso. Esta medida se entiende bien para determinados efectos acústicos importantes y comunes, como reverberación, obstrucción, portalling y oclusión.

Pero, si la simulación está conectada directamente a los parámetros de DSP de audio, ¿dónde está el ajuste del diseñador? ¿Qué ganamos? Bueno, ganamos una cantidad significativa de memoria al descartar las respuestas al impulso y retener unos cuantos parámetros de DSP. Y, para dar al diseñador algo de poder sobre el resultado final, solo necesitamos encontrar una manera de insertar el diseñador entre la simulación y el DSP de audio.

![Gráfico con respuesta al impulso estilizada con parámetros superpuestos](media/acoustic-parameters.png)

## <a name="sound-design-by-transforming-audio-dsp-parameters-from-simulation"></a>Diseño del sonido mediante la transformación de los parámetros de DSP de audio a partir de la simulación

Piense en el efecto que tienen sus gafas de sol sobre su visión del mundo. En un día brillante, las gafas pueden reducir el brillo a algo más cómodo. En una sala oscura, es posible que no pueda ver nada en absoluto. Las gafas no establecen un determinado nivel de brillo en todas las situaciones; solo hacen que todo esté más oscuro.

Si usamos simulación para controlar nuestro DSP de audio mediante los parámetros de oclusión y reverberación, podemos agregar un filtro después del simulador para ajustar los parámetros que "ve" el DSP. El filtro no impondría un determinado nivel de longitud de cola de oclusión o reverberación, igual que las gafas de sol no hacen que cada habitación tenga el mismo brillo. El filtro podría simplemente hacer que cada oclusor se ocluyera menos. O se ocluyera más. Al agregar y ajustar un filtro de parámetro de oclusión de "oscurecimiento", las habitaciones grandes y abiertas seguirían teniendo poco o ningún efecto de oclusión, mientras que las puertas aumentarían el efecto de oclusión de medio a fuerte, pero conservarían la suavidad de las transiciones de efectos que proporciona la simulación.

En este paradigma, la tarea del diseñador pasa de la selección de los parámetros acústicos para cada situación a la selección y el ajuste de los filtros para aplicar a los parámetros más importantes de DSP procedentes de la simulación. Como resultado, se elevan las actividades del diseñador de los problemas relacionados estrictamente con la configuración de transiciones suaves a los problemas más grandes de la intensidad de los efectos de oclusión y reverberación y la presencia de fuentes en la mezcla. Por supuesto, cuando la situación lo exige, un filtro siempre disponible es volver y elegir los parámetros de DSP para una fuente concreta de una situación concreta.

## <a name="sound-design-in-project-acoustics"></a>Diseño del sonido en Project Acoustics

El paquete de Project Acoustics integra cada uno de los componentes descritos anteriormente: un simulador, un codificador que extrae los parámetros y compila el recurso acústico, audio de DSP y una selección de filtros. El diseño del sonido con Project Acoustics supone elegir parámetros para los filtros que ajustan los parámetros de oclusión y reverberación derivados de la simulación y aplicados al DSP de audio, con controles dinámicos que se exponen en el editor de juego y el motor de audio.

## <a name="next-steps"></a>Pasos siguientes
* Pruebe el paradigma de diseño mediante el [inicio rápido de Project Acoustics para Unity](unity-quickstart.md) o el [inicio rápido de Project Acoustics para Unreal](unreal-quickstart.md)
* Explore los [controles de diseño de Project Acoustics para Unity](unity-workflow.md) o los [controles de diseño de Project Acoustics para Unreal](unreal-workflow.md).

