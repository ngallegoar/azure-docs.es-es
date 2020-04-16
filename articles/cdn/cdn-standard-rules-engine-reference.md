---
title: Referencia del motor de reglas estándar de Azure CDN | Microsoft Docs
description: Documentación de referencia para las condiciones de coincidencia y las acciones del motor de reglas estándar de Azure Content Delivery Network (Azure CDN).
services: cdn
author: asudbring
ms.service: azure-cdn
ms.topic: article
ms.date: 11/01/2019
ms.author: allensu
ms.openlocfilehash: 6d4fa4451c3db3d6f2a506eabd5676d18b0219f4
ms.sourcegitcommit: 8dc84e8b04390f39a3c11e9b0eaf3264861fcafc
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 04/13/2020
ms.locfileid: "81259908"
---
# <a name="standard-rules-engine-reference-for-azure-cdn"></a>Referencia del motor de reglas estándar de Azure CDN

En el [motor de reglas estándar](cdn-standard-rules-engine.md) de Azure Content Delivery Network (Azure CDN), una regla consta de una o varias condiciones de coincidencia y una acción. En este artículo se muestran descripciones detalladas de las condiciones de coincidencia y las características disponibles en el motor de reglas estándar para Azure CDN.

El motor de reglas está diseñado para ser la autoridad definitiva sobre cómo Azure CDN estándar procesa los tipos específicos de solicitudes.

**Usos comunes de las reglas**:

- Invalidar o definir una directiva de memoria caché personalizada.
- Redirigir solicitudes.
- Modificar los encabezados de solicitudes y respuestas HTTP.

## <a name="terminology"></a>Terminología

Para definir una regla en el motor de reglas, establezca las [condiciones de coincidencia](cdn-standard-rules-engine-match-conditions.md) y las [acciones](cdn-standard-rules-engine-actions.md):

 ![Estructura de reglas de Azure CDN](./media/cdn-standard-rules-engine-reference/cdn-rules-structure.png)

Cada regla puede tener hasta cuatro condiciones de coincidencia y tres acciones. Cada punto de conexión de Azure CDN puede tener hasta cinco reglas. 

Se incluye en el límite actual de cinco reglas para un punto de conexión de Azure CDN una *regla global* predeterminada. La regla global no tiene condiciones de coincidencia, y las acciones que se definen en una regla global siempre se desencadenan.

## <a name="syntax"></a>Sintaxis

La forma en que los caracteres especiales se tratan en una regla varía en función de cómo las diferentes condiciones de coincidencia y acciones controlan los valores de texto. Una condición de coincidencia o acción puede interpretar el texto de una de las siguientes maneras:

- [Valores literales](#literal-values)
- [Valores de carácter comodín](#wildcard-values)


### <a name="literal-values"></a>Valores literales

El texto que se interpreta como un valor literal trata todos los caracteres especiales, *excepto el símbolo %* , como parte del valor que debe coincidir en una regla. Por ejemplo, una condición de coincidencia literal establecida en `'*'` se cumple solo cuando se encuentra el valor exacto `'*'`.

Se usa un símbolo de porcentaje para indicar la codificación de direcciones URL (p. ej., `%20`).

### <a name="wildcard-values"></a>Valores de carácter comodín

El texto que se interpreta como un valor de carácter comodín asigna un significado adicional a los caracteres especiales. En la tabla siguiente se describe cómo se interpretan los caracteres especiales específicos en el motor de reglas estándar:

Carácter | Descripción
----------|------------
\ | Se usa una barra diagonal inversa para el escape de cualquier carácter especificado en esta tabla. Debe especificarse una barra diagonal inversa directamente antes del carácter especial al que debe aplicarse el escape. Por ejemplo, la sintaxis siguiente aplica el escape a un asterisco: `\*`
% | Se usa un símbolo de porcentaje para indicar la codificación de direcciones URL (p. ej., `%20`).
\* | Un asterisco es un carácter comodín que representa uno o más caracteres.
espacio | Un carácter de espacio indica que una condición de coincidencia debe cumplirse mediante uno de los patrones o valores especificados.
comillas simples | Una comilla simple no tiene ningún significado especial. Sin embargo, un conjunto de comillas simples indica que un valor debe tratarse como un valor literal. Las comillas simples pueden usarse de las siguientes maneras:<ul><li>Para permitir que se cumpla una condición de coincidencia siempre que el valor especificado coincida con cualquier parte del valor de comparación.  Por ejemplo, `'ma'` coincidiría con cualquiera de las siguientes cadenas: <ul><li>/business/**ma**rathon/asset.htm</li><li>**ma**p.gif</li><li>/business/template.**ma**p</li></ul><li>Para permitir especificar un carácter especial como carácter literal. Por ejemplo, puede especificar un carácter de espacio literal enmarcando un carácter de espacio en un conjunto de comillas simples (`' '` o `'<sample value>'`).</li><li>Para permitir que se especifique un valor en blanco. Especifique un valor en blanco mediante la especificación de un conjunto de comillas simples ( **''** ).</li></ul>**Importante**:<br /><ul><li>Si el valor especificado no contiene un carácter comodín, el valor se considerará automáticamente un valor literal. No es necesario especificar un conjunto de comillas simples para un valor literal.</li><li>Si no se usa una barra diagonal inversa para aplicar el escape a otro carácter de esta tabla, se omite cuando se especifique dentro de un conjunto de comillas simples.</li><li>Otra manera de especificar un carácter especial como carácter literal es aplicar el escape con una barra diagonal inversa (`\`).</li></ul>

## <a name="next-steps"></a>Pasos siguientes

- [Condiciones de coincidencia en el motor de reglas estándar](cdn-standard-rules-engine-match-conditions.md)
- [Acciones en el motor de reglas estándar](cdn-standard-rules-engine-actions.md)
- [Aplicación de HTTPS mediante el motor de reglas estándar](cdn-standard-rules-engine.md)
- [Información general de Azure CDN](cdn-overview.md)
