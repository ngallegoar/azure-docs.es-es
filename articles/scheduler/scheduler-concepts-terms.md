---
title: Conceptos, términos y entidades
description: Aprenda los conceptos, terminología y jerarquía de entidades, incluidos trabajos y colecciones de trabajos de Azure Scheduler.
services: scheduler
ms.service: scheduler
ms.suite: infrastructure-services
author: derek1ee
ms.author: deli
ms.reviewer: klam, estfan
ms.topic: conceptual
ms.date: 08/18/2016
ms.openlocfilehash: 0a744c2de320ddad2e7959cae7b62d7990879953
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/28/2020
ms.locfileid: "78898573"
---
# <a name="concepts-terminology-and-entities-in-azure-scheduler"></a>Conceptos, terminología y entidades de Azure Scheduler

> [!IMPORTANT]
> [Azure Logic Apps](../logic-apps/logic-apps-overview.md) reemplaza a Azure Scheduler, que se [va a retirar](../scheduler/migrate-from-scheduler-to-logic-apps.md#retire-date). Para seguir utilizando los trabajos configurados en Scheduler, [migre a Azure Logic Apps](../scheduler/migrate-from-scheduler-to-logic-apps.md) cuanto antes. 
>
> Scheduler ya no está disponible en Azure portal, pero la [API REST](/rest/api/scheduler) y los [cmdlets de PowerShell para Azure Scheduler](scheduler-powershell-reference.md) siguen disponibles en la actualidad para que pueda administrar los trabajos y las colecciones de trabajos.

## <a name="entity-hierarchy"></a>Jerarquía de entidades

La API REST de Azure Scheduler expone y usa estas entidades principales o los recursos:

| Entidad | Descripción |
|--------|-------------|
| **Trabajo** | Define una única acción periódica, con estrategias simples o complejas para su ejecución. Las acciones pueden incluir solicitudes HTTP, de cola de Storage, de cola de Service Bus o de tema de Service Bus. | 
| **Colección de trabajos** | Contiene un grupo de trabajos y mantiene configuraciones, cuotas y aceleradores compartidos por los trabajos de la colección. Como propietario de la suscripción de Azure, puede crear colecciones de trabajos y agrupar los trabajos en función de los límites de uso o de la aplicación. Una colección de trabajos tiene estos atributos: <p>- Está limitada a una región. <br>- Le permite aplicar cuotas para que pueda restringir el uso de todos los trabajos de una colección. <br>- Las cuotas incluyen MaxJobs y MaxRecurrence. | 
| **Historial de trabajos** | Describe los detalles de ejecución de un trabajo, por ejemplo, el estado y los detalles de la respuesta. |
||| 

## <a name="entity-management"></a>Administración de entidades

A grandes rasgos, la API REST de Scheduler expone estas operaciones para administrar entidades.

### <a name="job-management"></a>Administración de trabajos

Admite operaciones para crear y editar trabajos. Todos los trabajos deben pertenecer a una colección de trabajos existente, así que no hay ninguna creación implícita. Para más información, consulte [API REST de Scheduler: Trabajos](https://docs.microsoft.com/rest/api/scheduler/jobs). Esta es la dirección del identificador URI de estas operaciones:

```
https://management.azure.com/subscriptions/{subscriptionID}/resourceGroups/{resourceGroupName}/providers/Microsoft.Scheduler/jobCollections/{jobCollectionName}/jobs/{jobName}
```

### <a name="job-collection-management"></a>Administración de la colección de trabajos

Admite operaciones para crear y editar trabajos y colecciones de trabajos, que se asignan a las cuotas y a la configuración compartida. Por ejemplo, las cuotas especifican el número máximo de trabajos y un intervalo menor de periodicidad. Para más información, consulte [API REST de Scheduler: Colecciones de trabajos](https://docs.microsoft.com/rest/api/scheduler/jobcollections). Esta es la dirección del identificador URI de estas operaciones:

```
https://management.azure.com/subscriptions/{subscriptionID}/resourceGroups/{resourceGroupName}/providers/Microsoft.Scheduler/jobCollections/{jobCollectionName}
```

### <a name="job-history-management"></a>Administración del historial de trabajos

Admite la operación GET para recuperar 60 días de historial de ejecución de trabajos, por ejemplo, el tiempo de trabajo transcurrido y los resultados de la ejecución del trabajo. Incluye compatibilidad de parámetros de cadena de consulta para filtrar basándose en el estado y la situación. Para más información, consulte [API REST de Scheduler: Trabajos - Enumeración del historial de trabajos](https://docs.microsoft.com/rest/api/scheduler/jobs/listjobhistory). Esta es la dirección del identificador URI de esta operación:

```
https://management.azure.com/subscriptions/{subscriptionID}/resourceGroups/{resourceGroupName}/providers/Microsoft.Scheduler/jobCollections/{jobCollectionName}/jobs/{jobName}/history
```

## <a name="job-types"></a>Tipos de trabajo

Azure Scheduler admite varios tipos de trabajo: 

* Trabajos HTTP, incluidos los trabajos HTTPS que admiten SSL, para cuando tenga el punto de conexión de un servicio o carga de trabajo existente
* Trabajos de la cola de almacenamiento para cargas de trabajo que utilizan colas de Storage, como la publicación de mensajes en colas de Storage
* Trabajos de cola de Service Bus para cargas de trabajo que usan colas de Service Bus
* Trabajos de tema de Service Bus para cargas de trabajo que usan temas de Service Bus

## <a name="job-definition"></a>Definición del trabajo

En el nivel superior, un trabajo de Scheduler tiene estas partes básicas:

* La acción que se ejecuta cuando se activa el temporizador de trabajo
* Opcional: El tiempo para ejecutar el trabajo
* Opcional: Cuándo y con qué frecuencia se debe repetir el trabajo
* Opcional: Una acción de error que se ejecuta si se produce un error en la acción principal

El trabajo también incluye datos proporcionados por el sistema, como el siguiente entorno en tiempo de ejecución programado del trabajo. La definición de código del trabajo es un objeto en formato JavaScript Object Notation (JSON), que tiene estos elementos:

| Elemento | Obligatorio | Descripción | 
|---------|----------|-------------| 
| [**startTime**](#start-time) | No | La hora de inicio para el trabajo con un desplazamiento de zona horaria en [formato ISO 8601](https://en.wikipedia.org/wiki/ISO_8601) | 
| [**action**](#action) | Sí | Los detalles de la acción principal, que puede incluir un objeto **errorAction** | 
| [**errorAction**](#error-action) | No | Los detalles de la acción secundaria que se ejecuta si se produce un error en la acción principal |
| [**recurrence**](#recurrence) | No | Los detalles como la frecuencia y el intervalo para un trabajo periódico | 
| [**retryPolicy**](#retry-policy) | No | Los detalles de la frecuencia de reintentos de una acción | 
| [**state**](#state) | Sí | Los detalles del estado actual del trabajo |
| [**status**](#status) | Sí | Los detalles del estado actual del trabajo, que está controlado por el servicio |
||||

Este es un ejemplo que muestra una definición de trabajo completa para una acción HTTP con detalles más completos de los elementos que se describen en secciones posteriores: 

```json
"properties": {
   "startTime": "2012-08-04T00:00Z",
   "action": {
      "type": "Http",
      "request": {
         "uri": "http://contoso.com/some-method", 
         "method": "PUT",          
         "body": "Posting from a timer",
         "headers": {
            "Content-Type": "application/json"
         },
         "retryPolicy": { 
             "retryType": "None" 
         },
      },
      "errorAction": {
         "type": "Http",
         "request": {
            "uri": "http://contoso.com/notifyError",
            "method": "POST"
         }
      }
   },
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
   "state": "Disabled",
   "status": {
      "lastExecutionTime": "2007-03-01T13:00:00Z",
      "nextExecutionTime": "2007-03-01T14:00:00Z ",
      "executionCount": 3,
      "failureCount": 0,
      "faultedCount": 0
   }
}
```

<a name="start-time"></a>

## <a name="starttime"></a>startTime

En el objeto **startTime**, puede especificar la hora de inicio y un desplazamiento de la zona horaria en [formato ISO 8601](https://en.wikipedia.org/wiki/ISO_8601).

<a name="action"></a>

## <a name="action"></a>action

El trabajo de Scheduler ejecuta una **acción** principal según la programación especificada. Scheduler admite acciones HTTP, de cola de Storage, de cola de Service Bus y de tema de Service Bus. Si se produce un error en el objeto **action** principal, Scheduler puede ejecutar un objeto [**errorAction**](#erroraction) secundario que se encargue del error. El objeto **action** describe estos elementos:

* El tipo de servicio de la acción
* Los detalles de la acción
* Un objeto **errorAction** alternativo

El ejemplo anterior describe una acción HTTP. A continuación se muestra un ejemplo de una acción de cola de Storage:

```json
"action": {
   "type": "storageQueue",
   "queueMessage": {
      "storageAccount": "myStorageAccount",  
      "queueName": "myqueue",                
      "sasToken": "TOKEN",                   
      "message": "My message body"
    }
}
```

A continuación se muestra un ejemplo de una acción de cola de Service Bus:

```json
"action": {
   "type": "serviceBusQueue",
   "serviceBusQueueMessage": {
      "queueName": "q1",  
      "namespace": "mySBNamespace",
      "transportType": "netMessaging", // Either netMessaging or AMQP
      "authentication": {  
         "sasKeyName": "QPolicy",
         "type": "sharedAccessKey"
      },
      "message": "Some message",  
      "brokeredMessageProperties": {},
      "customMessageProperties": {
         "appname": "FromScheduler"
      }
   }
},
```

A continuación se muestra un ejemplo de una acción de tema de Service Bus:

```json
"action": {
   "type": "serviceBusTopic",
   "serviceBusTopicMessage": {
      "topicPath": "t1",  
      "namespace": "mySBNamespace",
      "transportType": "netMessaging", // Either netMessaging or AMQP
      "authentication": {
         "sasKeyName": "QPolicy",
         "type": "sharedAccessKey"
      },
      "message": "Some message",
      "brokeredMessageProperties": {},
      "customMessageProperties": {
         "appname": "FromScheduler"
      }
   }
},
```

Para más información sobre los tokens de Firma de acceso compartido (SAS), consulte [Uso de firmas de acceso compartido (SAS)](../storage/common/storage-dotnet-shared-access-signature-part-1.md).

<a name="error-action"></a>

## <a name="erroraction"></a>errorAction

Si se produce un error en el objeto **action** principal del trabajo, Scheduler puede ejecutar un objeto **errorAction** secundario que se encargue del error. En el objeto **action** principal, puede especificar un objeto **errorAction** para que Scheduler pueda llamar a un punto de conexión de tratamiento de errores o enviar una notificación al usuario. 

Por ejemplo, si ocurre un desastre en el punto de conexión principal, puede utilizar el objeto **errorAction** para llamar a un punto de conexión secundario o para notificar un punto de conexión de control de errores. 

Igual que la **acción** primaria, puede hacer que la acción de error utilice una lógica simple o compuesta basada en otras acciones. 

<a name="recurrence"></a>

## <a name="recurrence"></a>recurrence

Un trabajo se vuelve a producir si la definición de JSON del trabajo incluye el objeto **recurrence**, por ejemplo:

```json
"recurrence": {
   "frequency": "Week",
   "interval": 1,
   "schedule": {
      "hours": [10, 22],
      "minutes": [0, 30],
      "weekDays": ["Monday", "Wednesday", "Friday"]
   },
   "count": 10,
   "endTime": "2012-11-04"
},
```

| Propiedad | Obligatorio | Value | Descripción | 
|----------|----------|-------|-------------| 
| **frequency** | Sí, cuando se usa **recurrence** | "Minute", "Hour", "Day", "Week", "Month", "Year" | La unidad de tiempo entre las repeticiones | 
| **interval** | No | 1 a 1000, ambos inclusive | Un entero positivo que determina el número de unidades de tiempo entre cada repetición según la **frecuencia** | 
| **schedule** | No | Varía | Los detalles de las programaciones más complejas y avanzadas. Consulte **hours**, **minutes**, **weekDays**, **months** y **monthDays** | 
| **hours** | No | De 1 a 24 | Una matriz con las marcas de hora para saber cuándo ejecutar el trabajo | 
| **minutes** | No | De 0 a 59 | Una matriz con las marcas de minutos para saber cuándo ejecutar el trabajo | 
| **months** | No | De 1 a 12 | Una matriz con los meses en los que se puede ejecutar el trabajo | 
| **monthDays** | No | Varía | Una matriz con los días del mes en los que se puede ejecutar el trabajo | 
| **weekDays** | No | "Monday", "Tuesday", "Wednesday", "Thursday", "Friday", "Saturday", "Sunday" | Una matriz con los días de la semana en los que se puede ejecutar el trabajo | 
| **count** | No | <*none*> | El número de repeticiones. El valor predeterminado es que se repita indefinidamente. No se puede usar **count** y **endTime** a la vez, pero se cumple la regla del que finaliza primero. | 
| **endTime** | No | <*none*> | La fecha y hora en la que se detiene la periodicidad. El valor predeterminado es que se repita indefinidamente. No se puede usar **count** y **endTime** a la vez, pero se cumple la regla del que finaliza primero. | 
||||

Para obtener más información acerca de estos elementos, consulte [Creación de programaciones complejas y periodicidad avanzada](../scheduler/scheduler-advanced-complexity.md).

<a name="retry-policy"></a>

## <a name="retrypolicy"></a>retryPolicy

Para el caso de que un trabajo de Scheduler pueda producir un error, puede configurar una directiva de reintentos, que determina si Scheduler reintenta la acción y cómo lo hace. De manera predeterminada, Scheduler reintenta el trabajo otras cuatro veces, a intervalos de 30 segundos. Puede hacer que esta directiva sea más o menos agresiva, por ejemplo, esta directiva reintenta una acción dos veces al día:

```json
"retryPolicy": { 
   "retryType": "Fixed",
   "retryInterval": "PT1D",
   "retryCount": 2
},
```

| Propiedad | Obligatorio | Value | Descripción | 
|----------|----------|-------|-------------| 
| **retryType** | Sí | **Fixed**, **None** | Determina si se especifica una directiva de reintentos (**fixed**) o no (**none**). | 
| **retryInterval** | No | PT30S | Especifica el intervalo y la frecuencia entre los reintentos en [ formato ISO 8601](https://en.wikipedia.org/wiki/ISO_8601#Combined_date_and_time_representations). El valor mínimo es de 15 segundos, mientras que el valor máximo es de 18 meses. | 
| **retryCount** | No | 4 | Especifica el número de reintentos. El valor máximo es 20. | 
||||

Para más información, consulte [Alta disponibilidad y confiabilidad de Scheduler](../scheduler/scheduler-high-availability-reliability.md).

<a name="status"></a>

## <a name="state"></a>state

El estado de un trabajo es **Enabled** (Habilitado), **Disabled** (Deshabilitado), **Completed** (Completado) o **Faulted** (Con errores), por ejemplo: 

`"state": "Disabled"`

Para cambiar los trabajos a estado **Habilitado** o **Deshabilitado**, puede usar la operación PUT o PATCH en esos trabajos.
Sin embargo, si un trabajo tiene el estado **Completado** o **Con errores**, no puede actualizar el estado, aunque puede realizar la operación DELETE en el trabajo. Scheduler elimina los trabajos completados y con errores después de 60 días. 

<a name="status"></a>

## <a name="status"></a>status

Después de que se inicia un trabajo, Scheduler devuelve información sobre el estado del trabajo mediante el objeto **status**, que solo controla Scheduler. Sin embargo, puede encontrar el objeto **status** dentro del objeto **job**. Esta es la información que incluye el estado de un trabajo:

* Hora de la ejecución anterior, si la hubiera
* Hora de la siguiente ejecución programada de los trabajos en curso
* El número de ejecuciones de trabajos
* El número de errores, si existen.
* El número de errores, si existen

Por ejemplo:

```json
"status": {
   "lastExecutionTime": "2007-03-01T13:00:00Z",
   "nextExecutionTime": "2007-03-01T14:00:00Z ",
   "executionCount": 3,
   "failureCount": 0,
   "faultedCount": 0
}
```

## <a name="next-steps"></a>Pasos siguientes

* [Creación de programaciones complejas y periodicidad avanzada](scheduler-advanced-complexity.md)
* [Referencia de API de REST de Azure Scheduler](/rest/api/scheduler)
* [Referencia de cmdlets de PowerShell de Azure Scheduler](scheduler-powershell-reference.md)
* [Límites, cuotas, valores predeterminados y códigos de error](scheduler-limits-defaults-errors.md)
