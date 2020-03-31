---
title: 'Localization: Azure Active Directory B2C'
description: Especifique el elemento Localization de una directiva personalizada en Azure Active Directory B2C.
services: active-directory-b2c
author: msmimart
manager: celestedg
ms.service: active-directory
ms.workload: identity
ms.topic: reference
ms.date: 03/11/2020
ms.author: mimart
ms.subservice: B2C
ms.openlocfilehash: e73eae4d66f4ff94a48dfa27e258f8ba8ef87633
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/28/2020
ms.locfileid: "79126758"
---
# <a name="localization"></a>Localización

[!INCLUDE [active-directory-b2c-advanced-audience-warning](../../includes/active-directory-b2c-advanced-audience-warning.md)]

El elemento **Localization** permite incluir varios idiomas o configuraciones regionales en la directiva para los recorridos del usuario. Admitir la localización en las directivas le permite:

- Configurar la lista explícita de los idiomas admitidos en una directiva y seleccionar un idioma predeterminado.
- Proporcionar cadenas y colecciones específicas del idioma.

```XML
<Localization Enabled="true">
  <SupportedLanguages DefaultLanguage="en" MergeBehavior="ReplaceAll">
    <SupportedLanguage>en</SupportedLanguage>
    <SupportedLanguage>es</SupportedLanguage>
  </SupportedLanguages>
  <LocalizedResources Id="api.localaccountsignup.en">
  <LocalizedResources Id="api.localaccountsignup.es">
  ...
```

El elemento **Localization** contiene los atributos siguientes:

| Atributo | Obligatorio | Descripción |
| --------- | -------- | ----------- |
| habilitado | No | Valores posibles: `true` o `false`. |

El elemento **Localization** contiene los siguientes elementos XML:

| Elemento | Repeticiones | Descripción |
| ------- | ----------- | ----------- |
| SupportedLanguages | 1:n | Lista de idiomas admitidos. |
| LocalizedResources | 0:n | Lista de recursos localizados. |

## <a name="supportedlanguages"></a>SupportedLanguages

El elemento **SupportedLanguages** contiene los atributos siguientes:

| Atributo | Obligatorio | Descripción |
| --------- | -------- | ----------- |
| DefaultLanguage | Sí | El idioma que se usará como el predeterminado para los recursos localizados. |
| MergeBehavior | No | Valores de enumeración de los valores combinados con cualquier elemento ClaimType presente en una directiva principal con el mismo identificador. Use este atributo al sobrescribir una notificación especificada en la directiva de base. Valores posibles: `Append`, `Prepend` o `ReplaceAll`. El valor `Append` especifica que la colección de datos presente tiene que anexarse al final de la colección especificada en la directiva principal. El valor `Prepend` especifica que la colección de datos presente tiene que agregarse antes de la colección especificada en la directiva principal. El valor `ReplaceAll` especifica que la colección de datos definidos en la directiva principal tiene que omitirse y, en su lugar, tienen que usarse los datos definidos en la directiva actual. |

### <a name="supportedlanguages"></a>SupportedLanguages

El elemento **SupportedLanguages** contiene los elementos siguientes:

| Elemento | Repeticiones | Descripción |
| ------- | ----------- | ----------- |
| SupportedLanguage | 1:n | Muestra contenido que cumple con una etiqueta de idioma según RFC 5646 (etiquetas para identificar idiomas). |

## <a name="localizedresources"></a>LocalizedResources

El elemento **LocalizedResources** contiene los atributos siguientes:

| Atributo | Obligatorio | Descripción |
| --------- | -------- | ----------- |
| Identificador | Sí | Identificador único para recursos localizados. |

El elemento **LocalizedResources** contiene los elementos siguientes:

| Elemento | Repeticiones | Descripción |
| ------- | ----------- | ----------- |
| LocalizedCollections | 0:n | Define colecciones enteras de distintas referencias culturales. Una colección puede tener un número distinto de elementos y cadenas diferentes para varias referencias culturales. Entre los ejemplos de colecciones, se incluyen las enumeraciones que aparecen en los tipos de notificación. Por ejemplo, una lista de países o regiones se muestra al usuario en una lista desplegable. |
| LocalizedStrings | 0:n | Define todas las cadenas en distintas referencias culturales, excepto las que aparecen en colecciones. |

### <a name="localizedcollections"></a>LocalizedCollections

