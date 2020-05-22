---
title: datos, recopilación
description: Información sobre los datos de ejemplo que se recopilan al desarrollar la aplicación
ms.topic: conceptual
ms.date: 05/06/2020
ms.openlocfilehash: 7abb3736eb9d7c73465ffa646b79e8e7dd7ae88b
ms.sourcegitcommit: bb0afd0df5563cc53f76a642fd8fc709e366568b
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 05/19/2020
ms.locfileid: "83599328"
---
# <a name="data-collection-for-your-app"></a>Recopilación de datos de la aplicación

Una aplicación de Language Understanding (LUIS) necesita datos como parte del desarrollo de aplicaciones.

## <a name="data-used-in-luis"></a>Datos usados en LUIS

LUIS usa texto como datos a fin de entrenar y probar la aplicación de LUIS para la clasificación de [intenciones](luis-concept-intent.md) y para la extracción de [entidades](luis-concept-entity-types.md). Necesita un conjunto de datos suficientemente grande que incluya datos suficientes a fin de crear conjuntos de datos independientes para el entrenamiento y las pruebas con la diversidad y la distribución que se indican a continuación.  Los datos de cada uno de estos conjuntos no deben superponerse.

## <a name="training-data-selection-for-example-utterances"></a>Selección de los datos de entrenamiento, por ejemplo, expresiones

Seleccione expresiones para el conjunto de entrenamiento en función de los criterios siguientes:

* **Le recomendamos usar datos reales**:
    * **Datos reales de la aplicación cliente**: seleccione expresiones que sean datos reales de la aplicación cliente.  Si el cliente envía un formulario web con su consulta hoy y usted está creando un bot, puede empezar con los datos del formulario web.
    * **Datos de colaboración abierta distribuida**: si no tiene ningún dato existente, considere la posibilidad de usar expresiones de colaboración abierta distribuida.  Intente obtener expresiones de colaboración abierta distribuida de su población de usuarios para su escenario a fin de obtener la mejor aproximación de los datos reales que verá su aplicación. Las expresiones creadas por personas de colaboración abierta distribuida son mejores que las que generan los equipos.  Cuando compile un conjunto de datos de expresiones sintéticas generadas a partir de patrones específicos, este no contendrá gran parte de la variación natural que verá con las expresiones creadas por personas y no terminará proporcionando una buena generalización en la producción.
* **Diversidad de datos**:
    * **Diversidad de regiones**: asegúrese de que los datos de cada intención sean lo más variados posible, como _frases_ (elección de palabras) y _gramática_.  Si está enseñando una intención de directivas de recursos humanos sobre días de vacaciones, asegúrese de tener expresiones que representen los términos que se usan para todas las regiones a las que atiende.  Por ejemplo, en Europa, los usuarios pueden preguntar sobre `taking a holiday` y, en el caso de los usuarios de EE. UU., pueden preguntar sobre `taking vacation days`.
    * **Diversidad del idioma**: si tiene usuarios con varios idiomas nativos que se comunican en un segundo idioma, asegúrese de tener expresiones que representen a los hablantes no nativos.
    * **Diversidad de entrada**: considere su ruta de acceso de entrada a los datos. Si va a recopilar datos de una persona, un departamento o un dispositivo de entrada (micrófono), es probable que le falte diversidad, que será importante para que la aplicación obtenga información sobre todas las rutas de acceso de entrada.
    * **Diversidad de puntuación**: considere la posibilidad de que los usuarios usen distintos niveles de puntuación en las aplicaciones de texto y asegúrese de contar con diversidad en relación al uso de la puntuación. Si usa datos que proceden de la voz, esta no incluirá ningún signo de puntuación, por lo que sus datos tampoco deberían tenerlos.
* **Distribución de datos**: asegúrese de que los datos repartidos entre las intenciones sean los mismos que los que recibe la aplicación cliente. Si la aplicación de LUIS va a clasificar expresiones que son solicitudes para programar un permiso (50 %), pero también verá expresiones sobre la consulta de los días de permiso restantes (20%), los permisos que se aprueban (20 %) y charlas sobre otros temas (10 %), el conjunto de datos debe tener los porcentajes de ejemplo de cada tipo de expresión.
* **Uso de todos los formularios de datos**: si la aplicación de LUIS recopilará datos en varios formatos, asegúrese de incluirlos en las expresiones de entrenamiento. Por ejemplo, si la aplicación cliente recopila la entrada de texto y la de voz, debe tener tanto expresiones generadas de voz a texto como escritas con teclado.  Verá diferentes variaciones entre la forma en que hablan los usuarios y la que escriben, así como distintos errores en el reconocimiento de voz y ortográficos.  Toda esta variación debe representarse en los datos de entrenamiento.
* **Tendencias positivas y negativas**: para entrenar una aplicación de LUIS, esta debe saber la intención que tiene (positiva) y la que no tiene (negativa). En LUIS, las expresiones solo pueden ser positivas para una sola intención. Cuando se agrega una expresión a una intención, LUIS convierte automáticamente esa misma expresión de ejemplo en un ejemplo negativo para todas las demás intenciones.
* **Datos fuera del ámbito de la aplicación**: si la aplicación va a ver expresiones que se encuentran fuera de sus intenciones definidas, asegúrese de proporcionarlas. Los ejemplos que no están asignados a una intención definida determinada se etiquetarán con la intención **Ninguna**.  Es importante tener ejemplos realistas para la intención **Ninguna** a fin de predecir correctamente las expresiones que están fuera del ámbito de las intenciones definidas.

    Por ejemplo, si está creando un bot de recursos humanos centrado en el tiempo de permiso y tiene las tres intenciones siguientes:
    * Programar o editar un permiso
    * Consultar los días de permiso disponibles
    * Aprobar/desaprobar un permiso

    Le recomendamos que se asegure de que tiene expresiones que cubren estas intenciones, pero que también cubran posibles expresiones de fuera de ese ámbito que la aplicación deba atender, como las siguientes:
    * `What are my medical benefits?`
    * `Who is my HR rep?`
    * `tell me a joke`
