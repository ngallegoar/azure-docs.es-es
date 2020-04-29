---
title: 'Expresiones condicionales para Azure CDN: motor de reglas de Verizon Premium'
description: Documentación de referencia sobre las condiciones y características de coincidencia del motor de reglas de Azure CDN de Verizon Premium.
services: cdn
author: asudbring
ms.service: azure-cdn
ms.topic: article
ms.date: 05/31/2019
ms.author: allensu
ms.openlocfilehash: 25b3c1a011e39adea651a6daa1d4ab9aa3a9d186
ms.sourcegitcommit: 849bb1729b89d075eed579aa36395bf4d29f3bd9
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 04/28/2020
ms.locfileid: "81253516"
---
# <a name="azure-cdn-from-verizon-premium-rules-engine-conditional-expressions"></a>Expresiones condicionales del motor de reglas de Azure CDN de Verizon Premium | Microsoft Docs

En este artículo se muestran descripciones detalladas de las expresiones condicionales para el [motor de reglas](cdn-verizon-premium-rules-engine.md) de Azure Content Delivery Network (CDN).

La primera parte de una regla es la expresión condicional.

Expresión condicional | Descripción
-----------------------|-------------
IF | Una expresión IF siempre es una parte de la primera instrucción en una regla. Al igual que las demás expresiones condicionales, esta instrucción IF debe asociarse a una coincidencia. Si no se ha definido ninguna expresión condicional adicional, esta coincidencia determina los criterios que deben cumplirse para poder aplicar un conjunto de funciones a una solicitud.
AND IF | Una expresión AND IF solo puede agregarse después de los siguientes tipos de expresiones condicionales: IF, AND IF. Indica que hay otra condición que debe cumplirse para la instrucción IF inicial.
ELSE IF| Una expresión ELSE IF especifica una condición alternativa que debe cumplirse antes de que se realice un conjunto de funciones específicas de esta instrucción ELSE IF. La presencia de una instrucción IF ELSE indica el final de la instrucción anterior. La única expresión condicional que se puede colocar después de una instrucción ELSE IF es otra instrucción ELSE IF. Esto significa que una instrucción ELSE IF solo puede usarse para especificar una única condición adicional que debe cumplirse.

**Ejemplo**: ![Condición de coincidencia de red CDN](./media/cdn-rules-engine-reference/cdn-rules-engine-conditional-expression.png)

 > [!TIP]
   > Una regla posterior puede invalidar las acciones especificadas por una regla anterior.
   > Ejemplo: Una regla de detección de todo protege todas las solicitudes a través de la autenticación basada en token. Puede crearse otra regla directamente debajo de ella para realizar una excepción para ciertos tipos de solicitudes.

## <a name="next-steps"></a>Pasos siguientes

- [Información general de Azure CDN](cdn-overview.md)
- [Referencia del motor de reglas](cdn-verizon-premium-rules-engine-reference.md)
- [Condiciones de coincidencia del motor de reglas](cdn-verizon-premium-rules-engine-reference-match-conditions.md)
- [Características del motor de reglas](cdn-verizon-premium-rules-engine-reference-features.md)
- [Invalidación del comportamiento HTTP predeterminado mediante el motor de reglas](cdn-verizon-premium-rules-engine.md)