---
title: Vigencia de tokens configurable
titleSuffix: Microsoft identity platform
description: Obtenga información sobre cómo establecer la vigencia de los tokens emitidos por la Plataforma de identidad de Microsoft.
services: active-directory
author: rwike77
manager: CelesteDG
ms.service: active-directory
ms.subservice: develop
ms.workload: identity
ms.topic: conceptual
ms.date: 10/23/2020
ms.author: ryanwi
ms.custom: aaddev, identityplatformtop40, content-perf, FY21Q1, contperfq1
ms.reviewer: hirsin, jlu, annaba
ms.openlocfilehash: 4accae27dc092a4900e6092c62c7f4978a46668a
ms.sourcegitcommit: 59f506857abb1ed3328fda34d37800b55159c91d
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/24/2020
ms.locfileid: "92503783"
---
# <a name="configurable-token-lifetimes-in-microsoft-identity-platform-preview"></a>Vigencia de tokens configurable en la Plataforma de identidad de Microsoft (versión preliminar)

Puede especificar la vigencia de un token emitido por la Plataforma de identidad de Microsoft. La vigencia de los tokens se puede configurar para todas las aplicaciones de una organización, para una aplicación multiinquilino (multiorganización) o para una entidad de servicio específica de una organización. No obstante, actualmente no se admite la configuración de la vigencia de los tokens para las [entidades de servicio de identidad administrada](../managed-identities-azure-resources/overview.md).

