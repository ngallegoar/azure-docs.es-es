---
title: Ejemplos de transformación de notificaciones StringCollection para directivas personalizadas
titleSuffix: Azure AD B2C
description: Ejemplos de transformación de notificaciones StringCollection para el esquema de Identity Experience Framework (IEF) de Azure Active Directory B2C.
services: active-directory-b2c
author: msmimart
manager: celestedg
ms.service: active-directory
ms.workload: identity
ms.topic: reference
ms.date: 04/21/2020
ms.author: mimart
ms.subservice: B2C
ms.openlocfilehash: cac7e6feb632456b63b97ead057f9ecaf49322ea
ms.sourcegitcommit: ffc6e4f37233a82fcb14deca0c47f67a7d79ce5c
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 04/21/2020
ms.locfileid: "81729715"
---
# <a name="stringcollection-claims-transformations"></a>Transformaciones de notificaciones StringCollection

[!INCLUDE [active-directory-b2c-advanced-audience-warning](../../includes/active-directory-b2c-advanced-audience-warning.md)]

En este artículo se proporcionan ejemplos para usar las transformaciones de notificaciones de la colección de cadenas del esquema de Identity Experience Framework en Azure Active Directory B2C (Azure AD B2C). Para más información, vea [ClaimsTransformations](claimstransformations.md).

## <a name="additemtostringcollection"></a>AddItemToStringCollection

Agrega una notificación de cadena a una nueva notificación stringCollection de valores únicos.

| Elemento | TransformationClaimType | Tipo de datos | Notas |
| ---- | ----------------------- | --------- | ----- |
| InputClaim | item | string | Elemento ClaimType que se agregará a la notificación de salida. |
| InputClaim | collection | stringCollection | [Opcional] Si se especifica, la transformación de notificaciones copia los elementos de esta colección y agrega el elemento al final de la notificación de la colección de salida. |
| OutputClaim | collection | stringCollection | Valor ClaimType que se genera después de que se haya invocado a esta transformación de notificaciones, con el valor especificado en la notificación de entrada. |

Use esta transformación de notificaciones para agregar una cadena a una clase stringCollection nueva o existente. Se usa normalmente en un perfil técnico **AAD-UserWriteUsingAlternativeSecurityId**. Antes de crear una nueva cuenta de redes sociales, la transformación de notificaciones **CreateOtherMailsFromEmail** lee el ClaimType y agrega el valor al ClaimType **otherMails**.

La siguiente transformación de notificaciones agrega el ClaimType **email** al ClaimType **otherMails**.

```XML
<ClaimsTransformation Id="CreateOtherMailsFromEmail" TransformationMethod="AddItemToStringCollection">
  <InputClaims>
    <InputClaim ClaimTypeReferenceId="email" TransformationClaimType="item" />
    <InputClaim ClaimTypeReferenceId="otherMails" TransformationClaimType="collection" />
  </InputClaims>
  <OutputClaims>
    <OutputClaim ClaimTypeReferenceId="otherMails" TransformationClaimType="collection" />
  </OutputClaims>
</ClaimsTransformation>
```

### <a name="example"></a>Ejemplo

- Notificaciones de entrada:
  - **collection**: ["someone@outlook.com"]
  - **item**: "admin@contoso.com"
- Notificaciones de salida:
  - **collection**: ["someone@outlook.com", "admin@contoso.com"]

## <a name="addparametertostringcollection"></a>AddParameterToStringCollection

Agrega un parámetro de cadena a una nueva notificación stringCollection de valores únicos.

| Elemento | TransformationClaimType | Tipo de datos | Notas |
| ---- | ----------------------- | --------- | ----- |
| InputClaim | collection | stringCollection | [Opcional] Si se especifica, la transformación de notificaciones copia los elementos de esta colección y agrega el elemento al final de la notificación de la colección de salida. |
| InputParameter | item | string | El valor que se agregará a la notificación de salida. |
| OutputClaim | collection | stringCollection | El valor ClaimType que se genera después de que se haya invocado esta transformación de notificaciones, con el valor especificado en el parámetro de entrada. |

Use esta transformación de notificaciones para agregar un valor de cadena a una clase stringCollection nueva o existente. En el ejemplo siguiente se agrega una dirección de correo electrónico constante (admin@contoso.com) a la notificación **otherMails**.

```XML
<ClaimsTransformation Id="SetCompanyEmail" TransformationMethod="AddParameterToStringCollection">
  <InputClaims>
    <InputClaim ClaimTypeReferenceId="otherMails" TransformationClaimType="collection" />
  </InputClaims>
  <InputParameters>
    <InputParameter Id="item" DataType="string" Value="admin@contoso.com" />
  </InputParameters>
  <OutputClaims>
    <OutputClaim ClaimTypeReferenceId="otherMails" TransformationClaimType="collection" />
  </OutputClaims>
</ClaimsTransformation>
```

### <a name="example"></a>Ejemplo

- Notificaciones de entrada:
  - **collection**: ["someone@outlook.com"]
- Parámetros de entrada
  - **item**: "admin@contoso.com"
