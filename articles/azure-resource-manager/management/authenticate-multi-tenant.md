---
title: Autenticación entre inquilinos
description: Se describe cómo Azure Resource Manager gestiona las solicitudes de autenticación entre inquilinos.
ms.topic: conceptual
ms.date: 10/11/2019
ms.openlocfilehash: 7a13ba6f6cbfc10c52484c45e4011da7a0d8ee4c
ms.sourcegitcommit: 829d951d5c90442a38012daaf77e86046018e5b9
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/09/2020
ms.locfileid: "75474630"
---
# <a name="authenticate-requests-across-tenants"></a>Autenticación de solicitudes entre inquilinos

Al crear una aplicación multiinquilino, puede que deba administrar las solicitudes de autenticación para los recursos que se encuentran en distintos inquilinos. Un caso común es cuando una máquina virtual de un inquilino debe unirse a una red virtual de otro inquilino. Azure Resource Manager proporciona un valor de encabezado para almacenar los tokens auxiliares a fin de autenticar las solicitudes en diferentes inquilinos.

## <a name="header-values-for-authentication"></a>Valores de encabezado de autenticación

La solicitud tiene los siguientes valores de encabezado de autenticación:

| Nombre de encabezado | Descripción | Valor de ejemplo |
| ----------- | ----------- | ------------ |
| Authorization | Token primario | Bearer &lt;primary-token&gt; |
| x-ms-authorization-auxiliary | Tokens auxiliares | Bearer &lt;auxiliary-token1&gt;, EncryptedBearer &lt;auxiliary-token2&gt;, Bearer &lt;auxiliary-token3&gt; |

El encabezado auxiliar puede contener hasta tres tokens auxiliares. 

En el código de la aplicación multiinquilino, obtenga el token de autenticación de los otros inquilinos y almacénelos en los encabezados auxiliares. Todos los tokens deben proceder del mismo usuario o de la misma aplicación. Se debe haber invitado al usuario o a la aplicación como invitados a los otros inquilinos.

## <a name="processing-the-request"></a>Procesamiento de la solicitud

Cuando la aplicación envía una solicitud a Resource Manager, la solicitud se ejecuta con la identidad del token primario. El token primario debe ser válido y no debe haber expirado. Este token debe ser de un inquilino que pueda administrar la suscripción.

Cuando la solicitud hace referencia a un recurso de otro inquilino, Resource Manager comprueba los tokens auxiliares para determinar si se puede procesar la solicitud. Todos los tokens auxiliares del encabezado deben ser válidos y no deben haber expirado. Si alguno de los tokens ha expirado, Resource Manager devuelve un código de respuesta 401. La respuesta incluye el identificador de cliente y el identificador de inquilino del token que no es válido. Si el encabezado auxiliar contiene un token válido para el inquilino, se procesa la solicitud entre inquilinos.

## <a name="next-steps"></a>Pasos siguientes

* Para más información sobre las solicitudes de autenticación, consulte [Flujos de autenticación y escenarios de aplicaciones](../../active-directory/develop/authentication-flows-app-scenarios.md).
* Para más información sobre los tokens, consulte [Tokens de acceso de Azure Active Directory](../../active-directory/develop/access-tokens.md).