> [!IMPORTANT]
> Después del 30 de enero de 2021, los inquilinos ya no podrán configurar la duración de los tokens de actualización y sesión, y Azure Active Directory dejará de respetar la configuración existente de los tokens de sesión y actualización en las directivas después de esa fecha. Después de la retirada, todavía podrá configurar la duración de los tokens de acceso.
> Se han implementado  [funciones de administración de sesiones de autenticación](../conditional-access/howto-conditional-access-session-lifetime.md) en el acceso condicional de Azure AD. Puede usar esta nueva característica para configurar la vigencia de los tokens de actualización mediante la configuración de la frecuencia de inicio de sesión. El acceso condicional es una característica de Azure AD Premium P1 y puede evaluar si Premium es adecuado para la organización en la [página de precios de Premium](https://azure.microsoft.com/en-us/pricing/details/active-directory/). 
> 
> En el caso de los inquilinos que no usen la administración de sesiones de autenticación en el acceso condicional después de la fecha de retirada, pueden esperar que Azure AD respetará la configuración predeterminada que se describe en la sección siguiente.

## <a name="configurable-token-lifetime-properties-after-the-retirement"></a>Propiedades de vigencia de tokens configurables después de la retirada
La configuración de los tokens de actualización y sesión se ve afectada por las siguientes propiedades y sus correspondientes valores establecidos. Después de la retirada de la configuración de tokens de actualización y sesión, Azure AD solo respetará el valor predeterminado que se describe a continuación, con independencia de que las directivas tengan valores personalizados configurados.  

|Propiedad   |Cadena de propiedad de directiva    |Afecta a |Valor predeterminado |
|----------|-----------|------------|------------|
|Tiempo máximo de inactividad del token de actualización |MaxInactiveTime  |Tokens de actualización |90 días  |
|Antigüedad máxima del token de actualización (un solo factor)  |MaxAgeSingleFactor  |Tokens de actualización (para los usuarios)  |Hasta que se revoca  |
|Antigüedad máxima del token de actualización (varios factores)  |MaxAgeMultiFactor  |Tokens de actualización (para los usuarios) |180 días  |
|Antigüedad máxima del token de sesión (un solo factor)  |MaxAgeSessionSingleFactor |Tokens de sesión (persistentes y no persistentes)  |Hasta que se revoca |
|Antigüedad máxima del token de sesión (varios factores)  |MaxAgeSessionMultiFactor  |Tokens de sesión (persistentes y no persistentes)  |180 días |

Puede usar el cmdlet [Get-AzureADPolicy](/powershell/module/azuread/get-azureadpolicy?view=azureadps-2.0-preview&preserve-view=true) para identificar directivas de vigencia de token cuyos valores de propiedad difieren de los valores predeterminados de Azure AD.

Para comprender mejor cómo se usan las directivas en el inquilino, puede usar el cmdlet [Get-AzureADPolicyAppliedObject](/powershell/module/azuread/get-azureadpolicyappliedobject?view=azureadps-2.0-preview&preserve-view=true) para identificar qué aplicaciones y entidades de servicio están vinculadas a las directivas. 

Si el inquilino tiene directivas que definen valores personalizados para las propiedades de configuración de tokens de sesión y actualización, Microsoft recomienda actualizarlas a los valores que reflejen los valores predeterminados descritos anteriormente. Si no se realiza ningún cambio, Azure AD respetará de forma automática los valores predeterminados.  

## <a name="overview"></a>Información general

En Azure AD, un objeto de directiva representa un conjunto de reglas que se exigen en algunas o todas las aplicaciones de una organización. Cada tipo de directiva tiene una estructura única con un conjunto de propiedades que luego se aplican a los objetos a los que están asignadas.

Puede designar una directiva como la directiva predeterminada para su organización. La directiva se aplicará a cualquier aplicación que resida dentro de esa organización, siempre y cuando no se haya reemplazado por una directiva con una prioridad más alta. También puede asignar una directiva a aplicaciones específicas. El orden de prioridad varía según el tipo de directiva.

Para obtener ejemplos, lea [Configuración de la vigencia de los tokens](configure-token-lifetimes.md).

> [!NOTE]
> La directiva de vigencia del token configurable solo se aplica a los clientes móviles y de escritorio que tienen acceso a los recursos SharePoint Online y OneDrive para la Empresa, y no se aplica a las sesiones del explorador web.
> Para administrar la vigencia de las sesiones del explorador web para SharePoint Online y OneDrive para la Empresa, use la característica de [vigencia de sesiones de acceso condicional](../conditional-access/howto-conditional-access-session-lifetime.md). Consulte el [blog de SharePoint Online](https://techcommunity.microsoft.com/t5/SharePoint-Blog/Introducing-Idle-Session-Timeout-in-SharePoint-and-OneDrive/ba-p/119208) para más información sobre cómo configurar los tiempos de expiración de sesiones inactivas.

## <a name="token-types"></a>Tipos de token

Las directivas de vigencia del token se pueden establecer para tokens de actualización, tokens de acceso, tokens SAML, tokens de sesión y tokens de identificador.

### <a name="access-tokens"></a>Tokens de acceso

Los clientes utilizan tokens de acceso para acceder a un recurso protegido. Solo se puede utilizar un token de acceso para una combinación específica de usuario, cliente y recurso. Los tokens de acceso no se pueden revocar y son válidos hasta que caducan. Un individuo maltintencionado que haya obtenido un token de acceso puede usarlo durante toda su vigencia. El ajuste de la vigencia de un token de acceso es un balance entre la mejora del rendimiento del sistema y el aumento de la cantidad de tiempo que el cliente conserva el acceso después de que la cuenta de usuario está deshabilitada. Se consigue un rendimiento mejorado del sistema al reducir el número de veces que un cliente necesita adquirir un token de acceso nuevo.  El valor predeterminado es 1 hora. Después de 1 hora, el cliente debe usar el token de actualización para adquirir un nuevo token de actualización (normalmente en modo silencioso) y un token de acceso. 

### <a name="saml-tokens"></a>Tokens de SAML

Muchas aplicaciones SAAS basadas en web usan tokens SAML, que se obtienen mediante el punto de conexión del protocolo SAML2 de Azure Active Directory. También las usan las aplicaciones mediante WS-Federation. La duración predeterminada del token es de 1 hora. Desde la perspectiva de una aplicación, el período de validez del token se especifica mediante el valor NotOnOrAfter del elemento `<conditions …>` en el token. Una vez finalizado el período de validez del token, el cliente debe iniciar una nueva solicitud de autenticación, que a menudo se satisfará sin iniciar sesión de forma interactiva como resultado del token de sesión de inicio de sesión único (SSO).

El valor de NotOnOrAfter se puede cambiar mediante el parámetro `AccessTokenLifetime` en un `TokenLifetimePolicy`. Se establecerá en la duración configurada en la directiva, si la hay, más un factor de sesgo de reloj de cinco minutos.

Tenga en cuenta que la confirmación de asunto NotOnOrAfter especificada en el elemento `<SubjectConfirmationData>` no se ve afectada por la configuración de la vigencia del token. 

### <a name="refresh-tokens"></a>Tokens de actualización

Cuando un cliente adquiere un token de acceso para acceder a un recurso protegido, recibe también un token de actualización. El token de actualización se usa para obtener nuevos pares de tokens de acceso/actualización cuando el token de acceso actual expira. Un token de actualización se enlaza a una combinación de usuario y cliente. Se puede [revocar en cualquier momento](access-tokens.md#token-revocation) un token de actualización y se comprobará la validez de este cada vez que se use.  Los tokens de actualización no se revocan cuando se utilizan para obtener nuevos tokens de acceso; sin embargo, un procedimiento recomendado consiste en eliminar de forma segura el token antiguo cuando se obtiene uno nuevo. 

Es importante diferenciar entre clientes públicos y confidenciales, ya que esto afecta al período de tiempo que se pueden usar los tokens de actualización. Para más información sobre los diferentes tipos de clientes, consulte [RFC 6749](https://tools.ietf.org/html/rfc6749#section-2.1).

#### <a name="token-lifetimes-with-confidential-client-refresh-tokens"></a>Vigencia de los tokens de actualización de cliente confidencial
Los clientes confidenciales son aplicaciones que pueden almacenar de forma segura una contraseña (secreto) de un cliente. Pueden comprobar que las solicitudes proceden de la aplicación cliente protegida y no de un individuo malintencionado. Por ejemplo, una aplicación web es un cliente confidencial ya que puede almacenar un secreto de cliente en el servidor web. Y, por tanto, este secreto no se expone. Dado que estos flujos resultan más seguros, las duraciones predeterminadas de tokens de actualización emitidos para estos flujos será `until-revoked`, no se podrán cambiar mediante la directiva y no se revocarán en los restablecimientos de contraseña voluntarios.

#### <a name="token-lifetimes-with-public-client-refresh-tokens"></a>Vigencia de los tokens de actualización de cliente público

Los clientes públicos no pueden almacenar de forma segura una contraseña (secreto) de cliente. Por ejemplo, una aplicación de iOS o Android no puede ofuscar un secreto del propietario del recurso y, por ello, se considera a la aplicación un cliente público. Se pueden establecer directivas sobre los recursos para evitar que los tokens de actualización de clientes públicos anteriores a un período especificado obtengan un nuevo par de tokens de acceso/actualización. Para ello, utilice la [propiedad de tiempo máximo de inactividad del token de actualización](#refresh-token-max-inactive-time) (`MaxInactiveTime`). También puede utilizar directivas para establecer un período más allá del cual ya no se aceptan los tokens de actualización. Para ello, use la propiedad [Antigüedad máxima del token de actualización (un solo factor)](#single-factor-session-token-max-age) o [Antigüedad máxima del token de sesión (varios factores)](#multi-factor-refresh-token-max-age). Puede ajustar la vigencia del token de actualización para controlar cuándo y con qué frecuencia es necesario que el usuario vuelva a escribir las credenciales en lugar de volver a autenticarse de forma silenciosa al usar una aplicación cliente pública.

> [!NOTE]
> La propiedad Max Age es el período de tiempo que se puede usar un token único. 

### <a name="id-tokens"></a>Tokens de identificador
Los tokens de identificador se pasan a los clientes nativos y de sitios web. Los tokens de identificador contienen información de perfil de un usuario. Un token de identificador se enlaza a una combinación específica de usuario y cliente. Los tokens de identificador se consideran válidos hasta que expiran. Normalmente, una aplicación web relaciona la vigencia de la sesión de un usuario en la aplicación con la vigencia del token de identificador emitido para el usuario. Puede ajustar la vigencia del token de identificador para controlar la frecuencia con la que la aplicación web expirará la sesión de la aplicación y requerirá que el usuario se vuelva a autenticar en la Plataforma de identidad de Microsoft (de forma silenciosa o interactiva).

### <a name="single-sign-on-session-tokens"></a>Tokens de sesión de inicio de sesión único
Cuando un usuario se autentica con la Plataforma de identidad de Microsoft, se establece una sesión mediante inicio de sesión único (SSO) con la Plataforma de identidad de Microsoft y el explorador del usuario. El token de SSO, en forma de cookie, representa esta sesión. El token de sesión SSO no está enlazado a una aplicación cliente o de recursos específica. Los tokens de sesión SSO se pueden revocar y su validez se comprueba cada vez que se utilizan.

La Plataforma de identidad de Microsoft usa dos tipos de tokens de sesión SSO: persistente y no persistente. El explorador almacena los tokens de sesión persistentes como cookies. Los tokens de sesión no persistentes se almacenan como cookies de sesión. (Las cookies de sesión se destruyen cuando se cierra el explorador). Por lo general, se almacena un token de sesión no persistente. Pero, si el usuario selecciona la casilla **Mantener la sesión iniciada** durante la autenticación, un token de sesión persistente se almacena.

Los tokens de sesión no persistentes tienen una vigencia de 24 horas. Los tokens persistentes tienen una vigencia de 90 días. Cada vez que se utilice el token de sesión de inicio de sesión único dentro del período de validez, este se amplía otras 24 horas o 90 días, dependiendo del tipo de token. Si un token de sesión SSO no se usa dentro de su período de validez, se considerará caducado y ya no se aceptará.

Puede utilizar una directiva para establecer el período de tiempo después de la emisión del primer token de sesión, más allá del cual ya no se aceptará este token. (Para ello, utilice la propiedad de antigüedad máxima del token de sesión). Puede ajustar la vigencia de un token de sesión para controlar cuándo y con qué frecuencia el usuario debe volver a escribir las credenciales en lugar de autenticarse de forma silenciosa cuando se usa una aplicación web.

### <a name="token-lifetime-policy-properties"></a>Propiedades de la directiva de vigencia del token
Una directiva de vigencia del token es un tipo de objeto de directiva que contiene reglas de vigencia del token. Use las propiedades de la directiva para controlar las vigencias de tokens especificados. Si no se establece ninguna directiva, el sistema aplica el valor de vigencia predeterminado.

### <a name="configurable-token-lifetime-properties"></a>Propiedades de vigencia de tokens configurables
| Propiedad | Cadena de propiedad de directiva | Afecta a | Valor predeterminado | Mínima | Máxima |
| --- | --- | --- | --- | --- | --- |
| Vigencia del token de acceso |AccessTokenLifetime<sup>2</sup> |Tokens de acceso, tokens de identificador, tokens de SAML2 |1 hora |10 minutos |1 día |
| Tiempo máximo de inactividad del token de actualización |MaxInactiveTime |Tokens de actualización |90 días |10 minutos |90 días |
| Antigüedad máxima del token de actualización (un solo factor) |MaxAgeSingleFactor |Tokens de actualización (para los usuarios) |Hasta que se revoca |10 minutos |Hasta que se revoca<sup>1</sup> |
| Antigüedad máxima del token de actualización (varios factores) |MaxAgeMultiFactor |Tokens de actualización (para los usuarios) | 180 días |10 minutos |180 días<sup>1</sup> |
| Antigüedad máxima del token de sesión (un solo factor) |MaxAgeSessionSingleFactor |Tokens de sesión (persistentes y no persistentes) |Hasta que se revoca |10 minutos |Hasta que se revoca<sup>1</sup> |
| Antigüedad máxima del token de sesión (varios factores) |MaxAgeSessionMultiFactor |Tokens de sesión (persistentes y no persistentes) | 180 días |10 minutos | 180 días<sup>1</sup> |

* <sup>1</sup>365 días es la vigencia explícita máxima que se puede establecer para estos atributos.
* <sup>2</sup>Para que funcione el cliente web de Microsoft Teams, se recomienda establecer AccessTokenLifetime en un valor superior a 15 minutos para Microsoft Teams.

### <a name="exceptions"></a>Excepciones
| Propiedad | Afecta a | Valor predeterminado |
| --- | --- | --- |
| Antigüedad máxima de los tokens de actualización (emitidos para usuarios federados con información de revocación insuficiente<sup>1</sup>) |Tokens de actualización (emitidos para usuarios federados con información de revocación insuficiente<sup>1</sup>) |12 horas |
| Tiempo máximo de inactividad del token de actualización (emitido para clientes confidenciales) |Tokens de actualización (emitidos para clientes confidenciales) |90 días |
| Antigüedad máxima del token de actualización (emitido para clientes confidenciales) |Tokens de actualización (emitidos para clientes confidenciales) |Hasta que se revoca |

* <sup>1</sup> Entre los usuarios federados que tienen información de revocación insuficiente se incluyen todos los usuarios que no tienen el atributo "LastPasswordChangeTimestamp" sincronizado. A estos usuarios se les da esta antigüedad máxima tan corta porque Azure Active Directory no puede comprobar cuándo se deben revocar los tokens asociados a una credencial antigua (como una contraseña que se ha modificado) y deben realizar comprobaciones más frecuentes para asegurarse de que el usuario y los tokens asociados están aún activos. Para mejorar esta experiencia, los administradores de los inquilinos deben asegurarse de que sincronizan el atributo "LastPasswordChangeTimestamp" (esto se puede establecer en el objeto de usuario con PowerShell o mediante AADSync).

### <a name="policy-evaluation-and-prioritization"></a>Evaluación y prioridades de directivas
Puede crear y, a continuación, asignar una directiva de vigencia del token a una aplicación específica, a su organización y a las entidades de servicio. Se pueden aplicar varias directivas a una aplicación específica. La directiva de vigencia del token que entra en vigor sigue estas reglas:

* Si una directiva se asigna explícitamente a la entidad de servicio, se aplicará.
* Si no hay ninguna directiva que se asigne explícitamente a la entidad de servicio, se aplicará una directiva asignada explícitamente a la organización primaria de la entidad de servicio.
* Si no hay ninguna directiva que se asigne explícitamente a la entidad de servicio o a la organización, se aplicará la directiva asignada a la aplicación.
* Si no se ha asignado ninguna directiva a la entidad de servicio, la organización o el objeto de aplicación, se aplican los valores predeterminados. (Consulte la tabla que aparece en [Propiedades de vigencia de tokens configurables](#configurable-token-lifetime-properties)).

Para más información sobre la relación entre objetos de aplicación y objetos de entidad de servicio, consulte [Objetos de aplicación y de entidad de servicio de Azure Active Directory](app-objects-and-service-principals.md).

La validez de un token se evalúa en el momento en el que se usa. La directiva con la prioridad más alta en la aplicación a la que se accede es la que se aplica.

Todos los intervalos de tiempo usados aquí tienen formato según el objeto [TimeSpan](/dotnet/api/system.timespan) de C#: D.HH:MM:SS.  Por lo tanto, 80 días y 30 minutos sería `80.00:30:00`.  La D inicial puede eliminarse si es cero, por lo que 90 minutos sería `00:90:00`.  

> [!NOTE]
> Este es un escenario de ejemplo.
>
> Un usuario desea acceder a dos aplicaciones web: Aplicación web A y B.
> 
> Factores:
> * Ambas aplicaciones web están en la misma organización primaria.
> * La directiva 1 de vigencia del token con una antigüedad máxima del token de sesión de ocho horas se establece como valor predeterminado de la organización primaria.
> * La aplicación web A es una aplicación web de uso habitual que no está vinculada a ninguna directiva.
> * La aplicación web B se usa para procesos altamente confidenciales. Su entidad de servicio está vinculada a la directiva 2 de vigencia del token, que tiene una antigüedad máxima de token de sesión de 30 minutos.
>
> A las 12:00 p.m. el usuario inicia una nueva sesión en el explorador e intenta acceder a la aplicación web A. El usuario es redirigido a la Plataforma de identidad de Microsoft y se le pide que inicie sesión. Esta acción crea una cookie con un token de sesión en el explorador. El usuario es redirigido de nuevo a la aplicación web A con un token de identificador que le permite acceder a la aplicación.
>
> A las 12:15 p.m., el usuario intenta acceder a la aplicación web B. El explorador le redirige a la Plataforma de identidad de Microsoft, que detecta la cookie de sesión. La entidad de servicio de la aplicación web B está vinculada a la directiva 2 de vigencia del token, pero también forma parte de la organización primaria con la directiva 1 de vigencia del token predeterminada. La directiva 2 de vigencia del token se aplica porque las directivas vinculadas a entidades de servicio tienen una prioridad más alta que las directivas predeterminadas de la organización. El token de sesión se emitió originalmente en los últimos 30 minutos, por lo que se considera válido. El usuario es redirigido de nuevo a la aplicación web B con un token de identificador que le concede acceso.
>
> A la 1:00 p.m., el usuario intenta acceder a la aplicación web A y se le redirige a la Plataforma de identidad de Microsoft. La aplicación web A no está vinculada a ninguna directiva, pero como está en una organización con la directiva predeterminada 1 de vigencia del token, se aplica esta directiva. Se detectó la cookie de sesión que se emitió originalmente en las últimas ocho horas. En modo silencioso, se redirige al usuario a la aplicación web A con un nuevo token de identificador. No es necesario que el usuario se autentique.
>
> Inmediatamente después, el usuario intenta acceder a la aplicación web B y se le redirige a la Plataforma de identidad de Microsoft. Como antes, se aplica la directiva 2 de vigencia del token. Dado que el token se emitió hace más de 30 minutos, se le solicita al usuario que vuelva a escribir sus credenciales de inicio de sesión. Se emite un nuevo token de sesión y de identificador. El usuario puede acceder entonces a la aplicación web B.
>
>

## <a name="configurable-policy-property-details"></a>Detalles de las propiedades de directiva configurables
### <a name="access-token-lifetime"></a>Vigencia del token de acceso
**Cadena:** AccessTokenLifetime

**Afecta a:** Tokens de acceso, tokens de identificador, tokens de SAML2

**Resumen:** esta directiva controla cuánto tiempo se consideran válidos los token de acceso y de identificador para este recurso. Reducir la vigencia de los tokens de acceso disminuye el riesgo de que un individuo malintencionado use un token de acceso o de identificador durante un período de tiempo prolongado. (Estos tokens no se pueden revocar). El inconveniente es que afecta negativamente al rendimiento, ya que los tokens tendrán que reemplazarse con más frecuencia.

Para obtener un ejemplo, consulte [Creación de una directiva para inicio de sesión web](configure-token-lifetimes.md#create-a-policy-for-web-sign-in).

### <a name="refresh-token-max-inactive-time"></a>Tiempo máximo de inactividad del token de actualización
**Cadena:** MaxInactiveTime

**Afecta a:** Tokens de actualización

**Resumen:** esta directiva controla la antigüedad máxima que puede tener un token de actualización hasta que un cliente ya no pueda usarlo para recuperar un nuevo par de tokens de acceso/actualización al intentar acceder a este recurso. Dado que un nuevo token de actualización normalmente se devuelve cuando se usa un token de actualización, esta directiva impedirá el acceso si el cliente intenta acceder a algún recurso con el token de actualización actual durante el período de tiempo especificado.

Esta directiva obligará a los usuarios que no hayan estado activos en su cliente a volver a autenticarse para recuperar un nuevo token de actualización.

El tiempo máximo de inactividad del token de actualización debe establecerse en un valor inferior a la antigüedad máxima del token de un solo factor y la antigüedad máxima del token de actualización de varios factores.

Para obtener un ejemplo, consulte [Creación de una directiva para una aplicación nativa que llama a una API web](configure-token-lifetimes.md#create-a-policy-for-a-native-app-that-calls-a-web-api).

### <a name="single-factor-refresh-token-max-age"></a>Antigüedad máxima del token de actualización (un solo factor)
**Cadena:** MaxAgeSingleFactor

**Afecta a:** Tokens de actualización

**Resumen:** esta directiva controla cuánto tiempo un usuario puede seguir usando tokens de actualización para obtener nuevos pares de tokens de acceso/actualización desde la última vez que se autenticara correctamente con un solo factor. Después de que un usuario se autentica y recibe un nuevo token de actualización, este puede utilizar el flujo del token de actualización durante el período de tiempo especificado. (Esto ocurre siempre que el token de actualización actual no esté revocado y no se quede sin usar más tiempo que el período de inactividad). En ese momento, el usuario se verá obligado a volver a autenticarse para recibir un nuevo token de actualización.

Reducir la antigüedad máxima obliga a los usuarios a autenticarse con más frecuencia. Puesto que la autenticación de un solo factor se considera menos segura que la autenticación multifactor, se recomienda establecer esta propiedad en un valor igual o inferior al de la propiedad de antigüedad máxima del token de actualización de varios factores.

Para obtener un ejemplo, consulte [Creación de una directiva para una aplicación nativa que llama a una API web](configure-token-lifetimes.md#create-a-policy-for-a-native-app-that-calls-a-web-api).

### <a name="multi-factor-refresh-token-max-age"></a>Antigüedad máxima del token de actualización (varios factores)
**Cadena:** MaxAgeMultiFactor

**Afecta a:** Tokens de actualización

**Resumen:** esta directiva controla cuánto tiempo un usuario puede seguir usando tokens de actualización para obtener nuevos pares de tokens de acceso/actualización desde la última vez que se autenticara correctamente con varios factores. Después de que un usuario se autentica y recibe un nuevo token de actualización, este puede utilizar el flujo del token de actualización durante el período de tiempo especificado. (Esto ocurre siempre que el token de actualización actual no esté revocado y no se quede sin usar más tiempo que el período de inactividad). En ese momento, los usuarios se verán obligados a volver a autenticarse para recibir un nuevo token de actualización.

Reducir la antigüedad máxima obliga a los usuarios a autenticarse con más frecuencia. Puesto que la autenticación de un solo factor se considera menos segura que la autenticación multifactor, se recomienda establecer esta propiedad en un valor igual o superior al de la propiedad de antigüedad máxima del token de actualización de un solo factor.

Para obtener un ejemplo, consulte [Creación de una directiva para una aplicación nativa que llama a una API web](configure-token-lifetimes.md#create-a-policy-for-a-native-app-that-calls-a-web-api).

### <a name="single-factor-session-token-max-age"></a>Antigüedad máxima del token de sesión (un solo factor)
**Cadena:** MaxAgeSessionSingleFactor

**Afecta a:** Tokens de sesión (persistentes y no persistentes)

**Resumen:** esta directiva controla cuánto tiempo un usuario puede seguir usando tokens de actualización para obtener un nuevo token de identificador y de sesión desde la última vez que se autenticara correctamente con un solo factor. Después de que un usuario se autentica y recibe un nuevo token de sesión, este puede utilizar el flujo del token de sesión durante el período de tiempo especificado. (Esto ocurre siempre que el token de sesión actual no esté revocado y no haya expirado). Tras el período de tiempo especificado, el usuario se ve obligado a autenticarse para recibir un nuevo token de sesión.

Reducir la antigüedad máxima obliga a los usuarios a autenticarse con más frecuencia. Puesto que la autenticación de un solo factor se considera menos segura que la autenticación multifactor, se recomienda establecer esta propiedad en un valor igual o inferior al de la propiedad de antigüedad máxima del token de sesión de varios factores.

Para obtener un ejemplo, consulte [Creación de una directiva para inicio de sesión web](configure-token-lifetimes.md#create-a-policy-for-web-sign-in).

### <a name="multi-factor-session-token-max-age"></a>Antigüedad máxima del token de sesión (varios factores)
**Cadena:** MaxAgeSessionMultiFactor

**Afecta a:** Tokens de sesión (persistentes y no persistentes)

**Resumen:** esta directiva controla cuánto tiempo un usuario puede seguir usando tokens de actualización para obtener un nuevo token de identificador y de sesión desde la última vez que se autenticara correctamente con varios factores. Después de que un usuario se autentica y recibe un nuevo token de sesión, este puede utilizar el flujo del token de sesión durante el período de tiempo especificado. (Esto ocurre siempre que el token de sesión actual no esté revocado y no haya expirado). Tras el período de tiempo especificado, el usuario se ve obligado a autenticarse para recibir un nuevo token de sesión.

Reducir la antigüedad máxima obliga a los usuarios a autenticarse con más frecuencia. Puesto que la autenticación de un solo factor se considera menos segura que la autenticación multifactor, se recomienda establecer esta propiedad en un valor igual o superior al de la propiedad de antigüedad máxima del token de sesión de un solo factor.

## <a name="cmdlet-reference"></a>Referencia de cmdlets

Estos son los cmdlets del [módulo de versión preliminar de PowerShell para Graph de Azure Active Directory](/powershell/module/azuread/?view=azureadps-2.0-preview#service-principals&preserve-view=true&preserve-view=true).

### <a name="manage-policies"></a>Administración de directivas

Los cmdlets siguientes se pueden usar para administrar directivas.

| Cmdlet | Descripción | 
| --- | --- |
| [New-AzureADPolicy](/powershell/module/azuread/new-azureadpolicy?view=azureadps-2.0-preview&preserve-view=true) | Crea una nueva directiva. |
| [Get-AzureADPolicy](/powershell/module/azuread/get-azureadpolicy?view=azureadps-2.0-preview&preserve-view=true) | Obtiene todas las directivas de AzureAD o una directiva especificada. |
| [Get-AzureADPolicyAppliedObject](/powershell/module/azuread/get-azureadpolicyappliedobject?view=azureadps-2.0-preview&preserve-view=true) | Obtiene todas las aplicaciones y entidades de servicio vinculadas a una directiva. |
| [Set-AzureADPolicy](/powershell/module/azuread/set-azureadpolicy?view=azureadps-2.0-preview&preserve-view=true) | Actualiza una directiva existente. |
| [Remove-AzureADPolicy](/powershell/module/azuread/remove-azureadpolicy?view=azureadps-2.0-preview&preserve-view=true) | Elimina la directiva especificada. |

### <a name="application-policies"></a>Directivas de aplicación
Los cmdlets siguientes se pueden usar para directivas de aplicación.</br></br>

| Cmdlet | Descripción | 
| --- | --- |
| [Add-AzureADApplicationPolicy](/powershell/module/azuread/add-azureadapplicationpolicy?view=azureadps-2.0-preview&preserve-view=true) | Vincula la directiva especificada a una aplicación. |
| [Get-AzureADApplicationPolicy](/powershell/module/azuread/get-azureadapplicationpolicy?view=azureadps-2.0-preview&preserve-view=true) | Obtiene la directiva asignada a una aplicación. |
| [Remove-AzureADApplicationPolicy](/powershell/module/azuread/remove-azureadapplicationpolicy?view=azureadps-2.0-preview&preserve-view=true) | Quita una directiva de una aplicación. |

### <a name="service-principal-policies"></a>Directivas de la entidad de servicio
Los cmdlets siguientes se pueden usar para las directivas de entidad de servicio.

| Cmdlet | Descripción | 
| --- | --- |
| [Add-AzureADServicePrincipalPolicy](/powershell/module/azuread/add-azureadserviceprincipalpolicy?view=azureadps-2.0-preview&preserve-view=true) | Vincula la directiva especificada a una entidad de servicio. |
| [Get-AzureADServicePrincipalPolicy](/powershell/module/azuread/get-azureadserviceprincipalpolicy?view=azureadps-2.0-preview&preserve-view=true) | Obtiene cualquier directiva vinculada a la entidad de servicio especificada.|
| [Remove-AzureADServicePrincipalPolicy](/powershell/module/azuread/remove-azureadserviceprincipalpolicy?view=azureadps-2.0-preview&preserve-view=true) | Quita la directiva de la entidad de servicio especificada.|

## <a name="license-requirements"></a>Requisitos de licencia

Necesita una licencia de Azure AD Premium P1 para usar esta característica. Para obtener la licencia correcta para sus requisitos, consulte [Comparación de las características con disponibilidad general de las ediciones Gratis y Prémium](https://azure.microsoft.com/pricing/details/active-directory/).

Los clientes con [licencias de Microsoft 365 Empresa](/office365/servicedescriptions/microsoft-365-service-descriptions/microsoft-365-business-service-description) también tienen acceso a características de acceso condicional.

## <a name="next-steps"></a>Pasos siguientes

Para obtener más información, lea los [ejemplos configuración de la vigencia de los tokens](configure-token-lifetimes.md).
