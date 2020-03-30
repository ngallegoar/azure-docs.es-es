---
title: 'Depuración de las UDF: Azure Digital Twins | Microsoft Docs'
description: Conozca las estrategias recomendadas para depurar funciones definidas por el usuario en Azure Digital Twins.
ms.author: alinast
author: alinamstanciu
manager: bertvanhoof
ms.service: digital-twins
services: digital-twins
ms.topic: conceptual
ms.date: 01/21/2020
ms.custom: seodec18
ms.openlocfilehash: 518383488aa878dab75aec7ad5da664332b62ad0
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/27/2020
ms.locfileid: "76511644"
---
# <a name="how-to-debug-user-defined-functions-in-azure-digital-twins"></a>Depuración de funciones definidas por el usuario en Azure Digital Twins

En este artículo se resume cómo diagnosticar y depurar las funciones definidas por el usuario en Azure Digital Twins. A continuación, se identifican algunos de los escenarios más comunes que se producen al depurarlas.

>[!TIP]
> Lea [Configuración de la supervisión y el registro](./how-to-configure-monitoring.md) para obtener más información sobre cómo configurar las herramientas de depuración en Azure Digital Twins mediante registros de actividad, registros de diagnóstico y Azure Monitor.

## <a name="debug-issues"></a>Depuración de problemas

Saber cómo diagnosticar problemas en Azure Digital Twins permite analizarlos de forma eficaz, identificar sus causas y proporcionar soluciones adecuadas a los mismos.

Para tal fin se proporcionan varias herramientas de diagnóstico, análisis y registro.

### <a name="enable-logging-for-your-instance"></a>Habilitación del registro en la instancia

Azure Digital Twins es compatible con un eficaz sistema de registro, supervisión y análisis. Los desarrolladores de soluciones pueden usar los registros de Azure Monitor, los registros de diagnóstico, el registro de actividad y otros servicios para dar soporte a las complejas necesidades de supervisión de una aplicación de IoT. Las opciones de registro se pueden combinar para consultar o mostrar registros en diferentes servicios y ofrecer una cobertura de registro pormenorizada para muchos servicios.

* Para ver una configuración de registro específica de Azure Digital Twins, lea [Configuración de la supervisión y el registro](./how-to-configure-monitoring.md).
* Consulte la introducción a [Azure Monitor](../azure-monitor/overview.md) para obtener información acerca de la eficaz configuración del registro que se habilita a través de Azure Monitor.
* Lea el artículo [Recopilación y consumo de datos de registro provenientes de los recursos de Azure](../azure-monitor/platform/platform-logs-overview.md) para configurar el registro de diagnóstico en Azure Digital Twins mediante Azure Portal, la CLI de Azure o PowerShell.

Una vez configurado, podrá seleccionar todas las categorías de registro y métricas, así como usar áreas de trabajo del análisis del registro de Azure Monitor para dar soporte a sus esfuerzos de depuración.

### <a name="trace-sensor-telemetry"></a>Seguimiento de la telemetría del sensor

Para hacer un seguimiento de la telemetría del sensor, verifique que la configuración de diagnóstico esté habilitada para su instancia de Azure Digital Twins. A continuación, asegúrese de que todas las categorías de registro deseadas están seleccionadas. Por último, confirme que los registros deseados se envían a los registros de Azure Monitor.

Para hacer coincidir un mensaje de telemetría de sensor con sus registros correspondientes, puede especificar un identificador de correlación en los datos del evento que se envían. Para ello, establezca la propiedad `x-ms-client-request-id` en un GUID.

Después de enviar la telemetría, abra el análisis de registros de Azure Monitor para consultar los registros con el identificador de correlación configurado:

```Kusto
AzureDiagnostics
| where CorrelationId == 'YOUR_CORRELATION_IDENTIFIER'
```

| Valor de la consulta | Reemplazar por |
| --- | --- |
| YOUR_CORRELATION_IDENTIFIER | El identificador de correlación que se especificó en los datos del evento |

Para leer todas las consultas de registros de telemetría recientes:

```Kusto
AzureDiagnostics
| order by CorrelationId desc
```

Si habilita el registro para su función definida por el usuario, los registros aparecen en su instancia de análisis de registros con la categoría `UserDefinedFunction`. Para recuperarlos, escriba la siguiente condición de consulta en el análisis de registros:

```Kusto
AzureDiagnostics
| where Category == 'UserDefinedFunction'
```

Para obtener más información acerca de estas eficaces operaciones de consulta, lea [Introducción a las consultas](../azure-monitor/log-query/get-started-queries.md).

## <a name="identify-common-issues"></a>Identificación de los problemas comunes

Es importante tanto diagnosticar como identificar los problemas más comunes para buscar una solución. En las siguientes subsecciones se resumen varios problemas comunes encontrados al desarrollar funciones definidas por el usuario.

[!INCLUDE [Digital Twins Management API](../../includes/digital-twins-management-api.md)]

### <a name="check-if-a-role-assignment-was-created"></a>Asegúrese de que se creó una asignación de roles

Sin no se crea una asignación de roles dentro de la API de administración, la función definida por el usuario no tiene acceso para realizar acciones como enviar notificaciones, recuperar metadatos y configurar valores calculados dentro de la topología.

Compruebe si existe una asignación de roles para la función definida por el usuario a través de la API de administración:

