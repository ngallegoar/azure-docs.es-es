---
title: Plantillas de Azure Blockchain Tokens
description: Las plantillas de Azure Blockchain Tokens son plantillas estandarizadas y reutilizables que simplifican la creación y la implementación de tokens basados en libros de contabilidad.
ms.date: 11/04/2019
ms.topic: conceptual
ms.reviewer: brendal
ms.openlocfilehash: 9600a6a251552acd319cc68d2bd281584d65546d
ms.sourcegitcommit: 849bb1729b89d075eed579aa36395bf4d29f3bd9
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 04/28/2020
ms.locfileid: "79227388"
---
# <a name="azure-blockchain-tokens-templates"></a>Plantillas de Azure Blockchain Tokens

[!INCLUDE [Preview note](./includes/preview.md)]

Una plantilla de Azure Blockchain Tokens es una plantilla estandarizada y reutilizable que simplifica la creación y la implementación de tokens basados en libros de contabilidad. Una plantilla se compone de una fórmula, que se basa en la gramática del [Marco de taxonomía de tokens (TTF)](overview.md#token-taxonomy-framework). La gramática abarca el tipo de token base y el conjunto de comportamientos para el token.  

Por ejemplo, la plantilla de token **τϜ{d,m,b,r}** describe un token de base fungible que se puede subdividir, acuñar, gastar y tiene compatibilidad con roles.
  
## <a name="base-token-types"></a>Tipos de token de base

Al definir y crear el token basado en el libro de contabilidad para su activo en particular, es importante tener en cuenta el token base que se va a usar.

### <a name="fungible"></a>Fungible

Los tokens fungibles (𝜏F) tienen un valor intercambiable entre sí, siempre que se encuentren en la misma clase o serie. Un token tiene el mismo valor que otro token, o una cantidad determinada de tokens tiene el mismo valor que otra cantidad igual. Por ejemplo, un dólar es un token fungible. Si hay dos personas que tienen un billete de dólar, pueden intercambiar estos billetes de dólar sin ninguna consecuencia. Los billetes de dólar tienen el mismo valor. 

### <a name="non-fungible"></a>No fungible

Los tokens no fungibles (𝜏N) no son intercambiables con otros tokens del mismo tipo, ya que normalmente tienen valores diferentes. Por ejemplo, un título de propiedad es un token no fungible. Los títulos de propiedad de dos pisos diferentes en un complejo de apartamentos no tienen necesariamente el mismo valor, debido a la ubicación de la unidad o a la planta en la que se encuentra la unidad. El valor percibido de los dos tokens de título de propiedad no es igual.

### <a name="hybrid"></a>Híbrido

Los tokens híbridos son tokens que tienen componentes de tokens fungible y tokens no fungibles. Un token híbrido es un tipo de token de base que posee una clase del otro tipo de token.

#### <a name="hybrid-non-fungible-base-with-fungible-segments"></a>Base híbrida no fungible con segmentos fungibles

Un token de base híbrida no fungible con segmentos fungibles tiene una base no fungible con segmentos de token fungible.
Por ejemplo, una entrada a un concierto es un token híbrido en el que la fecha y la hora del concierto es el token de base no fungible. Las entradas en varias secciones de plazas para el concierto dado son los segmentos con tokens fungibles. Los vales son intercambiables en las secciones de plazas individuales, pero no entre las secciones.

#### <a name="hybrid-fungible-base-with-non-fungible-segments"></a>Base híbrida fungible con segmentos no fungibles

Un token de base híbrida fungible con segmentos no fungibles tiene una base fungible con segmentos de token no fungible. Por ejemplo, una garantía hipotecaria es un token híbrido en el que varios propietarios son la base fungible que se divide entre varios propietarios. La garantía es intercambiable. Las hipotecas individuales son los segmentos no fungibles que representan la garantía hipotecaria específica.

## <a name="token-behaviors"></a>Comportamientos de tokens

Un comportamiento de token define las capacidades o restricciones del token. El comportamiento incluye propiedades auxiliares que forman parte de la definición del token. Los comportamientos se pueden aplicar a todos los tipos de token o solo a uno. Los comportamientos pueden ser internos o externos en función de los efectos del comportamiento. Un comportamiento interno habilita o restringe las propiedades en el propio token. Un comportamiento externo habilita o restringe la invocación del comportamiento desde un actor externo.

Para obtener más información sobre los comportamientos de tokens del Marco de taxonomía de token (TTF) admitidos por Azure Blockchain Tokens, consulte [composición de tokens](composability.md).

## <a name="pre-built-token-templates"></a>Plantillas de token predefinidas

Azure Blockchain Tokens proporciona cuatro plantillas de token predefinidas que se pueden usar sin modificaciones. Puede llamar a estas plantillas predefinidas para la mayoría de los casos de uso para empezar a crear, implementar y administrar los tokens rápidamente.

### <a name="commodity-tokens"></a>Tokens de mercancías

Los tokens de mercancías tienen un valor constante y son transferibles. Por ejemplo, un barril de petróleo o una unidad de energía.

**𝜏F{~d,t,m,b,r}** - fungible, completo, se puede transferir, acuñar, gastar y tiene compatibilidad con roles

Muchos escenarios de cadena de bloques requieren transparencia y visibilidad en la cadena de suministro o en varias organizaciones. Los tokens de mercancías se basan en estos casos de uso comunes. Los tokens son intercambiables y constantes. La plantilla de token de mercancías es flexible y personalizable con metadatos.

### <a name="qualified-tokens"></a>Tokens cualificados

Los tokens cualificados representan algo ganado y normalmente están asociados a una entidad y no se pueden transferir. Por ejemplo, un diploma o una infracción por mal estacionamiento.

**𝜏N{s,~t}** - no fungible, singleton y no transferible

Varios escenarios de auditoría y atestación requieren que la titularidad del token no se pueda cambiar. Existe un conjunto de casos de uso, que tienen la necesidad de proporcionar un token cualificado que indique si la asociación es correcta o no.

### <a name="asset-tokens"></a>Tokens de activos

Los tokens de activos tienen un valor único que depende del artículo y no se puede comoditizar. Por ejemplo, un artefacto de museo o un título de propiedad.

**𝜏N{s,~t}** - no fungible, singleton y transferible

Los tokens de activos pueden confundirse con los tokens de mercancías. La principal diferencia entre los dos tokens es que los tokens de activos son intrínsecamente únicos, y el valor depende del tipo de token que sea. Por ejemplo, una pieza de arte, como una pintura al óleo hecha por un artista establecido, es un token de activos. Sin embargo, una impresión de la Mona Lisa se considera un token de mercancías. Del mismo modo, un título de propiedad es un token de activos, ya que el valor existe en las cualidades subjetivas de la propiedad.

### <a name="ticket-tokens"></a>Tokens de vales

Los tokens de vales tienen un valor constante, pero normalmente expiran. Por ejemplo, un billete de avión.

**𝜏N{m,b,r}** - no fungible, se puede acuñar, gastar y tiene compatibilidad con roles.

Normalmente, los tokens de vales tienen una fecha de expiración que los hace distintos de un token de mercancías normal. Por ejemplo, un billete de avión, la entrada para un concierto o para un evento deportivo tienen opciones de asiento asignadas con fechas de uso específicas. No es fácil intercambiar entradas con distintas fechas o áreas de asiento.

## <a name="next-steps"></a>Pasos siguientes

Si necesita más flexibilidad para su escenario, aprenda a crear sus propias plantillas de token con la [composición de tokens](composability.md).