El elemento **LocalizedCollections** contiene los elementos siguientes:

| Elemento | Repeticiones | Descripción |
| ------- | ----------- | ----------- |
| LocalizedCollection | 1:n | Lista de idiomas admitidos. |

#### <a name="localizedcollection"></a>LocalizedCollection

El elemento **LocalizedCollection** contiene los atributos siguientes:

| Atributo | Obligatorio | Descripción |
| --------- | -------- | ----------- |
| ElementType | Sí | Hace referencia a un elemento ClaimType o a un elemento de la interfaz de usuario en el archivo de directiva. |
| ElementId | Sí | Cadena que contiene una referencia a un tipo de notificación que ya se ha definido en la sección ClaimsSchema y que se usa si se establece **ElementType** en un elemento ClaimType. |
| TargetCollection | Sí | La colección de destino. |

El elemento **LocalizedCollection** contiene los elementos siguientes:

| Elemento | Repeticiones | Descripción |
| ------- | ----------- | ----------- |
| Elemento | 0:n | Define una opción disponible para que el usuario seleccione una notificación en la interfaz de usuario, como un valor en una lista desplegable. |

El elemento **Item** contiene los atributos siguientes:

| Atributo | Obligatorio | Descripción |
| --------- | -------- | ----------- |
| Texto | Sí | Cadena para mostrar fácil de identificar que se mostrará al usuario en la interfaz de usuario para esta opción. |
| Value | Sí | Valor de notificación de cadena asociado a la selección de esta opción. |
| SelectByDefault | No | Indica si esta opción se puede seleccionar o no de forma predeterminada en la interfaz de usuario. Valores posibles: True o False. |

En el ejemplo siguiente, se muestra cómo usar el elemento **LocalizedCollections**. Contiene dos elementos **LocalizedCollection**, uno para inglés y otro para español. Ambos establecen la colección **Restriction** de la notificación `Gender` con una lista de elementos para inglés y español.

```XML
<LocalizedResources Id="api.selfasserted.en">
 <LocalizedCollections>
   <LocalizedCollection ElementType="ClaimType" ElementId="Gender" TargetCollection="Restriction">
      <Item Text="Female" Value="F" />
      <Item Text="Male" Value="M" />
    </LocalizedCollection>
</LocalizedCollections>

<LocalizedResources Id="api.selfasserted.es">
 <LocalizedCollections>
   <LocalizedCollection ElementType="ClaimType" ElementId="Gender" TargetCollection="Restriction">
      <Item Text="Femenino" Value="F" />
      <Item Text="Masculino" Value="M" />
    </LocalizedCollection>
</LocalizedCollections>
```

### <a name="localizedstrings"></a>LocalizedStrings

El elemento **LocalizedStrings** contiene los elementos siguientes:

| Elemento | Repeticiones | Descripción |
| ------- | ----------- | ----------- |
| LocalizedString | 1:n | Cadena localizada. |

El elemento **LocalizedString** contiene los atributos siguientes:

