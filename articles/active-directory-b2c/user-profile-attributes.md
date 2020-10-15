---
title: Atributos de perfil de usuario en Azure Active Directory B2C
description: Obtenga información sobre los atributos de tipo de recurso de usuario que son compatibles con el perfil de usuario del directorio de Azure AD B2C. Descubra los atributos integrados, las extensiones y cómo se asignan los atributos a Microsoft Graph.
services: active-directory-b2c
author: msmimart
manager: celestedg
ms.service: active-directory
ms.workload: identity
ms.topic: conceptual
ms.date: 3/20/2020
ms.author: mimart
ms.subservice: B2C
ms.openlocfilehash: 1e6965e15b7482935148ae7fcd2edf0f3cc722b2
ms.sourcegitcommit: 829d951d5c90442a38012daaf77e86046018e5b9
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/09/2020
ms.locfileid: "83738565"
---
# <a name="user-profile-attributes"></a>Atributos de perfil de usuario

El perfil de usuario del directorio de Azure Active Directory (Azure AD) B2C incluye un conjunto integrado de atributos, por ejemplo, givenName, surname, city, postalCode y telephoneNumber. Puede extender el perfil de usuario con sus propios datos de aplicación sin necesitar un almacén de datos externo. La mayoría de los atributos que se pueden utilizar con los perfiles de usuario de Azure AD B2C también se admiten en Microsoft Graph. En este artículo se describen los atributos de perfil de usuario que se admiten en Azure AD B2C. También se indican los atributos que no son compatibles con Microsoft Graph, así como los atributos de Microsoft Graph que no se deben usar con Azure AD B2C.

> [!IMPORTANT]
> No debe utilizar atributos de extensión o integrados para almacenar datos personales confidenciales, como credenciales de cuenta, números de identificación gubernamental, datos de titulares de tarjetas, datos de cuentas financieras, información sanitaria o la información básica confidencial.

