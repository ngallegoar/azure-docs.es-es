---
title: 'Contraseñas prohibidas dinámicamente: Azure Active Directory'
description: Prohibición de contraseñas no seguras del entorno con las contraseñas prohibidas dinámicamente de Azure AD
services: active-directory
ms.service: active-directory
ms.subservice: authentication
ms.topic: conceptual
ms.date: 07/11/2018
ms.author: iainfou
author: iainfoulds
manager: daveba
ms.reviewer: rogoya
ms.collection: M365-identity-device-management
ms.openlocfilehash: ef127d120b32f5344bce0f68d79f48401087f0ce
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/28/2020
ms.locfileid: "79231016"
---
# <a name="eliminate-bad-passwords-in-your-organization"></a>Elimine las contraseñas incorrectas de su organización

Los líderes del sector recomiendan que no se use la misma contraseña en varios sitios, que sea compleja y que no sea sencilla del tipo "Contraseña123". ¿Cómo pueden las organizaciones asegurarse de que los usuarios siguen los procedimientos recomendados? ¿Cómo puede asegurarse de que los usuarios no usen contraseñas débiles o incluso variaciones en contraseñas no seguras?

El paso inicial para tener contraseñas más seguras es proporcionar instrucciones a los usuarios. La guía actual de Microsoft sobre este tema puede encontrarse en el siguiente vínculo:

[Guía de contraseñas de Microsoft](https://www.microsoft.com/research/publication/password-guidance)

Tener una buena orientación es importante, pero incluso con eso sabemos que muchos usuarios seguirán eligiendo contraseñas poco seguras. Para proteger su organización, Protección con contraseña de Azure AD detecta y bloquea las contraseñas débiles y sus variantes conocidas, así como los términos débiles adicionales que son específicos de su organización.

Para más información sobre los esfuerzos de seguridad actuales, consulte [Microsoft Security Intelligence Report](https://www.microsoft.com/security/operations/security-intelligence-report) (Informe de inteligencia de seguridad de Microsoft).

## <a name="global-banned-password-list"></a>Lista global de contraseñas prohibidas

El equipo de Azure AD Identity Protection analiza constantemente los datos de telemetría de seguridad de Azure AD en búsqueda de contraseñas poco seguras o en peligro o, más específicamente, los términos de base débil que suelen utilizarse como base para las contraseñas no seguras. Cuando se encuentran tales términos débiles, se agregan a la lista de contraseñas prohibidas global. El contenido de la lista global de contraseñas prohibidas no se basa en ningún origen de datos externo. La lista global de contraseñas prohibidas se basa completamente en los resultados continuos de la telemetría y el análisis de seguridad de Azure AD.

Siempre que se cambia o se restablece una nueva contraseña para cualquier usuario de cualquier inquilino de Azure AD, se usa la versión actual de la lista global de contraseñas prohibidas como entrada para validar la seguridad de la contraseña. Esta validación da como resultado contraseñas mucho más seguras para todos los clientes de Azure AD.

> [!NOTE]
> Los delincuentes cibernéticos también usan estrategias similares en sus ataques. Por lo tanto, Microsoft no hace público el contenido de esta lista.

## <a name="custom-banned-password-list"></a>Lista personalizada de contraseñas prohibidas

Puede que algunas organizaciones quieran ir un paso más allá y agregar sus propias personalizaciones a la lista global de contraseñas prohibidas con lo que Microsoft llama listas personalizadas de contraseñas prohibidas. Microsoft recomienda que los términos agregados a esta lista se centren principalmente en términos específicos de la organización, como:

- Nombres de marca
- Nombres de producto
- Ubicaciones (por ejemplo, la oficina central de la compañía)
- Términos internos específicos de la empresa
- Abreviaturas que tienen un significado específico en la empresa

Una vez agregados los términos a la lista personalizada de contraseñas prohibidas, se combinarán con los términos de la lista global de contraseñas prohibidas al validar las contraseñas.

> [!NOTE]
> La lista personalizada de contraseñas prohibidas se limita a tener un máximo de 1000 términos. No está diseñado para bloquear listas muy grandes de contraseñas. Para aprovechar al máximo las ventajas de la lista personalizada de contraseñas prohibidas, Microsoft recomienda que primero revise y comprenda el algoritmo de evaluación de contraseñas (consulte [Cómo se evalúan las contraseñas](concept-password-ban-bad.md#how-are-passwords-evaluated)) antes de agregar nuevos términos a la lista de prohibidos personalizada. Comprender cómo funciona el algoritmo permitirá a su empresa detectar y bloquear de forma eficaz un gran número de contraseñas no seguras y sus variantes.

Por ejemplo: considere un cliente denominado "Contoso", con sede en Londres y que crea un producto llamado "widget". Para este tipo de clientes, sería mucho menos seguro intentar bloquear variantes específicas de estos términos, como:

- "Contoso!1"
- "Contoso@London"
- "ContosoWidget"
- "!Contoso"
- "LondonHQ"
- ...etcetera

En su lugar, es mucho más eficaz y seguro bloquear solo los términos básicos de la clave:

- "Contoso"
- "London"
- "Widget"

El algoritmo de validación de contraseñas bloqueará automáticamente las variantes y las combinaciones débiles de los anteriores.

La lista personalizada de contraseñas prohibidas y la posibilidad de habilitar la integración local de Active Directory se administra mediante Azure Portal.

![Modifique la lista personalizada de contraseñas prohibidas en Métodos de autenticación](./media/concept-password-ban-bad/authentication-methods-password-protection.png)

## <a name="password-spray-attacks-and-third-party-compromised-password-lists"></a>Ataques de pulverización de contraseñas y listas de contraseñas en peligro de terceros

Una ventaja clave de la protección con contraseña de Azure AD es que le ayudar a defenderse contra los ataques de pulverización de contraseñas. La mayoría de los ataques de pulverización de contraseñas no intentan atacar ninguna cuenta individual determinada más de unas pocas veces, porque este comportamiento aumenta las probabilidades de que los detecten, ya sea mediante el bloqueo de cuentas u otros medios. Por lo tanto, la mayoría de los ataques de pulverización de contraseñas se basan en el envío de un número pequeño de las contraseñas más débiles conocidas en cada una de las cuentas de una empresa. Esta técnica permite al atacante buscar rápidamente una cuenta en peligro y, al mismo tiempo, evitar posibles umbrales de detección.

La protección con contraseña de Azure AD está diseñada para bloquear de forma eficaz todas las contraseñas débiles conocidas que se puedan usar en ataques de pulverización de contraseñas, y se basa en los datos de telemetría de seguridad del mundo real tal y como los ve Azure AD.  Microsoft conoce los sitios web de terceros que indican que millones de contraseñas se han puesto en peligro en infracciones de seguridad conocidas públicamente. Es habitual que los productos de validación de contraseñas de terceros se basen en la comparación de fuerza bruta con esos millones de contraseñas. Microsoft considera que estas técnicas no son la mejor manera de mejorar la seguridad general de las contraseñas dadas las estrategias típicas que utilizan los atacantes de pulverización de contraseñas.

> [!NOTE]
> La lista global de contraseñas prohibidas de Microsoft no se basa en ningún origen de datos de terceros, incluidas las listas de contraseñas en peligro.

Aunque la lista global de no permitidos de Microsoft es pequeña en comparación con algunas listas masivas de terceros, sus efectos de seguridad se amplifican por el hecho de que proviene de los datos de telemetría de seguridad reales obtenidos de ataques de pulverización de contraseñas reales, además del hecho de que el algoritmo de validación de contraseñas de Microsoft utiliza técnicas inteligentes de coincidencia aproximada. El resultado final es que detectará y bloqueará eficazmente millones de las contraseñas débiles más comunes que se usan en su empresa. Los clientes que decidan agregar términos específicos de la organización a la lista de contraseñas prohibidas personalizada también se beneficiarán del mismo algoritmo.

Puede consultar información adicional sobre los problemas de la seguridad basada en contraseñas en [Your Pa$$word doesn't matter](https://techcommunity.microsoft.com/t5/Azure-Active-Directory-Identity/Your-Pa-word-doesn-t-matter/ba-p/731984) (Su contraseña no importa).

## <a name="on-premises-hybrid-scenarios"></a>Escenarios híbridos locales

La protección de cuentas que están solo en la nube es muy útil, pero muchas organizaciones conservan escenarios híbridos que incluyen implementaciones locales de Windows Server Active Directory. Las ventajas de la seguridad de la protección con contraseña de Azure AD también se pueden extender al entorno de Active Directory en Windows Server mediante la instalación de agentes locales. Ahora es obligatorio que los usuarios y administradores que cambien o restablezcan contraseñas en Active Directory cumplan con la misma directiva de contraseñas que los usuarios que solo están en la nube.

## <a name="how-are-passwords-evaluated"></a>Cómo se evalúan las contraseñas

Cada vez que un usuario cambia o restablece su contraseña, se comprueba la solidez y la complejidad de la nueva contraseña validándola con la combinación de la lista global y la lista personalizada de contraseñas prohibidas (si esta última está configurada).

Incluso si una contraseña de usuario contiene una contraseña prohibida, la contraseña podría aceptarse si la contraseña global es lo suficientemente segura. Una contraseña recién configurada recorrerá los pasos siguientes para evaluar su solidez general a fin de determinar si debe aceptarse o rechazarse.

### <a name="step-1-normalization"></a>Paso 1: Normalización

En primer lugar, una nueva contraseña pasa por un proceso de normalización. Esta técnica permite que un pequeño conjunto de contraseñas prohibidas se asigne a un conjunto mucho más grande de contraseñas potencialmente inseguras.

La normalización consta de dos partes.  En primer lugar, todas las letras mayúsculas se cambian a minúsculas.  En segundo lugar, se realizan las sustituciones de caracteres comunes, por ejemplo:  

| Letra original  | Letra sustituida |
| --- | --- |
| "0"  | "o" |
| '1'  | "l" |
| "$"  | "s" |
| "\@"  | "a" |

Ejemplo: suponga que está prohibida la contraseña "blank" y que un usuario intenta cambiar su contraseña a "Bl@nK". Aunque "Bl@nk" no está expresamente prohibida, el proceso de normalización convierte esta contraseña a "blank", que es una contraseña no permitida.

### <a name="step-2-check-if-password-is-considered-banned"></a>Paso 2: Comprobar si la contraseña se considera prohibida

#### <a name="fuzzy-matching-behavior"></a>Comportamiento de la coincidencia aproximada

La coincidencia aproximada se usa en la contraseña normalizada para identificar si contiene una contraseña que se encuentra en la lista global o personalizada de contraseñas prohibidas. El proceso de búsqueda de coincidencias se basa en una distancia de edición de una (1) comparación.  

Ejemplo: Suponga que está prohibida la contraseña "abcdef" y que un usuario intenta cambiar su contraseña a una de las siguientes:

"abcdeg"    *(el último carácter se ha cambiado de "f" a "g")* "abcdefg"     *("g" anexada al final)* "abcde"     *(se ha eliminado la "f" final)*

Ninguna de las contraseñas anteriores coincide específicamente con la contraseña prohibida "abcdef". Sin embargo, dado que cada ejemplo se encuentra dentro de una distancia de 1 edición del término prohibido "abcdef", se considera que todas coinciden con "abcdef".

#### <a name="substring-matching-on-specific-terms"></a>Coincidencia de subcadenas (en términos específicos)

La coincidencia de subcadenas se utiliza en la contraseña normalizada para comprobar el nombre y apellido del usuario, así como el nombre del inquilino (tenga en cuenta que no se realiza la coincidencia de nombres de inquilino cuando se validan contraseñas en un controlador de dominio de Active Directory).

Ejemplo: suponga que tenemos un usuario llamado Pol que desea restablecer su contraseña a "P0l123fb". Después de la normalización, esta contraseña se convertiría en "pol123fb". La coincidencia de subcadenas detecta que la contraseña contiene el nombre del usuario "Pol". Aunque "P0l123fb" no se encontraba específicamente en ninguna lista de contraseñas prohibidas, la coincidencia de subcadenas encontró "Pol" en la contraseña. Por lo tanto, esta contraseña se rechazaría.

#### <a name="score-calculation"></a>Cálculo de puntuación

El siguiente paso consiste en identificar todas las instancias de contraseñas prohibidas en la nueva contraseña normalizada del usuario. A continuación:

1. A cada contraseña prohibida que se encuentre en la contraseña de un usuario se le concede un punto.
2. Cada carácter único restante recibe un punto.
3. Una contraseña debe tener al menos cinco (5) puntos para que se acepte.

En los dos ejemplos siguientes, supongamos que Contoso usa la protección de contraseñas de Azure AD y tiene "contoso" en la lista personalizada. Supongamos también que "blank" está en la lista global.

Ejemplo: un usuario cambia su contraseña a "C0ntos0Blank12".

Después de la normalización, esta contraseña se convierte en "contosoblank12". El proceso de coincidencia encuentra que esta contraseña contiene dos contraseñas prohibidas: contoso y blank. La puntuación entonces sería la siguiente:

[contoso] + [blank] = [1] + [2] = 4 puntos. Como la contraseña está por debajo de cinco (5) puntos, se rechazará.

Ejemplo: Un usuario cambia su contraseña a "ContoS0Bl@nkf9!".

Después de la normalización, esta contraseña se convierte en "contosoblankf9!". El proceso de coincidencia encuentra que esta contraseña contiene dos contraseñas prohibidas: contoso y blank. La puntuación entonces sería la siguiente:

[contoso] + [blank] + [f] + [9] + [!] = 5 puntos. Como esta contraseña tiene al menos cinco (5) puntos, se acepta.

   > [!IMPORTANT]
   > Tenga en cuenta que el algoritmo de contraseñas prohibidas, junto con la lista global, puede cambiar (y de hecho cambia) en cualquier momento en Azure en función de la investigación y el análisis de seguridad en curso. En el caso del servicio de agente del controlador de dominio local, los algoritmos actualizados solo surtirán efecto después de volver a instalar el software del agente del controlador de dominio.

## <a name="license-requirements"></a>Requisitos de licencia

|   | Protección con contraseña de Azure AD con la lista global de contraseñas prohibidas | Protección con contraseña de Azure AD con la lista personalizada de contraseñas prohibidas|
| --- | --- | --- |
| Usuarios solo en la nube | Azure AD Free | Azure AD Premium (P1 o P2) |
| Usuarios sincronizados desde un entorno local de Windows Server Active Directory | Azure AD Premium (P1 o P2) | Azure AD Premium (P1 o P2) |

> [!NOTE]
> Los usuarios de Windows Server Active Directory local que no se sincronizaron con Azure Active Directory también se aprovechan de la protección con contraseña de Azure AD según la licencia existente para los usuarios sincronizados.

Puede encontrar información adicional sobre licencias, incluidos los costos, en la [página de precios de Azure Active Directory](https://azure.microsoft.com/pricing/details/active-directory/).

## <a name="what-do-users-see"></a>Lo que ven los usuarios

Si un usuario intenta restablecer una contraseña a algo que estaría prohibido, verá el siguiente mensaje de error:

Lamentablemente, la contraseña contiene una palabra, frase o patrón que permite adivinar la contraseña fácilmente. Vuelva a intentarlo con una contraseña diferente.

## <a name="next-steps"></a>Pasos siguientes

- [Configuración de la lista personalizada de contraseñas prohibidas](howto-password-ban-bad.md)
- [Habilitar agentes de Protección con contraseña de Azure AD locales](howto-password-ban-bad-on-premises-deploy.md)
