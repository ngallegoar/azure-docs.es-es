---
title: Programación de tareas y flujos de trabajo periódicos en Azure Logic Apps
description: Información general sobre la programación de tareas, procesos y flujos de trabajo automatizados y periódicos con Azure Logic Apps
services: logic-apps
ms.suite: integration
ms.reviewer: deli, jonfan, logicappspm
ms.topic: conceptual
ms.date: 03/25/2020
ms.openlocfilehash: 6d00c7d7cc88427a3500b28891ec70bb8a4bbb43
ms.sourcegitcommit: ac4a365a6c6ffa6b6a5fbca1b8f17fde87b4c05e
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 05/10/2020
ms.locfileid: "83005203"
---
# <a name="schedule-and-run-recurring-automated-tasks-processes-and-workflows-with-azure-logic-apps"></a>Programación y ejecución de tareas, procesos y flujos de trabajo automatizados y periódicos con Azure Logic Apps

Logic Apps le ayuda a crear y ejecutar tareas y procesos periódicos y automatizados según una programación. Al crear un flujo de trabajo de aplicación lógica que comienza con un desencadenador de periodicidad o un integrador de ventana deslizante integrado, que son desencadenadores del tipo programación, puede ejecutar tareas inmediatamente, en un momento posterior o en un intervalo periódico. Puede llamar a servicios dentro y fuera de Azure, como los puntos de conexión HTTP o HTTPS, publicar mensajes en servicios de Azure, como Azure Storage y Azure Service Bus, u obtener archivos cargados en un recurso compartido de archivos. Con el desencadenador de periodicidad, también puede configurar programaciones complejas y periodicidades avanzadas para ejecutar tareas. Para más información acerca de los desencadenadores y las acciones de programación integrados, consulte [Desencadenadores de programación](#schedule-triggers) y [Acciones de programación](#schedule-actions). 

> [!TIP]
> Puede programar y ejecutar cargas de trabajo periódicas sin necesidad de crear una aplicación lógica independiente para cada trabajo programado y ejecutarla en el [límite de flujos de trabajo por región y suscripción](../logic-apps/logic-apps-limits-and-config.md#definition-limits). En su lugar, puede usar el patrón de la aplicación lógica que se crea mediante [la plantilla de inicio rápido de Azure: programador de trabajos de Logic Apps](https://github.com/Azure/azure-quickstart-templates/tree/master/301-logicapps-jobscheduler/).
>
> La plantilla del programador de trabajos de Logic Apps crea una aplicación lógica CreateTimerJob que llama a una aplicación lógica TimerJob. Luego, puede llamar a la aplicación lógica CreateTimerJob como una API, para lo que debe realizar una solicitud HTTP y usar una programación como entrada de dicha solicitud. En cada llamada a la aplicación lógica CreateTimerJob se llama también a la aplicación lógica TimerJob, que crea una instancia de TimerJob que se ejecuta continuamente según la programación especificada, o hasta que se alcance un límite especificado. De ese modo, puede ejecutar tantas instancias de TimerJob como desee sin preocuparse por los límites del flujo de trabajo, ya que las instancias no son definiciones o recursos individuales del flujo de trabajo de la aplicación lógica.

En esta lista se muestran varias tareas de ejemplo que se pueden ejecutar con los desencadenadores de programación integrados:

* Obtenga datos internos, por ejemplo, mediante la ejecución de un procedimiento almacenado de SQL todos los días.

* Obtenga datos externos, por ejemplo, mediante la extracción de informes meteorológicos de NOAA cada 15 minutos.

* Envíe datos de informes, por ejemplo, un correo electrónico con un resumen de todos los pedidos de la semana anterior que superen una cantidad específica.

* Procese datos, por ejemplo, comprima las imágenes cargadas en el día actual todos los días laborables durante las horas que no sean punta.

* Limpie datos, por ejemplo, elimine todos los tweets que tengan más de tres meses.

* Archive datos, por ejemplo, inserte facturas a un servicio de copia de seguridad cada día a la 1:00 durante los próximos nueve meses.

También puede usar las acciones integradas de programación para pausar el flujo de trabajo antes de que se ejecute la siguiente acción, por ejemplo:

* Esperar hasta un día de la semana para enviar una actualización de estado por correo electrónico.

* Retrasar el flujo de trabajo hasta que una llamada HTTP tenga tiempo para completarse antes de reanudarse y recuperar el resultado.

En este artículo se describen las funcionalidades de los desencadenadores y las acciones de programación integrados.

<a name="schedule-triggers"></a>

## <a name="schedule-triggers"></a>Programación de desencadenadores

Puede iniciar el flujo de trabajo de la aplicación lógica mediante el desencadenador de periodicidad o el de ventana deslizante, que no están asociados con ningún servicio o sistema específicos. Estos desencadenadores inician y ejecutan su flujo de trabajo en función de la periodicidad que especifica, en la que debe seleccionar el intervalo y la frecuencia, como el número de segundos, minutos, horas, días, semanas o meses. También puede establecer la fecha de inicio y la hora, así como la zona horaria. Cada vez que se activa un desencadenador, Logic Apps crea una instancia de flujo de trabajo para la aplicación lógica y la ejecuta.

Las diferencias entre estos desencadenadores son las siguientes:

* **Periodicidad**: su flujo de trabajo se ejecuta en intervalos de tiempo regulares según la programación especificada. Si se pierden las periodicidades, el desencadenador de periodicidad no procesa las periodicidades perdidas, sino que las reinicia con el siguiente intervalo programado. También puede establecer una fecha de inicio y una hora, así como la zona horaria. Si selecciona "Día", puede especificar horas del día y los minutos de la hora, por ejemplo, cada día a las 2:30. Si selecciona "Semana", también puede seleccionar los días de la semana, por ejemplo, el miércoles y el sábado. Para obtener más información, consulte [Creación, programación y ejecución de tareas y flujos de trabajo periódicos con el desencadenador de periodicidad](../connectors/connectors-native-recurrence.md).

* **Ventana deslizante**: su flujo de trabajo se ejecuta a intervalos de tiempo regulares que administran los datos en fragmentos continuos. Si se pierden las periodicidades, el desencadenador de ventana deslizante retrocede y procesa las repeticiones perdidas. Puede especificar una fecha de inicio y una hora, la zona horaria y una duración para retrasar cada periodicidad del flujo de trabajo. Este desencadenador no admite programaciones avanzadas; por ejemplo, las horas específicas del día, los minutos de la hora y los días de la semana. Para obtener más información, consulte [Creación, programación y ejecución de tareas y flujos de trabajo periódicos con el desencadenador de ventana deslizante](../connectors/connectors-native-sliding-window.md).

<a name="schedule-actions"></a>

## <a name="schedule-actions"></a>Programación de acciones

Después de realizar cualquier acción en el flujo de trabajo de la aplicación lógica, puede usar las acciones Retraso y Retraso hasta para que el flujo de trabajo espere antes de que se ejecute la siguiente acción.

* **Retraso**: permite esperar durante el número especificado de unidades de tiempo, como segundos, minutos, horas, días, semanas o meses, antes de ejecutar la siguiente acción. Para obtener más información, consulte [Delay the next action in workflows](../connectors/connectors-native-delay.md) (Retraso de la acción siguiente en los flujos de trabajo).

* **Retraso hasta**: permite esperar hasta la fecha y hora especificadas antes de ejecutar la siguiente acción. Para obtener más información, consulte [Delay the next action in workflows](../connectors/connectors-native-delay.md) (Retraso de la acción siguiente en los flujos de trabajo).

## <a name="patterns-for-start-date-and-time"></a>Patrones para la fecha y hora de inicio

<a name="start-time"></a>

Estos son algunos patrones que muestran no solo cómo se puede controlar la periodicidad con la fecha y hora de inicio, sino también la forma en que el servicio Logic Apps ejecuta estas periodicidades:

| Hora de inicio | Periodicidad sin programación | Periodicidad con programación (desencadenador de periodicidad solo) |
|------------|-----------------------------|----------------------------------------------------|
| {none} | Ejecuta la primera carga de trabajo al instante. <p>Ejecuta futuras cargas de trabajo en función de la hora de la última ejecución. | Ejecuta la primera carga de trabajo al instante. <p>Ejecuta futuras cargas de trabajo en función de la programación especificada. |
| Hora de inicio en el pasado | Desencadenador de **periodicidad**: Calcula los tiempos de ejecución en función de la hora de inicio especificada y descarta las horas de ejecución anteriores. Ejecuta la primera carga de trabajo la hora de la próxima ejecución. <p>Ejecuta futuras cargas de trabajo en función de los cálculos de la hora de la última ejecución. <p><p>Desencadenador de **ventana deslizante**: Calcula los tiempos de ejecución en función de la hora de inicio especificada y respeta las horas de ejecución anteriores. <p>Ejecuta futuras cargas de trabajo en función de los cálculos de la hora de inicio especificada. <p><p>Para más información, vea el ejemplo a continuación de esta tabla. | Ejecuta la primera carga de trabajo *no antes* de la hora de inicio, en función de la programación calculada a partir de la hora de inicio. <p>Ejecuta futuras cargas de trabajo en función de la programación especificada. <p>**Nota:** Si especifica una periodicidad con una programación, pero no especifica horas o minutos para la programación, las horas de la futura ejecución se calculan usando las horas o los minutos, respectivamente, a partir de la hora de la primera ejecución. |
| Hora de inicio en la actualidad o en el futuro | Ejecuta la primera carga de trabajo a la hora de inicio especificada. <p>Ejecuta futuras cargas de trabajo en función de los cálculos de la hora de la última ejecución. | Ejecuta la primera carga de trabajo *no antes* de la hora de inicio, en función de la programación calculada a partir de la hora de inicio. <p>Ejecuta futuras cargas de trabajo en función de la programación especificada. <p>**Nota:** Si especifica una periodicidad con una programación, pero no especifica horas o minutos para la programación, las horas de la futura ejecución se calculan usando las horas o los minutos, respectivamente, a partir de la hora de la primera ejecución. |
||||

> [!IMPORTANT]
> Cuando las periodicidades no especifican opciones de programación avanzadas, las futuras periodicidades se basan en la hora de la última ejecución.
> Las horas de inicio de estas periodicidades pueden cambiar debido a factores como la latencia durante las llamadas de almacenamiento. Para asegurarse de que la aplicación lógica no pierde una periodicidad, especialmente cuando la frecuencia se especifica en días o unidades superiores, use una de estas opciones:
> 
> * Proporcione una hora de inicio para la periodicidad.
> 
> * Especifique las horas y los minutos en que se ejecutará la periodicidad mediante las propiedades **A estas horas** y **En estos minutos**.
> 
> * Use el [desencadenador de ventana deslizante](../connectors/connectors-native-sliding-window.md), en lugar del de periodicidad.

*Ejemplo de una hora de inicio anterior con periodicidad, pero sin programación*

Imagine que la fecha y hora actuales son 8 de septiembre de 2017 y la 1:00 p. m. Especifique la fecha y hora de inicio 7 de septiembre de 2017 a las 2:00 p. m., que se encuentra en el pasado, y una periodicidad de ejecución cada dos días.

| Hora de inicio | Hora actual | Periodicidad | Programación |
|------------|--------------|------------|----------|
| 2017-09-**07**T14:00:00Z <br>(**07**-09-2017a las 2:00 p. m) | 2017-09-**08**T13:00:00Z <br>(**08**-09-2017 a la 1:00 p. m.) | Cada dos días | {none} |
|||||

En el caso del desencadenador de periodicidad, el motor de Logic Apps calcula las horas de ejecución en función de la hora de inicio, descarta las horas de ejecución anteriores, usa la siguiente hora de inicio futura para la primera ejecución y calcula las ejecuciones futuras en función de la última hora de ejecución.

Este es el aspecto de esta periocidad:

| Hora de inicio | Hora de la primera ejecución | Hora de las futuras ejecuciones |
|------------|----------------|------------------|
| **07**-09-2017 a las 2:00 p. m. | **09**-09-2017 a las 2:00 p. m. | **11**-09-2017 a las 2:00 p. m. </br>**13**-09-2017 a las 2:00 p. m. </br>**15**-09-2017 a las 2:00 p. m. </br>y así sucesivamente. |
||||

Por tanto, no importa el tiempo que haya transcurrido desde que especificó la hora de inicio; por ejemplo, **05**-09-2017 a las 2:00 p. m. o **01**-09-2017 a las 2:00 p. m., ya que su primera ejecución siempre usa la siguiente hora de inicio.

En el caso del desencadenador de ventana deslizante, el motor de Logic Apps calcula las horas de ejecución en función de la hora de inicio, respeta las horas de ejecución anteriores, usa la hora de inicio para la primera ejecución y calcula las ejecuciones futuras en función de la hora de inicio.

Este es el aspecto de esta periocidad:

| Hora de inicio | Hora de la primera ejecución | Hora de las futuras ejecuciones |
|------------|----------------|------------------|
| **07**-09-2017 a las 2:00 p. m. | **07**-09-2017 a las 2:00 p. m. | **09**-09-2017 a las 2:00 p. m. </br>**11**-09-2017 a las 2:00 p. m. </br>**13**-09-2017 a las 2:00 p. m. </br>**15**-09-2017 a las 2:00 p. m. </br>y así sucesivamente. |
||||

Por tanto, no importa el tiempo que haya transcurrido desde que especificó la hora de inicio; por ejemplo, **05**-09-2017 a las 2:00 p. m. o **01**-09-2017 a las 2:00 p. m., ya que su primera ejecución siempre usa la hora de inicio especificada.

<a name="example-recurrences"></a>

## <a name="example-recurrences"></a>Ejemplo de periodicidades

A continuación, se incluyen varias periodicidades de ejemplo que puede configurar para los desencadenadores que admiten las opciones:

| Desencadenador | Periodicidad | Intervalo | Frecuencia | Hora de inicio | En estos días | A estas horas | En estos minutos | Nota: |
|---------|------------|----------|-----------|------------|---------------|----------------|------------------|------|
| Periodicidad, <br>Ventana deslizante | Ejecutar cada 15 minutos (ninguna fecha y hora de inicio) | 15 | Minute | {none} | {unavailable} | {none} | {none} | Esta programación se inicia inmediatamente y después calcula las futuras periodicidades en función de la hora de la última ejecución. |
| Periodicidad, <br>Ventana deslizante | Ejecutar cada 15 minutos (con fecha y hora de inicio) | 15 | Minute | *startDate*T*startTime*Z | {unavailable} | {none} | {none} | Esta programación no se inicia *antes* de la fecha y hora de inicio especificadas y después calcula las futuras periodicidades en función de la hora de la última ejecución. |
| Periodicidad, <br>Ventana deslizante | Ejecutar cada hora, a la hora (con fecha y hora de inicio) | 1 | Hour | *startDate*Thh:00:00Z | {unavailable} | {none} | {none} | Esta programación no se inicia *antes* de la fecha y hora de inicio especificadas. Las periodicidades futuras se ejecutan cada hora en la marca de minuto "00", que se calcula a partir de la hora de inicio. <p>Si la frecuencia es "Week" o "Month", esta programación se ejecuta respectivamente solo un día de la semana o un día del mes. |
| Periodicidad, <br>Ventana deslizante | Ejecutar cada hora todos los días (sin fecha y hora de inicio) | 1 | Hour | {none} | {unavailable} | {none} | {none} | Esta programación se inicia inmediatamente y calcula las futuras periodicidades en función de la hora de la última ejecución. <p>Si la frecuencia es "Week" o "Month", esta programación se ejecuta respectivamente solo un día de la semana o un día del mes. |
| Periodicidad, <br>Ventana deslizante | Ejecutar cada hora todos los días (con fecha y hora de inicio) | 1 | Hour | *startDate*T*startTime*Z | {unavailable} | {none} | {none} | Esta programación no se inicia *antes* de la fecha y hora de inicio especificadas y después calcula las futuras periodicidades en función de la hora de la última ejecución. <p>Si la frecuencia es "Week" o "Month", esta programación se ejecuta respectivamente solo un día de la semana o un día del mes. |
| Periodicidad, <br>Ventana deslizante | Ejecutar cada 15 minutos después de la hora, cada hora (con fecha y hora de inicio) | 1 | Hour | *startDate*T00:15:00Z | {unavailable} | {none} | {none} | Esta programación no se inicia *antes* de la fecha y hora de inicio especificadas. Las futuras periodicidades se ejecutan en la marca de minuto "15", que se calcula a partir de la hora de inicio, es decir, a las 00:15 a. m., 1:15 a. m., 2:15 a. m. y así sucesivamente. |
| Periodicidad | Ejecutar cada 15 minutos después de la hora, cada hora (sin fecha y hora de inicio) | 1 | Día | {none} | {unavailable} | 0, 1, 2, 3, 4, 5, 6, 7, 8, 9, 10, 11, 12, 13, 14, 15, 16, 17, 18, 19, 20, 21, 22, 23 | 15 | Esta programación se ejecuta a las 00:15 a. m., 1:15 a. m., 2:15 a. m., etc. Además, esta programación tiene una frecuencia equivalente de "Hour" y una hora de inicio de "15" minutos. |
| Periodicidad | Ejecutar cada 15 minutos en las marcas de minuto especificadas (sin fecha ni hora de inicio) | 1 | Día | {none} | {unavailable} | 0, 1, 2, 3, 4, 5, 6, 7, 8, 9, 10, 11, 12, 13, 14, 15, 16, 17, 18, 19, 20, 21, 22, 23 | 0, 15, 30, 45 | Esta programación no se inicia hasta la siguiente marca especificada de 15 minutos. |
| Periodicidad | Ejecutar diariamente a las 8:00 a.m. *más* la marca de minuto de cuando guarde la aplicación lógica | 1 | Día | {none} | {unavailable} | 8 | {none} | Sin una fecha y hora de inicio, esta programación se ejecuta en función de la hora a la que se guarda la aplicación lógica (operación PUT). |
| Periodicidad | Ejecutar diariamente a las 8:00 a.m. (con fecha y hora de inicio) | 1 | Día | *startDate*T08:00:00Z | {unavailable} | {none} | {none} | Esta programación no se inicia *antes* de la fecha y hora de inicio especificadas. Las repeticiones futuras se ejecutan diariamente a las 8:00 a.m. | 
| Periodicidad | Ejecutar diariamente a las 8:00 a. m. (sin fecha ni hora de inicio) | 1 | Día | {none} | {unavailable} | 8 | 00 | Esta programación se ejecuta todos los días a las 8:00 a. m. |
| Periodicidad | Ejecutar diariamente a las 8:00 a. m. y a las 4:00 p. m. | 1 | Día | {none} | {unavailable} | 8, 16 | 0 | |
| Periodicidad | Ejecutar diariamente a las 8:30 a.m., 8:45 a.m., 4:30 p.m. y 4:45 p.m. | 1 | Día | {none} | {unavailable} | 8, 16 | 30, 45 | |
| Periodicidad | Ejecutar cada sábado a las 5:00 p. m. (ninguna fecha y hora de inicio) | 1 | Semana | {none} | "Saturday" | 17 | 0 | Esta programación se ejecuta cada sábado a las 5:00 p. m. |
| Periodicidad | Ejecutar cada sábado a las 5:00 p. m. (con fecha y hora de inicio) | 1 | Semana | *startDate*T17:00:00Z | "Saturday" | {none} | {none} | Esta programación no se inicia *antes* de la fecha y hora de inicio especificadas; en este caso, el 9 de septiembre de 2017 a las 5:00 p. m. Las futuras periodicidades se ejecutan todos los sábados a las 5:00 p. m. |
| Periodicidad | Ejecutar todos los martes y jueves a las 5:00 p.m. *más* la marca de minuto de cuando guarde la aplicación lógica| 1 | Semana | {none} | "Tuesday", "Thursday" | 17 | {none} | |
| Periodicidad | Ejecutar cada hora durante el horario laboral | 1 | Semana | {none} | Seleccione todos los días excepto el sábado y el domingo. | Seleccione las horas del día que desee. | Seleccione los minutos de la hora que desee. | Por ejemplo, si su horario laboral es de las 8:00 a. m. a las 5:00 p. m., entonces, seleccione "8, 9, 10, 11, 12, 13, 14, 15, 16, 17" como las horas del día *más* "0" como los minutos de la hora. |
| Periodicidad | Ejecutar una vez al día los fines de semana | 1 | Semana | {none} | "Saturday", "Sunday" | Seleccione las horas del día que desee. | Seleccione los minutos de la hora que correspondan. | Esta programación se ejecuta todos los sábados y domingos según la programación especificada. |
| Periodicidad | Ejecutar cada 15 minutos quincenalmente y solo los lunes | 2 | Semana | {none} | "Monday" | 0, 1, 2, 3, 4, 5, 6, 7, 8, 9, 10, 11, 12, 13, 14, 15, 16, 17, 18, 19, 20, 21, 22, 23 | 0, 15, 30, 45 | Esta programación se ejecuta cada dos lunes en cada marca de 15 minutos. |
| Periodicidad | Ejecutar cada mes | 1 | Month | *startDate*T*startTime*Z | {unavailable} | {unavailable} | {unavailable} | Esta programación no se inicia *antes* de la fecha y hora de inicio especificadas y calcula las futuras periodicidades en función de la fecha y hora de inicio. Si no se especifica una fecha y hora de inicio, esta programación usa la fecha y hora de creación. |
| Periodicidad | Ejecutar cada hora durante un día al mes | 1 | Month | {see note} | {unavailable} | 0, 1, 2, 3, 4, 5, 6, 7, 8, 9, 10, 11, 12, 13, 14, 15, 16, 17, 18, 19, 20, 21, 22, 23 | {see note} | Si no se especifica una fecha y hora de inicio, esta programación usa la fecha y hora de creación. Para controlar los minutos de la programación de periodicidad, especifique los minutos de la hora, una hora de inicio o use la hora de creación. Por ejemplo, si la hora de inicio u hora de creación es 8:25 a. m., esta programación se ejecuta a las 8:25 a. m., 9:25 a. m., 10:25 a. m., y así sucesivamente. |
|||||||||

<a name="run-once"></a>

## <a name="run-one-time-only"></a>Ejecución de solo una vez

Si quiere ejecutar su aplicación lógica solo una vez en el futuro, puede usar la plantilla **Scheduler: Run once jobs** (Programador:ejecutar trabajos una vez). Después de crear una nueva aplicación lógica, pero antes de abrir el Diseñador de aplicaciones lógicas, en la sección **Plantillas**, en la lista **Categoría**, seleccione **Programación** y, a continuación, seleccione esta plantilla:

![Seleccione la plantilla "Scheduler: Run once jobs" (Programador:ejecutar trabajos una vez)](./media/concepts-schedule-automated-recurring-tasks-workflows/choose-run-once-template.png)

O bien, si puede iniciar la aplicación lógica con el desencadenador **Cuando se recibe una solicitud HTTP - Solicitar**, pase la hora de inicio como parámetro para el desencadenador. Para la primera acción, agregue la acción **Retraso hasta: Programación** y especifique la hora en que empezará a ejecutarse la acción siguiente.

## <a name="next-steps"></a>Pasos siguientes

* [Creación, programación y ejecución de tareas y flujos de trabajo periódicos con el desencadenador de periodicidad](../connectors/connectors-native-recurrence.md)
* [Creación, programación y ejecución de tareas y flujos de trabajo periódicos con el desencadenador de ventana deslizante](../connectors/connectors-native-sliding-window.md)
* [Pausa de flujos de trabajo con acciones de retraso](../connectors/connectors-native-delay.md)
