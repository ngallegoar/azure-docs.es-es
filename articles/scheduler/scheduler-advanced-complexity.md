---
title: Creación de programaciones de trabajo avanzadas y periodicidades
description: Aprenda a crear programaciones avanzadas y periodicidades para trabajos en Azure Scheduler
services: scheduler
ms.service: scheduler
author: derek1ee
ms.author: deli
ms.reviewer: klam, estfan
ms.suite: infrastructure-services
ms.topic: article
ms.date: 11/14/2018
ms.openlocfilehash: b85932bf0d4fd080afadef2bc28d6a218b2d627a
ms.sourcegitcommit: 829d951d5c90442a38012daaf77e86046018e5b9
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/09/2020
ms.locfileid: "78898599"
---
# <a name="build-advanced-schedules-and-recurrences-for-jobs-in-azure-scheduler"></a>Creación de programaciones avanzadas y periodicidades para trabajos en Azure Scheduler

> [!IMPORTANT]
> [Azure Logic Apps](../logic-apps/logic-apps-overview.md) reemplaza a Azure Scheduler, que se [va a retirar](../scheduler/migrate-from-scheduler-to-logic-apps.md#retire-date). Para seguir utilizando los trabajos configurados en Scheduler, [migre a Azure Logic Apps](../scheduler/migrate-from-scheduler-to-logic-apps.md) cuanto antes. 
>
> Scheduler ya no está disponible en Azure portal, pero la [API REST](/rest/api/scheduler) y los [cmdlets de PowerShell para Azure Scheduler](scheduler-powershell-reference.md) siguen disponibles en la actualidad para que pueda administrar los trabajos y las colecciones de trabajos.

En un trabajo de [Azure Scheduler](../scheduler/scheduler-intro.md), la programación es el núcleo que determina cuándo y cómo el servicio Scheduler ejecuta el trabajo. Con Scheduler puede configurar varias programaciones únicas o periódicas para un trabajo. Las programaciones únicas se ejecutan solo una vez en un momento determinado; básicamente son programaciones periódicas que se ejecutan solo una vez. Las programaciones periódicas se ejecutan con una frecuencia determinada. Gracias a esta flexibilidad, puede usar Scheduler en varios escenarios empresariales, como:

* **Eliminación de datos con regularidad**: cree un trabajo diario que elimine todos los tweets que tengan más de tres meses.

* **Archivo de datos**: cree un trabajo mensual que envíe el historial de facturación a un servicio de copia de seguridad.

* **Solicitud de datos externos**: cree un trabajo que se ejecute cada 15 minutos y extraiga un nuevo informe meteorológico de NOAA.

* **Procesamiento de imágenes**: cree un trabajo de día laboral que se ejecute fuera de las horas punta y use informática en nube para comprimir las imágenes cargadas durante el día.

En este artículo se describen trabajos de ejemplo que puede crear con Scheduler y la [API REST de Azure Scheduler](/rest/api/scheduler) y se incluye la definición de la notación de objetos JavaScript (JSON) de cada programación. 

## <a name="supported-scenarios"></a>Escenarios admitidos

Estos ejemplos muestran la gama de escenarios que admite Azure Scheduler y la creación de programaciones para diversos patrones de comportamiento, como:

* Ejecutar una vez en una fecha y hora específicas.
* Ejecutar y repetir un número específico de veces.
* Ejecutar inmediatamente y repetir.
* Ejecutar y repetir cada *n* minutos, horas, días, semanas o meses a partir de un período de tiempo específico.
* Ejecutar y repetir con frecuencia semanal o mensual, pero solo en días específicos de la semana o del mes.
* Ejecutar y repetir más de una vez durante un tiempo específico. Por ejemplo, el último viernes y último lunes de cada mes, o a las 5:15 y a las 17:15 todos los días.

Más adelante en este artículo se describen estos escenarios con mayor detalle.

<a name="create-scedule"></a>

## <a name="create-schedule-with-rest-api"></a>Creación de programaciones con las API REST

Para crear una programación básica con la [API REST de Azure Scheduler](/rest/api/scheduler), siga estos pasos:

1. Registre su suscripción de Azure con un proveedor de recursos mediante la [operación de registro con la API REST de Resource Manager](https://docs.microsoft.com/rest/api/resources/providers). El nombre del proveedor para el servicio Azure Scheduler es **Microsoft.Scheduler**. 

1. Cree una colección de trabajos mediante la [operación de creación o actualización para colecciones de trabajos](https://docs.microsoft.com/rest/api/scheduler/jobcollections) de la API REST de Scheduler. 

1. Cree un trabajo mediante la [operación de creación o actualización para trabajos](https://docs.microsoft.com/rest/api/scheduler/jobs/createorupdate). 

## <a name="job-schema-elements"></a>Elementos de esquema para los trabajos

Esta tabla proporciona información general de alto nivel de los principales elementos JSON que puede usar al configurar la periodicidad y las programaciones para los trabajos. 

| Elemento | Obligatorio | Descripción | 
|---------|----------|-------------|
| **startTime** | No | Un valor de cadena de fecha y hora en [formato ISO 8601](https://en.wikipedia.org/wiki/ISO_8601) que especifica cuándo el trabajo se inicia por primera vez en una programación básica. <p>Para las programaciones complejas, el trabajo no se inicia antes de **startTime**. | 
| **recurrence** | No | Las reglas de periodicidad de ejecución del trabajo. El objeto **recurrence** admite los siguientes elementos: **frequency**, **interval**, **schedule**, **count** y **endTime**. <p>Si usa el elemento **recurrence** elemento, también debe usar **frequency**, mientras que los demás elementos de **recurrence** son opcionales. |
| **frequency** | Sí, cuando se usa **recurrence** | Unidad de tiempo de la periodicidad, se admiten estos valores: "Minute", "Hour", "Day", "Week", "Month", "Year" | 
| **interval** | No | Entero positivo que determina el número de unidades de tiempo de la periodicidad según el valor de **frequency**. <p>Por ejemplo, si **interval** es 10 y **frequency** es "Week", el trabajo se repite cada diez semanas. <p>Estos son los intervalos máximos para cada frecuencia: <p>- 18 meses <br>- 78 semanas <br>- 548 días <br>- Para las horas y los minutos, el intervalo es 1 <= <*interval*>< = 1000. | 
| **schedule** | No | Define los cambios en la periodicidad según las marcas de minuto, de hora, de días de la semana y días del mes especificados | 
| **count** | No | Entero positivo que especifica el número de veces que debe ejecutarse el trabajo antes de finalizar. <p>Por ejemplo, cuando tiene un trabajo diario tiene **count** establecido en 7 y la fecha de inicio es el lunes, terminará de ejecutarse el domingo. Si la fecha de inicio ya ha pasado, se calcula la primera ejecución desde la hora de creación. <p>Sin valor para **endTime** o **count**, el trabajo se ejecuta indefinidamente. No se pueden usar **count** y **endTime** en el mismo trabajo, se cumplirá la regla del que finalice primero. | 
| **endTime** | No | Un valor de cadena de fecha o de fecha y hora en [formato ISO 8601](https://en.wikipedia.org/wiki/ISO_8601) que especifica cuándo el trabajo deja de ejecutarse. Puede establecer un valor para **endTime** que se encuentre en el pasado. <p>Sin valor para **endTime** o **count**, el trabajo se ejecuta indefinidamente. No se pueden usar **count** y **endTime** en el mismo trabajo, se cumplirá la regla del que finalice primero. |
|||| 

Por ejemplo, este esquema JSON describe una programación básica y la periodicidad de un trabajo: 

```json
"properties": {
   "startTime": "2012-08-04T00:00Z", 
   "recurrence": {
      "frequency": "Week",
      "interval": 1,
      "schedule": {
         "weekDays": ["Monday", "Wednesday", "Friday"],
         "hours": [10, 22]                      
      },
      "count": 10,       
      "endTime": "2012-11-04"
   },
},
``` 

*Valores de fecha y de fecha y hora*

* Las fechas en los trabajos de Scheduler incluyen solo la fecha y siguen la [especificación ISO 8601](https://en.wikipedia.org/wiki/ISO_8601).

* La fecha y hora en los trabajos de Scheduler incluyen la fecha y la hora, siguen la [especificación ISO 8601](https://en.wikipedia.org/wiki/ISO_8601) y se consideran como parte de la zona horaria UTC si no se especifica diferencia horaria con UTC. 

Para más información, consulte [Conceptos, terminología y entidades de Azure Scheduler](../scheduler/scheduler-concepts-terms.md).

<a name="start-time"></a>

## <a name="details-starttime"></a>Detalles: startTime

En la siguiente tabla se describe cómo **startTime** controla la forma en la que se ejecuta un trabajo:

| startTime | Sin periodicidad | Periodicidad, sin programación | Periodicidad con programación |
|-----------|---------------|-------------------------|--------------------------|
| **Sin hora de inicio** | Ejecutar inmediatamente una vez. | Ejecutar inmediatamente una vez. Realizar ejecuciones posteriores calculadas desde la última hora de ejecución. | Ejecutar inmediatamente una vez. Realizar ejecuciones posteriores según una programación de periodicidad. | 
| **Hora de inicio en el pasado** | Ejecutar inmediatamente una vez. | Calcula la hora de la primera ejecución tras la hora de inicio y realiza la ejecución a esa hora. <p>Realizar ejecuciones posteriores calculadas desde la última hora de ejecución. <p>Consulte el ejemplo a continuación de esta tabla. | Inicie el trabajo *como pronto* a la hora de inicio especificada. La primera repetición se basa en la programación que se calcula a partir de la hora de inicio. <p>Realizar ejecuciones posteriores según una programación de periodicidad. | 
| **Hora de inicio en el futuro o la hora actual** | Se ejecuta una vez a la hora de inicio especificada. | Se ejecuta una vez a la hora de inicio especificada. <p>Realizar ejecuciones posteriores calculadas desde la última hora de ejecución. | Inicie el trabajo *como pronto* a la hora de inicio especificada. La primera aparición se basa en la programación que se calcula a partir de la hora de inicio. <p>Realizar ejecuciones posteriores según una programación de periodicidad. |
||||| 

Supongamos que este ejemplo cumple estas condiciones: una hora de inicio en el pasado con periodicidad, pero sin programación.

```json
"properties": {
   "startTime": "2015-04-07T14:00Z", 
   "recurrence": {
      "frequency": "Day",
      "interval": 2
   }
}
```

* La fecha y la hora actuales son "8 de abril de 2015 a la 13:00".

* La fecha y la hora de inicio son"7 de abril de 2015 a las 14:00", lo cual es anterior a la fecha y la hora actuales.

* La periodicidad es de cada dos días.

1. En estas condiciones, la primera ejecución será el 9 de abril de 2015 a las 14:00. 

   Scheduler calcula las ejecuciones en función de la hora de inicio, descarta las instancias pasadas y utiliza la siguiente instancia futura. 
   En este caso, **startTime** es el 7 de abril de 2015 a las 14:00, por lo que la siguiente instancia es dos días a partir de ese momento, es decir, el 9 de abril de 2015 a las 14:00.

   La primera ejecución es la misma si **startTime** es 2015-04-05 14:00 o 2015-04-01 14:00. Después de la primera ejecución, las ejecuciones posteriores se calculan en función de la programación. 
   
1. Las ejecuciones se sucederán en este orden: 
   
   1. 2015-04-11 a las 14:00
   1. 2015-04-13 a las 14:00 
   1. 2015-04-15 a las 14:00
   1. y así sucesivamente.

1. Por último, cuando un trabajo tiene una programación sin horas ni minutos especificados, se asumen los valores predeterminados de las horas y minutos de la primera ejecución, respectivamente.

<a name="schedule"></a>

## <a name="details-schedule"></a>Detalles: schedule

Puede usar **schedule** para *limitar* el número de ejecuciones de trabajos. Por ejemplo, si un trabajo tiene un valor de **frequency** de "mes" y tiene una programación que solo se ejecuta el día 31, el trabajo solo se ejecutará en los meses que tengan 31 días.

También puede usar **schedule** para *expandir* el número de ejecuciones de trabajos. Por ejemplo, si un trabajo con un valor de **frequency** de "mes" tiene una programación que se ejecuta los días 1 y 2 del mes, el trabajo se ejecuta en los días primero y segundo del mes en lugar de solo una vez al mes.

Si especifica más de un elemento de programación, el orden de evaluación es de mayor a menor: número de semana, mes, día, día de la semana, hora y minuto.

En la siguiente tabla se describen los elementos de schedule con detalle:

| Nombre JSON | Descripción | Valores válidos |
|:--- |:--- |:--- |
| **minutes** |Minutos de la hora en la que se ejecuta el trabajo. |Una matriz de enteros. |
| **hours** |Horas del día en las que se ejecuta el trabajo. |Una matriz de enteros. |
| **weekDays** |Días de la semana en los que se ejecuta el trabajo. Solo se puede especificar con una frecuencia semanal. |Una matriz de cualquiera de los valores siguientes (el tamaño máximo de la matriz es 7):<br />- "Monday"<br />- "Tuesday"<br />- "Wednesday"<br />- "Thursday"<br />- "Friday"<br />- "Saturday"<br />- "Sunday"<br /><br />No distingue mayúsculas de minúsculas. |
| **monthlyOccurrences** |Determina los días del mes en los que se ejecutará el trabajo. Solo se puede especificar con una frecuencia mensual. |Una matriz de objetos de **monthlyOccurrences**:<br /> `{ "day": day, "occurrence": occurrence}`<br /><br /> **day** es el día de la semana en el que se ejecuta el trabajo. Por ejemplo, *{Sunday}* es todos los domingos del mes. Necesario.<br /><br />El valor de **occurrence** es la repetición del elemento day durante el mes. Por ejemplo, *{Sunday, -1}* es el último domingo del mes. Opcional. |
| **monthDays** |Día del mes en el que se ejecuta el trabajo. Solo se puede especificar con una frecuencia mensual. |Una matriz de los valores siguientes:<br />- Cualquier valor <= -1 y >= -31<br />- Cualquier valor >= 1 y <= 31|

## <a name="examples-recurrence-schedules"></a>Ejemplos: Programaciones de periodicidad

Los ejemplos siguientes muestran varias programaciones de periodicidad. Los ejemplos se centran en el objeto de programación y en sus subelementos.

Estas programaciones asumen que **interval** está establecido en 1\.. Los ejemplos también suponen los valores correctos de **frequency** para los valores de **schedule**. Por ejemplo, no puede tener un valor de **frequency** de "day" y tener también una modificación de **monthDays** en el objeto **schedule**. Estas restricciones se han descrito anteriormente en el artículo.

| Ejemplo | Descripción |
|:--- |:--- |
| `{"hours":[5]}` |Se ejecuta a las 5 a. m. todos los días.<br /><br />Scheduler hace corresponder cada valor en "horas" con cada valor de "minutos", uno por uno, para crear una lista de todas las veces en las que se ejecuta el trabajo. |
| `{"minutes":[15], "hours":[5]}` |Se ejecuta a las 5:15 a. m. todos los días. |
| `{"minutes":[15], "hours":[5,17]}` |Se ejecuta a las 5:15 a. m. y 5:15 p. m. todos los días. |
| `{"minutes":[15,45], "hours":[5,17]}` |Se ejecuta a las 5:15 a. m., 5:45 a. m., 5:15 p. m. y 5:45 p. m. todos los días. |
| `{"minutes":[0,15,30,45]}` |Se ejecuta cada 15 minutos. |
| `{hours":[0, 1, 2, 3, 4, 5, 6, 7, 8, 9, 10, 11, 12, 13, 14, 15, 16, 17, 18, 19, 20, 21, 22, 23]}` |Se ejecuta cada hora.<br /><br />Este trabajo se ejecuta cada hora. El minuto viene controlado por el valor **startTime**, si se especifica. Si no se especifica ningún valor **startTime**, el minuto lo controla la hora de creación. Por ejemplo, si la hora de inicio o la hora de creación (lo que se aplique) es 12:25 p. m., el trabajo se ejecutará a las 00:25, 01:25, 02:25, …, 23:25.<br /><br />La programación equivale a un trabajo con un valor de **frequency** de "hora", un valor de **interval** de 1 y ningún valor de **schedule**. La diferencia es que esta programación puede usarse con diferentes valores en **frequency** e **interval** para crear también otros trabajos. Por ejemplo, si el valor de **frequency** fuera “mes”, la programación se ejecutaría solo una vez al mes en lugar de todos los días (como sucedería si **frequency** fuera “día”). |
| `{minutes:[0]}` |Se ejecuta cada hora durante la hora.<br /><br />Este trabajo también se ejecuta cada hora, pero a la hora exacta (por ejemplo, 12 a. m., 1 a. m., 2 a. m., etc.) Esta programación es equivalente a un trabajo con un valor de **frecuency** de "hora", un valor de **startTime** de cero minutos y ningún valor de **schedule**, si la frecuencia es "día". No obstante, si el valor de **frequency** es "semana" o "mes", la programación se ejecuta únicamente un día a la semana o un día al mes, respectivamente. |
| `{"minutes":[15]}` |Se ejecuta a "y cuarto" cada hora.<br /><br />Se ejecuta cada hora a partir de las 00:15 a. m., 1:15 a. m., 2:15 a. m., y así sucesivamente. Finaliza a las 11:15 p. m. |
| `{"hours":[17], "weekDays":["saturday"]}` |Se ejecuta a las 5 p. m. los sábados de cada semana. |
| `{hours":[17], "weekDays":["monday", "wednesday", "friday"]}` |Se ejecuta a las 5 p. m. los lunes, miércoles y viernes de cada semana. |
| `{"minutes":[15,45], "hours":[17], "weekDays":["monday", "wednesday", "friday"]}` |Se ejecuta a las 5:15 p. m. y 5:45 p. m. los lunes, miércoles y viernes de cada semana. |
| `{"hours":[5,17], "weekDays":["monday", "wednesday", "friday"]}` |Se ejecuta a las 5 a. m. y a las 5 p. m. los lunes, miércoles y viernes de cada semana. |
| `{"minutes":[15,45], "hours":[5,17], "weekDays":["monday", "wednesday", "friday"]}` |Se ejecuta a las 5:15 a. m., 5:45 a. m., 5:15 p. m. y 5:45 p. m. los lunes, miércoles y viernes de cada semana. |
| `{"minutes":[0,15,30,45], "weekDays":["monday", "tuesday", "wednesday", "thursday", "friday"]}` |Se ejecuta cada 15 minutos los días laborables. |
| `{"minutes":[0,15,30,45], "hours": [9, 10, 11, 12, 13, 14, 15, 16] "weekDays":["monday", "tuesday", "wednesday", "thursday", "friday"]}` |Se ejecuta cada 15 minutos los días laborables entre las 9 a. m. y las 4:45 p.m. |
| `{"weekDays":["sunday"]}` |Se ejecuta los domingos a la hora de inicio. |
| `{"weekDays":["tuesday", "thursday"]}` |Se ejecuta los martes y los jueves a la hora de inicio. |
| `{"minutes":[0], "hours":[6], "monthDays":[28]}` |Se ejecuta a las 6 a. m. del día 28 de cada mes (suponiendo un valor de **frequency** de mes). |
| `{"minutes":[0], "hours":[6], "monthDays":[-1]}` |Se ejecuta a las 6 a. m. el último día del mes.<br /><br />Si desea ejecutar un trabajo en el último día del mes, use -1 en lugar de día 28, 29, 30 o 31. |
| `{"minutes":[0], "hours":[6], "monthDays":[1,-1]}` |Se ejecuta a las 6 a. m. el primer y el último día del mes. |
| `{monthDays":[1,-1]}` |Se ejecuta el primer y último día de cada mes a la hora de inicio. |
| `{monthDays":[1,14]}` |Se ejecuta el primer y el catorceavo día de cada mes a la hora de inicio. |
| `{monthDays":[2]}` |Se ejecuta el segundo día del mes a la hora de inicio. |
| `{"minutes":[0], "hours":[5], "monthlyOccurrences":[{"day":"friday", "occurrence":1}]}` |Se ejecuta el primer viernes de cada mes a las 5 a. m. |
| `{"monthlyOccurrences":[{"day":"friday", "occurrence":1}]}` |Se ejecuta el primer viernes de cada mes a la hora de inicio. |
| `{"monthlyOccurrences":[{"day":"friday", "occurrence":-3}]}` |Se ejecuta el tercer viernes desde el final del mes, todos los meses, a la hora de inicio. |
| `{"minutes":[15], "hours":[5], "monthlyOccurrences":[{"day":"friday", "occurrence":1},{"day":"friday", "occurrence":-1}]}` |Se ejecuta el primer viernes de cada mes a las 5:15 a. m. |
| `{"monthlyOccurrences":[{"day":"friday", "occurrence":1},{"day":"friday", "occurrence":-1}]}` |Se ejecuta el primer y último viernes de cada mes a la hora de inicio. |
| `{"monthlyOccurrences":[{"day":"friday", "occurrence":5}]}` |Se ejecuta el quinto viernes de cada mes a la hora de inicio.<br /><br />Si no existe el quinto viernes en un mes, el trabajo no se ejecuta. Puede considerar usar -1 en lugar de 5 para la repetición si desea ejecutar el trabajo en el último viernes del mes. |
| `{"minutes":[0,15,30,45], "monthlyOccurrences":[{"day":"friday", "occurrence":-1}]}` |Se ejecuta cada 15 minutos el último viernes del mes. |
| `{"minutes":[15,45], "hours":[5,17], "monthlyOccurrences":[{"day":"wednesday", "occurrence":3}]}` |Se ejecuta a las 5:15 a. m., 5:45 a. m., 5:15 a. m. y las 5:45 a. m. el tercer miércoles de cada mes. |

## <a name="next-steps"></a>Pasos siguientes

* [Conceptos, terminología y jerarquía de entidades de Azure Scheduler](scheduler-concepts-terms.md)
* [Referencia de API de REST de Azure Scheduler](/rest/api/scheduler)
* [Referencia de cmdlets de PowerShell de Azure Scheduler](scheduler-powershell-reference.md)
* [Límites, valores predeterminados y códigos de error de Azure Scheduler](scheduler-limits-defaults-errors.md)