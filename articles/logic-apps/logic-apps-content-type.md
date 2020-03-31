---
title: Control de los tipos de contenido
description: Aprenda a administrar varios tipos de contenido de los flujos de trabajo durante el tiempo de diseño y el tiempo de ejecución en Azure Logic Apps
services: logic-apps
ms.suite: integration
ms.reviewer: klam, logicappspm
ms.topic: conceptual
ms.date: 07/20/2018
ms.openlocfilehash: ae0abe288edda2ce01311d8533b1f104409efce0
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/27/2020
ms.locfileid: "75666880"
---
# <a name="handle-content-types-in-azure-logic-apps"></a>Control de tipos de contenido en Azure Logic Apps

Por una aplicación lógica pueden pasar diversos tipos de contenido como, por ejemplo, JSON, XML, archivos planos y datos binarios. Aunque Logic Apps admite todos los tipos de contenido, algunos tienen compatibilidad nativa y no requieren conversiones en las aplicaciones lógicas. Puede que otros tipos exijan conversiones según sea el caso. En este artículo se describe cómo Logic Apps controla los tipos de contenido y cómo estos tipos se pueden convertir correctamente cuando sea necesario.

Para determinar la forma adecuada de controlar los tipos de contenido, Logic Apps se basa en el valor del encabezado `Content-Type` en llamadas HTTP, por ejemplo:

