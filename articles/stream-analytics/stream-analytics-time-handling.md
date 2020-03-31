---
title: Descripción del control del tiempo en Azure Stream Analytics
description: Obtenga información sobre cómo funciona el control de tiempo en Azure Stream Analytics, por ejemplo, cómo elegir la mejor hora de inicio, cómo controlar los eventos retrasados y adelantados y las métricas de control de tiempo.
author: mamccrea
ms.author: mamccrea
ms.reviewer: mamccrea
ms.service: stream-analytics
ms.topic: conceptual
ms.date: 03/05/2018
ms.openlocfilehash: 367b7c2e1ce1c8b3c0dbc02003218b76096b409d
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/27/2020
ms.locfileid: "75354646"
---
# <a name="understand-time-handling-in-azure-stream-analytics"></a>Descripción del control del tiempo en Azure Stream Analytics

En este artículo, trataremos cómo puede tomar decisiones de diseño para resolver problemas de control de tiempo prácticos en el servicio de Azure Stream Analytics. Las decisiones de diseño de control del tiempo están estrechamente relacionadas con los factores de ordenación de eventos.

## <a name="background-time-concepts"></a>Conceptos de tiempo de fondo

Para contextualizar la discusión, vamos a definir algunos conceptos de fondo:

- **Hora del evento**: hora en la que se produjo el evento original. Por ejemplo, cuando un automóvil en movimiento en la carretera se acerca a una cabina de peaje.

- **Tiempo de procesamiento**: el tiempo que transcurre desde que el evento llega al sistema de procesamiento y se observa. Por ejemplo, cuando un sensor de la cabina de peaje detecta el automóvil y el sistema informático tarda unos instantes en procesar los datos.

- **Marca de agua**: marcador de hora del evento que indica qué eventos puntuales se han introducido en el procesador de streaming. Las marcas de agua permiten al sistema indicar el progreso claro sobre la ingesta de eventos. Por la naturaleza de las transmisiones, los datos de evento entrantes nunca se detienen, por lo que las marcas de agua indican el progreso a un determinado punto en la transmisión.

   El concepto de marca de agua es importante. Las marcas de agua permiten a Stream Analytics determinar cuándo el sistema puede producir resultados completos, correctos y repetibles que no necesitan retirarse. El procesamiento puede realizarse de una forma garantizada, que es predecible y repetible. Por ejemplo, si es necesario realizar un recuento para alguna condición de control de errores, las marcas de agua son puntos de inicio y finalización seguros.

