---
title: 'Procesadores de telemetría (versión preliminar): Application Insights Java de Azure Monitor'
description: Procesadores de telemetría para Application Insights Java de Azure Monitor
ms.topic: conceptual
ms.date: 10/29/2020
ms.openlocfilehash: e3c41a7a9968a7de743f0c513b1f2b194501d0df
ms.sourcegitcommit: 0dcafc8436a0fe3ba12cb82384d6b69c9a6b9536
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 11/10/2020
ms.locfileid: "94425803"
---
# <a name="telemetry-processors-preview-for-azure-monitor-application-insights-java"></a>Procesadores de telemetría (versión preliminar) para Application Insights Java de Azure Monitor

> [!NOTE]
> Esta característica aún sigue en la fase de versión preliminar.

Actualmente, el agente 3.0 de Java para Application Insights tiene las funcionalidades necesarias para procesar los datos de telemetría antes de que estos se exporten.

### <a name="some-use-cases"></a>Algunos casos de uso:
 * Enmascaran datos confidenciales
 * Agregan dimensiones personalizadas condicionalmente
 * Actualizan el nombre de telemetría que se usa para la agregación y visualización

### <a name="supported-processors"></a>Procesadores compatibles:
 * Procesador de atributos
 * Procesador de intervalos

## <a name="to-get-started"></a>Inicio

Cree un archivo de configuración denominado `applicationinsights.json` y colóquelo en el mismo directorio que `applicationinsights-agent-***.jar`, con la siguiente plantilla.

```json
{
  "connectionString": "InstrumentationKey=00000000-0000-0000-0000-000000000000",
  "preview": {
    "processors": [
      {
        "type": "attribute",
        ...
      },
      {
        "type": "attribute",
        ...
      },
      {
        "type": "span",
        ...
      }
    ]
  }
}
```

## <a name="includeexclude-spans"></a>Inclusión o exclusión de intervalos

El procesador de atributos y el procesador de intervalos exponen la opción de proporcionar un conjunto de propiedades de un intervalo con el que coincidir, con el fin de determinar si el intervalo debe incluirse o excluirse del procesador. Para configurar esta opción, en `include` o `exclude`, al menos son necesarios un elemento `matchType` y uno de estos dos: `spanNames` o `attributes`. La configuración de inclusión o exclusión se admite para tener más de una condición especificada. Todas las condiciones especificadas deben evaluarse en true para que se produzca una coincidencia. 

**Campo obligatorio**: 
* `matchType` controla cómo se interpretan los elementos de las matrices `spanNames` y `attributes`. Los valores posibles son `regexp` o `strict`. 

**Campos opcionales**: 
* `spanNames` debe coincidir con al menos uno de los elementos. 
* `attributes` especifica la lista de atributos con la que coincidir. Todos estos atributos deben coincidir exactamente para que se produzca una coincidencia.

> [!NOTE]
> Si se especifican `include` y `exclude`, se comprobarán las propiedades `include` antes que las propiedades `exclude`.

#### <a name="sample-usage"></a>Ejemplo de uso

A continuación, se muestra cómo especificar el conjunto de propiedades de intervalo para indicar los intervalos a los que se debe aplicar este procesador. Las propiedades `include` indican cuáles deben incluirse y las propiedades `exclude` filtran aún más los intervalos que no se deben procesar.

```json
{
  "connectionString": "InstrumentationKey=00000000-0000-0000-0000-000000000000",
  "preview": {
    "processors": [
      {
        "type": "attribute",
        "include": {
          "matchType": "strict",
          "spanNames": [
            "svcA",
            "svcB"
          ]
        },
        "exclude": {
          "matchType": "strict",
          "attributes": [
            {
              "key": "redact_trace",
              "value": "false"
            }
          ]
        },
        "actions": [
          {
            "key": "credit_card",
            "action": "delete"
          },
          {
            "key": "duplicate_key",
            "action": "delete"
          }
        ]
      }
    ]
  }
}
```

Con la configuración anterior, los siguientes intervalos coinciden con las propiedades y se aplican acciones del procesador:

