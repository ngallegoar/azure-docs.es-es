---
title: 'Planeamiento de la aplicación: QnA Maker'
description: Para planear la aplicación de QnA Maker, es necesario comprender cómo funciona QnA Maker y cómo interactúa con otros servicios de Azure, así como estar familiarizado con algunos conceptos de las bases de conocimiento.
ms.topic: conceptual
ms.date: 07/2/2020
ms.openlocfilehash: d19ec51aec7e71b6f040a03543f72af3aed09556
ms.sourcegitcommit: 9b5c20fb5e904684dc6dd9059d62429b52cb39bc
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 07/02/2020
ms.locfileid: "85875388"
---
# <a name="plan-your-qna-maker-app"></a>Planeamiento de la aplicación de QnA Maker

Para planear la aplicación de QnA Maker, es necesario comprender cómo funciona QnA Maker y cómo interactúa con otros servicios de Azure, así como estar familiarizado con algunos conceptos de las bases de conocimiento.

## <a name="azure-resources"></a>Recursos de Azure

Cada [recurso de Azure](azure-resources.md#resource-purposes) creado con QnA Maker tiene un propósito específico. Dado que cada recurso tiene su propia finalidad, límites y [plan de tarifa](azure-resources.md#pricing-tier-considerations), es importante comprender lo que hacen estos recursos como parte del proceso de planeamiento.

|Recurso|Propósito|
|--|--|
| Recurso de [QnA Maker](azure-resources.md#qna-maker-resource)|Creación y predicción de consultas|
| Recurso de [Cognitive Search](azure-resources.md#cognitive-search-resource)|Almacenamiento y búsqueda de datos|
| Recurso de [App Service y plan de App Service](azure-resources.md#app-service-and-app-service-plan)|Consulta de un punto de conexión de predicción|
| Recurso de [Application Insights](azure-resources.md#application-insights)|Telemetría de predicción de consultas|

### <a name="resource-planning"></a>Planeamiento de recursos

Mientras se familiariza con la creación y la predicción de consultas, puede usar el nivel gratis (`F0`) de cada recurso para disfrutar de la experiencia de creación y predicción de consultas. Cuando pase a un escenario de producción activo, vuelva a evaluar la selección de recursos.

#### <a name="qna-maker-resource"></a>Recurso QnA Maker

Un solo recurso de QnA Maker puede hospedar más de una base de conocimiento. El número de bases de conocimiento está determinado por la cantidad de índices admitidos en el plan de tarifa de Cognitive Search. Obtenga más información sobre la [relación de los índices con las bases de conocimiento](azure-resources.md#index-usage).

#### <a name="knowledge-base-size-and-throughput"></a>Tamaño y rendimiento de la base de conocimiento

Cuando vaya a compilar una aplicación real, planee los recursos para el tamaño de la base de conocimiento y las solicitudes de predicción de consultas que espere.

El tamaño de la base de conocimiento lo controlan los siguientes elementos:
* Los límites del plan de tarifa del [recurso de Cognitive Search](../../../search/search-limits-quotas-capacity.md)
* Los [límites de QnA Maker](../limits.md)

La solicitud de predicción de consultas de la base de conocimiento se controla mediante la aplicación web y el plan de la aplicación web. Consulte la [configuración recomendada](azure-resources.md#recommended-settings) para decidir el plan de tarifa.

### <a name="resource-sharing"></a>Uso compartido de recursos

Si ya tiene algunos de estos recursos en uso, puede considerar la posibilidad de compartirlos. Aunque algunos recursos [pueden compartirse](azure-resources.md#share-services-with-qna-maker), se trata de un escenario avanzado.

Todas las bases de conocimiento creadas en el mismo recurso de QnA Maker comparten el punto de conexión de predicción de consultas de **prueba**.

### <a name="understanding-impact-of-resource-selection"></a>Descripción del impacto de la selección de recursos

Para seleccionar los recursos de manera adecuada, la base de conocimiento debe responder a las predicciones de consultas correctamente.

Si la base de conocimiento no funciona de forma correcta, el problema suele deberse a una administración de recursos inadecuada.

En el caso de una selección de recursos incorrecta, es necesario investigar para determinar qué [recurso debe cambiar](azure-resources.md#when-to-change-a-pricing-tier).

## <a name="knowledge-bases"></a>Bases de conocimiento

Una base de conocimiento está vinculada directamente a su recurso de QnA Maker y contiene los pares de preguntas y respuestas (QnA) que se usan para responder a las solicitudes de predicción de consultas.

### <a name="language-considerations"></a>Consideraciones de idioma

La primera base de conocimiento que se crea en el recurso de QnA Maker establece el idioma del recurso. Solo se puede tener un idioma para un recurso de QnA Maker.

Estructure los recursos de QnA Maker por idioma o use el [Traductor](../../translator/translator-info-overview.md) para cambiar una consulta de otro idioma al de la base de conocimiento antes de enviar la consulta al punto de conexión de predicción de consultas.

### <a name="ingesting-data-sources"></a>Ingesta de orígenes de datos

Los [orígenes de datos](knowledge-base.md) ingeridos, que se usan para crear una base de conocimiento, pueden ser uno de los siguientes:

* URL pública
* URL privada de SharePoint
* Archivo

El proceso de ingesta convierte los [tipos de contenido admitidos](content-types.md) a Markdown. La edición posterior de la *respuesta* se realiza con Markdown. Después de crear la base de conocimiento, puede editar los [pares de preguntas y respuestas](question-answer-set.md) en el portal de QnA Maker mediante la [creación de texto enriquecido](../how-to/edit-knowledge-base.md#rich-text-editing-for-answer).

### <a name="data-format-considerations"></a>Consideraciones sobre el formato de los datos

Dado que el formato final de un par de preguntas y respuestas es Markdown, es importante entender la [compatibilidad con Markdown](../reference-markdown-format.md).

Las imágenes vinculadas deben estar disponibles desde una dirección URL pública para que se puedan mostrar en el panel prueba del portal de QnA Maker, así como en cualquier aplicación cliente, ya que QnA Maker no proporciona autenticación para el contenido, incluidas las imágenes.

### <a name="bot-personality"></a>Personalidad de bot

Agregue una personalidad de bot a la base de conocimiento con la función de [charla](../how-to/chit-chat-knowledge-base.md). Esta personalidad incluye respuestas en un determinado tono de conversación, como *profesional* y *amistoso*. La función de charla se proporciona como un conjunto conversacional, que puede agregar, editar y quitar como le convenga.

Se recomienda una personalidad de bot si el bot se conecta a la base de conocimiento. Si se va a conectar a varios servicios, uno de los cuales es la base de conocimiento, puede optar por seguir usando la función de charla en la base de conocimiento, pero debe revisar cómo interactúa el servicio de bot para saber si el diseño de la arquitectura es el correcto para su uso.

### <a name="conversation-flow-with-a-knowledge-base"></a>Flujo de conversación con una base de conocimiento

Normalmente, el flujo de conversación empieza con un saludo por parte de un usuario, como `Hi` o `Hello`. La base de conocimiento puede responder de manera general, como `Hi, how can I help you`, y mostrar una serie de mensajes de seguimiento para continuar con la conversación.

Debe diseñar el flujo de conversación con un bucle en mente para que el usuario sepa cómo usar el bot y que este no lo abandone durante la conversación. Los [mensajes de seguimiento](../how-to/multiturn-conversation.md) proporcionan vinculación entre los pares de preguntas y respuestas, que hacen posible el flujo de conversación.

### <a name="authoring-with-collaborators"></a>Creación con colaboradores

Los colaboradores podrían ser otros desarrolladores que comparten la pila de desarrollo completa de la aplicación de base de conocimiento, o tal vez se limiten a crear la base de conocimiento.

La creación de la base de conocimiento admite varios [permisos de acceso basado en roles](../how-to/collaborate-knowledge-base.md) que se apliquen en Azure Portal para limitar el ámbito de las capacidades de un colaborador.

## <a name="integration-with-client-applications"></a>Integración con aplicaciones cliente

La integración con [aplicaciones cliente](integration-with-other-applications.md) implica enviar una consulta al punto de conexión del tiempo de ejecución de predicción. Se envía una consulta a la base de conocimiento específica con una solicitud basada en REST o en el SDK al punto de conexión de la aplicación web de QnA Maker.

Para autenticar correctamente una solicitud de cliente, la aplicación cliente debe enviar las credenciales correctas y el identificador de la base de conocimiento. Si usa Azure Bot Service, configure la opción como parte de la configuración del bot en Azure Portal.

### <a name="conversation-flow-in-a-client-application"></a>Flujo de conversación en una aplicación cliente

El flujo de conversación en una [aplicación cliente](integration-with-other-applications.md), como un bot de Azure, puede requerir alguna funcionalidad antes y después de interactuar con la base de conocimiento.

Si la aplicación cliente es compatible con el flujo de conversación, ya sea a través de métodos alternativos para controlar los mensajes de seguimiento o mediante la función de charla, debe diseñar estos en una fase inicial y asegurarse de que la consulta que usa la aplicación cliente se administra correctamente, ya sea con otro servicio o por medio de su envío a la base de conocimiento.

### <a name="dispatching-between-qna-maker-and-language-understanding-luis"></a>Envío entre QnA Maker y Language Understanding (LUIS)

Una aplicación cliente puede proporcionar varias características, pero la base de conocimiento solo responde a una de estas. Las demás características tendrán que entender el texto de la conversación y extraer su significado.

Una arquitectura de aplicación cliente habitual consiste en usar juntos QnA Maker y [Language Understanding (LUIS)](../../LUIS/what-is-luis.md). LUIS ofrece la clasificación y la extracción de texto de cualquier consulta, incluso a otros servicios, mientras que QnA Maker proporciona respuestas de la base de conocimiento.

En una [arquitectura compartida](../choose-natural-language-processing-service.md), el envío entre los dos servicios se realiza con la herramienta [Dispatch](https://github.com/Microsoft/botbuilder-tools/tree/master/packages/Dispatch) de Bot Framework.

### <a name="active-learning-from-a-client-application"></a>Aprendizaje activo desde una aplicación cliente

QnA Maker usa el _aprendizaje activo_ para mejorar la base de conocimiento al sugerir preguntas alternativas a una respuesta. La aplicación cliente es responsable de una parte de este [aprendizaje activo](active-learning-suggestions.md). A través de mensajes de carácter conversacional, la aplicación cliente puede determinar que la respuesta que se devuelve desde la base de conocimiento no era la que el usuario buscaba y, de este modo, elegir la mejor respuesta. La aplicación cliente debe [enviar esta información de nuevo a la base de conocimiento](active-learning-suggestions.md#how-you-give-explicit-feedback-with-the-train-api) para mejorar la calidad de la predicción.

### <a name="providing-a-default-answer"></a>Aportación de una respuesta predeterminada

Si la base de conocimiento no encuentra una respuesta, devuelve la _respuesta predeterminada_. Esta respuesta se puede configurar desde el portal de QnA Maker, en la página **Configuración** o las [API](https://docs.microsoft.com/rest/api/cognitiveservices/qnamaker/knowledgebase/update#request-body).

Esta respuesta predeterminada es diferente de la que ofrece el bot de Azure. La respuesta predeterminada del bot de Azure se configura en Azure Portal para el bot como parte de la configuración y se devuelve cuando no se cumple el umbral de puntuación.

## <a name="prediction"></a>Predicción

La predicción es la respuesta de la base de conocimiento e incluye más información que simplemente la respuesta. Para obtener una respuesta de predicción de consultas, use la [API GeneateAnswer](query-knowledge-base.md).

### <a name="prediction-score-fluctuations"></a>Fluctuaciones de la puntuación de predicción

Una puntuación puede cambiar en función de varios factores:

* El número de respuestas solicitadas para [GenerateAnswer](query-knowledge-base.md) con la propiedad `top`
* La variedad de preguntas alternativas disponibles
* El filtrado de metadatos
* La consulta enviada a la base de conocimiento `test` o `production`

Hay una [clasificación de las respuestas en dos fases](query-knowledge-base.md#how-qna-maker-processes-a-user-query-to-select-the-best-answer):
* Cognitive Search (primera clasificación): para que se obtenga una respuesta de Cognitive Search, el número de _respuestas permitidas_ debe ser lo bastante alto como para que Cognitive Search devuelva las mejores respuestas, de modo que pasen al clasificador de QnA Maker.
* QnA Maker (segunda clasificación): se aplica la caracterización y el aprendizaje automático para determinar la mejor respuesta.

### <a name="service-updates"></a>Actualizaciones del servicio

Las actualizaciones de servicio se administran automáticamente mediante la aplicación de las [actualizaciones más recientes del entorno de ejecución](../how-to/set-up-qnamaker-service-azure.md#get-the-latest-runtime-updates).

### <a name="scaling-throughput-and-resiliency"></a>Escalado, rendimiento y resistencia

El escalado, el rendimiento y la resistencia se determinan mediante los [recursos de Azure](../how-to/set-up-qnamaker-service-azure.md), los planes de tarifa y cualquier arquitectura circundante, como [Traffic Manager](../how-to/set-up-qnamaker-service-azure.md#business-continuity-with-traffic-manager).

### <a name="analytics-with-application-insights"></a>Análisis con Application Insights

Todas las consultas a la base de conocimiento se almacenan en Application Insights. Use nuestras [principales consultas](../how-to/get-analytics-knowledge-base.md) para entender las métricas.

## <a name="development-lifecycle"></a>Ciclo de vida del desarrollo

El [ciclo de vida del desarrollo](development-lifecycle-knowledge-base.md) de una base de conocimiento es constante, ya que consta de la edición, la prueba y la publicación de la base de conocimiento.

### <a name="knowledge-base-development-of-qna-maker-pairs"></a>Desarrollo de pares de QnA Maker de la base de conocimiento

Los [pares de preguntas y respuestas](question-answer-set.md) deben diseñarse y desarrollarse en función del uso de la aplicación cliente.

Cada par puede contener:
* Metadatos: pueden filtrarse al realizar consultas. Esto le permite etiquetar los pares de preguntas y respuestas con información adicional sobre el origen, el contenido, el formato y el propósito de los datos.
* Mensajes de seguimiento: determine una ruta en la base de conocimiento para que el usuario llegue a la respuesta correcta.
* Preguntas alternativas: son importantes para permitir que la búsqueda haga coincidir la respuesta con las diversas formas de la pregunta. Las [sugerencias de aprendizaje activo](active-learning-suggestions.md) se convierten en preguntas alternativas.

### <a name="devops-development"></a>Desarrollo de DevOps

El desarrollo de una base de conocimiento que se va a insertar en una canalización de DevOps requiere que dicha base de conocimiento esté aislada durante las [pruebas por lotes](../quickstarts/batch-testing.md).

Una base de conocimiento comparte el índice de Cognitive Search con todas las demás bases de conocimiento del recurso de QnA Maker. Aunque la base de conocimiento está aislada por partición, si se comparte el índice, se puede producir una diferencia en la puntuación cuando se compara con la base de conocimiento publicada.

Para tener la _misma puntuación_ en las bases de conocimiento `test` y `production`, aísle un recurso de QnA Maker en una única base de conocimiento. En esta arquitectura, el recurso solo debe persistir mientras dure la prueba por lotes aislada.

## <a name="next-step"></a>Paso siguiente

* [Recursos de Azure](../how-to/set-up-qnamaker-service-azure.md)
* [Pares de preguntas y respuestas](question-answer-set.md)