Como recursos adicionales sobre este tema, vea las entradas de blog de Tyler Akidau [Streaming 101](https://www.oreilly.com/ideas/the-world-beyond-batch-streaming-101) y [Streaming 102](https://www.oreilly.com/ideas/the-world-beyond-batch-streaming-102).

## <a name="choosing-the-best-starting-time"></a>Elección de la mejor hora de inicio

Stream Analytics ofrece a los usuarios dos opciones para elegir la hora del evento:

1. **Hora de llegada**  

   La hora de llegada se asigna en el origen de entrada, cuando el evento llega al origen. Puede obtener acceso a la hora de llegada si usa la propiedad **EventEnqueuedTime** con los datos de entrada de Event Hubs, la propiedad **IoTHub.EnqueuedTime** para IoT Hub y la propiedad **BlobProperties.LastModified** para las entradas de blobs.

   El uso de la hora de llegada es el comportamiento predeterminado y se usa para escenarios de archivado de datos, donde no es necesaria ninguna lógica temporal.

2. **Hora de aplicación** (también se denomina hora del evento)

   La hora de aplicación se asigna cuando el evento se genera y forma parte de la carga del evento. Para procesar eventos según el tiempo de aplicación, use la cláusula **Timestamp by** en la consulta de selección. Si la cláusula **Timestamp by** no está presente, los eventos se procesarán según la hora de llegada.

   Es importante usar una marca de tiempo en la carga cuando la lógica temporal está implicada. De este modo, los retrasos en el sistema de origen o en la red pueden tenerse en cuenta.

## <a name="how-time-progresses-in-azure-stream-analytics"></a>Progreso del tiempo en Azure Stream Analytics

Cuando se usa la hora de aplicación, la progresión del tiempo se basa en los eventos de entrada. Es difícil para el sistema de procesamiento de transmisiones saber si no hay eventos o si los eventos están retrasados. Por este motivo, Azure Stream Analytics genera marcas de agua heurísticas de las maneras siguientes para cada partición de entrada:

1. Siempre que haya algún evento entrante, la marca de agua es la hora del evento más grande que hemos visto hasta ahora menos el tamaño del período de tolerancia de desorden.

2. Siempre que no haya un evento entrante, la marca de agua es el la hora de llegada estimada actual (el tiempo transcurrido en la máquina virtual interna que procesa los eventos desde la última vez que se ve un evento de entrada más la hora de llegada del evento de entrada) menos el período de tolerancia de llegada tardía.

   Solo se puede estimar la hora de llegada porque la hora de llegada real se genera en el agente de eventos de entrada, como Event Hubs, y no en la máquina virtual de Azure Stream Analytics que procesa los eventos.

El diseño tiene dos propósitos adicionales, además de generar marcas de agua:

1. El sistema genera los resultados de manera puntual con o sin eventos entrantes.

   Usted tiene el control sobre la puntualidad con la que desean ver los resultados de salida. En Azure Portal, en la página **Ordenación de eventos** del trabajo de Stream Analytics, puede configurar la opción **Eventos desordenados**. Al configurar esa opción, tenga en cuenta la compensación de la puntualidad con la tolerancia de los eventos desordenados en la transmisión de eventos.

   El período de tolerancia de llegada tardía es importante para seguir generando marcas de agua, incluso en ausencia de eventos de entrada. En ocasiones, puede haber un período donde no entren eventos entrantes, por ejemplo, cuando un flujo de entrada de eventos es escaso. Este problema se agrava por el uso de varias particiones en el agente de eventos de entrada.

   Los sistemas de procesamiento de datos de transmisión sin un período de tolerancia de llegada tardía pueden sufrir salidas retrasadas cuando las entradas son escasas y se usan varias particiones.

2. El comportamiento del sistema tiene que ser repetible. La repetibilidad es una propiedad importante de un sistema de procesamiento de datos de transmisión.

   La marca de agua se deriva de la hora de llegada y la hora de aplicación. Ambas son persistentes en el agente de eventos y, por tanto, repetibles. En el caso de que la hora de llegada tenga que estimarse en ausencia de eventos, Azure Stream Analytics registra la hora de llegada estimada para la repetibilidad durante la reproducción con la finalidad de recuperación de errores.

Tenga en cuenta que si decide usar la **hora de llegada** como la hora del evento, no es necesario para configurar la tolerancia de desorden ni la tolerancia de llegada tardía. Como se garantiza que la **hora de llegada** aumenta uniformemente en el agente de eventos de entrada, Azure Stream Analytics simplemente pasa por alto las configuraciones.

## <a name="late-arriving-events"></a>Eventos de llegada tardía

Por definición del período de tolerancia de llegada tardía, para cada evento entrante, Azure Stream Analytics compara la **hora del evento** con la **hora de llegada**; si la hora del evento está fuera del período de tolerancia, puede configurar el sistema para que descarte el evento o ajuste la hora del evento para que esté dentro de la tolerancia.

Tenga en cuenta que una vez generadas las marcas de agua, el servicio puede recibir eventos con la hora del evento inferior a la marca de agua. Puede configurar el servicio para **descartar** esos eventos o para **ajustar** la hora del evento al valor de la marca de agua.

Como parte del ajuste, **System.Timestamp** del evento se establece en el nuevo valor, pero el propio campo de la **hora del evento** no se cambia. Este ajuste es la única situación donde **System.Timestamp** de un evento puede ser diferente al valor del campo de hora del evento, y puede provocar resultados inesperados al generarse.

## <a name="handling-time-variation-with-substreams"></a>Control de la variación de tiempo con transmisiones secundarias

El mecanismo de generación de marcas de agua heurísticas aquí descrito funciona correctamente en la mayoría de los casos donde la hora generalmente está sincronizada entre los diferentes remitentes de eventos. Sin embargo, en la vida real, especialmente en muchos escenarios de IoT, el sistema tiene poco control sobre el reloj en los remitentes de eventos. Los remitentes de eventos podrían ser todo tipo de dispositivos en el campo, tal vez con diferentes versiones de hardware y software.

En lugar de usar una marca de agua global para todos los eventos en una partición de entrada, Stream Analytics tiene otro mecanismo denominado transmisiones secundarias para ayudarle. Puede utilizar transmisiones secundarias en su trabajo mediante la escritura de una consulta de trabajo que utilice la cláusula [**TIMESTAMP BY**](/stream-analytics-query/timestamp-by-azure-stream-analytics) y la palabra clave **OVER**. Para designar la transmisión secundaria, proporcione un nombre de columna de clave después de la palabra clave **OVER**, como `deviceid`, de modo que el sistema aplique las directivas de tiempo por esa columna. Cada transmisión secundaria obtiene su propia marca de agua independiente. Este mecanismo es útil para permitir la generación de salida a tiempo, cuando se trabaja con grandes sesgos de reloj o retrasos de red entre remitentes de eventos.

Las transmisiones secundarias son una solución única de Azure Stream Analytics que otros sistemas de procesamiento de datos de transmisión no ofrecen. Stream Analytics aplica el período de tolerancia de llegada tardía a los eventos entrantes cuando se usan transmisiones secundarias. El valor predeterminado (cinco segundos) probablemente es demasiado pequeño para dispositivos con marcas de tiempo divergentes. Se recomienda que comience con cinco minutos y realice ajustes según su patrón de sesgo de reloj de dispositivo.

## <a name="early-arriving-events"></a>Eventos de llegada temprana

Es posible que haya observado otro concepto de período de llegada temprana, que es lo opuesto al período de tolerancia de llegada tardía. Este período se fija en cinco minutos y tiene una finalidad diferente al de llegada tardía.

Dado que Azure Stream Analytics garantiza que siempre genera los resultados completos, solo se puede especificar la **hora de inicio del trabajo** como la primera hora de salida del trabajo, no la hora de entrada. La hora de inicio del trabajo es necesaria para que se procese el período completo, no solo desde la mitad del período.

Después, Stream Analytics, deriva la hora de inicio de la especificación de consulta. Sin embargo, dado que el agente de eventos de entrada solo se indexa por hora de llegada, el sistema tiene que convertir la hora del evento inicial a la hora de llegada. El sistema puede iniciar el procesamiento de eventos desde ese punto en el agente de eventos de entrada. Con el límite de período de llegada temprana, la conversión es sencilla. Es la hora del evento de inicio menos el período de llegada temprana de cinco minutos. Este cálculo también significa que el sistema quita todos los eventos que se ve que tienen un tiempo de evento cinco minutos superior a la hora de llegada.

Este concepto se utiliza para garantizar que el procesamiento es repetible con independencia de dónde se inicie la salida. Sin este mecanismo, no sería posible garantizar la repetibilidad, como muchos otros sistemas de transmisión afirman que hacen.

## <a name="side-effects-of-event-ordering-time-tolerances"></a>Efectos secundarios de las tolerancias de tiempo de ordenación de eventos

Los trabajos de Stream Analytics tienen varias opciones de **ordenación de eventos**. Dos se pueden configurar en Azure Portal: la opción **Eventos desordenados** (tolerancia de desorden) y la opción **Eventos que llegan tarde** (tolerancia de llegada tardía). La tolerancia de **llegada temprana** es fija y no se puede ajustar. Estas directivas de tiempo las usa Stream Analytics para proporcionar garantías sólidas. Sin embargo, esta configuración a veces tiene algunas implicaciones inesperadas:

1. Envío accidental de eventos que son demasiado tempranos.

   Normalmente, los eventos tempranos no se deben enviar. No obstante, es posible que los eventos tempranos se envíen a la salida si el reloj del remitente se ejecuta demasiado rápido. Todos los eventos de llegada temprana se quitan, por lo que no verá ninguno de ellos en la salida.

2. Envío de eventos antiguos a Event Hubs para que Azure Stream Analytics los procese.

   Aunque los eventos antiguos pueden parecer inofensivos al principio, debido a la aplicación de la tolerancia de llegada tardía, dichos eventos se pueden quitar. Si los eventos son demasiado antiguos, el valor de **System.Timestamp** se modifica durante la ingesta de eventos. Debido a este comportamiento, actualmente Azure Stream Analytics es más adecuado para escenarios de procesamiento de eventos casi en tiempo real que para escenarios de procesamiento de eventos históricos. Puede establecer el tiempo **Eventos que llegan tarde** en el mayor valor posible (20 días) para evitar este comportamiento en algunos casos.

3. Las salidas parecen que se retrasan.

   La primera marca de agua se genera a la hora calculada: la **hora del evento máxima** que el sistema ha observado hasta ahora, menos el tamaño el período de la tolerancia de desorden. De forma predeterminada, la tolerancia de desorden se configura en cero (00 minutos y 00 segundos). Cuando la establece en un valor de tiempo más alto distinto de cero, la primera salida del trabajo de transmisión se retrasa ese valor de tiempo (o más) debido a la primera hora de la marca de agua que se calcula.

4. Las entradas son escasas.

   Cuando no hay ninguna entrada en una partición determinada, la hora de la marca de agua se calcula como la **hora de llegada** menos el período de tolerancia de llegada tardía. Por consiguiente, si los eventos de entrada son poco frecuentes y escasos, la salida se puede retrasar esa cantidad de tiempo. El valor predeterminado de **Eventos que llegan tarde** es cinco segundos. Debería esperar que se produjera cierto retraso al enviar los eventos de entrada uno a uno, por ejemplo. Los retrasos pueden empeorar al establecer el período **Eventos que llegan tarde** en un valor grande.

5. El valor de **System.Timestamp** es diferente a la hora del campo **Hora del evento**.

   Como se describió anteriormente, el sistema ajusta la hora del evento por la tolerancia de desorden o los períodos de tolerancia de llegada tardía. El valor de **System.Timestamp** del evento se ajusta, pero no el campo **Hora del evento**.

## <a name="metrics-to-observe"></a>Métricas que se deben observar

Puede observar diferentes efectos de tolerancia de hora de ordenación de eventos mediante las [métricas de trabajo de Stream Analytics](stream-analytics-monitoring.md). Las métricas siguientes son pertinentes:

|Métrica  | Descripción  |
|---------|---------|
| **Eventos desordenados** | Indica el número de eventos recibidos desordenados, que se eliminan o se les asigna una marca de tiempo ajustada. Esta métrica se ve afectada directamente por la opción **Eventos desordenados** de la página **Ordenación de eventos** del trabajo en Azure Portal. |
| **Eventos de entrada retrasada** | Indica el número de eventos que llegan tarde desde el origen. Esta métrica incluye eventos que se han quitado o han hecho que su marca de tiempo se ajustara. Esta métrica se ve afectada directamente por la configuración de la opción **Eventos que llegan tarde** de la página **Ordenación de eventos** del trabajo en Azure Portal. |
| **Primeros eventos de entrada** | Indica el número de eventos que llegan de forma temprana desde el origen que se han quitado o cuya marca de tiempo se ha ajustado si su anticipación era superior a cinco minutos. |
| **Retraso de la marca de agua** | Indica el retraso del trabajo de procesamiento de datos de transmisión. En la sección siguiente dispone de más información.|

## <a name="watermark-delay-details"></a>Detalles de la métrica Retraso de la marca de agua

La métrica **Retraso de la marca de agua** se calcula como el tiempo de reloj del nodo de procesamiento menos la marca de agua más grande que ha visto hasta ahora. Para más información, consulte la [entrada de blog de retraso de marca de agua](https://azure.microsoft.com/blog/new-metric-in-azure-stream-analytics-tracks-latency-of-your-streaming-pipeline/).

Puede haber varias razones por las que este valor de la métrica es mayor que cero bajo una operación normal:

1. Retraso de procesamiento inherente de la canalización de transmisión. Normalmente, este retraso es nominal.

2. El período de tolerancia de desorden introdujo el retraso porque la marca de agua se reduce según el tamaño del período de tolerancia.

3. El período de llegada tardía introdujo el retraso porque la marca de agua se reduce según el tamaño del período de tolerancia.

4. Sesgo del reloj del nodo de procesamiento que genera la métrica.

Hay una serie restricciones de recursos adicionales que pueden hacer que la canalización de transmisión se ralentice. La métrica de retraso de marca de agua puede aumentar debido a:

1. Recursos de procesamiento insuficientes en Stream Analytics para controlar el volumen de eventos de entrada. Para escalar verticalmente los recursos, consulte [Descripción y ajuste de las unidades de streaming](stream-analytics-streaming-unit-consumption.md).

2. No hay suficiente rendimiento dentro de los agentes de eventos de entrada, por lo que se limitan. Para posibles soluciones, consulte [Escalado vertical y automático de las unidades de procesamiento de Azure Event Hubs](../event-hubs/event-hubs-auto-inflate.md).

3. Los receptores de salida no se aprovisionan con capacidad suficiente, por lo que se limitan. Las posibles soluciones varían ampliamente según el tipo de servicio de salida que se utiliza.

## <a name="output-event-frequency"></a>Frecuencia de eventos de salida

Azure Stream Analytics usa el progreso de la marca de agua como el único desencadenador para generar eventos de salida. Dado que la marca de agua se deriva de datos de entrada, es repetible durante la recuperación de errores y también en el reprocesamiento iniciado por el usuario.

Cuando se usan [agregados en ventanas](stream-analytics-window-functions.md), el servicio solo genera salidas al final de las ventanas. En algunos casos, los usuarios puede que deseen ver agregados parciales generados desde las ventanas. Actualmente no se admiten agregados parciales en Azure Stream Analytics.

En otras soluciones de transmisión, los eventos de salida se podrían materializar en varios puntos de desencadenador, dependiendo de circunstancias externas. En algunas soluciones, es posible que los eventos de salida de un períodos de tiempo dado se generen varias veces. A medida que se refinan los valores de entrada, los resultados de agregados se vuelven más precisos. Al principio, se podría especular con los eventos, que podrían revisarse con el tiempo. Por ejemplo, cuando un determinado dispositivo está desconectado de la red, un sistema podría usar un valor estimado. Más adelante, el mismo dispositivo pasa a estar conectado a la red. Es entonces cuando los datos del evento real podrían incluirse en la transmisión de entrada. Los resultados de salida del procesamiento de ese período de tiempo genera un resultado más preciso.

## <a name="illustrated-example-of-watermarks"></a>Ejemplo ilustrado de marcas de agua

Las siguientes imágenes muestran el progreso de las marcas de agua en distintas circunstancias.

En esta tabla se muestran los datos de ejemplo que se representan después en el gráfico. Tenga en cuenta que la hora del evento y la hora de llegada varían, a veces coincidiendo y a veces no.

| Hora del evento | Hora de llegada | deviceId |
| --- | --- | --- |
| 12:07 | 12:07 | device1
| 12:08 | 12:08 | device2
| 12:17 | 12:11 | device1
| 12:08 | 12:13 | device3
| 12:19 | 12:16 | device1
| 12:12 | 12:17 | device3
| 12:17 | 12:18 | device2
| 12:20 | 12:19 | device2
| 12:16 | 12:21 | device3
| 12:23 | 12:22 | device2
| 12:22 | 12:24 | device2
| 12:21 | 12:27 | device3

En esta ilustración, se usan las tolerancias siguientes:

- Los períodos de llegada temprana son de cinco minutos
- Los períodos de llegada tardía son de cinco minutos
- El período de reordenación es de dos minutos

1. Ilustración del procesamiento de las marcas de agua a través de estos eventos:

   ![Ilustración de marcas de agua de Azure Stream Analytics](media/stream-analytics-time-handling/WaterMark-graph-1.png)

   Procesos importantes ilustrados en el gráfico anterior:

   1. El primer evento (device1) y el segundo evento (device2) tienen horas alineadas y se procesan sin ajustes. La marca de agua progresa en cada evento.

   2. Cuando el tercer evento (device1) se procesa, la hora de llegada (12:11) precede a la hora del evento (12:17). El evento llega seis minutos antes, por lo que el evento se quita porque la tolerancia de llegada temprana es de cinco minutos.

      La marca de agua no progresa en este caso de evento temprano.

   3. El cuarto evento (device3) y el quinto evento (device1) tienen horas alineadas y se procesan sin ajustes. La marca de agua progresa en cada evento.

   4. Cuando el sexto evento (device3) se procesa, la hora de llegada (12:17) y la hora del evento (12:12) están por debajo del nivel de la marca de agua. La hora del evento se ajusta al nivel de la marca de agua (12:17).

   5. Cuando el duodécimo evento (device3) se procesa, la hora de llegada (12:27) es seis minutos después de la hora del evento (12:21). Se aplica la directiva de llegada tardía. Se ajusta la hora del evento (12:22), que es posterior a la marca de agua (12:21), por lo que no se aplican más ajustes.

2. Segunda ilustración del progreso de las marcas de agua sin una directiva de llegada temprana:

   ![Ilustración de marcas de agua sin directiva temprana de Azure Stream Analytics](media/stream-analytics-time-handling/watermark-graph-2.png)

   En este ejemplo, no se aplica ninguna directiva de llegada temprana. Los eventos de valores atípicos que llegan anticipadamente aumentan significativamente la marca de agua. Tenga en cuenta que el tercer evento (deviceId1 a las 12:11) no se quita en este escenario y la marca de agua se eleva a 12:15. Por consiguiente, la hora del cuarto evento se retrasa siete minutos (de las 12:08 a las 12:15).

3. En la ilustración final, se usan transmisiones secundarias (A TRAVÉS de DeviceId). Se realiza un seguimiento de varias marcas de agua, una por transmisión. El resultado es que hay menos eventos con sus horas ajustadas.

   ![Ilustración de marcas de agua de transmisiones secundarias de Azure Stream Analytics](media/stream-analytics-time-handling/watermark-graph-3.png)

## <a name="next-steps"></a>Pasos siguientes

- [Puntos a tener en cuenta sobre el orden de eventos de Azure Stream Analytics](stream-analytics-out-of-order-and-late-events.md)
- [Métricas de trabajo de Stream Analytics](stream-analytics-monitoring.md)