* Intervalo1 Nombre: 'svcB' Atributos: {env: producción, test_request: 123, credit_card: 1234, redact_trace: "false"}

* Intervalo2 Nombre: 'svcA' Atributos: {env: almacenamiento provisional, test_request: false, redact_trace: true}

Los siguientes intervalos no coinciden con las propiedades de inclusión y no se aplican acciones del procesador:

* Intervalo3 Nombre: 'svcB' Atributos: {env: producción, test_request: true, credit_card: 1234, redact_trace: false}

* Intervalo4 Nombre: 'svcC' Atributos: {env: des, test_request: false}

## <a name="attribute-processor"></a>Procesador de atributos 

El procesador de atributos modifica los atributos de un intervalo. Opcionalmente, admite la capacidad de incluir o excluir intervalos.
Toma una lista de acciones que se realizan en el orden especificado en el archivo de configuración. Las acciones admitidas son:

* `insert`: inserta un nuevo atributo en intervalos en los que la clave aún no existe
* `update`: actualiza un atributo en intervalos en los que la clave existe
* `delete`: elimina un atributo de un intervalo
* `hash`: aplica el algoritmo hash (SHA1) a un valor de atributo existente

Para las acciones `insert` y `update`
* `key` es obligatorio
* uno de estos dos elementos, `value` o `fromAttribute`, es obligatorio
* `action` es obligatorio.

Para la acción `delete`,
* `key` es obligatorio
* `action`: `delete` es obligatorio.

Para la acción `hash`,
* `key` es obligatorio
* `action` : `hash` es obligatorio.

La lista de acciones puede componerse para crear escenarios enriquecidos como, por ejemplo, el atributo de reposición, la copia de valores en una nueva clave y la redacción de información confidencial.

#### <a name="sample-usage"></a>Ejemplo de uso

En el ejemplo siguiente se muestra cómo insertar claves o valores en intervalos:

```json
{
  "connectionString": "InstrumentationKey=00000000-0000-0000-0000-000000000000",
  "preview": {
    "processors": [
      {
        "type": "attribute",
        "actions": [
          {
            "key": "attribute1",
            "value": "value1",
            "action": "insert"
          },
          {
            "key": "key1",
            "fromAttribute": "anotherkey",
            "action": "insert"
          }
        ]
      }
    ]
  }
}
```

En el ejemplo siguiente se muestra cómo configurar el procesador para actualizar solo las claves existentes en un atributo:

```json
{
  "connectionString": "InstrumentationKey=00000000-0000-0000-0000-000000000000",
  "preview": {
    "processors": [
      {
        "type": "attribute",
        "actions": [
          {
            "key": "piiattribute",
            "value": "redacted",
            "action": "update"
          },
          {
            "key": "credit_card",
            "action": "delete"
          },
          {
            "key": "user.email",
            "action": "hash"
          }
        ]
      }
    ]
  }
}
```

En el ejemplo siguiente se muestra cómo procesar intervalos que tienen un nombre de intervalo que coincide con los patrones regexp.
Este procesador quitará el atributo "token" y ofuscará el atributo "password" en intervalos donde el nombre del intervalo coincide con "auth.\*" y donde el nombre del intervalo no coincide con "login.\*".

```json
{
  "connectionString": "InstrumentationKey=00000000-0000-0000-0000-000000000000",
  "preview": {
    "processors": [
      {
        "type": "attribute",
        "include": {
          "matchType": "regexp",
          "spanNames": [
            "auth.*"
          ]
        },
        "exclude": {
          "matchType": "regexp",
          "spanNames": [
            "login.*"
          ]
        },
        "actions": [
          {
            "key": "password",
            "value": "obfuscated",
            "action": "update"
          },
          {
            "key": "token",
            "action": "delete"
          }
        ]
      }
    ]
  }
}
```

## <a name="span-processors"></a>Procesadores de intervalos

El procesador de intervalos modifica el nombre del intervalo o los atributos de un intervalo en función del nombre del intervalo. Opcionalmente, admite la capacidad de incluir o excluir intervalos.