* [application/json](#application-json) (native type)
* [text/plain](#text-plain) (native type)
* [application/xml y application/octet-stream](#application-xml-octet-stream)
* [Otros tipos de contenido](#other-content-types)

<a name="application-json"></a>

## <a name="applicationjson"></a>application/json

Logic Apps almacena y trata cualquier solicitud con el tipo de contenido *application/json* como un objeto JSON (JavaScript Object Notation). De forma predeterminada, puede analizar el contenido JSON sin realizar ninguna conversión. Para analizar una solicitud que tiene un encabezado con el tipo de contenido "application/json", puede usar una expresión. En este ejemplo se devuelve el valor `dog` de la matriz `animal-type` sin conversiones: 
 
`@body('myAction')['animal-type'][0]` 
  
  ```json
  {
    "client": {
       "name": "Fido",
       "animal-type": [ "dog", "cat", "rabbit", "snake" ]
    }
  }
  ```

Si trabaja con datos JSON que no especifican un encabezado, puede convertirlos manualmente a JSON mediante la [función json()](../logic-apps/workflow-definition-language-functions-reference.md#json), por ejemplo: 
  
`@json(triggerBody())['animal-type']`

### <a name="create-tokens-for-json-properties"></a>Creación de tokens para las propiedades JSON

Logic Apps ofrece la posibilidad de generar tokens fáciles de usar que representan las propiedades del contenido JSON para que se pueda hacer referencia a esas propiedades y utilizarlas más fácilmente en el flujo de trabajo de la aplicación lógica.

* **Desencadenador de solicitud**

  Si utiliza este desencadenador en el Diseñador de aplicación lógica, puede proporcionar un esquema JSON que describa la carga que espera recibir. 
  El diseñador analiza el contenido JSON mediante este esquema y genera tokens fáciles de usar que representan las propiedades del contenido JSON. 
  Luego puede hacer referencia fácilmente a estas propiedades y utilizarlas a lo largo del flujo de trabajo de la aplicación lógica. 
  
  Si no tiene un esquema, puede generarlo. 
  
  1. En el desencadenador de solicitud, seleccione **Use sample payload to generate schema** (Usar una carga de ejemplo para generar el esquema).  
  
  2. En **Enter or paste a sample JSON payload** (Especificar o pegar una carga de JSON de ejemplo), proporcione una carga de ejemplo y luego elija **Listo**. Por ejemplo: 

     ![Proporcionar una carga JSON de ejemplo](./media/logic-apps-content-type/request-trigger.png)

     El esquema generado aparece ahora en el desencadenador.

     ![Proporcionar una carga JSON de ejemplo](./media/logic-apps-content-type/generated-schema.png)

     Esta es la definición subyacente para el desencadenador de solicitud en el editor de la vista de código:

     ```json
     "triggers": { 
        "manual": {
           "type": "Request",
           "kind": "Http",
           "inputs": { 
              "schema": {
                 "type": "object",
                 "properties": {
                    "client": {
                       "type": "object",
                       "properties": {
                          "animal-type": {
                             "type": "array",
                             "items": {
                                "type": "string"
                             },
                          },
                          "name": {
                             "type": "string"
                          }
                       }
                    }
                 }
              }
           }
        }
     }
     ```

  3. En la solicitud, incluya un encabezado `Content-Type` y establezca el valor del encabezado en `application/json`.

* **Acción "Parse JSON"** (Analizar JSON)

  Cuando se usa esta acción en el Diseñador de aplicación lógica, se puede analizar la salida JSON y generar tokens fáciles de usar que representan las propiedades del contenido JSON. 
  Luego puede hacer referencia fácilmente a estas propiedades y utilizarlas a lo largo del flujo de trabajo de la aplicación lógica. Al igual que el desencadenador de solicitud, esta acción le permite especificar o generar un esquema JSON para el contenido que quiere analizar. 
  De este modo, puede consumir más fácilmente datos de Azure Service Bus, Azure Cosmos DB, etcétera.

  ![Parse JSON](./media/logic-apps-content-type/parse-json.png)

<a name="text-plain"></a>

## <a name="textplain"></a>text/plain

Cuando la aplicación lógica recibe los mensajes HTTP que tienen el encabezado `Content-Type` establecido en `text/plain`, la aplicación lógica almacena los mensajes en formato sin procesar. Si incluye estos mensajes en las acciones posteriores sin conversión alguna, las solicitudes salen con el encabezado `Content-Type` establecido en `text/plain`. 

Por ejemplo, si trabaja con un archivo plano, es posible que obtenga una solicitud HTTP con el encabezado `Content-Type` establecido en el tipo de contenido `text/plain`:

`Date,Name,Address`</br>
`Oct-1,Frank,123 Ave`

Si luego, en una acción posterior, envía esta solicitud como cuerpo de otra solicitud, por ejemplo, `@body('flatfile')`, esa segunda solicitud también tendrá un encabezado `Content-Type` establecido en `text/plain`. Si trabaja con datos que son texto sin formato pero no se ha especificado un encabezado, puede convertir manualmente esos datos a texto mediante la [función string()](../logic-apps/workflow-definition-language-functions-reference.md#string) como, por ejemplo, esta expresión: 

`@string(triggerBody())`

<a name="application-xml-octet-stream"></a>

## <a name="applicationxml-and-applicationoctet-stream"></a>application/xml and application/octet-stream

Logic Apps siempre conserva el encabezado `Content-Type` en una solicitud o respuesta HTTP que reciba. Por lo tanto, si la aplicación lógica recibe contenido con el encabezado `Content-Type` establecido en `application/octet-stream` y ese contenido se incluye en una acción posterior sin conversión alguna, la solicitud de salida tiene `Content-Type` establecido en `application/octet-stream`. De esta forma, Logic Apps puede garantizar que los datos no se pierdan mientras se mueven por el flujo de trabajo. Aunque el estado de la acción (entradas y salidas) se almacena en un objeto JSON cuando este pasa por el flujo de trabajo. 

## <a name="converter-functions"></a>Funciones de conversión

Para conservar algunos tipos de datos, Logic Apps convierte el contenido en una cadena con codificación base64 binaria con los metadatos adecuados que conservan la carga `$content` y `$content-type`, que se convierten automáticamente. 

En esta lista se describe cómo Logic Apps convierte el contenido al usar estas [funciones](../logic-apps/workflow-definition-language-functions-reference.md):

* `json()`: convierte los datos a `application/json`
* `xml()`: convierte los datos a `application/xml`
* `binary()`: convierte los datos a `application/octet-stream`
* `string()`: convierte los datos a `text/plain`
* `base64()`: convierte el contenido a una cadena codificada en base64
* `base64toString()`: convierte una cadena codificada en base64 a `text/plain`
* `base64toBinary()`: convierte una cadena codificada en base64 a `application/octet-stream`
* `dataUri()`: convierte una cadena a un identificador URI de datos
* `dataUriToBinary()`: convierte un identificador URI de datos a una cadena binaria
* `dataUriToString()`: convierte un identificador URI de datos a una cadena

Por ejemplo, si recibe una solicitud HTTP donde `Content-Type` se establece en `application/xml`, por ejemplo, este contenido:

```html
<?xml version="1.0" encoding="UTF-8" ?>
<CustomerName>Frank</CustomerName>
```

Puede convertir este contenido mediante la expresión `@xml(triggerBody())` con las funciones `xml()` y `triggerBody()`, y luego usar este contenido más adelante. También puede usar la expresión `@xpath(xml(triggerBody()), '/CustomerName')` con las funciones `xpath()` y `xml()`. 

## <a name="other-content-types"></a>Otros tipos de contenido

Logic Apps funciona con otros tipos de contenido y es compatible con ellos, pero puede que haya que obtener manualmente el cuerpo del mensaje decodificando la variable `$content`.

Por ejemplo, suponga que la aplicación lógica se desencadena mediante una solicitud con el tipo de contenido `application/x-www-url-formencoded`. Para conservar todos los datos, la variable `$content` del cuerpo de solicitud tiene una carga que se codifica como cadena base64:

`CustomerName=Frank&Address=123+Avenue`

Como la solicitud no es texto sin formato o JSON, se almacena en la acción como sigue:

```json
"body": {
   "$content-type": "application/x-www-url-formencoded",
   "$content": "AAB1241BACDFA=="
}
```

Logic Apps ofrece funciones nativas para el tratamiento de datos de formulario como, por ejemplo: 

* [triggerFormDataValue()](../logic-apps/workflow-definition-language-functions-reference.md#triggerFormDataValue)
* [triggerFormDataMultiValues()](../logic-apps/workflow-definition-language-functions-reference.md#triggerFormDataMultiValues)
* [formDataValue()](../logic-apps/workflow-definition-language-functions-reference.md#formDataValue) 
* [formDataMultiValues()](../logic-apps/workflow-definition-language-functions-reference.md#formDataMultiValues)

También puede tener acceso manual a los datos mediante una expresión, como en este ejemplo:

`@string(body('formdataAction'))` 

Si quiere que la solicitud de salida tenga el mismo encabezado de tipo de contenido, `application/x-www-url-formencoded`, puede agregar la solicitud al cuerpo de la acción sin realizar ninguna conversión como, por ejemplo, `@body('formdataAction')`. Pero este método solo funciona si el cuerpo es el único parámetro de la entrada `body`. Si intenta usar la expresión `@body('formdataAction')` en una solicitud `application/json`, obtendrá un error en tiempo de ejecución, ya que el cuerpo se envía codificado.
