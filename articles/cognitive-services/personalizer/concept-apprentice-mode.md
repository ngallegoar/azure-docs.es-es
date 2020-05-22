---
title: 'Modo de aprendiz: Personalizer'
description: ''
ms.topic: conceptual
ms.date: 05/01/2020
ms.openlocfilehash: 2697ab4b32edbd4841f2b11725fda46e90e7ae7e
ms.sourcegitcommit: bb0afd0df5563cc53f76a642fd8fc709e366568b
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 05/19/2020
ms.locfileid: "83599308"
---
# <a name="use-apprentice-mode-to-train-personalizer-without-affecting-your-existing-application"></a>Uso del modo de aprendiz para el entrenamiento de Personalizer sin que ello afecte a la aplicación existente

Debido a la naturaleza del aprendizaje de refuerzo **real**, un modelo de Personalizer solo se puede entrenar en un entorno de producción. Al implementar un nuevo caso de uso, el modelo de Personalizer no funciona de forma eficaz porque se requiere tiempo para que el modelo esté suficientemente entrenado.  El **modo de aprendiz** es un comportamiento de aprendizaje que simplifica esta situación y le permite aumentar la confianza en el modelo, sin que el desarrollador cambie nada de código.

[!INCLUDE [Important Blue Box - Apprentice mode pricing tier](./includes/important-apprentice-mode.md)]

## <a name="what-is-apprentice-mode"></a>¿Qué es el modo de aprendiz?

Es similar a la forma en que un aprendiz aprende de un patrón y con experiencia puede mejorar. El modo de aprendiz es un _comportamiento_ que permite a Personalizer aprender observando los resultados obtenidos de la lógica de la aplicación existente.

Personalizer entrena mediante la imitación de la misma salida que la aplicación. A medida que fluyen los eventos, Personalizer puede _ponerse al día_ con la aplicación existente sin que ello afecte a la lógica y los resultados existentes. Las métricas, disponibles en Azure Portal y la API, le ayudan a comprender el rendimiento a medida que el modelo aprende.

Una vez que Personalizer ha aprendido y alcanzado cierto nivel de comprensión, el desarrollador puede cambiar el comportamiento del modo de aprendiz al modo en línea. En ese momento, Personalizer comienza a influir en las acciones de Rank API.

## <a name="purpose-of-apprentice-mode"></a>Propósito del modo de aprendiz

El modo de aprendiz le proporciona confianza en el servicio Personalizer y sus capacidades de Machine Learning, y garantiza que el servicio recibe información de la que se puede aprender, sin poner en peligro el tráfico en línea.

Las dos razones principales para usar el modo de aprendiz son las siguientes:

* Mitigación de los **arranques en frío**: el modo de aprendiz ayuda a administrar y evaluar el costo del tiempo de aprendizaje de un "nuevo" modelo (cuando no se devuelve la mejor acción y no se logra un nivel de efectividad satisfactorio alrededor del 75-85 %).
* **Validación de las características de acción y contexto**: las características enviadas en acciones y en el contexto pueden ser inadecuadas o inexactas (escasas, excesivas, incorrectas o demasiado específicas para entrenar Personalizer para alcanzar el grado de eficacia ideal). Use las [evaluaciones de características](concept-feature-evaluation.md) para buscar y corregir problemas con las características.

## <a name="when-should-you-use-apprentice-mode"></a>¿Cuándo se debe usar el modo de aprendiz?

Use el modo de aprendiz para entrenar Personalizer para mejorar su eficacia en los siguientes escenarios, a la vez que impide que Personalizer afecte a la experiencia de los usuarios:

* Está implementando Personalizer en un caso de uso nuevo.
* Ha cambiado significativamente las características que envía en contexto o acciones.
* Ha cambiado significativamente cuándo y cómo se calculan las recompensas.

El modo de aprendiz no es una manera eficaz de medir el impacto de Personalizer en las puntuaciones de recompensa. Para medir el grado de eficacia de Personalizer en la elección de la mejor acción posible para cada llamada de Rank, use [evaluaciones sin conexión](concepts-offline-evaluation.md).

## <a name="who-should-use-apprentice-mode"></a>¿Quién debe usar el modo de aprendiz?

El modo de aprendiz es útil para desarrolladores, científicos de datos y responsables de la toma de decisiones empresariales:

* Los **desarrolladores** pueden usar el modo de aprendiz para garantizar que Rank API y Reward API se usan correctamente en la aplicación, y que las características que se envían a Personalizer desde la aplicación no contienen errores ni características no pertinentes, como una marca de tiempo o un elemento UserID.

* Los **científicos de datos** pueden usar el modo de aprendiz para comprobar que las características son eficaces para entrenar los modelos de Personalizer y que los tiempos de espera de recompensas no son demasiado largos o cortos.

* Los **responsables de la toma de decisiones empresariales** pueden usar el modo de aprendiz para evaluar el potencial de Personalizer para mejorar los resultados (es decir, las recompensas) en comparación con la lógica de negocios existente. Esto les permite tomar una decisión informada que afecta a la experiencia del usuario, donde los ingresos reales y la satisfacción del usuario están en juego.

## <a name="comparing-behaviors---apprentice-mode-and-online-mode"></a>Comparación de comportamientos: modo de aprendiz y modo en línea

Aprendizaje cuando el modo de aprendiz difiere del modo en línea de las siguientes maneras.

