---
title: Capacidad de composición de Azure Blockchain Tokens
description: La capacidad de composición de Azure Blockchain Tokens proporciona flexibilidad para crear tokens para escenarios avanzados.
ms.date: 11/04/2019
ms.topic: conceptual
ms.reviewer: brendal
ms.openlocfilehash: a3fe1b290917de20b7c3af31fe386ed93580d850
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/27/2020
ms.locfileid: "74325111"
---
# <a name="azure-blockchain-tokens-composability"></a>Capacidad de composición de Azure Blockchain Tokens

[!INCLUDE [Preview note](./includes/preview.md)]

La capacidad de composición de tokens proporciona flexibilidad para crear tokens para escenarios avanzados. Puede tener un escenario complejo que no se pueda implementar mediante las [cuatro plantillas de token pregeneradas](templates.md#base-token-types). La capacidad de composición de tokens le permite diseñar sus propias plantillas de token mediante la adición o eliminación de comportamientos definidos. Al crear una nueva plantilla de token, Azure Blockchain Tokens comprueba todas las reglas de gramática del token. Las plantillas compuestas se guardan en el servicio Azure Blockchain Tokens para su emisión en redes de cadenas de bloques conectadas.

Puede usar los [comportamientos de tokens](templates.md#token-behaviors) de las secciones siguientes para diseñar la plantilla de token.

## <a name="burnable-b"></a>Consumible (b)

Capacidad de quitar los tokens del suministro.

Por ejemplo, cuando se canjean puntos de tarjeta de crédito en línea para una tarjeta regalo, se consumen los puntos de la tarjeta de crédito.

## <a name="delegable-g"></a>Delegable (g)

Capacidad de delegar las acciones realizadas en el token de su propiedad.

El delegado puede realizar acciones si fuese el propietario del token. Por ejemplo, puede usar un token delegable para implementar un voto. Un token delegable permite que otra persona vote en nombre del propietario del token del voto.

## <a name="logable-l"></a>Registrable (l)

Capacidad de registrarse.

Por ejemplo, puede emitir un token registrable para la distribución de una película a cada sala de cine que proyecte una película específica. Para cada película que se vaya a reproducir, se debe registrar una transacción para cada proyección, ya que los pagos de regalías se aplican a cada proyección durante el lanzamiento de la película. La generación de actores puede usar los tokens de la película para validar los pagos por proyección y por sala de cine de la distribución.

## <a name="mint-able-m"></a>Acuñable (m)

Capacidad de acuñar tokens adicionales para la clase de token. El rol del acuñador incluye el comportamiento acuñable.

Por ejemplo, una empresa minorista que desea implementar un programa de fidelidad puede usar tokens acuñables para dicho programa. Pueden acuñar puntos de fidelidad adicionales para sus clientes a medida que crece su base de clientes.  

## <a name="non-subdividable-or-whole-d"></a>No subdivisible o entero (~ d)

Restricción para evitar que un token se divida en partes más pequeñas.

Por ejemplo, un cuadro no se puede subdividir en varias partes más pequeñas. 

## <a name="non-transferable-t"></a>Intransferible (~t)

Restricción para evitar un cambio de propiedad del propietario del token inicial.

Por ejemplo, un título universitario es un token intransferible. Una vez que un título se ha entregado a un graduado, no se puede transferir a otra persona.

## <a name="roles-r"></a>Roles (r)

Capacidad de definir roles en la clase de plantilla del token para comportamientos específicos.

Puede proporcionar una lista de nombres de rol que un token admita en el momento de la creación del token. Al especificar roles, el usuario puede asignar roles a estos comportamientos. Actualmente, solo se admite el rol de acuñador.

## <a name="singleton-s"></a>Singleton (s)

Restricción para permitir el suministro de un token.

Por ejemplo, un artefacto de museo es un token de singleton. Los artefactos de museo son únicos. Un token que representa un artefacto solo tiene un elemento en el suministro.

## <a name="subdividable-d"></a>Subdivisible (d)

Capacidad de dividir un token en partes más pequeñas.

Por ejemplo, un dólar se puede subdividir en centavos.

## <a name="transferable-t"></a>Transferible (t)

Capacidad de transferir la propiedad del token.

Por ejemplo, un título de propiedad es un token transferible, que se puede transferir de una persona a otra cuando se vende la propiedad.

## <a name="next-steps"></a>Pasos siguientes

Para obtener más información, consulte [Administración de cuentas de Azure Blockchain Tokens](account-management.md).