### <a name="name-a-span"></a>Asignación de un nombre para un intervalo

Se requiere la configuración siguiente como parte de la sección de nombre:

* `fromAttributes`: el valor de atributo de las claves se usa para crear un nuevo nombre en el orden especificado en la configuración. Todas las claves de atributo deben especificarse en el intervalo para que el procesador cambie su nombre.

La configuración siguiente se puede configurar de forma opcional:

* `separator`: una cadena, que se especifica, se usará para dividir los valores
> [!NOTE]
> Si el cambio de nombre depende de los atributos modificados por el procesador de atributos, asegúrese de que el procesador de intervalos se especifica después del procesador de atributos en la especificación de canalización.

#### <a name="sample-usage"></a>Ejemplo de uso

En el siguiente ejemplo se especifican los valores de atributo "db.svc", "operation" e "id", que formarán el nuevo nombre del intervalo, en ese orden, separado por el valor "::".
```json
{
  "connectionString": "InstrumentationKey=00000000-0000-0000-0000-000000000000",
  "preview": {
    "processors": [
      {
        "type": "span",
        "name": {
          "fromAttributes": [
            "db.svc",
            "operation",
            "id"
          ],
          "separator": "::"
        }
      }
    ]
  }
}
```

### <a name="extract-attributes-from-span-name"></a>Extracción de atributos del nombre del intervalo

Toma una lista de expresiones regulares con las que va a coincidir el nombre del intervalo y para extraer atributos de la misma en función de las subexpresiones. Se debe especificar en la sección `toAttributes`.

Se requiere la siguiente configuración:

`rules`: una lista de reglas para extraer valores de atributo del nombre del intervalo. Los valores del nombre del intervalo se reemplazan por los nombres de atributo extraídos. Cada regla de la lista es una cadena de patrón de expresión regular. El nombre del intervalo se comprueba con la expresión regular. Si la expresión regular coincide, todas las subexpresiones con nombre de la expresión regular se extraen como atributos y se agregan al intervalo. Cada nombre de subexpresión se convierte en un nombre de atributo y la parte que coincide con la subexpresión se convierte en el valor de atributo. La parte coincidente del nombre del intervalo se reemplaza por el nombre de atributo extraído. Si los atributos ya existen en el intervalo, se sobrescribirán. El proceso se repite para todas las reglas en el orden en que se especifican. Cada regla subsiguiente funciona en el nombre del intervalo que es la salida después de procesar la regla anterior.

#### <a name="sample-usage"></a>Ejemplo de uso

Supongamos que el nombre del intervalo de entrada es /api/v1/document/12345678/update. La aplicación de los siguientes resultados en el nombre del intervalo de salida /api/v1/document/{documentId}/update agregará un nuevo atributo "documentId"="12345678" al intervalo.
```json
{
  "connectionString": "InstrumentationKey=00000000-0000-0000-0000-000000000000",
  "preview": {
    "processors": [
      {
        "type": "span",
        "name": {
          "toAttributes": {
            "rules": [
              "^/api/v1/document/(?<documentId>.*)/update$"
            ]
          }
        }
      }
    ]
  }
}
```

A continuación se muestra cómo cambiar el nombre del intervalo por "{operation_website}" y agregar el atributo {Key: operation_website, Value: oldSpanName } cuando el intervalo tiene las siguientes propiedades:
- El nombre del intervalo contiene '/' en cualquier parte de la cadena.
- El nombre del intervalo no es 'donot/change '.
```json
{
  "connectionString": "InstrumentationKey=00000000-0000-0000-0000-000000000000",
  "preview": {
    "processors": [
      {
        "type": "span",
        "include": {
          "matchType": "regexp",
          "spanNames": [
            "^(.*?)/(.*?)$"
          ]
        },
        "exclude": {
          "matchType": "strict",
          "spanNames": [
            "donot/change"
          ]
        },
        "name": {
          "toAttributes": {
            "rules": [
              "(?<operation_website>.*?)$"
            ]
          }
        }
      }
    ]
  }
}
```