|Área|Modo de aprendiz|Modo en línea|
|--|--|--|
|Impacto en la experiencia del usuario|Puede usar el comportamiento del usuario existente para el entrenamiento de Personalizer. Para ello, permita que observe (sin alterar) cuál habría sido su **acción predeterminada** y la recompensa obtenida. Esto significa que la experiencia de los usuarios y sus resultados empresariales no se verán afectados.|Muestra la acción superior devuelta desde la llamada a Rank para afectar al comportamiento del usuario.|
|Velocidad de aprendizaje|Personalizer aprenderá más lentamente en el modo de aprendiz que en el modo en línea. El modo de aprendiz solo puede aprender observando las recompensas que ha obtenido la **acción predeterminada**, lo que limita la velocidad de aprendizaje, ya que no se puede realizar ninguna exploración.|Aprende más rápido porque puede aprovechar el modelo actual y explorar nuevas tendencias.|
|"Tope" de eficacia del aprendizaje|Personalizer puede aproximarse, muy pocas veces igualar y nunca superar el rendimiento de la lógica de negocios base (el total de recompensas logrado por la **acción predeterminada** de cada llamada a Rank).|Personalizer debe superar las aplicaciones de línea de base y, con el tiempo, cuando se detenga, debe realizar una evaluación sin conexión y una evaluación de características para continuar obteniendo mejoras para el modelo. |
|Valor de Rank API para rewardActionId|La experiencia de los usuarios no se ve afectada, ya que _rewardActionId_ siempre es la primera acción que se envía en la solicitud Rank. En otras palabras, Rank API no hace nada visible para la aplicación durante el modo de aprendiz. Las Reward API de la aplicación no deben cambiar el modo en que se usa Reward API entre un modo y otro.|La experiencia de los usuarios se modificará en función del valor _rewardActionId_ que Personalizer elija para la aplicación. |
|Evaluaciones|Personalizer mantiene una comparación de los totales de recompensas que la lógica de negocios predeterminada obtiene y los totales de recompensas que Personalizer obtendría en el modo en línea en ese momento. Existe una comparación disponible en Azure Portal para ese recurso.|Evalúe la eficacia de Personalizer mediante [evaluaciones sin conexión](concepts-offline-evaluation.md), que le permiten comparar las recompensas totales que Personalizer ha obtenido con las recompensas potenciales de la línea de base de la aplicación.|

Nota sobre la eficacia del modo de aprendiz:

* La eficacia de Personalizer en el modo de aprendiz no suele acercarse al 100 % de la línea de base de la aplicación y nunca lo supera.
* Los procedimientos recomendados serían no intentar llegar al 100 %. El objetivo sería un intervalo del 75 al 85 % en función del caso de uso.

## <a name="using-apprentice-mode-to-train-with-historical-data"></a>Uso del modo de aprendiz para el entrenamiento con datos históricos

Si tiene una cantidad significativa de datos históricos, puede usarlos para el entrenamiento de Personalizer. Puede usar el modo de aprendiza para reproducir los datos a través de Personalizer.

Configure Personalizer en el modo de aprendiz y cree un script que llame a Rank con las características de acciones y de contexto de los datos históricos. Llame a Reward API en función de los cálculos de los registros de estos datos. Necesitará aproximadamente 50 000 eventos históricos para ver algunos resultados, pero se recomiendan 500 000 para una mayor confianza en los resultados.

Al realizar el entrenamiento a partir de datos históricos, se recomienda que los datos enviados (características de contexto y acciones, su diseño en el archivo JSON usado para las solicitudes de Rank y el cálculo de la recompensa en este conjunto de datos de aprendizaje) coincidan con los datos (características y cálculo de recompensas) disponibles de la aplicación existente.

Los datos sin conexión y post facto suelen ser más incompletos, presentan más ruido y se diferencian en el formato. Aunque el entrenamiento a partir de datos históricos es posible, los resultados de hacerlo pueden ser no concluyentes y no predecir correctamente el grado de aprendizaje de Personalizer, sobre todo si las características varían entre los datos pasados y la aplicación existente.

Normalmente, para Personalizer, en comparación con el entrenamiento con datos históricos, el cambio de comportamiento al modo de aprendiz y el aprendizaje a partir de una aplicación existente es una vía más eficaz para tener un modelo eficaz, con menos trabajo, ingeniería de datos y trabajo de limpieza.

## <a name="using-apprentice-mode-versus-ab-tests"></a>Uso del modo de aprendiz frente a las pruebas A/B

Solo es útil realizar pruebas A/B de los tratamientos de Personalizer una vez que se ha validado y está aprendiendo en el modo en línea. En el modo de aprendiz, solo se usa la **acción predeterminada**, lo que significa que todos los usuarios verán la experiencia de control de forma eficaz.

Aunque Personalizar sea solo el _tratamiento_, el mismo desafío se presenta cuando la validación de los datos es adecuada para el entrenamiento de Personalizer. En su lugar, se podría usar el modo de aprendiz, con un 100 % de tráfico y donde todos los usuarios obtengan la experiencia de control (sin alteraciones).

Cuando tiene un caso de uso que utiliza Personalizer y el aprendizaje en línea, los experimentos A/B le permiten realizar cohortes controladas y una comparación científica de los resultados, que pueden ser más complejos que las señales usadas para las recompensas. Una pregunta de ejemplo a la que podría responder una prueba A/B es la siguiente: `In a retail website, Personalizer optimizes a layout and gets more users to _check out_ earlier, but does this reduce total revenue per transaction?`

## <a name="next-steps"></a>Pasos siguientes

* Obtener información sobre [eventos activos e inactivos](concept-active-inactive-events.md)