* **Ejemplos poco frecuentes**: la aplicación deberá contar con ejemplos poco frecuentes, además de los comunes.  Si la aplicación nunca ha detectado ejemplos poco frecuentes, no podrá identificarlos en producción. Si usa datos reales, podrá predecir con más precisión el modo en que la aplicación de LUIS funcionará en producción.

### <a name="quality-instead-of-quantity"></a>Calidad en lugar de cantidad

Tenga en cuenta la calidad de los datos existentes antes de agregar más datos.  Con LUIS, se usa la enseñanza automática.  La combinación de las etiquetas y las características de aprendizaje automático que define es lo que usa la aplicación de LUIS.  Esta no se basa simplemente en la cantidad de etiquetas para tomar la mejor predicción,  sino que lo más importante es la diversidad de los ejemplos y su representación de lo que la aplicación de LUIS verá en producción.

### <a name="preprocessing-data"></a>Preprocesamiento de datos

Los siguientes pasos de preprocesamiento le ayudarán a crear una aplicación de LUIS mejorada:

* **Retire duplicados**: las expresiones duplicadas no perjudicarán, pero tampoco ayudan, de modo que si las elimina, se ahorrará tiempo de etiquetado.
* **Aplique el mismo preprocesamiento de la aplicación cliente**: si la aplicación cliente, que llama al punto de conexión de predicción de LUIS, aplica el procesamiento de datos en tiempo de ejecución antes de enviar el texto a LUIS, debe entrenar la aplicación de LUIS con los datos que se procesan de la misma manera. Por ejemplo, si la aplicación cliente usa [Bing Spell Check](../bing-spell-check/overview.md) para corregir la ortografía de las entradas de LUIS, corrija las expresiones de entrenamiento y prueba antes de agregarlas a LUIS.
* **No aplique nuevos procesos de limpieza que la aplicación cliente no use**: si la aplicación cliente acepta texto generado por voz directamente sin ninguna limpieza, como de gramática o puntuación, las expresiones deben reflejar lo mismo, así como cualquier signo de puntuación que falte y cualquier otro reconocimiento incorrecto que deba tener en cuenta.
* **No limpie los datos**: no elimine la entrada con formato incorrecto que podría obtener de reconocimientos de voz ilegibles, presiones de teclas accidentales o texto mal escrito. Si la aplicación va a ver entradas como estas, es importante que esté entrenada y probada para ellas. Agregue una intención de _entrada con formato incorrecto_ si no espera que la aplicación la entienda. Etiquete estos datos para ayudar a la aplicación de LUIS a predecir la respuesta correcta en tiempo de ejecución. La aplicación cliente puede elegir una respuesta adecuada a expresiones ininteligibles como `Please try again`.

### <a name="labeling-data"></a>Etiquetado de datos

* **Etiquete el texto como si fuera correcto**: las expresiones de ejemplo deben tener todos los formatos de una entidad etiquetada. Esto incluye texto mal escrito y traducido de forma errónea.

### <a name="data-review-after-luis-app-is-in-production"></a>Revisión de datos cuando la aplicación de LUIS está en producción

[Revise las expresiones de punto de conexión ](luis-concept-review-endpoint-utterances.md) para supervisar el tráfico real de las expresiones una vez tenga implementada una aplicación en producción.  De este modo, podrá actualizar las expresiones de entrenamiento con datos reales, lo que mejorará la aplicación. Todas las aplicaciones compiladas con datos de escenarios no reales o de colaboración abierta distribuida deberán mejorarse en función de su uso real.

## <a name="test-data-selection-for-batch-testing"></a>Selección de datos de prueba para las pruebas por lotes

Todos los principios enumerados anteriormente para el entrenamiento de expresiones se aplican a las expresiones que debe usar para el [conjunto de pruebas](luis-concept-batch-test.md). Asegúrese de que la distribución entre las intenciones y las entidades refleje la real tanto como sea posible.

No vuelva a usar expresiones del conjunto de entrenamiento en el conjunto de pruebas. Esto sesga los resultados de forma incorrecta y no le proporcionará la indicación adecuada de cómo se ejecutará la aplicación de LUIS en producción.

Una vez publicada la primera versión de la aplicación, deberá actualizar el conjunto de pruebas con expresiones del tráfico real para asegurarse de que el conjunto de pruebas refleje la distribución de producción y usted puede supervisar el rendimiento realista a lo largo del tiempo.

## <a name="next-steps"></a>Pasos siguientes

> [!div class="nextstepaction"]
> [Información sobre cómo LUIS modifica los datos antes de la predicción](luis-concept-data-alteration.md)