```URL
GET YOUR_MANAGEMENT_API_URL/roleassignments?path=/&traverse=Down&objectId=YOUR_USER_DEFINED_FUNCTION_ID
```

| Valor del parámetro | Reemplazar por |
| --- | --- |
| YOUR_USER_DEFINED_FUNCTION_ID | El identificador de la función definida por el usuario para la que recuperar las asignaciones de roles|

Descubra cómo [crear una asignación de roles para su función definida por el usuario](./how-to-user-defined-functions.md), si no existe ninguna asignación de roles.

### <a name="check-if-the-matcher-works-for-a-sensors-telemetry"></a>Compruebe si funciona el buscador de coincidencias para la telemetría de un sensor

Con la siguiente llamada a la API de administración de las instancias de Azure Digital Twins, puede determinar si se aplica un buscador de coincidencias determinado para el sensor específico.

```URL
GET YOUR_MANAGEMENT_API_URL/matchers/YOUR_MATCHER_IDENTIFIER/evaluate/YOUR_SENSOR_IDENTIFIER?enableLogging=true
```

| Parámetro | Reemplazar por |
| --- | --- |
| *YOUR_MATCHER_IDENTIFIER* | El identificador del buscador de coincidencias que quiere evaluar |
| *YOUR_SENSOR_IDENTIFIER* | El identificador del sensor que quiere evaluar |

Respuesta:

```JavaScript
{
    "success": true,
    "logs": [
        "$.dataType: \"Motion\" Equals \"Motion\" => True"
    ]
}
```

### <a name="check-what-a-sensor-triggers"></a>Compruebe lo que desencadena un sensor

Con la siguiente llamada a las API de administración de Azure Digital Twins, puede determinar los identificadores de las funciones definidas por el usuario que desencadenan la telemetría entrante del sensor determinado:

```URL
GET YOUR_MANAGEMENT_API_URL/sensors/YOUR_SENSOR_IDENTIFIER/matchers?includes=UserDefinedFunctions
```

| Parámetro | Reemplazar por |
| --- | --- |
| *YOUR_SENSOR_IDENTIFIER* | El identificador del sensor para enviar la telemetría |

Respuesta:

```JavaScript
[
    {
        "id": "48a64768-797e-4832-86dd-de625f5f3fd9",
        "name": "MotionMatcher",
        "spaceId": "2117b3e1-b6ce-42c1-9b97-0158bef59eb7",
        "conditions": [
            {
                "id": "024a067a-414f-415b-8424-7df61392541e",
                "target": "Sensor",
                "path": "$.dataType",
                "value": "\"Motion\"",
                "comparison": "Equals"
            }
        ],
        "userDefinedFunctions": [
            {
                "id": "373d03c5-d567-4e24-a7dc-f993460120fc",
                "spaceId": "2117b3e1-b6ce-42c1-9b97-0158bef59eb7",
                "name": "Motion User-Defined Function",
                "disabled": false
            }
        ]
    }
]
```

### <a name="issue-with-receiving-notifications"></a>Problema con la recepción de notificaciones

Si no recibe notificaciones desde dentro de la función definida por el usuario desencadenada, asegúrese de que su parámetro de tipo de objeto de topología coincide con el tipo del identificador que se utiliza.

Ejemplo **incorrecto**:

```JavaScript
var customNotification = {
    Message: 'Custom notification that will not work'
};

sendNotification(telemetry.SensorId, "Space", JSON.stringify(customNotification));
```

Esta situación se produce porque el identificador usado hace referencia a un sensor mientras el tipo de objeto de la topología especificado es `Space`.

Ejemplo **correcto**:

```JavaScript
var customNotification = {
    Message: 'Custom notification that will work'
};

sendNotification(telemetry.SensorId, "Sensor", JSON.stringify(customNotification));
```

La manera más fácil de no experimentar este problema es usar el método `Notify` en el objeto de metadatos.

Ejemplo:

```JavaScript
function process(telemetry, executionContext) {
    var sensorMetadata = getSensorMetadata(telemetry.SensorId);

    var customNotification = {
        Message: 'Custom notification'
    };

    // Short-hand for above methods where object type is known from metadata.
    sensorMetadata.Notify(JSON.stringify(customNotification));
}
```

## <a name="common-diagnostic-exceptions"></a>Excepciones de diagnóstico comunes

Si habilita la configuración de diagnóstico, es posible que se produzcan estas excepciones comunes:

1. **Limitación**: la función definida por el usuario se restringirá si supera los límites de la tasa de ejecución descrita en el artículo [Service Limits](./concepts-service-limits.md) (Límites del servicio). Ninguna operación se ejecutará correctamente hasta que expiren los límites.

1. **No se encontraron datos**: si la función definida por el usuario intenta tener acceso a metadatos que no existen, se produce un error en la operación.

1. **No autorizado**: si la función definida por el usuario no tiene una asignación de roles establecida o no tiene permisos suficientes para tener acceso a ciertos metadatos de la topología, se produce un error en la operación.

## <a name="next-steps"></a>Pasos siguientes

- Obtenga información sobre cómo habilitar [la supervisión y los registros](./how-to-configure-monitoring.md) en Azure Digital Twins.

- Lea el artículo [Información general del registro de actividad de Azure](../azure-monitor/platform/platform-logs-overview.md) para conocer más opciones de registro.
