---
title: Regla personalizada del firewall de aplicaciones web para Azure Front Door
description: Obtenga información sobre cómo utilizar las reglas personalizadas del firewall de aplicaciones web (WAF) que protegen sus aplicaciones web frente a ataques malintencionados.
author: vhorne
ms.service: web-application-firewall
ms.topic: article
services: web-application-firewall
ms.date: 09/05/2019
ms.author: victorh
ms.openlocfilehash: 158bfe30bf48ee420be8efb9ff32fff0e555d9e7
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/28/2020
ms.locfileid: "79475831"
---
#  <a name="custom-rules-for-web-application-firewall-with-azure-front-door"></a>Reglas personalizadas para el firewall de aplicaciones web con Azure Front Door

El firewall de aplicaciones web (WAF) de Azure con Front Door le permite controlar el acceso a sus aplicaciones web en función de las condiciones que defina. Una regla WAF personalizada consta de un número de prioridad, un tipo de regla, condiciones de coincidencia y una acción. Existen dos tipos de reglas personalizadas: reglas de coincidencia y reglas de límite de frecuencia. Una regla de coincidencia controla el acceso en función de un conjunto de condiciones coincidentes, mientras que una regla de limitación de frecuencia controla el acceso en función de las condiciones coincidentes y la frecuencia de las solicitudes entrantes. Puede deshabilitar una regla personalizada para impedir que se evalúe, pero mantener la configuración. 

## <a name="priority-match-conditions-and-action-types"></a>Prioridad, condiciones de coincidencia y tipos de acción

Puede controlar el acceso con una regla WAF personalizada que defina un número de prioridad, un tipo de regla, una matriz de condiciones de coincidencia y una acción. 

- **Prioridad:** es un entero único que describe el orden de evaluación de las reglas WAF. Las reglas con valores de prioridad más bajos se evalúan antes que las reglas con valores más altos. Los números de prioridad deben ser únicos entre todas las reglas personalizadas.

- **Acción:** define cómo enrutar una solicitud si coincide con una regla WAF. Puede elegir una de las siguientes acciones para que se aplique cuando una solicitud coincida con una regla personalizada.

    - *Permitir*:WAF reenvía la solicitud al back-end, registra una entrada en los registros de WAF y se cierra.
    - *Bloquear*: la solicitud está bloqueada y WAF envía una respuesta al cliente sin reenviar la solicitud al servidor back-end. WAF registra una entrada en los registros de WAF.
    - *Registro*: WAF registra una entrada en los registros de WAF y continúa evaluando la regla siguiente.
    - *Redirigir*: WAF redirige la solicitud a un URI especificado, registra una entrada en los registros de WAF y se cierra.

- **Condición de coincidencia**: define una variable de coincidencia, un operador y un valor de coincidencia. Cada regla puede contener varias condiciones de coincidencia. Una condición de coincidencia se puede basar en la ubicación geográfica, las direcciones IP del cliente (CIDR), el tamaño o la coincidencia de cadenas. La coincidencia de cadenas puede ser en una lista de variables de coincidencia.
  - **Variable de coincidencia:**
    - RequestMethod
    - QueryString
    - PostArgs
    - RequestUri
    - RequestHeader
    - RequestBody
    - Cookies
  - **Operador:**
    - Any: a menudo se usa para definir la acción predeterminada si ninguna regla coincide. Any es un operador de coincidencia de todo.
    - Igual
    - Contains
    - LessThan: restricción del tamaño.
    - GreaterThan: restricción del tamaño.
    - LessThanOrEqual: restricción del tamaño.
    - GreaterThanOrEqual: restricción del tamaño.
    - BeginsWith
    - EndsWith
    - Regex
  
  - **Regex** no admite las siguientes operaciones: 
    - Referencias inversas y captura de subexpresiones
    - Aserciones arbitrarias de ancho cero
    - Referencias de subrutinas y patrones recursivos
    - Patrones condicionales
    - Verbos de control de retroceso
    - Directiva de un solo byte \C
    - Directiva de coincidencia de nueva línea \R
    - Directiva de inicio de restablecimiento de coincidencia \K
    - Llamadas y código insertado
    - Agrupación atómica y cuantificadores de posesivos

  - **Negate [opcional]:** Puede establecer la condición *negate* para que sea true si el resultado de una condición debe negarse.
      
  - **Transform [opcional]:** Una lista de cadenas con los nombres de las transformaciones que se van a realizar antes de la coincidencia. Pueden ser las siguientes:
     - Uppercase 
     - Minúsculas
     - Trim
     - RemoveNulls
     - UrlDecode
     - UrlEncode
     
   - **Valor de coincidencia:** Los valores del método de solicitud HTTP admitidos incluyen:
     - GET
     - POST
     - PUT
     - HEAD
     - Delete
     - LOCK
     - UNLOCK
     - PROFILE
     - OPCIONES
     - PROPFIND
     - PROPPATCH
     - MKCOL
     - COPY
     - MOVE

## <a name="examples"></a>Ejemplos

### <a name="waf-custom-rules-example-based-on-http-parameters"></a>Ejemplo de reglas WAF personalizadas basadas en parámetros HTTP

A continuación se incluye un ejemplo que muestra la configuración de una regla personalizada con dos condiciones de coincidencia. Las solicitudes provienen de un sitio especificado, según se define en el origen de referencia y la cadena de consulta no contiene "password".

```
# http rules example
{
  "name": "AllowFromTrustedSites",
  "priority": 1,
  "ruleType": "MatchRule",
  "matchConditions": [
    {
      "matchVariable": "RequestHeader",
      "selector": "Referer",
      "operator": "Equal",
      "negateCondition": false,
      "matchValue": [
        "www.mytrustedsites.com/referpage.html"
      ]
    },
    {
      "matchVariable": "QueryString",
      "operator": "Contains",
      "matchValue": [
        "password"
      ],
      "negateCondition": true
    }
  ],
  "action": "Allow",
  "transforms": []
}

```
A continuación se muestra un ejemplo de configuración para bloquear el método "PUT":

``` 
# http Request Method custom rules
{
  "name": "BlockPUT",
  "priority": 2,
  "ruleType": "MatchRule",
  "matchConditions": [
    {
      "matchVariable": "RequestMethod",
      "selector": null,
      "operator": "Equal",
      "negateCondition": false,
      "matchValue": [
        "PUT"
      ]
    }
  ],
  "action": "Block",
  "transforms": []
}
```

### <a name="size-constraint"></a>Restricción del tamaño

Puede crear una regla personalizada que especifique la restricción del tamaño en la parte de una solicitud entrante. Por ejemplo, la regla siguiente bloquea una URL que tienen más de 100 caracteres de longitud.

```
# http parameters size constraint
{
  "name": "URLOver100",
  "priority": 5,
  "ruleType": "MatchRule",
  "matchConditions": [
    {
      "matchVariable": "RequestUri",
      "selector": null,
      "operator": "GreaterThanOrEqual",
      "negateCondition": false,
      "matchValue": [
        "100"
      ]
    }
  ],
  "action": "Block",
  "transforms": []
}
```

## <a name="next-steps"></a>Pasos siguientes
- [Configuración de una directiva del firewall de aplicaciones web con Azure PowerShell](waf-front-door-custom-rules-powershell.md) 
- Más información sobre [Firewall de aplicaciones web con Azure Front Door](afds-overview.md)
- Aprenda a [crear una instancia de Front Door](../../frontdoor/quickstart-create-front-door.md).

