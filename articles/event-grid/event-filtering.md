---
title: Filtrado de eventos para Azure Event Grid
description: Se describe cómo filtrar eventos al crear una suscripción de Azure Event Grid.
ms.topic: conceptual
ms.date: 07/07/2020
ms.openlocfilehash: 837209d4197c271598155776b8d171a705e1f454
ms.sourcegitcommit: 829d951d5c90442a38012daaf77e86046018e5b9
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/09/2020
ms.locfileid: "86120099"
---
# <a name="understand-event-filtering-for-event-grid-subscriptions"></a>Descripción del filtrado de eventos para suscripciones de Event Grid

En este artículo se describen las distintas formas de filtrar los eventos que se envían al punto de conexión. Al crear una suscripción de eventos, se le presentan tres opciones de filtrado:

* Tipos de eventos
* El asunto empieza o termina con
* Operadores y campos avanzados

## <a name="event-type-filtering"></a>Filtrado de tipos de evento

De forma predeterminada, todos los [tipos de evento](event-schema.md) del origen de eventos se envían al punto de conexión. Puede decidir enviar solo determinados tipos de eventos al punto de conexión. Por ejemplo, puede recibir una notificación de las actualizaciones de sus recursos pero no de otras operaciones, como las eliminaciones. En ese caso, filtre por el tipo de evento `Microsoft.Resources.ResourceWriteSuccess`. Proporcione una matriz con los tipos de eventos, o especifique `All` para obtener todos los tipos de evento para el origen del evento.

La sintaxis JSON para el filtrado por tipo de evento es:

```json
"filter": {
  "includedEventTypes": [
    "Microsoft.Resources.ResourceWriteFailure",
    "Microsoft.Resources.ResourceWriteSuccess"
  ]
}
```

## <a name="subject-filtering"></a>Filtrado de asunto

Para filtrar solo por asunto, especifique un valor inicial o final para el asunto. Por ejemplo, puede especificar que el asunto termina con `.txt` para obtener solo los eventos relacionados con la carga de un archivo de texto a la cuenta de almacenamiento. O bien, puede filtrar que el sujeto empieza por `/blobServices/default/containers/testcontainer` para obtener todos los eventos de ese contenedor, pero no otros contenedores en la cuenta de almacenamiento.

Al publicar eventos en temas personalizados, cree asuntos para los eventos que faciliten que los suscriptores sepan si les interesa el evento. Los suscriptores usan la propiedad del asunto para filtrar y redirigir eventos. Considere la posibilidad de agregar la ruta de acceso de donde se produjo el evento, para que los suscriptores pueden filtrar por los segmentos de esa ruta de acceso. La ruta de acceso permite que los suscriptores filtren eventos de una manera más amplia o más restringida. Si proporciona una ruta de acceso de tres segmentos como `/A/B/C` en el asunto, los suscriptores pueden filtrar por el primer segmento `/A` para obtener un amplio conjunto de eventos. Esos suscriptores obtienen eventos con asuntos como `/A/B/C` o `/A/D/E`. Otros suscriptores pueden filtrar por `/A/B` para obtener un conjunto de eventos más reducido.

La sintaxis JSON para el filtrado por asunto es:

```json
"filter": {
  "subjectBeginsWith": "/blobServices/default/containers/mycontainer/log",
  "subjectEndsWith": ".jpg"
}

```

## <a name="advanced-filtering"></a>Filtrado avanzado

Para filtrar por valores en los campos de datos y especificar el operador de comparación, use la opción de filtrado avanzada. En el filtrado avanzado, se especifica lo siguiente:

* tipo de operador: el tipo de comparación.
* clave: el campo de datos de evento que se usa para filtrar. Puede ser un número, un booleano o una cadena.
* valores: el valor o los valores que se compararán con la clave.

Si especifica un único filtro con varios valores, se realiza una operación **OR**, por lo que el valor del campo de clave debe ser uno de estos valores. Este es un ejemplo:

```json
"advancedFilters": [
    {
        "operatorType": "StringContains",
        "key": "Subject",
        "values": [
            "/providers/microsoft.devtestlab/",
            "/providers/Microsoft.Compute/virtualMachines/"
        ]
    }
]
```

Si especifica varios filtros diferentes, se realiza una operación **AND**, por lo que se debe cumplir cada condición de filtro. Este es un ejemplo: 

```json
"advancedFilters": [
    {
        "operatorType": "StringContains",
        "key": "Subject",
        "values": [
            "/providers/microsoft.devtestlab/"
        ]
    },
    {
        "operatorType": "StringContains",
        "key": "Subject",
        "values": [
            "/providers/Microsoft.Compute/virtualMachines/"
        ]
    }
]
```

### <a name="operators"></a>Operadores

Los operadores disponibles para los **números** son:

* NumberGreaterThan
* NumberGreaterThanOrEquals
* NumberLessThan
* NumberLessThanOrEquals
* NumberIn
* NumberNotIn

El operador disponible para los **booleanos** es: 
- BoolEquals

Los operadores disponibles para las **cadenas** son:

* StringContains
* StringBeginsWith
* StringEndsWith
* StringIn
* StringNotIn

Todas las comparaciones de cadenas **no** distinguen mayúsculas de minúsculas.

### <a name="key"></a>Clave

Para los eventos en el esquema de Event Grid, use los siguientes valores para la clave:

* ID
* Tema
* Asunto
* EventType
* DataVersion
* Datos del evento (por ejemplo, Data.key1)

Para los eventos en el esquema de Cloud Events, use los siguientes valores para la clave:

* EventId
* Source
* EventType
* EventTypeVersion
* Datos del evento (por ejemplo, Data.key1)

Para el esquema de entrada personalizado, use los campos de datos de evento (por ejemplo, Data.key1).

### <a name="values"></a>Valores

Los valores pueden ser:

* number
* string
* boolean
* array

### <a name="limitations"></a>Limitaciones

El filtrado avanzado tiene las siguientes limitaciones:

* 5 filtros avanzados y 25 valores de filtro en todos los filtros por suscripción de Event Grid
* 512 caracteres por valor de cadena
* 5 valores para los operadores **in** y **not in**
* Claves que incluyen el carácter **`.` (punto)** . Por ejemplo, `http://schemas.microsoft.com/claims/authnclassreference` o `john.doe@contoso.com`. Actualmente, las claves no admiten caracteres de escape. 

La misma clave se puede usar en más de un filtro.

### <a name="examples"></a>Ejemplos

### <a name="stringcontains"></a>StringContains

```json
"advancedFilters": [{
    "operatorType": "StringContains",
    "key": "data.key1",
    "values": [
        "microsoft", 
        "azure"
    ]
}]
```

### <a name="stringbeginswith"></a>StringBeginsWith

```json
"advancedFilters": [{
    "operatorType": "StringBeginsWith",
    "key": "data.key1",
    "values": [
        "event", 
        "grid"
    ]
}]
```

### <a name="stringendswith"></a>StringEndsWith

```json
"advancedFilters": [{
    "operatorType": "StringEndsWith",
    "key": "data.key1",
    "values": [
        "jpg", 
        "jpeg", 
        "png"
    ]
}]
```

### <a name="stringin"></a>StringIn

```json
"advancedFilters": [{
    "operatorType": "StringIn",
    "key": "data.key1",
    "values": [
        "exact", 
        "string", 
        "matches"
    ]
}]
```

### <a name="stringnotin"></a>StringNotIn

```json
"advancedFilters": [{
    "operatorType": "StringNotIn",
    "key": "data.key1",
    "values": [
        "aws", 
        "bridge"
    ]
}]
```

### <a name="numberin"></a>NumberIn

```json

"advancedFilters": [{
    "operatorType": "NumberIn",
    "key": "data.counter",
    "values": [
        5,
        1
    ]
}]

```

### <a name="numbernotin"></a>NumberNotIn

```json
"advancedFilters": [{
    "operatorType": "NumberNotIn",
    "key": "data.counter",
    "values": [
        41,
        0,
        0
    ]
}]
```

### <a name="numberlessthan"></a>NumberLessThan

```json
"advancedFilters": [{
    "operatorType": "NumberLessThan",
    "key": "data.counter",
    "value": 100
}]
```

### <a name="numbergreaterthan"></a>NumberGreaterThan

```json
"advancedFilters": [{
    "operatorType": "NumberGreaterThan",
    "key": "data.counter",
    "value": 20
}]
```

### <a name="numberlessthanorequals"></a>NumberLessThanOrEquals

```json
"advancedFilters": [{
    "operatorType": "NumberLessThanOrEquals",
    "key": "data.counter",
    "value": 100
}]
```

### <a name="numbergreaterthanorequals"></a>NumberGreaterThanOrEquals

```json
"advancedFilters": [{
    "operatorType": "NumberGreaterThanOrEquals",
    "key": "data.counter",
    "value": 30
}]
```

### <a name="boolequals"></a>BoolEquals

```json
"advancedFilters": [{
    "operatorType": "BoolEquals",
    "key": "data.isEnabled",
    "value": true
}]
```


## <a name="next-steps"></a>Pasos siguientes

* Para obtener información sobre el filtrado de eventos con PowerShell y la CLI de Azure, consulte [Filter events for Event Grid](how-to-filter-events.md) (Filtrado de eventos para Event Grid).
* Para comenzar a usar rápidamente Event Grid, vea [Creación y enrutamiento de eventos personalizados con Azure Event Grid](custom-event-quickstart.md).