| Atributo | Obligatorio | Descripción |
| --------- | -------- | ----------- |
| ElementType | Sí | Referencia a un elemento de tipo de notificación o a un elemento de la interfaz de usuario en la directiva. Valores posibles: `ClaimType`, `UxElement`, `ErrorMessage`, `Predicate` o `GetLocalizedStringsTransformationClaimType`. El valor `ClaimType` se usa para localizar uno de los atributos de la notificación, según se especifica en el elemento StringId. El valor `UxElement` se usa para localizar uno de los elementos de la interfaz de usuario, según se especifica en el elemento StringId. El valor `ErrorMessage` se usa para localizar uno de los mensajes de error del sistema, según se especifica en el elemento StringId. El valor `Predicate` se usa para localizar uno de los mensajes de error de [Predicate](predicates.md), según se especifica en el elemento StringId. El valor `InputValidation` se usa para localizar uno de los mensajes de error de grupo de [PredicateValidation](predicates.md), según se especifica en el elemento StringId. El valor `GetLocalizedStringsTransformationClaimType` se usa para copiar las cadenas localizadas en las notificaciones. Para más información, consulte la [transformación de notificaciones GetLocalizedStringsTransformation](string-transformations.md#getlocalizedstringstransformation).  | 
| ElementId | Sí | Si **ElementType** se establece en `ClaimType`, `Predicate` o `InputValidation`, este elemento contiene una referencia a un tipo de notificación que ya se ha definido en la sección ClaimsSchema. |
| StringId | Sí | Si **ElementType** se establece en `ClaimType`, este elemento contiene una referencia a un atributo de un tipo de notificación. Valores posibles: `DisplayName`, `AdminHelpText` o `PatternHelpText`. El valor `DisplayName` se usa para establecer el nombre para mostrar de la notificación. El valor `AdminHelpText` se usa para establecer el nombre del texto de ayuda del usuario de la notificación. El valor `PatternHelpText` se usa para establecer el texto de ayuda del patrón de la notificación. Si **ElementType** se establece en `UxElement`, este elemento contiene una referencia a un atributo de un elemento de la interfaz de usuario. Si **ElementType** se establece en `ErrorMessage`, este elemento especifica el identificador de un mensaje de error. Vea la lista completa de los identificadores de `UxElement` en [Id. de cadena Localization](localization-string-ids.md).|


En el ejemplo siguiente, se muestra una página de registro localizada. Los primeros tres valores de **LocalizedString** establecen el atributo de la notificación. El tercero cambia el valor del botón Continuar. El último cambia el mensaje de error.

```XML
<LocalizedResources Id="api.selfasserted.en">
  <LocalizedStrings>
    <LocalizedString ElementType="ClaimType" ElementId="email" StringId="DisplayName">Email</LocalizedString>
    <LocalizedString ElementType="ClaimType" ElementId="email" StringId="UserHelpText">Please enter your email</LocalizedString>
    <LocalizedString ElementType="ClaimType" ElementId="email" StringId="PatternHelpText">Please enter a valid email address</LocalizedString>
    <LocalizedString ElementType="UxElement" StringId="button_continue">Create new account</LocalizedString>
   <LocalizedString ElementType="ErrorMessage" StringId="UserMessageIfClaimsPrincipalAlreadyExists">The account you are trying to create already exists, please sign-in.</LocalizedString>
  </LocalizedStrings>
</LocalizedResources>
```

En el ejemplo siguiente, se muestra una localización de **UserHelpText** de **Predicate** con el id. `IsLengthBetween8And64`. Y un elemento **UserHelpText** localizado de **PredicateGroup** con id. `CharacterClasses` de **PredicateValidation** con id. `StrongPassword`.

```XML
<PredicateValidation Id="StrongPassword">
  <PredicateGroups>
    ...
    <PredicateGroup Id="CharacterClasses">
    ...
    </PredicateGroup>
  </PredicateGroups>
</PredicateValidation>

...

<Predicate Id="IsLengthBetween8And64" Method="IsLengthRange">
  ...
</Predicate>
...


<LocalizedString ElementType="InputValidation" ElementId="StrongPassword" StringId="CharacterClasses">The password must have at least 3 of the following:</LocalizedString>

<LocalizedString ElementType="Predicate" ElementId="IsLengthBetween8And64" StringId="HelpText">The password must be between 8 and 64 characters.</LocalizedString>
```

## <a name="set-up-localization"></a>Configurar la localización

En este artículo, se muestra cómo admitir varios idiomas o configuraciones regionales en la directiva para los recorridos del usuario. Localization necesita tres pasos: configurar la lista explícita de idiomas admitidos; proporcionar las colecciones y cadenas específicas del idioma; y editar el elemento ContentDefinition de la página.

### <a name="set-up-the-explicit-list-of-supported-languages"></a>Configurar la lista explícita de idiomas admitidos

En el elemento **BuildingBlocks**, agregue el elemento **Localization** con la lista de idiomas admitidos. En el ejemplo siguiente, se muestra cómo definir la compatibilidad de localización para inglés (predeterminado) y español:

```XML
<Localization Enabled="true">
  <SupportedLanguages DefaultLanguage="en" MergeBehavior="ReplaceAll">
    <SupportedLanguage>en</SupportedLanguage>
    <SupportedLanguage>es</SupportedLanguage>
  </SupportedLanguages>
</Localization>
```

## <a name="next-steps"></a>Pasos siguientes

Consulte los siguientes artículos para obtener ejemplos de localización:

- [Personalización de idioma con directiva personalizada en Azure Active Directory B2C](custom-policy-localization.md)
- [Personalización de idioma con flujos de usuario en Azure Active Directory B2C](user-flow-language-customization.md)
