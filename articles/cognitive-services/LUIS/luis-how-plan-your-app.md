---
title: 'Planificación de la aplicación: LUIS'
description: Resuma las intenciones y entidades de aplicación relevantes y cree los planes de aplicación en Language Understanding Intelligent Services (LUIS).
ms.topic: conceptual
ms.date: 04/14/2020
ms.openlocfilehash: dfed27a05973a2ea2e9a97eaa1c233b847b33d87
ms.sourcegitcommit: ea006cd8e62888271b2601d5ed4ec78fb40e8427
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 04/14/2020
ms.locfileid: "81382302"
---
# <a name="plan-your-luis-app-schema-with-subject-domain-and-data-extraction"></a>Planeamiento del esquema de aplicación de LUIS con el dominio de sujeto y la extracción de datos

Un esquema de aplicación de LUIS contiene [intenciones](luis-glossary.md#intent) y [entidades](luis-glossary.md#entity) de interés para el [dominio](luis-glossary.md#domain) de sujeto. Las intenciones clasifican las [expresiones](luis-glossary.md#utterance) de usuario y las entidades extraen los datos de esas expresiones.

## <a name="identify-your-domain"></a>Identificar el dominio

Una aplicación de LUIS se centra en un dominio de sujeto. Por ejemplo, podría darse el caso de que tuviera una aplicación de viajes que controla la reserva de billetes, vuelos, hoteles y alquiler de vehículos. Otra aplicación podría proporcionar contenido relacionado con el ejercicio, el seguimiento de resultados de fitness y el establecimiento de objetivos. Identificar el dominio le ayuda a buscar palabras o frases que son importantes para su dominio.

> [!TIP]
> LUIS ofrece [dominios creados previamente](luis-how-to-use-prebuilt-domains.md) para muchos escenarios comunes. Compruebe si puede usar un dominio creado previamente como punto de partida para su aplicación.

## <a name="identify-your-intents"></a>Identificar las intenciones

Piense en las [intenciones](luis-concept-intent.md) que son importantes para la tarea de la aplicación.

Veamos el ejemplo de una aplicación de viajes, que dispone de funciones para reservar un vuelo y consultar el tiempo del destino del usuario. Puede definir las intenciones `BookFlight` y `GetWeather` de estas acciones.

En una aplicación más compleja con más funciones, tiene más intenciones, así que debe definirlas cuidadosamente para que no sean demasiado específicas. Por ejemplo, es posible que `BookFlight` y `BookHotel` deban ser intenciones independientes, pero que `BookInternationalFlight` y `BookDomesticFlight` sean demasiado parecidas.

> [!NOTE]
> Se recomienda usar únicamente las intenciones que necesite para llevar a cabo las funciones de la aplicación. Si define demasiadas intenciones, a LUIS le resultará más difícil clasificar las expresiones correctamente. Si define demasiado pocas, podrían ser tan generales que se superpongan.

Si no necesita identificar la intención general del usuario, agregue todas las expresiones de usuario de ejemplo a la intención `None`. Si la aplicación necesita más intenciones, puede crearlas más adelante.

## <a name="create-example-utterances-for-each-intent"></a>Creación de expresiones de ejemplo para cada intención

Para empezar, evite crear demasiadas expresiones para cada intento. Cuando haya determinado las intenciones, cree 15 o 30 expresiones de ejemplo por intención. Cada expresión debe ser diferente de la expresión anteriormente proporcionada. Una buena variedad de expresiones incluyen el recuento total de palabras, la elección de las palabras, el tiempo verbal y la [puntuación](luis-reference-application-settings.md#punctuation-normalization).

Para más información, consulte [Comprender cuáles son las expresiones correctas para la aplicación de LUIS](luis-concept-utterance.md).

## <a name="identify-your-entities"></a>Identificar las entidades

En las expresiones de ejemplo, identifique las entidades que desea extraer. Para reservar un vuelo necesita información como el destino, la fecha, la aerolínea, el tipo de billete y la clase del viaje. Cree las entidades para estos tipos de datos y, a continuación, marque las [entidades](luis-concept-entity-types.md) en las expresiones de ejemplo, ya que son importantes para llevar a cabo una intención.

A la hora de determinar las entidades que va a usar en la aplicación, tenga en cuenta que hay distintos tipos de entidades para capturar las relaciones entre los tipos de objetos. [Las entidades de LUIS](luis-concept-entity-types.md) proporcionan más detalles sobre los distintos tipos.

> [!TIP]
> LUIS ofrece [entidades pregeneradas](luis-prebuilt-entities.md) para escenarios comunes de usuarios de conversación. Considere la posibilidad de usar entidades pregeneradas como punto de partida para el desarrollo de aplicaciones.

## <a name="next-steps"></a>Pasos siguientes

> [!div class="nextstepaction"]
> [Aprendizaje del ciclo de desarrollo de LUIS](luis-concept-app-iteration.md)