También pueden integrarse con sistemas externos. Por ejemplo, puede usar Azure AD B2C para la autenticación, pero delegar en una base de datos externa de administración de las relaciones con el cliente (CRM) o de fidelización de clientes como el origen de autoridad de los datos de los clientes. Para obtener más información, consulte la solución de [perfil remoto](https://github.com/azure-ad-b2c/samples/tree/master/policies/remote-profile).

En la tabla siguiente se enumeran los atributos de [tipo de recurso de usuario](https://docs.microsoft.com/graph/api/resources/user) que son compatibles con el perfil de usuario del directorio de Azure AD B2C. En la tabla siguiente se proporciona información sobre cada atributo:

- El nombre de atributo que usa Azure AD B2C (seguido del nombre de Microsoft Graph entre paréntesis, si es diferente).
- El tipo de datos del atributo.
- La descripción del atributo.
- Si el atributo está disponible en Azure Portal.
- Si el atributo se puede usar en un flujo de usuario.
- Si el atributo se puede usar en un [perfil técnico de Azure AD](active-directory-technical-profile.md) de una directiva personalizada, y en qué sección (&lt;InputClaims&gt;, &lt;OutputClaims&gt;o &lt;PersistedClaims&gt;).

|Nombre     |Tipo     |Descripción|Azure Portal|Flujos de usuario|Directiva personalizada|
|---------|---------|----------|------------|----------|-------------|
|accountEnabled  |Boolean|Si la cuenta de usuario está habilitada o deshabilitada: **true** si la cuenta está habilitada; en caso contrario, **false**.|Sí|No|Persistente, salida|
|ageGroup        |String|Grupo de edad del usuario. Valores posibles: null, Undefined, Minor, Adult y NotAdult.|Sí|No|Persistente, salida|
|alternativeSecurityId ([Identidades](manage-user-accounts-graph-api.md#identities-property))|String|Una única identidad de usuario del proveedor de identidades externo.|No|No|Entrada, persistente, salida|
|alternativeSecurityIds ([Identidades](manage-user-accounts-graph-api.md#identities-property))|Colección de identidades de seguridad alternativa|Una colección de identidades de usuario de proveedores de identidades externos.|No|No|Persistente, salida|
|city            |String|La ciudad en la que se encuentra el usuario. La longitud máxima es 128.|Sí|Sí|Persistente, salida|
|consentProvidedForMinor|String|Indica si se ha proporcionado el consentimiento para un menor. Valores permitidos: null, granted, denied o notRequired.|Sí|No|Persistente, salida|
|country         |String|El país o la región donde se encuentra el usuario. Ejemplo: "EE. UU." o "Reino Unido". La longitud máxima es 128.|Sí|Sí|Persistente, salida|
|createdDateTime|DateTime|La fecha de creación del objeto de usuario. Solo lectura.|No|No|Persistente, salida|
|creationType    |String|Si la cuenta de usuario se creó como una cuenta local para un inquilino de Azure Active Directory B2C, el valor es LocalAccount o nameCoexistence. Solo lectura.|No|No|Persistente, salida|
|dateOfBirth     |Date|La fecha de nacimiento.|No|No|Persistente, salida|
|department      |String|El nombre del departamento en el que trabaja el usuario. La longitud máxima es 64.|Sí|No|Persistente, salida|
|DisplayName     |String|El nombre para mostrar del usuario. La longitud máxima es 256.|Sí|Sí|Persistente, salida|
|facsimileTelephoneNumber<sup>1</sup>|String|El número de teléfono del equipo de fax del trabajo del usuario.|Sí|No|Persistente, salida|
|givenName       |String|EL nombre del usuario. La longitud máxima es 64.|Sí|Sí|Persistente, salida|
|jobTitle        |String|El puesto del usuario. La longitud máxima es 128.|Sí|Sí|Persistente, salida|
|immutableId     |String|Un identificador que se usa normalmente para los usuarios migrados desde Active Directory local.|No|No|Persistente, salida|
|legalAgeGroupClassification|String|La clasificación de grupo de edad legal. Valor de solo lectura que se calcula en función de las propiedades ageGroup y consentProvidedForMinor. Valores permitidos: null, minorWithOutParentalConsent, minorWithParentalConsent, minorNoParentalConsentRequired, notAdult y adult.|Sí|No|Persistente, salida|
|legalCountry<sup>1</sup>  |String|País o región para fines legales.|No|No|Persistente, salida|
|mail            |String|La dirección SMTP del usuario, por ejemplo, "bob@contoso.com". Solo lectura.|No|No|Persistente, salida|
|mailNickName    |String|El alias de correo electrónico del usuario. La longitud máxima es 64.|No|No|Persistente, salida|
|mobile (mobilePhone) |String|EL número de teléfono móvil principal del usuario. La longitud máxima es 64.|Sí|No|Persistente, salida|
|netId           |String|El identificador de red.|No|No|Persistente, salida|
|objectId        |String|El identificador único global (GUID) que es el identificador único del usuario. Ejemplo: 12345678-9abc-def0-1234-56789abcde. Valor de solo lectura e inmutable.|Solo lectura|Sí|Entrada, persistente, salida|
|otherMails      |Colección de cadenas|Una lista de direcciones de correo electrónico adicionales del usuario. Ejemplo: ["bob@contoso.com", "Robert@fabrikam.com"].|Sí (correo electrónico alternativo)|No|Persistente, salida|
|password        |String|La contraseña de la cuenta local durante la creación del usuario.|No|No|Guardado|
|passwordPolicies     |String|La directiva de la contraseña. Es una cadena que consta de un nombre de directiva diferente separado por una coma. Por ejemplo: "DisablePasswordExpiration, DisableStrongPassword".|No|No|Persistente, salida|
|physicalDeliveryOfficeName (officeLocation)|String|La ubicación del lugar de trabajo del usuario. La longitud máxima es 128.|Sí|No|Persistente, salida|
|postalCode      |String|El código postal de la dirección del usuario. El código postal es específico del país o la región del usuario. En Estados Unidos de América, este atributo contiene el código ZIP. La longitud máxima es 40.|Sí|No|Persistente, salida|
|preferredLanguage    |String|El idioma preferido del usuario. Debe seguir el código ISO 639-1. Ejemplo: "en-US".|No|No|Persistente, salida|
|refreshTokensValidFromDateTime|DateTime|Los tokens de actualización emitidos antes de esta hora no son válidos. Las aplicaciones obtendrán un error al usar un token de actualización no válido para adquirir un nuevo token de acceso. Si esto ocurre, la aplicación tendrá que adquirir un nuevo token de actualización realizando una solicitud al punto de conexión de autorización. Solo lectura.|No|No|Output|
|signInNames ([Identidades](manage-user-accounts-graph-api.md#identities-property)) |String|El nombre de inicio de sesión único del usuario de cuenta local de cualquier tipo en el directorio. Se usa para obtener un usuario con el valor de inicio de sesión sin especificar el tipo de cuenta local.|No|No|Entrada|
|signInNames.userName ([Identidades](manage-user-accounts-graph-api.md#identities-property)) |String|El nombre de inicio de sesión único del usuario de cuenta local en el directorio. Se usa para crear u obtener un usuario con un nombre de usuario de inicio de sesión específico. Si se especifica en PersistedClaims solo durante la operación de revisión, se quitarán otros tipos de signInNames. Si desea agregar un nuevo tipo de signInNames, también debe conservar los atributos signInNames que ya existan.|No|No|Entrada, persistente, salida|
|signInNames.phoneNumber ([Identidades](manage-user-accounts-graph-api.md#identities-property)) |String|El número de teléfono único del usuario de cuenta local en el directorio. Se usa para crear u obtener un usuario con un número de teléfono de inicio de sesión específico. Si se especifica en PersistedClaims solo durante la operación de revisión, se quitarán otros tipos de signInNames. Si desea agregar un nuevo tipo de signInNames, también debe conservar los atributos signInNames que ya existan.|No|No|Entrada, persistente, salida|
|signInNames.emailAddress ([Identidades](manage-user-accounts-graph-api.md#identities-property))|String|La dirección de correo electrónico única del usuario de cuenta local en el directorio. Se usa para crear u obtener un usuario con una dirección de correo electrónico de inicio de sesión específica. Si se especifica en PersistedClaims solo durante la operación de revisión, se quitarán otros tipos de signInNames. Si desea agregar un nuevo tipo de signInNames, también debe conservar los atributos signInNames que ya existan.|No|No|Entrada, persistente, salida|
|state           |String|El estado o provincia de la dirección del usuario. La longitud máxima es 128.|Sí|Sí|Persistente, salida|
|streetAddress   |String|La dirección postal del lugar de trabajo del usuario. La longitud máxima es 1024.|Sí|Sí|Persistente, salida|
|strongAuthentication AlternativePhoneNumber<sup>1</sup>|String|El número de teléfono secundario del usuario que se usa para la autenticación multifactor.|Sí|No|Persistente, salida|
|strongAuthenticationEmailAddress<sup>1</sup>|String|La dirección SMTP del usuario. Ejemplo: "bob@contoso.com". Este atributo se usa para el inicio de sesión con la directiva de nombre de usuario, para almacenar la dirección de correo electrónico del usuario. La dirección de correo electrónico que se usa en un flujo de restablecimiento de contraseña.|Sí|No|Persistente, salida|
|strongAuthenticationPhoneNumber<sup>1</sup>|String|El número de teléfono principal del usuario que se usa para la autenticación multifactor.|Sí|No|Persistente, salida|
|surname         |String|Los apellidos del usuario. La longitud máxima es 64.|Sí|Sí|Persistente, salida|
|telephoneNumber (primera entrada de businessPhones)|String|El número de teléfono principal del lugar de trabajo del usuario.|Sí|No|Persistente, salida|
|userPrincipalName    |String|El nombre principal de usuario (UPN) del usuario. El UPN es un nombre de inicio de sesión del estilo de Internet del usuario basado en el estándar de Internet RFC 822. El dominio debe encontrarse en la colección de dominios comprobados del inquilino. Esta propiedad es necesaria cuando se crea una cuenta. El valor es inmutable.|No|No|Entrada, persistente, salida|
|usageLocation   |String|Es necesario para los usuarios a los que se asignarán licencias debido al requisito legal de comprobar la disponibilidad de los servicios en los países o regiones. No acepta valores NULL. Código de país o región de dos letras (norma ISO 3166). Ejemplos: "US", "JP" y "GB".|Sí|No|Persistente, salida|
|userType        |String|Un valor de cadena que se puede usar para clasificar tipos de usuario en el directorio. El valor debe ser Miembro. Solo lectura.|Solo lectura|No|Persistente, salida|
|userState (externalUserState)<sup>2</sup>|String|Solo para la cuenta de Azure AD B2B; indica si la invitación está en el estado PendingAcceptance o Accepted.|No|No|Persistente, salida|
|userStateChangedOn (externalUserStateChangeDateTime)<sup>2</sup>|DateTime|Muestra la marca de tiempo del cambio más reciente de la propiedad UserState.|No|No|Persistente, salida|
|<sup>1 </sup>No es compatible con Microsoft Graph.<br><sup>2 </sup>No debe utilizarse con Azure AD B2C.||||||


## <a name="extension-attributes"></a>Atributos de extensión

A menudo, necesitará crear sus propios atributos, como en los siguientes casos:

- Una aplicación orientada al cliente necesita persistir en un atributo como **LoyaltyNumber**.
- Un proveedor de identidades tiene un identificador de usuario único como **uniqueUserGUID** que se debe guardar.
- Un recorrido del usuario personalizado debe persistir en un estado de un usuario como **migrationStatus**.

Azure AD B2C extiende el conjunto de atributos almacenado en cada cuenta de usuario. Los atributos de extensión [extienden el esquema](https://docs.microsoft.com/graph/extensibility-overview#schema-extensions) de los objetos de usuario en el directorio. Los atributos de extensión solo se pueden registrar en un objeto de aplicación, aunque podrían contener datos de un usuario. El atributo de extensión se adjunta a la aplicación denominada b2c-extensions-app. No modifique esta aplicación, ya que la usa Azure AD B2C para almacenar los datos de usuario. Puede encontrar esta aplicación en los registros de aplicaciones de Azure Active Directory.

> [!NOTE]
> - Se pueden escribir hasta 100 atributos de extensión en cualquier cuenta de usuario.
> - Si se elimina la aplicación b2c-extensions-app, estos atributos de extensión se quitan de todos los usuarios, junto con los datos que contienen.
> - Si la aplicación elimina un atributo de extensión, se quita de todas las cuentas de usuario y los valores se eliminan.
> - El nombre subyacente del atributo de extensión se genera con el formato "Extensión_" + Identificador de aplicación + "_" + Nombre del atributo. Por ejemplo, si crea un atributo de extensión LoyaltyNumber y el identificador de aplicación de b2c-extensions-app es 831374b3-bd50-41bf-aa54-263ec9e050fc, el nombre del atributo de extensión subyacente será: extension_831374b3bd5041bfaa54263ec9e050fc_LoyaltyNumber. Use el nombre subyacente al ejecutar consultas de Graph API para crear o actualizar cuentas de usuario.

Se admiten los siguientes tipos de datos al definir una propiedad en una extensión de esquema:

|Tipo de propiedad |Observaciones  |
|--------------|---------|
|Boolean    | Valores posibles: **true** o **false**. |
|DateTime   | Debe especificarse en formato ISO 8601. Se almacenará en UTC.   |
|Entero    | Valor de 32 bits.               |
|String     | 256 caracteres como máximo.     |

## <a name="next-steps"></a>Pasos siguientes
Obtenga más información sobre los atributos de extensión:
- [Extensiones de esquema](https://docs.microsoft.com/graph/extensibility-overview#schema-extensions)
- [Definición de atributos personalizados con flujos de usuario](user-flow-custom-attributes.md)
- [Definición de atributos personalizados con directivas personalizadas](custom-policy-custom-attributes.md)
