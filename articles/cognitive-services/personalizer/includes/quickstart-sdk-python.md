---
title: archivo de inclusión
description: archivo de inclusión
services: cognitive-services
author: diberry
manager: nitinme
ms.service: cognitive-services
ms.subservice: personalizer
ms.topic: include
ms.custom: include file
ms.date: 04/27/2020
ms.author: diberry
ms.openlocfilehash: d35f11125809167c4834b33f913d9369a6402ac0
ms.sourcegitcommit: 34a6fa5fc66b1cfdfbf8178ef5cdb151c97c721c
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 04/28/2020
ms.locfileid: "82188893"
---
[Documentación de referencia](https://docs.microsoft.com/python/api/azure-cognitiveservices-personalizer/azure.cognitiveservices.personalizer?view=azure-python) | [Código fuente de la biblioteca](https://github.com/Azure/azure-sdk-for-python/tree/master/sdk/cognitiveservices/azure-cognitiveservices-personalizer) | [Paquete (PyPi)](https://pypi.org/project/azure-cognitiveservices-personalizer/) | [Ejemplos](https://github.com/Azure-Samples/cognitive-services-personalizer-samples/blob/master/quickstarts/python/sample.py)

## <a name="prerequisites"></a>Prerequisites

* Una suscripción a Azure: [cree una cuenta gratuita](https://azure.microsoft.com/free/)
* [Python 3.x](https://www.python.org/)

## <a name="using-this-quickstart"></a>Uso de este inicio rápido


Hay que dar varios pasos para usar este inicio rápido:

* En Azure Portal, cree un recurso de Personalizer
* En la página **Configuración** del recurso Personalizer de Azure Portal, cambie la frecuencia de actualización del modelo a un intervalo muy corto.
* En un editor de código, cree un archivo de código y edítelo
* En la línea de comandos o en el terminal, instale el SDK desde la línea de comandos
* En la línea de comandos o en el terminal, ejecute el archivo de código

[!INCLUDE [Create Azure resource for Personalizer](create-personalizer-resource.md)]

[!INCLUDE [Change model frequency](change-model-frequency.md)]

## <a name="install-the-python-library-for-personalizer"></a>Instalación de la biblioteca de Python para Personalizer

Instale la biblioteca cliente de Personalizer para Python con el siguiente comando:

```console
pip install azure-cognitiveservices-personalizer
```

## <a name="object-model"></a>Modelo de objetos

El cliente de Personalizer es un objeto [PersonalizerClient](https://docs.microsoft.com/python/api/azure-cognitiveservices-personalizer/azure.cognitiveservices.personalizer.personalizer_client.personalizerclient?view=azure-python) que se autentica en Azure mediante Microsoft.Rest.ServiceClientCredentials, que contiene la clave.

Para solicitar el mejor elemento del contenido, cree un elemento [RankRequest](https://docs.microsoft.com/python/api/azure-cognitiveservices-personalizer/azure.cognitiveservices.personalizer.models.rankrequest?view=azure-python) y, luego, páselo al método client.Rank. El método Rank devuelve un elemento RankResponse.

Para enviar una puntuación de recompensa a Personalizer, establezca el identificador de evento y la puntuación de recompensa (valor) que se enviarán al método [Reward](https://docs.microsoft.com/python/api/azure-cognitiveservices-personalizer/azure.cognitiveservices.personalizer.operations.events_operations.eventsoperations?view=azure-python#reward-event-id--value--custom-headers-none--raw-false----operation-config-) en la clase EventOperations.

La determinación de la recompensa en este inicio rápido es trivial. En un sistema de producción, la determinación de lo que afecta a la [puntuación de recompensa](../concept-rewards.md) y cuánto le afecta, puede ser un proceso complejo que decida cambiar con el tiempo. Esta debe ser una de las decisiones de diseño principales en la arquitectura de su instancia de Personalizer.

## <a name="code-examples"></a>Ejemplos de código

Estos fragmentos de código muestran cómo realizar las siguientes acciones con la biblioteca cliente de Personalizer para Python:

* [Creación de un cliente de Personalizer](#create-a-personalizer-client)
* [API Rank](#request-the-best-action)
* [API Reward](#send-a-reward)

## <a name="create-a-new-python-application"></a>Creación de una nueva aplicación de Python

Cree una aplicación de Python en el IDE o editor que prefiera con el nombre `sample.py`.

## <a name="add-the-dependencies"></a>Adición de las dependencias

En el directorio del proyecto, abra el archivo **sample.py** en el editor o IDE que prefiera. Agregue la siguiente línea de código:

[!code-python[Add module dependencies](~/samples-personalizer/quickstarts/python/sample.py?name=Dependencies)]

## <a name="add-personalizer-resource-information"></a>Incorporación de la información de recursos de Personalizer

Cree variables para la clave y el punto de conexión de Azure del recurso extraídos de las variables de entorno, denominadas `PERSONALIZER_RESOURCE_KEY` y `PERSONALIZER_RESOURCE_ENDPOINT`. Si ha creado las variables de entorno una vez iniciada la aplicación, el editor, IDE o shell que se esté ejecutando se tendrá que cerrar y volver a cargar para acceder a la variable. Los métodos se crearán más adelante en este inicio rápido.

El nombre del recurso forma parte de la dirección URL del punto de conexión: `https://<your-resource-name>.api.cognitive.microsoft.com/`.

[!code-python[Create variables to hold the Personalizer resource key and endpoint values found in the Azure portal.](~/samples-personalizer/quickstarts/python/sample.py?name=AuthorizationVariables)]

## <a name="create-a-personalizer-client"></a>Creación de un cliente de Personalizer

A continuación, cree un método para devolver un cliente de Personalizer. El parámetro para el método es `PERSONALIZER_RESOURCE_ENDPOINT` y el elemento ApiKey es `PERSONALIZER_RESOURCE_KEY`.

[!code-python[Create the Personalizer client](~/samples-personalizer/quickstarts/python/sample.py?name=Client)]

## <a name="get-content-choices-represented-as-actions"></a>Obtención de opciones de contenido representadas como acciones

Las acciones representan las opciones de contenido entre las que quiere que Personalizer seleccione el mejor elemento de contenido. Agregue los métodos siguientes a la clase Program para representar el conjunto de acciones y sus características.

[!code-python[Present time out day preference to the user](~/samples-personalizer/quickstarts/python/sample.py?name=getActions)]

[!code-python[Present time out day preference to the user](~/samples-personalizer/quickstarts/python/sample.py?name=createUserFeatureTimeOfDay)]

[!code-python[Present food taste preference to the user](~/samples-personalizer/quickstarts/python/sample.py?name=createUserFeatureTastePreference)]

## <a name="create-the-learning-loop"></a>Creación del bucle de aprendizaje

El bucle de aprendizaje de Personalizer es un ciclo de llamadas [Rank](#request-the-best-action) y [Reward](#send-a-reward). En este inicio rápido, cada llamada Rank para personalizar el contenido, va seguida de una llamada Reward para indicar a Personalizer cómo es de eficaz el funcionamiento del servicio.

El siguiente código pasa por un ciclo en el que se pregunta al usuario sus preferencias en la línea de comandos, se envía esa información a Personalizer para que seleccione la mejor acción, se presenta la selección al cliente para que elija de la lista y, después, se envía una recompensa a Personalizer en la que se señala el grado de acierto que tuvo el servicio al clasificar la selección.

[!code-python[The Personalizer learning loop ranks the request.](~/samples-personalizer/quickstarts/python/sample.py?name=mainLoop&highlight=9,10,29)]

Agregue los métodos siguientes, que [obtienen las opciones de contenido](#get-content-choices-represented-as-actions), antes de ejecutar el archivo de código:

* `get_user_preference`
* `get_user_timeofday`
* `get_actions`

## <a name="request-the-best-action"></a>Solicitud de la mejor acción


Para completar la solicitud Rank, el programa solicita las preferencias del usuario para crear un elemento `currentContent` de las opciones de contenido. El proceso puede crear contenido para excluir de las acciones, que se muestra como `excludeActions`. Para recibir la respuesta, la solicitud Rank necesita las acciones y sus características, las características de currentContext, excludeActions y un identificador de evento único.

Este inicio rápido tiene características de contexto simples de hora del día y preferencias alimentarias del usuario. En los sistemas de producción, la determinación y [evaluación](../concept-feature-evaluation.md) de [acciones y características](../concepts-features.md) no es una cuestión trivial.

[!code-python[The Personalizer learning loop ranks the request.](~/samples-personalizer/quickstarts/python/sample.py?name=rank)]

## <a name="send-a-reward"></a>Envío de una recompensa


Para obtener la puntuación de recompensa que se enviará en la solicitud Reward, el programa obtiene la selección del usuario desde la línea de comandos, asigna un valor numérico a la selección y, después, envía el identificador único del evento y la puntuación de recompensa como un valor numérico a API Reward.

En este inicio rápido se asigna un número simple como puntuación de recompensa: 0 o 1. En sistemas de producción, determinar cuándo y qué enviar a la llamada [Reward](../concept-rewards.md) puede ser más complicado, en función de las necesidades específicas.

[!code-python[The Personalizer learning loop sends a reward.](~/samples-personalizer/quickstarts/python/sample.py?name=reward&highlight=9)]

## <a name="run-the-program"></a>Ejecución del programa

Ejecute la aplicación con el comando de Python desde el directorio de aplicación.

```console
python sample.py
```

![El programa del inicio rápido realiza dos preguntas para recopilar las preferencias del usuario, conocidas como características, y, después, proporciona la acción principal.](../media/csharp-quickstart-commandline-feedback-loop/quickstart-program-feedback-loop-example.png)