- Notificaciones de salida:
  - **collection**: ["someone@outlook.com", "admin@contoso.com"]

## <a name="getsingleitemfromstringcollection"></a>GetSingleItemFromStringCollection

Obtiene el primer elemento de la colección de la cadena proporcionada.

| Elemento | TransformationClaimType | Tipo de datos | Notas |
| ---- | ----------------------- | --------- | ----- |
| InputClaim | collection | stringCollection | ClaimTypes que usa la transformación de notificaciones para obtener el elemento. |
| OutputClaim | extractedItem | string | Elementos ClaimTypes que se producen después de que se invoque este elemento ClaimsTransformation. El primer elemento de la colección. |

En el ejemplo siguiente se lee la notificación **otherMails** y se devuelve el primer elemento de la notificación de **correo electrónico**.

```XML
<ClaimsTransformation Id="CreateEmailFromOtherMails" TransformationMethod="GetSingleItemFromStringCollection">
  <InputClaims>
    <InputClaim ClaimTypeReferenceId="otherMails" TransformationClaimType="collection" />
  </InputClaims>
  <OutputClaims>
    <OutputClaim ClaimTypeReferenceId="email" TransformationClaimType="extractedItem" />
  </OutputClaims>
</ClaimsTransformation>
```

### <a name="example"></a>Ejemplo

- Notificaciones de entrada:
  - **collection**: ["someone@outlook.com", "someone@contoso.com"]
- Notificaciones de salida:
  - **extractedItem**: "someone@outlook.com"


## <a name="stringcollectioncontains"></a>StringCollectionContains

Comprueba si un tipo de notificación StringCollection contiene un elemento.

| Elemento | TransformationClaimType | Tipo de datos | Notas |
| ---- | ----------------------- | --------- | ----- |
| InputClaim | inputClaim | stringCollection | Tipo de la notificación que se va a buscar. |
|InputParameter|item|string|Valor que se va a buscar.|
|InputParameter|ignoreCase|string|Especifica si la comparación distingue entre mayúsculas y minúsculas en las cadenas que se están comparando.|
| OutputClaim | outputClaim | boolean | El valor ClaimType que se genera después de que se haya invocado esta ClaimsTransformation. Indicador booleano si la colección contiene una cadena de este tipo. |

En el siguiente ejemplo se comprueba si el tipo de notificación stringCollection `roles` contiene el valor **admin**.

```XML
<ClaimsTransformation Id="IsAdmin" TransformationMethod="StringCollectionContains">
  <InputClaims>
    <InputClaim ClaimTypeReferenceId="roles" TransformationClaimType="inputClaim"/>
  </InputClaims>
  <InputParameters>
    <InputParameter  Id="item" DataType="string" Value="Admin"/>
    <InputParameter  Id="ignoreCase" DataType="string" Value="true"/>
  </InputParameters>
  <OutputClaims>
    <OutputClaim ClaimTypeReferenceId="isAdmin" TransformationClaimType="outputClaim"/>
  </OutputClaims>
</ClaimsTransformation>
```

- Notificaciones de entrada:
    - **inputClaim**: ["reader", "author", "admin"]
- Parámetros de entrada:
    - **item**: "Admin"
    - **ignoreCase**: "true"
- Notificaciones de salida:
    - **outputClaim**: "true"

## <a name="stringcollectioncontainsclaim"></a>StringCollectionContainsClaim

Comprueba si un tipo de notificación StringCollection contiene un valor de notificación.

| Elemento | TransformationClaimType | Tipo de datos | Notas |
| ---- | ----------------------- | --------- | ----- |
| InputClaim | collection | stringCollection | Tipo de la notificación que se va a buscar. |
| InputClaim | item|string| Tipo de notificación que contiene el valor que se va a buscar.|
|InputParameter|ignoreCase|string|Especifica si la comparación distingue entre mayúsculas y minúsculas en las cadenas que se están comparando.|
| OutputClaim | outputClaim | boolean | El valor ClaimType que se genera después de que se haya invocado esta ClaimsTransformation. Indicador booleano si la colección contiene una cadena de este tipo. |

En el siguiente ejemplo se comprueba si el tipo de notificación del elemento stringCollection `roles` contiene el valor del tipo de notificación `role`.

```XML
<ClaimsTransformation Id="HasRequiredRole" TransformationMethod="StringCollectionContainsClaim">
  <InputClaims>
    <InputClaim ClaimTypeReferenceId="roles" TransformationClaimType="collection" />
    <InputClaim ClaimTypeReferenceId="role" TransformationClaimType="item" />
  </InputClaims>
  <InputParameters>
    <InputParameter Id="ignoreCase" DataType="string" Value="true" />
  </InputParameters>
  <OutputClaims>
    <OutputClaim ClaimTypeReferenceId="hasAccess" TransformationClaimType="outputClaim" />
  </OutputClaims>
</ClaimsTransformation> 
```

- Notificaciones de entrada:
    - **collection**: ["reader", "author", "admin"]
    - **item**: "Admin"
- Parámetros de entrada:
    - **ignoreCase**: "true"
- Notificaciones de salida:
    - **outputClaim**: "true"
