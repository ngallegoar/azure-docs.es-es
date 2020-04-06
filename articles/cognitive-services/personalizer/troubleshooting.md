---
title: 'Solución de problemas: Personalizer'
description: En este artículo se incluyen respuestas a preguntas para solucionar problemas sobre Personalizer.
ms.topic: troubleshooting
ms.date: 02/26/2020
ms.author: diberry
ms.openlocfilehash: 904953f028eb31afe42cf477ac05be43e8b72a4d
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/28/2020
ms.locfileid: "80336023"
---
# <a name="personalizer-troubleshooting"></a>Solución de problemas de Personalizer

En este artículo se incluyen respuestas a preguntas para solucionar problemas sobre Personalizer.

## <a name="transaction-errors"></a>Errores de transacción

<details>
<summary><b>Obtengo una respuesta HTTP 429 (demasiadas solicitudes) del servicio. ¿Qué puedo hacer?</b></summary>

**Respuesta**: Si seleccionó un plan de tarifa gratis al crear la instancia de Personalizer, existe un límite de cuota para el número permitido de solicitudes de Rank. Revise la tasa de llamadas API para Rank API (en el panel Métricas de Azure Portal del recurso de Personalizer) y ajuste el plan de tarifa (en el panel Plan de tarifa) si tiene previsto que el volumen de llamadas aumente hasta superar el umbral del plan de tarifa seleccionado.

</details>

<details>
<summary><b>Obtengo un error 5xx en las llamadas a Rank API o Reward API. ¿qué debo hacer?</b></summary>

**Respuesta**: Estos problemas deben ser fáciles de solucionar. Si persisten, póngase en contacto con el servicio de soporte técnico al seleccionar **Nueva solicitud de soporte técnico** en la sección **Soporte técnico y solución de problemas**en Azure Portal del recurso de Personalizer.

</details>

## <a name="learning-loop"></a>Bucle de aprendizaje

<details>
<summary>
<b>El bucle de aprendizaje no logra una coincidencia del 100 % con el sistema sin Personalizer. ¿Cómo puedo corregirlo?</b></summary>

**Respuesta**: Motivos por los que no alcanza su objetivo con el bucle de aprendizaje:
* No hay suficientes características enviadas con la llamada API Rank.
* Errores en las características enviadas, como, por ejemplo, el envío de datos de características no agregados, como marcas de tiempo a la API Rank.
* Errores con el procesamiento de bucles, como no enviar datos de recompensa a la API Reward para eventos.

Para corregir este problema, debe cambiar el procesamiento mediante la modificación de las características enviadas al bucle o asegurarse de que la recompensa es una evaluación correcta de la calidad de la respuesta de Rank.

</details>

<details>
<summary>
<b>El bucle de aprendizaje parece que no aprende. ¿Cómo puedo corregirlo?</b></summary>

**Respuesta**: El bucle de aprendizaje necesita unas pocas miles de llamadas a Reward antes de que las llamadas a Rank se prioricen eficazmente.

Si no está seguro de cómo se está comportando el bucle de aprendizaje actualmente, ejecute una [evaluación sin conexión](concepts-offline-evaluation.md) y aplique la directiva de aprendizaje corregido.

</details>

<details>
<summary><b>Sigo obteniendo resultados de clasificación con las mismas probabilidades para todos los elementos. ¿Cómo puedo saber si Personalizer está aprendiendo?</b></summary>

**Respuesta**: Personalizer devuelve las mismas probabilidades en un resultado de la Rank API cuando acaba de iniciar y tiene un modelo _vacío_, o cuando se restablece el bucle de Personalizer y el modelo todavía está dentro del período establecido en **Frecuencia de actualización del modelo**.

Cuando comience el nuevo período de actualización, se usará el modelo actualizado y verá el cambio de probabilidades.

</details>

<details>
<summary><b>El bucle de aprendizaje estaba aprendiendo, pero parece que ya no es así, y la calidad de los resultados de Rank no es tan buena. ¿qué debo hacer?</b></summary>

**Respuesta**:
* Asegúrese de haber completado y aplicado una evaluación en Azure Portal para ese recurso de Personalizer (bucle de aprendizaje).
* Asegúrese de que se hayan enviado (a través de Reward API) y procesado todas las recompensas.

</details>


<details>
<summary><b>¿Cómo puedo saber si el bucle de aprendizaje se está actualizando periódicamente y usándose para puntuar los datos?</b></summary>

**Respuesta**: Puede encontrar la hora de la última actualización del modelo en la página **Configuración de modelo y aprendizaje** de Azure Portal. Si ve una marca de tiempo antigua, es probable que no esté enviando las llamadas de Rank y Reward. Si el servicio no tiene datos entrantes, no se actualiza el aprendizaje. Si nota que el bucle de aprendizaje no se actualiza con suficiente frecuencia, puede modificar la **Frecuencia de actualización del modelo** del bucle.

</details>

## <a name="offline-evaluations"></a>Evaluaciones sin conexión

<details>
<summary><b>La importancia de las características de una evaluación sin conexión devuelve una lista larga con cientos o miles de elementos. ¿Qué ha ocurrido?</b></summary>

**Respuesta**: Esto suele deberse a que se enviaron marcas de tiempo, Id. de usuario u otras características específicas.

</details>

<details>
<summary><b>He creado una evaluación sin conexión y se realizó con éxito casi al instante. ¿Por qué ocurre esto? No se ve ningún resultado.</b></summary>

**Respuesta**: La evaluación sin conexión utiliza los datos de modelo entrenado de los eventos en ese período de tiempo. Si no ha enviado ningún dato en el período entre las horas inicial y final de la evaluación, esta se completará sin ningún resultado. Envíe una nueva evaluación sin conexión seleccionando un intervalo de tiempo con los eventos que sabe que se enviaron a Personalizer.

</details>


## <a name="learning-policy"></a>Directiva de aprendizaje

<details>
<summary><b>¿Cómo puedo importar una directiva de aprendizaje?</b></summary>

**Respuesta**: Obtenga más información sobre los [conceptos de la directiva de aprendizaje](concept-active-learning.md#understand-learning-policy-settings) y [cómo aplicar](how-to-manage-model.md) una nueva directiva de aprendizaje. Si no quiere seleccionar una directiva de aprendizaje, puede usar [la evaluación sin conexión](how-to-offline-evaluation.md) para sugerir una directiva de aprendizaje en función de los eventos actuales.

</details>

## <a name="security"></a>Seguridad

<details>
<summary><b>La clave de API para mi bucle está en peligro. ¿Qué puedo hacer?</b></summary>

**Respuesta**: Puede regenerar una clave después de intercambiarla con los clientes para que usen otra clave. Tener dos claves le permite propagar la clave de manera diferida sin producir tiempo de inactividad. Se recomienda hacerlo de forma periódica como medida de seguridad.

</details>

## <a name="next-steps"></a>Pasos siguientes

[Configuración de la frecuencia de actualización del modelo](how-to-settings.md#model-update-frequency)